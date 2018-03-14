## 学习API
通过API，学习WPILibJ里的常用类和方法。

如何通过[API](http://first.wpi.edu/FRC/roborio/release/docs/java/)学习？（个人经验）
1. 看常量、构造方法和方法的文档解释，通过名称猜测作用
2. 看方法的参数所涉及的类
3. 看API中各类和各接口间的逻辑关系

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

其它不常用的类也可以了解一下~