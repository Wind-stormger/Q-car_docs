# MakeCode编程：制作计数器与转速测量

在MakeCode的原始积木中有判断micro:bit的AB俩按钮是否被按下的积木"button A is pressed"，但没有记录按钮被按下多少次的积木，我们可以自己设计一套程序来实现这样的“计数器”功能。

我们可以先拆分出为了达成“计数”的目的而需要完成的每一个动作与状态。
1.在按钮还没有被按下之前，按钮的状态是“未被按下”
2.在按钮被按下的那一刻，按钮的状态从“未被按下”改变为“被按下”
3.在按钮被松开回弹的那一刻，完成了“按下过一次按钮”的过程，此时计数值加1，按钮的状态从“被按下”变更回“未被按下”

这其中最难以理解也可能被惯性思维所忽略的细节是按钮的状态改变，在设计程序的时候，如果仅仅是用"if"条件积木判断按钮被按下或未被按下，那么在循环条件判断的过程中，假设“if”判断到按钮被按下就使计数值加1，在按钮被松开回弹之前，循环运行的条件判断会反复让计数值加1（循环的速度取决于硬件设备的运算速度）。

所以我们需要在“if”条件判断中加入一个记录按钮状态的变量，当满足按钮被按下且之前记录按钮状态的变量为“未被按下”的条件时，改变记录按钮状态的变量为“被按下”，当满足按钮未被按下即松开回弹且之前记录按钮状态的变量为“被按下”的条件时，使计数值加1，改变记录按钮状态的变量为“未被按下”，这样在循环运行的条件判断中，计数值仅在完成了“按下过一次按钮”这一过程后才会加1，即可用这样的思路设计记录按钮A和按钮B各自被按下过的次数的“计数器”。
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode14.png" width="100%"></div>
这其中最外层的“function”积木为自定义函数积木，函数名可以自由定义，放在其中的积木只会在该函数积木被主程序调用了才会执行，这有利于区分程序各个功能模块，方便多次调用而无需重复编写，并能极大提升程序易读性和可维护性，方便对某一函数进行调试，修改。

如果想调用这个"Counter"函数，就在主程序中加入"call Counter"来使用，这很像是一个扩展积木的使用方法。

<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode15.png" width="100%"></div>

“while true”循环内的积木会无限循环执行，所以"Counter"函数会被循环执行实现持续计数的功能。

"serial write number"积木可以向USB串口传输数据，将micro:bit通过USB线与电脑连接，即可在MakeCode的控制台或是其他串口工具上读取到数据。

"running time(ms)"积木会获取程序从启动或复位以来至读取这个积木为止的时间，单位为毫秒，即获取从开机到现在所经过的时间。我们设计一个if条件判断，用和前文类似的方法，加入一个变量用于储存上次满足条件的时间，