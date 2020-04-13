### 实例一 京东商品页面的爬取


```python
import requests
r = requests.get("https://item.jd.com/100005687872.html")
r.status_code
```




    200




```python
r.encoding ##这说明京东网站提供页面信息的相关编码
```




    'gbk'




```python
r.text[:1000]
```




    '<!DOCTYPE HTML>\n<html lang="zh-CN">\n<head>\n    <!-- shouji -->\n    <meta http-equiv="Content-Type" content="text/html; charset=gbk" />\n    <title>【中控智慧WX108】企业微信云考勤机WX108  支持人脸指纹识别/手机打卡/无接触考勤 智慧刷脸机复工打卡更安全【行情 报价 价格 评测】-京东</title>\n    <meta name="keywords" content="ZKTecoWX108,中控智慧WX108,中控智慧WX108报价,ZKTecoWX108报价"/>\n    <meta name="description" content="【中控智慧WX108】京东JD.COM提供中控智慧WX108正品行货，并包括ZKTecoWX108网购指南，以及中控智慧WX108图片、WX108参数、WX108评论、WX108心得、WX108技巧等信息，网购中控智慧WX108上京东,放心又轻松" />\n    <meta name="format-detection" content="telephone=no">\n    <meta http-equiv="mobile-agent" content="format=xhtml; url=//item.m.jd.com/product/100005687872.html">\n    <meta http-equiv="mobile-agent" content="format=html5; url=//item.m.jd.com/product/100005687872.html">\n    <meta http-equiv="X-UA-Compatible" content="IE=Edge">\n    <link rel="canonical" href="//item.jd.com/100005687872.html"/>\n        <link rel="dns-prefetch" href="//misc.360buyimg.com"/>\n    <link rel="dns-prefetch" href="//static.360buyimg.com"/'




```python
import requests
url = "https://item.jd.com/100005687872.html"
try:
    r = requests.get(url)
    r.raise_for_status()
    r.encoding = r.apparent_encoding
    print(r.text[:1000])
except:
    print("爬取失败")
```

    <!DOCTYPE HTML>
    <html lang="zh-CN">
    <head>
        <!-- shouji -->
        <meta http-equiv="Content-Type" content="text/html; charset=gbk" />
        <title>【中控智慧WX108】企业微信云考勤机WX108  支持人脸指纹识别/手机打卡/无接触考勤 智慧刷脸机复工打卡更安全【行情 报价 价格 评测】-京东</title>
        <meta name="keywords" content="ZKTecoWX108,中控智慧WX108,中控智慧WX108报价,ZKTecoWX108报价"/>
        <meta name="description" content="【中控智慧WX108】京东JD.COM提供中控智慧WX108正品行货，并包括ZKTecoWX108网购指南，以及中控智慧WX108图片、WX108参数、WX108评论、WX108心得、WX108技巧等信息，网购中控智慧WX108上京东,放心又轻松" />
        <meta name="format-detection" content="telephone=no">
        <meta http-equiv="mobile-agent" content="format=xhtml; url=//item.m.jd.com/product/100005687872.html">
        <meta http-equiv="mobile-agent" content="format=html5; url=//item.m.jd.com/product/100005687872.html">
        <meta http-equiv="X-UA-Compatible" content="IE=Edge">
        <link rel="canonical" href="//item.jd.com/100005687872.html"/>
            <link rel="dns-prefetch" href="//misc.360buyimg.com"/>
        <link rel="dns-prefetch" href="//static.360buyimg.com"/


### 实例2：亚马逊商品页面的爬取


```python
r = requests.get("https://www.amazon.cn/dp/B07CKT8Z57/ref=lp_1397649071_1_1?s=music-players&ie=UTF8&qid=1580539992&sr=1-1")
r.status_code
```




    503



抓取页面发现返回的状态码为503，出现了错误。

我们查看编码方式并转换为备用编码方式(r.apparent_encoding)，但是，返回的页面我们发现还是出现了错误，回想前面的， 有些网站对网络爬虫是由一定的限制的，那么网站怎样自动识别是不是机器爬虫来完成的，这就要看发送的请求头部了


```python
r.encoding
```




    'ISO-8859-1'




