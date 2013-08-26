---
layout: post
title: "IOS获得广播地址"
date: 2013-08-05 16:13
comments: true
categories: IOS
---
最近开发过程中，遇到需要获得ios系统广播地址的问题，经过一番google，获得一个很好的解决方法，如下：
####1、
``` objc
#include <sys/types.h>
#include <sys/socket.h>
#include <ifaddrs.h>
#include <arpa/inet.h>
- (NSString *)getBroadcastAddress
{
    NSString *broadCastAddress = @"";
    struct ifaddrs *interfaces = NULL;
    struct ifaddrs *temp_addr = NULL;
    int success = 0;
    // retrieve the current interfaces - returns 0 on success
    success = getifaddrs(&interfaces);
     if (success == 0) {
        temp_addr = interfaces;
      while(temp_addr != NULL)
        {
            // check if interface is en0 which is the wifi connection on the iPhone
            if(temp_addr->ifa_addr->sa_family == AF_INET)
            {
                if([[NSString stringWithUTF8String:temp_addr->ifa_name]                isEqualToString:@"en0"])
                {
                    broadCastAddress = [NSString stringWithUTF8String:inet_ntoa(((struct sockaddr_in *)temp_addr->ifa_dstaddr)->sin_addr)];
      }
     }
       temp_addr = temp_addr->ifa_next;
        }
    }
      freeifaddrs(interfaces);
      return broadCastAddress;
}
```
####2、
需要在xcode该工程里配置
{% img /images/broadcastaddr_setting.jpeg %} 
加入CFNetwork.framework、SystemConfiguration.framework