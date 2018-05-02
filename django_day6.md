1、set_cookie(key, value, max_age=10)

max_age 存活时间：秒

2、加载static方式
* {% load static%} {% static ''%}

* /static/xxx.css

3、delete_cookie(key)

4、上传图片
* pip install Pillow
* 页面form中加上enctype='multipart/form-data'

5.面向切面编码 AOP
```
def x (func f):
	def g():
		# 登录验证
		xxxx
		f()
	return g
```
process_request: 在处理url路由之前进行处理逻辑

process_response: 在响应返回浏览器之前调用

process_view：调用视图之前执行

process_templates_resposne：在视图刚好执行完的时候调用

6、分页
Paginator对象
* page(number): 返回number页的数据
* count()
* num_pages：多少页
* page_range:[1,2,3,4]

page对象
* has_next:是否有下一页
* next_page_number:下一页
* has_previous:是否有上一页
* previous_page_number:上一页
* number:当前页数