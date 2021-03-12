<style> g{ font-size:26px; color: #2F4F4F; background:#778899; border-radius: 5px; opacity: 85%; } </style>

#
# <g>项目路径：</g>⛷️🤺

```python
##############################################################################
#
#      \app
#      │  
#      │  
#      │
#      ├─   /models
#      │        __init__
#      │        data.py  
#      │
#      ├─   /static
#      │      └ /js
#      │         └  echarts.min.js
#      │     
#      │
#      ├─   /templates
#      │      └ index.html  
#      │        
#      │
#      │─   __init__.py
#      │        
#      │
#      └──   routes.py
#      
#       start.py       
#
##############################################################################
```

# 🕴️echarts文件的下载
> [!WARNING|label:]**echarts.min.js 文件的下载：**<br>
- 进入[echarts官网](https://echarts.apache.org/zh/index.html)--》下载--》Dist--》Tags选择版本--》点击echarts.js或者echarts.min.js--》点击Raw进入网站--》复制网站链接--》打开fdm下载即可
- 上步骤截图：<br>

    - ![](./2.jpg)
    - ![](./3.jpg)
    - ![](./4.jpg)
    - ![](./5.jpg)

<br><br><br>

# 各版本区别🛶
> [!Danger|label:]**最简版：**<br>
- 文件夹及文件分布：
   - app文件夹和start.py在同一级
   - app文件夹里面包括static文件夹和templates文件夹以及__init__.py和routes.py两个py文件
       - static文件夹里只有js文件夹，js文件夹里放有echarts.min.js文件
       - templates文件夹里存有网站文件
   - ![](./1.jpg)
- 最简版echarts是在routes文件中的return上面直接写 ***列表*** 数据，然后直接返回到html文件中利用
- 在html文件中定义函数的时候怎样的写法：
    - ```python
        #在routes中的样子
        a=['詹姆斯','库里','哈登']
        b=[2,4,8]
        #在html中的样子
        var name_list = [{% for i in a %} '{{i}}', {% endfor %}];
        var data = [{% for i in b %} {{i}}, {% endfor %}];
      ```
<br>

> [!Tip|label:]**稍微复杂版：**<br>
- 文件夹布置一样
- 稍复杂版只是把routes里面需要传输的值写成字典，然后在html文件中读取键值对，然后呈现成pie图像。
- 稍微复杂版echarts是在routes文件中的return上面直接写 ***字典*** 数据，然后直接返回到html文件中利用
- 在html文件中定义函数的时候怎样的写法：
    - ```python
        #在routes中的样子
        name=['詹姆斯','库里','哈登']
        num=[2,4,8]
        list_data=[]
        for i in range(len(name)):
            list_data.append(dict(name=name[i],value=num[i]))
        return render_template('index.html', data_list=list_data)
        #在html中的样子
        var list_data = [{% for i in data_list %}  {value:{{i['value']}},name:'{{i['name']}}' },  {% endfor %}];
      ```
    - *注意*： 因为迭代上面name，会得到没有引号的字符串，所以name这里要加引号

<br>

> [!Note|label:]***标准版：***
- 文件夹及文件分布：
    - 与上面的不同的是，多了一个models文件夹，里面包含一个__init__.py文件和一个data.py文件，后面的文件名随便取，上图：
    - ![](./6.jpg)
- data 文件里则是在数据库的各个表里面创建字段来储存数据，如图：
    - ![](./7.jpg)
    - 里面的__tablename__是库的表名，要看这些表是来自哪个库的，前往app文件夹里面__init__ app.config第一个配置文件后面写的是哪个库。
    - ![](./9.jpg)
    - 第一个fld_index是序号，primary_key则是自动排序，该字段在html文件中不需要去引用了
    - ![](./8.jpg)

<br><br><br>
    
# 各个文件之间的内在联系🥏

- 在app文件夹里面的__init__导入SQLAlchemy，连接数据库里面的库并给到db变量
- 在models文件夹里面的data文件从app文件夹导入db，即库
- 在data文件里创建不同的表来存储不同的数据，不同表名上面的类名可以随便取，但是表名要跟数据库里面的表名一致
- 在models文件夹里面的__init__导入data文件里面的类名
- 这时就可以在routes文件里面从models文件夹里面直接导类，即导了这些类里面的表
- 来到routes里面，定义不同的函数，这些函数的作用是用query功能查询表里面的字段，并返回出查询的结果
- 往下看到routes文件的路由功能，用变量接住上面函数返回的值，并传递给html文件
- 来到html文件，因为传递到html文件的是query.all()，是一个列表，所以要迭代，迭代对象是表的全部字段
- 选i.fld_sale_count等的字段，为什么用点.呢，因为i是类下面的变量
- 最后就呈现到了网页中了


**上图！！！！：**
- ![](./10.jpg)

<br><br><br>

# 各文件里面的代码详解(按联系顺序)🎳
<br><br>

> [!WARNING|label:]## app项目里面的__init__.py文件：
从flask导Flask包
```python
from flask import Flask
```
第一步，创建程序实例,仅有的一个参数则是包的名字：
```python
app = Flask(__name__)
```
配置app：包含连接mysql数据库的一个库
```python
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql+pymysql://root:mldn123456@192.168.17.128:3306/db_phone_analysis_data_3?charset=utf8'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
app.config['SECRET_KEY'] = '123456'

#  mysql+pymysql://root:mldn123456@192.168.17.128:3306/db_phone_analysis_data_3?charset=utf8
#  mysql+pymysql://数据库账号：密码@地址：端口/库名?编码
```
从flask_sqlalchemy导入SQLAlchemy
```python
from flask_sqlalchemy import SQLAlchemy
```
将app加进SQLAlchemy包里，并创建db对象，最后从app导入routes
```python
db = SQLAlchemy(app)
from app import routes
```
<br>

> [!Danger|label:]## model文件里面的data.py文件：
从app导入db对象，db对象则是一个连接数据库的一个包
```python
from app import db
```
链接不同的表，则要创建不同的类，并在类名的后面括号里加入db.Model，例如：
```python
class Tbl_Sales_Cpu_Count(db.Model):
```
类里面__tablenaem__后面的名字是__init__连接的库里面的表的名字，表名要跟数据库的一致,类名自主定义<br>
Column则是定义字段的方法:里面所定义的整型，字符型前需要加db. ，因为db可以理解为处理数据库的工具<br>
primary_key为自增长序列，所以这一个字段在html文件中引用的时候就不需要引用

<br>

> [!Tip|label:]## model文件里面的__init__.py文件：
从本文件夹中的data文件中导入类
```python
from .data import Tbl_Sales_Os_Count,Tbl_Sales_Cpu_Count,Tbl_Sales_Rom_Count,Tbl_Sales_Ram_Count
```
这时候，在其他文件中要用到这个models文件夹中的模块的时候，就可以直接从models中导包，不需要.models.data import *** 或者有更多个model文件，这时__init__文件就起到了作用。

<br>

> [!Note|label:]## app文件里面的routes.py文件
可以直接从app下的models导包，导的包来自models文件下__init__文件里导的包<br>
而这里导的类则用在定义下面函数要返回的值，返回的则是表的字段。<br>
在html文件中就可以点.出表的字段
```python
from app.models import Tbl_Sales_Os_Count,Tbl_Sales_Cpu_Count,Tbl_Sales_Rom_Count,Tbl_Sales_Ram_Count
```
如果要正版的使用查询功能，则需要用到数据库，那么就需要从app文件中导db，如这样使用
```python
from app import db
res = db.session.query(Tbl_Sales_Cpu_Count).all()
#就是查找这个函数里面的所有，而这个函数定义的是一个表，意思则是查询表的所有字段
```
使用简化版的则是：
```python
res = Tbl_Sales_Cpu_Count.query.all()
#这里就不需要导db
```
@app.toute下面的函数返回的值直接进入该路由路径里输出的值<br>
@app.toute下面的函数下面的函数名随便定义。<br>
从flask 导入 render_template 这个包是为了把返回的值传到html文件中。

<br>

> [!WARNING|label:]## templates文件里面的index.html文件
- head：
    - title后面的字段是显示在网页标签上的字段
        - ![](./11.jpg)
    - script中src属性定义的是echarts文件路径
    - body定义的是一个盒子，给该盒子设定id，在一个html文件中，如果在body有多个script，里面引用的id不能重复，所以在一个html文件中，如果不引用相同的id所定义的东西，那id就不能重复。
- body:
    - 先定义一个盒子，准备一个具备高宽的 DOM 容器。
    - 然后写script
        - 基于准备好的dom，初始化echarts实例
        - ```python
          var myChart = echarts.init(document.getElementById('main'));
          ```
        <br>
        
        - **指定图表的配置项和数据**🧩🧩🧩
        <br><br>
        - 柱状图或者折线图
          ```python
            var option = {
            title: {
                text: 'ECharts 入门示例'
            },
            tooltip: {},
            legend: {
                data:['销量']
            },
            xAxis: {
                data: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
            },
            yAxis: {},
            series: [{
                name: '销量',
                type: 'bar', #或者折线图   line
                data: [5, 20, 36, 10, 10, 20]
            }]
          };
          ```
        - 饼图

        ```python
            option = {
                title: {
                    text:{text:'手机cpu型号销量统计',
                          left:'center'},,
                    subtext: '纯属虚构',
                    left: 'center'
                    },
                tooltip: {},
                legend: {},
                series: [
                {
                    name: '销量',
                    type: 'pie',
                    radius: '50%',
                    data: [
                        {% for i in cpu_res %}
                        {value:{{i.fld_sale_count}},name:'{{i.fld_cpu_name}}'},
                        {% endfor %}
                    ], 
                }]
                };
        ```
        **柱状图，折线图的option里面包括title，tooltip，legend，xAxis，yAxis，series**<br>
        **饼图的series里面包括name，type，data** 🎬

        **饼图的option里面包括title，tooltip，legend，series**<br>
        **饼图的series里面包括name，type，radius，data**🏹

        **以上的图是data里面的值直接写里面，也可以写在定义盒子后面，定义变量，然后此处引用变量就行**🎸

        - 使用刚指定的配置项和数据显示图表。
        - ```python
            myChart.setOption(option)
          ```
# 启动整个项目♨️♨️♨️♨️♨️
```python
from app import app
if __name__=='__main__':
    app.run()
```