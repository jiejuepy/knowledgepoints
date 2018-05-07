
## 埋点
* SEO url点击率 
* 作业。统计添加学生的点击次数 既：url /stu/addStu post请求的次数 

1、首先在models中创建一个统计点击次数的表
```
class Visit(models.Model):

	v_url = models.CharField(max_length=30)
	v_times = models.IntegerField()


	class Meta:
		db_table = 'day7_visit'
```
2、创建中间件
```
class VisitTimes(MiddlewareMixin):

	def process_request(self, request):
		path = request.path
	#如果访问的是一个新链接，get方法出错，执行except的内容
	#如果访问的是老链接，执行try的方法
		try:
			visit = Visit.objects.get(v_url=path)
			if visit:
				visit.v_times += 1
				visit.save()
		except Exception as e:
			print(e)
			Visit.objects.create(v_url=path, v_times=1)
```
3、在settings的MIDDLEWARE=[]里面加入创建的中间件

## django自带的注册登录注销
1、首先先把我们自己设置的 AuthMiddleware中间件在settings中注释掉
2、在uauth中的urls里面添加
```
urlpatterns = [
	url(r'^dj_login/', views.djlogin),
    url(r'^dj_regist/', views.djregist),
    url(r'^dj_logout/', views.djlogout)
    ]
```
 3、在uauth中的views中添加相应的方法
 ```
 def djlogin(request):
 	if request.method == 'GET':
 		return render(request, 'login.html')
 	if request.method == 'POST':
 		name = request.POST.get('name')
 		password = request.POST.get('password')
 		# 验证用户名和密码
 		user = auth.authenticate(username=name, password=password)
 		if user:
 			auth.login(request, user)
 			return HttpResponseRedirect('/stu/index/')
 		else:
 			return render(request, 'day6_login.html')

 def djregist(request):
 	if request.method == 'GET':
 		return render(request, 'day6_regist.html')

 	if request.method == 'POST':
 		name = request.POST.get('name')
 		password = request.POST.get('password')
 		#from django.contrib.auth.models import 
 		#此处的User是django自带管理系统的
 		User.objects.create_user(username=name, password=password)
 		return HttpResponseRedirect('/uauth/dj_login')

 def djlogout(request):
 	if request.method == 'GET':
 		auth.logout(request)
 		return HttpResponseRedirect('/uauth/dj_login')
```
 4、由于在中间件注销了我们自己设置的中间件登录验证代码，因此现在要实现访问其他页面需要已登录的条件,需要在urls中方法加上login_required(),强制登录

## 日志logging

四个组成： 
- loggers：用来处理传入的信息
- handlers：用来处理信息的
- filters：过滤loggers传递给handlers的信息，加一些处理控制
- formatters：格式化，将需要保存到日志文件中的信息进行统一格式化

错误等级：
- CRITICAL>ERROR>WARNING>INFO>DEBUG

* critical：重大的bug
* error：系统里面有错误
* warning：警告
* info：正常
* debug：调试信息

在settings中设置log的配置：
```python
#创建日志的路径
LOG_PATH = os.path.join(BASE_DIR, 'log')
# 如果地址不存在，则自动创建log文件夹
if not os.path.isdir(LOG_PATH):
    os.mkdir(LOG_PATH)

LOGGING = {
    'version': 1,
    #true为禁用loggers
    'disable_existing_loggers': False,
    #格式化, 可以根据需要设置多种格式化版本
    'formatters': {
        'default': {
            'format': '%(levelname)s %(funcName)s %(module)s %(created)s %(message)s'
        },
        'simple': {
            'format': '%(levelname)s  %(module)s %(created)s %(message)s'
        }
    },

    'handlers': {
        'stu_handlers': {
            'level': 'DEBUG',
            # 日志文件指定大小，比如为5M，超过5M重新备份，然后写入新的日志
            'class': 'logging.handlers.RotatingFileHandler',
            'maxBytes': 5 * 1024 * 1024,
            # 文件存放地址
            'filename': '%s/log.txt' % LOG_PATH,
            'formatter': 'default'
        },
        'uauth_handlers': {
            'level': 'DEBUG',
            # 日志文件指定大小，比如为5M，超过5M重新备份，然后写入新的日志
            'class': 'logging.handlers.RotatingFileHandler',
            'maxBytes': 5 * 1024 * 1024,
            # 文件存放地址
            'filename': '%s/uauth_log.txt' % LOG_PATH,
            'formatter': 'simple'
        }
    },
    'loggers': {
        'stu': {
            'handlers': ['stu_handlers'],
            'level': 'INFO'
        },
        'auth': {
            'handlers': ['uauth_handlers'],
            'level': 'INFO'
        }
    },
    'filters': {
    
    }
}
```
## rest和restful风格
1.安装：
pip install djangorestframework
pip install django-filter

2.settings的app加上REST_FRAMEWORK =[]
3.urls中加上url，后面不要加/  用SimpleRouter的方法添加url
4.views中定义方法，继承CRUD，与Postman(测试API工具)中对应
* mixins.RetrieveModelMixin  get获取指定某一个
* mixins.ListModelMixin get获取所有信息
* mixins.DestroyModelMixin, Delete删除
* mixins.CreateModelMixin, post创建

5.创建serializers.py序列化

规定在API返回哪些数据，以及对错误信息进行说明

6.为了规范丢给前端的返回数据，创建RenderResponse.py

7.在settings中配置restful api返回结果

8.修改返回的结构：分页

9.搜索过滤
* 先卸载 pip uninstall djangorestframework
* 再装 pip install djangorestframework==3.4.6
* 创建 filter.py
用django_filters定义搜索的字段
* 在settings中增加'DEFAULT_FILTER_BACKENDS': ('rest_framework.filters.DjangoFilterBackend','rest_framework.filters.SearchFilter'),