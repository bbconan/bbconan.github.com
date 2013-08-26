---
layout: post
title: "HMAC_SHA1算法的object-c实现"
date: 2013-08-08 15:26
comments: true
categories: IOS
---
做项目时，用到HMAC_SHA1算法。代码实现，如下：
``` objc
static NSString *HMAC_SHA1 = @"HmacSHA1";
static NSString *APPKEY = @"123456";//这是key
- (NSString *)generateSignature:(NSString *)data
{
    const char *cKey = [APPKEY cStringUsingEncoding:NSASCIIStringEncoding];
    const char *cData = [data cStringUsingEncoding:NSASCIIStringEncoding];
    
    unsigned char cHMAC[CC_SHA1_DIGEST_LENGTH];
    
    CCHmac(kCCHmacAlgSHA1, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
    
    NSData *HMAC = [[[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)] autorelease];
    
    NSString *hash = [HMAC base64Encoding];
    
    return hash;
}
```
这里用到NSData类别:<https://github.com/WideTag/WideNoise-iOS/blob/master/WideNoise/Categories/NSData%2BBase64Encoding.m/>