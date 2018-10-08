---
title: 在Mac上搭建自己的服务器——tomcat
---
## 目的
1. 在本地起一个服务进程，在浏览器中输入`localhost:8080`即可访问`tomcat`欢迎页,如下图所示

![](http://p4k6er8dp.bkt.clouddn.com/18-9-25/1164953.jpg)

2. 再能够把自己的项目部署上去，在浏览器中输入`localhost:8080/urprojectname`即可访问自己的页面

## 准备
1. jdk
	* 安装方法：在[官网](https://www.oracle.com/technetwork/java/javase/downloads/index.html)选择合适的版本下载，在下载之前需要`Accept License Agreement` ，之后根据提示安装即可
	* 验证方法：在终端输入`java`，若出现下图提示，则安装成功
	
	![](http://p4k6er8dp.bkt.clouddn.com/18-9-25/50018498.jpg)
	* 详细步骤：[百度经验](https://jingyan.baidu.com/article/7f766daffd99354101e1d095.html)
2. tomcat
	安装方法：在[官网](http://tomcat.apache.org)上下载解压并安装，为了方便管理，可以将解压之后的文件重命名为`ApacheTomcat`放进`/Users/mymacname/Library`，以下统一使用`ApacheTomcat`

## 部署过程
1. 安装好`tomcat`之后，首先启动`tomcat`，需要进入`ApacheTomcat`的`bin`中，会发现如下文件

	![](http://p4k6er8dp.bkt.clouddn.com/18-9-25/33621618.jpg)

	> 注意`startup.sh`与`shutdown.sh`为控制`tomcat`开启与关闭的脚本

启动`tomcat`需要`/bin`的路径下输入以下命令,若出现`Tomcat started`。则成功启动
	```
	sh startup.sh
	```

2. 这个时候就可以进浏览器打开`localhost:8080`去访问`tomcat`欢迎页
3. 关键步骤：如何把自己写的项目部署上去，不管是打包之后的项目还是简单的`html`页面，都可以用此方法。将自己的项目方在一个文件夹中，我自己的项目文件为`mood`，下文会涉及到这个文件名，放进`/ApacheTomcat/webapps`中，即可
4. 这个时候需要先关闭`tomcat`，再重启
	* 关闭命令
	```
	sh startup.sh
	```
	* 打开命令
	```
	sh shutdown.sh
	```

> 注意以上命令都需要在`/bin`的路径，即`startup.sh`与`shutdown.sh`存在的文件夹中使用

5. 在浏览器中输入`localhost:8080/mood`，就可以访问你的项目页面了，最终展示如下

![](http://p4k6er8dp.bkt.clouddn.com/18-9-25/80936594.jpg)

> 注意：如果自己的页面是`404`,首先可以看一下，复制过的文件有没有带的副本导致找不到文件，其次需要考虑将自己项目放进`webapps`之后，是否有关闭在打开，而不是直接刷新,最后一点就是需要在`localhost:8080`后面加你的项目文件夹的名称，而不是`index.html`













