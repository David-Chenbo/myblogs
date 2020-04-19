# pip安装python包的三种方法

#### 1、从官方库下载

pip install 包名

```
pip install pandas
```
从官方库下载有时候速度很慢

#### 2、从国内镜像下载


> ```
> 豆瓣：https://pypi.douban.com/simple
> 中国科学技术大学：https://mirrors.ustc.edu.cn/pypi/web/simple/
> 清华大学TUNA：https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple/
> 阿里云：https://pypi.mirrors.ustc.edu.cn/simple/
> ```



可以在使用pip install -i 国内镜像地址 包名

```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pandas
```

* 如果不想每次输这一长串地址，你也可以把url写入pip配置文件中，这样每次执行pip的时候就不需要指定源地址了,将下面的内容写入到pip.ini文件中。

```
[global]trusted-host = pypi.tuna.tsinghua.edu.cnindex-url = https://pypi.tuna.tsinghua.edu.cn/simple/
```

#### 3、安装whl文件

从下面网站下载相应包的whl文件
https://www.lfd.uci.edu/~gohlke/pythonlibs/#twisted

```
下载 pandas‑0.23.4‑cp36‑cp36m‑win_amd64.whl
pip intall whl的地址/pandas‑0.23.4‑cp36‑cp36m‑win_amd64.whl
```


