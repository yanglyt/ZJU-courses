![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-03-28 200238.png)

| 姓  名： | 杨吉祥               |
| -------- | -------------------- |
| 学  院： | 计算机科学与技术学院 |
| 系：     | 竺可桢学院图灵班     |
| 专  业： | 计算机科学与技术     |
| 学  号： | 3230106222           |

<div style="page-break-after:always;"></div>

# 1.DNS

1. 完整命令为`nslookup -type=NS cubicy.icu`，`cubicy.icu`的权威域名服务器是`itzel.ns.cloudflare.com`和

`sam.ns.cloudflare.com`

<img src="C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-07-04 191132.png" style="zoom: 80%;" />

2. 完整命令为`nslookup -type=A cubicy.icu`,得到ip地址为`104.21.31.206`，`172.67179.244`

<img src="C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-07-04 191642.png" style="zoom: 80%;" />

3. 多次查询 DNS A 记录，每次的响应都一样

   <img src="C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-07-04 203006.png" style="zoom:80%;" />

4. 借助 DNS 服务器，我们可以对同一个域名返回不同的 IP 地址，将不同的ip地址返回给用户可以减少单个服务器的负载，提高网站性能；当一个服务器出现故障时，可以将其他可用的ip地址返回给用户，提高了网站的稳定性；当服务器需要升级，维护时，可以根据需要调整服务器配置，而不会影响用户的访问

5. 完整命令为`nslookup -type=TXT cubicy.icu`,得到文本为`5aSx5oGL44K944Oz44Kw5rKi5bGx6IG044GE44GmCuazo+OBhOOBpuOBsOOBi+OCiuOBruengeOBr+OCguOBhgrmjajjgabjgZ/jgYTjgYvjgokK5b+Y44KM44Gf44GE44GL44KJCuOCguOBhiDlkJvjga7jgZPjgajjgarjgpPjgaYK5b+Y44KM44Gh44KD44GG44GL44KJ44GtICAKU1VLSVNVS0lTVUtJU1VLSVNVS0k=`,利用cyberchef得到这是一段日语，大致就是

   失恋ソング沢山聴いて

   泣いてばかりの私はもう

   捨てていたら

   忘れたいんだったら

   もう 君のことなんて

   忘れちゃうかなぁ

   SUKISUKISUKISUKISUKI

<img src="C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-07-04 194506.png" style="zoom:80%;" />

6. 这三个域名的ip(ipv4)地址相同

<img src="C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-07-04 194910.png" style="zoom:80%;" />

7. 直接通过ip地址访问无法成功，虽然访问失败原因很多，但根据下一个问题的提示推测地址不是服务器的真实地址，而可能是中转结点的地址

<img src="C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-07-04 201334.png" style="zoom:80%;" />

8. 可能是借助了CDN服务，CDN会根据用户的位置选择最近的服务器来提供内容，从而减少延迟提高加载速度，所以即使源服务器的地址发生改变，但离用户最近的服务器不变，那么访问者眼中的ip地址就无需发生改变

<div style="page-break-after:always;"></div>

# 2.HTTP

1. - 报文内容如下

   <img src="C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-07-04 223700.png" style="zoom:80%;" />

   该报文由请求行，请求头部，空行和请求数据四部分构成。请求行包括请求方法，请求URL，HTTP协议版本，该报文请求方法为POST，HTTP协议版本为1.1;请求头部由关键字和值组成，每行一对；请求头部下方要有一空行；因为请求方法为POST，所以有请求数据，位于空行下方

   - 网站的登陆状态是通过cookie来保持的，当用户成功登录后，服务器会在HTTP响应的头部中设置一个名为“Set-Cookie”的字段，其中包含了一个唯一的标识符，即Cookie。浏览器会在本地存储这个Cookie，并在每次向服务器发送请求时，将这个Cookie包含在HTTP请求的头部中，以便服务器识别用户。另一种常见的机制是使用Session。在用户登录成功后，服务器会创建一个会话（Session），并将会话ID发送给客户端（通常存在于Cookie中）。服务器会在后续的请求中检查这个会话ID，以确定用户的身份和状态。
     2. 虽然TCP是无边界的字节流服务，但HTTP协议通过自身的报文格式来区分不同的包。HTTP报文头部包含了关于报文的元数据信息，如请求方法、状态码、内容长度等。这些头部信息提供了关于消息内容的重要描述，帮助接收端正确处理每个HTTP消息。在HTTP报文中，头部信息和消息主体之间有一个空行来分隔二者。这个空行充当了头部信息和消息主体的分界线，帮助接收端准确地识别出每个HTTP消息的头部和主体部分。HTTP协议定义了请求和响应的报文格式，每个报文包含起始行、头部信息和可选的消息主体。这种结构化的报文格式使得接收端能够准确地解析出每个HTTP消息。

