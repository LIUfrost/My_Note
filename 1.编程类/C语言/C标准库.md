---
data: 2025-12-18T21:26:00
tags:
  - C
---
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
