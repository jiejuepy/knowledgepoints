
exclude() 查询不满足条件的其他情况
filter() 和get()的区别：
* get方法是从数据库的取得一个匹配的结果，返回一个对象，如果记录不存在的话，它会报错。如果你用django的get去取得关联表的数据的话，而关键表的数据如果多于2条的话也会报错。
* filter方法是从数据库的取得匹配的结果，返回一个对象列表，如果记录不存在的话，它会返回[]

1.关联
1:1 OneToOneField 主键和外键是一对一的关系，在关联表中，只能关联一个主表的id


3.on_delete
默认是级联关系cascade 主表删除，从表也删除
set_null, 主表删除,从表关联字段为null
protect， 不让删除
set_default 主表删除，从表关联字段设置为默认值

4.静态资源加载
2种方式
* static/images/xxx.png
* 
```
{% load static%}

{% static 'images/xxx.png'%}
```

5.htlm中的for
```
{% for i in stu %}
{% empty %}
	xxxxx(友好显示)
{% endfor %}
```
6.if语句
```
{% if xxx %}
{ ednif %}
```
7.ifequal
```
{% ifequal forloop.counter 3 %}
    姓名：<strong>{{ stu.stu_name }}</strong>
{% else %}
    姓名：{{ stu.stu_name }}
{% endifequal %}
```
8.forloop
forloop.counter 总是一个表示当前循环的执行次数的整数计数器。 这个计数器是从1开始的，所以在第一次循环时 forloop.counter 将会被设置为1。
计数从0开始：{{ forloop.counter0 }} <br>
计数从1开始：{{ forloop.counter }}<br>
计数从最后开始，到1停：{{ forloop.revcounter }} <br>
计数从最后开始，到0停：{{ forloop.revcounter0 }} <br>

9.过滤器（|）
在变量显示前修改
推荐篇文章，讲的比较详细：https://www.cnblogs.com/huangxm/p/6286144.html
