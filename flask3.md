1. 挖坑
block  endblock

2. 加载css

django：
第一种方式：
```
{% load static %}
<link rel="stylesheet" href="{% static 'css/index.css' %}">
```
第二种方式：
```
<link rel="stylesheet" href="/static/css/index.css">
```

flask：
第一种方式：
```
<link rel="stylesheet" href="/static/css/index.css">
```

第二种方式：
```
<link rel="stylesheet" href="{{ url_for('static', filename='css/index.css') }}">
```

3. 过滤器
safe：渲染标签

striptags：渲染之前去掉标签

trim：去掉空格

length：计算长度

第一个字母{{ i|first }}
最后一个字母{{ i|last }}
小写{{ i|lower }}
大写{{ i|upper }},
首字母大写{{ i|capitalize }}

4. 数据库
pip install flask-sqlalchemy

pip install pymysql

	primary_key：指定主键
	autoincrement：自增
	unique：唯一
	default：默认值
	Integer：整形
	String：字符串
	__tablename__：指定数据库名称

SQLALCHEMY_DATABASE_URI='mysql+pymysql://root:123456@localhost:3306/flask3'

5. 事务

原子性，一致性，隔离性，持久性