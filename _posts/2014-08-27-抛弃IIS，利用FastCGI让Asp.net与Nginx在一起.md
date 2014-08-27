---
layout: post
title: 抛弃IIS，利用FastCGI让Asp.net与Nginx在一起
---

1. 用virtualbox建Linux虚拟机，桥接网卡，让Linux和Windows在一个局域网内。
2. Linux上安装Nginx，Nginx新建一个网站，域名随便（在hosts文件中把域名指向127.0.0.1即可），关键配置如下：  
	server_name  fcgitest.com;  
	location / {  
        fastcgi_pass   10.0.30.222:9000;#Windows上.Net的FastCGI端口  
        include        fastcgi_params;  
    }
3. Windows上，下载[https://github.com/wmeyer/SharpCGI/downloads](https://github.com/wmeyer/SharpCGI/downloads "SharpCGI-0.2-Precompiled.zip")，为了简单，直接打开里面的FastCGIAppCSharp项目，监听Ip改为0.0.0.0。
4. Windows上，在hosts文件中把域名指向Linux的Ip，在浏览器中打开域名，应该可以看到正确的结果了。
5. 下面再用ab做个压力测试  
	sudo apt-get install apache2-utils  
	ab -c 100 -n 9000 http://fcgitest.com/  
> Requests per second:    1742.95 [#/sec] (mean)

小结：Linux和Nginx未做如何调优，Nginx默认主页并发是4000多左右，这个结果勉强还可以，毕竟抛弃了笨重的IIS。下一步可以做更多的测试及调优，毕竟这样操作还需要把MVC框架整合进来，如果没有明显的性能提升，谁也不想这么折腾。Mono也提供了FastCGI服务器，但是Mono的坑太多，折腾不起。事情到了这，我不禁想要是Delphi也实现了FastCGI，还不如用Delphi呢，能给一个.NET活下去的理由么？貌似也只剩下快速开发这一项了，对，.NET施展最多的领域小型企业应用，这种不怎么赚钱的项目，老板怎么肯花钱？.Net开发人员的工资也必然比较低。

下面再做一个.NET服务器集群  
修改Nginx的域名配置文件，关键部分   
	upstream fastcgi {  
        server 10.0.30.222:9000;  
        server 10.0.30.223:9000 ;  
    }  
	server_name  fcgitest.com;  
	location / {  
        fastcgi_pass   fastcgi;  
        include        fastcgi_params;  
    }  
再次进行压力测试，结果：  
> Requests per second:2567.90 [#/sec] (mean)  

**并发提高了一倍**。这里可能有人会问Session的问题，Windows上有自带的Session服务，可以独立一台服务器用来存放Session。