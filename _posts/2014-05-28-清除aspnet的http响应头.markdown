---
layout: post
title: 清除Asp.Net的Http响应头
date: 2014-05-28 13:52:10
tags:
---
配置文件如下面所示
``` xml
<httpProtocol>
    <customHeaders>
                <clear/>//清除
                <add name="Access-Control-Allow-Origin" value="*" />//添加自定义
                <add name="Access-Control-Allow-Headers" value="Content-Type" />
            </customHeaders>
        </httpProtocol>
  </system.webServer>
```
