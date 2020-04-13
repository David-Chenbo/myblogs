#### 状态码200表示访问成功


```python
import requests
r = requests.get("http://www.baidu.com")
r.status_code
```




    200




```python
# 使用utf-8 编码打印内容
r.encoding = "utf-8"
print(r.text[:100])
```

    <!DOCTYPE html>
    <!--STATUS OK--><html> <head><meta http-equiv=content-type content=text/html;charse


####  Requests库中的两个重要对象

* Response  返回对象包含爬虫返回的内容
* Request


```python
import requests
r = requests.get("http://www.baidu.com")
print(r.status_code)
print(type(r))
print(r.headers)
```

    200
    <class 'requests.models.Response'>
    {'Cache-Control': 'private, no-cache, no-store, proxy-revalidate, no-transform', 'Connection': 'keep-alive', 'Content-Encoding': 'gzip', 'Content-Type': 'text/html', 'Date': 'Tue, 24 Mar 2020 03:42:02 GMT', 'Last-Modified': 'Mon, 23 Jan 2017 13:27:36 GMT', 'Pragma': 'no-cache', 'Server': 'bfe/1.0.8.18', 'Set-Cookie': 'BDORZ=27315; max-age=86400; domain=.baidu.com; path=/', 'Transfer-Encoding': 'chunked'}


### Response对象的属性

|属性|说明|
|:---|:---|
|r.status_code|HTTP请求的返回状态，200表示返回成功，404表示失败,其他也是失败|
|r.text|HTTP响应内容的字符串形式，即，url对应的页面内容|
|r.encoding|从HTTP header中猜测的响应内容的编码方式|
|r.apparent_encoding|从内容中分析出的响应内容的编码方式（备选编码方式）|
|r.content|HTTP响应内容的二进制格式|


```python
import requests
r = requests.get("http://www.baidu.com")
r.status_code
```




    200




```python
print(r.text[:100])
```

    <!DOCTYPE html>
    <!--STATUS OK--><html> <head><meta http-equiv=content-type content=text/html;charse



```python
r.encoding #从头部分析编码内容
```




    'ISO-8859-1'




```python
r.apparent_encoding  #从文本中分析可能的编码格式 
```




    'utf-8'




```python
r.encoding ='utf-8'
```


```python
print(r.text[:100])
```

    <!DOCTYPE html>
    <!--STATUS OK--><html> <head><meta http-equiv=content-type content=text/html;charse


### 爬取网页的通用代码框架


```python
import requests
def getHTMLText(url):
    try:
        r = requests.get(url,timeout=30)
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        return r.text[:100]
    except:
        return "产生异常"
```


```python
if __name__ == "__main__":
    url = "http://www.baidu.com"
    print(getHTMLText(url))
```

    <!DOCTYPE html>
    <!--STATUS OK--><html> <head><meta http-equiv=content-type content=text/html;charse



```python
if __name__ == "__main__":
    url = "http:www.baidu.com"
    print(getHTMLText(url))
```

    产生异常


#### 理解Requests库的异常

|异常|说明|
|:---|:---|
|requests.ConnectionError|网络连接异常，如DNS查询失败，拒绝连接等|
|requests.HTTPError|HTTP错误异常|
|requests.URLRequired|URL缺失异常|
|requests.TooManyRedirects|超过最大重定向次数，产生重定向异常|
|requests.ConnectTimeout|连接远程服务器超时异常|
|requests.Timeout|请求URL超时，产生超时异常|

|异常|说明|
|:---|:---|
|r.raise_for_status()|如果不是200，产生异常requests.HTTPError（作用是指返回为200，连接正常，如果不是，则是异常！）|

#### Requests 库的7个主要方法（Requests类继承了request对象的六种方法））

|方法|说明|
|:---|:---|
|requests.request()|构造一个请求，支撑以下各方法的基础方法|
|requests.get()|获取HTML网页的主要方法，对应于HTTP的GET|
|requests.head()|获取HTML网页头信息的主要方法，对应于HTTP的HEAD|
|requests.post()|向HTML网页提交POST请求的方法，对应于HTTP的POST|
|requests.put()|向HTML网页提交PUT请求的方法，对应于HTTP的PUT|
|requests.patch()|向HTML网页提交局部修改请求，对应于HTTP的PATCH|
|requests.delete()|向HTML页面提交删除请求,对应于HTTP的DELETE|

HTTP协议
HTTP,Hypertext Transfer Protocol,超文本传输协议
HTTP是一种基于“请求与相应”模式的，无状态的应用层协议
ps:用户请求，服务器响应，无状态指第一次请求和第二次请求无关联，应用层协议指的是工作在http协议之上

HTTP协议采用URL作为定位网络资源的标识
URL格式  http://host[:port][path]
host:合法的Internet主机域名或IP地址
port:端口号，缺省端口号为80
path:请求资源的路径
    
举例
http://www.bit.edu.cn
http://220.181.111.188/duty

HTTP URL的理解
URL是通过HTTP协议存取资源的Internet路径，一个URL对应一个数据资源

* HTTP协议对资源的操作

