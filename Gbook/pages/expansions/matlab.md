附A：微积分基础

一.历史碎片
微积分学是古老的课题，古希腊的泰勒斯、阿基米德的相关著作中就已经有微积分的萌芽，三国时期的刘徽也有割圆术和求体积的思想出现。早在庄子的时代，就已经有了极限的概念“一尺之棰，日取其半，万世不竭”。
到了十七世纪，有许多问题需要解决，这些问题也就促成了微积分的产生因素。这些问题可归结为4类：第一类是求运动物体的即时速度、第二类是求曲线切线、第三类是求函数最大最小值、第四类是求曲线及曲面面积。
任何伟大的发明都要站在巨人的肩膀上，微积分的产生与发展经历了上千年，是许许多多数学家的贡献，到了十七世纪，出现了两位集大成者：牛顿、莱布尼茨。他们最主要的贡献是将一直独立的微分学（切线问题）和积分学（求积问题）联系到了一起，甚至他们所创建的微积分符号一直沿用至今。
科学上的巨大需要战胜了逻辑上的顾忌。他们需要做的事情太多了，他们急于去攫取新的成果，基本问题只好先放一放。所以在微积分一方面被快速投入应用，另一方面出现了越来越多的悖论，中心问题是无穷小量的问题，这引发了第二次数学危机，直到19世纪通过极限的概念才得以解决。
二.内容分支
微积分的基本内容包括微分学和积分学，微分学的主要内容包括“极限、导数、微分”，积分学的主要内容包括“定积分、不定积分”。现在，微积分已成了数学分析的代名词。

三.常用符号
数学当中的符号要从古希腊和罗马帝国说起，罗马文明是古希腊文明的延伸，罗马帝国曾统治亚欧非大陆长达一千多年，所以其文明影响着亚欧非大部分地区。罗马帝国以拉丁文为母语并传承者古希腊文明，而数学贡献多源于西方，他们以拉丁文、古希腊字母作为数学符号，这样即不会与流通语言产生冲突，又达到了可查找、可辨识的效果。
微分符号：dx, dy 其中d源自拉丁语“Differentia”（差）；积分符号：ʃ 来源于拉丁语“Summa”（总和）。
四.运算
//TBD
五.应用
//TBD




附B：MATLAB

一．简介
MATLAB 是美国MathWorks公司出品的商业数学软件，用于算法开发、数据可视化、数据分析以及数值计算的高级技术计算语言和交互式环境，主要包括MATLAB和Simulink两大部分。MATLAB技术代表了当今科学计算软件的先进水平。
二．自动控制与MATLAB
MATLAB内置众多模块化工具，其中包括模糊逻辑工具箱和PID工具，通过simmulink模块还可以查看仿真图示，观察不同参数下的图像运行轨迹，通过工具的使用可以更清楚的了解PID等算法的运行机制。
三．MATLAB的使用
//TBD


-------

```
Matlab 命令行绘图

将数学公式输入到MATLAB，即可绘图，展现该公式的图像。

打开MATLAB即进入命令行模式

先输入X的值：

>>X=0:0.1:5

表示X的值从0到5，步长为0.1 ,
		即X轴的值为(5-0)*0.1 => 0、0.5、1、1.5 。。。

>>Y=2*X

输入公式

>>plot(x,y,'-r')

plot是matlab内置函数（英文意思为绘图），其中-r代表图像线条颜色即红色
再次回车即可看到图像。

需要注意 .* 与 * ， ./ 与 / 的区别，前者是矩阵内容的组合乘法，后者是矩阵本身相乘
或者说.* ./才是相当于编程使用中的乘除法。
```

---
### 如何保持图（将多个曲线画在一张图中）

plot(x,y,'-r')
hold on //每次画图前，调用一次即可
plot(x1,y1,'-g')
plot(x2,y2,'-b')
...

### 如何标注曲线

legend('曲线1的标注', '曲线2的标注','曲线3的标注');

