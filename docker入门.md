## docker简介 ##
- 平台：开发，容纳，运行程序的平台
- docker平台：在容器安全的隔离运行应用程序
- docker引擎：cs结构。
  1.  server
  2.  rest api
  3.  cli
- docker对象
  1. 镜像，操作系统的概念
  2. 容器，进程的概念（镜像加载到容器中）
  3. 网络
  4. 数据卷，数据的持久化部分
- docker守护进程
  1. docker守护进程。运行在host之上，不用直接交互，通过cli进行交互。
- docker client
  1. 从用户接受指令和docker的守护进程通信。
- docker inside
  1. images：镜像，只读的模板。
  2. registers注册表：共有的私有的库，下载镜像等。共有的库：有很多现有的镜像。
  3. container：容器，通过镜像创建容器。每个容器是独立，安全运行的平台。容器中运行的是docker的组件。


## cenots6.x docker安装 ##
参考这个博客
``
http://qicheng0211.blog.51cto.com/3958621/1582909/
``


## 基本命令 ##
``docker ps -a`` 显示所有docker容器  
``docker run CMD``  
命令解释：  
docker ：os使用docker  
run    ：docker的子命令  
hello-world ：加载的镜像  

``docker run docker/whalesay cowsay boo-boo``  
查找并运行指定镜像，本地没有的话，会直接拉取仓库中的镜像到本地


构建镜像
--------
1. mkdir dockerbuild
2. touch Dockerfile  
centOs6有点问题
这个再查吧。



tag  push 
-------------------
1. docker images
2. 找到镜像id
3. 使用image tag标记镜像  
``docker tag imageId xxatdocker/docker-whale：latest``   
3. ``docker login --username-xx --emaile=xx`` 登陆docker
4. ``docker push xxatdocker/dcker-whale``发布镜像

pull
-----------------
1. docker images
2. 删除本地docker镜像文件  
``docker rmi -f hello-world``
3. docker run it18zhang/hello-world
4. 验证是否成功 
``docker images``


容器中运行应用
---------------------
- ``docker run ubuntu /bin/echo "hello world"``
在buntu中运行一个echo ""命令。容器运行时间和命令时间相同。  


- ``docker run -t -i ubuntu /bin/bash``  
-i 交互式  
-t 伪终端  
/bin/bash 容器中启动/bin/bash
运行成功即进入交互式容器中，即buntu虚拟机中；  
- ``exit / ctrl + d``退出交互式shell
- ``docker run -d ubuntu /bin/sh -c "while true;do echo hello world ;sleep 1;done"``创建容器以守护进程方式运行。    
-d 运行容器  
-c
输出容器id，通过该id操作容器。  
``docker ps -a``查看容器中运行的进程，以及容器名称  
``docker logs 容器id/名称`` 查看容器日志  
``docker stop 容器id/名称``停止docker容器 


运行简单的命令
------------------
1. 运行容器，使用trainning/webapp镜像
``docker run -d -P training/webapp python app.py`` -d：后台方式  
-P:映射所有暴露的端口  
``docker run -d -p 80:5000 training/webapp python app.py``  
-p: 映射指定的端口  
宿主机是80映射到容器的5000  

端口映射
----------
1. ``docker port 容器名称 5000``  查看虚拟机中的端口在宿主机上的映射端口  
2. ``docker-maching ip``  
查看容器的ip 

查看web容器的log
----
``docker logs -f con_name ``

查看容器中的进程
----
``docker top con_id ``

检查容器
-----------
``docker inspect con_id``

启动重启/重启重启是一样的
----
``docker start con_id ``  

镜像的管理
-------
- ``docker images``  列出镜像  
- 不指定tag默认使用least版本  
- `` ``更新提交影响
- ``docker commit -m "modify a.txt" -a "liangjt" ce8614f337d9 unbuntu:v2``提交变化给容器


- ``docker tag img_id unbuntu:v2``给镜像打标记  
- ``docker images rep:tag``







