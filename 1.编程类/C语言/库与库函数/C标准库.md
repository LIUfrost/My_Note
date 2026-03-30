---
data: 2025-12-18T21:26:00
tags:
  - C
---
## **长度修饰符**
### **整数类型**
- `%d` - 十进制有符号整数
- `%i` - 十进制有符号整数（与%d基本相同）
- `%u` - 十进制无符号整数
- `%o` - 八进制无符号整数
- `%x` - 十六进制无符号整数（小写字母a-f）
- `%X` - 十六进制无符号整数（大写字母A-F）
### **short 类型**
- `%hd` - short int（十进制）
- `%hu` - unsigned short int（十进制）
- `%ho` - short int（八进制）
- `%hx` - short int（十六进制）
### **long 类型**
- `%ld` - long int（十进制）
- `%li` - long int（十进制）
- `%lu` - unsigned long int（十进制）
- `%lo` - long int（八进制）
- `%lx` - long int（十六进制）
- `%lX` - long int（十六进制大写）
### **long long 类型（C99）**
- `%lld` - long long int（十进制）
- `%lli` - long long int（十进制）
- `%llu` - unsigned long long int（十进制）
- `%llo` - long long int（八进制）
- `%llx` - long long int（十六进制）
### **size_t 类型**
- `%zd` - size_t（十进制）
- `%zu` - size_t（无符号十进制）
### **ptrdiff_t 类型**
- `%td` - ptrdiff_t（十进制）
### **intmax_t 类型**
- `%jd` - intmax_t（十进制）
- `%ju` - uintmax_t（十进制）
```C
int num = 123;
printf("%d", num);      // 输出：123
printf("%5d", num);     // 输出：  123（宽度5，右对齐）
printf("%-5d", num);    // 输出：123  （宽度5，左对齐）
printf("%05d", num);    // 输出：00123（前补零）
printf("%x", 255);      // 输出：ff
printf("%X", 255);      // 输出：FF
printf("%.2f", 3.1415); // 输出：3.14（保留两位小数）
```
## 位运算
### &(按位与)
==**如果两个操作数对应的二进制位数都为1，该位运算结果为1，否则为0**==
- 取一个数中的某些特定位：
	只需构建另一个二进制数，在特定位为1，其余位为0，按位与即可
### |(按位或)
==**如果一旦有一个1，位运算结果为1，否则为0**==
- 用来运算复杂的布尔值：
	如需要两个数至少有一个>0：bool sign = (a>0) | (b>0)
### ^(按位异或)
==**如果相同则位运算为1，否则为0**==
- 用来运算复杂的布尔值:
	如需要两个数同号：bool sign = !((a>0) ^ (b>0))
### >>(按位右移)
==将>>左边的操作数的各个二进制位右移若干位==
- 右移n位相当于除以2^n
### <<(按位左移)
==**将<<左边的操作数的各个二进制位左移移若干位**==
- 相当于乘2^n
## <assert.h>
- `assert(expression)`
	- 用于测试表达式是否为真
	- 如果expression为假（结果为0）,assert会输出错误信息并终止程序
	- 错误信息包括：触发断言的表达式，源文件名，行号
	- 可以通过定义NDEBUG来禁用所有的assert断言
		- 例如在开头写`#define NDEBUG`,则后续assert语句会被处理为空语句
## <ctype.h>
#### 测试函数（返回布尔值）
- `int isainum(int c)`:检查所传的字符是否是字母或数字
- `int isalpha(int c)`:该函数检查所传的字符时候是字母
- `int iscntrl(int c)`:检查所传的字符是否是控制字符
- `int isdigit(int c)`:检查所上传字符是否是十进制数字
- `int islower(int c)`:检查所传字符是否是小写字母
- `int isprint(int c)`:检查所传字符是否是可打印的
- `int ispunct(int c)`:检查所传字符是否是标点符号字符
- `int isspace(int c)`:检查所传字符是否是空白字符
- `int isupper(int c)`:检查所传字符是否是大写字母
- `int isxdigit(int c)`:检查所传字符是否是十六进制数字
#### 转换函数（返回无符号字符）
- `int tolower(int c)`:将大写字母转化为小写字母
- `int toupper(int c)`:将小写字母转化为大写字母
```c
int c = getchar();
char c_max = (char)toupper(c);
```

## <limits.h>
#### 库宏
- `CHAR_MIN(MAX)`:char类型的最小值/最大值
- `SCHAR_MIN(MAX)`:sighed char类型的最小值/最大值（-128～127）
- `UCHAR_MAX`:unsighed char类型的最大值（255）
- `(U)INT_MIN(MAX)`:int类型的最大值/最小值
- `(U)LONG_MIN(MAX)`:long类型的最大值/最小值
## <local.h>
#### 库宏
- `LC_ALL`:用于设置和查询所有本地化类别
- `LC_COLLATE`:用于设置和查询字符串比较的本地化信息 
- `LC_MONETARY`:用于设置和查询货币格式的本地化信息
- `LC_TIME`:用于设置和查询时间格式的本地化信息
- `LC_CTYPE`:用于设置和查询字符处理的本地化信息
#### 库函数
- `char *setlocal(int category,const char *local)`:设置或读取地域化信息
- `struct lconv *localconv(void)`:设置或读取地域化信息
- `local_t newlocale(int category_mask,const char* locale,locale_t base)`创建一个新的本地化对象
- `freelocale(locale_t locale)`:释放一个本地化对象
- `locale_t uselocale(locale_t newloc)`:设置或查询线程的本地化对象
#### 实例
*设置和查询本地化信息*
```c
#include <stdio.h>
#include <locale.h>

int main() {
    // 设置本地化信息为用户环境变量中的默认设置
    setlocale(LC_ALL, "");

    // 获取和打印当前的本地化信息
    printf("Current locale for LC_ALL: %s\n", setlocale(LC_ALL, NULL));
    printf("Current locale for LC_TIME: %s\n", setlocale(LC_TIME, NULL));
    printf("Current locale for LC_NUMERIC: %s\n", setlocale(LC_NUMERIC, NULL));

    return 0;
}
```
*获取数字和货币信息*
```c
#include <stdio.h>
#include <locale.h>

int main() {
    // 设置本地化信息为用户环境变量中的默认设置
    setlocale(LC_ALL, "");

    // 获取本地化的数字和货币格式信息
    struct lconv *lc = localeconv();

    // 打印数字和货币格式信息
    printf("Decimal point character: %s\n", lc->decimal_point);
    printf("Thousands separator: %s\n", lc->thousands_sep);
    printf("Currency symbol: %s\n", lc->currency_symbol);

    return 0;
}
```
*使用I定义本地化对象*
```c
#include <stdio.h>
#include <locale.h>
#include <xlocale.h>

int main() {
    // 创建一个新的本地化对象，使用 "en_US.UTF-8" 区域设置
    locale_t newloc = newlocale(LC_ALL_MASK, "en_US.UTF-8", (locale_t)0);

    // 将当前线程的本地化对象设置为新的本地化对象
    locale_t oldloc = uselocale(newloc);

    // 获取和打印当前线程的本地化信息
    printf("Current locale for LC_NUMERIC: %s\n", setlocale(LC_NUMERIC, NULL));

    // 释放新的本地化对象
    uselocale(oldloc);
    freelocale(newloc);

    return 0;
}
```
