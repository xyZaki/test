
# 1 传感器

## 1.1 对射传感器 

对射传感器又叫透射传感器，因常被做成凹槽式所以又称凹槽传感器。对射传感器由发射器接收器组成，通过发射器的光线直接进入接收器，当它们之间的光线被阻断时光电开关就产生开关信号。对射传感器的特点是可检测不透明物体或低透明度物体。

## 1.2 反射传感器 

反射传感器也是由发射器和接收器组成，除此之外还可能有反射镜，发射器和接收器放置在一端，当有接近物体时或监测物体明暗变化时会通过漫反射（无反光镜）或镜面反射（有反光镜）使接收器接收光信号，从而触发开关信号。

要注意的是上述的光信号多指红外线，正常环境中也有一定量的红外线，所以接收器会检测红外线强度，进而判断是否触发开关信号。另外红外线也会被黑色物体吸收，或者说黑色物体是不反射或反射较少光线的，所以给待检测物体适当位置布置黑块也是一种反射传感器的检测方式。

## 1.3 常开与常闭

- 常开：常态下断路/断开/开路，信号/电平为0
- 常闭：常态下短路/闭合/闭路，信号/电平为1

如果传感器是运行决策设备，那么“不作为”通常不会使情况更糟，如果传感器损坏时的状态为A，则传感器要设置为A状态，并且执行机构执行时需要的决策应设计为!A。假设传感器损坏时为短路，呈高电平，则传感器设置为常闭，这样传感器未作决策和损坏时的状态相同，而执行机构执行时需要的决策是传感器为低电平，这样传感器损坏时使执行机构是“不作为”的，属于安全的设计。

如果传感器是精度检测设备，那么常开和常闭的选择没有很大的意义。

## 2 编码器

### 2.1 概念

编码器（encoder）是将光、磁或电刷等信号转换成电信号的用于角和线位移测量及反馈的设备，属运动控制类传感器。按信号读出方式分为接触式编码器和非接触式编码器，按工作原理分为增量式编码器和绝对式编码器。

### 2.2 增量式编码器

增量式编码器是将位移转换成周期性的电信号，再把这个电信号转变成计数脉冲，用脉冲的个数表示位移的大小。

### 2.2.1 光电式增量编码器

光电式增量编码器利用光电转换原理输出A、B、Z或A、B、C、D、Z五组方波，除Z相外，其它各相的相位相差90°，通过相位差即可判定旋转方向，通过对任一项计数即可测量或反馈角位移或线位移。
带有C和D相的增量式编码器常用于伺服系统，将C和D相信号反向，叠加到A和B相中可增强信号的稳定性。Z相在编码器每旋转一圈时（在特定位置）输出一个脉冲，用于基准点定位。另外部分增量式编码器还具有A-、B-、（C-、D-）Z-相，形成差分对形式输出信号以增强抗干扰能力。

![增量式编码器](https://github.com/Jim-CodeHub/Skills-list/raw/master/image/Sensor/Incre-encoder.png)

增量式编码器机械寿命长、抗干扰、可靠性高，适合长距离传输。缺点是没有转轴的绝对位置信息。最高精度可达10000P/R（脉冲/转）。

### 2.2.2 编码器开关

编码器开关又称为数字电位器，精度一般为10~20P/R，常作为功率调节器件、广泛应用于智通家居、多媒体音响、仪器仪表、家用电器等设备。

![编码器开关](https://github.com/Jim-CodeHub/Skills-list/raw/master/image/Sensor/Encoder-SW.png)

通过调节编码器开关旋钮即可输出相位，通过程序计数读取和方向判定即可输出决策信号以控制输出系统。

### 2.3 绝对式编码器

绝对式编码器的每一个位置对应一个确定的数字码，因此它的示值只与测量的起始和终止位置有关，而与测量的中间过程无关。
绝对式编码器的码盘均分为若干弧段，每个弧段拥有同样数量的扇区，同一弧段的扇区进行二进制编码器以形成该扇区的码值，这样就可以输出转轴的绝对位置。格雷码码盘（又称循环码盘）设计为相邻弧段之间只相差一个扇区，使得读出误差减小。

![绝对式编码器](https://github.com/Jim-CodeHub/Skills-list/raw/master/image/Sensor/ABS-Encoder.png) <br> <center> <font color=gray> IPC standards tree </font> </center> <br>

如图是精度为4位的绝对式编码器，共2^4=16个弧段，位数越高、弧段越多，精度越高，目前有16位的绝对式编码器。

### 2.4 电路连接（增量式）

增量式编码器有A、B或A、B、C、D相位各差90°的脉冲输出线，可单向和双向计数，各相接线可连接至单片机外部中断或GPIO（中间可能需要滤波、退耦和放大等电路），单向计数时任选一相接口即可。





开环与闭环
开环系统与闭环系统最重要的区别是反馈系统。
	开环系统是没有反馈系统的，其控制机构发出命令后，执行机构开始执行，而执行机构的结果是未知的。比如步进电机
	闭环系统拥有反馈系统，其控制机构发出命令后，执行机构开始执行，并有监测机构检测并反馈执行结果告知控制机构执行机构的执行状态，进而以此为依据进行进一步调整。比如伺服电机

	动力系统+反馈系统 = 闭环。如果给步进电机的执行机构在适当位置放置（高精度）编码器，那么这就是一个闭环，也就是常说的"伺服系统“



