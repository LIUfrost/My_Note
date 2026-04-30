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
t = tree(1,[tree(5,[tree(7)]),tree(6)])
t
>>>[1,[5,7],[6]]
label(t)
>>> 1
branches(t)
>>>[[5,[7]],[6]]
```
#### 递推实现各种函数
```python
# 递推写斐波那契数列
def fib_tree(n):
	if n<=1:
		return tree(n)
	else:
		left,right = fib_tree(n-2),fib_tree(n-1)
		return tree(label(left) + label(right),[left,right])
# 叶子数量之和
def count_leaves(t):
	if is_leaf(t):
		return 1
	else:
		branch_counts = [count_leaves(b) for b in branches]
		return sum(branch_counts)#
#列表的一个特性
#如果对一个列表求和，你会得到一个包含所有列表元素的列表
sum([[1],[2,5]],[])  #最后空括号提供起始值
>>>[1,2,5]
# 返回一个包含所有leaf label的列表
def leaves(tree):
	if is_tree(tree):
		return [label(tree)]
	else:
		return sum([leaves(b) for b in branches(tree)])
# 返回一个和原来一样的tree，但leaf label的值加一
def increment_leaves(t):
	if is_leaf(t):
		return tree(label(t) + 1)
	else:
	bs = [increment_leaves(b) for b in branches(t)]
	return tree(label(t),bs)
# 返回一个和原来一样的tree，但每个label的值都加一
def increment(t):
	return tree(label(t)+ 1,[increment(b) for b in branches(t)])
# 打印一棵树
def print_tree(t,indent = 0):
	print(' ' * indent + str(label(t)))
	for b in branches(t):
		print_tree(b,indent + 1)
# 打印每个leaf的label，和从根到这个leaf的label之和
def print_sums(t,so_far):
	so_far = so_far + label(t)
	if is_leaf(t):
		print(so_far)
	else:
		for b in branches(t):
			print_sums(b,so_far)
# 给定一个total值，返回所有label和为total的路径
def count_paths(t,total):
	if label(t) = total:
		found = 1
	else:
		found = 0
	return found + sum([count_paths(b,total - lavel(t)) for b in branches(t)])
```
#### 树的类实现
```python
class Tree:
	def __init__(self,label,branches=[]):
		self.label = label
		for branch in branches:
			assert isinstance(branch,Tree)
		self.branches = list(branches)
def fib_tree(n):
	if n == 0 or n == 1:
		return Tree(n)
	else:
		left = fib_tree(n-2)
		rlght = fib_tree(n_1)
		fib_n = left.lable + right.label
		return Tree(fib_n,[left,right])
```
### 对象

对象的重要信息在于他的**属性**，使用**点表达式**来访问和运算。
点表达式除了可以绑定对象值的属性，还可以绑定到函数的属性。
这些统称为**方法**
**方法**是通过点表达式访问的任何东西，然后像函数一样调用
一类对象称之为**类**，**类**可以作为一级值参数传递给函数
#### string对象
```python
s = 'Hello'
s.upper()
>>>'HELLO'
s.swapcase()
>>>'hELLO'
# ASCII值
a = 'A'
ord(a)
>>>65
dex(ord(a))
>>>'0x41'
```
#### 对象内容可以改变
```python
suits = ['coin','string','myriad']
original_suits = suits
suits.pop()
>>>'myriad'
suits.remove('string')
suits
>>>['coin']
suits.append('cup')
suits.extend(['sword','club'])
suits
>>>['coin','cup','sword','club']
suits[2] = 'spade'
suits[0:2] = ['heart','diamond']
suits
>>>['heart','diamond','spade','club']
original_suits
>>>['heart','diamond','spade','club']  # 也一同改变了
```
对于不可变数据比如string和integer，a=b，b改变不影响a，本质是创建了一个新的object；
对于可变数据list1 = list2，list2改变会影响list1，英文本质上是同一个object
```python
numerals = {'I':1,'v':5}
numerals['V'] = 11  # 会改变数据
numerals['L'] = 50  # 增加数据L
numerals.pop('L')   # 弹出L
numerals.get('L')   # 不会有任何值，L已经被弹出
```
#### 身份运算符
```python
[10] == [10]
>>>True
a = [10]
b = [10]
a == b
>>>True
a is b
>>>False
a.extend([20,30])
a
>>>[10,20,30]
c = b
c is b
>>>True
```
#### 可变默认参数是危险的
```python
def f(s = [])
	s.append(5)
	return len(s)