3. - 直接访问源服务器地址是失败的

     ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-07-04 231340.png)

   -  **直接IP访问**：当用户直接通过IP地址访问一个主机时，请求中的目标地址字段会包含目标主机的IP地址，而不包含域名。主机接收到这样的请求后，会直接使用请求中的IP地址来确定目标主机。

     **通过域名访问**：当用户通过域名访问一个主机时，请求中的目标地址字段会包含域名，而不是IP地址。主机接收到这样的请求后，首先需要将域名解析为对应的IP地址。主机会查询DNS服务器，获取域名对应的IP地址，然后再使用该IP地址来确定目标主机。

   简言之，主机通过检查请求中的目标地址信息来区分直接IP访问和通过域名访问。如果请求中包含IP地址，则主机直接使用该IP地址；如果请求中包含域名，则主机需要先将域名解析为对应的IP地址，然后再确定目标主机。

   - 我对比了一下拦截域名访问和拦截ip访问的报文，发现二者Host不同，所以我将Host改为域名就可以了

<img src="C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-07-04 234233.png" style="zoom:80%;" />

<div style="page-break-after:always;"></div>

# 3. 预习

用burp suite抓包后查看请求，按照post请求格式将url，header以及cookie照搬，因为成绩页面有两页，所以发送了两次请求，查看第一页数据时，发现不用发送data也能查看，所以就省去了data，查看第二页数据发现要用到data，就把data也照搬，然后再用find函数将成绩，绩点，课程名称找到，从而查看成绩

<img src="C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-07-17 130317.png" style="zoom:80%;" />

<img src="C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-07-17 130309.png" style="zoom:80%;" />

<img src="C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-07-17 125101.png" style="zoom:80%;" />

代码如下

