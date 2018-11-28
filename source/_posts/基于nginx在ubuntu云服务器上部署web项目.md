---
title: 基于nginx在ubuntu云服务器上部署web项目（11.27）
---

## 前言
写过类似的文章是在Centos7.5上部署项目，其实原理差不多，所以此篇文章就不多赘述，只写区别点

## 准备
1. 安装nginx(在ubuntu里面不是yum)
	```
	apt-get update;
	apt-get install nginx;
	```
2. 安装git

## 部署过程
1. 首先可以查看安装是否成功，需要在终端输入命令，启动服务
	```
	service nginx start
	```

	这个时候就可以在浏览器中输入服务器IP访问到`nginx`访问页

2. 去`github`上找到自己需要部署的项目，找一个合适的文件夹`clone`下来，并`pwd`查看路径

3. 部署自己项目，首先需要打开配置文件

	```
	vim /etc/nginx/sites-available/default
	```

	配置文件内容如下图所示

	![](https://ws1.sinaimg.cn/large/006XqmrNly1fxmrfzwv33j30v60a6gmb.jpg)

	将配置文件中的`root`后的内容修改为部署项目的路径，`index`后面就是展示主页

4. 重启nginx，之后在浏览器中输入`ip`即可访问
	```
	service nginx restart
	```

## 最后完善
1. 可以购买域名然后，配到服务器上，具体见[基于nginx在centos7.5云服务器上部署web项目](http://qm36mmz.xyz/2018/09/30/%E5%9F%BA%E4%BA%8Enginx%E5%9C%A8centos7.5%E4%BA%91%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E9%83%A8%E7%BD%B2web%E9%A1%B9%E7%9B%AE/)
