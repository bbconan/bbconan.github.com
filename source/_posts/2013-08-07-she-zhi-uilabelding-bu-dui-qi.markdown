---
layout: post
title: "设置UILabel文字顶部对齐"
date: 2013-08-07 14:45
comments: true
categories: iOS
---
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;xcode中UILabel是默认垂直居中的。UILabel拥有textAlignment属性，可以用NSTextAlignmentLeft, NSTextAlignmentRight, NSTextAlignmentCenter三种设置水平方向的文字对齐方式。那怎么做，可以是文字顶部对齐呢？下面有两种方法：
<!--more-->
###方法一
1、label先设置完text内容，即[label setText:@"你好"]。

2、label然后调用sizeToFit方法，即[label sizeToFit]。

由于label已经是文字的高度和宽度了，所以此时已经顶部对齐。
###方法二
最正统的方法，利用object-c的category特性，修改UILabel的代码。如下 :
``` objc
// -- file: UILabel+VerticalAlign.h
#pragma mark VerticalAlign
@interface UILabel (VerticalAlign)
- (void)alignTop;
- (void)alignBottom;
@end

// -- file: UILabel+VerticalAlign.m
@implementation UILabel (VerticalAlign)
- (void)alignTop {
    CGSize fontSize = [self.text sizeWithFont:self.font];
    double finalHeight = fontSize.height * self.numberOfLines;
    double finalWidth = self.frame.size.width;    //expected width of label
    CGSize theStringSize = [self.text sizeWithFont:self.font constrainedToSize:CGSizeMake(finalWidth, finalHeight) lineBreakMode:self.lineBreakMode];
    int newLinesToPad = (finalHeight  - theStringSize.height) / fontSize.height;
    for(int i=0; i<newLinesToPad; i++)
        self.text = [self.text stringByAppendingString:@"\n "];
}

- (void)alignBottom {
    CGSize fontSize = [self.text sizeWithFont:self.font];
    double finalHeight = fontSize.height * self.numberOfLines;
    double finalWidth = self.frame.size.width;    //expected width of label
    CGSize theStringSize = [self.text sizeWithFont:self.font constrainedToSize:CGSizeMake(finalWidth, finalHeight) lineBreakMode:self.lineBreakMode];
    int newLinesToPad = (finalHeight  - theStringSize.height) / fontSize.height;
    for(int i=0; i<newLinesToPad; i++)
        self.text = [NSString stringWithFormat:@" \n%@",self.text];
}
@end
```
然后，调用即可。


####参考资料
<http://blog.devtang.com/blog/2011/11/20/set-uilabel-text-align-top/>