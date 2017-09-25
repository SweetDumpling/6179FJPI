
# 6179 FRC Java Programming Introduction


> 触摸机器人的灵魂


### 前言

此教程是一个分支版本，编写意图有所不同。这是一个更纯粹的教程，以能够让读者快速上手为目的。由于编写时间不同，本文可能会更加全面深入，并且会有大量官方文档上提及较少的功能，以及部分的WPILibJ内部源码的解析。

本文可能会有大量错误，建议仅作为参考，实际情况以实际情况为准:)。一定要看官方文档，本文可以作为兴趣的激发，但绝不是官方文档的替代！可信度: 官方文档＞master分支教程＞本教程。

以下是几条建议：
- 善用[**搜索引擎**](http://www.google.com.hk/)
，善用[**官网**](https://www.firstinspires.org/)
，善用[**官方manaul**](https://wpilib.screenstepslive.com/s/4485)
，善用**例程**，善用[**API**](http://first.wpi.edu/FRC/roborio/release/docs/java/)
。
- 语言**基础要扎实**，面向对象的思想要明白。
- **细节决定成败**。实例化谁先谁后？正负号？某些未考虑的特殊情况？算法效率？可读性？
- 永远不要立flag认为自己的程序绝对没有问题。
- 代码要**备份**！
- 代码要**备份**！
- 代码要**备份**！（最好要用版本控制工具）

### 开始
本教程默认你已经根据官方文档安装好了Driver Station和Eclipse的FRC插件，并且拥有一定Java和面向对象基础，会使用Eclipse IDE和Github。以下几个名词如果你还不清楚，请自学Java的面向对象：

  `类`  `实例 `   `继承` `重写` `重载` `抽象` `接口`  `向上转型` 等。


### 比赛流程
要编写FRC程序，首先要了解FRC究竟是怎样的一个比赛。比赛的整个流程是怎样的？这样才能知道编写程序所要完成的目的。以下是问题清单：
- 一场比赛分为哪几个阶段？
- 各个阶段有什么特征？不同点在哪里？
- 你觉得每个阶段的关键在哪里？

>机器人在赛场上的任务每年都在变化。在多数情况下，任务包括由机器人通过预先编好的程序进行动作的10-15秒的自动操作阶段，以及随后的由机器人操作员控制完成的大约2分钟的手动控制阶段。（百度百科）

自动阶段底盘控制较重要，手动阶段Joystick与上层机构的配合较重要。但如果队伍实力变强了，这些或许会发生变化。

更多信息请参考当年的官方规则手册。

### 控制对象
要开始学习控制程序，得先了解所要控制的对象，也就是硬件部分。以下是问题清单：

- Motor有哪几种？SpeedController有哪几种？它们之间的关系？
- Servo是做什么用的？
- 气动有哪些控制对象？它们之间的关系是怎样的？
- Gyro是什么？Encoder是什么？加速度计是什么？还有什么其它传感器？
- Joystick是用来做什么的？它有哪些输入？它连接到哪里？
- RoboRIO上的各个接口是用来做什么的？
- SmartDashboard是做什么用的？

由于本教程面向FRC编程，硬件部分将会较少提及。但硬件对于FRC每个部分都是极其重要的，你们可以向老队员学习或者看官方文档的介绍，也可以通过搜索引擎和百科类网站了解。这里仅介绍程序与这些硬件的关系。

Motor是间接被程序控制的，你们可以尝试将Motor直接接在电池两端，它会正常旋转。而在机器人中使用时，Motor的两端被接在SpeedController上，SpeedController再连接电池。注意观察，SpeedController又有两根信号线连接直RoboRIO的PWM接口。这表明，**程序是直接控制SpeedController，SpeedController再对电流进行某些调整，来控制Motor的速度的**。

**Servo**不需要SpeedController，它只有一种速度，直接接在PWM上。(但也不一定，它**有时**会有些奇怪的用法）

**气动**受程序控制的只有Compressor和(Double)Solenoid，**Compressor**是自动的，**一般**不用写任何关于它的代码。**Solenoid**是程序直接控制的，用来控制气流通断，从而控制活塞开合。注意，Solenoid的信号线和**气动的**其它部件一样，都接在**PCM**上，再通过CAN线接到RoboRIO，与RoboRIO的其它接口无关。关于气动，注意以下几点：

- Compressor是自动的，只要它被连入了电路，Enable后就会开始加压。当气压到达某个值，便会自动关闭。再次低于某个值后，又会开启。如果想要进行某些控制，需要用到Compressor类。
- 如果Enable后Compressor没有任何动作，换一个Switch看看:)。Switch是控制自动加压的东西，如果坏了，程序会认为气压已满。
- DoubleSolenoid是双向的，也就是可以进行拉和推（都有力），还能控制让它“放松”，也就是有三种状态（前通、后通、断）。Solenoid是单向的，只有两种状态，即加力和放松（通和断）。


每个传感器都有自己的说明文档，根据各自的连接方式接到RoboRIO上即可。API里都有相应的类进行控制，直接调用就好了。 **善用API** 。

 **Joystick** 连到电脑上，按键对应关系在DriverStation上可以看到。程序的编写一般有过图形编程基础，知道如何响应按钮事件的人都可以胜任。

RoboRIO上的不同接口的作用，在官方文档有很详细的说明。

**SmartDashboard**是一个图形化的数据窗口。你可以把需要的参数发送在上面,也可以发送一个SendableChooser来选择自动阶段运行的Command。可以在上面显示摄像头反馈的图像。

### 程序结构
程序结构分为包的结构和代码的结构。可以先看示例代码的TankDrive（Sample）和PacGoat（Command-Based）。如果有些代码不明白，照着API来看。 **(建议该模块与“学习API”模块同时进行)** 

- frc程序分为**Sample**和**Command-Based**两种，他们之间有什么不同？你能说出它们各自的优势吗？
- Sample中Robot类的结构？运行逻辑？
- Command-Based有哪几个包？例程中每个包里的类分别有什么共同点（代码上，逻辑上）？
- Command-Based中Robot类的结构？对应的比赛阶段？
- Command-Based中Subsystem的子类对应机器人硬件结构的逻辑？
- Command-Based中Command的子类所override的方法有哪些？它之间的关系（类比于循环语句）？
- Command-Based中OI类的作用？RobotMap的作用？
- Command-Based中Robot类里要写什么？实例化了上面哪些类？每个阶段它都调用了什么？

Sample一般只在测试的时候才会用到，参照TankDrive大致了解一下用法即可。下面为大体的结构：

```
public class RobotTemplate extends SampleRobot {
    /**
    * 此方法在每次进入自动阶段时被调用一次。
    */
    public void autonomous() {
        while (isAutonomous() && isEnable()) {
            // . . .
            Timer.delay(0.05);
        }
    }
    /**
    * 此方法在每次进入手动阶段时被调用一次。
    */
    public void operatorControl() {
        while (isOperatorControl() && isEnabled()) {
            // . . .
            Timer.delay(0.05);
        }
    }
}
```

**Command-Based的用法需要仔细学习。**

Command-Based的Subsystem包主要放一些与硬件模块对应的程序类，它们都继承于Subsystem类。这些类没有固定的格式，方法名都是自己定义的。比如一个收球机构，你可以在里面定义一个SpeedController，再定义一个 `start()` 来使SpeedController开始旋转以完成开始收球的动作，一个 `stop()` 来使SpeedController停止旋转以使收球动作停止。你也可以尝试把整个收射球机构写在一个Subsystem里，定义 `shoot()` ， `collect()` ， `blend()` 等大量方法。不过，需要知道的是，**一个Subsystem是不能同时被两个Command访问的**。如果像上面那样写，就意味着收射球机构每次只能被一个Command调用，这样想要使一个Subsystem中的两个部分同时运行不相关的动作，就很难实现；即使实现了，各个Command之间也会混乱不堪。所以，一个Subsystem所包括的范围的选定是极为重要的。这个范围，需要通过实际需要来选定。

Subsystem唯一需要override的方法是 `initDefaultCommand()` 。你可以在里面添加一句 `setDefaultCommand(new XxxCommand());` 来设置此Subsystem的默认Command。它将会 **在该Subsystem空闲时被调用** 。比如，在手动阶段，你想让某Command暂时取得底盘的控制，但如果直接调用这个Command，它会与原有的手动操控Command(DriveWithJoystick)发生冲突。这时候便可以把DriveWithJoystick设置为一个DefaultCommand，再调用你需要的Command，当Command退出后，DriveWithJoystick便会（受到Scheduler的调用而）重新运行起来。

Commands包主要包含一些与机器人动作功能相对应的类，类似对机器人发出的一个“指令”。它们都继承于Command类。Command与Subsystem不同，它**有固定的“格式”**。创建后要override一些固定的方法：
-  `initialize()` 
-  `execute()` 
-  `isFinished()` 
-  `end()` 
-  `interrupted()` 

它们的关系类似这样：
```
initialize();
while(true){
    execute();
    if(isFinished()) {
        end();
        break;
    }
    if(有Joystick Button事件或其它Command抢占等意外终止) {
        interrupted();
        break;
    }
}
```
如果你想让一个Command直接运行，你可以调用它的 `start()` 方法。

在Command类的构造方法里，要加上一句 `requires(Robot.XxxSubsystem);` 。这一句实际上声明了对于Robot类中实例化的某一个XxxSubsystem的占用。它有点类似线程的占用，只不过当新的Command调用出现时，是将之前的Command interrupt掉并执行当前Command， **而不是** 等待其执行完再执行。但有时也有某些Command无法被interrupt掉，这时便会自动丢弃掉新的Command不再执行。

Robot和OI类看PacGoat例程的Robot和OI类就能很容易明白。直接照着写即可。RobotMap类可有可无，但如果将每个接口的映射关系写在一个类里，维护起来会方便很多。

要注意写代码的习惯。千万不要写混乱的代码，即使它能运行。更不要写构思巧妙但很难看懂的代码。而且除非必要，O(n)但生涩难懂的代码是不如O(n^2)但简单易懂的代码的。因为**一段很难“看”的代码是没有人愿意检查维护的，于是便更容易在赛场上挂掉**。（Java由于本身的特性，比C/C++更稳定易控，但最好养成这种习惯。）不要企图靠注释来解释你混乱的代码。优秀的代码本身就是注释。即使别人通过读注释理解了你的代码，但混乱的代码仍然难以维护。

还有变量名一定要准确，不要怕长，因为直观的变量名是你唯一可以不写注释的理由，而且要用英文，用英文，用英文，不要拼音。

程序各部分一定要充分**解耦**，也就是不能有任何不该某个类管的功能被某个类管了。每个类任务要明确，代码要充分符合类名、方法名、变量名的逻辑。Subsystem和Command要充分分开。如果你没有这么做，当你写第二个依赖同一Subsystem的Command时就知道错了。

#### 深入理解Command的运行机制

要理解Command究竟是如何运行的，就不得不提到Scheduler这个类。或许你也发现了，在Robot类中有一句 `Scheduler.getInstance().run();` 。如果你没有仔细读过官方文档，或许你会疑惑它的作用。Scheduler究竟是做什么用的？

通过上面那句代码我们可以推测Scheduler是一个单例的类，它类似一个执行Command的表，在整个程序中只有一个实例。在执行 `run()` 方法时实际上依次发生了下面几件事：
1. 遍历每一个Buttons和Triggers，如果它的值为true（被按下），便会将其关联的Command加入一个“将被执行”的表中。
2. 遍历“正在执行”表中的所有Command，依次分别执行一次它们的 `execute()` 和 `isFinished()` 方法，如果 `isFinished()` 返回true，便会将该Command从表中删除，并调用 `end()` 方法。
3. 遍历“将被执行”表中所有Command。
  - 如果它依赖的Subsystem没有被占用，便会直接被加入“正在执行”表中，执行它的 `initialize()` 方法。
  - 如果依赖的Command被占用
    - 占用它的Command是interruptible的，便会将原Command从“正在执行”表中删除，并执行其 `interrupted()` 方法。将新的Command加入“正在执行”表中，执行它的 `initialize()` 方法。
    - 占用它的Command不是interruptible的，便直接将新的Command从“将被执行”表中删除。
4. 遍历Subsystem，如果它是空闲的，将它的DefaultCommand加入“将被执行”表中。

上述过程可能与实际情况有所不同。有一些比较复杂的过程，比如按键终止事件的实现过程并没有写进去（其实笔者SweetDumpling也不知道😂），所以实际情况可能比上述复杂得多。上面只是能够帮助你理解这个过程，实际情况还是以实际情况为准:)。

我们还知道，Robot类中XxxPeriodic是每次接收到DriveStation的指令便执行一次的，而DS的指令间隔是2ms。所以Scheduler每2ms便执行一次上述过程，注意代码的运行效率:)。还有如果两个Command调用的间隔小于2ms的话，它们便有同时启动和间隔2ms启动的两种可能。

上面的过程类似一个多线程的实现，不过它是以Command的 `execute()` 和 `isFinished()` 为基本单位，而非机器指令。而且它的许多机制也与多线程不同，但你完全可以将每个Command当做一个线程来看待，并且是一个较为“稳定”的线程。因为Scheduler内部已经帮你解决了冲突的问题。

### 学习API
通过API，学习WPILIBJ里的常用类和方法。

如何通过[API](http://first.wpi.edu/FRC/roborio/release/docs/java/)学习？（个人经验）
1. 看常量、构造方法、方法的文档解释，通过名称猜测作用
2. 看方法的参数所涉及的类
3. 看父类，看子类，看“兄弟类”（父类的子类）

常用类：

- Accelerometer（RoboRio内置加速度计，**不靠谱，谨慎使用**）
- ADXRS450_Gyro（外接的Gyro）
- Button
- CameraServer (思考：CameraServer怎么获得实例？能new吗？为什么是getInstance()？)
- Command（requires()的作用？）
- CommandGroup（Command与CommandGroup的关系？）
- Subsystem
- Compressor
- DigitalInput
- DigitalOutput
- Solenoid（单向电磁阀）
- DoubleSolenoid（双向电磁阀）
- Encoder
- Gyro（内置gyro，**似乎不靠谱**）
- Joystick
- JoystickButton
- LiveWindow
- RobotDrive（tankDrive()和arcadeDrive()分别是什么意思？通过参数名猜测一下）
- SampleRobot
- IterativeRobot
- Scheduler
- SendableChooser
- Servo
- SmartDashboard
- Timer
- SpeedController
- Talon
- Victor



### 尝试
啊哈，搭建起测试用的硬件吧。接线方法在官方给的文档里有说明，你们也可以向老队员或者老师学习接线的方法。把电机、全套气动、Servo接在一块电路里，写一个Commmand-Based，并用Joystick上的不同按钮分开控制。弄一些传感器，把信息打印到SmartDashboard上。体会程序的魅力吧！别忘了编译前祈祷无Bug。
烧程序的方法在官方文档里有说明。

啊哈，遇到Bug了吧？把逻辑弄清，参照例程看
这几道题很简单。你或许会觉得最后一道题没思路？**多看看API。**

看有什么不对。然后检查以下几点：
1. Eclipse插件更新了吗？
2. RoboRIO固件更新？刷Java了吗？配置对不对？
3. 网络连接正常吗？重启路由器或者RoboRIO试试？
4. **程序真的没问题？**

### 练习

写代码解决或验证以下问题：
- 两个使用不同Subsystem的Command能否同时运行？
- 两个使用同一个Subsystem的Command能否同时运行？
- 怎样实现按一下按钮电机转动，再按一下电机停止？
- 怎样实现按下按钮电机转动，松开按钮电机停止？
- 怎样实现按下A按钮电机全速，然后按下B按钮电机半速，松开B按钮电机全速，松开A按钮电机停止？(单独按下B按钮应当没有效果)


### 底盘算法
看PacGoat的DriveForward类。

**思考：** 这个类的作用？它实现了什么动作？Kp的意义？这个类在运动方向上有什么不足？

**练习：** 改进这个类的不足，并用这个算法来通过Gyro控制机器人转角。

**思考2：** 你有什么其它更好的控制算法？实现出来尝试一下。

这个类是用来进行前进距离校准的。**Kp是最后开始减速的位置到最终位置距离的倒数。减速是为了防止惯性导致实际距离偏大。**

不过这个类没法做到后退以及运动中方向偏转的矫正。尝试实现这些吧！

### 视觉
学习Grip和OpenCV，看API学习如何通过摄像头获得图像。官方教程里有通过目标像素坐标换算为实际位置的方法。

- 如何将摄像头的图像显示在SmartDashboard上？
- 观察场地为视觉准备的反光胶布的性质，用灯照照看，有什么发现？
- 学习计算机视觉相关知识，整个图像处理流程是怎样的？

FRC比赛中一些可以用到视觉的对象会用一种 **反光胶布** 标记起来。你们在初中物理题里一定见到过一种特殊的反射材质，它可以将光线沿它射来的方向反射回去。这种胶带便具有这样的性质。你可以将一种 **绿色LED光圈** 套在 **摄像头** 上，向前照射，便能通过摄像头获得反光胶布反射的绿色的光。这样，通过OpenCV识别和处理便容易多了。

OpenCV的java版资料很少，不过你们可以把网上查到的C++代码直接翻成Java。

**流程：** 获得图像->处理->识别出目标位置->换算为实际位置->根据相关算法采取动作。

**推荐学习：** ~~《学习OpenCV》(可能过时)，~~[CSDN浅墨的教程](http://m.blog.csdn.net/column/details?alias=opencv-tutorial)以及[OpenCV官方文档](http://docs.opencv.org/trunk/#gsc.tab=0)。

最好能详细了解一下数字图像处理。

### 本教程的维护

如果发现本教程中有错误或不足的地方，或者学到某个地方感觉无法理解而被卡住，请务必在学完后对相应位置进行修改。

-------
### Author(s):

- SweetDumpling(<sweetdumpling000@163.com>)