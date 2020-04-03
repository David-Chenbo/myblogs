### 1、信息标记的三种形式 

这节我们来说一些信息标记的三种方法，什么是信息的标记，我的理解就是将信息按照格式组织起来，以便更好的理解其含义，有类似字典的结构，比如一个人有本名和笔名，那如果有人问，这是两个名字怎么是一个人呢？你就可以说，一个是本名，一个是笔名。

**信息的标记**
* 形成信息组织结构，增加信息维度
* 有利于通讯，存储和展示
* 标记的结构和信息一样有着重要的价值
* 有利于程序理解与处理，应用

我们结合HTML的信息标记来理解一下：

我们知道HTML是Hyper Text Markup Language的缩写，那其实HTML就是WWW（World Wide Web）的信息组织方式，它能将声音、图像、视频这些超文本嵌入到文本中去，HTML是通过预定义的<>...</>标签形式组织不同类型的信息。如我们之前将尝试用的demo页面：

 ![img](https://img-blog.csdnimg.cn/20200206105507637.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01BUlNfMDk4,size_16,color_FFFFFF,t_70) 

这里就有html、head、body、p、a这几种标签来标识。

目前国际公认的信息标记的三种形式分别是：**XML**、**JSON**、**YAML**，我们来分别介绍一下：

* **XML（eXtensible Markup Language）**是一种类似HTML的标记语言，以标签为主来构建信息，如下面这条语句：

```
<img src = "china.jpg" size = "10">...</img>
```


其嵌入注释的形式也是跟HTML一样的形式，我们可以说XML是基于HTML的一种通用型语言表达格式

*  **JSON（JavaScript Object Notion）**是JavaScript中面向对象语言的一种表达实行，其采用有类型的键值对来组织信息，如（键key）“name”：（值value）“北京理工大学”， 

```
"name":"北京理工大学"
```

 如果值为数字，可以不适用“”双引号，而当我们一个键key对应的值有多个值时，可以用[]，并且中间用","逗号隔开，而如果使用嵌套的话，则使用{}大括号隔开 

```
"name"：{
    "newName"："北京理工大学",
    "oldName"："延安自然科学院"
}
```

*  **YMAL（YAML Ain't Markup Language）**采用**无类型的键值对**key : value，即没有双引号，如: 

```
name:北京理工大学
```

而YAML采用嵌套的形式时，使用缩进来表示，跟python语法类似：

```
name:
    newName:北京理工大学
    oldName:延安自然科学院
```

YAML在表达并列关系时使用-来表示：

```
name:
    -北京理工大学
    -延安自然科学院
```

YAML还可以使用一个|竖线号来表示整块数据，此时数据可能跨越多行，且使用#来表示注释：

```
text: |   # 简介
上海健康医学院诞生于“健康中国”新时代，是一所上海市属应用技术型本科医学院校。
学校地处浦东张江科学城国际医学园区，拥有北苑、南苑和新南苑三个生机勃勃的校区。
学校现有在校生1.5万余人，开设了14个本科专业和27个专科专业，
具有临床医学、护理学、康复治疗学、生物医学工程、药学、医学影像技术、医学检验技术、健康服务与管理、卫生检验与检疫、临床工程技术等多个特色专业。
其中，临床工程技术、医疗产品管理等专业为全国首创，健康服务与管理、医学影像技术等多个专业填补上海市空白。
```

事实上，目前世界上所有信息都可以使用这三种组织形式来表达，使其更加有效的发挥信息的作用。

### 2、三种信息标记形式的比较

首先，我们看一下我们之前讲的三种形式的不同点在哪：

*   **XML：是一种用<>...标签标记信息的表达形式**  

```
<name>...</name>
    <name />
   <!--  -->
```

*   **JSON：是一种用有类型的键值对来标记信息的表达形式** 

```
"key":"value"
"key":"["value1","value2"]
"key":{"subkey":"subvalue"}
```

*     **YAML：是一种用无类型的键值对来表达信息的表达形式** 

```
"key":"value"
"key":"["value1","value2"]
"key":{"subkey":"subvalue"}
```

下面我们结合各个类型实例来看一下：

```
# XML实例
<person>
    <firstName>Tian</firstName>
    <lastName>Song</lastName>
    <address>
        <streetAddr>中关村南大街5号</streetAddr>
        <city>北京市</city>
        <zipcode>100081</zipcode>
    </address>
    <prof>Computer System</prof><prof>Security</prof>
</person>
```

```
# JSON实例
{
    "firstName":"Tian",
    "lastName":"Song",
    "address":{
                "streetAddr":"中关村南大街5号",
                "city":"北京市",
                "zipcode":"100081",
              },
    "prof":["Computer System","Security"]
}
```

```
# YAML类型
firstName:Tian
lastName:Song
address:
    streetAddr:中关村南大街5号
    city:北京市
    zipcode:100081
prof:
-Compuer System
-Security
```

从这三个实例上我们直观的感受就是，有效信息所占的比例不同，那么接下来我们在深入对比一下：

* XML：最早的通用信息的标记语言，可扩展性好，但繁琐。广泛应用于Internet上的信息交互与传递
* JSON：信息有类型，适合程序处理（js），较XML简洁。应用于移动应用云端和结点的信息通信，但无注释
* YAML：信息无类型，文本信息比例最高，可读性好。应用于各类系统的配置文件，有注释易读

三种方式都有其各自适合使用的领域和价值，但是都反映了信息与信息之间关系的价值。

### 3、信息提取的一般方法

下面我们来介绍一下如何在获取的信息中提取自己所需要的的信息：

* 方法一：完整解析信息的标记形式，再提取关键信息

如XML，JSON，YAML

简单说我们需要标记解析器，例如：bs4库的标签树遍历
**优点**：信息解析准确。
**缺点**：提取过程繁琐，速度慢。

* 方法二：无视标记形式，直接搜索关键信息

如：搜索

对信息的文本查找函数即可
**优点**：提取过程简洁，速度较快。
**缺点**：提取过程准确性与信息内容相关。

* 融合方法：结合形式解析与搜索方法，提取关键信息。

XML、JSON、YAML 搜索
需要标记解析器以及文本查找函数
下面我们以bs4库为例来解释一下：

实例：提取HTML中所有的URL连接：

思路：1）搜索到所有<a>标签
     2）解析<a>标签格式，提取href后的链接内容

我们继续引用之前的demo实例来帮助理解：


```python
import requests
r = requests.get("http://python123.io/ws/demo.html")
demo=r.text
from bs4 import BeautifulSoup
soup = BeautifulSoup(demo,"html.parser")
for link in soup.find_all('a'):
    print(link.get('href'))
```

    http://www.icourse163.org/course/BIT-268001
    http://www.icourse163.org/course/BIT-1001870001


我们看一下demo的文本内容：

```
<html>
 <head>
  <title>
   This is a python demo page
  </title>
 </head>
 <body>
  <p class="title">
   <b>
    The demo python introduces several python courses.
   </b>
  </p>
  <p class="course">
   Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:
   <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">
    Basic Python
   </a>
   and
   <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">
    Advanced Python
   </a>
   .
  </p>
 </body>
</html>
```



我们看到我们已经得到了这个HTML中所包含的url链接内容。

### 4、基于bs4库的HTML内容查找方法


```python
import requests
r = requests.get("http://python123.io/ws/demo.html")
r.status_code
```




    200




```python
demo = r.text
demo
```




    '<html><head><title>This is a python demo page</title></head>\r\n<body>\r\n<p class="title"><b>The demo python introduces several python courses.</b></p>\r\n<p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:\r\n<a href="http://www.icourse163.org/course/BIT-268001" class="py1" id="link1">Basic Python</a> and <a href="http://www.icourse163.org/course/BIT-1001870001" class="py2" id="link2">Advanced Python</a>.</p>\r\n</body></html>'



* BeautifulSoup库提供了一个下面这种非常极其特别常用的方法：

```
<>.find_all(name,attrs,recursive,string,**kwargs)
```

* name：对标签名称的检索字符串 

返回一个列表类型，存储查找的结果，示例如下：


```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(demo,"html.parser")
soup.find_all('a')
```




    [<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>,
     <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>]



可以看出来，只需要在()内增加参数标签，即可查找标签内容，如果想要一次查询多个，则可以使用列表将若干参数添加在同一个列表中，另外，如果我们想要查找所有标签，只需要使用循环将参数置为True：


```python
soup.find_all(['a','b'])
```




    [<b>The demo python introduces several python courses.</b>,
     <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>,
     <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>]




```python
for tag in soup.find_all(True):
    print(tag.name)
```

    html
    head
    title
    body
    p
    b
    p
    a
    a


如果再提一个要求，如只显示b开头的标签，如b和body，我们这里需要导入一个新的第三方库，正则表达式库re，我们先看一下示例，后面我们会讲到这个库


```python
import re
for tag in soup.find_all(re.compile('b')):
    print(tag.name)
```

    body
    b


* attrs：对标签属性值的检索字符串，可标注属性检索

如果我们需要查找p标签中包含course字符串的信息，看示例：


```python
soup.find_all('p','course')
```




    [<p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:
     <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a> and <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>.</p>]



如果我们查找以id属性等于link1的标签元素：


```python
soup.find_all(id = 'link1')
```




    [<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>]



下面我们查找一个并不存在的id域为link的标签元素：


```python
soup.find_all(id = 'link')
```




    []



这里返回了一个空的列表，我们发现，前面的这个元素的返回值也是列表类型，且属性值必须为精确类型的，如果我们需要部分内容，如包含link或者部分内容，就仍然需要正则表达式：


```python
import re
soup.find_all(id = re.compile('link'))
```




    [<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>,
     <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>]



其实这就类似我们在搜索引擎搜索一个东西，但是我们不知道我们要搜索的内容的全名，这个正则表达式就是类似这样，我们搜索包含link的所有内容并返回；

* recursive：是否对子孙全部检索，默认True

默认我们搜索的是从某一个标签开始的后续所有子孙节点的标签信息，如果是搜索部分的子孙节点，则需要置为False：


```python
soup.find_all('a')
```




    [<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>,
     <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>]




```python
soup.find_all('a',recursive = False)
```




    []



这里我们将recursive置为False后，返回的是一个空列表，说明从soup根节点开始，儿子节点上没有名为a的标签信息，a标签应该在子孙的后续标签节点中。

* string：<>...</>中字符串区域的检索字符串


```python
soup.find_all(string = "Basic Python")
```




    ['Basic Python']



这里检索到的字符串信息需要指定正确的Basic Python才可以，另外我们还可以使用正则表达式来检索不太全面的信息：


```python
import re
soup.find_all(string = re.compile("python"))
```




    ['This is a python demo page',
     'The demo python introduces several python courses.']



这里我们就很容易检索到包含python字符串的信息，且返回值形式还是列表。

BeautifulSoup库分find_all()方法非常常用，另外它还要7个常用的扩展方法：

|方法|说明|
|:---|:---|
|<>.find()|搜索且只返回一个结果，字符串类型，同.find_all()参数|
|<>.find_parents()|在先辈节点中搜索，返回列表类型，同.find_all()参数|
|<>.find_parent()|在先辈节点中返回一个结果，字符串类型，同.find()参数|
|<>.find_next_siblings()|在后续平行节点中搜索，返回列表类型，同.find_all()参数|
|<>.find_next_sibling()|在后续平行节点中返回一个结果，字符串类型，同.find()参数|
|<>.find_previous_siblings()|在前序平行节点中搜索，返回列表类型，同.find_all()参数|
|<>.find_previous_sibling()|在前序平行节点中返回一个搜索结果，字符串类型，同.find()参数|

这7种方法就是返回值的个数不相同，其用法与find_allw()无大的差别，多多练习熟练使用也很简单。


```python

```
