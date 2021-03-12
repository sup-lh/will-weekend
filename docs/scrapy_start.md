<style> g{ font-size:26px; color: #27408B; background: #87CEFF; border-radius: 5px; opacity: 85%; } </style>

#
# **<g>ready？</g>**


?> 阿巴阿巴阿巴阿巴阿巴 💤💤

<br>

?> 阿巴阿巴阿巴阿巴阿巴 🔇🔇

<br>

# **go!** 
<br>

?> 新建一个文件夹，vs<br>
打开集成终端，输入<br>
scrapy -h<br>
看到后面startproject复制粘贴输入：<br>
scrapy startproject 文件名<br>

**创建普通scrapy架构**<br>

?> cd 进spider文件中<br>
scrapy genspider example example.com<br>

**创建爬虫scrapy架构**<br>

?> cd 进spider文件中<br>
scrapy genspider -t crawl example example.com<br>

***启动爬虫文件***<br>

?> cd 进spider文件<br>
scrapy crawl quotes



<br><br><br><br>

# 爬虫文件 🎊🎊
- **爬虫文件里面的start_requests函数会第一次执行，所以就算函数外面的start_urls变量里面有多个url，在有start_requests的情况下也会失效**
```python
    def start_requests(self):
        urls = [
            'http://www.baidu.com',
            'http://www.baidu.com',
        ]
        for url in urls:
            yield scrapy.Request(url=url, callback=self.parse)
```
**yield的作用：**<br>

?> 类似于return，但是return会直接跳出，当再次执行带有return的函数时会从头运行，而yield则有挂起的作用，当再次执行时会从yield中继续执行<br>后面的parse不用加括号
```python
urls = [
        'http://www.baidu.com',
        'http://www.baidu.com',
]
for url in urls:
        yield scrapy.Request(url=url, callback=self.parse)
```


**在parse函数里面打爬虫代码**<br>

?> 第一次请求已经在start_requests函数里面已经得到，parse函数所带的response就是html<br>
```python
 def parse(self, response):
```

**要<br>from 文件名.items import 文件名Item<br>**
```python
class QuotesSpider(scrapy.Spider):
```

**因为<br>在parse函数开始处 item = NewsItem() 格式化类 最后return item 提交item**
```python
item = TutorialItem()
巴拉巴拉巴拉
return item
```
**方法使用：**<br>

?> 之前用的<br>BeautifulSoup<br>
相当于现在的<br>title=response.css('title::text').get()<br>
这里的 ::text 则是定位到的标签中的text属性<br>

**注意：**<br>

?> scrapy旧版写法：<br>
.extract_first()   字符串<br>
.extract()   列表<br><br>
scrapy新版写法：<br>
.get()  字符串<br>
.get_all()  列表<br>
```python
#之前的：
from bs4 import BeautifulSoup
soup = BeautifulSoup(response.text,'lxml')
list_data=soup.select('#u1 a')
#现在的：
list_data=response.css("#u1 a::attr(href)").get()
```
**注意：**<br>

?>在爬虫文件中所有要用到response的都要在函数后面括号里面加response，如（这里第一个函数是发起请求的，不用加response，后面parse直接用到了第一个网页，就要加response）：<br>
```python
    def start_requests(self):
        urls = [
            'http://www.baidu.com',
            'http://www.baidu.com',
        ]
        for url in urls:
            yield scrapy.Request(url=url, callback=self.parse)

    def parse(self, response):

        print(response.css('title::text').get())
        print(response.css("#u1 a::attr(href)").get())

        soup = BeautifulSoup(response.text,'lxml')
        print(soup.title)
```




<br><br><br><br>

# pipeline文件 ☢️

## **写入csv文件** ☣️
先导包：<br>
```python
from scrapy.exporters import CsvItemExporter
```
这个类里面有三个函数，分别是创建，写入，关闭，如下：
```python
class NewsPipelineCsv(object):

    def __init__(self):
        self.file = open('news_data.csv','wb')#创建文件
        self.exporter = CsvItemExporter(self.file,encoding = 'utf-8')
        self.exporter.start_exporting()#启动写入
    
    def process_item(self, item, spider):
        self.exporter.export_item(item)
        return item

    def close_spider(self,spider):
        self.exporter.finish_exporting()#关闭写入
        self.file.close()#关闭文件
```

?>第一个函数<br>里面__init__()<br>
self.exporter = JsonItemExporter(self.file,encoding = 'utf-8')<br>
JsonItemExporter就是导的包<br>

?>基本思路：创建文件-->修改编码-->启动写入-->写入item，然后提交item-->关闭写入-->关闭文件<br><br>


## **写入json文件** ➰
先导包：<br>
```python
from scrapy.exporters import JsonItemExporter
```

?>除去类的名字和导的包不一样，其他和csv一样<br>

**因为换行的______**<br>
?>在写入的函数里面要添加一行<br>
self.exporter.indent = 1<br>

**注意：**<br>
?>1：类的括号里面要加入object（也可不加）<br>
2:创建文件的时候是'wb'写入<br>

**css找元素的使用：**<br>
```python
title = response.css('title::text').extract_first()
就是soup中这样的使用：
title=soup.select_one('title').text

当要选中元素的时候就是这样的使用(复习bs4,Xpath,css 在选择上的区别)：
title = response.css('a.u-icn.u-icn-81.icn-add::attr(href)').extract_first()
相当于bs4：
title=soup.select_one('a.u-icn.u-icn-81.icn-add')['href']
相当于xPath：
title=response.xpath("//div[@class='opt hshow']/a[1]/@href")

```


## **写入数据库** 干巴爹！！！🌱

- 导包：
```python
import pymysql
```

?>同样也是三个函数，但是与写入csv和json不同的是：<br>
数据库第一个是创建游标、创建连接，而不是创建文件；<br>
第二个文件是写入数据库，要try里面return item；<br>
第三个文件是关闭连接和关闭游标。




<br><br><br><br>

# setting文件 💠


?>ROBOTSTXT_OBEY<br>
改成False，为不遵循这个协议<br><br><br>
找到ITEM_PIPELINES配置，<br>
把pipeline文件里面的定义的类的名字加入，调节优先级（优先级1为最大优先级）<br>
如图所示设置为300，301，302（字典形式）
```python
ITEM_PIPELINES = {
   'news.pipelines.NewsPipelineCsv': 300,
   'news.pipelines.NewsPipelineJson': 301,
   'news.pipelines.NewsPipelineMysql': 302,
}
```

**注意：**<br>
?>pipeline文件里面没有的类<br>
setting文件里面的优先级设置里面也不能有这个类的名字


<br><br><br><br>

?>如果不跳进链接爬取，就在第一个链接爬取，则：


```python
方法一：
直接在parse里面写，response则是该网页的请求
直接提交一个列表，dict后面的则是item文件里面的变量


    def parse(self, response):
        title = response.css('div.ns_area.list ul li a::text').extract()
        list_data=[]
        for i in title:
            list_data.append(dict(title_data=i))
        return list_data
            
        urls = response.css('div.ns_area.list ul li a::attr(href)').extract()
        data_list=[]
        for url in urls:
            data_list.append(dict(url_data=url))
        return data_list


方法二：
不用提交item
直接返回一个字典列表

因为只能返回一个值，所以只能二选一

    def parse(self, response):
        # title = response.css('div.ns_area.list ul li a::text').extract()
        # list_data=[]
        # for i in title:
        #     list_data.append(dict(title_data=i))
        # return list_data
            
        urls = response.css('div.ns_area.list ul li a::attr(href)').extract()
        data_list=[]
        for url in urls:
            data_list.append(dict(url_data=url))
        return data_list
```


💪💪💪💪💪💪💪💪💪💪💪💪💪💪💪


