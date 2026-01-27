---
data: 2025-10-16T19:32:00
tags:
  - C
---
# *指针*
## 基本形式
```C
#include <stdio.h>
int main(){
float x = 3.14159;
float *p;
p = &x;
printf("%f\n",*p);
return 0;
}
```
-  指针类型：__float \*p__
- 地址只能进行加减和关系运算，不能乘和除。
- __%p__：==用于打印十六进制的地址值==
- 不同类型的指针不能相互赋值
## 指向数组元素的指针
```c
double a[5]={1.1,2.2,3.3,4.4,5.5};
double *p=a;
for(i=0;i<5;i++){
	printf("a[%d]=%f,%p"i,*p,p);
	p=p+1;
}
```
- 此时，p=p+1，并不是把地址值加一，==而是把地址值加一个sizeof==（此处为double）
```c
double *p = &a[3];
printf("%f",*++p);    //p从a[3]指向了a[4]，再打印值
printf("%f",*p--);    //先打印a[4],再将p指向a[3]
```
## 指针运算
- 关系运算：指针可以和0与NULL比较，判断指针值是否为空
- __野指针__：未经过初始化和赋值的指针
- __悬空指针__：原本所指向的内存空间已经被释放的指针
```c
int a[5];
printf("%d",*(a+3));    //指输出a[3]
```
- 上述例子指出a[i]是转换为==\*（a+i）==的形式输出的（==下标与指针的换算关系==）
## 指向字符串的指针
```c
char *str = "hello world";   //str中存储了‘h’的地址
```
- ==算法：统计字符串中字母出现的个数==
```c
char *str = "xxxxxxxxxxxxx";
char c;
int anum[26] = {0};
while(c=*str++){           //当取出字符串末尾\0时，其值也刚好为0，循环停止
	if(c>='a'&&c<='z')
		anum[c-'a']++;     //通过字母获取下标
	if(c>='A'&&c<='B')
		anum[c-'A']++;
}
```
## 二维数组与指针
- __二维数组的存放方式__：先行后列
- ==行指针的命名方式==
```c
//行指针的命名方式
double score[5][4];
double (*p)[4];  //由于()，*先于p结合，指明p为指针，然后遇double [4],说明指向长度为4的行
```

- *注意：\*p\[j]相当于p\[j]\[0],而(\*p)\[j]相当于p\[0]\[j]*
## 字符串数组与指针
```c
char name[13][20];
char (*p)[20];
p = name;
*(*(p+i)+j);   
```
- name赋值后，p指向name【0】，p+i指向name的第i行；
- \*（p+i）即为name的第I行,也可以看作name的第i行的名称；
- （\*（p+i）+j）就表示name的第i行第j列的元素的地址；
- **name的第i行第j列的元素可以表示为**：==p\[i\]\[j\];(\*(p+i))\[j\];\*(p\[i\]+j);\*(\*(p+i)+j)==
- ==算法：将小写字母改为大写==
```c
char name[13][20]={"xxxx","xxxx","xxxxx"};
char (*p)[20];
int i,j;
p = name;
for(i = 0;i<13;i++){
	for(j=0;j<20;j++){
		if((*(*(p+i)+j)>='a')&&(*(*(p+i)+j)<='z'))
			p[i][j] -=32;
	}
}
```
- *注：char(\*p)\[N\]，则\*p为char\[N]类型，而(\*p)\[N]为char类型*
- 字符串数组的缺点：每一行是等长的，容易造成存储空间的浪费
## 指针数组与面向指针的指针
- 定义指针数组：
```c
char *pname[13] = {"xxxx","xxxx","xxxxx"...};
```
		注：由于[]的优先级比*高，所以pname优先与[]结合，表明pname是一个数组
		此时，pname[i]存储了指向第i个字符串起始字符的指针，故可以通过pname[i]来访问
		指针类型的元素大小固定，故不需要每个一维数组等长，例如[1]中5个字节，[3]中6个字节
		pname[i]可以被修改，而name[i]不可以
		pname[i]必须先指向有效数据，name[i]可以直接使用
- 指向指针的指针：
```c
char **p;
```
```c
//一个例子
char *pname[13] = {"xxxx","xxxx","xxxxx"...};
char **p;
int i;
p = pname;
for(i = 1;i<13;i++){
	p++;
	printf("%s\n",*p);     //效果：依次输出字符串
}
return 0;
```
		此时，要打出的*p仍是地址（pname[i]，类型为char *）
		但由于用%s打印，所以会从*p地址中读取字符，然后打印