f()
>>>1
f()
>>>2    # 函数默认参数在定义时已经创建，以后共用同一块内存，不会重新创建
```
#### 利用可变参数创建可变函数
```python
# 一开始有100块钱
withdraw = make_withdraw_list(100)
# 创建函数的父框架
withdraw(25)
>>>75
withdraw(15)
>>>60
# 将withdraw这个行为，和100这个数据绑定到一起了
def make_withdraw_list(balance):
	b = [balance]
	def withdraw(amount):
		if amount > b[0]:
			return 'Insufficient funds'
		b[0] = b[0] - amount
		return b[0]
	return withdraw
withdraw = make_withdraw_list(100)
withdraw(25)   # 闭包思想
```

### 元组
所有被逗号间隔的数据，而没被框起来的都视为元组
```python
(2,3,4,6)
>>>(2,3,4,6)
3,4,5,6
>>>(3,4,5,6)
()
>>>()
typle([3,4,5])
>>>(3,4,5)
(2,)
>>>(2,)  # 一个元素时后面要加逗号
(2,3) + (4,5)
>>>(2,3,4,5)
```
>元组不可变，所以可以作为字典的键值，但列表不可以
>一个不可变对象中包含了可变对象，那么这个不可变对象依然可以被改变
```python
s = ([1,2],3)
s[0][0] = 4
s
>>>([4,2],3)
```
### 迭代器
- 使用迭代器可以隐式的表示顺序数据
- 是编程语言中的常见接口，在python中，他们用作访问许多不同容器的元素的一种方式
- 容器可以提供一个迭代器，迭代器反过来以特定顺序访问容器的元素
**iter(iterable)**:返回一个迭代器
**next(iterator)**:返回指定迭代器的下一个元素
- 所有迭代器都是可变对象，当使用next的时候会到下一个值
#### 迭代器创建与使用
```python
s = [[1,2],2,3]
t = iter(s)
t
>>>list_iterator object
next(t)
>>>[1,2]
list(t)
>>>[2,3]   # 因为[1,2]这个元素已经遍历过了
next(t)
>>>StopIteration # 已经遍历完了
# 遍历字典
d = {'one':1,'two':2,'three':3}
k = iter(d.keys())# v = iter(d.values())  i = iter(d.items())
next(k)           # next(i)
>>>'one'          # ('one',1)
# iter(d.keys())等同于iter(d)
# 如果在创建迭代器之后，再更改了字典中的项的数目或结构，那么这个迭代器就无效了
# 如果只是更改了字典中的键的值，那依然存在
```
#### For语句在迭代器中的使用
```python
r = range(3,6)
list(r)
>>>[3,4,5]
ri = iter(r)
next(ri)
>>>3
for i in ri:
	print(i)
>>>4
>>>5    # 3已经被遍历过了
```
#### 内置迭代函数
```python
map(func,iterable)  # 返回一个迭代器，遍历iterable中的每个值并应用到f中
filter(func,iterable) # 如果func为真，则返回iterable的迭代器
zip(first_iter,second_iter) # 返回(x,y)对的迭代器
reversed(sequence) # 返回迭代器，以相反的顺序迭代sequence

list(iterable)  # 查看迭代器中所有值,并返回一个列表
tuple(iterable) # 将迭代器中所有值放到元组中
sorted(iterable) # 创建一个包含可迭代对象中所有元素的排序列表
```
>[惰性运算]：不会立刻计算数值，而是迭代到需要计算了才计算
例子
```python
bcd = ['b','c','d']
[x.upper() for x in bcd]
>>>['B','C','D']
map(lambda x: x.upper(), bcd)
>>><map object>
m = map(lambda x:x.upper(),bcd)
next(m)
>>>'B'
def double(x)：
	print(x,'=>',2*x)
	return 2*x
m = map(double,range(3,7))
f = lambda y:y >= 10
t = filter(f,m)
next(t)
>>>3 => 6
>>>4 => 8
>>>5 => 10
>>>10
t = [1,2,3,2,1]
reversed(t) == t
>>> False
list(reversed(t))
>>>[1,2,3,2,1]
list(reversed(t)) == t
>>>True
```
Other function
```python
list(zip([1,2],[3,4,5]))
>>>[(1,3),(2,4)]
list(zip([1,2],[3,4,5],[6,7]))
>>>[(1,3,6),(2,4,7)]
# 判断是否是回文
def palindrome(s):
	return list(s) == list(reversed(s))
    # 或
    return all([a == b for a,b in zip(s,reversed(s))])	
