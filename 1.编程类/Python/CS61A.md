---
tags:
  - python
---
## Functions
### 赋值
```python
#多变量赋值
a,b=1,3
#函数名可以赋值
max(1,3)      #max原来是一个函数
f=max         #max的功能赋值到f中
max=9         #max不再是一个函数，而是一个值
```
```python
#库
from operator import add,mul #运算库
from math import pi #数学库
#print函数
print(None)
>>None
None
>>            #输出空
print(2,3)
>>2 3
print(print(1),print(2))
>>1
>>2
>>None None
```
>[函数的分类]
>Pure Functions：just return values
>Non-Pure Functions：have side effects
>A side effect isn’t a value it‘s anything that happens as a consequence of calling a function
### 函数
```Python
#函数定义
def xxx():
	return xxx
def area():
	return pi * radius *radius
radius = 3
area()        #会自动检测radius的值并输出结果

def xxx():
	xxx       #不返回值
#赋值顺序
a=3
b=2
a,b=a+b,a     #先计算a+b和a的值，再进行赋值
#函数可以返回多个值
def divide_exact(n,d):
	return n//d, n%d
quotient,remainder = divide_exact
#函数形式参数可以设置默认值
def divide_exact(n,d=10):   #如果调用的时候只有一个参数，则默认d=10
	xxxxxx                 #如果调用的时候给了d的值，则d改变
```
>[在终端运行Python文件]
>python main.py：直接运行文件
>python -i main.py:交互状态下运行文件
### 运算符
```Python
2013 / 10
>>201.3
2013 // 10
>>201
2013 % 10
>>3
from operator import truediv,floordiv,mod
truediv(2013,10)
>>201.3
floordiv(2013,10)
>>201
mod(2013,10)
>>3
```
### 语句
#### Conditional Statements(条件语句)
```python
def abs(x):
	if x<0:
		return -x
	elif x==0:
		return 0
	else:
		return x
```
>[条件假值]：False, 0, ' ', None
>[条件真值]：Anything else
#### While Statements(循环语句)
```python
i,total - 0,0
while i<3:
	i = i + 1
	total = total + i
```
>[典型例子]
```python
def if_(c,t,f):
	if c:
		return t
	else:
		return f
from math impoty sqrt
def real_sqrt1(x):
	return if_(x>=0,sqrt(x),0)
def real_sqrt2(x):
	if x>=0:
		return sqrt(x)
	else:
		return 0
#测试终端
>>>real_sqrt1(4)
2.0
>>>real_sqrt1(-4)
ValueError:math domain error    #函数会先尝试算出所有实际参数的值
>>>real_sqrt2(4)
2.0
>>>real_sqrt2(-4)
0
```
#### Logical Operators(条件运算)
- and
- or
```python
from math import sqrt
def has_big_sqrt(x):
	return x>0 and sqrt(x)>10
#如果左边为假，则右边不会被运算，这样避免了程序崩溃
```
#### assert语句
```python
from math imort pi,sqrt
def area(r,shape_constant):
	assert r>0,'A length must be positive'
	return r * r * shape_constant
def area_square(r):
	return area(r,1)
def area_circle(r):
	return area(r,pi)
#如果assert语句的前面为真，则不输出后面的话，若为假，则报错并输出后面的话
#泛化处理典型例子
```


