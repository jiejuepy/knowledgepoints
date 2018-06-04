
### 下面的一段HTML代码将作为例子
```
html_doc = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
"""
```

1. prettify() 将一个bs对象按照标准格式进行格式化输出
```
from bs4 import BeautifulSoup
soup = BeautifulSoup(html_doc)

print(soup.prettify())
```

2. 几个简单的浏览结构化的方法
	* soup.标签：找出第一个该类标签
	```
	print(soup.title)
	# <title>The Dormouse's story</title>
	```
	* soup.标签.name:获取tag的name属性值
	```
		print(soup.title.name)
		# title
	```
    * soup.title.text 和 soup.title.string 的区别:
	```
	print(soup.title.text)
	print(type(soup.title.text))
	print(soup.title.string)
	print(type(soup.title.string))
	# The Dormouse's story
	# <class 'str'>
	# The Dormouse's story
	# <class 'bs4.element.NavigableString'>
	```
	* find_all（） 和 find（）：前者返回符合条件的标签列表，后者返回一个bs4的tag对象
	* soup.get_text() 获取文档中的所有的文字内容

3. tag标签属性值的操作
	* 获取属性值:获取文档中第一个p标签的所有class属性，返回list，转换的文档是XML格式,那么tag中不包含多值属性
	```
	print(soup.p.get('class'))
    print(soup.p.attrs['class'])
    print(soup.p['class'])
	#['title']
	#['title']
	#['title']
	```
	* 添加、修改属性、删除属性：
	```
	soup.b['class'] = 'content'
    print(soup.b)
    #<b class="content">The Dormouse's story</b>
    soup.b['class'] = 'text'
    print(soup.b)
    #<b class="text">The Dormouse's story</b>
    del soup.b['class']
    print(soup.b)
    #<b>The Dormouse's story</b>
    ```

4. 子节点
	* .contents 属性可以将tag的直接子节点以列表的方式输出 
	```
	print(soup.head.contents)
	# [<title>The Dormouse's story</title>]
	```
	* .children 生成器，可以对tag的直接子节点进行循环
	```
	for j in soup.head.children:
        print(j)
    #<title>The Dormouse's story</title>
    for i in soup.title.children:
        print(i)
	#The Dormouse's story
	```
	* .descendants 属性可以对所有tag的子孙节点进行递归循环 
	```
    for k in soup.head.descendants:
    	print(k)
    # <title>The Dormouse's story</title>
	# The Dormouse's story
	```
	* .string 和 .strings \ stripped_strings:tag只有一个 NavigableString 类型子节点,那么这个tag可以使用 .string 得到子节点;tag中包含多个字符串 [2] ,可以使用 .strings 来循环获取;输出的字符串中可能包含了很多空格或空行,使用 .stripped_strings 可以去除多余空白内容;

5. 父节点
	* .parent:
	```
	<head>标签是<title>标签的父节点;
	文档title的字符串也有父节点:<title>标签;
	文档的顶层节点比如<html>的父节点是 BeautifulSoup 对象;
	BeautifulSoup 对象的 .parent 是None;
	```
	* .parents:通过元素的 .parents 属性可以递归得到元素的所有父辈节点,下面的例子使用了 .parents 方法遍历了\<\a>标签到根节点的所有节点.
	```
    for parent in soup.a.parents:
    	print(parent.name)
	#p
	#body
	#html
	#[document]
	```
6. 兄弟节点
	* .next_sibling 和 .previous_sibling：\<\b>标签有 .next_sibling 属性,但是没有 .previous_sibling 属性,因为\<\b>标签在同级节点中是第一个.同理,\<\c>标签有 .previous_sibling 属性,却没有 .next_sibling 属性:
	```
	sibling_soup = BeautifulSoup('<a><b>text1</b><c>text2</c></b></a>')
	```
	* 在html_doc文档中，第一个\<\a>标签的.next_sibling结果不是第二个标签，而是','
	* 通过 .next_siblings 和 .previous_siblings 属性可以对当前节点的兄弟节点迭代输出;

