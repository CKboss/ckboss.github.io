title: MailGun API发送电子邮件

date: 2015-11-20 22:51:17

tags:
- Python3小玩意

categories:
- Python3小玩意

---
![MailGun](https://e45a551211fc56cb40b0-7d31421b6f9e7cf93475b75027a9bfbb.ssl.cf2.rackcdn.com/e6c50080e48828348b948be33d5efdb232a25789/images/mailgun.svg)

##### Python是一门简单又功能强大的脚本语言....

##### 配合MailGun提供的API,则可以轻松组装成一把威力强大的*MailGun*.

##### [MailGun官网](https://www.mailgun.com/)
MailGun提供电子邮件发送的API,免费账号可以每月发送10000封邮件,不验证域名的话每天限量300封

注册账号后就会提供两个可以调用的API,调用方式非常简单如下:

```python
import smtplib
import requests
from email.mime.text import MIMEText

def send_simple_message():
    return requests.post(
        "https://api.mailgun.net/v3/sandboxxxxxxxxxxxx.mailgun.org/messages",
        auth=("api", "key-ccxxxxxxxxxxxxxxxxxx"),
        data={"from": "TestSender <justaTest@tt.com>",
              "to": "X <xxxxx@sina.com>",
              "subject": "Just a Test",
              "text": "rt Just a Test"})

def send_smtp_message():
    msg = MIMEText('Testing some Mailgun awesomness')
    msg['Subject'] = "Hello"
    msg['From']    = "foo@tt.cc"
    msg['To']      = "xxxxx@sina.com"

    s = smtplib.SMTP('smtp.mailgun.org', XXX)

    s.login('postmaster@sandboxbxxxxxxxxxxxxxx.mailgun.org', 'xxxxxxxxxxxxxxxxx') 
	#填自己的账号和密码
    s.sendmail(msg['From'], msg['To'], msg.as_string())
    s.quit()


if __name__=='__main__':
    send_simple_message()
    send_smtp_message()

```


以上就大功告成了,可以发送邮件测试一下.

![](http://ww2.sinaimg.cn/mw1024/50a04a61gw1ey7uhjz32tj20xh0dbq6a.jpg)

~~电子邮件的寄信人是可以随意伪造的,真的可以做一把SpamGun哦~~