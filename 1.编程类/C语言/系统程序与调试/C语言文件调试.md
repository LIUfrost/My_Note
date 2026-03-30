---
tags:
  - C
data: 2026-3-12
---

## gcc
```C
//产生调试文件
gcc -g -o xxxx xxxx.c
//进入gdb环境
gdb
file xxxx
//设置断点
b xxx.c : 15
//运行
run 
//查看变量名
print xxx(变量名)
p xxx(变量名)
//查看变量类型
ptype xxx
//查看端点信息
info b
//删除断点
delete 断点编号
//更改参数值
print xxx(变量名) = xxx
//函数调用堆栈
bt
//单步执行
next  &n
step   &s
//继续执行到下一个断电
continue &c 
//查看文件代码
list &l
//退出gdb
quit &q
```
## 阴影内存(shadow memory)
**阴影内存**是AddressSanitizer用来追踪程序内存状态的一种特殊内存区域。

==工作原理==
- 程序每分配8字节的内存，addressSanitizer就用1字节的“阴影内存”来记录这8字节的状态
==阴影字节的含义==
- 00 -> 可寻址(这8个字节都可以用)
- 01-07 -> 部分可寻址(只有前N个字节可用)
- fa -> 堆左红区(不可访问，越界保护)
- f3 -> 堆右红区(不可访问)
- fd -> 已释放的内存
==查找出错点的阴影内存==
- 在AddressSanitizer中，==\=>==箭头指向的就是最关键的内存行
## C语言常见报错
### 一个例子
#### case1
Line 29: ================================================================= \=\=23\=\=ERROR: ==AddressSanitizer==: ==heap-buffer-overflow== on address ==0x5020000000f3== at pc 0x55559eb22f48 bp 0x7ffff104bdb0 sp 0x7ffff104bda0 ==WRITE of size 1 at 0x5020000000f3== thread T0 #0 0x55559eb22f47 in gp solution.c:29 #1 0x55559eb2268c in gp solution.c:29 #2 0x55559eb2268c in gp solution.c:29 #3 0x55559eb22639 in gp solution.c:29 #4 0x55559eb22639 in gp solution.c:29 #5 0x55559eb21f8d in generateParenthesis solution.c:29 #6 0x55559eb21f8d in main solution.c:29 #7 0x7f81780d11c9 (/lib/x86_64-linux-gnu/libc.so.6+0x2a1c9) (BuildId: 8e9fd827446c24067541ac5390e6f527fb5947bb) #8 0x7f81780d128a in \_\_libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2a28a) (BuildId: 8e9fd827446c24067541ac5390e6f527fb5947bb) #9 0x55559eb22484 in \_start (solution+0x1f484) (BuildId: 9b71bfa895a2c46b4d7b1a4dc365c89d26667e93) ==0x5020000000f3 is located 0 bytes after 3-byte region \=\[0x5020000000f0,0x5020000000f3)==allocated by thread T0 here: #0 0x7f8178bfe9c7 in malloc ../../../../src/libsanitizer/asan/asan_malloc_linux.cpp:69 #1 0x55559eb2288f in gp solution.c:29 #2 0x55559eb2268c in gp solution.c:29 #3 0x55559eb2268c in gp solution.c:29 #4 0x55559eb22639 in gp solution.c:29 #5 0x55559eb22639 in gp solution.c:29 #6 0x55559eb21f8d in generateParenthesis solution.c:29 #7 0x55559eb21f8d in main solution.c:29 #8 0x7f81780d11c9 (/lib/x86_64-linux-gnu/libc.so.6+0x2a1c9) (BuildId: 8e9fd827446c24067541ac5390e6f527fb5947bb) #9 0x7f81780d128a in \_\_libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2a28a) (BuildId: 8e9fd827446c24067541ac5390e6f527fb5947bb) #10 0x55559eb22484 in \_start (solution+0x1f484) (BuildId: 9b71bfa895a2c46b4d7b1a4dc365c89d26667e93) ==SUMMARY==: AddressSanitizer: heap-buffer-overflow solution.c:29 in gp Shadow bytes around the buggy address: 0x501ffffffe00: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 0x501ffffffe80: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 0x501fffffff00: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 0x501fffffff80: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 0x502000000000: fa fa 00 fa fa fa 04 fa fa fa 00 fa fa fa 03 fa ==\=>0x502000000080: fa fa 00 fa fa fa 02 fa fa fa 00 fa fa fa\[03]fa== 0x502000000100: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa 0x502000000180: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa 0x502000000200: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa 0x502000000280: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa 0x502000000300: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa Shadow byte legend (one shadow byte represents 8 application bytes): Addressable: 00 Partially addressable: 01 02 03 04 05 06 07 Heap left redzone: fa Freed heap region: fd Stack left redzone: f1 Stack mid redzone: f2 Stack right redzone: f3 Stack after return: f5 Stack use after scope: f8 Global redzone: f9 Global init order: f6 Poisoned by user: f7 Container overflow: fc Array cookie: ac Intra object redzone: bb ASan internal: fe Left alloca redzone: ca Right alloca redzone: cb \=\=23\=\=ABORTING
>`1`.AddressSanitizer:内存地址检测器
>`2`.heap-buffer-overflow:堆缓冲区溢出
>`3`.\[0x5020000000f0, 0x5020000000f3\):内存区域->三个字节
>`4`.问题：试图在`0x5020000000f3`这个地址写入1个字节，但这个地址正好是分配区域的**末尾之后**
>`5`.0x502000000080: fa fa 00 fa fa fa 02 fa fa fa 00 fa fa fa\[03]fa: ==\[03]==表示这块内存是**部分可寻址**的，只有3个字节可以书写;==fa==表示堆左红区(**不可访问**);==03==正好对应**那三个可写的字节**
>`6`.WRITE of size 1 at 0x5020000000f3: 代表**试图写入1个字节**

