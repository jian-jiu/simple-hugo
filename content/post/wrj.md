---
title: 无人机
date: 2026-08-06
slug: wrj
categories:
    - 无人机
tags:
    - 无人机
---

根据嘉立创的 琛光 E1 Mini 工程，它使用的是经典 ESP32 30PIN 开发板，不是 ESP32-C3/S3。因此适用源码 board_config.h 中的默认 ESP32 配置。
1. 四个电机控制引脚
   代码中的电机顺序固定为：
   电机位置	代码名称	ESP32 GPIO
   左后	MOTOR_REAR_LEFT / RL	GPIO12
   右后	MOTOR_REAR_RIGHT / RR	GPIO13
   右前	MOTOR_FRONT_RIGHT / FR	GPIO15
   左前	MOTOR_FRONT_LEFT / FL	GPIO14


从机头向前看：
机头方向
↑

    左前 FL                 右前 FR
    GPIO14                  GPIO15

    左后 RL                 右后 RR
    GPIO12                  GPIO13
程序数组顺序是：
int motorPins[4] = {12, 13, 15, 14};
//                    RL  RR  FR  FL
电机控制方式：
使用 ESP32 的 LEDC PWM
默认频率：25 kHz
分辨率：10 bit
占空比范围：0～1023
默认用于空心杯有刷电机＋MOSFET
非常重要：ESP32 GPIO 不能直接驱动电机。GPIO12/13/14/15 接的是四路 MOSFET 栅极，电机电流由电池经过 MOSFET 提供。
如果改成无刷电调 ESC，源码提示需要：
PWM 频率改到约 400 Hz
配置 pwmStop
配置 pwmMin
配置 pwmMax
电机旋转方向应以 PCB 丝印为准；对角线两个电机方向相同。
2. MPU6500 接线
   MPU6500 使用 SPI 通信，而不是普通 I²C。
   MPU6500 模块引脚	SPI作用	ESP32 GPIO
   SCL / SCLK	SPI 时钟 SCK	GPIO18
   AD0 / SDO	SPI MISO	GPIO19
   SDA / SDI	SPI MOSI	GPIO23
   NCS / CS	SPI片选	GPIO5
   VCC	电源	PCB对应 VCC
   GND	地	GND
   INT	中断	未使用


也就是：
ESP32 GPIO18 ─── MPU6500 SCL/SCLK
ESP32 GPIO19 ─── MPU6500 AD0/SDO
ESP32 GPIO23 ─── MPU6500 SDA/SDI
ESP32 GPIO5  ─── MPU6500 NCS
ESP32 GND    ─── MPU6500 GND
PCB VCC      ─── MPU6500 VCC
嘉立创页面明确提到，陀螺仪模块只需要焊接：
VCC、GND、SCL、SDA、AD0、NCS
因此不需要连接 INT。
注意：不要仅根据模块上的 SCL/SDA 字样误认为它是 I²C。因为 NCS 被拉低并由 GPIO5控制时，MPU6500进入 SPI 模式：
SCL 作为 SPI SCK
SDA 作为 SPI MOSI
AD0 作为 SPI MISO
3. MPU6500数据读取方式
   初始化代码相当于：
   SPI.begin(18, 19, 23, 5);
   MPU9250 imu(SPI, 5);
   虽然驱动类名称叫 MPU9250，但项目硬件使用 MPU6500，该驱动兼容相关型号。
   初始化参数：
   参数	设置
   加速度计范围	±4g
   陀螺仪范围	±2000 °/s
   采样频率	约1 kHz
   数字低通滤波	DLPF_MAX
   读取数据	三轴陀螺仪＋三轴加速度计


主循环读取：
imu.waitForData();

imu.getGyro(gyro.x, gyro.y, gyro.z);
imu.getAccel(acc.x, acc.y, acc.z);
读取后还会进行：
陀螺仪零偏校准
加速度计偏置、比例校准
IMU安装方向转换
四元数姿态估计
PID姿态控制
该 PCB 上 MPU6500 的默认安装方向是：
芯片正面朝上
芯片 X 轴丝印 → 飞行器右侧
芯片 Y 轴丝印 → 飞行器前方
软件默认旋转：
Vector imuRotation(0, 0, -PI / 2);
等效于把传感器坐标近似转换为：
Vector(data.y, -data.x, data.z)
MPU6500没有磁力计，所以：
Roll、Pitch可以正常估算
Yaw只能通过陀螺仪积分估算
Yaw会随时间漂移，无法获得绝对航向
4. 电池电压检测
   功能	ESP32 GPIO
   电池电压 ADC	GPIO36