```python
import requests
import re
session=requests.session()
headers1={
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.6478.127 Safari/537.36',
    'Cookie':'JSESSIONID=C916B922D12BEBE738F84621C9679AEC;lang=zh-CN; JSESSIONID=D1478EDC510642414D93C8267687D60F; route=1a53339a9972202950f42a60e12340ac; _csrf=S8mwplVi9KWoF2WQ0TlCeN6uyrY%2FJVwvB6rt%2BNvmiEc%3D; _pv0=yHnyuogjl5YToLjfx9THdMlGvsyVsFMM%2Ft4iN%2BovkTon1wNrlfesNKo%2FlIO1jwGMWjfovylx3ml6F0ybAxF95Frailpe6J%2BBv%2BZQgZuThDNKOKboUGRc1Lh2V%2BH6SyHqLlGAZdRKdRvHFSRVeZkNJDw%2FwVWb8z4qi%2BRtYGbVaEn8wc20jbeSNBaK4BJ%2FKMtCY%2FuDuBS%2BajbeQyxXJlrMdkwrbgcqQlH823OQLioodYx3PTXimyjlXR44XXbilJ00MWyBYVuCBBO%2BG4bbbA%2BDi7hsoX4qc8Ia2mukLjdZ63HXxr3WeITvDXNymcLTRcYRaoFjLfh5yk6xaiZEFw3c0Xovc6rYD9wLjgr6DTK2YVbAHeCa%2B7S4gjLdY566%2BbnzNkahAtj1UZ29wLbwQgvG%2FZ8BmlobbTzuaXuNsieqXgo%3D; _pf0=nGK1uBR9hev9ZIftrowxVB98ac7idiPOmDqOWl7qsTU%3D; _pc0=pGoFzTKOp8709GLavob%2FFdQB2rF3Nb195ZZKEiRcdmoB3kDDfVOCduWykrmghfPb;iPlanetDirectoryPro=O4rhEuyZQVMuoLdOZwaeVJ%2B49o5Sze%2B3g3uk2GRIpbaqFEWuX5VqDUhPbiVrYD7C0w3BU6i%2F95AZiT7PITFLnn9dVyIvgjKNNGrtbqeuzFefVJk8ZwJ1HQp4N8Y8My1tHJGYjDRbtU%2BwFHnwUK17j4fZY8oNQBN5HM7cGHJjI9mUK8%2BzkwJXYtpbFAC6pmD3duwQaGqY27H0ygIcUJxFLTBwIcvQ8KwVqsu39MO%2Fi1MCY837fwjeRMnbm68oXWyexdesI1xGwsEKBHBI9Ehqbh00p%2FjT3B6HHZCMwJlPNJurPeCjXEP9z%2FwPGLVZ4y5qE2i%2FwhF8Me7aUjfCSSzmstLffUU5dFyIll9MPQzsP60%3D'
}
url1='http://zdbk.zju.edu.cn/jwglxt/cxdy/xscjcx_cxXscjIndex.html?doType=query&gnmkdm=N5083&su=3230106222'

response=session.post(url1,headers=headers1)
print(response.status_code)
content=response.text
keys=["cj","jd","kcmc"]
positions1={}
for key in keys:
    positions1[key]=[m.start() for m in re.finditer(key, content)]

for i in range(len(positions1["cj"])):
    cj=content[positions1["cj"][i]+5:positions1["cj"][i]+7]
    jd=content[positions1["jd"][i]+5:positions1["jd"][i]+8]
    for j in range(100):
        if content[positions1["kcmc"][i]+7+j]=="\"":
            break
    kcmc=content[positions1["kcmc"][i]+7:positions1["kcmc"][i]+7+j]
    print("课程名称:",kcmc)
    print("成绩:",cj)
    print("绩点:",jd)
    print("\n")

headers2={
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.6478.127 Safari/537.36',
    'Cookie':'JSESSIONID=C916B922D12BEBE738F84621C9679AEC; lang=zh-CN; JSESSIONID=D1478EDC510642414D93C8267687D60F; route=1a53339a9972202950f42a60e12340ac; _csrf=S8mwplVi9KWoF2WQ0TlCeN6uyrY%2FJVwvB6rt%2BNvmiEc%3D; _pv0=yHnyuogjl5YToLjfx9THdMlGvsyVsFMM%2Ft4iN%2BovkTon1wNrlfesNKo%2FlIO1jwGMWjfovylx3ml6F0ybAxF95Frailpe6J%2BBv%2BZQgZuThDNKOKboUGRc1Lh2V%2BH6SyHqLlGAZdRKdRvHFSRVeZkNJDw%2FwVWb8z4qi%2BRtYGbVaEn8wc20jbeSNBaK4BJ%2FKMtCY%2FuDuBS%2BajbeQyxXJlrMdkwrbgcqQlH823OQLioodYx3PTXimyjlXR44XXbilJ00MWyBYVuCBBO%2BG4bbbA%2BDi7hsoX4qc8Ia2mukLjdZ63HXxr3WeITvDXNymcLTRcYRaoFjLfh5yk6xaiZEFw3c0Xovc6rYD9wLjgr6DTK2YVbAHeCa%2B7S4gjLdY566%2BbnzNkahAtj1UZ29wLbwQgvG%2FZ8BmlobbTzuaXuNsieqXgo%3D; _pf0=nGK1uBR9hev9ZIftrowxVB98ac7idiPOmDqOWl7qsTU%3D; _pc0=pGoFzTKOp8709GLavob%2FFdQB2rF3Nb195ZZKEiRcdmoB3kDDfVOCduWykrmghfPb; iPlanetDirectoryPro=O4rhEuyZQVMuoLdOZwaeVJ%2B49o5Sze%2B3g3uk2GRIpbaqFEWuX5VqDUhPbiVrYD7C0w3BU6i%2F95AZiT7PITFLnn9dVyIvgjKNNGrtbqeuzFefVJk8ZwJ1HQp4N8Y8My1tHJGYjDRbtU%2BwFHnwUK17j4fZY8oNQBN5HM7cGHJjI9mUK8%2BzkwJXYtpbFAC6pmD3duwQaGqY27H0ygIcUJxFLTBwIcvQ8KwVqsu39MO%2Fi1MCY837fwjeRMnbm68oXWyexdesI1xGwsEKBHBI9Ehqbh00p%2FjT3B6HHZCMwJlPNJurPeCjXEP9z%2FwPGLVZ4y5qE2i%2FwhF8Me7aUjfCSSzmstLffUU5dFyIll9MPQzsP60%3D'
}
url2='http://zdbk.zju.edu.cn/jwglxt/cxdy/xscjcx_cxXscjIndex.html?doType=query&gnmkdm=N508301&su=3230106222'
data={
    'xn':'',
    'xq':'',
    'zscjl':'',
    'zscjr':'',
    '_search':'false',
    'nd':'1721182481791',
    'queryModel.showCount':'15',
    'queryModel.currentPage':'2',
    'queryModel.sortName':'xkkh',
    'queryModel.sortOrder':'asc',
    'time':'0'
}
response=session.post(url2,headers=headers2,data=data)
print(response.status_code)
content=response.text
positions2={}
for key in keys:
    positions2[key]=[m.start() for m in re.finditer(key, content)]

for i in range(len(positions2["cj"])):
    cj=content[positions2["cj"][i]+5:positions2["cj"][i]+7]
    jd=content[positions2["jd"][i]+5:positions2["jd"][i]+8]
    for j in range(100):
        if content[positions2["kcmc"][i]+7+j]=="\"":
            break
    kcmc=content[positions2["kcmc"][i]+7:positions2["kcmc"][i]+7+j]
    print("课程名称:",kcmc)
    print("成绩:",cj)
    print("绩点:",jd)
    print("\n")
```









