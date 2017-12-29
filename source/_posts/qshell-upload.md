title: qshell_upload
date: 2017-12-29 16:26:38
tags: qshell tool
---
# 使用qshell上传资源到七牛

#### 首先[下载qshell](https://developer.qiniu.com/kodo/tools/1302/qshell#2)

#### 接着 登录账户[文档](https://developer.qiniu.com/kodo/tools/1302/qshell#4)

#### 批量上传只需要用到两个命令 dircache  和 qupload 

[dircache 文档](https://github.com/qiniu/qshell/blob/master/docs/dircache.md)

[qupload 文档](https://github.com/qiniu/qshell/blob/master/docs/qupload.md)

#### 举个🌰

以下是我的本地目录结构，使用多个帐号可以根据官方推荐的方法分目录存放账户信息

```
│  file_list.txt
│  qshell.exe
│  success.txt
│  upload.conf
│  upload.log
│
└─ibt
    └─.qshell
            account.json
```

列举文件

```
qshell.exe dircache C:\Users\ibesteeth\Documents\www\public\activity\annual_report file_list.txt
```

配置上传 [配置文档](https://github.com/qiniu/qshell/blob/master/docs/qupload.md#%E9%85%8D%E7%BD%AE)

```
touch upload.conf
vim upload.conf
{
   "src_dir"            :   "C:\\Users\\ibesteeth\\Documents\\www\\public\\activity\\annual_report",
   "bucket"             :   "webstatic",
   "file_list"          :   "C:\\Users\\ibesteeth\\Documents\\qshell\\file_list.txt",
   "key_prefix"         :   "activity/annual_report/",
   "ignore_dir"         :   false,
   "overwrite"          :   true,
   "check_exists"       :   false,
   "check_hash"         :   false,
   "check_size"         :   false,
   "rescan_local"       :   true,
   "skip_suffixes"      :   ".DS_Store,.exe,.html,.db",
   "log_file"           :   "upload.log",
   "log_level"          :   "debug",
   "log_rotate"         :   1,
   "log_stdout"         :   true,
   "file_type"          :   0
}
```

开始上传

```
qshell.exe qupload -success-list success.txt upload.conf 
```