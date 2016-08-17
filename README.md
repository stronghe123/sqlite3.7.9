# sqlite3.7.9
# SQLite那么多的下载包

2016年的8月份打开http://www.sqlite.org/download.html  
发现 下载包真多，但是关于源码包只有四个，  

分别是[sqlite-amalgamation-3140100.zip](http://www.sqlite.org/2016/sqlite-amalgamation-3140100.zip) 只有若干个文件，混合版的sqlite3.c  
[sqlite-autoconf-3140100.tar.gz](http://www.sqlite.org/2016/sqlite-autoconf-3140100.tar.gz)       只有若干个文件，混合版的sqlite3.c跟很多TCL脚本等  
[sqlite-src-3140100.zip](http://www.sqlite.org/2016/sqlite-src-3140100.zip)                           开发环境下的最原始包。  
[sqlite-preprocessed-3140100.zip](http://www.sqlite.org/2016/sqlite-preprocessed-3140100.zip)    经过处理的，模块清晰的sqlite源码  

很明显了，我们要用preprocessed这份代码。但是[sqlite-preprocessed-3140100.zip](http://www.sqlite.org/2016/sqlite-preprocessed-3140100.zip)  这份代码已经22万行代码。  

我们再去查查相关的知识，  

SQLite外键(Foreign Key)支持 从SQLite 3.6.19 开始支持 外键约束，我们学习中尽量精简，  
就看<inside sqlite>该书用的SQLite3.3.6发行版，就是sqlite-3.3.6.tar.gz，这份代码就 6万多行。  
不过3.3.6版本不提供 preprocessed版本。还需要自己生成代码，有点火大。  
**不行，一定得找preprocessed代码来学习。**

继续找，也只有3.7版本开始才有提供preprocessed版本。
最后决定使用[sqlite-preprocessed-3070900.zip](http://sqlite.org/sqlite-preprocessed-3070900.zip) 来学习，妈的，这版本的代码有129个文件，一共14万行代码。  
 


> 用IDEA跑起来

 把所有代码扔到项目底下，增加一个cmake文件如下：

```
cmake_minimum_required(VERSION 3.5)
project(sqlite3)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99 -DSQLITE_CORE=1")

include_directories(${CMAKE_CURRENT_SOURCE_DIR}) #like gcc: -I
file(GLOB sources "*.c")
file(GLOB headers "*.h")
add_executable(sqlite3 ${sources} ${headers})
```

编译的时候，遇到两个错误。
1）编译的时候，有TCL的错：  
 
解决方法：把tclsqlite.c删除即可。tcl脚本功能就费掉了。。


2）编译的时候，有错如下：  

```
fts1 has a design flaw and has been deprecated.
```

解析：**FTS1**和FTS2都有设计的缺陷，现在已经被废弃，目前已经提供了FTS3或者FTS4，这些作为全文搜索的模块，弥补了以前的**FTS1**的不足。***_如果确定不会使用到全文搜索_***，可以直接使用SQLITE_CORE，禁用。将SQLITE_CORE添加到编译选项。

解决方法：在CMakeLists.txt加上这句话-DSQLITE_CORE=1即可：

```
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99 -DSQLITE_CORE=1")
```


最后成功跑起来。。
 

* * *
ends;