```python
r.apparent_encoding 
```




    'utf-8'




```python
r.text
```




    '<!DOCTYPE html>\n<!--[if lt IE 7]> <html lang="zh-CN" class="a-no-js a-lt-ie9 a-lt-ie8 a-lt-ie7"> <![endif]-->\n<!--[if IE 7]>    <html lang="zh-CN" class="a-no-js a-lt-ie9 a-lt-ie8"> <![endif]-->\n<!--[if IE 8]>    <html lang="zh-CN" class="a-no-js a-lt-ie9"> <![endif]-->\n<!--[if gt IE 8]><!-->\n<html class="a-no-js" lang="zh-CN"><!--<![endif]--><head>\n<meta http-equiv="content-type" content="text/html; charset=UTF-8">\n<meta charset="utf-8">\n<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">\n<title dir="ltr">Amazon CAPTCHA</title>\n<meta name="viewport" content="width=device-width">\n<link rel="stylesheet" href="https://images-na.ssl-images-amazon.com/images/G/01/AUIClients/AmazonUI-3c913031596ca78a3768f4e934b1cc02ce238101.secure.min._V1_.css">\n<script>\n\nif (true === true) {\n    var ue_t0 = (+ new Date()),\n        ue_csm = window,\n        ue = { t0: ue_t0, d: function() { return (+new Date() - ue_t0); } },\n        ue_furl = "fls-cn.amazon.cn",\n        ue_mid = "AAHKV2X7AFYLW",\n        ue_sid = (document.cookie.match(/session-id=([0-9-]+)/) || [])[1],\n        ue_sn = "opfcaptcha.amazon.cn",\n        ue_id = \'XP2135TD2DAZ80D51465\';\n}\n</script>\n</head>\n<body>\n\n<!--\n        To discuss automated access to Amazon data please contact api-services-support@amazon.com.\n        For information about migrating to our APIs refer to our Marketplace APIs at https://developer.amazonservices.com.cn/index.html/ref=rm_c_sv, or our Product Advertising API at https://associates.amazon.cn/gp/advertising/api/detail/main.html/ref=rm_c_ac for advertising use cases.\n-->\n\n<!--\nCorreios.DoNotSend\n-->\n\n<div class="a-container a-padding-double-large" style="min-width:350px;padding:44px 0 !important">\n\n    <div class="a-row a-spacing-double-large" style="width: 350px; margin: 0 auto">\n\n        <div class="a-row a-spacing-medium a-text-center"><i class="a-icon a-logo"></i></div>\n\n        <div class="a-box a-alert a-alert-info a-spacing-base">\n            <div class="a-box-inner">\n                <i class="a-icon a-icon-alert"></i>\n                <h4>è¯·è¾\x93å\x85¥æ\x82¨å\x9c¨ä¸\x8bæ\x96¹ç\x9c\x8bå\x88°ç\x9a\x84å\xad\x97ç¬¦</h4>\n                <p class="a-last">æ\x8a±æ\xad\x89ï¼\x8cæ\x88\x91ä»¬å\x8fªæ\x98¯æ\x83³ç¡®è®¤ä¸\x80ä¸\x8bå½\x93å\x89\x8dè®¿é\x97®è\x80\x85å¹¶é\x9d\x9eè\x87ªå\x8a¨ç¨\x8båº\x8fã\x80\x82ä¸ºäº\x86è¾¾å\x88°æ\x9c\x80ä½³æ\x95\x88æ\x9e\x9cï¼\x8cè¯·ç¡®ä¿\x9dæ\x82¨æµ\x8fè§\x88å\x99¨ä¸\x8aç\x9a\x84 Cookie å·²å\x90¯ç\x94¨ã\x80\x82</p>\n                </div>\n            </div>\n\n            <div class="a-section">\n\n                <div class="a-box a-color-offset-background">\n                    <div class="a-box-inner a-padding-extra-large">\n\n                        <form method="get" action="/errors/validateCaptcha" name="">\n                            <input type=hidden name="amzn" value="b2R5Wzj57ZoqaT94aEM0uQ==" /><input type=hidden name="amzn-r" value="&#047;dp&#047;B07CKT8Z57&#047;ref&#061;lp_1397649071_1_1?s&#061;music&#045;players&amp;ie&#061;UTF8&amp;qid&#061;1580539992&amp;sr&#061;1&#045;1" />\n                            <div class="a-row a-spacing-large">\n                                <div class="a-box">\n                                    <div class="a-box-inner">\n                                        <h4>è¯·è¾\x93å\x85¥æ\x82¨å\x9c¨è¿\x99ä¸ªå\x9b¾ç\x89\x87ä¸\xadç\x9c\x8bå\x88°ç\x9a\x84å\xad\x97ç¬¦ï¼\x9a</h4>\n                                        <div class="a-row a-text-center">\n                                            <img src="https://images-na.ssl-images-amazon.com/captcha/bfhuzdtn/Captcha_kjovxoccsq.jpg">\n                                        </div>\n                                        <div class="a-row a-spacing-base">\n                                            <div class="a-row">\n                                                <div class="a-column a-span6">\n                                                    <label for="captchacharacters">è¾\x93å\x85¥å\xad\x97ç¬¦</label>\n                                                </div>\n                                                <div class="a-column a-span6 a-span-last a-text-right">\n                                                    <a onclick="window.location.reload()">æ\x8d¢ä¸\x80å¼\xa0å\x9b¾</a>\n                                                </div>\n                                            </div>\n                                            <input autocomplete="off" spellcheck="false" id="captchacharacters" name="field-keywords" class="a-span12" autocapitalize="off" autocorrect="off" type="text">\n                                        </div>\n                                    </div>\n                                </div>\n                            </div>\n\n                            <div class="a-section a-spacing-extra-large">\n\n                                <div class="a-row">\n                                    <span class="a-button a-button-primary a-span12">\n                                        <span class="a-button-inner">\n                                            <button type="submit" class="a-button-text">ç»§ç»\xadè´\xadç\x89©</button>\n                                        </span>\n                                    </span>\n                                </div>\n\n                            </div>\n                        </form>\n\n                    </div>\n                </div>\n\n            </div>\n\n        </div>\n\n        <div class="a-divider a-divider-section"><div class="a-divider-inner"></div></div>\n\n        <div class="a-text-center a-spacing-small a-size-mini">\n            <a href="https://www.amazon.cn/gp/help/customer/display.html/ref=footer_claim?ie=UTF8&nodeId=200347160">ä½¿ç\x94¨æ\x9d¡ä»¶</a>\n            <span class="a-letter-space"></span>\n            <span class="a-letter-space"></span>\n            <span class="a-letter-space"></span>\n            <span class="a-letter-space"></span>\n            <a href="https://www.amazon.cn/gp/help/customer/display.html/ref=footer_privacy?ie=UTF8&nodeId=200347130">é\x9a\x90ç§\x81å£°æ\x98\x8e</a>\n        </div>\n\n        <div class="a-text-center a-size-mini a-color-secondary">\n          &copy; 1996-2015, Amazon.com, Inc. or its affiliates\n          <script>\n           if (true === true) {\n             document.write(\'<img src="https://fls-cn.amaz\'+\'on.cn/\'+\'1/oc-csi/1/OP/requestId=XP2135TD2DAZ80D51465&js=1" />\');\n           };\n          </script>\n          <noscript>\n            <img src="https://fls-cn.amazon.cn/1/oc-csi/1/OP/requestId=XP2135TD2DAZ80D51465&js=0" />\n          </noscript>\n        </div>\n    </div>\n    <script>\n    if (true === true) {\n        var elem = document.createElement("script");\n        elem.src = "https://images-cn.ssl-images-amazon.com/images/G/01/csminstrumentation/csm-captcha-instrumentation.min._V" + (+ new Date()) + "_.js";\n        document.getElementsByTagName(\'head\')[0].appendChild(elem);\n    }\n    </script>\n</body></html>\n'



