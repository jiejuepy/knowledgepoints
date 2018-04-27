
date :y 两位年 Y 四位年 h:12时制  H:24小时制
* 生日：{{ stu.stu_birth | date:'Y-m-d' }}
* 创建时间：{{ stu.stu_create_tim | date:'y-m-d h:m:s'}}

注释
* {# #}
* {% comment %}{% endcomment %}
大小写转化
* upper小写变大写
* lower大写变小写

数学成绩10倍：
```
{%   widthratio 10 1 stu.stu_shuxue  %}
widthratio 数 分母 分子
{%  widthratio  stu.stu_shuxue 1 10 %}
```

判断id能否被2整除 返回True / False
```
{%  if stu.id|divisibleby:2 %}
```

{% url 'namespace:name' value %}
工程url中定义namespace
详情页面保存信息

请求
post 提交数据隐藏了
get 提交数据在url上，
put 更新全部数据
patch 更新局部数据
delete 删除
Student.objects.filter(id=stu_id).delete()
Student.objects.filter(id=2).update(stu_name='大乔')


QueryDict 与 dict的区别
pk==id

cookie: 在浏览器
session: 在服务器

set_cookie(key, value, seconds)
del_cookie

令牌有过期时间，可以设置
服务端的令牌时间，放在MySQL，mongodb

Python Requests库 Get和Post的区别
网上百度了一下2者的区别，还是有点浆糊，大家可以看看知乎上的讨论：
https://www.zhihu.com/question/28586791