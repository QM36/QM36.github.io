---
title: 在Mac上搭建自己的服务器——Nginx
---
## 目的
1. 在本地起一个服务进程，在浏览器中输入`localhost:8080`即可访问`nginx`欢迎页,如下图所示

![](http://p4k6er8dp.bkt.clouddn.com/18-9-25/71679971.jpg)
2. 再能够把自己的项目部署上去，在浏览器中输入`localhost:8080`即可访问自己的页面

## 准备
1. brew

	安装方法：在终端输入一下内容
	```
	/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
	```
2. nginx

	安装方法：依赖`brew`安装，在终端输入
	```
	brew install nginx
	```

## 部署过程
1. 安装好`nginx`之后，首先启动`nginx`，在任何路径下都可以
	```
	nginx
	```
2. 打开浏览器输入`localhost:8080`，即可访问`nginx`欢迎页面
3. 关键步骤：如何把自己写的项目部署上去，不管是打包之后的项目还是简单的`html`页面，都可以用此方法。

	* 首先用`vim`进入到`nigin.cionf`配置文件里面
	```
	vim /usr/local/etc/nginx/nginx.conf
	```
	> 注意：进入`vim`之后，首先需要输入i即为开始编辑
	* 在`nginx.conf`配置文件中找到如下所示内容，修改`root`与`index`后面的内容，改配置文件略像`json`格式
	
	![](http://p4k6er8dp.bkt.clouddn.com/18-9-25/23771489.jpg)
	> 注意：`root`后面的内容为你的项目的文件夹的路径，这个路径从根目录开始用绝对路径，`index`后面为展示首页的名称，一般都是`index.html`。注意不要找错地方，因为配置文件中出现过很多类似这种结构体的地方，注意前面不能带`#`，否则为注释掉的部分，编辑结束之后先`esc`，待出现冒号之后输入`wq`意为保存并退出。另外如果你不知道自己项目的路径改怎么写，可以在首先找到自己的项目，在这个目录下，输入`pwd`
4. 这个时候需要先关闭`nginx`，再重启
	* 关闭命令
	```
	nginx -s stop
	```
	* 打开命令
	```
	nginx
	```
	* 重启命令
	```
	nginx -s reload
	```
	> 注意如果重启时候出现如下问题`nginx: [error] open() "/usr/local/var/run/nginx.pid" failed (2: No such file or directory)`，找到你的`nginx.conf`的文件夹目录，然后运行这个`nginx -c /usr/local/etc/nginx/nginx.conf`命令， 再运行`nginx -s reload`，就可以了。还有在你输入以上命令之后，如果没有任何返回，不要惊讶，证明没有错误，继续下一步就好
5. 在浏览器中输入`localhost:8080`，就可以访问你的项目页面了，最终展示如下

![](http://p4k6er8dp.bkt.clouddn.com/18-9-25/4277388.jpg)



















