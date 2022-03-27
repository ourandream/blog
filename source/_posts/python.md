---
title: python
tags: code
categories: languages
mathjax: true
abbrlink: a4d4b8b8
---
python较任意上手,基本语法较少,不过要熟练还是需要一定的练习.
准备先熟悉一下基础然后开始折腾python的库.
<!--more-->
# base
## 注释:
```python
#这是单行
'''
这是
多行
注释
'''
"""
这也是多行注释
"""
```
## 杂七杂八
python支持转义字符,如\n,若想让某个字符中转移字符等失效,在字符串前加r或R
```python
r'\\n'
```

python中不需要声明变量,直接使用即可,变量名的首个字符需要是_或字母,其余部分可以是_或字母或数字


在python中,一切皆对象,如字符串就是一种对象

在python中,执行程序时是一句一句执行的,一个物理行(自己看见的)对应一个逻辑行(python看见的),若想让一个语句有多行或一行有多个语句,可以按如下语法书写:
```python
i=\
5

i=3;i=5
```
在python中,缩进用于指定语句块,拥有同样缩进的语句属于一个语句块
一次缩进大概四格(推荐使用tab键)

range:
```python
print(list(range(1,5)))#结果为[1, 2, 3, 4]
print(list(range(1,5,2)))#2为步长,结果为[1, 3]
```

len:
```python
print(len('1234'))#输出字符串长度
```

# 数据结构
python有四种基本的数据结构,即list 	tuple dictionary and set
## list
即一类相同的对象组成的有序序列
```python
shoplist = ['apple', 'mango', 'carrot', 'banana']#创建list
shoplist.append('rice')#在list的末尾添加元素
shoplist.sort()#排序
del shoplist[0]#索引list中的元素和删除元素,注意删除后索引会重新排序
```

## tuple
多类不同的对象组成的序列,可索引,但不可进行排序 修改等操作
使用：
```python
zoo = ('python', 'elephant', 'penguin')
new_zoo = 'monkey', 'camel', zoo#不推荐这种写法
```
使用元组：
```python
print('All animals in new zoo are', new_zoo)#输出整个tuple
print('Animals brought from old zoo are', new_zoo[2])
#输出tuple的第三个元素
print('Last animal brought from old zoo is', new_zoo[2][2])
#输出tuple第三个元素中的第三个元素
```
注意:
```python
myempty = ()#声明一个空tuple很简单
singleton = (2 , )#声明一个元素的tuple必须按这种写法
```

## dictionary
dictionary即key与value相对应形成的数据结构,有点像地址簿的姓名对应地址
使用:
```python
ab = {
    'Swaroop': 'swaroop@swaroopch.com',
    'Larry': 'larry@wall.org',
    'Matsumoto': 'matz@ruby-lang.org',
    'Spammer': 'spammer@hotmail.com'
}

ab['Guido'] = 'guido@python.org'#添加key value对
print("Swaroop's address is", ab['Swaroop'])#使用

for name, address in ab.items(): 
	print('Contact {} at {}'.format(name, address))
#使用ab中的每个元素
```
注意key需要是不可变对象,value则可以是可变对象,当然也可以是不可变对象

## 序列
上述两种结构都是序列的一种,序列可进行索引操作，和c++中的数组索引类似，不同的操作如下
切片:
```python
print('Item -1 is', shoplist[-1])
#指向序列的最后一个,若是2则是倒数第二个
print('Item 1 to 3 is', shoplist[1:3])
# 第二和第三个
print('Item 2 to end is', shoplist[2:])
#从第三开始到最后
print('Item 1 to -1 is', shoplist[1:-1])
#第二个到倒数第二个
print('Item start to end is', shoplist[:])
#全部

#你也可以在切片时提供第三个参数 _步长_，默认的步长为 1。
shoplist[::2]
#步长2,等到0,2..等元素
shoplist[::-1]
#步长-1,得到倒数第一个,倒数第二个..等元素
```

## set
集合和数学中的集合类似,是无序简单对象的collection
```python
ab = set(['apple','pen'])
if 'pen' in ab:
	print('true')
if 3 not in ab:
	print('true')
#定义与检查是否在集合中

abc = ab.copy()
#复制集合
abc.add('rabbit')
#添加集合元素
ab.remove('pen')
#去除集合元素
if abc.issuperset(ab):
#检查是否为另一集合的大集,相同时仍算大集
	print(ab & abc)
	#输出集合的交集
```

## reference
当创建对象并利用其赋值给另一个对象时,实际两个标识符指向同一个对象:
```python
mylist = shoplist#指向同一个
mylist = shoplist[:]#通过全切片产生副本,指向对象不同
```

## 字符串:
用单引号或双引号指定字符串
```python
'strings'
"strings"
```
注意在python中字符串不可改变

