# AWS S3 协议应用场景

## 文件浏览器工具

### 功能说明

S3 Browser 是一种易于使用和强大的 Amazon S3 免费客户端。 它提供了一个简单的 Web 服务接口，可用于存储和检索任意数量的数据，无论任何时候从任何地方。 可以通过相关配置，直接操控 US3 对象存储的 Bucket 中的文件，进行上传，下载，删除等操作。

### 安装和使用

**适用的操作系统：Windows**

### 安装步骤

​	1.下载安装包

下载地址: [http://s3browser.com](http://s3browser.com/)

​	2.安装程序

进入下载页面，点击 Download S3 Browser，按照提示，进行安装即可。

### 使用方法

​	1.增加用户

点击左上角 Accounts 按钮，在下拉框中，点击 Add new account

在 Add new account 页面中，需要填写的项描述如下：

**Account Name:** 账户名称，用户自定义。

**Account Type:** 账户类型，选择 S3 Compatible Storage

**REST Endpoint:** 固定域名，填写参考支持 AWS S3 协议说明。比如：s3-cn-bj.ufileos.com

**Signature Version:** 签名版本，选择 Signature V4。

**Access Key ID:** Api 公钥，或者 Token。具体获取请参考 [S3 的 AccessKeyID 和 SecretAccessKey 说明](/ufile/s3/s3_introduction?id=s3-的-accesskeyid-和-secretaccesskey-说明) 。

**Secret Access Key:** API 私钥。具体获取请参考 [S3 的 AccessKeyID 和 SecretAccessKey 说明](/ufile/s3/s3_introduction?id=s3-的-accesskeyid-和-secretaccesskey-说明) 。

**Encrypt Access Keys with a passward:** 请勿勾选。

**Use secure transfer(SSL/TSL)：** 目前仅中国-北京二，中国-香港，越南-胡志明，韩国-首尔，巴西-圣保罗，美国-洛杉矶，美国-华盛顿地域支持 HTTPS，其他区域请勿勾选。

具体配置填写如下：

![img](images/addAccount.jpg)

点击左下角的Advanced S3-compatible storage settings配置签名版本以及URL风格

![img](images/addAccount1.png)

修改成功点击Close 关闭当前设置，点击Add new account 保存配置，则成功创建用户。

​	2.对象操作

### 控制台功能说明

![img](images/console.png)

特别说明：目前分片大小只支持 8M 具体配置如下： 1.点击上方工具栏 Tools，在下拉列表中选择 Options，选择 General，在弹出页面中，设置 Enable multipart uploads with size (in megabytes) 为 8，如下图所示：

![img](images/console2.png)

### 常见问题

#### 1.上传文件超过78G,报错400.显示分片大小为16MB

##### 问题原因：

​        s3分片数限制为1万条，设置分片为8M是，只能满足78G左右以下文件的上传，如果文件大小超过78G,会自动调整分片大小，保证分片数小于1万条。目前us3后端s3协议只支持8M的分片，如果分片大小不对，会返回400错误。

##### 解决方案：

- 使用us3cli工具进行上传(us3协议)。使用方式参考 [us3cli 工具简介](https://docs.ucloud.cn/ufile/tools/us3cli/introduction)

## 网络文件系统 S3FS

### 功能说明

s3fs 工具支持将 Bucket 挂载到本地，像使用本地文件系统一样直接操作对象存储中的对象。

### 安装和使用

**适用的操作系统 Linux、MacOS**

**适用s3fs版本：v1.83及以上**

#### 安装步骤

MacOS 环境

```
brew cask install osxfuse
brew install s3fs
```

RHEL 和 CentOS 7 或更新版本通过 EPEL：

```
sudo yum install epel-release
sudo yum install s3fs-fuse
```

Debian 9 和 Ubuntu 16.04 或更新版本

```
sudo apt-get install s3fs
```

CentOS 6 及其以下版本

需要编译 s3fs ，并且安装该程序

#### 获取源码

首先，您需要从  上将源码下载到指定目录，以 `/data/s3fs` 为例：

```
1. cd /data
2. mkdir s3fs
3. cd s3fs
4. wget https://github.com/s3fs-fuse/s3fs-fuse/archive/v1.83.zip
```

#### 安装依赖项

CentOS 系统下安装依赖软件：

```
  sudo yum install automake gcc-c++ git libcurl-devel libxml2-devel
  fuse-devel make openssl-devel fuse unzip
```

#### 编译和安装 s3fs

进入安装目录，执行如下命令进行编译和安装：

```
1. cd /data/s3fs
2. unzip v1.83.zip
3. cd s3fs-fuse-1.83/
4. ./autogen.sh
5. ./configure
6. make
7. sudo make install
8. s3fs --version #查看 s3fs版本号
```

可以查看 s3fs 的版本号，到此，s3fs 已经安装成功。

备注：
在执行第五步，`./configure` 的过程中，可能会遇到以下的问题。汇总为:

报错： configure: error: Package requirements (fuse >= 2.8.4 libcurl >= 7.0 libxml-2.0 >= 2.6 ) were not met:

原因: fuse 版本过低，此时，您需要手动安装 fuse 2.8.4 及以上版本，安装命令示例如下：

1. yum -y remove fuse-devel #卸载当前版本的 fuse

2. wget https://.com/libfuse/libfuse/releases/download/fuse_2_9_4/fuse-2.9.4.tar.gz

3. tar -zxvf fuse-2.9.4.tar.gz

4. cd fuse-2.9.4

5. ./configure

6. make

7. make install

8. export

   PKG_CONFIG_PATH=/usr/lib/[pkgconfig:/usr/lib64/pkgconfig/:/usr/local/lib/pkgconfig](http://pkgconfig/usr/lib64/pkgconfig/:/usr/local/lib/pkgconfig)

9. modprobe fuse #挂载 fuse 内核模块

10. echo "/usr/local/lib" >> /etc/[ld.so](http://ld.so/).conf

11. ldconfig #更新动态链接库

12. pkg-config --modversion fuse #查看 fuse 版本号，当看到 “2.9.4” 时，表示 fuse 2.9.4 安装成功

### s3fs 使用方法

#### 配置密钥文件

在 `${HOME}/` 目录中创建 `.passwd-s3fs` 文件。文件格式为 `[API 公钥:API 秘钥]`。

公私钥获取方式具体请参考获取请参考 [S3 的 AccessKeyID 和 SecretAccessKey 说明](/ufile/s3/s3_introduction?id=s3-的-accesskeyid-和-secretaccesskey-说明)。

例如:

```
 [root@10-9-42-233 s3fs-fuse-1.83]# cat ~/.passwd-s3fs
 AKdDhQD4Nfyrr9nGPJ+d0iFmJGwQlgBTwxxxxxxxxxxxx:7+LPnkPdPWhX2AJ+p/B1XVFi8bbbbbbbbbbbbbbbbb
```

将文件设置读写权限。

```
 chmod 600 ${HOME}/.passwd-s3fs
```

#### 执行挂载操作

操作指令解释:

- 建立 US3 挂载文件路径 `${LocalMountPath}`

- 获取已创建的存储空间（Bucket）名称 `${UFileBucketName}`

  注意:空间名称不带域名后缀，比如 US3 空间名称显示为`test.cn-bj.ufileos.com`，则`${UFileBucketName}=test`

- 根据 US3 存储空间所在地域，本地服务器是否在 UCloud 内网，参考[支持 AWS S3 协议说明](/ufile/s3/s3_introduction)

- 执行命令。

参数说明如下：

```
s3fs ${UFileBucketName} ${LocalFilePath}
-o url={UFileS3URl} -o passwd_file=~/.passwd-s3fs
-o dbglevel=info
-o curldbg,use_path_request_style,allow_other
-o retries=1 //错误重试次数
-o multipart_size="8" //分片上传的大小为 8MB，目前仅支持该值 -o
multireq_max="8" //当上传的文件大于 8MB 是采用分片上传，目前UFile 的 S3
接入层不允许 PUT 单个文件超过 8MB，所以该值建议必填
-f //表示前台执行，后台执行则省略
-o parallel_count="32" //并行操作数，可以提高分片并发操作，建议不要超过 128
```

**示例：**

s3fs s3fs-test /data/vs3fs -o url=[http://internal.s3-cn-bj.ufileos.com](http://internal.s3-cn-bj.ufileos.com/) -o passwd_file=~/.passwd-s3fs -o dbglevel=info -o curldbg,use_path_request_style,allow_other,nomixupload -o retries=1 -o multipart_size="8" -o multireq_max="8" -o parallel_count="32"

#### 挂载效果

执行 `df -h` 指令，可以看到 s3fs 程序的运行。效果如下:

![img](images/dfh.png)

此时，可以看到 `/data/vs3fs` 目录下的文件和指定 bucket 的文件，保持一致。 也可以通过 tree 执行，查看文件结构。安装指令：`yum install -y tree` 效果如下：

![img](images/yum.png)

#### 文件上传和下载

挂载 US3 存储空间和后，可以像使用本地文件夹一样使用 US3 存储空间。

1. 拷贝文件到 `${LocalMountPath}` ，即是上传文件。
2. 将文件从 `${LocalMountPath}` 拷贝到其他路径，即下载文件。

**注意：**

1. 路径不符合 Linux 文件路径规范的路径，可以在 US3 管理控制台看到，但不会在 Fuse 挂载的 `\${LocalMountPath}` 下显示。
2. Fuse 使用枚举文件清单会比较缓慢，建议直接使用指定到具体文件的命令，如 vim、cp、rm 指定具体文件。

#### 删除文件

将文件从 `${LocalMountPath}` 删除掉，则 US3 存储空间中，该文件也被删除掉。

#### 卸载US3存储空间

```
sudo umount ${LocalMountPath}
```

### 性能数据

写入吞吐量40MB/s左右 读取吞吐量能达到166 MB/s（跟并发量相关）

## goofys

### 功能说明

goofys 工具同 s3fs, 也支持将 Bucket 挂载到本地，像使用本地文件系统一样直接操作对象存储中的对象。 性能方面比 s3fs 更优.

### 安装与使用

**适用的操作系统：Linux，MacOS**

### 使用步骤

下载可执行文件：

[Mac X86-64](https://github.com/ufilesdk-dev/ufile-fs/releases/download/v0.21.1/ufile-fs-mac.tar.gz) [Linux X86-64](https://github.com/ufilesdk-dev/ufile-fs/releases/download/v0.21.1/ufile-fs-linux.tar.gz)

使用如下命令解压到指定目录：

```
tar -xzvf goofys-0.21.1.tar.gz
```

默认在 `$HOME/.aws/credentials` 文件里面配置 bucket 的公私钥，格式如下：

```
[default]
aws_access_key_id = TOKEN_*****9206d
aws_secret_access_key = 93614*******b1dc40
```

执行挂载命令 `./goofys --endpoint your_ufile_endpoint your_bucket your_local_mount_dir`, 例如：

```
./goofys  --endpoint http://internal.s3-cn-bj.ufileos.com/suning2 ./mount_test:
```

挂载效果如图：

![img](images/goofys_mount.png)

测试挂载是否成功, 可以拷贝一个本地文件到 mount_test 目录, 看是否上传到 US3。

### 其它操作（删除,上传,获取,卸载）

同 s3fs, 可参考上面的 s3fs 操作

### 性能数据

4核8G 的 UHost 虚拟机, 上传 500MB 以上的文件, 平均速度可达140MB/s

## 基于 US3 的 FTP 服务

### 功能说明

对象存储支持通过 FTP 协议直接操作 Bucket 中的对象和目录，包括上传文件、下载文件、删除文件（不支持进入文件夹)。

### 安装和使用

**适用的操作系统：Linux**

### 安装步骤

#### 搭建环境

使用 s3fs 工具将 Bucket 挂载到本地。具体安装方式步骤参考基于 S3FS、US3 搭建**网络文件系统**的内容。

#### 安装依赖项

先检查下本地是否有 FTP 服务，执行命令 `rpm -qa | grep vsftpd`，如果显示未安装，则执行以下命令，安装 FTP。

运行以下命令安装 vsftpd。

```
yum install -y vsftpd
```

#### 开启本地 fpd 服务

执行以下命令，开启 ftp 服务。

```
service vsftpd start
```

### S3FS 使用方法

#### 添加账户

1. 运行以下命令创建 ftptest 用户，并且设置指定目录。

   useradd ${username} -d {SpecifiedDirectory}

   (删除用户命令：sudo userdel -r newuser)

2. 运行以下命令修改 ftptest 用户密码。

   passwd ${username}

#### 客户端使用

此时，您可以在外部任何一台机器上连接该服务器，输入您的用户名和密码，来管理 bucket 的文件

```
ftp ${ftp_server_ip}
```

## s3cmd

### 功能说明

s3cmd是一个免费的命令行工具,用于使用S3协议上传、检索和管理数据，它最适合熟悉命令行程序的用户，广泛用于批处理脚本和自动备份。

### 安装和使用

**适用的操作系统：Linux、MacOS、Windows**

#### 安装步骤
```
1.下载安装包
https://s3tools.org/download ,这里以目前最新版本2.1.0为例
2.解压安装包
tar xzvf s3cmd-2.1.0.tar.gz
3.移动路径
mv s3cmd-2.1.0 /usr/local/s3cmd
4.创建软连接
ln -s /usr/local/s3cmd/s3cmd /usr/bin/s3cmd (权限不足可以使用sudo)
5.执行配置命令,填写必要信息(直接跳过也可以，可以放在下一步手动填写)
s3cmd --configure
6.填写配置
vim ~/.s3cfg
打开当前配置，填写以下参数
access_key = "TOKEN公钥/API公钥"
secret_key = "TOKEN私钥/API私钥"
host_base = "s3协议域名,例如： s3-cn-bj.ufileos.com"
host_bucket = "请求风格，例如: %(bucket)s.s3-cn-bj.ufileos.com"
multipart_chunk_size_mb = 8 "us3 支持的s3协议分片大小为8M,所以这里只能填8"

```
access_key: 参考[Token公钥](https://console.ucloud.cn/ufile/token)/[API公钥](https://console.ucloud.cn/uapi/apikey)

secret_key: 参考 [Token私钥](https://console.ucloud.cn/ufile/token)/[API私钥](https://console.ucloud.cn/uapi/apikey)

host_base： 参考 [s3协议域名](https://docs.ucloud.cn/ufile/s3/s3_introduction)

#### 示例配置项

```
[default]
access_key = "TOKEN_xxxxxxxxx"
access_token =
add_encoding_exts =
add_headers =
bucket_location = US
check_ssl_certificate = True
check_ssl_hostname = True
connection_pooling = True
content_disposition =
content_type =
default_mime_type = binary/octet-stream
delay_updates = False
delete_after = False
delete_after_fetch = False
delete_removed = False
dry_run = False
enable_multipart = True
encrypt = False
expiry_date =
expiry_days =
expiry_prefix =
follow_symlinks = False
force = False
get_continue = False
gpg_passphrase =
guess_mime_type = True
host_base = s3-cn-bj.ufileos.com
host_bucket = %(bucket)s.s3-cn-bj.ufileos.com
human_readable_sizes = False
invalidate_default_index_on_cf = False
invalidate_default_index_root_on_cf = True
invalidate_on_cf = False
kms_key =
limit = -1
limitrate = 0
list_md5 = False
log_target_prefix =
long_listing = False
max_delete = -1
mime_type =
multipart_chunk_size_mb = 8
multipart_max_chunks = 10000
preserve_attrs = True
progress_meter = True
proxy_host =
proxy_port = 80
public_url_use_https = False
put_continue = False
recursive = False
recv_chunk = 65536
reduced_redundancy = False
requester_pays = False
restore_days = 1
restore_priority = Standard
secret_key = "xxxxxxxxxxxxxxxxxxx"
send_chunk = 65536
server_side_encryption = False
signature_v2 = False
signurl_use_https = False
simpledb_host = sdb.amazonaws.com
skip_existing = False
socket_timeout = 300
stats = False
stop_on_error = False
storage_class =
throttle_max = 100
upload_id =
urlencoding_mode = normal
use_http_expect = False
use_https = False
use_mime_magic = True
verbosity = WARNING
website_index = index.html
```
#### 使用方法

##### 1.上传文件
```
s3cmd put test.txt s3://bucket1
```

##### 2.删除文件
```
s3cmd del s3://bucket1/test.txt
```

##### 3.下载文件
```
s3cmd get s3://bucket1/test.txt
```

##### 4.拷贝文件
```
s3cmd cp s3://bucket1/test.txt s3://bucket2/test.txt
```
##### 其他常用操作
```
1.上传文件夹
s3cmd put -r ./dir s3://bucket1/dir1
2.下载文件夹
s3cmd get -r s3://bucket1/dir1 ./dir1
3.删除文件夹
s3cmd del s3://bucket1/dir1
4.列取bucket列表
s3cmd ls
5.列取文件列表
s3cmd ls s3://bucket1
6.归档文件取回
s3cmd restore s3://bucket1
```

## rclone

### 功能说明

rclone是一个命令行程序，用于管理云存储上的文件,支持s3协议

### 安装和使用

#### 安装步骤

```
curl https://rclone.org/install.sh | sudo bash

//参考 https://rclone.org/install/
```

#### 配置

```
rclone config
```

#### 配置参考

```
[s3]           //这里可以填写s3,ucloud等自定义名，作为命令前缀
type = s3
provider = Other
env_auth = false
access_key_id = xxxxxxxx
secret_access_key = xxxxxxxxxxx
endpoint = http://s3-cn-bj.ufileos.com   //参考
location_constraint = cn-bj
acl = private
bucket_acl = private // public/private
chunk_size = 8M                          //目前只支持8M分片
no_check_bucket = true
no_head = true
```

access_key_id: 参考[Token公钥](https://console.ucloud.cn/ufile/token)/[API公钥](https://console.ucloud.cn/uapi/apikey)

secret_access_key: 参考 [Token私钥](https://console.ucloud.cn/ufile/token)/[API私钥](https://console.ucloud.cn/uapi/apikey)

endpoint： 参考 [s3协议域名](https://docs.ucloud.cn/ufile/s3/s3_introduction)

#### 使用方法

说明: 以下命令前缀（配置内的中括号内容）以remote为例，使用过程中需要自行修改

##### 1.查看所有bucket

```
rclone lsd remote:
```

##### 2.列取文件列表

```
rclone ls remote:bucket
```

##### 3.上传文件

```
rclone copy ./test.txt remote:bucket
```

##### 4.删除文件

```
rclone delete remote:bucket/test.txt
```

## Cyberduck

### 功能说明

Cyberduck是一个跨平台的文件管理器，支持s3协议，适用于macos系统

### 安装和使用

#### 安装步骤

```
下载地址：https://cyberduck.io/download/

打开软件即可使用
```

#### 配置

```
Cyberduck配置目录为：
~/Library/Group Containers/G69SCX94XU.duck/Library/Application Support/duck/
在配置目录下创建命名为”default.properties”的文件
```

#### 配置参考

```
文件内容为：
s3.upload.multipart.lookup=false
s3.upload.multipart.size=8388608
```

完成后效果如下图所示

![](/images/pic/cyberduck1.png)

#### 使用方法

按照下图所示配置

![](/images/pic/cyberduck2.png)

access_key_id: 参考[Token公钥](https://console.ucloud.cn/ufile/token)/[API公钥](https://console.ucloud.cn/uapi/apikey)

secret_access_key: 参考 [Token私钥](https://console.ucloud.cn/ufile/token)/[API私钥](https://console.ucloud.cn/uapi/apikey)

endpoint： 参考 [s3协议域名](https://docs.ucloud.cn/ufile/s3/s3_introduction)

##### 支持操作说明

支持上传，下载，删除，列表，同步，预签名url等操作。

不支持移动，拷贝，重命名，新建加密库，恢复，归档等。
