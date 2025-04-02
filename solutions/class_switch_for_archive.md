# 归档转标准/低频存储类型

## 背景

- 提供⽅法要将归档类型⽂件转换为标准类型或者低频类型⽂件。
## 方案
- 使用激活加copy的方式创建出指定存储类型的新文件。 
- 具体流程:
1. 存储空间上传归档文件file1
2. 对file1解冻
3. 使用copy操作在同一存储空间中创建出指定文件类型的新文件file2
4. 根据实际场景需要删除原文件file1

## 参考代码

```go
import (
        "net/http"
        ufsdk "github.com/ufilesdk-dev/ufile-gosdk"
        "github.com/ufilesdk-dev/ufile-gosdk/example/helper"
        "log"
        "os"
        "time"
)

const (
        FakeFilePath    = "./FakeFile.txt"
        FileSize    =  5 * 1024 * 1024 * 1024
        ConfigFile    = "config.json"
        srcFileKey = "test_archive_5g.txt"
        newFileKey = "test_standard_5g.txt"
)

func MPutCopy(req *ufsdk.UFileRequest, dstkeyName, srcBucketName, srcKeyName string) error {
        state, err := req.InitiateMultipartUpload(dstkeyName, "") //初始化分片上传
        if err != nil {
                return err
        }
        parts, err := ufsdk.SplitFileByPartSize(FileSize, int64(state.BlkSize))
        if err != nil {
                log.Fatalf("SplitFileByPartSize Err:%v", err)
        }

        for _, part := range parts {
                // 拷贝分片
                err = req.UploadPartCopy(state, part.Number, srcBucketName, srcKeyName, part.Offset, part.Size)
                if err != nil {
                        req.AbortMultipartUpload(state)
                        return err
                }
        }

        return req.FinishMultipartUpload(state) // 完成分片上传
}

func main() {
        log.SetFlags(log.Lshortfile)
        if _, err := os.Stat(FakeFilePath); os.IsNotExist(err) {
                helper.GenerateFakefile(FakeFilePath, FileSize)
        }
        config, err := ufsdk.LoadConfig(ConfigFile)
        if err != nil {
                panic(err.Error())
        }

        header := make(http.Header)
        req, err := ufsdk.NewFileRequestWithHeader(config, header, nil)
        if err != nil {
                panic(err.Error())
        }

        //1、上传归档存储类型文件
        //存储类型，目前支持的类型分别是标准:"STANDARD"、低频:"IA"、冷存:"ARCHIVE"
        header.Set("X-Ufile-Storage-Class", "ARCHIVE")
        log.Println("正在上传归档存储类型文件。。。。")
        err = req.AsyncMPut(FakeFilePath, srcFileKey, "")
        if err != nil {
                log.Fatalln("文件上传失败，失败原因：", err.Error())
        }
        log.Println("文件上传成功，文件名：", srcFileKey)

        //2、解冻归档存储类型文件
        log.Println("正在解冻归档存储类型文件。。。。")
        err = req.Restore(srcFileKey)
        if err != nil {
                log.Fatalln("文件解冻失败，失败原因：", err.Error())
        }
        log.Println("文件解冻成功，文件名：", srcFileKey)
        //解冻需要几秒的生效时间
        time.Sleep(10 * time.Second)
        //3、copy创建新文件并指定存储类型为标准类型
        log.Println("正在创建标准类型的新文件。。。。")
        header.Set("X-Ufile-Storage-Class", "STANDARD")

        // 大于100M的文件使用UploadPartCopy
        if FileSize > 100 * 1024 *1024 {
                err = MPutCopy(req, newFileKey, req.BucketName, srcFileKey)
        } else { // 小文件copy
                err = req.Copy(newFileKey, req.BucketName, srcFileKey)
        }
        if err != nil {
                log.Fatalln("文件转换存储类型失败，失败原因：", err.Error())
        }
        log.Println("创建标准类型的新文件成功，新文件名：", newFileKey)

        //4、检查新文件存储类型
        log.Println("正在获取文件列表。。。。")
        list, err := req.PrefixFileList("test_", "", 10)
        if err != nil {
                log.Fatalln("获取文件列表失败，错误信息为：", err.Error())
        }
        log.Printf("获取文件列表返回的信息是：\n%s\n", list)

        //5、删除原文件
        log.Println("正在删除原文件。。。。")
        //err = req.DeleteFile(srcFileKey)
        if err != nil {
                log.Fatalln("文件删除失败。")
        }
        log.Println("原文件删除成功。")
}
```

## 测试用例

- 北京100k文件
![class_switch_100k](/images/pic/class_switch_1.png)

- 北京5g文件
![class_switch_5g](/images/pic/class_switch_2.png)
