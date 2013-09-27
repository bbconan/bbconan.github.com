---
layout: post
title: "dyld:lazy symbol bingding failed:Symbol not found 问题的解决"
date: 2013-08-06 16:44
comments: true
categories: iOS
---
遇到这么一个蛋疼的问题：dyld: lazy symbol binding failed: Symbol not found: _objc_setProperty_nonatomic
  Referenced from: /Users/imove-1/Library/Application Support/iPhone Simulator/5.1/Applications/24E4730A-9DB0-44F7-97E0-2D08F4AEC9DD/iMove.app/iMove<br/>
  Expected in: /Applications/Xcode.app/Contents/Developer/<br/>Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator5.1.sdk/System/Library/<br/>Frameworks/Foundation.framework/Foundation
  
  经过一番研究，发现出错原因：项目中，我是用cocoapods管理第三方包的。其中第三方包SwipeView的iOS Deployment Target 默认为iOS 6.1，改成4.3就可以了。
  
{% img /images/dyld_lazy_symbol.jpeg %}