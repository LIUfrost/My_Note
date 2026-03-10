---
tags:
  - python
---
\*\*:乘方  \\\\:取整
bool值：0，空字符‘’

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
	[闭包]：在函数嵌套定义时，一个函数记住了它被创建时所处的环境(变量等)，即使离开这个环境，他依旧可以访问这些变量
```python
#case1
def pirate(arggg):
	print('matey')
	def plunder(arggg):
		return arggg
	return plunder
#注意：plunder里的arggg已经不是原来的arggg了
>>>pirate(pirate(pirate))(5)(7)
matey
matey
Error
#case2
def horse(mask):
	horse = mask
	def mask(horse):
		return horse
	return horse(mask)
mask = lambda horse:horse(2)
horse(mask)
```
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
### 一个高阶函数的例子
```python
from wave import open
from struct import Struct
from math import floor
frame_rate = 11025
def encode(x):
	"""Encode float x between -1 and 1 as two bytes"""
	i = int(16384*x)
	return Struct('h').pack(i)

def paly(sampler,name='song.wav',seconds=2):
	"""Write the output of a sampler function as a wav file"""
	out = open(name,'wb')
	out.setnchannels(1)
	out.setsampwidth(2)
	out.setframerate(frame_rate)
	t=0
	while t<seconfd*frame_rate:
		sample = sampler(t)
		out.writeframes(encode(sample))
		t = t+1
	out.close()

def tri(frequency,amplitude=0.3):
	"""A continuous triangle wave"""
	period = frame_rate // requency
	def sampler(t):
		saw_wave = t / period - floor(t/period + 0.5)
		tri_wave = 2*abs(2*saw_wave)-1
		return amplitude * tri_wave
	return sampler
c_freq, e_freq, g_freq = 261.63, 329.63, 392.00
def both(f,g):
	return lambda t: f(t) + g(t)

def note(f,start,end,fade = 0.01):
	def sampler(t):
		seconds = t/frame_rate
		if seconds<start:
			return 0
		elif seconds > end:
			return 0
		elif seconds<start + fade:
			return (seconds-start)/fade *f(t)
		elif seconds>end - fade:
			return(end - seconds)/fade * f(t) 
		else:
			return f(t)
	return sampler
c, e = tri(c_freq), tri(e_freq)
play(both(note(c,0,1/4),note(e,1/2,1)))
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
### 函数的抽象
```python
def square(x):
	return mul(x,x)
def sum_square(x,y):
	return square(x) + square(y)
#问题是：sum_square需要知道多少square的信息呢
#需要知道square有一个调用值 ———— yes
#需要知道square的固有名字是square———— NO
#需要知道square的作用是平方————yes
#需要知道square需要调用mul这个函数————NO
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
- while的一些用法
```python
#利用while求反函数
def search(f):
	x = 0
	while not f(x):
		x += 1
	return x

def inverse(f):
	return lambda y: search(lambda x: f(x) == y)
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
### 修饰器
```python
def trace1(fn):
	"""Returns a version  of fn that first prints before it 
	is called
	fn - a function of 1 argument
	"""
	def traced(x):
		print('calling',fn,'on argument',x)
		return fn(x)
	return traced
@trace1
def square(x):
	return x*x
#is identical to
def square(x):
	return x*x
square = trace1(square)
```
### 递归
- 函数内部可以引用原函数
```python
def print_sums(x):
	print(n)
	def next_sum(x):
		return print_sums(n+k)
	return next_sum
print_sums(1)(3)(5)
>>>1
>>>4
>>>9
```
#### 递归函数(recursive functions)
- 定义：其主体调用了他自身，无论直接还是间接
```python
#sum digits without a while statement
def split(n):
	return n//10,n%10
def sum_digits(n):
	if n<10:
		return n
	else:
		all_but_last,last = split(n)
		return sum_digits(all_but_last) + last