7. 搜索文档树
 	* 过滤器
 	-- 字符串
 	```
 	soup.find_all('b') 找出所有的<b>标签
 	```
 	-- 正则表达式
 	```
 	soup.find_all(re.compile("^b")) 找出所有以b开头的标签，例子中<body>和<b>都能被找到
 	soup.find_all(re.comile("t")) 找出所有标签中包含't'的标签
 	```
 	-- 列表
 	```
 	soup.find_all(["a", "b"]) 找到文档中所有<a>标签和<b>标签
 	```
 	-- True
 	```
 	soup.find_all(True) True:可以匹配任何值,下面代码查找到所有的tag,但是不会返回字符串节点
 	```
 	-- 方法<br>
 	以下方法找出包含 class 属性却不包含 id 属性的标签：
 	```
 	def has_class_but_no_id(tag):
    	return tag.has_attr('class') and not tag.has_attr('id')
    print(soup.find_all(has_class_but_no_id))
	# [<p class="title content"><b>The Dormouse's story</b></p>, <p class="story">Once upon a # time there were three little sisters; and their names were
	# <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
	# <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> and
	# <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>;
	# and they lived at the bottom of a well.</p>, <p class="story">...</p>]
	```
	* find_all
	```
	-- find_all( name , attrs , recursive , text , **kwargs )
	soup.find_all("p", "title")
	# [<p class="title"><b>The Dormouse's story</b></p>]
	soup.find_all(href=re.compile("elsie"), id='link1')
	# [<a class="sister" href="http://example.com/elsie" id="link1">three</a>]
	```
	-- 特殊属性的搜索
	```
	data_soup = BeautifulSoup('<div data-foo="value">foo!</div>')
	data_soup.find_all(data-foo="value")
	# SyntaxError: keyword can't be an expression
	data_soup.find_all(attrs={"data-foo": "value"})
	# [<div data-foo="value">foo!</div>]
	```
	-- 按CSS搜索
	class 在Python中是保留字,使用 class 做参数会导致语法错误.从Beautiful Soup的4.1.1版本开始,可以通过 class_ 参数搜索有指定CSS类名的tag:
	```
	print(soup.find_all("a", class_="sister"))
	# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
	#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
	#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
	```
	-- text参数
	class 在Python中是保留字,使用 class 做参数会导致语法错误.从Beautiful Soup的4.1.1版本开始,可以通过 class_ 参数搜索有指定CSS类名的tag:
	```
	print（soup.find_all(text="Elsie")）
	# ['Elsie']
	```
	-- limit参数
	```
	soup.find_all("a", limit=2)
	# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
	#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
	```
	-- recursive 参数
	调用tag的 find_all() 方法时,Beautiful Soup会检索当前tag的所有子孙节点,如果只想搜索tag的直接子节点,可以使用参数 recursive=False :
	```
	soup.html.find_all("title")
	# [<title>The Dormouse's story</title>]
	soup.html.find_all("title", recursive=False)
	# []
	```

	* Beautiful Soup中还有10个用于搜索的API.它们中的五个用的是与 find_all() 相同的搜索参数,另外5个与 find() 方法的搜索参数类似.区别仅是它们搜索文档的不同部分.记住: find_all() 和 find() 只搜索当前节点的所有子节点,孙子节点等. find_parents() 和 find_parent() 用来搜索当前节点的父辈节点,搜索方法与普通tag的搜索方法相同,搜索文档搜索文档包含的内容. 我们从一个文档中的一个叶子节点开始.

	* CSS选择器
	Beautiful Soup支持大部分的CSS选择器,在 Tag 或 BeautifulSoup 对象的 .select() 方法中传入字符串参数,即可使用CSS选择器的语法找到tag:<br>
	-- 通过tag标签逐层查找:
	```
	soup.select("html head title")
	# [<title>The Dormouse's story</title>]
	```
	-- 找到某个tag标签下的直接子标签:
	```
	soup.select("head > title")
	# [<title>The Dormouse's story</title>]
	```
	```
	soup.select("p > a")
	# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
	#  <a class="sister" href="http://example.com/lacie"  id="link2">Lacie</a>,
	#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
	```
	```
	soup.select("p > a:nth-of-type(2)")
	# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
	```
	```
	soup.select("p > #link1")
	# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]
	soup.select("body > a")
	# []
	```
	-- 找到兄弟节点标签:(所有兄弟和直系兄弟)
	```
	soup.select("#link1 ~ .sister")
		# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
		#  <a class="sister" href="http://example.com/tillie"  id="link3">Tillie</a>]
	soup.select("#link1 + .sister")
		# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
	```
	-- 通过CSS的类名查找:
	```
	soup.select(".sister")
		# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
		#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
		#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
	soup.select("[class~=sister]")
		# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
		#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
		#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
	```
	-- 通过tag的id查找:
	```
	soup.select("#link1")
		# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]
	soup.select("a#link2")
		# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
	```
	-- 通过是否存在某个属性来查找:
	```
	soup.select('a[href]')
		# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
		#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
		#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
	```
	-- 通过属性的值来查找:
	```
	soup.select('a[href="http://example.com/elsie"]')
		# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]
	soup.select('a[href^="http://example.com/"]')
		# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
		#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
		#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
	soup.select('a[href$="tillie"]')
		# [<a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
	soup.select('a[href*=".com/el"]')
		# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]
	```
