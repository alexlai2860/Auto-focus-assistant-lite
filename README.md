# Auto-focus-assistant-lite
This is an lite version of the original auto-focus-assistant. In this project, we're trying to build the AF-assistant with a microcomputer and a 2D lidar instead of using the X86 computer and the depth camera

### 研究背景/background
* 近年来，随着个人Vlog和短视频的兴起，视频拍摄的门槛越来越低，用户对于摄影机性能的要求也越来越高<br>
对于个人用户或小团队用户而言，摄影机性能主要体现在两个方向: **画质和易用性**<br>
前者在此处不作详细展开，后者主要包括：体积重量、人体工学、对焦性能和防抖性能等。<br>
其中， **自动对焦系统** 是极为重要的一部分，而各厂家由于技术路线或专利的限制，对焦性能参差不齐<br>
在当下的市场上，有很多画质优秀的影像设备由于易用性不佳，而导致了大量用户的流失<br>
例如，来自Panasonic和blackmagic design的摄影机，他们有着出色的画质但缺乏可靠的自动对焦系统<br>
* #### 本项目旨在改善摄影机的自动对焦性能，实现自动或半自动对焦

### 工作原理/principle
本项目是基于单片机和2D激光雷达构建的主动式对焦系统<br>
+ #### 基础部分<br>
先通过2D激光雷达使用TOF方式获取一定范围的2D深度图，同时获取该范围内最近物体的距离<br>
再通过单片机处理信号和发送信号，通过映射函数，将电机驱动至指定的位置，实现对焦动作<br>
每隔一定时间（如100ms）执行一次上述循环，实现连续对焦功能<br>
+ #### 衍生部分<br>
正如很多有经验的司机不愿意使用自动挡<br>
很多有经验的摄影师也不愿意完全依赖自动对焦<br>
因此，我们通过逆向工程，读取铁头公司推出的电机控制手柄的通信协议<br>
将其滚轮和按钮用于自动对焦系统的控制和模式切换<br>
同时,利用单片机驱动触摸屏，将雷达信号以俯视图的形式，和实时电机位置混合后投射在屏幕上<br>
为摄影师或跟焦员提供直观易懂的雷达视图以辅助对焦<br>
从而实现“手自一体”的对焦方式<br>

### 硬件结构/hardware structure
本项目的硬件结构主要由以下四个部件组成:<br>

#### 传感器/sensor
+ 传感器用于感知目标距离相机的距离，给控制器提供信息用于驱动电机<br>
+ 此处选用的传感器是2D激光雷达，选型理由如下:<br>
  相较于单点式激光雷达，2D激光雷达可以覆盖更大的范围，当目标物体偏离中心时依然可以正常对焦<br>
  相较于深度相机和面阵激光雷达，2D激光雷达所需要的算力和功耗都较小，适合搭配单片机使用<br>
  同时，2D激光雷达成本相对低廉，为未来的商业化保留了可能性<br>