```
#### 什么时候使用迭代器
- 数据类型更改后，使用迭代器不用重写代码
- 多个方法调用相同迭代器时不会返回同一个值，因为迭代下标一直在变化
### 生成器
**生成器**是一种特殊类型的迭代器，从**生成函数**返回
生成器的特征：利用**yield**返回值
生成器对象的作用是帮助迭代调用的函数产生所有值
生成器函数是一个只产生值，但不返回值的函数
生成器是在调动生成器函数的自动生成的迭代器
#### 基本操作
```python
def plus_minus(x):
	yield x，
	yield -x
t = plus_minus(3)
next(t)
>>>3
next(t)
>>>-3
t
>>><generator object>
def evens(start,end):
	even = start + (start % 2)
	while even < end:
		yield even
		even += 2
t = evens(2,10)
next(t)
>>>2
next(t)
>>>4   # 直到8
```
每次调用生成器函数时，他会执行到yield，并记住当前所处位置
```python
# yield from语句
def a_then_b(a,b):
	for x in a :
		yield x
	for x in b:
		yield x
# 等价于
def a_then_b(a,b):
	yield from a
	yield from b
# 
def countdown(k):
	if k>0:
		yield k
		for x in countdown(k - 1):
			yield x
		# 等价于
		yield from countdown(k - 1)
# 返回一个单词的所有前缀
def prefixes(s):
	if s:
		yield from prefixes(s[:-1])
		yield s
list(prefixes('both'))
>>>['b','bo','bot','both']
```
#### 例子
```python
# 将一个数分割成不大于另一个数
def list_partitions(n,m):
	if n<0 or m == 0:
		return []
	else:
		exact_match = []
		if n == m:
			exact_match = [[m]]
		with_m = [p+[m] for p in list_partitions(n-m,m)]
		without_m = list_partitions(n,m-1)
		return exact_match + with_m + without_m
# 等价于
def list_partitions(n,m):
	if n > 0 and m > 0:
		if n == m:
			yield str(m)
		for p in partitions(n-m,m):
			yield p + '+' + str(m)	
		yield from partitions(n,m-1)
```
### 定义类
```python
class Account:
	def _init_(self,account_holder):   # 在创建对象时自动调用的函数
		self.balance = 0
		self.holder = account_holder
	# a = Account('John')
	def deposit(self,amount):
		self.balance = self.balance + amount
		return self.balance
	# a.deposit(10) => 余额增加10
	def withdraw(self,amount):
		if amount > self.balannce:
			return 'Insufficient funds'
		self.balance = self.balance - amount
		return self.balance
```
在Python中，一个新的属性是可以随时添加的
属性的对象也可以是对象
由类创建的对象是可变异的，即可以用is来辨别两个对象是否在同一个地址
```python
b = Account('Ada')
b.balance
>>>0
b.balance = 20
a.backup = b # 添加了一个新属性，并且其对象b也是一个对象
a.backup.balance
>>>20
```
### 类属性
#### 基本属性
如果所有类的实例的某个属性都一样，那可以直接把它定义为**类属性**
**类属性**不是实例的一部分，而是类的一部分
```python
class Account:
	interest = 0.02  # A class attribute
	def _init_(self,account_holder):
		self.balance = 0
		self.holder = account_holder
tom_account = Account('Tom')
tom_accouny.interest
>>>0.02
```
对于一个点表达式`<expression>.<name>`
- 首先找expression的位置
- 再找该实例的属性name
- 如果找不到就在类属性里面找
```python
# getattr找某个属性对应的值
# hasattr找是否存在这个属性
tom_account.balance
getattr(tom_account,'balance')
>>>10
hasattr(tom_account,'deposit')
>>>True
```
#### 更改属性值
```python
tom_account.interest
>>>0.02
Account.interest = 0.04
tom_account.interest
>>>0.04    # 类属性不会锁定，可以更改
tom_account.interest = 0.07  # 在tom对象中创建了一个属性interest
tom_account.interest
>>>0.07
Account.interest = 0.05
tom_account.interest
>>>0.05
```
>此时，python不会去实例属性或类属性中寻找是否有interest这个属性
>而是直接添加或修改这个属性的值
#### 绑定方法
绑定方法是**函数**，也是**类属性**，其中self参数已经填充为类的一个实例
```python
type(Account.deposit)
>>>class 'function'
type(tom_account.deposit)
>>>class 'method'
Account.deposit(tom_account,1001)
>>>1011
tom_account.deposit(1007)
>>>2018
```
### 继承
**继承**是将**多个类练习联系在一起**的一种方法，并非每个类都孤立存在
```python
class <name>(<base class>):
	<suite>
