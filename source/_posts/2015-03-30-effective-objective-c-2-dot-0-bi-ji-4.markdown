---
layout: post
title: "Effective Objective-C 2.0 笔记4"
date: 2015-03-30 21:19
comments: true
categories: iOS
---
#四、协议和类别
##①、delegate要用weak修饰，因为可以避免互相引用。
##②、利用category可以使继承关系变为可控制的段。
##③、扩展里可以添加私有方法、私有属性。
##④、在扩展里添加类的实例变量。
##⑤、利用分类来提供一个匿名对象。它可以生成一个没有名字的inline(内联)类。
##⑥、NSDictionary中键值key是copy的，而值value是retain的。
