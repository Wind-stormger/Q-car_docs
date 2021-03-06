# MakeCode编程：制作计数器与转速测量(复杂)

## 1.micro:bit 按钮计数器
在MakeCode的原始积木中有判断micro:bit的AB俩按钮是否被按下的积木"button A is pressed"，但没有记录按钮被按下多少次的积木，我们可以自己设计一套程序来实现这样的“计数器”功能。

我们可以先拆分出为了达成“计数”的目的而需要完成的每一个动作与状态。
1. 在按钮还没有被按下之前，按钮的状态是“未被按下”
2. 在按钮被按下的那一刻，按钮的状态从“未被按下”改变为“被按下”
3. 在按钮被松开回弹的那一刻，按钮的状态从“被按下”变更回“未被按下”，完成了“按下过一次按钮”的过程，此时计数值加1

这其中最难以理解也可能被惯性思维所忽略的细节是按钮的状态改变，在设计程序的时候，如果仅仅是用"if"条件积木判断按钮被按下或未被按下，那么在循环条件判断的过程中，假设“if”判断到按钮被按下就使计数值加1，在按钮被松开回弹之前，循环运行的条件判断会反复让计数值加1（循环的速度取决于硬件设备的运算速度）。

所以我们需要在“if”条件判断中加入一个记录按钮状态的变量，当满足按钮被按下且之前记录按钮状态的变量为“未被按下”的条件时，改变记录按钮状态的变量为“被按下”，当满足按钮未被按下即松开回弹且之前记录按钮状态的变量为“被按下”的条件时，使计数值加1，改变记录按钮状态的变量为“未被按下”，这样在循环运行的条件判断中，计数值仅在完成了“按下过一次按钮”这一过程后才会加1，即可用这样的思路设计记录按钮A和按钮B各自被按下过的次数的“计数器”。
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode14.png" width="100%"></div>
这其中最外层的“function”积木为自定义函数积木，函数名可以自由定义，放在其中的积木只会在该函数积木被主程序调用了才会执行，这有利于区分程序各个功能模块，方便多次调用而无需重复编写，并能极大提升程序易读性和可维护性，方便对某一函数进行调试，修改。

如果想调用这个"Counter"函数，就在主程序中加入"call Counter"来使用，这很像是一个扩展积木的使用方法。

<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode15.png" width="100%"></div>

"while true"循环内的积木会无限循环执行，所以"Counter"函数会被循环执行实现持续计数的功能。

"serial write number"积木可以向USB串口传输数据，将micro:bit通过USB线与电脑连接，即可在MakeCode的控制台或是其他串口工具上读取到数据。在其中添加AB按钮的计数值的变量即可实时在控制台看到各自的计数值。

"running time(ms)"积木会获取程序从启动或复位以来至读取这个积木为止的时间，单位为毫秒，即获取从开机到现在所经过的时间。我们设计一个if条件判断，用和前文类似的方法，加入一个变量用于储存上次满足条件的时间，然后设置判断条件为当前时间减去上次满足条件时的时间大于等于100毫秒时执行一次内部积木，向串口输出AB按钮的计数值然后使变量储存此次满足条件的时间，在"while true"循环内无限循环执行条件判断实现间隔100ms向串口输出一次计数值的功能。

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

Q-Car的扩展积木中有一个"Set the infrared status to (on)"积木可以控制Q-Car上的红外对管启动或关闭。  
在MakeCode中，我们先调用"Set the infrared status to (on)"积木启动红外对管，然后将前文中的用于判断按钮A是否被按下的"if"条件判断放置在"while"循环中：
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode16.png" width="100%"></div>
这其中串口输出改为了"serial wite line"积木，可以输出文本和数字，在"while"循环中间隔100ms输出一次"A_was_pressed"变量的状态"true"或"false"。
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/Q-car_wheel_3.jpg" width="100%"></div>
将程序下载进micro:bit，然后保持USB串口连接，将micro:bit插入Q-Car的插槽，旋转右轮使编码盘的凹口对向红外对管，此时检查串口输出信息，可以看到"false"
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/Q-car_wheel_4.jpg" width="100%"></div>
旋转右轮使编码盘的凸柱对向红外对管，此时检查串口输出信息，可以看到"true"  

## 3.Q-Car车轮的编码盘计数器
显然，我们可以利用红外对管对左右两轮的编码盘进行计数了。但是，当快速转动车轮的时候，会发现串口输出的"A_was_pressed"变量的状态不发生改变了。这是因为"button A is pressed"积木内有一个消除抖动的功能，当检测到按钮A被按下超过一定时间程序才会判定按钮A被按下，而车轮带动编码盘快速旋转时，红外对管所输出的高电平信号持续时间会低于消抖的判断时间，就无法被"button A is pressed"积木识别为"true"了。

所以为了使程序在车轮快速转动时也能正常计数，根据前文所述的P5,P11引脚和A,B按钮的关系，我们可以将计数器程序里的积木做出相应的修改：
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode17.png" width="100%"></div>
"digital read pin P5"积木即读取P5引脚的数字量信号0或1，对应其低电平或高电平。而"on start"内的主程序则可以无需改动，这也正是"function"函数积木的好处。
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode15.png" width="100%"></div>

## 4.转速测量
我们先简单的陈述转速的定义，即物体在单位时间内旋转360°的次数。RPM是一个较常用的转速单位，表示设备每分钟的旋转次数。

我们从直觉上理解，通过计数器统计1分钟Q-Car车轮的编码盘计数值除以12即可得到RPM转速值，但这样显然得到的信息时效性滞后了太多。如果将统计时间缩短到5秒甚至1秒，例如统计1秒Q-Car车轮的编码盘计数值除以12再乘以60，也可得到RPM转速值。
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode20.png" width="100%"></div>
"running time(ms)"积木的使用方法和前文中计数器间隔100ms输出一次计数值的方法一致，只需改变间隔时间为1000ms即可。

将"call RPM_Measurement_1"积木置入"while true"循环内即可实现间隔1s 从USB串口输出一次RPM转速值。
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode21.png" width="50%"></div>
条条道路通罗马，测速的方法当然不止这一种，稍微改变一下思路，记录下通过编码盘12个凸柱即车轮旋转一周所花费的时间，60秒(1分钟)除以这个时间，即为RPM转速值。
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode22.png" width="100%"></div>
将"call RPM_Measurement_2"积木置入"while true"循环内即可实现车轮每旋转一周后从USB串口输出一次RPM转速值。
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode23.png" width="50%"></div>
