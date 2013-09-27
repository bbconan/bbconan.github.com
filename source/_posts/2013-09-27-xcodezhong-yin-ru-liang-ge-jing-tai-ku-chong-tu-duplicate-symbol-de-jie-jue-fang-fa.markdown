---
layout: post
title: "xcode中引入两个静态库冲突'duplicate symbol'的解决方法"
date: 2013-09-27 14:17
comments: true
categories: iOS
---

&nbsp;&nbsp;&nbsp;&nbsp;在开发过程中，可能需要引入第三方的静态库，有时候会出现duplicate symbol的错误。例如:duplicate symbol _OBJC_METACLASS_$_Reachability in:/你的xcode路径/DerivedData/iMove-cwhkpttlcvtnivfvakvpiakhvoak/Build/Products/Debug-iphoneos/libPods.a(Reachability.o)<br>/你的项目路径/libVParser.a(Reachability.o)<br>
ld: 208 duplicate symbols for architecture armv7<br>
clang: error: linker command failed with exit code 1 (use -v to see invocation)
<!--more -->
&nbsp;&nbsp;&nbsp;&nbsp;这里有个解决方法：如果静态库结构一样，可以将2个库合并或者将其中1个库中重复的.o文件删除。下面以删除libVParser.a重复的.o文件为例。打开终端。<br>
#####1、、首先，查看文件结构<br>
输入 lipo -info libVParser.a<br>
结果 
Architectures in the fat file: libVParser.a are: armv7 (cputype (12) cpusubtype (11)) 
#####2、将armv7解压出来<br>
输入 
lipo libVParser.a -thin armv7 -output libVParser-armv7.a 
####3、新建一个文件夹存放解压出来的.o文件 <br>
 输入 mkdir armv7 <br>
 输入 cd armv7
#####4、将静态库文件解压 <br>
输入 
ar -x ../libVParser-armv7.a
#####5、删除重复的.o文件(如果是合并静态库的话，把另一个静态库按同样步骤解压。然后，对比，那些文件可以合并。)
#####6、删除（合并）完后，进行打包。<br>
输入 
libtool -static -o ../libnewVPaser.a *.o <br>

此时，用新生成的静态库文件就可以正常编译了。


 
 

	