```
- 通过这个类语句创造的子类和其基类共享所有属性；
- 子类可能会覆盖某些继承的属性，以修改其行为，但未更改的任何内容将保持不变；
```python
# 创建一个支票账户类
# 他具有更低的利息(interest)
# Deposit操作相同
# 取钱时会产生$1的利息
class CheckingAccount(Account):
	withdraw_fee = 1
	interest = 0.01
	def withdraw(self,amount):
		return Account.withdraw(self,amount + self.withdraw_fee)
```
>[行为解析]——基类上的属性不会复制到子类里面。
>为了在查找一个属性：
>先在当前定义的类里面查找，如果有，返回
>如果没有，则在其基类里面找
#### 多重继承
```python
class SavingsAccount(Account):
	deposit_fee = 2
	def deposit(self,amount):
		return Account.deposit(self,amount - self.deposit_fee)
class AsSeenOnTVAccount(CheckingAccount,SavingsAccount):
	def _init_(self,account_holder):
		self.holder = account_holder
		self.balance = 1  # 注册时赠送1美元
```
### 字符串表示
#### repr字符串
字符串分为两部分：**str**对人类可读，**repr**对python解释器可读
```python
from fractions import Fraction
half = Fraction(1,2)
repr(half)
>>>'Fraction(1,2)'
str(half)
>>>'1/2'
print(half)
>>>1/2    # print通常打印的是str
eval(repr(half))
>>>Fraction(1,2)
s = "hello world"
s
>>>'hello world'
print(repr(s))
>>>'hello world'
print(s)
>>>hello world
print(str(s))
>>>hello world
str(s)
>>>'hello world'
repr(s)
>>>"'hello world'"
eval(repr(s))
>>>'hello world'
eval(s)
>>>error
```
repr和eval是互逆的，repr将字符串加工为python解释器可以理解的字面量，所以在原先的基础上再加了一层引号，而eval的执行对象是字面量，所以单纯eval(s)时会报错
#### F—Strings
```python
'pi starts with' + str(pi) + '...'
# 等价于
f'pi starts with {pi}...'
```
#### 多态函数
多态函数是一种适用于许多数据类型的函数
str 和 repr都是多态的，他们可以提供给任意函数
```python
# repr衍生出一个零参数方法_repr_
half.__repr__()
>>>'Fraction(1,2)'
# str衍生出一个零参数方法_str_
half.__str__()
>>>'1/2'
def repr(x):
	return type(x).__repr__(x)  
	# type会返回x的类，_repr_是类属性，所以后面参数还要填
	
```
使用接口
```python
class Ratio:
	def __init__(self,n,d):
		self.numer = n
		self.denom = d
	def __reor__(self):
		return 'Ratio({0},{1})'.format(self.numer,self.denom)
	def __str__(self):
		return '{0}/{1}'.format(self.numer,self.denom)
