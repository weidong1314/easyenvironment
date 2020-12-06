#### CentOS7下Elasticsearch单机版安装

##### 1.ElasticSearch概述

ElasticSearch是一款**基于Apache Lucene**构建的**开源搜索引擎**，它**采用Java编写**并**使用Lucene构建索引**、提供**搜索功能**，ElasticSearch的目标是让全文搜索变得简单，开发者可以通过它简单明了的**RestFul AP**I轻松地实现搜索功能，而不必去面对Lucene的复杂性。ES能够轻松的进行大规模的横向扩展，以支撑PB级的结构化和非结构化海量数据的处理。

##### 2.Elasticsearch能够做什么

数字、文本、地理位置、结构化数据、非结构化数据。适用于所有数据类型

##### 3.ElasticSearch下载（与JDK捆绑版）

说明：与JDK捆绑版不需要单独安装JDK

Elasticsearch是使用Java构建的，并且在每个发行版中都包含来自JDK维护者（GPLv2 + CE）的捆绑版本的 OpenJDK。捆绑的JVM是推荐的JVM，位于ElasticSearch安装目录下的jdk/elasticsearch目录内

###### 3.1官网地址（中文）

https://www.elastic.co/cn/

![image-20201205223621960](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201205223621960.png)

###### 3.2最新版本下载地址

https://www.elastic.co/cn/downloads/elasticsearch

![image-20201205223821654](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201205223821654.png)

###### 3.3其他版本下载地址入口页面

https://www.elastic.co/guide/en/elasticsearch/reference/current/install-elasticsearch.html

![image-20201205224520059](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201205224520059.png)

###### 3.4其他版本下载地址

https://www.elastic.co/cn/downloads/past-releases#elasticsearch

![image-20201205225322790](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201205225322790.png)

##### 4.ElasticSearch下载（未与JDK捆绑版）

说明：比较灵活，需要自己单独安装JDK，

###### 4.1ElasticSearch与JDK版本对应关系

官方建议：Java 9，Java 10，Java 12和Java 13是短期版本。我们建议不要使用它们，除非您准备好应对这种情况带来的快速释放节奏。

此处我们最好使用Oracle JDK 1.7+，推荐使用Oracle JDK 1.8

![image-20201205230552275](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201205230552275.png)

4.2未与JDK捆绑版下载地址(最新版)

https://www.elastic.co/cn/downloads/elasticsearch-no-jdk

![image-20201205230911292](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201205230911292.png)



##### 5.安装前准备

说明：由于需要下载准备的文件比较大，直接采用浏览器直接下载的方式，然后利用Xftp、WinScp等工具上传到Linux系统创建好的用于安装elasticsearch的目录下。网速较好的情况下，可以直接使用wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.0.0-linux-x86_64.tar.gz命令下载

###### 5.1elasticsearch版本选择

此处选择elasticsearch-7.0.0-linux-x86 JDK捆绑版，大小为330 MB ,可根据实际需求自行选择合适版本

![image-20201205225853432](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201205225853432.png)

https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.0.0-linux-x86_64.tar.gz

![image-20201205231828192](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201205231828192.png)

5.2 安装Oracle JDK1.8

##### 6.安装

说明：6.安装的操作均用root用户操作

###### 6.1创建安装目录

```shell
mkdir -p elasticsearch-7.0.0
cd elasticsearch-7.0.0
```

![image-20201205232952995](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201205232952995.png)

###### 6.2解压缩elasticsearch-7.0.0安装包

```shell
tar -zxf elasticsearch-7.0.0-linux-x86_64.tar.gz
```

![image-20201205232919526](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201205232919526.png)

###### 6.3修改/config/elasticsearch.yml配置文件

```shell
cd config
vim elasticsearch.yml
#/config/elasticsearch.yml配置外网访问 默认外网无法访问 打开network.host注释 修改为改为0.0.0.0对外开放，如对特定ip开放则改为指定ip
network.host: 0.0.0.0
#可更改端口不为9200 默认为9200
http.port: 9200 
#解决这个报错ERROR: [1] bootstrap checks failed
#[1]: the default discovery settings are unsuitable for production use; at least one of 
#[discovery.seed_hosts, discovery.seed_providers, cluster.initial_master_nodes] must be configured
cluster.initial_master_nodes: ["node-1"]
```

![image-20201206095729942](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201206095729942.png)



###### 6.4修改系统环境变量

```shell
vim /etc/sysctl.conf
#添加如下内容
vm.max_map_count=262144
#修改完需要重启才能生效
sysctl -w vm.max_map_count=262144
```

![image-20201205234253119](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201205234253119.png)

