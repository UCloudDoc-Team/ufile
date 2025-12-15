# 1.7.11

## 新增

- 支持莫斯科地域显示

# 1.7.10

## 新增

- 支持在配置文件中使用 sts token 认证

# 1.7.9 

## 修改

- move优化为 copy 再delete

# 1.7.8

## 新增

- 新增了从us3同步文件到本地，支持增量和断点的功能
- du bucket支持通过listobject获取结果
- 解决cp 断点上传500错误

# 1.7.3

## 新增

- 增加"--language"非交互式创建配置语言选项，可选ZH/EN,其中ZH表示中文,EN表示英文,不使用此选项表示默认ZH中文
- 增加交互式创建配置提示选择语言ZH/EN,默认ZH中文

# 1.7.2

## 修复

- 现在cp命令在递归的上传整个文件夹时，当文件夹中任一个文件所有分片都上传完毕，会立刻调用mput finish，而不是等待整批文件都上传完
- 现在上传时会使用bucket配置的存储类型，而不是STANDARD

# 1.7.0

## 新增

- 增加"--debug"全局选项，来打印额外的配置信息以及http请求信息

# 1.6.2

## 修复

- 现在cp命令在下载时，会正确的替换掉本地文件，而不是部分覆盖本地文件

# 1.6.1

## 修复

- 现在sign命令生成的url会有一个3600s的默认过期时间

# 1.6.0

## 新增

- 新增了sync命令的--no-delete选项，可以在同步时默认不删除源端不存在的文件。

# 1.5.2

## 修复

- 修复了一个list时的ui bug

# 1.5.1

## 新增

- 新增了sync命令的-f选项，可以在同步删除时不弹出确认信息
- 新增了针对503和500的错误提示

# 1.5.0

## 修改

- rcat命令可以指定storage class，mimetype和metadata
- cp命令在上传文件夹时，如果指定了 [--include --exclude --rinclude --rexclude] 参数中的一个，就不会把文件夹名作为对象键前缀的一部分。
- config命令在交互式配置时会优先询问是否需要自定义endpoint，而不是先要求选择region

## 新增

- 新增了ls 命令的--format选项，可以选择使用json或yaml格式来输出列表，选项的参数大小写不敏感。（例如us3cli ls us3://bucket --format YAML)

# 1.4.0

## 新增

- 对专有云32M分片大小的支持

- 增加了对如下命令列取时携带--prefix-file-list参数来指定使用prefix-file-list接口列取的功能

  * cp 

  * du 
  * modify 
  * mv 
  * rm 
  * restore 
  * sync

# 1.3.0

## 修改

- cat命令目前可以并发下载
- 修复了du命令的一个bug，该bug会导致子账号拿不到桶的数据统计信息
- 现在可以并发的上传和下载，但是您无法同时执行两个同样的cp, sync,或du命令。（例如：您无法同时执行两个 cp ./test us3://bucket/prefix/test 命令）
- 现在modify命令在指定--recursive参数时可以批量的修改同一个prefix下的所有对象。
- 优化us3cli的升级逻辑，修复升级失败后再次打开cli出现的找不到可执行文件的错误
- 修复在根目录执行拷贝操作时通配符失效的问题
- 修复代理配置无法清除的问题

## 新增

- 新增了四个对于token的操作: create-token, delete-token, update-token, describe-token, 分别对应于token的增删改查。
- 新增了三个新的区域: jpn-tky, th-bkk, inspurcloud。
- 新增了ls命令的一个参数: --prefix-file-list, 设置这个参数将会调用prefix-file-list接口来拉取列表。

# 1.2.2
## 修改

- 修复上传重试失败问题
- 修复进度条展示出现负数问题
- 修复存在空目录时下载报错问题

## 新增

- 新增.svg文件的mimetype自动识别

# 1.2.0

## 新增

- 增加了动态进度条显示开关，默认开启
- du命令增加查看目录数据量功能
- ls命令新增查看projecdid功能
- ls展示文件列表新增文件数目显示信息
- cp命令增加自动识别mimetype功能
- sync命令新增指定storageclass（存储类型），mimetype（多媒体文件格式），metadata（元数据）上传功能
- sync命令新增mimetype自动识别功能
- 配置项新增内容加密、https以及代理功能

## 修改

- 修改ls下展示文件激活信息，修改激活时间格式，新增Restoring标志，表示文件正在激活中
- 修改sync中的remote模式为local模式，功能不变

## 删除

- 删除cp中的hit秒传功能
