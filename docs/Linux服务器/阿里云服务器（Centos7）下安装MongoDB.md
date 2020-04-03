# 阿里云服务器（Centos7）下安装MongoDB

### 0、配置云服务器的安全组

因为我用的是阿里云的服务器，所以就以阿里云的配置来说明。因为从外网访问服务器，需要开放一定的端口，所以要对服务器的访问规则进行配置。阿里云是用安全组来管理这些规则的，所以需要对安全组进行配置。

阿里云为了安全起见，默认只开放了22、80等少数端口。而MongoDB默认采用8888端口，因此在安全组配置中，需要将此端口开放。

 **设置过程：云服务器管理控制台》云服务器ECS》网络和安全》安全组》配置规则》添加安全组规则**

![1584537929138](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1584537929138.png)

### 1、mongodb介绍

MongoDB 是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。他支持的数据结构非常松散，是类似json的bson格式，因此可以存储比较复杂的数据类型。Mongo几乎可以实现类似关系数据库单表查询的绝大部分功能，还支持对数据建立索引。

### 2、下载解压MongoDB

官网地址：[https://www.mongodb.com/download-center?jmp=nav#community](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.mongodb.com%2Fdownload-center%3Fjmp%3Dnav%23community)
我使用的安装包地址：https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel70-4.0.9.tgz

百度网盘下载：https://pan.baidu.com/s/1d_1J6ALOaXFCoXENzQBCXQ

提取码：cx1l

 1）定位到 /usr/local目录 

```Linux
/usr/local
```

![1584538633290](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1584538633290.png)

 2）下载安装包到 usr/local/ 目录下，然后解压

```shell
wget  https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel70-4.0.9.tgz
tar -zxvf mongodb-linux-x86_64-rhel70-4.0.9.tgz
```

![1584539283498](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1584539283498.png)

3）重命名文件夹 

```Linux
mv mongodb-linux-x86_64-rhel70-4.0.9.tgz mongoDB
```
4）结果如图
![1584539363180](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1584539363180.png)

### 3、配置环境变量和初始化操作

 1）配置环境变量 

```
vi /etc/profile
```

 按下字母 I 键，开始编辑，
在 export PATH USER LOGNAME MAIL HOSTNAME HISTSIZE HISTCONTROL 一行的上面添加如下内容: 

```
export PATH=/usr/mongodb/mongodb-4.0.10/bin:$PATH
```

![1584539592560](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1584539592560.png)

 按ESC退出编辑，并输入 :wq 保存退出 

 2）再通过下面的命令使环境变量生效： 

```
source /etc/profile
```

 3）回到mongodb目录下创建数据库目录 

```
cd /usr/local/mongoDB/
```

 4）在该目录下新建配置文件 

```
 touch mongodb.conf 
```

5）创建数据库目录

```
mkdir db
```

6）创建日志目录

```
mkdir log
```

7）设置文件夹权限，方便操作

```
chmod 755 db
chmod 755 log
```

8）创建日志文件

```
cd log
touch mongodb.log
```

![1584542751333](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1584542751333.png)

### 4、修改配置文件内容

1）在mongodb.conf 中添加以下内容

```
port=27017 #端口
dbpath= /usr/local/mongoDB/db #数据库存文件存放目录
logpath= /usr/local/mongoDB/log/mongodb.log #日志文件存放路径
logappend=true #使用追加的方式写日志
fork=true #以守护进程的方式运行，创建服务器进程
maxConns=100 #最大同时连接数
noauth=true #不启用验证
journal=true #每次写入会记录一条操作日志（通过journal可以重新构造出写入的数据）。
             #即使宕机，启动时wiredtiger会先将数据恢复到最近一次的checkpoint点，然后重放后
续的journal日志来恢复。
storageEngine=wiredTiger  #存储引擎，有mmapv1、wiretiger、mongorocks
bind_ip = 0.0.0.0  #设置成全部ip可以访问，这样就可以在windows中去连虚拟机的MongoDB，也可以
设置成某个网段或者某个ip
```

### 5、启动mongodb

```
mongod --config /usr/local/mongoDB/mongodb.conf
```

![1584543276333](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1584543276333.png)

# ![1584543332993](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1584543332993.png)

### 推荐使用可视化工具连接

推荐使用工具： Studio3T
测试连接图片 

![1584543563305](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1584543563305.png)

![1584543600719](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1584543600719.png)

![1584543618212](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1584543618212.png)

连接成功

怎么下载和使用该工具，可以看我的另外一篇博客：
**MongoDB可视化工具Studio 3T的使用**
https://blog.csdn.net/xyb0926/article/details/92116654

