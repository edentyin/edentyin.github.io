---
title: zookeeper 使用与python模块
description : zookeeper 使用
date: 2018-09-24 16:00:00
categories:
 - 中间件
tags:
 - zookeeper
 - python
 - zkpython
---

# 一 基础CLI操作
	1） 进入zk：
		# zookeeper-client
	2） 创建Znodes：
		类别：临时，顺序,持久(default)
		语法：create /path “data”
		操作：
			· 创建持久类型
				# create /FirstZnode “Myfirstzookeeper-app_persist"
			· 创建顺序类型
				# create -s /FirstOrderZnode “Myfirstzookeeper-app_order"
			· 创建临时类型
				# create -e /FirstTmpZnode “Myfirstzookeeper-app_tmp"
	3)	获取数据：
		语法：get /path 
		操作：
			# get /FirstZnode
	4)	设置数据：
		语法：set /path “data”
		操作：
			# set /FirstZnode “change”
	5） 创建子项/子节点
		语法：create /parent/path/subnode/path /“data”
		操作：
			# create /FirstZnode/Child1 “firstchildren”
	6)	查看目录结构
		语法：ls /path
		操作：
			# ls /FirstZnode
	7）	检查状态：
		语法：stat /path
		操作：
			# stat /FirstZnode 
	8) 移除Znode
		语法：rmr /path
		操作：
			rmr /FirstZnode
  
# 二 安装zkpython，通过python操作zk
	1）安装zk客户端：（选择zk的时候最好选择3.4.6以上版本，以下的可能存在bug： https://issues.apache.org/jira/browse/ZOOKEEPER-2049）
		# wget https://archive.apache.org/dist/zookeeper/zookeeper-3.4.10/zookeeper-3.4.10.tar.gz
		# tar -zxvf zookeeper-3.4.10.tar.gz
		# cd zookeeper-3.4.10
		# ./configure
		# make 
		# make install
		此时安装完成zk的客户端

		NOTICE： 因为我已经有CDH集群，并在集群里面已经安装了zk，故采用CDH的环境。后续zkpython需要用到libzookeeper_mt.so.2这个文件，故需要把环境变量配置。
				我在/etc/profile里面添加了
				export LD_LIBRARY_PATH=/opt/cloudera/parcels/CDH-5.8.3-1.cdh5.8.3.p0.2/lib64:$LD_LIBRARY_PATH
				如不知路径是什么，可以通过  # find / -name 'libzookeeper_mt.so.2'   来查找具体路径

	2）安装zkpython,经过多次试验，python3+ 的基本都会报错，看了zookeeper.c 应该是很多python版本的语法问题，最后采用python2.7安装成功。
		· 使用pip
			# pip install zkpython
		· 使用源码，下载 https://pypi.org/project/zkpython/0.4.2/#files
			# tar -zxvf zkpython-0.4.2.tar.gz
			# cd zkpython-0.4.2
			# python setup.py install
			有些项目无法找到zk的资源，可在 setup.py 的zookeepermodule 参数列表里面添加现有的zk资源路径
	3） 使用：
		>>> import zookeeper
		>>> handler = zookeeper.init("localhost:2181");  # 连接zk，localhost可更改为具体ip
		>>> dir(zookeeper) # 方法列表
		其他操作网上查查啦~~~ （https://www.cnblogs.com/msnsj/p/4242604.html）
		使用列子：https://www.cnblogs.com/DjangoBlog/p/3808543.html

# 三 zk的使用场景
	
	http://jm.taobao.org/2011/10/08/1232/
