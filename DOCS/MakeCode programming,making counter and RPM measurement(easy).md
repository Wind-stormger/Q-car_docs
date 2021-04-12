# MakeCode编程：制作计数器与转速测量(简单)

## 1.micro:bit 按钮计数器

在MakeCode的原始积木中，有一个"forever"积木，其内部的程序会在"on start"积木内的程序执行完后在后台无限循环执行，若"on start"内程序还未执行完成或是有"while true"积木使程序进入while循环，则不会执行"forever"积木，而在每一次循环的间隙，都允许运行其他的事件处理程序，例如"on button A pressed"积木，该积木在“按钮A被按过一次”的事件发生时运行一次。

我们可以利用"forever"与"on button pressed"积木做出一个简单的按钮计数器。

<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode24.png" width="50%"></div>

这其中设置了一个"A_was_pressed_times"变量，而"change A_was_pressed_times by 1"积木则是使该变量自加1。按下一次按钮A，变量自加1，然后一直循环显示变量值，即记录按钮A按下次数并显示在LED矩阵上。

## 2.Q-Car的车轮与红外对管
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/Q-car_wheel_1.jpg" width="100%"></div>
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/Q-car_wheel_2.jpg" width="100%"></div>
在Q-Car上有一对D字轴橡胶车轮，带有12线编码盘，在底部紧贴编码盘的位置放置了红外对管用于采集编码盘的信息。
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/Infrared_tube_pair.png" width="25%"></div>
红外对管一侧发射红外光，一侧接收红外光，接收端将反射回的光线强弱的信息转化为电压高低的信号。
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode18.png" width="50%"></div>
与右车轮的编码盘紧贴的红外对管与micro:bit的P5引脚相连，P5引脚高电平时等效于按钮A被按下，低电平等效于按钮A被松开。
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode19.png" width="50%"></div>
左轮的编码盘紧贴的红外对管与micro:bit的P11引脚相连，P11引脚高电平时等效于按钮B被按下，低电平等效于按钮B被松开。  

以右轮为例，可以制作一个测试程序，检测右轮编码盘对应的红外对管向P5引脚输出高低电平信号。
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode25.png" width="75%"></div>

<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/Q-car_wheel_3.jpg" width="75%"></div>
将程序下载进micro:bit，然后保持USB串口连接，将micro:bit插入Q-Car的插槽，旋转右轮使编码盘的凹口对向红外对管，此时micro:bit显示0，即表明此时红外对管输出给P5引脚的是低电平信号
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/Q-car_wheel_4.jpg" width="75%"></div>
旋转右轮使编码盘的凸柱对向红外对管，此时micro:bit显示1，即表明此时红外对管输出给P5引脚的是高电平信号

## 3.Q-Car车轮的光电计数器

显然，我们可以利用红外对管对左右两轮的编码盘进行计数了。

以右轮为例，参照micro:bit按钮计数器即可设计出Q-Car车轮上的编码盘与红外对管组合的光电计数器。
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode26.png" width="100%"></div>

我们可以加入一个间隔一段时间输出一次内容的功能，如上图所示。"running time(ms)"积木会获取程序从启动或复位以来至读取这个积木为止的时间，单位为毫秒，即获取从开机到现在所经过的时间。在"forever"积木中加入"if"条件积木，设置一个变量"A_was_pressed_times"用于储存每当条件满足时的时间数值，判断条件为当前时间减去变量大于等于1000ms时，显示一次计数值，将此次满足条件的时间数值储存到变量中用于下次判断，即实现了间隔1s让micro:bit显示一次计数值。

## 4.车轮转速测量
我们先简单的陈述转速的定义，即物体在单位时间内旋转360°的次数。RPM是一个较常用的转速单位，表示设备每分钟的旋转次数。

我们从直觉上理解，通过计数器统计1分钟Q-Car车轮的编码盘计数值除以12即可得到RPM转速值，但这样显然得到的信息时效性滞后了太多。如果将统计时间缩短到5秒甚至1秒，例如统计1秒Q-Car右轮的编码盘计数值除以12再乘以60，也可得到右轮RPM转速值。
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode27.png" width="100%"></div>
条条道路通罗马，测速的方法当然不止这一种，稍微改变一下思路，记录下通过编码盘12个凸柱即车轮旋转一周所花费的时间，60秒(1分钟)除以这个时间，即为RPM转速值。
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode28.png" width="100%"></div>
其中在给"Right_whell_speed"变量赋值时将计算所得值通过"round"积木取整，这样方便LED显示出整数而不会显示一长串小数。