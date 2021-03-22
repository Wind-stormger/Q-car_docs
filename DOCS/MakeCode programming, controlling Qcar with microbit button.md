# MakeCode编程：用micro:bit的按钮控制Q-Car

MakeCode图形编程对初学者是极其友好的，相较于（code）代码这样由抽象的字符组合来表达程序逻辑，图形化的（blocks）积木非常利于人从视觉上认知和了解程序的基本逻辑，这可以很大程度上减少人在接触编程时直觉感受到的阻碍，低门槛的入门加上平滑的难度进阶，再过渡到代码编程时就能自然的以从图形编程中学会的知识为逻辑基础，即可有效降低抵触心理，增强自我学习的信心。  

现在我们打开MakeCode，安装上Q-Car扩展积木，即可开始编写用于控制Qcar的程序。  
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode2.png" width="100%"></div>

在basic选项栏中，有一个“on start”积木，这是一个在主程序刚启动或复位时运行的事件处理程序，即启动和复位时就会立即执行它内部的程序。
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode4.png" width="25%"></div>

在input选项栏中，有一个“on button A pressed”积木，这是一个(event handler)事件处理程序，当指定的事件（按钮A被按过）发生时，就执行它内部的程序。
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode5.png" width="25%"></div>

将想要用的积木拖拽出来放置在编辑区即可使用。例如：
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/screenshot-makecode3.png" width="50%"></div>

从上图中可以很轻易了解程序中实现了什么功能，主程序刚启动或复位时，初始化Q-Car的电机（init the motor），有三个事件处理程序，在按钮A按下后，让Q-Car前进（Let the Q-Car go foward），在按钮B按下后，让Q-Car后退（Let the Q-Car go back），在按钮AB同时被按下后，让Q-Car停下（Let the Q-Car stop）。

没有任何（code）代码编程经验的人也能轻易通过积木之间的嵌套关系以及积木上直白的功能描述了解整个程序的逻辑。

将程序下载到micro:bit里,插入Q-Car插槽，接通电源，即可实现对应程序功能。
<div align=center><img src="https://raw.githubusercontent.com/Wind-stormger/Q-car_docs/main/DOCS/picture/Q-car_side_view.jpg" width="75%"></div>