硬件使用电阻分压：
电池正极
│
10kΩ
│
├──── GPIO36
│
33kΩ
│
GND
程序换算公式：
Vbat = Vadc × 43 / 33
电池电压保护值：
状态	电压
空载/怠速预警	3.5 V
飞行中低电警告	3.3 V
危急、自动降落	3.0 V
低于此值认为没有接电池	0.5 V


每0.5秒进行一次采样，经典 ESP32 每次平均16次 ADC 读数。
5. 实体遥控接收机
   功能	配置
   串口	Serial2
   ESP32接收引脚 RX	GPIO4
   ESP32发送引脚 TX	未启用，-1
   默认协议	CRSF/ELRS
   默认波特率	420000
   可选协议	SBUS，100000


ELRS/CRSF接线：
接收机 TX ─── ESP32 GPIO4
接收机 GND ── GND
接收机电源 ── PCB对应电源
注意是接收机的 TX 接 ESP32 的 RX GPIO4。
当前固件只接收遥控数据，没有启用串口遥测回传，因此 ESP32 TX 没有分配引脚。
6. 状态LED
   功能	ESP32 GPIO
   状态LED	GPIO2


经典 ESP32 配置中：
BOARD_LED_PIN = 2
BOARD_LED_INVERTED = 0
即高电平点亮。LED用于表示：
初始化
解锁状态
低电量
遥控失联
异常状态
7. 扩展接口
   经典 ESP32 默认预留 I²C：
   接口	ESP32 GPIO
   I²C SDA	GPIO21
   I²C SCL	GPIO22


用于扩展板上的传感器，例如：
BMP388气压计
QMC5883L磁力计
VL53L1X激光测距
PMW3901光流传感器
另外两个专用控制引脚：
设备	功能	ESP32 GPIO
VL53L1X	XSHUT	GPIO32
PMW3901	SPI片选 CS	GPIO33


PMW3901可与 MPU6500共享 SPI 总线：
SCK  = GPIO18
MISO = GPIO19
MOSI = GPIO23
但使用单独的片选：
MPU6500 CS  = GPIO5
PMW3901 CS  = GPIO33
8. E1 Mini完整引脚表
   GPIO	功能
   GPIO2	状态LED
   GPIO4	ELRS/CRSF/SBUS接收
   GPIO5	MPU6500 SPI CS
   GPIO12	左后电机 RL
   GPIO13	右后电机 RR
   GPIO14	左前电机 FL
   GPIO15	右前电机 FR
   GPIO18	MPU6500/扩展SPI SCK
   GPIO19	MPU6500/扩展SPI MISO
   GPIO21	扩展I²C SDA
   GPIO22	扩展I²C SCL
   GPIO23	MPU6500/扩展SPI MOSI
   GPIO32	VL53L1X XSHUT
   GPIO33	PMW3901 SPI CS
   GPIO36	电池电压ADC


Wi-Fi和网页遥控器使用 ESP32 内部无线功能，不占用额外外部 GPIO。
来源：
board_config.h
motors.ino
imu.ino
battery.ino
琛光 E1 Mini 嘉立创工程


2:03








驱动电机的四路 MOSFET 栅极 我能不能单独做一个pcb，然后电源稳压也单独做一个，自己用3d打印机打印支架，这样使用可行吗






2:14






可行，而且模块化后更方便调试和更换。建议拆成三个模块，但必须处理好供电噪声、公共地、重量和重心。
1S锂电池 3.0～4.2V
│
├── 电机/MOSFET板 ── 四个电机
│       ↑
│    4路PWM信号
│
└── 稳压板 ── ESP32 + MPU6500

所有模块的GND必须连接在一起
1. 四路MOSFET板
   每路典型电路：
   电池正极 VBAT ───── 电机 ───── MOSFET Drain
   MOSFET Source ── GND

ESP32 GPIO ── 10～100Ω ── MOSFET Gate
│
10k～100kΩ
│
GND
四路信号对应：
电机	ESP32信号
左后 RL	GPIO12
右后 RR	GPIO13
右前 FR	GPIO15
左前 FL	GPIO14