## 函数中的指针
### I.指针作函数参数
- 函数的形参和实参是不同的变量，之间的关系仅仅是：**在调用时，实参值复制到对应的行参上**
- 指针作为实参时；
```c
void swap2(int *px,int *py){
	int t;
	t = *px,*px = *py,*py = t;
}
int main(){
	int a = 3,b = 5;
	int *pa, *pb;
	pa = &a;
	pb = &b;
	swap2(pa,pb);    //结果为：调换了a和b的赋值
	return 0;
}
```

> [原因] 
> pa和pb分别是指向a，b的指针，并分别传递给形参px，py，因此px，py也分别指向a，b
> swap2（）将\*px和\*py值进行交换，也就将a和b进行交换，虽然函数返回后，px和py不存在了，但对a和b的修改保存了下来。
- **优点**：函数返回值 只能返回一个数，但利用指针可以返回多个
- ==算法：求浮点数的整数和小数部分==
```c
void decompose (double x,long *int_part,double *frac_part){
	*int_part = (long)x;
	*frac_part = x - *int_part;
}
int main(){
	long i;
	double d;
	decompose(3.14159,&i,&d);
	printf("int_part = %d",frac_part = %f\n,i,d);
	return 0;
}
```
- ***printf()和scanf()的本质***
```c
char format[] = "%10.2f\n";
double f = 3.1415926535897;
printf(format, f);
```

> [注意] printf（）和scanf（）的第一个参数都是指向字符串起始字符的指针
>        通常以字符串常量作为实参，实际上也可以使用字符数组。（可用于修改输出格式串）
### II.命令行参数
- ==main的两种互相等价的原型形式==
```c
int main(int argc,char **argv);
int main(int argc,char *argv[]);
```

> [注意] argv定义为字符指针数组；
>        这两种形式主要用于需要命令行参数的应用场合
- **命令行参数**:在运行一个程序或命令时，在程序名后输入的额外文本信息，用于向程序传递特定的指令，选项或数据。
- **用处**：（无界面操作）对于在服务器上运行的，没有图形界面的程序，命令行参数是用户和程序交互的主要方式。（*例如：在Windows命令提示符界面运行程序：program arg1 arg2*）
- **在main（）的两个形参中**，argc表示命令行参数的个数，包括可执行程序名本身；
                        argv是一个指向字符指针数组起始元素的指针；
                        此字符指针数组共argc个字符指针元素，每一个指向一个字符串的起始字符，字符串依次存储了包括可执行程序名在内的所有命令行参数
- eg.==使用命令行参数实现echo功能==
```c
int main(int argc,char *argv[]){
	while(--argc>0)
		printf("%s%c",*++argc,(argc>1)?' ':'\n');
	return 0;
}
```

> [注意] 在windows命令提示符界面执行该程序，并带有命令行参数，如“*echo CProgramming*”
>        此时，源程序通过argv\[0\]来知道可执行程序的名字为“*echo.exe*”
## 指针用作函数返回值

> [注意] 若函数返回的指针所指向的内存已经被释放，该指针就变成悬空指针，可能导致严重错误
- **一个错误的例子**
```c
int *fault(int x,int y){
	int t;
	if(x>y)
		t = x;
	else
		t = y;
	return(&t);
}
```
>fault函数的返回值是其内部定义的局部变量t的地址，并存到堆栈中。
>函数返回后，局部变量t的内存空间被释放。

> [解释] 若返回t
> 在函数返回前，将t的值复制到返回值寄存器或调用者的栈帧中，以后调用者得到的是值的副本，
> 不依赖原t的内存空间。
- **解决方法**：malloc（）动态存储；static关键字静态存储；字符串常量一般也存放在静态存储区
## 用函数处理字符串
### 求字符串长度
```c
int udf_strlen(char *s){
	int len = 0;
	while(*s++)
		len ++;
	return len;
}
```
>\*s++:s先自增，但s++的值仍是自增前的值
### 字符串复制
```c
char *copy(char *d,char *s){
	char *t = d;
	while(*d++ = *s++);    //注意是=而不是==
	return t;
}
```
### 字符串连续
```c
char *udf_strcat(char *d,char *s){
	char *t = d;
	while(*d)
		d ++;
	while(*d++ = *s++);    
	return t;
```
## 指向函数的指针
- 形式
```c
int func(int x,int y);
int (*pf)(int x,int y);
pf = func;
```
>由于函数名被视为指向函数的指针，因此（\*pf）和pf是相等的
>==\*pf（1，2） = pf（1，2） = func（1，2）==