```
>[验证方法]：利用数学归纳法验证
- 相互递归
```python
#Luhn算法：用于计算信用卡号的校验和
"""
原理：从左往右数，将所有奇数位上的数字翻倍，再将其十位和个位相加
再将所得到的所有数字相加
"""
def luhn_sum(n):
	if n<10:
		return n
	else:
		all_but_last,last = split(n)
		return luhn_sum_double(all_but_last) + last
def luhn_sum_double(n):
	all_but_last,last = split(n)
	luhn_digit = sum_digits(2*last)
	if n<10:
		return luhn_digit
	else:
		return luhn_sum(all_but_last) + luhn_digit
#Inverse Cascade
def inverse_cascade(n):
	grow(n)
	print(n)
	shrink(n)
def f_then_g(f,g,n):
	if n:
		f(n)
		g(n)
grow = lambda n:f_then_g(grow,print,n//10)
shrink = lambda n:f_then_g(print,shrink,n//10)
```
#### 树形递归
- 会产生树形过程，每当执行递归函数的主体，对该函数的调用超过一次
```python
def fib(n):
	if n==0:
		return 0
	elif n==1:
		return 1
	else:
		return fib(n-2) + fib(n-1)
```
### 列表
```python
odds = [41,43,47,49]
odd[0]
>>>41
a,b = odds[0:2]
>>>a赋值为41，b赋值为43
len(odds)
>>>4
getitem(digit,3)
>>>8
```
#### 列表的基本操作
```python
digits = [1,8,2,8]
#求长度
len(digits)
>>>4
#复制和融合
[2,7] + digits*2
>>>[2,7,1,8,2,8,1,8,2,8]
add([2,7],mul(digits,2))
>>>[2,7,1,8,2,8,1,8,2,8]
#混合列表
pairs = [[10,20],[30,40]]
pairs[1]
>>>[30,40]
pairs[1][0]
>>>30
```
#### Containers
```python
digits = [1,8,2,8]
1 in digits
>>> True
5 in digits
>>> False
'1' in digits
>>> False
[1,8] in digits
>>> False
[1,2] in [3,[1,2],4]
>>> True
```
#### For 循环遍历
```python
def count(s,value):
	total,index = 0,0
	while index < len(s):
		element = s[index]
		if element == value:
			totla += 1
		index = index + 1
	return total
    #等效于
    for element in s:
	    if element == value
		    total += 1
	return total
```
#### 序列解包(Sequence Unpacking)
```python
pairs = [[1,2],[2,2],[3,2],[4,4]]
same_count = 0
for x,y in pairs:
	if x == y
		same_count = same_count + 1
same_count
>>>2
```
#### Range Type
- A range is a sequence of consecutive integers
>[特点]：掐头去尾——range(-2,2) = -2,-1,0,1
>[好处]：I.长度直接等于end - start
> 		II.元素等于starting value + index
- 列表构造器(list constructor)
```python
list(range(-2,2))
[-2,-1,0,1]
list(range(4))
[0,1,2,3]
```
>[注]use \"\_\"来代表你不在乎的变量
>例如`for _ in range(5)`
#### list comprehension(列表推导)
```python
odds = [1,3,5]
[x+1 for x in odds]
>>>[2,4,6]
[x+1 for x in odds if 25 % x == 0]
>>>[2,6]
#例子：寻找一个数的因数
def divisiors(n):
	return [i] + [x for x in range(2,n) if n %x==0]
	
```
#### slicing(切片)
```python
odds = [3,5,7,9,11]
list(range(1,3))
>>>[1,2]
[odds[i] for i in range(1,3)]
>>>[5,7]
#切片(掐头去尾)
odds[1:3]
>>>[5,7]
odds[:3]
>>>[3,5,7]
```
#### sum&all&max
>一个内置函数sum：对列表中的数据求和
>sum(list,(start值))，其中start可以忽略
>例如：sum([2,5,7])
```python
#sum函数
sum([2,3,4])
>>>9
sum([[2,3],[4]],[])
>>>[2,3,4]
sum([[2,3],[4]])
>>>error    #后面不填默认为0，一个数字和列表不能直接相加
#max函数
#max(iterable[,key=func])->value
#max(a,b,c,...[,key=func])->value
max(range(5))
>>>4
max(range(10),key=lambda x:7-(x-4)*(x-2))
>>>3     #抛物线顶点为3
#all函数
#all(iterable)->bool
#return true if bool(x) is true for all values x in the iterable
#if the iterable if empty,return true
[x<5 for x in range(5)]
>>>[True,True,True,True,True]
all([x<5 for x in range(5)])
>>>True
all(range(5))
>>>False  #因为含有0
```
#### 元素选择函数
```python
pair = [1,2]
from operator import getitem
getitem(pair,0)
>>>1
```
### 字符串
```python
exec('curry = lambda f: lambda x: lambda y: f(x,y)')
curry
>>>function
```
- 单引号等同于双引号，但字符串中有单引号时用双引号
- 多行分段的字符串用三个双引号
#### 基础操作
```python
city = 'Berkeley'
len(city)
>>> 8
city[3]
>>>'k'
#对于字符串这样操作，得到的仍然是字符串(只不过是一个字符)
#但对于列表，得到的是列表中的一个元素
‘here’ in "Where's Waldo?"
>>>True
#对于字符串，in可以查找连续的字符
```
### 字典
#### 基础操作
```python
numerals = {'I':1,'V':5,'X':10}
numerals
>>>{'I':1,'V':5,'X':10}
len(numerals)
>>>3
numerals['X']
>>>10
list(numerals)
>>>['I','V','X']
numerals.values()
>>>dict_values([1,5,10]) #这是一个序列
sum(numerals.values())
>>>16
list(numerals.values())
>>>[1,5,10]
#一个键不能对应多个值
{1:'first',1:'second'}
>>>{1:'second'}
#键不能是字典或者列表
```
#### filter expression
```python
{x*x:x for x in [1,2,3,4] if x>2}
>>>{9:3,16:4,25:5}
#重要
def index(keys,values,match):
	return {k: [v for v in values if match(k,v)] for k in keys}
	"""
	>>>index([7,9], range(30,50),lambda k,v:v%k==0)
	{7:[35,42,49], 9:[36,45]}
```
### 数据
#### data abstraction(数据抽象)
`例子`
当表示一个有理数：
需要一对数字来表示分子和分母；但不会具体算出来；
`constructor`:构造一个函数rational(n,d)，returns a rational number x
`selectors`:构造两个函数numer(x),denom(x)，returns the numerator and denominator of x
```python
def mul_rational(x,y):
	return rational(numer(x) * numer(y),
					denom(x) & denom(y))
#其中：rational是constructor; numer,denom是selectors
```
#### abstraction barriers(抽象屏障)
将程序的不同部分分开，使每个部分只需了解程序的其余部分。
允许你对程序的一部分进行更改，而对其他部分可以利用这些更改而不会产生不一致。
`对于创建有理数：`
- 有理数的操作使用了乘法加法这些函数
- 有理数的构成与实现使用了rational，numer和denom
- 这两部分**之间产生了屏障**
>这个屏障表示：对于有理数进行计算时的任何东西都应该只能使用这些函数，
>并且不应该使用不同层次的函数
### 树
#### 树的抽象表示
```python
#A tree has a root label and a list of branches
#constructor
def tree(label, branches=[]):
	for branch in branches:
		assert is_tree(branch)
	return [label] + list(branches)
#selectors
def label(tree):
	return tree[0]
def branches(tree):
	return tree[1:]

def is_tree(tree):
	if type(tree) != list or len(tree) < 1:
		return False
	for branch in branches(tree):
		if not is_tree(branch):
			return False
	return True
def is_leaf(tree):
	return not branches(tree)
```