|方法|说明|
|:---|:---|
|GET|请求获取URL位置的资源|
|HEAD|请求获取URL位置资源的响应消息报告，即获取该资源的头部信息|
|POST|请求向URL位置的资源后附加新的数据|
|PUT|请求向URL位置存储一个资源，覆盖原URL位置的资源|
|PATCH|请求局部更新URL位置的资源，即改变该处资源的部分内容（节省带宽）|
|DELETE|请求删除URL位置存储的资源|

### requests.request(method,url.**Kwargs)


```python
method:请求方法，对应get/put/post等7种
    r = requests.request('GET',url,**Kwargs)
    r = requests.request('HEAD',url,**Kwargs)
    r = requests.request('POST',url,**Kwargs)
    r = requests.request('PUT',url,**Kwargs)
    r = requests.request('PATCH',url,**Kwargs)
    r = requests.request('delete',url,**Kwargs)
    r = requests.request('OPTIONS',url,**Kwargs) 

url : 要控制页面的url链接
    http url的理解：
url是通过http协议存取资源的Internet路径，一个url对应一个数据资源。

**Kwargs:控制访问的参数，共13个
    params:字典或字节序列，作为参数增加到url中
    data:字典、字节序列或文件对象，作为request的内容（也就是url对应位置作为数据存储）
    json:JSON格式的数据，作为Request的内容
    headers:字典，HTTP定制头
    cookies:字典或CookieJar,Request中的cookie
    auth:元组，支持HTTP认证功能
    files:字典类型：传输文件
    timeout:设定超时时间，秒为单位
    proxies:字典类型，设定访问代理服务器，可以增加登录认证
    allow_redirects:True/False,默认为True,重定向开关
    stream:True/False,默认为True,获取内容立即下载开关
    verify:True/False,默认为True,认证SSH证书开关
    cert:本地SSL认证路径
```


```python
# params:字典或字节序列，作为参数增加到url中
kv ={'key1':'value1','key2':'value2'}imp
r = requests.request('GET','http://python123.io/ws',params=kv)
print(r.url)
```

    https://python123.io/ws?key1=value1&key2=value2



```python
#    data:字典、字节序列或文件对象，作为Request的内容
kv ={'key1':'value1','key2':'value2'}
r = requests.request('POST','http://python123.io/ws/demo.html',data=kv)
print(r.url)
```

    http://python123.io/ws/demo.html



```python
#   json:JSON格式的数据，作为Request的内容
kv ={'key1':'value1'}
r = requests.request('POST','http://python123.io/ws',json=kv)
print(r.url)
```

    http://python123.io/ws



```python
# headers:字典，HTTP定制头
hd = {'user-agent':'Chrom/10'}
r = requests.request('POST','http://python123.io/ws',headers = hd)
print(r.url)
```

    http://python123.io/ws



```python
#  files:字典类型：传输文件
fs = {'file':open('data.xls','rb')}
r = requests.request('POST','http://python123.io/ws',files=fs)
```


```python
#    timeout:设定超时时间，秒为单位
r = requests.request('GET','http://www.baidu.com',timeout=10)
```


```python
#  proxies:字典类型，设定访问代理服务器，可以增加登录认证
pxs = {'http':'http://user:pass@10.10.10.1:4321',
      'http':'http://10.10.10.1:4321'}
r = requests.request('GET','http://www.baidu.com',proxies=pxs)
# 防止逆追踪
```

#### Requests库的get()方法

r = requests.get(url,params =None,**kwargs)
    requests---返回一个包含服务器资源的Response对象 --Response
    get-- 构造一个向服务器请求资源的Request对象

url:要获取页面的URL链接
params:url中的额外参数，字典或字节流格式，可选
**kwargs:12个控制访问的参数    

### Requests库的head()方法


```python
r = requests.head(url,**kwargs)

url:要获取页面的URL链接
**kwargs:13个控制访问的参数
```


```python
r = requests.head("http://httpbin.org/get")
r.headers
```




    {'Date': 'Thu, 20 Feb 2020 05:39:43 GMT', 'Content-Type': 'application/json', 'Content-Length': '307', 'Connection': 'keep-alive', 'Server': 'gunicorn/19.9.0', 'Access-Control-Allow-Origin': '*', 'Access-Control-Allow-Credentials': 'true'}




```python
print(r.text)
```

    


#### Requests库的post()方法


```python
requests.post(url,data = None,json=None,**Kwargs)

url:要获取页面的URL链接
data:字典、字节序列或文件，Request的内容    
json:JSON格式的数据，Request的内容    
**kwargs:11个控制访问的参数
```


```python
payload = {'key1':'value1','key2':'value2'}
r = requests.post('http://httpbin.org/get',data = payload)
print(r.text)
```

    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
    <title>405 Method Not Allowed</title>
    <h1>Method Not Allowed</h1>
    <p>The method is not allowed for the requested URL.</p>
    


#### Requests库的put()方法


```python
requests.put(url,data = None,**Kwargs)

url:要更新页面的URL链接 
data:字典、字节序列或文件，Request的内容
**kwargs:12个控制访问的参数
```

#### Requests库的patch()方法


```python
requests.patch(url,data = None,**Kwargs)

url:要更新页面的URL链接 
data:字典、字节序列或文件，Request的内容
**kwargs:12个控制访问的参数
```

#### Requests库的delete()方法


```python
requests.delete(url,**Kwargs)

url:要删除页面的URL链接 
**kwargs:13个控制访问的参数
```
