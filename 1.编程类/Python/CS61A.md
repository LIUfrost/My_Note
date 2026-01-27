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
#函数可以作为一个参数传递给另一个函数
def cube(k):
	return pow(k,3)
def summation(n,term):
	total, k = 0,1
	while k <= n:
		total,k = total + term(k),k + 1  #这里term函数名作为参数
	return total
#函数可以作为返回值
def make_adder(n):
	def adder(k):
		return k + n
	return adder
add_three = make_adder(3)
add_three(4)
>>>7
make_adder(1)(3)      #make_adder(1):Operater;(2):Operand操作数
```
>[在终端运行Python文件]
>python main.py：直接运行文件
>python -i main.py:交互状态下运行文件
>[科里化]
>将一个需要多个参数的函数转化为多个只需要一个参数的函数
>[高阶函数]
>可以将另一个函数作为参数传递的函数
### 高阶函数
- 定义：就是一个接受另一个函数作为参数值或返回一个函数作为返回值或两者都有的函数
```Python
#例一
def make_adder(n):
	def adder(k):
		return k + n
	return adder
add_three = make_adder(3)
add_three(4)
#例二
def f(x,y):
	return g(x)
def g(a):
	return a + y
result = f(1,2)
>>>Error:y is not defined
```
>[框架/帧思维]每个函数调用的时候重新生成一个帧
>			   这个帧的父帧是定义此函数的帧
>[例一]                                                        [例二]

Global frame                                               Global frame
	make_adder->func make_adder(n)          f->func f(x,y)
	add_three->func adder(k)                        g->func g(a)
F1(parent = G)                                             f(parent = G)
	n->3                                                            x->1
	adder->func adder(k)                                y->2
	Return value->func adder(k)               g(parent = G)
F2(parent = F1)                                                a->1
	k->4
	return value->7
### Lambda Expressions
- 定义：是输出为函数的表达式
- 导入：我们知道，函数名字也可以通过另一个名字重新赋值，但仅限于函数已经定义过了，但如果我们要将一个变量赋值为一个新定义的函数呢？
```python
x = 10
square = x * x
x
>>>10
square
>>>100     #此时square不是一个函数，因为它只是代表x*x的数值
square = lambda x: x*x
square
>>><function <lambda> at xxxxxxxx>
square(4)
>>>16
square(10)
>>>100     #此时square变成了一个函数
```
>[lambda表达式]的基本格式
>lambda x:  返回值（其中，x作为形式参数传递）
>lambda表达式会被解释为一个可以以这种方式解释的函数
>[注意]Lambda表达式只能用于输出单个表达式
### 科里化
- 定义：科里化是将多参数函数转换为一个单参数高阶函数的行为，该函数返回一个接受其余参数的函数
```python
#第一种形式
def curry2(f)
	def g(x)
		def h(y)
			return f(x,y)
		return h
	return g
from operator import add
m = curry2(add)
add_three = m(3)
add_three(2)
>>>5
#第二种形式
curry2 = lambda f: lambda x: lambda y:f(x,y)
```
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