![image-20201205234518868](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201205234518868.png)

###### 6.5修改JVM分配大小

```shell
cd config
vim jvm.options
#Xms和Xmx大小要一致 默认是1g 可以根据虚拟机实际大小改变此值，如
-Xms2g
-Xmx2g
#或者
-Xms512m
-Xmx512m
```

![image-20201205234838622](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201205234838622.png)

###### 6.6修改最大文件描述符数量和用户最大线程数

```shell
vim /etc/security/limits.conf
# /etc/security/limits.conf中添加如下内容  只有root用户才有权限修改/etc/security/limits.conf
* soft nofile 65536
* hard nofile 65536
* soft nproc 4096
* hard nproc 4096
```

设置限制数量，第一列表示用户，* 表示所有用户

soft nproc ：单个用户可用的最大进程数量(超过会警告);
hard nproc：单个用户可用的最大进程数量(超过会报错);
soft nofile ：可打开的文件描述符的最大数(超过会警告);
hard nofile ：可打开的文件描述符的最大数(超过会报错);

![image-20201206000951180](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201206000951180.png)

###### 6.7配置ElasticSearch JAVA_HOME和PATH

如果本地配置了JAVA_HOME的环境路径，而且Java的版本比较低的话启动就会失败，ElasticSearch 中有JDK的版本最好使用该版本的JDK

```shell
#以root用户进入到ElasticSearch安装目录下
cd elasticsearch-7.0.0
vim bin/elasticsearch
#在开始的位置加入：
export JAVA_HOME=/root/soft/elasticsearch-7.0.0/elasticsearch-7.0.0/jdk #（此处es的jdk所在目录）
export PATH=$JAVA_HOME/bin:$PATH


```

![image-20201206003823912](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201206003823912.png)

![image-20201206004101325](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201206004101325.png)



##### 7.启动ElasticSearch

**ElasticSearch为了安全考虑，以root用户启动ElasticSearch会报错**

###### 7.1创建es用户，用于启动ElasticSearch

```shell
#以root用户进入到ElasticSearch安装目录下
cd elasticsearch-7.0.0
#添加一般用户es 用于启动ElasticSearch
useradd es
#给es用户设置密码为123456，可以替换成自己的密码
echo "123456" | passwd es --stdin
#更改文件夹所属权 将ElasticSearch目录权限改为es用户
chown -R es:es ./
#使es用户具有打开root目录的权限 此处elasticsearch-7.0.0我们这里安装在root目录下 不进行此步骤 则后面启动会报错
# 报错信息为：... Permission denied
chmod 777 /root
#在/etc/sudoers文件里给该用户es添加sudo权限
vim /etc/sudoers
## Allow root to run any commands anywhere 
#root    ALL=(ALL)       ALL 这行下面添加下面添加
es   ALL=(ALL)       ALL
```

![image-20201206002447420](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201206002447420.png)

![image-20201206005459531](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201206005459531.png)

![image-20201206005546341](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201206005546341.png)

###### 7.2切换到es用户

```shell
su es
```

![image-20201206002901243](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201206002901243.png)

###### 7.3启动ElasticSearch

前台启动：

说明：当你ctrl+c会终止进程

```shell
#进入到ElasticSearch安装目录下
cd elasticsearch-7.0.0
bin/elasticsearch 
#或者
./bin/elasticsearch
```

后台启动：

```shell
#进入到ElasticSearch安装目录下
cd elasticsearch-7.0.0
bin/elasticsearch -d
#或者
./bin/elasticsearch -d
```



##### 8.验证ElasticSearch是否启动成功

###### 方法一：启动过程中，当看到日志输出started的时候则说明ElasticSearch启动成功

![image-20201206100010828](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201206100010828.png)

###### 方法二：浏览器输入虚拟机地址:9200，比如：http://172.21.0.17:9200

输入下面信息的时候，则说明ElasticSearch启动成功

```
{
  "name" : "weidong",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "_na_",
  "version" : {
    "number" : "7.0.0",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "b7e28a7",
    "build_date" : "2019-04-05T22:55:32.697037Z",
    "build_snapshot" : false,
    "lucene_version" : "8.0.0",
    "minimum_wire_compatibility_version" : "6.7.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

###### 方法三：虚拟机里面执行命令：

```shell
curl 172.21.0.17:9200
```

输入下面信息的时候，则说明ElasticSearch启动成功

![image-20201206100627867](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201206100627867.png)

###### 方法四：虚拟机里面执行命令：

```
ps -ef |grep elasticsearch
```

输入下面信息的时候，则说明ElasticSearch启动成功

![image-20201206100818554](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201206100818554.png)