# MakeCode编程：制作计数器与转速测量(简单)

## 1.micro:bit 按钮计数器

在MakeCode的原始积木中，有一个"forever"积木，其内部的程序会在"on start"积木内的程序执行完后在后台无限循环执行，若"on start"内程序还未执行完成或是有"while true"积木使程序进入while循环，则不会执行"forever"积木，而在每一次循环的间隙，都允许运行其他的事件处理程序，例如"on button A pressed"积木，该积木在“按钮A被按过一次”的事件发生时运行一次。

我们可以利用"forever"与"on button pressed"积木做出一个简单的按钮计数器。

<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode24.png" width="50%"></div>

这其中设置了一个"A_was_pressed_times"变量，而"change A_was_pressed_times by 1"积木则是使该变量自加1。按下一次按钮A，变量自加1，然后一直循环显示变量值，即记录按钮A按下次数并显示在LED矩阵上。