设计时注意：
必须选用在 VGS=2.5V/3.3V 下明确标注导通电阻的逻辑电平 N-MOS。
不要只看器件写着“耐压30V、最大几十安”，很多MOSFET需要10V栅极电压才能完全导通。
栅极串联10～100Ω电阻，防止振荡并限制GPIO瞬时充放电电流。
每个栅极必须有下拉电阻，避免ESP32上电过程中电机突然转动。
MOSFET板上放置大容量低ESR电容，例如原项目使用的470µF电容。
建议每路电机加入续流二极管或合适的浪涌抑制电路。
可以在每个电机两端并联约10～100nF陶瓷电容抑制电刷噪声。
MOSFET和电机供电走线要短而宽，避免电压下降和PCB发热。
MOSFET需要按照电机堵转电流选择。小空心杯电机的正常电流可能不大，但堵转瞬间可能达到数安培，因此不能只按照正常悬停电流设计。
2. ESP32稳压板
   1S电池范围约为：
   满电：4.2V
   标称：3.7V
   低电：约3.0～3.3V
   如果使用ESP32 30PIN开发板，有两种供电方法。
   方案A：升压到5V
   1S电池 → 5V升压模块 → ESP32的5V/VIN
   这是比较稳妥、容易调试的方案：
   建议5V输出能力至少1A，最好留到1.5A左右。
   ESP32打开Wi-Fi时存在较大的瞬时电流。
   ESP32开发板再通过板载LDO产生3.3V。
   效率稍低，但供电简单。
   方案B：直接产生3.3V
   1S电池 → 3.3V Buck-Boost → ESP32的3V3
   效率可以更高，但要求更严格：
   必须是升降压稳压器，因为电池可能高于或低于3.3V。
   建议持续输出能力至少1A，瞬时能力更高。
   输出必须稳定在3.3V，不能超过ESP32允许范围。
   直接连接3V3会绕过开发板板载稳压和部分保护。
   使用USB调试时要防止两个电源相互倒灌。
   如果是第一次制作，建议先采用“1S升压到5V，再接ESP32的5V引脚”。
3. 电机和控制部分必须共地
   即使稳压板和MOSFET板分开，仍然必须：
   ESP32 GND ── MOSFET板GND ── 电池负极
   否则GPIO输出没有共同参考电位，MOSFET可能：
   无法正常导通
   随机导通
   发热
   导致电机失控
   建议采用星形接地：
   ┌── MOSFET板大电流地
   电池负极/星形接地点 ┤
   └── 稳压板和ESP32地
   不要让四个电机的大电流经过 MPU6500 或 ESP32 的地线再回到电池。
4. MPU6500最好和ESP32放在飞控板
   不建议把MPU6500放在电源板或电机MOSFET板上。比较合理的是：
   飞控板：ESP32 + MPU6500
   动力板：4路MOSFET + 大电容
   电源板：5V/3.3V稳压
   原因是：
   MOSFET开关会产生电磁干扰。
   电机振动和电刷噪声会影响陀螺仪。
   MPU6500应尽量安装在机体重心附近。
   MPU6500到ESP32的SPI线应尽可能短。
   经典ESP32与MPU6500接线：
   MPU6500	ESP32
   SCL/SCLK	GPIO18
   AD0/SDO/MISO	GPIO19
   SDA/SDI/MOSI	GPIO23
   NCS/CS	GPIO5
   GND	GND
   VCC	对应传感器电源


SPI线建议控制在几厘米以内。如果一定要用排线：
SCK线旁边安排GND线。
避免与四路电机线平行。
可以在SCK/MOSI靠近ESP32一端串联22～47Ω电阻减轻振铃。
5. 电池电压检测
   保持原项目电路和软件比例：
   电池正极
   │
   10kΩ
   │
   ├──── GPIO36
   │
   33kΩ
   │
   GND
   软件使用：
   Vbat = Vadc × 43 / 33;
   可以在GPIO36与GND之间增加约100nF电容，过滤电机产生的电压波动，但电阻比例不要随意改变，否则代码计算出来的电压会不正确。
   采样线应从电池输入处单独引出，不要从电机供电线末端取样。
6. 板间接口建议
   飞控板到MOSFET板至少需要：
   信号	说明
   PWM_RL	GPIO12
   PWM_RR	GPIO13
   PWM_FR	GPIO15
   PWM_FL	GPIO14
   GND	公共地


电机板电源接口：
信号	说明
VBAT	1S电池正极
GND	电池负极


连接器必须防反插、耐振动。四路PWM线尽量短，最好旁边至少有一根可靠的公共地线。
7. 3D打印支架注意事项
   可以自行打印，但需要注意：
   四个电机中心到机体中心的距离尽量完全一致。
   四个电机轴必须互相平行。
   ESP32和MPU6500尽量位于中心。
   电池位置应让整机重心靠近四电机几何中心。
   MPU6500安装方向应与原程序一致。
   如果飞控板旋转了90°，必须修改 imuRotation，否则姿态控制方向会错误。
   电机和桨叶按照PCB丝印规定的方向安装。
   PETG、尼龙或适度柔性的材料一般比脆性PLA更适合作为电机支架。
   支架不能太软，否则电机角度会随推力变化。
   模块、排线和连接器会增加重量，小型空心杯无人机对每一克都很敏感。
   模块化更适合原型调试；确认电路稳定后，如果追求续航和飞行性能，可以再把电源和MOSFET部分合并成更轻的动力板。
   首次测试时务必拆掉桨叶，先分别测试 mfl、mfr、mrl、mrr，确认位置和转向正确后再安装桨叶。