# 历史版本

## US3FS v2.1.0

### 新特性

- 增加`pprof_listen_ip`参数设置，用于设置pprof的监听ip

## US3FS v2.0.9

### 新特性

- 增加`enable_recursive_dir`参数设置，用于设置是否开启递归目录刷新，可能会增加拉取列表的延迟

## US3FS v2.0.8

### BUG修复

- 目录的ctime/mtime统一设置成20000401的时间,支持rsync命令设置目录的时间

## US3FS v2.0.7

### 新特性

- 新增文件夹递归刷新的特性

## US3FS v2.0.6

### BUG修复

- 修复非root用户挂载us3fs 文件中的用户组权限为root的问题

## US3FS v2.0.5

### 新特性

- 支持mv接口中文件大于100MB的场景

## US3FS v2.0.3

### BUG修复

- 修复部分场景下 GID, UID 显示错误

## US3FS v2.0.2

### BUG修复

- 修复小概率的大文件上传丢失最后一块分片的异常


## US3FS v2.0.1

### 新特性

- 增加`fuse_session_cnt`参数设置，用于设置us3fs与内核fuse模块的连接数，开启后读写性能都会有一定程度的提升

## US3FS v2.0.0

### 新特性

- 增加读缓存功能，提升读吞吐
- 增加同一个bucket多个挂载点之间操作同步功能

## US3FS V1.7.12

### BUG修复

- 修复部分网络连接异常问题

## US3FS V1.7.11

### 新特性

- 增加`mp_mask`参数，可对挂载点进行权限控制

## US3FS V1.7.10

### BUG修复

- 修复开启finish_write_when_release时，对正在结束上传的文件做mv操作时，会mv到一个空文件

## US3FS V1.7.9

### BUG修复

- 修复使用prefix参数报错问题

## US3FS V1.7.8

### 新特性

- 增加stats，gc，reload命令

### 改进

- 优化顺序读判断
- 优化读缓存淘汰的效率

## US3FS V1.7.7

### 新特性

- 增加 `--reload` 命令，可以通知运行在同一台机器上的us3fs实例reload它们使用的配置文件（仅针对us3fs_1.7.7版本及以后的版本）
- 优化预读的读取效率和内存使用

## US3FS V1.7.6

### BUG修复

- 修复部分场景下小文件上传失败的问题

### 新特性

- 增加--disable_remove参数，用来禁用 unlink, rmdir, rename等操作

## US3FS V1.7.5

### BUG修复

- 修复部分场景下rename导致inode本地丢失的问题
- 修复预读场景下读取压力大时内存占用过大的问题

## US3FS V1.7.4

### BUG修复

- 修复文件句柄多次close导致异常退出的问题

### 新特性

- host参数可配置IP加端口

### 改进

- 适配高版本的fuse配置

## US3FS V1.7.3

### BUG修复

- 修复使用skip_ne_dir_lookup时异常退出的问题

## US3FS V1.7.2

### BUG修复

- 修复crash日志在某些场景下会被异常清理

## US3FS V1.7.1

### BUG修复

- 修复某些场景下的文件查找失败

## US3FS V1.7.0

### 新特性

- 增加`cache_db`参数，支持将us3fs元数据缓存在磁盘
- 增加`finish_write_when_release`参数，支持异步结束文件上传
- 增加`read_after_write_finish`参数，支持读等待写入完成后才返回数据

### 改进

- 优化文件列表的加载速度

### BUG修复

- 修复部分异常场景下的内存未回收问题

## US3FS V1.6.8

### 新特性

- 支持写入时识别文件mimeType
- 增加enable_load_dentries参数，支持在us3fs挂载成功后提前加载目录信息，加快文件访问效率

### 改进

- 支持在配置文件里设置挂载参数
- bucket字段从配置文件移除，从而支持多个bucket使用同一份配置文件挂载
- 优化使用`-gfl`选项时拉文件列表的效率

### BUG修复

- 修复部分场景下本地inode cache不会更新问题
- 修复部分场景下dcache_timeout配置不生效问题
- 修复flush后再次write进程异常crash的问题
- 修复部分写入场景下，写入的文件长度为0的异常
- 修复inode同步延迟的问题

## US3FS V1.6.7

### 新特性

* 新增log_dir, log_max_age, log_rotation_time参数指定日志的输出路径

### BUG修复

