#### feek
`fseek`(FILE \*fp, int  offset, whence)：用于文件读写时光标移动
>[头文件]：stdio.h
>[whence]
>SEEK_SET：将指针移到文件开头
>SEEK_END：将指针移到文件结尾
>SEEK_CUR：将指针移到当前位置
### 标准工具函数(stdlib.h)
#### qsort
`qsort`(void \*base, size_t nmemb, size_t size, int (\*compar)(const void* ,const void \*))
>[头文件]：stdlib.h
>[参数说明]：base:要指向排序数组的指针；nmemb：数组中元素的个数；
>size:每个元素的大小(以字节为单位)；compar:比较函数的指针
>比较函数通常返回1，0，-1
```C
// 字符串比较（按字典序）
int cmp_string(const void *a, const void *b) {
    return strcmp(*(char**)a, *(char**)b);
}
// 结构体比较
typedef struct { int id; int score; } Student;
int cmp_student(const void *a, const void *b) {
    Student *s1 = (Student*)a;
    Student *s2 = (Student*)b;
    if (s1->score != s2->score)
        return s2->score - s1->score;  // 分数降序
    return s1->id - s2->id;            // 分数相同时id升序
}
```
#### bsearch(二分查找)
`void *bsearch(const void *key, const void *base, size_t nmemb,size_t size, int (*compar)(const void *, const void *));`
>[参数]：
>- `key`: 指向要查找元素的指针- `base`: 数组首地址（必须已排序）
>- `nmemb`: 元素个数- `size`: 每个元素大小
>- `compar`: 比较函数（同qsort）
```C
int arr[] = {1,3,5,7,9};
int key = 5;
int *p = bsearch(&key, arr, 5, sizeof(int), cmp_int);
if (p) printf("找到: %d\n", *p);
```
### 字符串处理函数(string.h)
#### strtok(字符串分割)
`strtok`char \*strtok(char \*str, const char \*delim)
>[头文件]：string.h
>[参数说明]：delim:遇到这里面的任何一个字符都会被分割；
>[过程]：**第一次调用**：传入要分割的字符串
>**后续调用**：传入 `NULL`，继续从上次的位置分割
>**返回值**：返回下一个标记的指针，如果没有更多标记则返回 `NULL`
>[工作原理]：遇到delim中的字符，就会把他替换为/0
>[注意事项]：会修改原来字符串(替换/0)； 不能交替处理两个字符串
>[多线程情况下]：使用strtok_r
```C
//示例
int main() {
    char str[] = "Hello,world!This is C programming";
    char *token;
    
    // 第一次调用，传入字符串
    token = strtok(str, " ,.!");
    
    while (token != NULL) {
        printf("%s\n", token);
        // 后续调用，传入 NULL
        token = strtok(NULL, " ,.!");
    }
    
    return 0;
}
int main() {
    char str[] = "apple,banana,orange";
    char *saveptr;  // 用于保存状态
    char *token;
    
    token = strtok_r(str, ",", &saveptr);
    while (token) {
        printf("%s\n", token);
        token = strtok_r(NULL, ",", &saveptr);
    }
    
    return 0;
}
```
#### sprintf(字符串加长)
`int sprintf(char *str, const char *format, ...);`
```C
#include <stdio.h>
int main() {
    char buffer[100];
    int a = 10;
    float b = 3.14;
    char c[] = "hello";
    
    // 格式化到字符串
    sprintf(buffer, "a=%d, b=%.2f, c=%s", a, b, c);
    
    printf("%s\n", buffer);  // 输出：a=10, b=3.14, c=hello
    
    return 0;
}
```
#### strncpy(带长度字符串复制)
`strncpy`char \*strncpy(char \*dest, const char \*src, size_t n);
>[头文件]：string.h
>[注意事项]：如果 src 长度 < n，会用 `\0` 填充剩余位置
#### strchr(查找字符首次出现)
`char *strchr(const char *str, int c);`
>[参数]：str：源字符串，c：要查找的字符(int类型，实际传入char)
>[注意事项]：**返回值：** 指向首次出现位置的指针，未找到返回NULL
>char \*p = strchr("hello world", 'o');  // p指向 "o world"
#### strrchr(查找字符最后一次出现)
`char *strrchr(const char *str, int c);`
>返回最后一次出现位置的指针
#### strstr(查找子串)
`char *strstr(const char *haystack, const char *needle);`
>[参数]：haystack:源字符串，needle：要查找的子串
>**返回值：** 指向子串首次出现位置的指针
>char \*p = strstr("hello world", "world");  // p指向 "world"
#### strspn(计算匹配字符长度)
`size_t strspn(const char *str, const char *accept);`
>[参数]：str：源字符串，accept：匹配字符集
>[返回值]：字符串开头连续属于accept的字符数
>size_t len = strspn("123abc456", "0123456789");  // len = 3
#### strcspn(计算不匹配字符长度)
`size_t strcspn(const char *str, const char *reject);`
>原理同上
>**返回值：** 字符串开头连续不属于reject的字符数
#### strncat(带长度字符串连接)
`char *strncat(char *dest, const char *src, size_t n);`
#### strcmp(字符串比较)
`int strcmp(const char *str1, const char *str2);`
>[返回值]：<0:str1< str2; 0:相等；
#### strncmp(带长度字符串比较)
`int strncmp(const char *str1, const char *str2, size_t n);`
#### atoi(字符串转整数)
`atoi`int atoi(char \*source);
>[注意事项]：忽略前导空格，遇到非数字停止
### 内存操作函数(string.h)
#### memcpy(带长度内存复制)
`memcpy`void \*memcpy(void \*dest, const void \*src, size_t n);
>[注意事项]：不检查 `\0` 结束符；内存不可重叠(否则用memmove)，参数可重叠
>[返回值]：dest(目标地址)指针
>int src[5] = {1,2,3,4,5};
>int dest[5];
>memcpy(dest, src, 5 * sizeof(int));
#### memset(内存填充)
`void *memset(void *s, int c, size_t n);`
>[参数]：s:目标地址，c:填充值，n:字节数
>int arr[100];
>memset(arr, 0, sizeof(arr));
#### realloc(重新分配)
`void *realloc(void *ptr, size_t size); `   // 重新分配`
### 字符处理函数(ctype.h)
int isalnum(int c);   // 字母或数字
int isalpha(int c);   // 字母
int isdigit(int c);   // 数字
int islower(int c);   // 小写字母
int isupper(int c);   // 大写字母
int isspace(int c);   // 空白字符（空格、\t、\n、\r、\f、\v）
int ispunct(int c);   // 标点符号
int isxdigit(int c);  // 十六进制数字
int iscntrl(int c);   // 控制字符
int isprint(int c);   // 可打印字符
int isgraph(int c);   // 可打印字符（不含空格）
int tolower(int c);   // 转小写
int toupper(int c);   // 转大写
```C
char c = 'A';
if (isupper(c)) {
    c = tolower(c);  // c = 'a'
}
```
### 数学函数(math.h)
#### 取整函数
double ceil(double x);   // 向上取整
double floor(double x);  // 向下取整
double round(double x);  // 四舍五入
double trunc(double x);  // 向零取整（C99）
#### 绝对值
int abs(int x);           // 整数绝对值
long labs(long x);        // 长整数绝对值
double fabs(double x);    // 浮点数绝对值
#### 幂和对数
double sqrt(double x);     // 平方根
double pow(double x, double y);  // x的y次方
double exp(double x);      // e的x次方
double log(double x);      // 自然对数
double log10(double x);    // 以10为底的对数
#### 三角函数
double sin(double x);      // 正弦（x为弧度）
double cos(double x);      // 余弦
double tan(double x);      // 正切
double asin(double x);     // 反正弦
double acos(double x);     // 反余弦
double atan(double x);     // 反正切
#### 其他
double fmod(double x, double y);  // 浮点数取余
double modf(double x, double \*ipart);  // 分解整数和小数部分
>[注意]：编译时加 -lm 链接数学库
### 随机数
```C
#include <time.h>
srand(time(NULL));           // 初始化种子
int r = rand() % 100;        // 0-99随机数
int r2 = rand() / (RAND_MAX / 100 + 1);  // 更均匀的分布
```
### 最大公约数
```C
int gcd(int a, int b){
    while(b){
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
```