+ 最终，我们选择了来自乐动机器人的LD19激光雷达<br>
  它拥有最远达12m的探测距离，采用905nm波长可以规避阳光干扰，扫描性能足够满足本项目的需求<br>
  ![image](https://github.com/alexlai2860/Auto-focus-assistant-lite/assets/71208694/281fdac5-efaf-42a9-bc72-0a3544252dc5)


#### 控制器/controller
+ 控制器用于实现“手自一体”的自动对焦模式选择和功能切换<br>
+ Tilta铁头公司是一家专业制作相机配件的器材供应商，他们于2018年底推出了原力N(NucleusN)电动跟焦系统<br>
  这套系统在这几年间不断更新壮大，提供了跟焦马达、跟焦手柄和蓝牙跟焦手轮等一系列解决方案<br>
  本项目也采用了铁头公司的原力N系统，主要针对其跟焦马达和跟焦手柄进行研发<br>
+ 在本项目中，通过逆向功能还原其通信协议，原力N系统的跟焦手柄被改造为控制手柄<br>
  我们对按键和滚轮等功能进行了重映射，具体的使用方法见后文“使用方法”部分<br>
  除此之外，该手柄还承担给执行器电机供电的功能<br>
  ![image](https://user-images.githubusercontent.com/71208694/234904265-ea06e599-4549-4483-97e3-278996c009fc.png)

#### 处理器/processor
+ 处理器用于处理传感器获取的距离信息，同时读取来自控制器的信号<br>
+ 经过处理后，处理器将信号发送给执行器(电机)<br>
  同时，处理器也用于协调系统各部分工作，与用户进行交互，并完成雷达俯视图的绘制(在触摸屏上)<br>
+ 此处选用的处理器是STM32单片机<br>
  因为本项目是STM32单片机课程设计，同时STM32兼顾了低价和高性能的需求<br>
+ ![image](https://github.com/alexlai2860/Auto-focus-assistant-lite/assets/71208694/6c590b1c-1cbb-42c2-bde6-71545a6c7df5)


#### 执行器/operator
+ 执行器是执行对焦动作的器件，一般为电机<br>
+ 在本项目中，执行器是Tilta铁头原力N跟焦电机<br>
  原力N作为民用影视行业广泛使用的跟焦电机之一，有着低廉的售价、良好的群众基础和优秀的性能<br>
  同样的，我们通过逆向还原铁头原力N的通信协议，完成了基于位置闭环的电机控制<br>
  ![image](https://user-images.githubusercontent.com/71208694/234904493-18fb9c13-21b8-49a4-9960-7fc6be5cf971.png)
  ![image](https://user-images.githubusercontent.com/71208694/234904598-e6157f63-2e73-4427-8ba5-1e947c6ab19e.png)

### 团队分工/team division
+ 赖建宇 : 负责逆向铁头原力N系统的通信协议和整体逻辑框架搭建
+ 黄桦 : 负责stm32选型和调试触摸屏，与雷达整合显示波形图
+ 李其銮 : 负责雷达数据的读取与处理

### 软件结构/software structure
+ 本项目软件架构较为简单，主要分为motor马达模块，serial串口模块，screen触摸屏模块
+ motor模块对此前Auto Focus Assistant项目中的多个函数进行了移植和重构
  + 包括拉格朗日插值函数、dis2pose距离转电机位置函数、ASCII与HEX格式的转换函数等等
    +  ![image](https://github.com/alexlai2860/Auto-focus-assistant-lite/assets/71208694/373a432f-8c64-4697-8da8-627f0de95b28)
  + 提供了三种运行模式：自动对焦模式、手动辅助对焦模式、电机行程校准模式
+ serial串口模块包括UASRT1、USART2和USART3
  + USART1用于接入串口助手实现离线调试，波特率设置为115200
  + USART2接激光雷达实现距离检测，波特率设置为230400
  + USART2接线示意图如下:
    + ![image](https://github.com/alexlai2860/Auto-focus-assistant-lite/assets/71208694/ccb4703f-3a0e-4d70-ace2-c459f8190159)

  + USART3用于和原力N通信，其中RX接控制手柄的TX，TX接电机的RX，波特率设置为115200
  + 逆向得到的USART3接线示意图如下：
    + ![b491169612154ddeea63de25a276a97](https://github.com/alexlai2860/Auto-focus-assistant-lite/assets/71208694/2ef850d8-95ca-4261-9e30-4ea071b4f70f)
  + 逆向得到的电机控制协议如下：
    + ![image](https://github.com/alexlai2860/Auto-focus-assistant-lite/assets/71208694/9a1cd28a-9608-4f09-b07b-72e58ee7d3e2)


+ screen模块用于驱动触摸屏，绘制触摸屏UI
  + 前期的UI构思如下：
  + ![f67467d78a86b0ecbd9002773f80ea2](https://github.com/alexlai2860/Auto-focus-assistant-lite/assets/71208694/34bf76fa-935a-4b91-84d9-c75f118deffd)
  + ![image](https://github.com/alexlai2860/Auto-focus-assistant-lite/assets/71208694/e78ee4f0-bfad-45d5-8038-b18608c17ed9)

  + 后期成品实际UI如下
  + ![image](https://github.com/alexlai2860/Auto-focus-assistant-lite/assets/71208694/32634e00-8fd6-4b96-8cdf-5816dffb8fc0)
  + UI说明：显示屏从上到下一次为状态显示栏、对焦波形图和相应文字说明。
  + 状态显示栏部分，左侧ON/OFF启动时字体颜色为绿色，关闭时为红色，右侧Auto和Manual可以通过按键切换，Auto为自动对焦模式（由单片机接管电机控制），Manual为手动对焦模式（通过滚轮编码器进行电机控制）。
  + 波形图部分，蓝色的曲线代表当前摄影机前方的测距数据，共由40个点构成，理论刷新率为10hz；绿色直线则代表当前手柄滚轮所指示的位置，会按照滚轮位置的变化而上下移动。波形图横坐标为-40°~40°，即最大对焦视场角为80°；波形图纵坐标为0-10m，即最远测距有效值为10m；波形图精度为10cm。

### 物料清单/BOM
+ 铁头原力N跟焦电机及手柄套装 * 1
+ 乐动机器人LD19激光雷达 * 1
+ 15mm 碳纤维管固定支架 * 1
+ STM32F103ZET6带320x240显示屏 * 1
+ micro USB 转 杜邦线 * 2
+ zh1.5 公口 转 2.54杜邦线 * 1 
+ 杜邦线若干
+ (3D打印固定件若干)

### 使用方法/user instruction
+ 按照接线示意图，连接单片机、控制手柄、雷达和跟焦电机
+ 先开启手柄电源，此时手柄为跟焦电机提供8V供电，电机启动
+ 再启动单片机，默认进行上电行程自动校准，随后进入自动对焦或手动辅助对焦状态
+ 此时你可以在雷达示波器上看到雷达波形图和当前电机位置指示，并可以通过按键切换工作模式（见软件架构部分）
