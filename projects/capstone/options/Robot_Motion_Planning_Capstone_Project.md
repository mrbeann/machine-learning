机器人运动规划毕业项目

规划并通过一个虚拟迷宫

# 项目描述

这个项目是从[Micromouse](https://en.wikipedia.org/wiki/Micromouse)竞赛中获得的灵感，一只机器人老鼠得到一个任务，规划一条路径，从这个迷宫的一个角落到达中央。这只机器人老鼠可以在给定的迷宫中运行多次。第一次运行时候，机器人老鼠设法画出迷宫的地图，不只是要到达中央位置，还要找出到达中央的最佳路径。在之后的几次运行中，机器人老鼠尝试着用之前学到的路线，在尽可能最快的时间内到达中央。[这个视频](https://www.youtube.com/watch?v%3D0JCsRpcrk3s) (Youtube) 是一个Micromouse竞赛的例子。在这个项目中，你要创建函数来控制一个虚拟机器人来通过一个虚拟迷宫。除了迷宫和机器人的规格，还会提供一个简化的世界模型；你的目标是在一系列测试迷宫中获得尽可能最快的时间。

# 项目要求

### 要求

*   Python 2.7.X
*   Numpy

### 交付成果

你的提交物中应该包含下列文件：

*   一个PDF报告，关于对问题所采用的算法和方法（见下）
*   测试虚拟机器人的Python脚本(如robot.py或者其他创建的脚本)
*   一个README文件，标题是“规划并通过一个虚拟迷宫”，正文中包含所有提交的文件列表。

以单一的压缩包（如.zip文件）提交所有要求的文件以供评分。

# 环境要求
### 迷宫规格

迷宫以一个n x n方格的网格的形式存在，n是偶数。n的最小值是12，最大值是16。沿着网格外围的，以及边上连接起若干内部方格的，是墙，会阻挡所有移动。机器人会从网格左下角的方格起步，初始方向为向上。起始方格总会有一堵墙在它的右侧（除了左侧和底部的外墙之外的），在上方还有一个开口。在网格的中央是一个2 x 2方格的目标房间；机器人必须从起始方格到达这里，才会记作一次成功的迷宫运行。

迷宫以文本文件形式提供给系统。在文本文件的第一行是一个数字，描述了迷宫每个维度的方格个数n。在接下来的n行中，会有n个逗号分隔的数字，描述了方格的哪条边是可以通过的。每个数表示一个4bit的数字，如果一条边是封闭的（墙），则该bit的值是0，如果一条边是开放的（没墙），则该bit的值为1；1的那位（最低位）与上边关联，2的位是右边，4的位是底边，8的位是左边。例如，数字10表示一个方格的左边和右边是开放的，而上边和底边有墙（0*1 + 1*2 + 0*4 + 1*8 = 10）。注意，根据数组的索引，文本文件中第一行数据与迷宫最左边的列关联，第一个元素是起始方格（迷宫的左下角）。

![maze_example.png](http://cn-static.udacity.com/mlnd/capstone/MLNDCapstoneProjectDescription-RobotMotionPlanning/images/image00.png)

### 机器人规格

机器人可以被认为处于它所在方格的中间，面向迷宫的四个基本方向之一。机器人有三个障碍感知器，分别装在机器人的前部、它的右侧及它的左侧。障碍感知器探测感知器方向上的开放方格数目；例如，在它的起始位置，机器人左侧和右侧的感知器会表明在那两个方向上没有开放方格，而前方至少有一个开放方格。在模拟器上的每一步，机器人会选择顺时针旋转或者逆时针旋转90度，然后向前或向后一段距离，最远为三个单位。假设机器人的转弯和移动是完美无误的。如果机器人将要移动进撞到一堵墙中，机器人会留在原地。移动之后，则经过了一步，感知器返回机器人新的位置和/或方向上的开放方格的读数，然后开始下一个时间单元。

更技术性地讲，在每一步的开始，机器人会收到感知器的读数，读数是一个三个数字的列表，数字是左侧、中间、右侧（按此顺序）的前方开放方格数目，传到“next_move”函数。然后“next_move”函数必须返回两个值，表示机器人在这一步的旋转和移动。旋转是一个整数，取三个值：-90,90或0中的一个，分别表示逆时针，顺时针和没有旋转。旋转之后是移动，为一个在[-3,3]之间（含）的整数。机器人会尝试移动多个方格，前进（正数）或后退（负数），或当碰到墙时停止移动。

### 得分

每个迷宫中，机器人必须运行两次。第一次运行中，允许机器人自由漫游迷宫来创建迷宫的地图。它必须在探测中从某个点进入目标房间，但在找到目标后，它仍然可以自由地继续探索迷宫。在进入目标房间后，机器人可以选择在任何时间结束探索。然后机器人移动回到起始位置和起始方向，准备第二次运行。现在它的目的是在尽可能最快的时间内从起始位置到达目标房间。机器人的迷宫得分等于执行第二次运行所用的步数，加上执行第一次运行所用步数的1/30。最大允许每个迷宫两次运行总共用一千步。

## 项目流程

### 任务规格

你可以从[这里](https://github.com/nd009/machine-learning/tree/zh-cn/projects/capstone/open_projects/robot_motion_planning)找到项目的初始代码。
项目的初始代码包括下列文件：

*   robot.py - 这个脚本创建了机器人的类。这是唯一的要你改动的脚本，也是你需要提交项目的主要脚本。
*   maze.py - 这个脚本包括了构建迷宫和用于机器人的移动和感知的墙检测的函数。
*   tester.py - 这个脚本用来测试机器人通过迷宫的能力
*   showmaze.py - 这个脚本可用于创建迷宫形状的可视化展示
*   test_maze_##.txt - 这些文件给出用于测试你的机器人的3个样例迷宫。请根据上述的规格，随意创建你自己的迷宫。

要运行测试程序，你可以从命令行中输入命令，如下：python tester.py test_maze_01.txt。你无须改动maze.py和tester.py；你需要改动的脚本是robot.py。如果你创建了额外的脚本，确保你在提交项目中包括了它们。

迷宫的可视化也遵从类似的语法，如 python showmaze.py test_maze_01.txt 。这个脚本使用海龟模块来显示迷宫；作画完毕后，你可以点击显示窗口来关闭窗口。你可以随意修改showmaze.py，但这不会被看到，不会被用来评审你的项目。

### 报告结构

作为项目提交的一部分，除了你的Python脚本，你要提供一份.pdf文档，记录你的项目，从开始到结束。你的项目文档应该遵照在[毕业项目](https://www.udacity.com/course/viewer%23!/c-nd009/l-5420178917/m-5479829792)和 [项目模板](https://docs.google.com/document/d/1B-vEOscvfqctGEMHTFDS9Nw7aqcE2iuwPRfp0jK8nf4/pub?embedded%3Dtrue)中提到的结构。因为项目的特性，有几段需要特别对应。对于这些段的指导方针如下：

1.  数据探索：用机器人规格的段来探讨机器人如何解析并探索它所在的环境。此外，应该比较详细地讨论三个给定的迷宫中的一个，比如一些引人注意的结构上的发现，你发现的（在若干步内）到达目标的可行方案。如果可能的话，尽量针对优化的路径！
2.  探索可视化：这段须要联系上一段，在其中你应该通过showmaze.py文件，给出三个样例迷宫其中之一的显示结果。在这个迷宫中，你在数据探索部分的说明应符合显示部分的表述。
3.  基准测试：你需要决定你认为合理的一个基准得分，以此你可以用来比较你的机器人结果。考虑上面的可视化和数据的探索：如果允许你用一千步来探索（第一次运行）和测试（第二次运行），给定在这个项目中定义的度量标准，你认为什么是合理的得分？这里没有正确或错误的答案，但这在之后的项目方案讨论中会对你有帮助。
4.  数据处理：因为这个项目中不需要数据预处理（感知器规格和环境设计已提供给你），确保提及不需要数据预处理以及为什么。
5.  自由形式的可视化：利用这段来提出你自己的迷宫。你的迷宫应该有同样的尺寸（12x12，14x14或16x16），在与三个样例迷宫同样的位置有目标和起始位置（你可以用test_maze_01.txt作为模板）。尽量创建出你觉得能体现你的机器人算法鲁棒性的设计，或者机器人实现中用到的方法能来放大潜在的问题。也请给出一个关于迷宫的一个简单讨论。
6. 改进：考虑发生在连续域的情况。比如，每个方格有一个单位长度，墙是0.1个单位厚，机器人是一个直径为0.4单位的圆。你的机器人代码上需要什么改动才能处理这增加的复杂度？在连续域内，有没有什么样的迷宫类型是不能在离散域里解决的？如果关于现在的项目，你还有其他扩展的想法，可以在这里描述并讨论。

确保在你的文档顶部附近，清晰表明你的提交的是“规划并通过一个虚拟迷宫”项目，因为微学位的毕业项目很多选择。

## 资源和链接

### 支持课程
[机器人人工智能 (cs373)](https://cn.udacity.com/course/artificial-intelligence-for-robotics--cs373)
[Artificial Intelligence for Robotics (cs373)](https://cn.udacity.com/course/artificial-intelligence-for-robotics--cs373)

### 更多链接

[APEC Micromouse Contest Rules 2015](http://www.apec-conf.org/wp-content/uploads/2013/10/APEC_2015_Micromouse_Contest_Rules.pdf)