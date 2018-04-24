## Django

MVC-- model数据存取层 \ view 表现层\ controller业务逻辑层

MVT--model负责业务和数据库的对象

   --view负责业务逻辑并适当调用model和template

   --template负责把页面渲染展示给用户
![](https://github.com/jiejuepy/knowledgepoints/blob/master/pictures/MVT.jpg)

django的模式严格来说应该是MVT

B/S 浏览器/服务器 - C/S 客户端/服务器

### 安装env环境
virtualenv虚拟环境创建指南

使用背景：有些项目是老版本，需要引入虚拟环境将每个项目环境隔离起来

pip install virtualenv虚拟环境创建

在D盘创建了env文件夹
```
virtualenv --no-site-packages testenv 创建了testenv虚拟环境，纯净环境
```
Windows
进入虚拟环境env/testenv/Scripts/activate
Linux
source bin/activate 退出source deactive

在里面可以pip安装我们需要的库

进入/退出 虚拟环境 activate/deactivate

### 安装django
pip install django==1.11版本号

--在虚拟环境下创建了helloworld文件夹项目
```
django-admin startproject helloworld 
```
wsgi网关
migrate映射

启动django服务
python manage.py runserver

修改settings.py 里面的LANGUAGE_CODE = 'zh-hans'页面改成中文

python manage.py runserver ip:端口 启动django项目(ip和端口可以不写，默认8000端口)

1.pycharm里面settings选择我们自己创建的虚拟环境解释器
2.配置debug
run > debug> edit configuration > +添加个python >配置script path：manage > parameters ：runserver 8080

* __init__.py:初始化，配置MySQL连接
* settings.py:配置信息位置，databases等
* urls.py:路由
* wsgi.py:网关
* admin.py管理后台注册模型

创建app
python manage.py startapp app(name)

app
* apps.py:settings.py里面注册app时需要使用到，一般不推荐这样使用
* from app.apps import AppConfig
* AppConfig.name
* models.py:写模型的地方
* db_tables:定义数据库中的表
* views.py:写处理业务逻辑的地方


1、在views.py
```
def first_hello(request):
    return HttpResponse('hello python')
```
2、在urls.py
```
urlpatterns = [
    url(r'hello', views.first_hello)]
```
3、127.0.0.1:8080/hello 显示hello python

迁移数据库
python manage.py makemigrations
python manage.py migrate

### MVT的简单连接
1.先建立一个叫stu的app，在settings的INSTALLED_APPS里面添加该app

python manage.py startapp stu

2.在stu里面新建立一个urls.py
```
# -*- coding:utf-8 -*-
from django.conf.urls import url

from stu import views
urlpatterns=[
    url(r'add/', views.addstudent),
]
```

3.在项目helloworld的urls添加app的urls：
```
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'app/', include('app.urls')),
    url(r'stu/', include('stu.urls'))
]
```

4.在stu的models.py里定义Student
```
class Student(models.Model):
    name = models.CharField(max_length=20)

    sex = models.BooleanField()

    class Meta:
        db_table = 'stu'
```

5.在stu的views.py里定义addstudent
```
from django.http import HttpResponse
from django.shortcuts import render

# Create your views here.
from stu.models import Student


def addstudent(request):

    stu = Student()
    stu.name = '张三'
    stu.sex = 1
    stu.save()
    return HttpResponse('添加成功')
```

6、settings里面连接本地的MySQL，设置host,user等参数。本地也要创建相应的数据库

7、迁移
```
>python manage.py makemigrations
>python manage.py migrate
```

8、刷新页面127.0.0.1:8080/stu/add

推荐一篇文章《Django基于ORM操作数据库的方法详解》http://www.aspku.com/tech/jiaoben/python/307274.html

创建超级管理员
python manage.py createsuperuser

admin.py管理后台注册模型

注册的第一种方式
admin.site.register()

注册的第二种方式 装饰器
@admin.register()

### 保持数据
```
stu = Student()
stu.name = 'xxx'
stu.save()
```
ORM 对象关系映射，翻译机

### 模式字段
* CharField(max_length=6):字符串
* BooleanField()：布尔类型
* DateField()：年月日，日期
* auto_now_add=True：第一次创建的时候赋值
* auto_now=True：每次修改的日期
* DataTimeField:年月日时分秒
* AutoField:自动增长
* DecimalField(max_digits=3, decimal_places=1)
	* max_digits：总位数
	*　decimal_places：小数后有几位
* TextField:存长文本信息
* IntegerField:整数
* FloatField：浮点型
* FileFiled:文件上传字段
* ImageField():上传图片
	* upload_to=''指定上传图片的路径

### 模型参数
default：默认值
null = True 数据库该字段可以为空
blank = True 网页表单填写的内容可以为空

primary_key(True/False):创建主键
unique(True/False)：唯一

如果在models修改字段名字，再执行迁移就更新了

在工程下面创建一个dictionary：templates
在settings下：'DIRS': [os.path.join(BASE_DIR, 'templates')]

objects 对象方法
通过模型.objects 来实现对数据的CRUD

select * from student;
模型.objects.all()

区别：get 和filter

get:返回一个满足条件的对象，没有满足条件的则直接报DoesNotExit的异常，如果查询结果有多个数据的话，就报MulitiObjectsReturned

filter()：返回满足条件的结果 

first()：返回第一条数据

last()：返回最后一条数据

count()：求和

gt大于 / gte大于等于

lt小于 / lte小于等于

from django.db.models import F，Q
F():操作数据表中的某列值，F()允许Django在未实际链接数据的情况下具有对数据库字段的值的引用，不用获取对象放在内存中再对字段进行操作，直接执行原生产sql语句操作。<br>
Q():对对象进行复杂查询，并支持&（and）,|（or），~（not）操作符。