* 优化高并发读写情况下的内存使用

## US3FS V1.6.6

### 新特性

* 支持低频存储

### BUG修复

* 索引ttl更新异常导致的缓存淘汰时间不准确

## US3FS V1.6.5

### BUG修复

* 使用host参数时tcp链接无法复用问题
* 大文件分片上传时tcp链接无法复用问题

## US3FS V1.6.4

### BUG修复

* 修复专有云场景写入异常

## US3FS V1.6.3

### BUG修复

* 修复写入文件提交最后尾部数据时，后端报错无法透传给用户态问题

## US3FS V1.6.2

### 改进

* 异常奔溃日志捕获

### BUG修复

* 修复写入文件时的panic问题
* 默认配置路径修复

## US3FS V1.6.0

### 新特性

* 支持通过命令行在windows平台挂载
* 支持指定ip列表进行访问

### 改进

* 清理逻辑

### BUG修复

* 并发读写内部数据结构导致的奔溃问题
* fuse kernel触发的forget操作导致的路径信息丢失
* -o allow_other参数无效问题

## US3FS v1.5.5

### 新特性

* 无

### 改进

* 客户端http请求负载均衡

### BUG修复

* 上传压力大情况下，会导致上传分片不完整，从而导致失败
* 空指针导致奔溃

## US3FS v1.5.4

### 新特性

* 新增max_background,congestion_threshold参数支持direct模式下高并发io
* 新增async_dio参数，允许fuse kernel对direct io异步
* 新增keep_pagecache参数对文件大小以及修改时间不变的文件在vfs中进行缓存

### 改进

* 自动补全缺失的"key"格式目录，目录只创建"key/"格式的文件，且路径解析时优先head "key/"格式的文件；减少目录索引创建，有效减少创建与查询时延问题
* 参数check_virtual_dir功能默认开启，且取消该参数，需要关闭使用disable_check_vdir参数；减少对虚拟目录进行判断带来的时延
* 新增skip_ne_dir_lookup参数来开启文件名后缀的过滤字典，减少head操作频率，目前过滤支持".jpe"、".jpeg"、".png"、".gz"、".tgz"、".gz"、".tgz"、".log"、".plot"、".js"、".html"、".css"、".apk"为后缀的文件，需要确保bucket下没有用以上后缀作为目录后缀的情况

### BUG修复

* 无

## US3FS v1.5.3

### 新特性

* 新增direct_read参数支持绕过us3fs cache直接读us3
* 简单的性能分析数据周期性输出，默认1h输出一次，可以通过perf_dump参数调整
* 创建目录的mime-type设置为"application/x-directory"，便于在console上进行区分

### 改进

* entry_timeout，attr_timeout，dcache_timeout参数默认开启，且设置为5min
* readahead参数修改支持字符串解析，如:16m(1MiB)，8k(8KiB)，512(512B)
* 自动补全缺失的"key"格式目录，减少lookup目录时两次head操作("key"和"key/"格式的)为一次
* 默认开启参数nodirbl的功能，并且取消该参数，替换为语义化参数check_virtual_dir
* 新增文件名后缀的过滤字典，减少head操作频率，目前仅过滤以".jpe"和".jpeg"为后缀的文件

### BUG修复
* 分配inode时未初始化指定的uid,gid信息，以及未初始化mtime,ctime
* 目录成员过多情况下读取不完整问题
* allow_other参数未生效问题

## US3FS v1.5.2

### 新特性

* 无

### BUG修复

* 修复了后端连接重置后读取文件导致崩溃的问题

## US3FS v1.5.1

### 新特性

* 支持使用GetFileList获取目录下文件；通过`--gfl`参数开启

### BUG修复

* 修复了`mv`空目录报错的问题。
* 取消了预读最大64MB的限制

## US3FS v1.5.0

### 新特性

* 支持US3后端动态分片
* 支持挂载时指定bucket前缀路径

## US3FS v1.4.0

### 新特性

* 支持使用本地缓存异步上传
* 支持小文件读取读已之写（Read-Your-Write）

### 改进

* 通过--level指定日志级别

### BUG修复

* 修复了文件名中有特殊字符无法写入的问题。
* 修复了写入过程中可能出现的进程中断的问题。
* 修复了在对象存储中创建同名的目录和文件，ls会显示两个目录的问题
* 修复了初始化分片上传失败会阻塞的问题
