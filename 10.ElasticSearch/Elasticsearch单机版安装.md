#### CentOS7下Elasticsearch单机版安装

##### 1.ElasticSearch概述

ElasticSearch是一款**基于Apache Lucene**构建的**开源搜索引擎**，它**采用Java编写**并**使用Lucene构建索引**、提供**搜索功能**，ElasticSearch的目标是让全文搜索变得简单，开发者可以通过它简单明了的**RestFul AP**I轻松地实现搜索功能，而不必去面对Lucene的复杂性。ES能够轻松的进行大规模的横向扩展，以支撑PB级的结构化和非结构化海量数据的处理。

##### 2.Elasticsearch能够做什么

数字、文本、地理位置、结构化数据、非结构化数据。适用于所有数据类型

##### 3.ElasticSearch下载（与JDK捆绑版）

说明：与JDK捆绑版不需要单独安装JDK

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

##### 6.安装

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
#/config/elasticsearch.yml配置外网访问 默认外网无法访问 打开network.host注释 修改为
network.host: 0.0.0.0
```

![image-20201205233449339](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201205233449339.png)

![image-20201205233632053](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201205233632053.png)

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
# /etc/security/limits.conf中添加如下内容
* soft nofile 65536
* hard nofile 65536
* soft nproc 4096
* hard nproc 4096
```

