

# AWS S3 协议支持说明

## 概述

S3协议是AWS推出，在对象存储行业成为事实标准，US3产品在自有标准的基础上，增加了针对S3 v4协议标准的兼容支持。

<video id="video" length=1000 width=800 controls="" preload="none" poster="https://static.ucloud.cn/d95327bb0a274715ba83f05ac3cda8fb.png">
      <source id="mp4" src="http://caozuozhinan.cn-bj.ufileos.com/周恭元1 US3对S3协议的支持.mp4">
      </video>


## 支持的 API

US3 目前的 S3 协议模块对标准 S3 协议的支持如下表：

| **编号** | **API名字**                                                                                                                                                                   | **备注说明**                                                                                                                  |
| :------: | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
|    1     | [HeadBucket](https://docs.aws.amazon.com/AmazonS3/latest/API/API_HeadBucket.html)                                                                                             | 检测 Bucket 是否存在以及您是否有权限访问该 Bucket                                                                             |
|    2     | [ListBuckets](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListBuckets.html)                                                                                           | 获取 Bucket 列表，只能获取公私钥或者 Token 拥有者创建的 Bucket                                                                |
|    3     | [GetBucketLocation](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketLocation.html)                                                                               | 返回所在地域名，不建议依赖该 API                                                                                              |
|    4     | [GetBucketAcl](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketAcl.html)                                                                                         | 没有太多意义，主要为了支持 S3 Browser 而实现，响应体中的 `Permission` 字段永远为 `FULL_CONTROL`                               |
|    5     | [GetBucketVersioning](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketVersioning.html)                                                                           | 没有太多意义，主要为了支持 S3 Browser 而实现，响应体中的 `Status` 字段永远为空字符串                                          |
|    6     | [PutBucketLifecycleConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketLifecycleConfiguration.html)                                                   | 为 Bucket 创建新的生命周期配置或替换现有的生命周期配置规则。注意，这将覆盖现有的所有生命周期配置规则                          |
|    7     | [GetBucketLifecycleConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketLifecycleConfiguration.html)                                                   | 获取 Bucket 中设置的生命周期配置规则                                                                                          |
|    8     | [DeleteBucketLifecycle](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteBucketLifecycle.html)                                                                       | 删除 Bucket 中设置的所有生命周期配置规则。注意，不支持删除 Bucket 中的指定某个或多个生命周期配置规则                          |
|    9     | [GetObjectAcl](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObjectAcl.html)                                                                                         | 获取 Object 的访问权限信息                                                                                                    |
|    10    | [PutObjectAcl](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObjectAcl.html)                                                                                         | 设置 Object 的访问权限信息                                                                                                    |
|    11    | [HeadObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_HeadObject.html)                                                                                             | 参考[US3 兼容 S3 API - v2.2.pdf](http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20-%20v2.2.pdf) |
|    12    | [PutObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObject.html)                                                                                               | 参考[US3 兼容 S3 API - v2.2.pdf](http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20-%20v2.2.pdf) |
|    13    | [PostObject](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectPOST.html)                                                                                             | 参考[US3 兼容 S3 API - v2.2.pdf](http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20-%20v2.2.pdf) |
|    14    | [CopyObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_CopyObject.html)                                                                                             | 参考[US3 兼容 S3 API - v2.2.pdf](http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20-%20v2.2.pdf) |
|    15    | [GetObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObject.html)                                                                                               | 参考[US3 兼容 S3 API - v2.2.pdf](http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20-%20v2.2.pdf) |
|    16    | [ListObjects](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListObjects.html)/[ListObjectsV2](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListObjectsV2.html)   | 参考[US3 兼容 S3 API - v2.2.pdf](http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20-%20v2.2.pdf) |
|    17    | [DeleteObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteObject.html)/[DeleteObjects](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteObjects.html) | 参考[US3 兼容 S3 API - v2.2.pdf](http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20-%20v2.2.pdf) |
|    18    | [CreateMultipartUpload](https://docs.aws.amazon.com/AmazonS3/latest/API/API_CreateMultipartUpload.html)                                                                       | 参考[US3 兼容 S3 API - v2.2.pdf](http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20-%20v2.2.pdf) |
|    19    | [UploadPart](https://docs.aws.amazon.com/AmazonS3/latest/API/API_UploadPart.html)                                                                                             | 参考[US3 兼容 S3 API - v2.2.pdf](http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20-%20v2.2.pdf) |
|    20    | [UploadPartCopy](https://docs.aws.amazon.com/AmazonS3/latest/API/API_UploadPartCopy.html)                                                                                     | 参考[US3 兼容 S3 API - v2.2.pdf](http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20-%20v2.2.pdf) |
|    21    | [CompleteMultipartUpload](https://docs.aws.amazon.com/AmazonS3/latest/API/API_CompleteMultipartUpload.html)                                                                   | 参考[US3 兼容 S3 API - v2.2.pdf](http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20-%20v2.2.pdf) |
|    22    | [AbortMultipartUpload](https://docs.aws.amazon.com/AmazonS3/latest/API/API_AbortMultipartUpload.html)                                                                         | 参考[US3 兼容 S3 API - v2.2.pdf](http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20-%20v2.2.pdf) |
|    23    | [ListMultipartUploads](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListMultipartUploads.html)                                                                         | 获取正在执行的分片上传请求 ID                                                                                                 |
|    24    | [ListParts](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListParts.html)                                                                                               | 获取正在执行分片上传的分片信息                                                                                                |
|    25    | [RestoreObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_RestoreObject.html)                                                                                       | 解冻处于归档状态的文件                                                                                                        |

注意:

* PutObject 目前仅支持 1GB 大小文件，如果需要上传大于 1GB 的文件，请采用分片上传的 API

* PostObject 目前仅支持最大 32MB 文件的上传

* CopyObject 目前仅支持最大 100MB 文件的拷贝

* UploadPart 目前仅支持 8MB 定长分片大小（最后一个分片允许小于 8MB）。若有不定长分片的需求，请联系技术支持

* US3 S3 对 AWS S3 兼容的存储类型及其转换规则参考 [存储类型转换规则](#存储类型转换规则)

* US3 的 ETag 计算方式与 AWS S3 存在部分差异，建议不依赖该 ETag

* 目前不支持 S3 API 的 MD5 校验，建议关闭:

      例如AWS S3 Java SDK:
      System.setProperty(SkipMd5CheckStrategy.DISABLEGETOBJECTMD5VALIDATION_PROPERTY,"");

      System.setProperty(SkipMd5CheckStrategy.DISABLEPUTOBJECTMD5VALIDATION_PROPERTY,"");

* US3 的访问权限（ACL）定义与 AWS S3 存在差异，具体参考 [访问权限定义（ACL）](#访问权限定义（ACL）)

* 目前文件访问权限控制 API（GetObjectAcl、PutObjectAcl）仅在部分地域支持

* 目前生命周期配置规则控制 API（PutBucketLifecycleConfiguration、GetBucketLifecycleConfiguration、DeleteBucketLifecycle）仅在部分地域支持

* 目前 UploadPartCopy 处于内测阶段。若有使用需求，请联系技术支持

* 目前不支持多版本功能（Versioning）

* 目前不支持标签功能（Tagging）

* ListObjects请求中的max-keys参数(请求返回对象的最大数量)最大值为5000

### 访问权限定义（ACL）

| US3 ACL           | [AWS S3 Canned ACL](https://docs.aws.amazon.com/AmazonS3/latest/userguide/acl-overview.html#canned-acl)     |
| ----------------- | ----------------------------------------------------------------------------------------------------------- |
| private           | private                                                                                                     |
| public-read       | public-read                                                                                                 |
| public-read-write | public-read-write                                                                                           |
| 不支持            | aws-exec-read<br>authenticated-read<br>bucket-owner-read<br>bucket-owner-full-control<br>log-delivery-write |

### 仅支持签名 V4

支持 V4 签名的场景：

1. URL 中携带参数（URL Query 部分的 x-amz-credential 字段）；

2. POST（表单中 x-amz-credential 域）;

3. Header 中携带参数（Authorization字段);

### S3 的 AccessKeyID 和 SecretAccessKey 说明

S3 的 AccessKeyID（或称AccessKey）和 SecretAccessKey（或称SecretKey）对应就是 UCloud 的 API 公钥和私钥，或者是 US3服务提供的 Token 公钥和 Token 私钥；

**注意：要求无论是 API 公私钥还是 Token 公私钥，要求操作的 bucket，必须满足以下条件:**

* 创建该 bucket 的账户与 API 公私钥的拥有者必须一致；

* 创建该 bucket 的账户与创建 Token 的账户必须一致；

### S3的分片大小说明

1. 为了达到更好的传输性能，默认情况下仅支持8M大小的分片。
2. 部分地域已开通动态分片功能，如果固定8M分片无法满足需求，可联系技术支持开通动态分片。

### API支持路径风格和虚拟主机风格

**路径风格格式为: `http://\${Endpoint}/\${bucket名字}/\${key名字}`，bucket 名字作为路径使用的一部分。**
例如，AWS S3 Java SDK 在 UCloud 北京地域走外网使用 US3 S3 服务则设置如下：


    "AWSCredential credentials = new BasicAWSCredentials(ACCESS_KEY,
    SECRET_KEY);
    ClientConfiguration clientConfig = new ClientConfiguration();
    ...
    S3ClientOptions clientOptions = S3ClientOptions.builder().build();
    clientOptions.setPathStyleAccess(true); // 表明使用路径风格API
    AmazonS3 conn = new AmazonS3Client(credential, clientConfig);
    conn.setS3ClientOptions(clientOptions);
    conn.setEndpoint("s3-cn-bj.ufileos.com");"

**虚拟主机风格: http://${bucket名字}.${Endpoint}/${key名字}，类似US3目前使用的URL形式。**

## 访问域名（Endpoint）

| **编号** | **地域** | **外网Endpoint**            | **内网Endpoint**                     |
| :------- | :------- | :-------------------------- | :----------------------------------- |
| 1        | 华北一   | s3-cn-bj.ufileos.com        | internal.s3-cn-bj.ufileos.com        |
| 2        | 华北二   | s3-cn-wlcb.ufileos.com      | internal.s3-cn-wlcb.ufileos.com      |
| 3        | 上海     | s3-cn-sh2.ufileos.com       | internal.s3-cn-sh2.ufileos.com       |
| 4        | 广州     | s3-cn-gd.ufileos.com        | internal.s3-cn-gd.ufileos.com        |
| 5        | 香港     | s3-hk.ufileos.com           | internal.s3-hk.ufileos.com           |
| 6        | 洛杉矶   | s3-us-ca.ufileos.com        | internal.s3-us-ca.ufileos.com        |
| 7        | 新加坡   | s3-sg.ufileos.com           | internal.s3-sg.ufileos.com           |
| 8        | 雅加达   | s3-idn-jakarta.ufileos.com  | internal.s3-idn-jakarta.ufileos.com  |
| 9        | 台北     | s3-tw-tp.ufileos.com        | internal.s3-tw-tp.ufileos.com        |
| 10       | 拉各斯   | s3-afr-nigeria.ufileos.com  | internal.s3-afr-nigeria.ufileos.com  |
| 11       | 圣保罗   | s3-bra-saopaulo.ufileos.com | internal.s3-bra-saopaulo.ufileos.com |
| 12       | 迪拜     | s3-uae-dubai.ufileos.com    | internal.s3-uae-dubai.ufileos.com    |
| 13       | 法兰克福 | s3-ge-fra.ufileos.com       | internal.s3-ge-fra.ufileos.com       |
| 14       | 胡志明市 | s3-vn-sng.ufileos.com       | internal.s3-vn-sng.ufileos.com       |
| 15       | 华盛顿   | s3-us-ws.ufileos.com        | internal.s3-us-ws.ufileos.com        |
| 16       | 孟买     | s3-ind-mumbai.ufileos.com   | internal.s3-ind-mumbai.ufileos.com   |
| 17       | 首尔     | s3-kr-seoul.ufileos.com     | internal.s3-kr-seoul.ufileos.com     |
| 18       | 东京     | s3-jpn-tky.ufileos.com      | internal.s3-jpn-tky.ufileos.com      |
| 19       | 曼谷     | s3-th-bkk.ufileos.com       | internal.s3-th-bkk.ufileos.com       |
| 20       | 伦敦     | s3-uk-london.ufileos.com    | internal.s3-uk-london.ufileos.com    |
| 21       | 莫斯科   | s3-rus-mosc.ufileos.com     | internal-s3-rus-mosc.ufileos.com     |

注意: *目前华北一，香港，胡志明，首尔，圣保罗，洛杉矶，华盛顿地域已经支持https协议，其他地域可支持路径风格https，后续支持虚拟主机风格https (所有地域内网不支持https)*

## 回调扩展功能支持

| **请求形式 API 名字**                          | **PUT Object** | **POST Object** | **Complete Multipart Upload** |
| ---------------------------------------------- | -------------- | --------------- | ----------------------------- |
| **在 URL 中携带参数**                          | √              | ×               | √                             |
| **在 Header 中携带参数**                       | √              | ×               | √                             |
| **在 POST 请求的 body 中使用表单域来携带参数** | ×              | √               | ×                             |

    √:支持
    ×:不支持

## 图片操作支持

参考 [图片处理服务](/ufile/service/pic)

## 存储类型转换规则

| **US3存储类型** | **S3存储类型**                                            | **US3对应S3默认存储类型** |
| --------------- | --------------------------------------------------------- | ------------------------- |
| STANDARD        | STANDARD<br/>STANDARD_IA                                  | STANDARD                  |
| IA              | ONEZONE_IA<br/>INTELLIGENT_TIERING<br/>REDUCED_REDUNDANCY | ONEZONE_IA                |
| ARCHIVE         | GLACIER<br/>DEEP_ARCHIVE                                  | GLACIER                   |