我们查看一下我们爬虫发送request请求对象的头部信息，我们可以看到其中有个‘User-Agent’：‘python-requests/2.22.0’，还记得第二节盗亦有道讲到的爬虫规范嘛，这相当于明确告诉亚马逊网站，我是爬虫发送的请求，所以遭到了拒绝




```python
r.request.headers
```




    {'User-Agent': 'python-requests/2.22.0', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'Connection': 'keep-alive'}



可是我们之前还学了更改头部信息，就是用字典添加键值对来完成的，这里我们给头部更换为‘Mozilla/5.0’，这表示这是一个标准的正常浏览器请求，其实也可设置明确的火狐，谷歌浏览器名称，此时再去请求，返回的状态码就是200了。


```python
kv = {'user-agent':'Mozilla/5.0'}
url ='https://www.amazon.cn/apb/page/?handlerName=OctopusDealLandingStreamV2&deals=b4f3071e&marketplaceId=AAHKV2X7AFYLW&showVariations=true&ref_=Oct_DotdV2_PC_2_PE_DOTD_b4f3071e&pf_rd_r=9JT1DH4BSTWHQW7AHES5&pf_rd_p=b589c077-3fcf-4508-9a36-29dd54653ac1&pf_rd_m=A1AJ19PSB66TGU&pf_rd_s=desktop-2'
r = requests.get(url,headers=kv)
r.status_code
```




    200