> [注意] （\*pf）的括号不能省略，若\*pf（1，2），表示对pf（1，2）的返回值做指针运算
- 优势：作为参数传递到某个函数中，利用传递进来的函数指针实现灵活的回调
- ==算法：一元函数定积分梯形法数值求解==
```c
double integral(double (*f)(double),double a,double b){
	double s,h;
	int n = 1000,i;      //f即为要积分的函数
	h = (b-a)/n;
	s = (f(a)+f(b))/2.0;
	for(i = 1;i<n;i++)
		s +=f(a+i*h);
	return s*h;
}
```
## 结构体指针
- *运算符“->”可用于访问结构体成员*
```c
struct strStudent{
	long int lnum;  //此处省略
}
int main(void){
	struct strStudent *ps;
	printf("%d",ps->lnum);
	return 0;
}
```
---
# *文件处理*
## 流与文件
- 流：数据读写操作都是通过流来实现的。
- 分类：标准输入流（stdin）：默认表示从键盘输入。scanf（）等会从这个流中读取字符
		标准输出流（stdout）：默认输出至显示器屏幕。
		标准错误流（stderr）：写入错误诊断的流。
- **缓冲文件系统**：  相较于内存数据操作，磁盘文件的读写速度是相对较慢的，
				若程序每次数据访问时都直接访问磁盘文件，读写效率会很低；
		而缓冲系统自动在内存中为每个正在使用的文件都开辟一个缓冲区。
		输出和输入的时候都先通过缓冲区，缓冲区装满后在进行下一步操作。
		*一次大块数据读写速度远大于多次小块数据读写*
## 文件指针
- 类型：FILE*
>*其中，FILE是在头文件stdio.h中定义的结构体类型的别名*
- 例如：
```c
typedef struct(){
	int level;               //缓冲区满空程度
	unsighed flags;          //文件状态标志
	char fd;                 //文件描述符
	unt bsize;               //缓冲区大小
	unsighed char *buffer;   //数据缓冲区指针
	unsighed char *curp;     //当前位置指针
	unsighed istemp;         //临时文件指示器
	short token;             //有效性检查
}
```
## 文件操作
- 文件模式

