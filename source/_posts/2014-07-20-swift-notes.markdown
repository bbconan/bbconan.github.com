---
layout: post
title: "记录碰到的几个问题"
date: 2014-07-20 18:08
comments: true
categories: iOS
---
####1、在ASIFormDataRequest的方法中，使用block的一个问题
```objc
        __weak ASIFormDataRequest  *request = [ASIFormDataRequest requestWithURL:url];
    	[request setCompletionBlock:^{
       [request xxMethod];
	}
```
<!--阅读更多-->
这段代码，在debug scheme下可以正常运行，但是在发布时的release scheme下有bug。因为编译器优化时\__\_weak指针会被置为nil，会被释放所以，下面的代码就无法执行。同理，___unsafe\_unretain变量也会存在同样的问题。解决方法是：强制持有指针
```objc
    ASIFormDataRequest  *request = [ASIFormDataRequest requestWithURL:url];
    __weak ASIFormDataRequest *weakRequest = request;
    __weak typeof(self) weakSelf = self;
    [request setCompletionBlock:^{
    [weakRequest xxMethod];
}
```
####参考资料
<http://stackoverflow.com/questions/7205128/fix-warning-capturing-an-object-strongly-in-this-block-is-likely-to-lead-to-a/>

####2、一些技巧
#####(1)、如何删掉所有的subView
  常见的方法就是遍历view的subviews，然后逐个移除。这里介绍两个简单的方法：
```objc 
[someNSView setSubviews:[NSArray array]]  
```
和
```objc
[[someUIView subviews] makeObjectsPerformSelector:@selector(removeFromSuperview)]
```
#####(2)、强制去除Xcode的编译警告
有时候，Xcode会提示一些代码编写存在的warnings，以让开发者能修改完善。但有时候，提示的warnings是正确的，不需要修改。怎么才能去除Xcode的警告呢？<br>
```objc
\#define SuppressPerformSelectorLeakWarning(Stuff) 
do { \
_Pragma("clang diagnostic push") \
_Pragma("clang diagnostic ignored \"-Warc-performSelector-leaks\"")
Stuff; \
_Pragma("clang diagnostic pop") \
} while (0)
```
_Pargma的处理方式是把字符串常量的内容（在删除两边的双引号，并把字符串常量内部的\"替换为"，把\\\替换为\\之后）看成是\#pragma指令中出现的预处理器标记。 这段代码的基本流程:

         1. push 当前警告入栈

         2. 忽略我们要消除的警告

         3. 执行会产生警告的代码

         4. pop 警告出栈——恢复之前的状态