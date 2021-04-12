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