```
#### 特殊方法
以两个下划线作为开头和结尾
```python
__init__  # 在对象构建时自动调用
__repr__  # 调用以产生表示对象的字符串
__add__   # 将一个对象添加到另一个对象中
__radd__  # a.__radd__(b) = b.__add__(a)
__bool__  # 将对象转换为True、False
__float__ # 将对象转换为实数
zero, one = 0,1
bool(zero),bool(one)
>>>(False,True)
one.__add__(two)
>>>3
zero.__bool__(),one.__bool__()
>>>(False,True)
# 在Ratio类中定义加法
	def __add__(self,other):
		if isinstance(other,int):
			n = self.numer + self.denom * other
			d = self.denom
		elif isinstance(other,Ratio):
			n = self.numer * other.denom + self.denom * other.numer
			d = self.denom * other.denom
		elif ininstance(other,float):
			return float(self) + other
		g = gcd(n,d)
		return Ratio(n//g,d//g)
	__radd__ = __add__     # 加法可交换
	def __float__(self):
		return self.numer/self.denom
def gcd(n,d):
	while n!=d:
		n,d = min(n,d),abs(n-d)
	return n
```
### 链表
#### 结构定义
```python
class Link:
	empty = ()
	def __init__(self,first,rest = empty):
		assert rest is Link.empty or isinstance(rest,Link)
		self.first = first
		self.rest = rest
```
#### 操作
```python
def range_link(start,end):
	"""
	range_link(3,6)
	>>>Link(3,Link(4,Link(5)))
	"""
	if start >= end:
		return Link.empty
	else:
		return Link(start,range_link(start + 1,end))
def map_link(f,s):
	if s is Link.empty:
		return s
	else:
		return Link(f(s.first),map_link(f,s.rest))
def filter_link(f,s):
	if s is Link.empty:
		return s
	filtered_rest = filter_link(f,s.rest)
	if f(s.first):
		return Link(s.first,filtered_rest)
	else:
		return filtered_rest
```
#### 链表修改
```python
s = Link(1,Link(2,Link(3)))
s.first = 5
t = s.rest
t.rest = s
s.rest.rest.rest.rest.rest.first
>>>2   # 5->2->5->2->5->2...
# add操作
def add(s,v):
	if s.first > v:
		s.first,s.srest = v,Link(s.first,s.rest)# 不能直接s，地址！
	elif s.first < v and empty(s.rest):
		s.rest = Link(v)
	elif s.first < v:
		add(s.rest,v)
	return s
# 剪枝操作
def prune(t,n):
	t.branches = [b for b in t.branches if b.label != n]
	for b in t.branches:
		prune(b,n)
```
### 效率
#### 计算效率
```python
计算函数调用次数的装饰器
def count(f):
	def counted(n):
		counted.call_count += 1 # 函数也是对象，也可以具有属性
		return f(n)
	counted.call_count = 0
	return counted
# fib为一个斐波那契数列的求值函数
fib = count(fib)
fib(5)
>>>5
fib.call_count
>>>15
```
#### 记忆化
**记忆化**是加速运行时间提升效率的极好方式
- 通过记忆之前计算过的结果来加速
```python
def memo(f):
	cache = {}
	def memoized(n):
		if n not in cache:
			cache[n] = f(n)
		return cache[n]
	return memoized
# 同样用fib，展示加速效果
fib = count(fib)
counted_fib = fib
fib = memo(fib)
fib = count(fib)
fib(30)
>>>832040
fib.call_count  
# 这个统计的是对memory调用的次数，无论是否进没进缓存，都要调用这个函数
>>>59
counted_fib.call_count
# 这个统计的是对memory内部的fib的调用次数，memory装饰后，fib只会0-30调用
>>>31
```
#### 计算空间
```python
def count_frames(f):
	def counted(n):
		counted.open_count += 1
		if counted.open_count > counted.max_count:
			counted.max_count = counted.open_count
		result = f(n)
		counted.open_count -= 1
		return result
	counted.open_count = 0
	counted.max_count = 0
	return counted
```
### 模块化设计
程序可以被分解为小的相当独立的部分
**通用的设计原则**：将处理不同关注点的程序部分隔离开来，这样不必一次考虑程序的所有要求
```python
# 根据similarity函数来排序并返回前k个值组成的列表
def similar(self,k,similarity):
	others = list(Restaurant.all)
	others.remove(self)
	return sorted(others,key = lambda r:-similarity(self,r))[:k]
```
### 数据实例
#### list
```python
s = [2,3]
t = [5,6]
# append ->将一个元素加入列表中
s.append(t)
t = 0
s->[2,3,[5,6]]  t->0
# extend ->将一个列表中的所有元素加入到另一个列表中
s.extend(t)
t[1] = 0
s->[2,3,[5,6]] t->[5,0]
# addition & slicing
a = s + [t]
b = a[1:]
a[1] = 9
b[1][1] = 0
s->[2,3]  t->[5,0]
a->[2,9,[5,0]]  b->[3,[5,0]] # 影响了原本的t
# list
t = list(s)
s[1] = 0
s->[2,0]   t->[2,3] # list将s复制了一遍并创建了个列表，所以和s地址不同
# slice assignment
s[0:0] = t # 相当于在0个位置前插入了t
s[3:] = t # 将三号位改为5，并在后面插入6
t[1] = 0
s->[5,6,2,5,6]  t->[5,0]
# pop->移除最后一个元素
t = s.pop()
s->[2] t->3
# remove->移除第一个和参数相同的元素
t.extend(t)
t.remove(5)
s->[2,3]  t->[6,5,6]
# slice assignment
s[:1] = []
t[0:2] = []
s->[3]  t->[]
# List in List
t = [1,2,3]
t[1:3] = [t]
t.extend(t)
t->[1,X,1,X] # 这里的X相当于t的头指针
# List in List
t = [[1,2],[3,4]]
t[0].append(t[1:2])
t->[[1,2,[[3,4]]],[3,4]]  # 注意3，4被两个中括号包裹，因为slice添加了
```
#### Iterables & Iterators
```python
# 返回最小绝对值的索引列表
def min_abs_indices(s):
	min_abs = min(map(abs,s))
	return [i for i in range(len(s)) if abs(s[i]) == min_abs]
	# 或者
	f = lambda i:avs(s[i]) == min_abs
	return list(filter(f,range(len(s))))
# 返回相邻两个数之和的最大值
def largest_adj_sum(s):
	return max([s[i] + s[i+1] for i in range(len(s) - 1)])
	# 或者
	return max([a+b for a,b in zip(s[:-1],s[1:])])
# 将每个数字d映射到以d为结尾的S中的元素列表，结果是一个字典
def digit_dict(s):
	return {d:[x for x in s if x % 10 == d] for d in range(10) if any([x % 10 == d for x in s])}
	# 或拆分一下
	last_digits = [x % 10 for x in s]
	return {d: [x for x in s if x%10 == d] for d in range(10) if d in last_digits}
# 是否所有在列表中的元素都有一个元素和它相同
def all_have_an_equal(s):
	return all([s[i] in s[:i] + s[i+1:] for i in range(len(s))])
	# 或者
	return all([sum([1 for y in s if y == x]) > 1 for x in s])
	# 或者
	return min([s.count(x) for x in s])>1
```
#### link list
```python
# 检查是否有序
def ordered(s,key = lambda x:x):
	if s is Link.empty or s.rest is Link.empty:
		return True
	elif key(s.first) > key(s.rest.first):
		return False
	else:
		return ordered(s.rest)
# 将两个链表有序合并
def merge(s,t):
	if s is Link.empty:
		return t
	elif t is Link.empty:
		return s
	elif s.first <= t.first:
		return Link(s.first,merge(s.rest,t))
	else:
		return Link(t.first,merge(t.rest,s))
# 不使用link使得两个链表有序合并
def merge(s,t):
	if s is Link.empty:
		return t
	elif t is Link.empty:
		return s
	elif s.first <= t.first:
		s.rest = merge_in_place(s.rest,t)
	else:
		t.rest = merge_in_place(t.rest,s)
```
### 异常
#### 自定义异常
```python
def double(x):
	if isinstance(x,str):
		raise TypeError('double takes only numbers')
	return 2*x
# 错误类型
hello
>>>NameError
()['hello']
>>>KeyError
def f():f()
f()
>>>RecursionError
```
#### Try语句
```python
try:
	x = 1/0
expect ZeroDivisionError as e:
	print('handling a',type(e))
	x = 0
>>>handling a <class 'ZeroDivisionError'>
x
>>>0
# 实现reduce函数
"""
reduce(mul,[2,4,8],1) == mul(mul(mul(1,2),4),8)
                                                          """
from operator import add,mul,truediv
def reduce(f,s,initial):
	for x in s:
		initial = f(initial,x)
	return initial
def divide_all(n,ds):
	try:
		return reduce(truediv,ds,n)
	except ZeroDivisionError:
		return float('inf')
divide_all(1024,[2,4,8])
>>>16.0
```
### 数据库
**SQL**语言：声明式语言
```SQL
create table cities as
	select 38 as latitude,122 as longitude,"Berkeley" as name union
	select 42,71,"Cambridge" union
	select 45,93,"Minneapolits";
select "west coast" as region,name from cities where longitude >= 115 union
select "other",name from cities where longitude < 115;
```
- select:创建了一个新的表，可以从头开始也可以投影表来创建
- create table：为表赋予了全局名称
- 两行数据并不需要要求有同样的列名称，但最终命名时仅使用第一个select语句中的列名称
- select语句不保证添加的行具有原来的顺序
#### 投影表
**select语句**可以将现有的表投影到新表中
```SQL
select [columns] from [table] where [condition] order by [order]
select word, one+two+four+eight as value from ints;
// *是缩写,相当于所有
select * from parents
// 算数表达式
select chair,single + 2 * couple as total from lift;
```