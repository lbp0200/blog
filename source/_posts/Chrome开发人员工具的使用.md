---
layout: post
title: Chrome开发人员工具的使用
date: '2014-04-29 23:03'
comments: true
published: true
keywords: 
description: 
excerpt: 
categories: 
tags: 
---

Chrome开发人员工具确实是神器，可以动态地修改网页在浏览器中呈现地样式。

之前没有仔细地研究过，最近又学习到了一些新的功能使用方法，下面都以Chrome34版为例。

例如，网页301或者302跳转，浏览器请求的log就被更新了，不知道当前网页是如何跳转来的，下面介绍类似于Fildder的网页log。

勾选Network下的Preserve log，就能达到效果了

![Alt text](/images/20140429231440.jpg)  

Chrome还支持多种事件监听，如网页被js修改。在Elements下，鼠标右键需要监听的Dom，Break On，属性被修改、Dom被修改、节点被删除。

在Sources下右侧还要更多的事件，XHR BreakPoints是监听AJAX的请求；Event Listener BreakPoints是监听整个网页的事件，如load、click鼠标点击等等……