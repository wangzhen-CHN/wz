---
title: mac端常用操作
date: 2020-10-20 15:27:45
tags:
---
## 链接远程服务器

在程序坞中右键终端图标，选择新建远程连接

选择「安全Shell（ssh）」

## 上传文件（夹）到远程服务器

在程序坞中右键终端图标，选择新建远程连接

选择「安全文件传输（sftp）」

#### 一、远程->本地

```js
1、文件
scp username@servername:/path/filename /localhost/filename
2、目录
scp -r username@servername:/path /localhoster
```
#### 二、本地->远程

```js
1、文件
scp /localhost/filename username@servername:/path/filename
2、目录
scp -r /localhost username@servername:/path
```