此时我们验证请求对象头部已经改变，且返回的信息也已经没有提示错误了


```python
r.request.headers
```




    {'user-agent': 'Mozilla/5.0', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'Connection': 'keep-alive'}




```python
r.text[:1000]
```




    '<!doctype html><html lang="zh-cn" class="a-no-js" data-19ax5a9jf="dingo"><!-- sp:feature:head-start -->\n<head><script>var aPageStart = (new Date()).getTime();</script><meta charset="utf-8"/>\n<script type=\'text/javascript\'>var ue_t0=ue_t0||+new Date();</script><!-- sp:feature:cs-optimization -->\n<meta http-equiv=\'x-dns-prefetch-control\' content=\'on\'><link rel="dns-prefetch" href="https://images-cn.ssl-images-amazon.com"><link rel="dns-prefetch" href="https://m.media-amazon.com"><link rel="dns-prefetch" href="https://completion.amazon.com"><script type=\'text/javascript\'>\nwindow.ue_ihb = (window.ue_ihb || window.ueinit || 0) + 1;\nif (window.ue_ihb === 1) {\n\nvar ue_csm = window,\n    ue_hob = +new Date();\n(function(d){var e=d.ue=d.ue||{},f=Date.now||function(){return+new Date};e.d=function(b){return f()-(b?0:d.ue_t0)};e.stub=function(b,a){if(!b[a]){var c=[];b[a]=function(){c.push([c.slice.call(arguments),e.d(),d.ue_id])};b[a].replay=function(b){for(var a;a=c.shift();)b(a[0],a[1],a[2])};b[a]'



* 下面我们来看这个代码的全框架：


