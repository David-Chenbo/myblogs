# Eclipse配置本地Hadoop开发环境 解决Hadoop问题---HADOOP_HOME and hadoop.home.dir are unset.

> Eclipse安装Hadoop插件配置Hadoop开发环境
一.问题描述：windows本地调试Hadoop程序时报错
错误信息：

```php
HADOOP_HOME and hadoop.home.dir are unset.    //通过本地搭建hadoop环境解决

org.apache.hadoop.io.nativeio.NativeIO$Windows.access0(Ljava/lang/String;I)Z    //通过下载winutils解决
```

![img](https://upload-images.jianshu.io/upload_images/4391407-1101ff3373be29a9.png?imageMogr2/auto-orient/strip|imageView2/2/w/1153/format/webp)

报错

其原因是需要在windows本地搭建Hadoop环境，下载winutils文件，并将hadoop-2.8.4包内的bin文件替换，将下载文件中hadoop.dll放到C：\Windows\System32下
二.解决过程如下：
1.下载hadoop，去官网下载对应的hadoop版本，我在linux集群搭建的是hadoop-2.8.4，因此将hadoop-2.8.4下载到windows本地

- [Hadoop下载地址](https://links.jianshu.com/go?to=https%3A%2F%2Fmirrors.tuna.tsinghua.edu.cn%2Fapache%2Fhadoop%2Fcommon%2F)

- 可以通过镜像下载

  ![img](https://upload-images.jianshu.io/upload_images/4391407-45d201966caa5e15.png?imageMogr2/auto-orient/strip|imageView2/2/w/916/format/webp)

  hadoop下载

  2.解压Hadoop到本地目录:

  ![img](https://upload-images.jianshu.io/upload_images/4391407-625a281e3cce76bb.png?imageMogr2/auto-orient/strip|imageView2/2/w/847/format/webp)

  解压Hadoop

  解压时可能出现解压出错的情况，这时候把winrar设置为管理员运行即可

  ![img](https://upload-images.jianshu.io/upload_images/4391407-78aa76f352c5b4d9.png?imageMogr2/auto-orient/strip|imageView2/2/w/1115/format/webp)

  winrar解压

  3.配置环境变量：
  (1)新建HADOOP_HOME环境变量:
  HADOOP_HOME的值为解压的hadoop-2.8.4的路径

![img](https://upload-images.jianshu.io/upload_images/4391407-34cb338b37543bcb.png?imageMogr2/auto-orient/strip|imageView2/2/w/608/format/webp)

HADOOP_HOME环境变量



(2)添加Path：
Path新增：

```undefined
%HADOOP_HOME%\bin
```

![img](https://upload-images.jianshu.io/upload_images/4391407-da7caa92622e10da.png?imageMogr2/auto-orient/strip|imageView2/2/w/712/format/webp)

新增Path


4.修改配置文件：在hadoop-2.8.4\etc\hadoop目录下(1)修改hadoop-env.cmd,改为自己的设置jdk目录



```bash
set JAVA_HOME=C:\Program Files\Java\jdk1.8.0_221
```

![img](https://upload-images.jianshu.io/upload_images/4391407-376dd38e8c61754d.png?imageMogr2/auto-orient/strip|imageView2/2/w/821/format/webp)

hadoop-env.cmd设置

5.下载winutils的windows版本：
[下载地址](https://links.jianshu.com/go?to=%5Bhttps%3A%2F%2Fgithub.com%2Fsteveloughran%2Fwinutils%5D(https%3A%2F%2Fgithub.com%2Fsteveloughran%2Fwinutils))

- 点击下载

  ![img](https://upload-images.jianshu.io/upload_images/4391407-c69764014b0a8133.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

  下载winutils

- 解压

  ![img](https://upload-images.jianshu.io/upload_images/4391407-2751d1c32f75fdbe.png?imageMogr2/auto-orient/strip|imageView2/2/w/1124/format/webp)

  解压

  因为我自己用的hadoop2.8.4所以我就近用的2.8.3

  ![img](https://upload-images.jianshu.io/upload_images/4391407-acbfa24874dd2bcc.png?imageMogr2/auto-orient/strip|imageView2/2/w/672/format/webp)

  选择2.8.3

- 将hadoop-2.8.3下bin文件夹与本地hadoop-2.8.4下的bin文件夹替换

  ![img](https://upload-images.jianshu.io/upload_images/4391407-c27ffa32c9c84755.png?imageMogr2/auto-orient/strip|imageView2/2/w/1169/format/webp)

  替换

![img](https://upload-images.jianshu.io/upload_images/4391407-ebf47e1d8254ac85.png?imageMogr2/auto-orient/strip|imageView2/2/w/946/format/webp)

winutils.exe文件

- 将替换后的hadoop-2.8.4中的bin文件夹下的hadoop.dll拷贝到C:\Windows\System32目录下

![img](https://upload-images.jianshu.io/upload_images/4391407-c22cb75dafdfc2a4.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

拷贝

6.问题解决！