#### 报错信息的优先级排序

- ==1.看错误类型==(最开头一行)
	\=\=23\=\=ERROR: AddressSanitizer: heap-buffer-overflow
	可知是堆缓冲区溢出
- ==2.看操作的类型和位置==(紧挨着)
	WRITE of size 1 at 0x5020000000f3
	可知是写入1字节时出错
- ==3.看调用栈==(#0,#1,#2...)
	#0 0x55559eb22f47 in gp solution.c:29
	#1 0x55559eb2268c in gp solution.c:29
	可知是solution.c的第29行出错，是gp函数
- ==4.看内存分配信息==
	0x5020000000f3 is located 0 bytes after 3-byte region
	\[0x5020000000f0,0x5020000000f3)
	可知是分配了3字节，写到了这3个字节的后面
- ==5.看阴影内存==
	主要是确认内存布局和边界
#### 链接错误
- ==1.未定义引用==
```C
//声明了函数但没定义
void myFunc();
int main(){
	myFunc();  // 链接时找不到实现
}
```
	undefined reference to 'myFunc'
	collect2: error: ld returned 1 exit status
#### 运行时错误
- ==1.段错误==
```c
int *p = NULL;
*p = 10;  // 访问空指针
```
	Segmentation fault (core dumped)
- ==2.AddressSanitizer类型错误==
```c
// heap-buffer-overflow(堆缓冲区溢出)
char *p = malloc(5);
p[5] = 'a';  // 越界

// stack-buffer-overflow(栈缓冲区溢出)
char arr[5];
arr[5] = 'a';  // 栈数组越界

//use-after-free(释放后使用)
char *p = malloc(10);
free(p);
*p = 'a';  // 使用已释放的内存

//double-free(重复释放)
char *p = malloc(10);
free(p);
free(p);  // 第二次释放

//memory leak(内存泄漏)
void func() {
    malloc(100);  // 分配了但没释放
}
```