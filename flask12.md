
1. flask
概念：flask '微'框架

django --> 完善完整高集成的框架
flask --> Flask 不包含数据库抽象层微框架，database，templetes需要自己去组装

2. 安装
创建虚拟环境virtualenv --no-site-packages flaskenv
	cd flaskenv
	cd Script
启动虚拟环境：activate

pip install flask

3. 运行flask
python xxx.py ---> 启动默认127.0.0.1:5000端口

4. 运行参数
debug = True  调试
port = '8000'  端口
host = '0.0.0.0'  IP

5. 修改启动方式
pip install flask-script

python hello.py runserver -p 端口 -h IP -d

6. 蓝图--管理url，规划url
pip install flask-blueprint

	a）初始化
	b）路由注册

7. route规则

django中
	\(\d+)\
	\<?P(\d+)>\

flask中
	<converter:name>

		string: 默认的字符串，可以省略
		int： 整形

		float：浮点型
		path：'/'也是当做字符串返回

		uuid：uuid类型
	
8. request 请求

args--->  GET请求，获取参数

form--->  POST请求，获取参数

files-->  上传的file文件

method--> 请求方式

base_url

path

cookies

9. 蓝图前缀

url_prefix='/hello'

10. response 响应

服务端自己创建，然后返回给客户端

传入自己定义的状态码

make_response('页面'，状态码)


11. session

安装:  pip install flask-session
	  pip install redis

使用数据库：

	SESSION_TYPE类型
	redis
	mongodb
	memcached
	sqlchemy





