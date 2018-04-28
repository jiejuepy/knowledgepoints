
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
* post 提交数据隐藏了
* get 提交数据在url上，
* put 更新全部数据
* patch 更新局部数据
* delete 删除
Student.objects.filter(id=stu_id).delete()

Student.objects.filter(id=2).update(stu_name='大乔')


### QueryDict 与 dict的区别
* 定义在django.http.QueryDict
* request对象的属性GET、POST都是QueryDict类型的对象
* 与python字典不同，QueryDict类型的对象用来处理同一个键带有多个值的情况
* 方法get()：根据键获取值
	* 只能获取键的一个值
	* 如果一个键同时拥有多个值，获取最后一个值

pk==id

cookie: 在浏览器
session: 在服务器

set_cookie(key, value, expire)(名字，值，过期时间)
![](https://github.com/jiejuepy/knowledgepoints/blob/master/pictures/cookie.jpg)

del_cookie

令牌有过期时间，可以设置

服务端的令牌时间，放在MySQL，mongodb

### Python Requests库 Get和Post的区别:一般的理解
GET后退按钮/刷新无害，POST数据会被重新提交（浏览器应该告知用户数据会被重新提交）。<br>
GET书签可收藏，POST为书签不可收藏。<br>
GET能被缓存，POST不能缓存 。<br>
GET编码类型application/x-www-form-url，POST编码类型encodedapplication/x-www-form-urlencoded 或 multipart/form-data。为二进制数据使用多重编码。<br>
GET历史参数保留在浏览器历史中。POST参数不会保存在浏览器历史中。<br>
GET对数据长度有限制，当发送数据时，GET 方法向 URL 添加数据；URL 的长度是受限制的（URL 的最大长度是 2048 个字符）。POST无限制。<br>
GET只允许ASCII字符。POST没有限制。也允许二进制数据。与POST相比，GET的安全性较差，因为所发送的数据是 URL 的一部分。在发送密码或其他敏感信息时绝不要使用GET！POST比GET更安全，因为参数不会被保存在浏览器历史或 web 服务器日志中。<br>
GET的数据在 URL 中对所有人都是可见的。POST的数据不会显示在 URL 中。<br>

网上百度了一下2者的区别，还是有点浆糊，大家可以看看知乎上的讨论：
1、https://www.zhihu.com/question/28586791
2、https://www.zhihu.com/question/28586791/answer/145424285