| 文本模式 | 二进制模式   | 意义                        |
| ---- | ------- | ------------------------- |
| r    | rb      | 只读，打开已有文件                 |
| w    | wb      | 只写，打开或创建文件，若文件已存在，文件长度清零  |
| a    | ab      | 追加，打开或创建文件，若文件已存在，从末尾写入数据 |
| r+   | r+b/rb+ | 读写，打开已有文件                 |
| w+   | w+b/wb+ | 读写，打开或创建文件，若文件已存在，文件长度清零  |
| a+   | a+b/ab+ | 读写，打开或创建文件，若文件已存在，从末尾写入数据 |
### 文件打开
```c
FILE *fopen(const char *filename,const char *mode);
```
>前者是文件路径，后者是期望打开模式
- 检查是否打开成功
```c
FILE *fp;
if ((fp = fopen("xxx.txt","r")) == NULL)
```
### 文件关闭
```c
fclose(fp);
```
>如果正常关闭文件：返回值是0，否则返回EOF，即-1。
### 文件读写
- 格式化读写函数fscanf(),fprintf()：格式化文本文件的读写；
- 数据块读写函数fread(),fwrite()：二进制文件的读写；
- 字符读写函数fgetc(),fputc()：文本文件和二进制文件逐个字节的读和写；
- 字符串读写函数fgets(),fputs()：用于对文本文件的字符串读和写。
```c
int fget(FILE *stream);
int fputc(int ch,FILE *stream);  //stream为读写的文件指针，ch为要录入的字符
```
>fgetc()：读取成功，将读取的字符看作unsighed char类型并转换为int作为返回值，否则返回EOF
>fputc()：写入成功，返回写入的字符，否则返回EOF
```c
char *fegts(char *str,int count,FILE *stream)//count为读取的最大字符数
int fputs(const char *str,FILE *stream)//str为要读写的字符串，stream为文件指针
```
>fgets()：  读取最多count-1个字符并添加\0；在遇到换行符或文件结束时提前终止，并添加\0；
>         读取成功返回str的值；否则返回空指针NULL（操作达到文件末尾无法读取任何字符）
>fputs()：不写入\0；若成功，返回非负值；否则返回EOF
- **rewind（FILE \*stream）：将指标移到文件开头**
### 判断文件结束
```c
int feof(FILE *stream);
int ferror(FILE *stream);
```
>feof()：文件达到文件结尾时，返回非零值；否则返回0
>ferror()：文件发生错误时，返回非零值；否则返回0
---
# *内存分配与链表*
## 动态存储分配
- 使用原因：变量和数组都是固定大小的。
```c
void *malloc(size_t size);
void *calloc(size_t n,size_t size);
```
>malloc()：分配了size字节的存储空间；
>calloc()：分配n\*size字节的存储空间；
>二者的返回值相同，若分配成功，则返回指向分配存储空间首地址的指针；否则返回空指针
```c
void free(void *ptr);   //ptr：指向存储空间首地址的指针
```
>free()：用于释放动态分配的存储空间。
- 动态分配的存储空间在内存中是连续的，与数组的存储方式相同->可以构造一维动态数组。
- 同理，也可以创建二维数组。
```c
//eg.
int **pb;
pb = (int **)malloc(col * sizeof(int *));
for (i = 0;i<row;i++)
	pb[i] = (int *)malloc(col * sizeof(int));
//创建了一个二维的动态数组
for (i = 0;i<row;i++)
	free(pb[i]);
free(pb);           //动态存储空间不再使用时，需释放其内存
```
## 链表
### 定义
- 链表：可以非连续，非顺序的存储结构，数据元素的逻辑顺序通过链表的指针链接次序实现。
- 结点：连表中的每一个元素，可以在运行时动态存储和释放。
- 分类：单链表，双向链表，循环链表，静态链表...
### 单链表
- 每个结点包括两个部分：==**存数数据元素的数据域，存储后继结点地址的指针域**==
>在无头结点单链表中，头指针指向第一个结点，通过头指针访问链表。
>在带头结点单链表中，第一个结点前还有一个结点，称为头结点（其数据域不存放有效数据），
>                   指针域指向链表的第一个结点。
- 单链表的最后一个结点指针域的值一般是空指针，表示它没有后继。（示意图中一般由^表示）
#### 1.链表结点类型
- ==*需声明结构体类型*==
```c
struct node(){
	int num;            //数据域
	struct node *next;  //指针域
}
```