更多字符串操作:
```python
# 这是一个字符串对象
name = 'Swaroop'

if name.startswith('Swa'):
    print('Yes, the string starts with "Swa"')
#检查是否已某个字符串开始
if 'a' in name:
    print('Yes, it contains the string "a"')
#检查某个字符串是否在其中
if name.find('war') != -1:
    print('Yes, it contains the string "war"')
#同上,但会返回字符串的位置,为-1则表明找不到
delimiter = '_*_'
mylist = ['Brazil', 'Russia', 'India', 'China']
print(delimiter.join(mylist))
#用指定字符串填充源字符,结果:Brazil_*_Russia_*_India_*_China
```
# input output
output一般用print,语法如下:
```python
print('strings')
```
若要使用变量输出,可用如下语法:
```python
age=10
stringT='hhh'
print('{1},your age is {0}'.format(age,stringT))
print('your age is {},{}'.format(age,stringT))
'''
注意{}中的数字是可选的,用来指定输出第几个变量
.format本质是调用str类的method
'''
print(message * times)
#该语句可实现变量的多次输出,注意message也可以是字符串常量
print(item, end=' ')#end指定输出的末尾,若不指定则默认为\n
```

format可进行一定调节设定进行输出:
```python
age=10
print('{0:.3f}'.format(age))#指定小数位为3位
# 填充下划线 (_) ，文本居中
# 将 '___hello___' 的宽度扩充为 11 
print('{0:_^11}'.format('hello'))
# 用基于关键字的方法打印显示 'Swaroop wrote A Byte of Pytho
print('{name} wrote {book}'.format(name='Swaroop', book='Python'))
```

# 运算符
+-*,== >=,>> <<,&等运算符与c++中的基本相同
需要注意的运算符如下:

|运算符|功能|
|---|---|
|**|乘方|
|/|除,但不是整除|
|//|整除|
|not and or|布尔与或非|

# 控制流
if-else 语句：
```python
t = 3

if t == 3:
	print('It\'s 3')
elif t == 2:
	print('It\'s 2')
else:
	print('It\'s nothing')
```

while语句:
```python
i = 3
while True:
	i*=2
	print(i)
		if(i>100):
			break
else:
	print(i*3)#注意若遇到break语句则else中的内容不会执行
```

for 语句:
```python
for i in range(1,5):#输出的i从1到4,循环执行四次
	if i == 3:
		continue#与c++中的相同
	print(i)
else:
	print('5')#不满足for的条件时执行,若遇break则不执行
```

# 函数
函数定义例子:
```python
def print_max(a, b):#也可无参数
    if a > b:
        print(a, 'is maximum')
    elif a == b:
        print(a, 'is equal to', b)
    else:
        print(b, 'is maximum')
```

在函数内定义的变量是局部变量,不会影响其他地方的同标识符的变量的值,若想让该变量成为全局变量,使用global:
```python
def func(): 
	global x
```

函数括号中的形参可有默认值:
```python
def say(message, times=1): 
	print(message * times)
#注意只有参数列表末尾的变量可有默认值,不可前面变量有默认值后面却没有
```

调用函数输入参数时可指定参数输入:
```python
def func(a, b=5, c=10):
    print('a is', a, 'and b is', b, 'and c is', c)

func(3, 7)
func(25, c=24)
func(c=50, a=100)
```

参数可为元组或字典,这样一个形参可介绍多个参数:
```python
def total(a=5, *numbers, **phonebook): 
	print('a', a)
#numbers接受元组,phonebook接受字典
```

return可和c++中一样使用,不过不用声明返回值类型

DocStrings
```python
def print_max(x, y):
    '''Prints the maximum of two numbers.

    The two values must be integers.'''
...
```
DocStrings 的书写惯例是：首行首字母大写，结尾有句号；第二行为空行；第三行以后为详细的描述。
可以通过\__doc__属性访问描述:
```python
print(print_max.__doc__)
```

# 模块
在python中，若想实现多文件协作，可使用module（模块），模块可用python写，也可以用c写
导入模块：
```python
import sys#不用.py
from math import sqrt#导入模块的一部分,一般不推荐使用避免命名冲突
sys.argv#使用模块中的函数或变量
```
导入模块是一个开销比较大的事情,可通过创建字节码文件(.pyc),然后导入字节码文件得到一样的效果,并提高效率

模块拥有\__name\__属性,若该属性值为  \__main__,则说明模块是直接在执行而不是被调用

若写自己的模块,有一个属性为\__version__,用于指定版本号

dir()函数返回对象里定义的一系列标识符,若对象是模块则返回变量,函数和类的标识符

程序包就是一个装满模块的文件夹，它有一个特殊的 `__init__.py` 文件，这个文件告诉 Python 这个文件夹是特别的，因为它装着 Python 的模块。



