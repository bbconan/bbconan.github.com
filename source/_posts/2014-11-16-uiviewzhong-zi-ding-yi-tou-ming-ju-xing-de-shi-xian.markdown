---
layout: post
title: "UIView中自定义透明矩形的实现"
date: 2014-11-16 23:06
comments: true
categories: iOS
---
&nbsp;&nbsp;&nbsp;&nbsp;在项目中遇到一个需求：做一个二维码扫描的界面。要求是背景黑色透明度80%，中间有个白色透明框
<!--more--><br>如图所示</br>
{%img /images/qrcode.png %}

###解决方案
在drawRect里自定义绘制view的界面
```objc
- (void)drawRect:(CGRect)rect{
    //创建路径并获取句柄
    CGMutablePathRef path = CGPathCreateMutable();
    //指定矩形
    CGRect rectangle = self.bounds;
    //将矩形添加到路径中
    CGPathAddRect(path,NULL, rectangle);
    //获取上下文
    CGContextRef currentContext = UIGraphicsGetCurrentContext();
    //将路径添加到上下文
    CGContextAddPath(currentContext, path);
    //设置矩形填充色
    CGContextSetFillColorWithColor(currentContext, [UIColor colorWithWhite:0.0f alpha:0.8f].CGColor);
    CGContextFillRect(currentContext, rectangle);
    CGContextClearRect(currentContext, CGRectMake(50.f, 50.f, 220.f, 220.f));
    //绘制
    CGContextDrawPath(currentContext, kCGPathFillStroke);
    CGPathRelease(path);
   
}
```