> [注意] next是指向结点结构体的指针，这说明next可以指向一个结点
> 	在单链表中，next将指向当前结点的后续结点。
- ==*定义链表的头指针（指向第一个结点的指针变量）*==
```c
struct node *head = NULL;
```
>head的初值为空指针，表示链表的初始状态是一个空表，没有任何结点。
- ==*创建一个新的结点*==
```c
struct node *p;
p = (struct node *)malloc(sizeof(struct node));
scanf("%d",&p->num);
p->next = NULL;
```
>创建了一个新的结点，由p指向这个结点，从键盘输入数据域的值，并对指针域设置为空指针。
- ==*链表的建立*==
==头插法==（插入第一个结点之前）
```c
struct node *CreateListF(void){
	struct node *head,*p;
	int num;
	
	head = NULL;
	printf("Input an integer:");
	scanf("%d",&num);

	while (num!=0){
		p = (struct node *)malloc(sizeof(struct node));
		p->num = num;
		p->next = head;
		head = p;
		printf("Input an integer:");
		scanf("%d",&num);
	}

	return head;  //由于头指针会被修改，一般要将头指针返回。
}
```
![头插法|500](链表_edit_225708402741596.jpg)
==尾插法==
```c
struct node *Createlist(void){
	struct node *node,*rear,*p;
	head = NULL;
	printf("input an integer:");
	scanf("%d",&num);

	while(num!=0){
		p = (struct node *)malloc(sizeof(struct node));
		p->next = NULL;
		p->num = num;
		if (!head)
			head = p;
		else
			rear->next = p;
		rear = p;
		printf("input an integer:");
		scanf("%d",%num);
	}
	return head
}
```
- ==*链表的遍历*==
```c
void PrintList(struct node *head){
	struct node *p = head;
	while (p){
		printf("%d\n",p->num);
		p = p->next;	
	}
}
```
==*插入结点*==
```c
struct node *InsertList(struct node *head,struct node *q){
	struct node *p;
	if(!head){
		head = q;
		return head;
	}
	if(head->num > q->num){
		q->next = head;
		head = q;
		return head;
	}
	p = head;
	while(p->next && p->next->num < q->num)  //注意：两条件不能交换！
		p = p->next;
	q->next = p->next;
	p->next = q;

	return head:
}
```
==*删除结点*==
```c
struct node *DeleteList(struct node *head,int num){
	struct node *p,*q;

	it(head && head->num == num){
		p = head;
		head = p->next;
		free(p);
		return head;
	}
	q = p = head;
	while(p && p->num != num){
		q = p;
		p = p->next;
	}
	if (p){
		q->next = p->next;
		free(p);
		return head;
	}
	reintf("No Found!\n");
	return headd;
}
```
### 广义表
- 是线性表的推广，数据域中可以是元素也可以是另一个广义表
```c
typedef struct GNode *GList;
struct GNode{
	int Tag;
	union{
		ElementType Data;
		GList SubList;
	}URegion;
	GList Next;
}
```
>Tag：标志域，0代表结点是单元素；1代表结点是广义表
>union：子表指针域SubList和单元素数据域Data复用，共用存储空间
### 双向链表
- 存储结构
```c
typedef struct DuLNode{
	ElemType data;
	struct DulNode *prior;
	struct DulNode *next;
}DuLNode,*DuLinkList;
```
### 多重链表
- 链表中的节点可以隶属于多个链
- 多重链表中指针域会有多个，如前文中包含的Next和SubList
---
# *附录*
## 枚举类型
```c
enum weekday{sun,mon,...};
enum weekday a, b, c;
```
- 可以在类型定义时，给每个元素赋值（默认为0，1，......）
```c
enum weekday{sun=7,mon=1,...}  //此时以后的元素从2开始赋值
a = （enum weekday） 2；        //可以使用赋的值给变量赋值
```
- 枚举值可以用来比较
```c
if(today == sun)
```
## 自定义数据
```c
typedef 原类型名 新类型名
typedef int ARRAY[20];
ARRAY a1,a2;     //a1,a2为整形数组，包含20个元素
```
## 位运算

| 运算符 | 含义   | 描述                  |
| --- | ---- | ------------------- |
| &   | 按位与  | 二进制位都为1，则为1；否则为0    |
| \|  | 按位或  | 二进制位有一个1，则为1；否则为0   |
| ^   | 按位异或 | 二进制位相同则0；否则为1       |
| ～   | 按位取反 | 单目运算符，对一个数二进制位取反    |
| <<  | 按位左移 | 二进制位全部左移N位，高位舍弃，右补0 |
| >>  | 按位右移 | 二进制位全部右移N位，低位舍弃，高位0 |
## 宏替换
```c
#define 标识符 字符序列
#define 宏名（形参列表） 宏体
#define SWAP(x,y) {unsighted long temp = x;x = y;y = temp};
```
- “#undef 宏名”：终止一个宏的作用范围
## 条件编译
```
#if 表达式
	语句1；
[#else
	语句2；]
#endif
```
>当表达式为真时，编译1；否则编译2（else语句可省略）

```
#ifdef 标识符
	语句1
[#else
	语句2]
#endif
```
>当标识符已经被定义时，编译1；否则2

```
#ifndef 标识符
	语句1
[#else
	语句2]
#endif
```
>与ifdef相反

- 例如
```
#ifdef WINDOWS
#define MYTYPE  long
#else
#define MYTYPE float	
#endif
```
>在WINDOWS平台上此数据类型为long，在其他时为float
>此时在WINDOWS上编译，可在程序的开始加上#define WINDOWS
>否则可以输入#define WINDOWS 0
## 时间测量
```c
#include <time.h>
int main(){
	double duration
	start = clock();
	xxxx;
	stop = clock();
	duration = ((double)(stop - start))/CLK_TCK;
	return 0;
}
```














