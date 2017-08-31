---
title: 搭建自己的 Sentry 服务
---
[Sentry 自动化异常提醒](https://laravel-china.org/articles/4235/sentry-automation-exception-alert) 这篇文章已经介绍的很清楚了，这里直接讲一讲搭建自己的 `sentry` 服务，这样就免费使用 `sentry` 这个服务。

## 1、 安装 `docker`

首先要确认你的 Ubuntu 版本是否符合安装 Docker 的前提条件。如果没有问题，你可以通过下边的方式来安装 Docker ：

使用具有 sudo 权限的用户来登录你的 Ubuntu 。

查看你是否安装了 wget

	 $ which wget
	 
如果 wget 没有安装，先升级包管理器，然后再安装它。

	 $ sudo apt-get update $ sudo apt-get install wget
	 
获取最新版本的 Docker 安装包

<!-- more -->

	 $ wget -qO- https://get.docker.com/ | sh
	 
系统会提示你输入sudo密码，输入完成之后，就会下载脚本并且安装 Docker 及依赖包。

通过 `docker --version` 可以查看版本号并确认是否安装成功。

由于某种神秘原因国内无法直接从 docker 官方库直接获取镜像 这里我们使用 [Docker 加速器](https://www.daocloud.io/mirror#accelerator-doc) 运行下面命令即可。

	curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://4031ebb7.m.daocloud.io

## 2、安装 `docker-compose`

这里推荐使用 Python 的 pip 管理工具来安装 `docker-compose` 

	$ sudo pip install -U docker-compose
	
到这里安装就结束了；`Compose` 已经安装完成。你可以使用 `docker-compose --version` 来进行测试 

## 3、 正式搭建sentry

做完了准备工作，就可以开始搭建sentry了。

从 [GitHub](https://github.com/getsentry/onpremise) 上面获取最新的 sentry

	git clone https://github.com/getsentry/onpremise.git
	
获取到本地之后，就可以根据他的README.md开始着手搭建了，整个过程还是比较顺利的。

进入 clone 下来的 `onpremise` 目录依次执行 

1. 创建对应的目录

		mkdir -p data/{sentry,postgres} 

2. 获取项目的 key

		docker-compose run --rm web config generate-secret-key

	![file](https://dn-phphub.qbox.me/uploads/images/201703/29/5978/cl5ribeIo5.png)

	复制获取到的 key 字符串 

		vim docker-compose.yml
	
	插入 docker-compose.yml 文件中	
	![file](https://dn-phphub.qbox.me/uploads/images/201703/29/5978/MI7aJM7jbz.png)

3. 创建项目的 `superuser` 
				
		docker-compose run --rm web upgrade
	
	该过程会让你输入 用户邮箱 和密码 一路走下去 即可。
		
4. 开启 sentry 服务
	
		docker-compose up -d 

这时候输入你的 http:://ip:9000 即可进入你的 sentry 


## 4、简单配置
	
登陆以后 右上角有 New Project

![file](https://dn-phphub.qbox.me/uploads/images/201703/29/5978/KqiVNsRUJb.png)

![file](https://dn-phphub.qbox.me/uploads/images/201703/29/5978/XRkeqwvTDh.png)

![file](https://dn-phphub.qbox.me/uploads/images/201703/29/5978/C480PIRJ5n.png)

体验 sentry 带来的快感吧！（本文完）

我的 [blog](http://www.tenyearsme.cn/blog/sentry-docker-create)
		
		