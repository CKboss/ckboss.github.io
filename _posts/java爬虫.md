title: Java爬虫简单介绍

date: 2016-01-12 00:18:46

tags:
- java

categories:
- 杂

---

###### 2016年第一更...

上个月写了不少java的爬虫,总结一下常用的java爬虫框架.

试用了两个比较常见的java爬虫相关框架,WebCollector和WebMagic.

## Part1 正常向爬虫框架

### WebCollector

---

###### 1. 项目主页:[WebCollector](https://github.com/CrawlScript/WebCollector)

###### 2. 简单的使用方法:

扩展一个Crawler类,根据需要重载两个方法即可...

```java
 @Override
    public void visit(Page page, CrawlDatums next) {
        System.out.println("visiting:"+page.getUrl()+"\tdepth="+page.getMetaData("depth"));
    }
```

visit 方法用于访问到指定页面时在该页面进行的操作,在这里可以写爬虫的具体的抓取操作.

```java
@Override
    public void afterVisit(Page page, CrawlDatums next) {
        super.afterVisit(page, next); 
		}
```

默认的 afterVisit 函数处理在找到的页面中将那些URL加入到后续的搜索队列中去,如果有需要可以重写这个函数.


WebCollector 的2.0版本中有一个非常好用的特性,对于每一个datum可以附加一个叫做MetaData的东西,可以用来记录跟随Datum的一些信息比如页面的深度,在一些情况下非常方便.

一个调用示例:

```java
 public static void main(String[] args) throws Exception {
        DemoDepthCrawler crawler=new DemoDepthCrawler("depth_crawler", true);
        for(int i=1;i<=5;i++){
            crawler.addSeed(new CrawlDatum("http://news.hfut.edu.cn/list-1-"+i+".html")
                    .putMetaData("depth", "1"));
        }
        /*正则规则用于控制爬虫自动解析出的链接，用户手动添加的链接，例如添加的种子、或
          在visit方法中添加到next中的链接并不会参与正则过滤*/
        /*自动爬取类似"http://news.hfut.edu.cn/show-xxxxxxhtml"的链接*/
        crawler.addRegex("http://news.hfut.edu.cn/show-.*html");
        /*不要爬取jpg|png|gif*/
        crawler.addRegex("-.*\\.(jpg|png|gif).*");
        /*不要爬取包含"#"的链接*/
        crawler.addRegex("-.*#.*");
        crawler.start(2);
    }
```

更多的内容可以取看代码中的example


### WebMagic

![WebMagic](https://camo.githubusercontent.com/77fe3da40f9b2c5839df0267890a2457a64003e0/68747470733a2f2f7261772e6769746875622e636f6d2f636f64653463726166742f7765626d616769632f6d61737465722f6173736574732f6c6f676f2e6a7067)

---

WebMagic是设计的非常好的一个爬虫,如果要使用java编写爬虫那么非常推荐使用.

可惜因为考虑到项目的兼容问题,暂时没有深入的研究这个爬虫.

项目地址: [WebMagic](https://github.com/code4craft/webmagic)

非常齐全的手册: [docs](http://webmagic.io/docs/)

## Part2 非主流爬虫

---

大部分的爬虫框架都是模拟浏览器的请求,但是很多网站是防爬虫的,或者有很多的不明js等动态网页信息...对于这样的网站可以想办法操作浏览器进行爬取.

在编写爬虫的时候,只要模拟用户的操作就可以了,比如要加载一个js不需要进行任何的分析,模拟操作后wait一下就加载好了.

这种方法的优点是可以有效的反反爬虫,方便的爬去动态内容,缺点也很明显,因为往往在后台需要调用浏览器,所以资源消耗很大,处理速度也很慢,在调用这类浏览器的时候可以考虑关闭图片的加载,css渲染等...

一些可供调用的浏览器...

#### [httpunit](http://httpunit.sourceforge.net/)
![httpunit](http://httpunit.sourceforge.net/doc/images/HttpUnit.jpg)

httpunit是一个java写的无界面浏览器,通过调用httpunit的api可以模拟浏览器访问网站.

**httpunit有个致命的缺点**,它对js支持非常的差,大部分的js都没法正常的解析,暂时不适合用来做解析动态网页的爬虫!!!

#### phantomjs

![](http://phantomjs.org/img/phantomjs-logo.png)

[项目主页](http://phantomjs.org/)

也是个浏览器,没有提供linux下的安装包,暂时没有试用这个...


#### selenium

![](http://www.seleniumhq.org/images/selenium-logo.png)

[项目主页](http://www.seleniumhq.org/)

selenium是一个网站测试工具,可以用来模拟用户的操作,可以支持java,python,c#等对多种语言对浏览器调用.

selenium 在 火狐 上有个叫做 selenium IDE的插件,安装这插件后可以记录用户的操作,并且可以自动生成代码,非常方便.

selenium 支持很多的浏览器包括httpuity,建议调用火狐/谷歌等保证对网页的兼容性.


