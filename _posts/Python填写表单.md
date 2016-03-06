title: Python3 处理Post操作
date: 2015-11-20 23:28:13
tags:
- Python3小玩意

categories:
- Python3小玩意

---

Python写爬虫的时候经常要填写一些表单,总结了一下POST操作的写法.

首先需要用浏览器找到POST连接,获得POST的数据的格式.

然后编写返回特定格式字典的函数.

```python
import random
import math,string

'''getPOST_1.py'''

def getData():
    D = {
    'selectItem':'on',
    'lessonid':'438',
    'StartDate':'2015-8-5 0:00:00',
    'CityId':'2',
    'Days':'2',
    }
    return D

class RandomChar():
  """用于随机生成汉字"""
  @staticmethod
  def Unicode():
    val = random.randint(0x4E00, 0x9FBF)
    return chr(val)

  @staticmethod
  def GB2312():
    head = random.randint(0xB0, 0xCF)
    body = random.randint(0xA, 0xF)
    tail = random.randint(0, 0xF)
    val = ( head << 8 ) | (body << 4) | tail
    str = "%x" % val
    return str.decode('hex').decode('gb2312')


def getPD():
    D = getData()
    return D

if __name__=='__main__':

    for i in range(1,10):
        print(getPD())
```

最后模拟浏览器发出请求,就可以了
注意:跑多线程的话很容易把小网站跑挂....

```python
import urllib.parse,urllib.request,http.cookiejar
import random
import threading
import time
import getPOST_1

cnt=0

def GetUrlRequest(Url,StrPostData):
    try:
        postdata=urllib.parse.urlencode(StrPostData)
        postdata=postdata.encode(encoding='UTF8')
        header={'User-Agent':'Mozilla/5.0 (Linux; Android 4.3; zh-cn; SAMSUNG-SM-G3518_TD Android/4.3 Release/11.15.2013 Browser/AppleWebKit537.36 Build/JSS15J) AppleWebKit/537.36 (KHTML, like Gecko) Version/1.5 Mobile Safari/537.36'}
        req= urllib.request.Request(
            url = Url,
            data = postdata,
            headers = header)
        ret = urllib.request.urlopen(req)
        print(ret.read().decode("UTF8"))
        print('---> '+str(cnt))
    except:
        print('can\'t connect')


Url = 'http://www.xxxxx.com/'


def gao():
    GetUrlRequest(Url,getPOST_1.getPD())


if __name__=='__main__':

    threads = []
    for i in range(0,20):
        cnt+=1
        threads.append(threading.Thread(target=gao))
    for t in threads:
        t.start()

```