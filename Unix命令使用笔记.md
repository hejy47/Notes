# Unix命令使用笔记

## 压缩解压

## 差异以及打补丁

参考[链接](https://www.cyberciti.biz/faq/appy-patch-file-using-patch-command/)

1. 使用`diff`创建统一的diff补丁文件。（类似于git中的git diff）
   1. `diff -u hello.c hello-new.c > hello.patch`
2. 使用`patch`应用补丁文件。（类似于git中的git apply）
   1. `patch < hello.patch`