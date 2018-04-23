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

pip install virtualenv虚拟环境创建,纯净环境，没有其他库

在D盘创建了env文件夹
```
virtualenv --no-site-packages testenv 创建了testenv虚拟环境
```
进入虚拟环境env/testenv/Scripts/activate

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

python manage.py runserver ip:端口 启动django项目

1.pycharm里面settings选择我们自己创建的虚拟环境解释器
2.配置debug
run > debug> edit configuration > 添加个python >配置script path > parameters

创建app
python manage.py startapp app(name)

app
* __init__.py:初始化
* admin.py管理后台注册模型
* apps.py:settings.py里面注册app时需要使用到，一般不推荐这样使用
* from app.apps import AppConfig
* AppConfig.name
* models.py:写模型的地方
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