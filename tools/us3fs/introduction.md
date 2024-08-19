# 工具简介

## 概述

US3FS是一个在Linux/Windows系统环境中，将US3的存储空间（Bucket）挂载到本地挂载点的工具，挂载成功后，您可以像操作本地文件一样操作存储空间（Bucket）中的文件。

## 版本和运行环境

### 软件版本

当前版本：

- linux: v2.0.5
- windows: v1.6.8

### 运行环境

- Linux：
  - CentOS 7.0 及以上 (可通过`cat /etc/redhat-release`查看)
  - Ubuntu 16.04 及以上 (可通过`cat /etc/issue`查看)
- Windows
  - 开启WinFsp服务

### 软件架构图

![image](/images/US3FS2.0架构图2.png)

## 主要功能

* 支持POSIX文件系统的大部分功能，如读；顺序写；权限；UID/GID。
* 使用US3的分片上传功能上传大文件。
* 支持Etag和MD5校验，保证数据一致性。

## 使用限制

* 不支持读取归档类型的文件
* 不支持随机写/追加写
* rename非原子操作
* 不支持硬/软链接
* 多个客户端挂载同一个US3 Bucket时，需要用户自行维护数据一致性。

