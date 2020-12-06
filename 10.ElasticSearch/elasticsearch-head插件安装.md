##### elasticsearch-head插件安装

##### 1.elasticsearch-head介绍

elasticsearch-head是一个用来浏览、与Elastic Search簇进行交互的web前端展示插件。elasticsearch-head是一个用来监控Elastic Search状态的客户端插件。

##### 2.elasticsearch-head前期准备

###### 2.1elasticsearch-head官网地址：https://github.com/mobz/elasticsearch-head

![image-20201206104950013](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201206104950013.png)

###### 2.2nodejs官网地址（中文）：http://nodejs.cn/

下载地址：https://cdn.npm.taobao.org/dist/node/v14.15.1/node-v14.15.1-linux-x64.tar.xz

![image-20201206104631626](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201206104631626.png)

##### 3.elasticsearch-head安装启动

```shell
#创建Node.js,head插件安装目录
mkdir node elasticsearch-head
#进入Node.js安装目录
cd node
#下载Node.js head插件运行依赖node
wget https://cdn.npm.taobao.org/dist/node/v14.15.1/node-v14.15.1-linux-x64.tar.xz
#解压Node.js
tar -xf node-v14.15.1-linux-x64.tar.xz
#重命名Node.js
mv node-v14.15.1-linux-x64 nodejs
#使用淘宝的镜像库进行下载，速度很快，使用cnpm替换npm
npm install -g cnpm --registry=https://registry.npm.taobao.org
#确认一下nodejs下bin目录是否有node 和npm文件和cnpm  建立软连接，变为全局
ln -s /root/soft/node/nodejs/bin/npm /usr/local/bin/ 
ln -s /root/soft/node/nodejs/bin/node /usr/local/bin/
ln -s /root/soft/node/nodejs/bin/cnpm  /usr/local/bin/
#查询node版本和cnpm，同时查看是否安装成功
node -v
cnpm -v
#进入head插件安装目录
cd elasticsearch-head

##############官方推荐的方式开始 此种方式比较慢  个人不推荐##############
##git克隆elasticsearch-head 这个下载比较慢 大概5分钟左右 耐心等待 大概5分钟左右 也可以直接下载到windows 通过Xftp、WinSCP等工具上传至head插件安装目录或者启动淘宝的镜像库下载
#git clone git://github.com/mobz/elasticsearch-head.git
##进入head插件解压目录 此处不是上面的head插件安装目录而是解压目录
#cd elasticsearch-head
##安装elasticsearch-head插件需要的依赖包 这个下载比较慢 大概5分钟左右 耐心等待 或者使用淘宝的镜像库进行下载，速度很快
#npm install
##############官方推荐的方式结束 此种方式比较慢  个人不推荐##############

##############使用淘宝镜像库方式开始##############
#下载head插件
wget https://github.com/mobz/elasticsearch-head/archive/master.zip
#解压head插件
unzip master.zip
#进入head插件解压目录 此处不是上面的head插件安装目录而是解压目录
cd elasticsearch-head-master
cnpm install
##############使用淘宝镜像库方式结束##############
#启动head插件  这个必须在head插件解压目录下执行
npm run start #前台启动head插件 ctrl+c会退出
nohup npm start& #后台启动head插件
```

![image-20201206183338566](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201206183338566.png)

![image-20201206184608606](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201206184608606.png)

##### 4.elasticsearch-head验证

###### 4.1打开浏览器输入：http://虚拟机地址:9100/

![image-20201206184821820](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201206184821820.png)

###### 4.2查看grunt服务进程号

ps -ef | grep grunt

![image-20201206190245307](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201206190245307.png)

##### 5.elasticsearch-head关闭

###### 以前台启动方式的关闭：

直接ctrl+c退出即可

###### 以后台启动方式的关闭：

ps -ef|grep grunt

或者查看9100端口占用进程号

netstat -tunlp|grep 9100

查看head插件进程号：这里是10609

kill -9  10609

即可

![image-20201206190048387](C:\Users\VULCAN\AppData\Roaming\Typora\typora-user-images\image-20201206190048387.png)