# windows操作系统如何搜索硬盘上某个命令



* 首先会从当前目录下搜索

* 当前目录搜索不到的话，会从环境变量path指定的路径当中搜索某个命令

* 如果都搜索不到，则会报错：

  例如：“‘sample’不是内部或外部命令，也不是可运行的程序或批处理文件”



# 路径

* ".." 代表上级路径
* "." 代表当前路径
