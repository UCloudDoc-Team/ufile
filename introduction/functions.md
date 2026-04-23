

# 功能概览

## 存储功能

US3 产品提供了以下对象存储的常用上传、下载功能支持。

### 普通上传（Put File）

普通上传是指用户使用对象存储 API 中的 Put File 方法上传单个 File，可以适用在任何一次 HTTP 请求交互即可完成上传的场景，比如小文件的上传。

### [表单上传（Post File)](https://docs.ucloud.cn/ufile/guide/file/put?id=%e8%a1%a8%e5%8d%95%e4%b8%8a%e4%bc%a0)

表单上传是一种适用于小文件上传的上传方式，是指用户使用对象存储 API 中的 Post File 请求来完成 File 的上传。

### [分片上传（Multipart Upload）](https://docs.ucloud.cn/ufile/guide/file/put?id=%e5%88%86%e7%89%87%e4%b8%8a%e4%bc%a0)

分片上传是指将待上传文件分成多个数据块来分别上传，上传完成之后再调用对象存储 API 将这些 Part 组合成一个 File。

### 普通下载（Get File）

普通下载是指用户下载已经上传的文件，文件下载是使用 HTTP 的 GET 请求来完成的。

### 分片下载（Range Get）

分片下载是指从 File 指定的位置开始下载的功能，对于大文件可以分为多次下载。如果中途下载中断，重启的时候也可以从上次最后完成的地点开始继续下载。

## 管理功能

US3 产品除了支持标准的对象存储上传、下载功能外，提供了丰富的管理功能。

### [域名管理](https://docs.ucloud.cn/ufile/guide/domain)

对象存储空间支持提供测试域名进行访问，您同时可以选择绑定自定义域名创建 CDN 加速。

### [静态网站托管](https://docs.ucloud.cn/ufile/guide/static_websit_hosring)

可以通过控制台对已经绑定的自定义域名的US3存储桶配置静态网站托管策略。

### [生命周期](https://docs.ucloud.cn/ufile/guide/lifecycle)

开通存储空间生命周期功能，您可以实现存储空间内所有文件或特定前缀文件的生命周期管理，设置生命周期规则进行归档存储或删除文件操作。

### [镜像回源](https://docs.ucloud.cn/ufile/guide/mirror)

当您向 US3 存储空间请求的数据不存在时，您可以通过镜像回源规则设置，对于获取数据的请求以多种方式进行回源读取，满足您对于数据热迁移、特定请求重定向的需求。

### [跨域访问](https://docs.ucloud.cn/ufile/guide/cors)

跨域访问（CORS）是指当用户从一个域名的网页去请求另一个域名的资源的操作。您可以通过跨域访问策略管理功能，对跨域访问的权限进行限制。

### [存储桶复制](https://docs.ucloud.cn/ufile/guide/multisite)

您可以选择两个不同的存储空间进行绑定，进行文件的同步。提供存储空间跨地域容灾能力的同时能够满足您数据复制的需求。

### [日志管理](https://docs.ucloud.cn/ufile/guide/logging)

在设置日志管理存储空间后，所有用户对于存储空间的访问日志，会以小时为单位，按照固定的命名，存储到您指定的存储空间中。

### [令牌管理](https://docs.ucloud.cn/ufile/guide/token)

令牌功能可以根据用户的需求灵活的开放存储空间和文件管理权限。

## 增值功能

### [图片处理](https://docs.ucloud.cn/ufile/service/introduction)

支持对上传保存在对象存储空间中的图片进行一系列图片处理操作。

### [解压缩服务](https://docs.ucloud.cn/ufile/service/zip)

解压缩服务是UCloud对外提供的低成本高可靠的解压服务，可设置规则对上传的压缩包自动解压。

### [文档预览](https://docs.ucloud.cn/ufile/service/doc_preview)

文档预览功能支持存储在US3的文字文件、演示文件、表格文件、以及pdf文件的在线预览。


