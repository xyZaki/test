
Jim C


目  录


一.
C简介
C语言起源于贝尔实验室，派生于B语言，为设计UNIX系统而研发。C语言采用结构化控制方式，并可直接操作寄存器和内存，简洁、快速、高效、易移植。
附：历史碎片，①[主流编程语言在知名软件的应用列表](http://www.lextrait.com/vincent/implementations.html)。②C[语言之父Dennis Ritchie的个人主页](http://www.bell-labs.com/usr/dmr/www/)。
二.C语句
语句（statement）是构造程序的基本成分，以分号“;”标识语句结束。
空语句是最简单的语句，只包含一个分号，不执行任何任务，可用于有语法要求的场合。

三.C结构




TBD
1.C编译过程、中间文件。。
![image](https://github.com/Jim-CodeHub/Skills-list/raw/master/image/Compile.png)

gcc例，编译过程：

gcc -E 1.c -o 1.i  //预处理工作-由预处理器完成-：导入包含文件，删除注释，展开宏，代码优化
				   //（删除多余空格，换行处理等）-->生成.i文件 可查看预处理状态
				   //预处理后的.i文件非常大，因程序引用了很多头文件都要导入

gcc -S 1.i -o 1.s  //编译工作-由编译器完成-：将c语言代码生成与平台相关的汇编代码，不同平台
				   //的编译器产生不同的汇编代码，众多平台为c设计了生成其平台的c编译器，这增强了c语言的可移植性
				   //编译后的.s文件很小

gcc -c 1.s -o 1.o  //汇编工作：-由汇编器完成-：将汇编代码生成二进制目标代码，汇编后的文件又变大

gcc 1.o -o 1.out   //链接工作：-由链接器完成：将目标代码与库进行链接生存可执行文件，链接后的文件最大

2.C标准
20世纪70年代末，随着微型计算机的发展，C语言开始移植到非UNIX环境中，并逐步成为独立的程序设计语言。在1978年，Kernighan和Dennis Richie的C Programing Language》第一版出版，在这本书中，C语言通常被表述成“K&R C”；
1988年ANSI对C语言进行了标准化，产生了“ANSI C”，在ANSI标准化的过程中，一些新的特征被加了进去，包括C函数库。之后，ANSI C标准被ISO采纳成为ISO 9899，ISO C的第一个版本文件在1990年出版。C标准在90年代末才经历了改进，这就是ISO9899:1999（1999年出版）。这个版本就是通常提及的C99，它被ANSI于2000年采用。
更多有关国际标准组织的知识请参见：《TBD》
C标准：
K&R C
ANSI C
ISO C （ANSI/ISO C）
C语言没有官方标准，Kernighan和Dennis Richie的C Programing Languag称为大家接收的标准，称为K&R C 或经典C，该附录定义了C语言却没有定义C库，因为缺乏任何官方标准，所以提供UNIX实现的库称为了一个事实标准。
c标准与POSIX
K&R C （旧式C 非官方标准）
C89	（ANSI C）
C90	（ISO C； 1990年发布）
C99	（ISO C； 1999年发布）
C11 （ISO C； 2011年发布）

posix标准
	定义操作系统API的标准，含C库标准、系统调用标准和其它接口标准
		
	ISO C 规定了标准C的方方面面
	POSIX 在标准C基础上增加了一系列API，（POSIX包含标准C库）。
	SUS（single UNIX Specification）：SUS是POSIX的扩展，POSIX是SUS的基本部分

	扩展标准C的还有其它标准：BSD、System V，因为它们是早期比较成功的Unix操作系统，所以很多特色函数都被广泛使用
	3.心得
	有符号数与无符号数不可比较

	循环变量要用有符号数 特别对于--的变量


	巧用预处理，特别是固定的数值运算，使用宏可以在预处理阶段由预处理器自动计算完成

	数组溢出与内存栈：
	1.
	char str[10];
	char str2[10];

	如果str溢出，将影响str2的值，特别是在裸机环境下
	（小端模式下，str2溢出，将影响str的值）
	2.
	    char str[2];
		    char str2[2];

			    str[-1] = 'a';
				    str[-2] = 'b';

					    putchar(str2[0]); //b
						    putchar(str2[1]); //a

							结构体报错问题：
							结构体(和联合、枚举等)存储大小未知的问题：
								1.没有原型 - 头文件没有包含
									2.写错原型


									常量指针与函数
									下面这段代码，在cstr和str作用域内，其值是不会改变的：

									const char * cstr = "123";
									char * str = "123";

									下面这段代码，在cstr2和str2作用域内，其值是否改变是不确定的：
									cosnt char * cstr2 = fun();
									fun2();

									char * str2 = fun();
									fun2();

									原因是cstr2或str2 指向的是函数返回值，这个值是很可能在栈中的，所以这个值在某个函数运行后
									会改变栈的数据，值即变化

									4.C++ 兼容C的预处理
									C与C++程序的兼容性
									“很多”C标准库头文件和很多成熟的第三方C库头文件都带有

#ifdef __cplusplus
									extern "C"
{
#endif
	...

#ifdef __cplusplus
}
#endif

这是避免C++调用C库时可能出错，所以加上预编译开关。
并且在C++中，自带宏“#define __cplusplus”

所以这样做的目的就是为了C兼容C++程序

5.NULL 有NUL
NUL - ASCll - ‘\0’ 不存在C中
NULL - 值为0的指针 在stdio.h中
值同 可互换

6.内联汇编（GCC）
一．简介
内联汇编是C/C++编译器支持的特性，这使得汇编代码可以嵌入到C/C++代码中，嵌入汇编代码的意义是使代码更灵活、更高效、更符合硬件特性。不同的编译器支持不同的内联汇编格式，一般以“asm、_asm、__asm__”开始，其内容以“()”或“{}”包含，其结尾以“;”等结束。
二．语法
GNU-GCC编译器支持内联汇编，分为基本内联和扩展内联，其格式如下：
------------------------------------------------------------------------------------------------------------------
·基本内联
__asm__ __volatile__ (“cmd”);
如命令部分有多条语句需要分行写的，可加“\n\t”。
__asm__ __volatile__ (
		“cmd1 \n\t”
		“cmd2 \n\t”
		... ...
		);
·扩展内联
__asm__ __volatile__ (“cmd” : “输出操作数” : “输入操作数” : “破坏描述部分”);
或
__asm__ __volatile__ (
		“cmd” : 
		“输出操作数” : /*可选*/
		“输入操作数” : /*可选*/
		“破坏描述部分” : /*可选*/
		);
------------------------------------------------------------------------------------------------------------------s
在扩展内联的cmd中可使用“%0、%1、%2...”表示输出或输入操作数，使用“%%寄存器名”来调用寄存器。输入输出操作数中可使用“r、=r、w、=w ... ...”等约束符，最后的破坏描述部分是描述使用的寄存器，这一部分通常省略，交给编译器自动解决。
7.自定义printf scanf

1. 
FILE uartio = FDEV_SETUP_STREAM(uart_putchar, uart_getchar, _FDEV_SETUP_RW); 
stdout = &uartio;

然后定义 uart_putchar() uart_getchar()

	就可以在单片机程序中使用printf()和scanf()


	2.
	使用

		myPrintf(const char *fmt, ...)
		{
					int i;
							char buff[BUFF_SIZE];
									
									va_list ap;
											va_start(ap, fmt);
													
													vsprintf(buff, fmt, ap);
															
															while(buff[i] != 0)
																		{
																						uart_putchar(buff[i]);
																									i++;
																											}
																	
																	va_end(ap);
																			...
																					}
	
	
	
注意：
	有些单片机的libc库不支持%f的格式化输出，代替输出的是一个？号
	8.以数组为形参的函数
#include <stdio.h>

	void fun(int p[2])
{
	    printf("%d\n", p[0]);
		    printf("%d\n", p[1]);
			    return;
}

int main(void)
{
	    int p[2] = {1,2};

		    fun(p);

			    return 0;
}


//实际上int p[2]的形参形式，与 int *p 或 int p[]、 int p[0]、 int p[100]的形式是等价的，
//给予数字的提示告诉了用户该函数需要指向元素的个数
9.解释型与编译型语言对比
两种语言的区别

本质上是 运行速度 与 移植性 的较量与权衡

注意，这里说的是移植性，而不是可移植性

现在的编译型语言可以做到可移植， 但移植速、移植时间、移植复杂性等需要考量
而解释型语言可以做到快速移植，或者可以基本不加改动的移植到其它系统中使用
（解释型语言移植的是解释器本身，而编译型语言除了编译时要用跨平台的编译器，还需要跨平台的运行库支持）

因为解释型语言依靠解释器，无论何时想要执行都必须有解释器存在，这使得解释型语言相对效率较低（解释解释，什么叫做惊喜）
而 编译型语言编译一次即可


现代是计算机 计算力过剩的年代，所以一般情况下感受不到解释器速度的延时
看起来与编译型编译出来的二进制代码执行速度无差别

10.restrict C99关键字
restrict 是C99标准添加的关键字

restrict 用于指针约束，以告知编译器所有访问该内存空间的都使用当前指针，即：内存空间的访问具有唯一性。

Note：该关键字是给编译器优化使用的，编译器根据该关键词优化出更具效率的汇编代码，而非用于程序间指针的约束，
也就是即便给指针使用该关键字，指针指向的空间仍然是可以通过其它指向该空间的指针改变的，所以restrict关键字
应该有两层含义：
	1. 告知编译器，该内存空间只通过该指针访问
		2. 编程人员不应该再使用其它指针指向该内存空间，也不能使两个restrict关键字约束的指针指向同一个内存空间，否则不能正确优化		
		11.


		C++更强调数据的处理方式， --- 数据
		C更强调处理算法。 --- 算法


		变量声明的意义：1.向前声明，作用域识别。2.语法检查 - 保证用过的每一个变量都是用户自己声明的、存在的、不重复的。

		定义对变量来说是开辟空间，对函数来说是实现。

		常量：
		编译器默认小数为double类型，整数为有符号int类型
		1.0f 强制指定该小数为float类型
		1.0L 强制指定该小数为long double类型
		1U 强制指定该整数为unsigned类型
		1L 强制指定该整数为long类型
		1UL 强制指定该整数为unsigned long类型



		char是否为有无符号数由编译器决定，如果有无符号很重要，则需要显示指定：signed char, unsigned char。

		C++11 重新定义了auto关键字的含义，在C中auto是存储类型，在C++中auto是一个变量类型，C++中auto变量因赋值改变而改变：
		auto a = 10； //a为int类型
		auto b = 10.0； //a为double类型
		auto c = 10.0f；//a为float类型

		整数与浮点数运算 结果为浮点数

		类型转换：
		数值类型转换发生在变量在任何情况下未进行预期输入的情况，如变量赋值、表达式计算、手动输入、函数传参等等。
		进行转换的数据类型通常是浮点数和整型，整型有级别大小之分
		有符号 long long > long > int > short > signed char
		无符号 unsigned xxx 同上
		浮点数 long double 》 double 》 float
		分级别的概念是为了在表达式中进行转换的依据：
		1.表达式中有一个浮点数，其它都转换为浮点数，浮点数转换按级别转，如有一个为long double，其它都转为long double，否则有一个double 都转换为double，否则由一个float都转换为float
		2.表达式中都是整数
		(1)如果都是有符号数或无符号数，则以最高级别为标杆转换
		(2)如果有无符号数混用
		①如果无符号数级别高，则按无符号为标杆转换，
		②如果有符号数级别高，且有符号数能表示无符号数所有区间取值，则转换为有符号数，否则转换为无符号数

		通常较小存储空间的类型向较大存储空间类型转换不会发生问题
		在赋值或传参时，将小空间整数转换为大空间整数没有问题，反之将截短。将浮点数转换为整数将丢失小数。将double转为float将丢失精度。

		以上进行转换时都是自动的，存在潜在问题也是自动处理的，除了自动的还可以强制转换

		C风格 int a = (int)10.1
		C++风格int a = int(10.1)
	除此之外C++还引入4个强制转换运算符


	assert 断言
	void assert（int expression）
	形参接收一个表达式的返回值，如果结果是0，则表示断言成功，系统立即停止运行，并报错
	如果返回1，则表示断言失败，程序继续运行
	例子：
	void fun(char *s)
{
	assert(NULL != s);//如果给形参s为NULL，则返回值为假，断言成功，程序停止，报错
}
![image](https://github.com/Jim-CodeHub/Skills-list/raw/master/image/unicode.png)