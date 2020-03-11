
# Jim C++

## Catalog ##

## 一. 简介 ##
&ensp;&ensp;&ensp;&ensp;C++起源于贝尔实验室，从C派生、与C兼容、编译时绑定。C++融合了POP、OOP和泛型编程（模板）等编程方式，使用 **类** 和 **对象** 来描述数据，强调数据的表达方式，具有**封装**、**继承**、**多态**、异常处理等特性。  
&ensp;&ensp;&ensp;&ensp;附：[C++之父Bjarne Stroustrup的个人主页](http://www.stroustrup.com/index.html)。  
&ensp;&ensp;&ensp;&ensp;![Bjarne Stroustrup](https://github.com/Jim-CodeHub/Skills-list/raw/master/image/Bjarne_Stroustrup.jpg)

## 二. 抽象
&ensp;&ensp;&ensp;&ensp;“抽象指从具体事物中被抽取出来的相对独立的各个方面、属性、关系等。” -- 《辞海》。C++的抽象是通过“**类**”来表示一类实物的共有属性，是泛型编程的基本思想。  
&ensp;&ensp;&ensp;&ensp;类的本质是操作受限的结构变体，对象是结构体变量，关键词<i><font color=purple>class</font></i>和<i><font color=purple>struct</font></i>在C++中等效。类的成员分为<strong>属性</strong>（变量/数据等）和<strong>方法</strong>（函数/行为/动作/功能等）。  

&ensp;&ensp;&ensp;&ensp;`语法：class/struct CLASS_NAME{...attribute list AND function list...};`  

&ensp;&ensp;&ensp;&ensp;传统结构体的使用方式是类的使用方式的子集，类还具有方法成员和权限控制能力。
## 三. 封装
&ensp;&ensp;&ensp;&ensp;C++类的成员具有访问限制：Private（私有，默认）、Protected（保护）、Public（公有）。
<table style="font-size:14; text-align:center; padding-left:26; width:763px;">
<tr>
<td>封装方式</td><td style="wid">访问限制</td>
</tr>
<tr>
<td>Public</td><td>无</td>
</tr>
<tr>
<td>Protected</td><td>类、友元函数、派生类</td>
</tr>
<tr>
<td>Private</td><td>类、友元函数</td>
</tr>
</table>
&ensp;&ensp;&ensp;&ensp;一般将数据成员设计为私有、方法成员设计为公有，仅对派生类开放的成员设计为保护类型。类的对象通过公有或保护方法来操作私有数据。C++将结构体扩展成类，具备类的所有特性，但默认访问限制为公有。  

&ensp;&ensp;&ensp;&ensp;`语法：class/struct CLASS_NAME{... pulic:... protected:... private:...};`

&ensp;&ensp;&ensp;&ensp;C++类的特殊方法成员可以在对象创建、复制和销毁时被系统自动调用：构造函数、拷贝构造函数和析构函数。  

&ensp;&ensp;&ensp;&ensp;<strong>1. 构造函数与创建对象</strong>  

&ensp;&ensp;&ensp;&ensp;构造函数是类中与类同名的公有方法成员，可缺省、可重载、无返回值，在对象被创建（于堆或栈）时由系统自动调用，常用于初始化属性成员和自定义初始动作。

&ensp;&ensp;&ensp;&ensp;`语法：class/struct CLASS_NAME{... pulic: CLASS_NAME(Param list){...};};`

&ensp;&ensp;&ensp;&ensp;&rarr; 创建对象的方法：  

&ensp;&ensp;&ensp;&ensp;+ `栈中：class/struct CLASS_NAME`&ensp;&ensp;&ensp;&ensp;&emsp;&emsp;&ensp;&ensp;&ensp;&nbsp;&nbsp;&nbsp;` OBJ <OPT [n]数组, (...)构造传参>;`  
&ensp;&ensp;&ensp;&ensp;+ `堆中：class/struct CLASS_NAME *Pobj = new OBJ <OPT [n]数组, (...)构造传参>;`  

&ensp;&ensp;&ensp;&ensp;默认构造函数形参为空，创建对象时未传参则调用默认构造函数。

&ensp;&ensp;&ensp;&ensp;<strong>2. 拷贝构造与复制对象</strong>  

&ensp;&ensp;&ensp;&ensp;拷贝构造函数是构造函数的特例，本质是构造函数的重载，含一个类引用形参，在对象被复制时由系统自动调用，用于处理因对象复制可能导致的内存泄露、重叠等问题。  
&ensp;&ensp;&ensp;&ensp;原则上凡是包含堆和指针成员的类都应该提供拷贝构造函数并合理重载“=”运算符。对象复制发生于三种情形：向函数形参传递对象值、从函数返回对象值、对象赋值。

&ensp;&ensp;&ensp;&ensp;`语法：class/struct CLASS_NAME{... pulic: CLASS_NAME(CLASS_NAME &){...};};`

&ensp;&ensp;&ensp;&ensp;&rarr; 浅拷贝与深拷贝：  
&ensp;&ensp;&ensp;&ensp;浅拷贝又称位拷贝，同普通变量赋值，即将A的内存空间数据按位复制到B。缺省的拷贝构造函数和未重载的“=”运算符都为浅拷贝。  
&ensp;&ensp;&ensp;&ensp;当类中存在指针时，如使用浅拷贝，对象A和B的指针成员将指向同一内存空间而导致内存重叠，如果指针指向堆，则在对象销毁时该空间会被释放两次，深拷贝需要设计拷贝构造函数，使对象复制发生时完全拷贝内存镜像。

&ensp;&ensp;&ensp;&ensp;<strong>3. 析构函数与销毁对象</strong>  

&ensp;&ensp;&ensp;&ensp;析构函数是类中与类同名的、带有“~”前缀的公有方法成员，可缺省、不可重载、无参、无返回值，可手动调用或在对象被销毁（于堆或栈）时由系统自动调用，常用于释放空间和自定义结束动作。  

&ensp;&ensp;&ensp;&ensp;`语法：class/struct CLASS_NAME{... pulic: ~CLASS_NAME(){...};};`  

&ensp;&ensp;&ensp;&ensp;&rarr; 销毁对象的方法：  

&ensp;&ensp;&ensp;&ensp;+ `栈中：栈中的对象在其所在作用域结束时自动销毁`  
&ensp;&ensp;&ensp;&ensp;+ `堆中：delete <OPT []数组对象 > Pobj;` 

&ensp;&ensp;&ensp;&ensp;<strong>4. 友元函数和友元类</strong>  

&ensp;&ensp;&ensp;&ensp;友元是类外的、可以访问该类中所有成员的函数或类，称友元函数和友元类，使用<font color=purple>friend</font>关键词声明。友元关系是单向的，不具有传递性且不能被继承。友元破坏了类的封装性又适当的打破了封装的局限性，使编程更加灵活。

&ensp;&ensp;&ensp;&ensp;`语法：class/struct CLASS_NAME{... friend R_TYPE FUN(...); ...friend class CLASS_NAME;};`  

## 四. 继承
&ensp;&ensp;&ensp;&ensp;继承是C++代码重用和功能扩展的重要机制<strong>子类（派生类）</strong>通过继承<strong>父类（超类、基类）</strong>来重用、实现和扩展父类的功能，并可多重继承，是<strong>自下而上</strong>的过程。  

&ensp;&ensp;&ensp;&ensp;`语法：class/struct CLASS_NAME : WAY CLASS_B, WAY CLASS_C, ...{...};`

&ensp;&ensp;&ensp;&ensp;WAY表示Public、Protected和Private继承方式，用于限制子类对父类成员的访问权限。

<table style="font-size:14; text-align:center; padding-left:26; width:763px;">

<tr>
<td rowspan=2>继承方式</td>
<td colspan=3>父类成员</td>
</tr>
<tr>
<td>public</td><td>protected</td><td>private</td>
</tr>
<tr>
<td>public</td><td>public</td><td>protected</td><td>不可见</td>
</tr>
<tr>
<td>Protected</td><td>protected</td><td>protected</td><td>不可见</td>
</tr>
<tr>
<td>Private</td><td>private</td><td>private</td><td>不可见</td>
</tr>
</table>
&ensp;&ensp;&ensp;&ensp;父类私有成员总是对子类不可见的，父类公有成员限制随继承方式改变，父类保护成员仅在私有继承时改变访问限制。在内存分配上，子类深拷贝了父类Public和Protected成员。  

&ensp;&ensp;&ensp;&ensp;构造、拷贝构造和析构函数不能被继承，子类构造对象时先调用父类（默认）构造函数，并可指定形参以调用父类重载的构造函数。子类拷贝对象时先调用父类拷贝构造函数、析构对象时反之。

&ensp;&ensp;&ensp;&ensp;<strong>1. 多继承与二义性</strong>  

&ensp;&ensp;&ensp;&ensp;一个子类可继承于多个父类（一般多继承），当不同的父类中拥有同名（和同参）成员，且子类可访问时，则可能产生二义性。消除一般多继承二义性的方法是子类重载或重写父类二义性成员或调用父类二义性成员时指明作用域“::”。  

&ensp;&ensp;&ensp;&ensp;<strong>2. 虚继承与二义性</strong>  

&ensp;&ensp;&ensp;&ensp;在一般多继承情形下，如果不同的父类继承于同一个超类（菱形多继承）且子类可访问时，则可能产生二义性。消除菱形多继承二义性也可通过消除一般多继承二义性的方法，或使用虚继承，即超类在派生父类时使用关键字“virtual”。

&ensp;&ensp;&ensp;&ensp;`语法：class/struct CLASS_PARENT : virtual WAY CLASS_Base, ...{...};`


## 五. 多态
&ensp;&ensp;&ensp;&ensp;C++继承衍生了多态特性，继承于同一父类的不同子类通过重写父类的<strong>（纯）虚函数</strong>来实现不同功能的方式称为多态，是**自下而上**的过程。  

&ensp;&ensp;&ensp;&ensp;<strong>1. 静态联编</strong>  

&ensp;&ensp;&ensp;&ensp;**函数重载**（和**操作符重载**）是实现静态联编的主要方式。在编译阶段，编译器会根据重载函数参数类型和个数的不同而生成不同的符号列表，从而形成可根据不同参数类型和个数来调用的同名函数。

&ensp;&ensp;&ensp;&ensp;&rarr;  函数重载  

&ensp;&ensp;&ensp;&ensp;在同一个作用域中声明的具有相同函数名而不同参数个数和（或）类型的函数，称为函数重载（与返回值无关）。  

&ensp;&ensp;&ensp;&ensp;附：默认形参与二义性  
&ensp;&ensp;&ensp;&ensp;C++允许在定义函数时指定形参默认值，顺序必须从右向左，调用函数时可以从右向左省略形参以使用默认形参。如果带有默认形参的函数又被重载，则调用时必须指定足够个数的参数以避免二义性。

&ensp;&ensp;&ensp;&ensp;`语法：R_TYPE FUN_NAME (Param1, Param2=N, Param3=M, ...){...};`

&ensp;&ensp;&ensp;&ensp;&rarr; 函数重写（覆盖）  

&ensp;&ensp;&ensp;&ensp;派生类会覆盖基类中与基类同名、同参函数，如需调用基类函数，则需使用作用域“::”。

&ensp;&ensp;&ensp;&ensp;&rarr; 运算符重载   

&ensp;&ensp;&ensp;&ensp;重载的运算符表现为由关键字<font color=purple>operator</font>指定的、以运算符为函数名的函数，重载的运算符必须是有效的C++运算符（附录D），参数个数必须与运算符操作数相等，<u style="paddin-bottom:20; border-bottom:20px">且至少一个属于复合类型（<font color=purple>class</font>/<font color=purple>struct</font>/<font color=purple>enum</font>）</u>。  

&ensp;&ensp;&ensp;&ensp;运算符将表达式中的第一个操作数作为第一个形参，第二个操作数作为第二个形参。操作符重载可以定义在类的内外，当在类中定义时，第一个形参省略，默认为所在类的类类型。运算符“=”必须在类中定义。

&ensp;&ensp;&ensp;&ensp;`语法：R_TYPE operator 操作符([class/struct/enum] NAME [OPT *指针, &引用, ...], ...){...};`

&ensp;&ensp;&ensp;&ensp;C++只检测形参的合法性，不检测函数内部实现机制，也不要求返回值，但重载的功能应该符合原运算符的语法规则、属性和优先级。合理的返回值设计也方便参与连续的链式运算。

&ensp;&ensp;&ensp;&ensp;<strong>2. 动态联编</strong>  

&ensp;&ensp;&ensp;&ensp;动态联编是C++多态的主要实现方式，它通过虚函数与虚类、纯虚函数与抽象类在运行而非编译时确定接口的实现。  

&ensp;&ensp;&ensp;&ensp;&rarr; 虚函数与虚基类  

&ensp;&ensp;&ensp;&ensp;将基类函数声明为虚函数，通过派生类的重写即可实现运行时多态，包含虚函数的类称为**虚基类**。声明成虚函数的意义在于对内存模型的建立（**虚表**），从而可用基类的指针通过赋值不同的派生类对象地址来实现不同的功能。

&ensp;&ensp;&ensp;&ensp;`语法：class/struct CLASS_NAME{public/protected: virtual R_TYPE FUN_NAME(...){...}; ...};`
 
&ensp;&ensp;&ensp;&ensp;构造、拷贝构造与析构函数是否声明为虚函数与虚函数表技术有关，参看附录E。

&ensp;&ensp;&ensp;&ensp;&rarr; 纯虚函数与抽象类

&ensp;&ensp;&ensp;&ensp;纯虚函数只有声明没有实现，也不能实例化对象，所有的实现都交给派生类，包含纯虚函数的类称为抽象类。抽象类的派生类必须实现抽象类中所有的纯虚函数，抽象类表现为一个行为接口。

&ensp;&ensp;&ensp;&ensp;`语法：class/struct CLASS_NAME{public/protected: virtual R_TYPE FUN_NAME(...)=0; ...};`


## 六. 模板
&ensp;&ensp;&ensp;&ensp;模板是C++泛型编程的基础，可以独立于任何特定变量类型的方式编写代码，包含函数模板和类模板。在设计函数和类时可以在任何关于变量类型的地方设置模板（T），在使用时可以指定任意类型变量以实现泛型编程。  

&ensp;&ensp;&ensp;&ensp;`语法：template <typename TYPE_NAME, ...> class CLASS_NAME{}; / ... FUN_NAME(...){}`  
&ensp;&ensp;&ensp;&ensp;`备注：模板函数的形参必须包含所定义的所有模板，不要求返回值。模板在类中可自由定义`  

&ensp;&ensp;&ensp;&ensp;<font size=4>**STL（Standard template library）**</font>   

&ensp;&ensp;&ensp;&ensp;标准模板库由HP实验室研发，现已成为C++标准。STL包含大量数据结构和算法的类模板，并将数据结构和算法分离。    

&ensp;&ensp;&ensp;&ensp;STL包含三种类型代码：容器（Containers）、算法（Algorithms）和迭代器（iterators）；容器是抽象数据类型的模板实现（模板类），即数据结构，如动态数组、列表、队栈、图等。算法用于操作容器（模板函数），如增、删、改、查等。迭代器（又称游标）用于遍历容器，如递增、递减、定位等。 
 
&ensp;&ensp;&ensp;&ensp;容器类中的方法是针对特定的数据结构而设计的，更多使用的是非成员方法（即算法）来操作容器。特别的，容器类中存在与算法同名的方法，原因是相比通用算法其执行更有效率。  

&ensp;&ensp;&ensp;&ensp;**模板使得算法独立于特定的数据类型，迭代器使得算法独立于使用的容器类型**。  
## 七. 异常
&ensp;&ensp;&ensp;&ensp;C++异常是处理程序错误的重要机制，不同于传统C的返回值判定、信号、断言、错误编号、跳转等处理方式，异常处理可获得更详细和精准的信息并将问题检测和问题处理分离，通过关键字$\color{#FF0000}{try}$检查异常、<font color=purple>throw</font>抛出异常、<font color=purple>catch</font>捕获异常，如果抛出异常后未进行捕获，则程序将被终止。

&ensp;&ensp;&ensp;&ensp;`语法：CODEBLOCK{throw [expression]}; try{CODEBLOCK}catch(exception param list){...}...catch(...){...}`  
  
&ensp;&ensp;&ensp;&ensp;<font color=purple>try-catch</font>语句可以（在<font color=purple>try</font>代码块中）嵌套，<font color=purple>catch</font>列表及外层<font color=purple>catch</font>列表中至少要有一个形参匹配所抛出的异常量类型，否则发生异常时程序将被终止。可在列表最后设置<font color=purple>catch(...)</font>以匹配任何异常类型。  

&ensp;&ensp;&ensp;&ensp;将嵌套内层捕获到的异常再抛出（到嵌套外层）的方法是再次利用<font color=purple>throw</font>语句，但表达式为空。 一般当嵌套内层无法处理某些异常时会利用该方法。  

&ensp;&ensp;&ensp;&ensp;`备注：throw语句将终止执行后续指令、expression可以是任何表达式语句`  

&ensp;&ensp;&ensp;&ensp; 在函数声明和定义时可使用关键词<font color=purple>noexcept</font>修饰以说明该函数不会抛出异常，如果该函数仍然抛出异常，则程序将被终止。这种机制可有效阻止异常的扩散。  

&ensp;&ensp;&ensp;&ensp;`语法：R_TYPE FUN_NAME(...) noexcept {...}`  

&ensp;&ensp;&ensp;&ensp;&rarr; 标准异常  

&ensp;&ensp;&ensp;&ensp;C++内置多个标准异常类，所有异常类都包含what()方法用以描述异常信息。其中exception是所有标准异常类的父类（详见附录F），可以通过继承标准异常来扩展异常。
  
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
## <article align=center>附录 D 不可重载运算符</article>

<table style="font-size:14; text-align:center; padding-left:26; width:763px;">
<tr>
<td>运算符</td><td style="wid">描述</td>
</tr>
<tr>
<td>.</td><td>成员访问运算符</td>
</tr>
<tr>
<td>.*, ->*</td><td>成员指针访问运算符</td>
</tr>
<tr>
<td>::</td><td>域运算符</td>
</tr>
<tr>
<td>sizeof</td><td>-</td>
</tr>
<tr>
<td>?:</td><td>-</td>
</tr>
<tr>
<td>#</td><td>预处理符号</td>
</tr>
</table>
<br>
## <article align=center>附录 E 虚函数表与指针</article>

&ensp;&ensp;&ensp;&ensp;编译器会为虚基类（和抽象类，下同）中的虚函数建立虚函数表（V-Table），虚函数表是一个数组，数组元素是按虚函数声明顺序排列的、指向各虚函数的指针（_vptr）。  

&ensp;&ensp;&ensp;&ensp;虚基类及其派生类都使用一个指针空间（前4Byte或8Byte）来存放虚函数表指针，虚基类每实例化一个对象，就为该对象添加一个虚函数表指针，对于多重继承的类就添加多个虚函数表指针。一个类的所有对象都共享同一个虚函数表。  

&ensp;&ensp;&ensp;&ensp;![V-Table](https://github.com/Jim-CodeHub/Skills-list/raw/master/image/V-Table.jpg)  

&ensp;&ensp;&ensp;&ensp;派生类继承虚基类的同时继承其虚函数表，派生类的虚函数指针列表存放于虚基类的虚函数指针列表之后，当派生类重写某个虚函数时，就将虚函数表中虚基类的虚函数地址覆盖掉。  

&ensp;&ensp;&ensp;&ensp;![V-Table-son](https://github.com/Jim-CodeHub/Skills-list/raw/master/image/V-Table-son.jpg)  

&ensp;&ensp;&ensp;&ensp;&rarr; 虚析构函数与内存泄漏  

&ensp;&ensp;&ensp;&ensp;当虚基类的指针指向派生类的对象实现多态时，销毁基类指针时系统只能调用基类析构函数而不会调用派生类的析构函数，从而可能产生内存泄漏。通过将基类析构函数声明为虚函数，在销毁基类指针时还会调用派生类的析构函数。  

&ensp;&ensp;&ensp;&ensp;&rarr; 虚基类成本分析 
 
&ensp;&ensp;&ensp;&ensp;TBD