```python
import requests
url = "https://www.amazon.cn/dp/B07CKT8Z57/ref=lp_1397649071_1_1?s=music-players&ie=UTF8&qid=1580539992&sr=1-1"
try:
    kv = {'user-agent':'Mozilla/5.0'}
    r= requests.get(url,headers = kv)
    r.raise_for_status()
    r.encoding = r.apparent_encoding
    print(r.text[:1000])
except:
    print("爬取失败")
```

    <!DOCTYPE html>
    <!--[if lt IE 7]> <html lang="zh-CN" class="a-no-js a-lt-ie9 a-lt-ie8 a-lt-ie7"> <![endif]-->
    <!--[if IE 7]>    <html lang="zh-CN" class="a-no-js a-lt-ie9 a-lt-ie8"> <![endif]-->
    <!--[if IE 8]>    <html lang="zh-CN" class="a-no-js a-lt-ie9"> <![endif]-->
    <!--[if gt IE 8]><!-->
    <html class="a-no-js" lang="zh-CN"><!--<![endif]--><head>
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
    <title dir="ltr">Amazon CAPTCHA</title>
    <meta name="viewport" content="width=device-width">
    <link rel="stylesheet" href="https://images-na.ssl-images-amazon.com/images/G/01/AUIClients/AmazonUI-3c913031596ca78a3768f4e934b1cc02ce238101.secure.min._V1_.css">
    <script>
    
    if (true === true) {
        var ue_t0 = (+ new Date()),
            ue_csm = window,
            ue = { t0: ue_t0, d: function() { return (+new Date() - ue_t0); } },
            ue_furl = "fls-cn.amazon.cn",
            ue_mid = "AAHKV2X7AFYLW",
     


这里，跟京东页面的爬取不太一样，因为我们要做一个头部处理，这对网站保护性比较好的网站也可以张确的获取到信息。

### 实例3：百度360搜索关键词提交

百度和360搜索我们都很清楚，但是有没有这样一种可能，用程序向这两个搜索引擎提交两个关键词，并且我们能搜索到它的结果呢？

* 百度的关键字接口 https://www.baidu.com/s?wd=keyword 
* 360的关键字接口 https://www.so.com/s?q=keyword

我们只需要在这两个url将我们想要的关键词替换掉keyword，就可以向搜索引擎提交关键词了。

还记得吗？我们之前提到有一个params字段，它可以向url字段中增加相关内容。

1.首先我们来构造一个键值对，假设我们要搜索的关键词是python，且假设我们用360的搜索来试一下


```python
import requests
kv = {'q':'python'}
r = requests.get("http://www.so.com/s",params = kv)
r.status_code
```




    200




```python
r.request.url
```




    'https://www.so.com/s?q=python'



接下来，使用response对象中包含的request的对象的信息，这个url表示我们向搜索引擎提交用来搜索关键词python的url



我们还可以来查看一下返回的内容的信息，先不要打印，我们来看一下长度：


```python
len(r.text)
```




    465563



这里表示我们用这条url提交的搜索请求返回的结果包含365k左右的信息。

如果你是想用百度的搜索引擎来尝试一下，只需要将代码中的q替换成wd，结果应该是这样的：


```python
'http://www.baidu.com/s?wd=Python'
```




    'http://www.baidu.com/s?wd=Python'




```python
# 百度全代码
import requests
keyword = "python"
try:
    kv={'wd':keyword}
    r = requests.get("https://www.baidu.com/s",params=kv)
    print(r.request.url)
    r.raise_for_status()
    print(len(r.text))
except:
    print("爬取失败")
```

    https://wappass.baidu.com/static/captcha/tuxing.html?&ak=c27bbc89afca0463650ac9bde68ebe06&backurl=https%3A%2F%2Fwww.baidu.com%2Fs%3Fwd%3Dpython&logid=7967836354924555337&signature=fbc3e398f110ff21de4ecb88c7e90dc8&timestamp=1585025066
    1519


###  实例4：网络图片的爬取和存储

* 网络图片的爬取

网络图片链接格式：http://www.example.com/picture.jpg

比如国家地理：http://www.nationalgeographic.com.cn/

我们随机选择一个图片，看下面这张图片的链接，这样的一个.jpg格式就是表示这是一个图片链接且是文件类型的，我们要做的就是利用python将它爬取出来，并保存到本机


```python
# 图片爬取全代码
import requests
import os
url = "http://pic37.nipic.com/20140113/8800276_184927469000_2.png"
root = "./"
path = root + url.split('/')[-1]
try:
    if not os.path.exists(root):
        os.mkdir(root)
    if not os.path.exists(path):
        r = requests.get(url)
        with open(path,'wb') as f:
            f.write(r.content)
            f.close()
            print("文件保存成功")
    else:
        print("文件已存在")
except:
    print("爬取失败")
```

    文件保存成功


 

事实上，想要实现这样一个功能的代码很简单，但是如果不规范代码的要求那就不能作为产品给用户使用，这也是做工程时的一个比较高的要求，所以一定要重视每个实例后标准的代码框架，培养代码规范的好习惯。

### 实例5：IP地址归属地的自动查询

既然要查询IP地址我们需要有一定的库来支持，但遗憾的是并没有这样一个库来支持IP地址查询。

但是我们发现一个网站，可以提供类似的功能：http://m.ip138.com/，它的主页是这样的


```python
# IP地址查询全代码
import requests
url = "http://m.138.com/ip.asp?ip="
try:
    r = requests.get(url + '202.204.80.112')
    r.raise_for_status()
    r.encoding = r.apparent_encoding
    print(r.text[-500:])
except:
    print("获取失败")
```

    获取失败

