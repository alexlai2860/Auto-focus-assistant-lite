# Auto-focus-assistant-lite
This is an lite version of the original auto-focus-assistant. In this project, we're trying to build the AF-assistant with a microcomputer and a 2D lidar instead of using the X86 computer and the depth camera

### 研究背景/background
近年来，随着个人Vlog和短视频的兴起，视频拍摄的门槛越来越低，用户对于摄影机性能的要求也越来越高<br>
对于个人用户或小团队用户而言，摄影机性能主要体现在两个方向:画质和易用性<br>
前者在此处不作详细展开，后者主要包括：体积重量、人体工学、对焦性能和防抖性能等。<br>
其中，自动对焦系统是极为重要的一部分，而各厂家由于技术路线或专利的限制，对焦性能参差不齐<br>
在当下的市场上，有很多画质优秀的影像设备由于易用性不佳，而导致了大量用户的流失<br>
例如，来自Panasonic和blackmagic design的摄影机，他们有着出色的画质但缺乏可靠的自动对焦系统<br>
本项目旨在改善摄影机的自动对焦性能，实现自动或半自动对焦<br>

### 工作原理/principle
本项目是基于单片机和2D激光雷达构建的主动式对焦系统<br>
+ 基础部分
先通过2D激光雷达使用TOF方式获取一定范围的2D深度图，同时获取该范围内最近物体的距离<br>
再通过单片机处理信号和发送信号，通过映射函数，将电机驱动至指定的位置，实现对焦动作<br>
每隔一定时间（如100ms）执行一次上述循环，实现连续对焦功能<br>
+ 衍生部分
正如很多有经验的司机不愿意使用自动挡<br>
很多有经验的摄影师也不愿意完全使用自动对焦<br>
因此，我们通过逆向工程，读取铁头控制手柄的通信协议和其编码器数值<br>

同时将
实现”手自一体”的对焦方式<br>

### 硬件结构/hardware structure
#### 传感器/sensor
#### 控制器/controller
#### 处理器/processor
#### 执行器/operator

### 物料清单/BOM
