---
tags:
  - C
---
## I.运行
`gcc -g text.c -o text`:产生编译文件
`gdb text`:进入gdb环境
## II.断点设置
`info b`:查看所有断点

`b 9 && b text.c:9`:在第九行设置断点
`b 函数名`:在函数处设置断点，在程序调用到函数时会断住
`b text.c:23 if b==0`:但b等于0时，在第23行断住

`condition 1 b==0`:当b等于0时，产生断点1

>根据规则设置断点
`rbreak printNum*`:对所有调用printNum函数都设置断点
`rbreak text.c`:对text.c中所有函数都设置断点

>设置临时断点
`tbreak text.c:10`:在第10行设置临时断点

>跳过多次设置断点
`ignore 1 30`:断点1跳过30次再生效

>禁用或启用断点
`disable`:禁用所有断点
`disable bnum`:禁用标号为bnum的断点
`enable`:启用所有断点

>断点清除
`clear`:删除当前行所有断点
`clear function`:删除函数名为function处的断点
`clear lineNum`:删除行号为lineNum处的断点
`delete bnum`:删除断点号为bnum的断点
## III.变量查看
`p a`:查看变量a的内容

>打印指针类
`p d`:打印指针地址
`p *d`:打印指针指向的第一个值
`p *d@10`:打印指针指向的后10个值

>打印节点类
`p *linknode`:打印节点内容
`p *$.next*`:打印下一个节点内容

>自动显示变量内容
`display e`:当程序断住时，自动显示e变量的值
`info display`:展示所有设置了display的变量
`delete display num`:删除num编号的display变量
## IV.程序调试
`n`:单步执行
`n num`:往后执行num行

`s`:单步进入(可以单步追踪到函数内部，如果没有进入到函数，则跟n没有区别)

`c`:继续执行到下一个断点

`u num`:继续执行到，直到到第num行
## V.编辑源码
`$ EDITOR=/usr/bin/vim`  
`$ export EDITOR`:设置编辑器路径

`edit 3`:编辑第3行
`edit printNum`:编辑printNum函数
`edit text.c:5`:编辑text.c第5行

`shell gcc -g -o main main.c`:重新编译程序
## VI.窗口模式
`gdb main -tui`

