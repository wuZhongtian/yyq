## STM 32开发工具 

### 1. STM32CobeIDE

- ![image-20241028165214407](./images/stm32/image-20241028165214407.png)
- 

## STM32F1基础知识

### 1. 单片机简介

- 单片机：单片微型计算机，集成电路芯片；
  - 优点：功耗低，体积小，集成度高，使用方便；
  
- CISC VS RISC

  - ![image-20240813110658817](./images/stm32/image-20240813110658817.png)

  - ![image-20240813110721970](./images/stm32/image-20240813110721970.png)
  - 冯诺依曼结构 VS 哈佛结构
    - ![image-20240813110928325](./images/stm32/image-20240813110928325.png)

### 2. Cortex_M系列介绍

- Cortex_M(Microcontroller--单片机/微控制器)
  - 高性能、低功耗，工控、消费电子

#### 2.1 ARM公司介绍

- ARM公司：只做内核设计和IP授权
  - ![image-20240813112522739](./images/stm32/image-20240813112522739.png)
  - 重点注意：ARM与其合作伙伴的分工
    - ARM：内核设计、IP授权
    - 合作伙伴：芯片设计（STM32系列合作伙伴是ST公司）
      - ![image-20240813113414854](./images/stm32/image-20240813113414854.png)
- ARM架构特点
  - ![image-20240813113614660](./images/stm32/image-20240813113614660.png)

#### 2.2 Cortex内核分类及特征

- ![image-20240813145442261](./images/stm32/image-20240813145442261.png)
- 这里主要用到Cortex-M内核

#### 2.3 Cortex-M3/4/7介绍

- ![image-20240813145548352](./images/stm32/image-20240813145548352.png)

### 3. 初识STM32

- STM32
  - ST：意法半导体
  - M：MCU/MPU
  - 32：32位
- STM32特点：性能高、功耗低、外设多、价格低、型号丰富、实用性好、开发简单
- STM32分类：5大类、18个系列、1000多个型号
- STM32命令规则：
  - ![image-20240814093609914](./images/stm32/image-20240814093609914.png)

- 芯片型号选择原则：性能从高到低，内存从大到小

### 4. STM32原理图设计

#### 4.1 学会查看数据手册

- 数据手册来源：
  - ST官网：https://www.st.com
    - ![image-20240814141753771](./images/stm32/image-20240814141753771.png)
    - 进入官网之后，直接搜索芯片型号，就会进入芯片数据手册下载界面，这里下载的是英文版的数据手册
    - ![image-20240814141930105](./images/stm32/image-20240814141930105.png)
    - 点击芯片型号进入下载界面
    - ![image-20240814142003298](./images/stm32/image-20240814142003298.png)
  - ST中文社区网：https://www.stmcu.org.cn/
    - ![image-20240814142120278](./images/stm32/image-20240814142120278.png)
    - 进入网页点击资料下载
    - ![image-20240814142154760](./images/stm32/image-20240814142154760.png)
    - 点击STM32 MCU
    - ![image-20240814142811326](./images/stm32/image-20240814142811326.png)
    - 进入界面之后，设置筛选条件，在译文那一栏是中文版的数据手册，点击对应的芯片型号即可进入下载界面下载

##### 4.1.1 数据手册各章节内容

- ![image-20240814144152509](./images/stm32/image-20240814144152509.png)
- 芯片的基本参数
  - ![image-20240814144235093](./images/stm32/image-20240814144235093.png)
- STM32引脚的六大类型
  - 电源引脚
  - 晶振引脚
  - GPIO引脚
  - BOOT引脚
  - 下载引脚
  - 复位引脚
- 下载接口
  - ![image-20240814144457352](./images/stm32/image-20240814144457352.png)
  - SWD接口性价比最高，占用线路少，既能下载，又能调试烧录
  - 串口：只能下载，不能调试
  - JTAG：可下载，可调试，缺点是占用资源多

##### 4.1.2 芯片的常用封装

- LQFP封装
  - ![image-20240814145032539](./images/stm32/image-20240814145032539.png)
  - 芯片上的原点表示引脚原点，以原点按逆时针从1开始计数到144，就是芯片的引脚分布
  - 对应右边原理图，有引脚标号
- BGA封装
  - ![image-20240814145522107](./images/stm32/image-20240814145522107.png)
  - 如上图所示，芯片的小圆点是A1，作为坐标原点，下面的芯片背面原点处有一个黄色的小三角符号

#### 4.2 最小系统

- 最小系统：保证MCU芯片能正常工作的最小电路组成单元
- ![image-20240814145955869](./images/stm32/image-20240814145955869.png)
- 芯片为最小电路单元预留的接口
  - ![image-20240814150320654](./images/stm32/image-20240814150320654.png)
  - VDD/VSS数字电源正负
  - VDDA/VSSA模拟电源正负

##### 4.2.1 数字电源电路模块

- ![image-20240814151703677](./images/stm32/image-20240814151703677.png)
- 输入5V，通过AMS1117-3.3稳压器，稳定输出VCC3.3，VCC3.3和VCC3.3M之间的连的的0Ω电阻是为了后续维修用的，也可以省略，直接使用VCC3.3

##### 4.2.2 模拟电源电路模块

- ![image-20240814152034832](./images/stm32/image-20240814152034832.png)
- 数字的3.3V电源通过RC低通滤波器给模拟部分供电
  - RC低通滤波器：阻高频，通低频

##### 4.2.3 参考电压电路模块

- ![image-20240814152537619](./images/stm32/image-20240814152537619.png)
- 参考电压这里使用排针引了出来，可以连接VDDA，也可以通过杜邦线接外部电压做参考电压

##### 4.2.4 RTC&后备区域供电引脚

- ![image-20240814171352295](./images/stm32/image-20240814171352295.png)
- 当板子断电时，BAT给RTC&后备区域供电

##### 4.2.5 复位电路

- ![image-20240814171836526](./images/stm32/image-20240814171836526.png)
  - 上电复位
    - 上电一瞬间，电容值为0，电容导通，RESET端是低电平状态，等电容充满电之后，RESET端变成高电平，即在上电瞬间可实现复位
  - 复位按键复位
    - 按下按键，让RESET保持低电平状态1~4.5ms即可实现复位
  - 这里的上拉电阻设置为10K，电容设为104，即10*10^4PF

##### 4.2.6 BOOT启动电路

- ![image-20240814172658715](./images/stm32/image-20240814172658715.png)
- BOOT0/1通过跳线帽与高电平1/低电平0相接

##### 4.2.7 晶振电路

- ![image-20240814173020654](./images/stm32/image-20240814173020654.png)
- 内部/外部低速晶振：32.768KHZ
- 外部晶振根据板子型号，常用的F103/F407是8MHZ

##### 4.2.8 下载调试电路

- ![image-20240814173330439](./images/stm32/image-20240814173330439.png)
- 串口下载
  - ![image-20240814174020637](./images/stm32/image-20240814174020637.png)
  - CH340是基于USB的串行转USB的通信芯片，广泛应用于微控制器和USB到UART转换，以实现硬件之间的通信
    - 硬件连接
      - USB接口：CH340的USB端口用于与电脑或其他USB设备连接，接收和传输数据
      - **UART接口**：通常通过两个引脚（一个用于接收，一个用于发送）与微控制器或串行设备连接，完成串行数据的交换
    - 工作原理
      - USB驱动程序
        - CH340芯片通常需要配合USB驱动程序使用，以便操作系统能够识别和使用这个USB设备。驱动程序提供了硬件抽象层，允许应用程序通过标准的串行通信库（如Windows下的COM端口或Unix/Linux下的文件描述符）与设备进行通信。
      - 芯片内部结构
        - **USB主机控制器**：负责与USB总线的通信，接收数据包并解析它们。
        - **数据缓冲和处理单元**：用于处理接收到的数据包，进行错误校验、数据格式转换等操作。
        - **UART引擎**：用于处理UART数据的收发，将接收到的串行数据转换为USB数据包，反之亦然。
      - 通信协议
        - **USB通信协议**：遵循USB规范，数据在USB总线上以特定的格式传输。
        - **UART通信协议**：遵循通用的串行通信标准，用于设备间的点对点通信。
    - 应用场景
      - **Arduino和其他微控制器**：通过CH340连接Arduino等微控制器到电脑或其他设备，实现编程、数据采集、控制等功能。
      - **USB调试和编程**：用于调试和对嵌入式系统的编程，如更新固件或执行测试代码。
      - **传感器或设备的串行数据传输**：将传感器的输出或设备的数据通过USB接口传输到PC或其他设备进行分析或显示。
    - 注意事项
      - 使用CH340时，可能需要安装特定的驱动程序和库，以确保与操作系统兼容并正确配置。此外，电路设计时需要确保电源管理正确，防止电源波动影响通信稳定性。
    - 总结
      - CH340通过将串行数据与USB数据格式转换，实现了微控制器和PC之间的通信，广泛应用于需要USB接口的嵌入式系统开发中。理解其基本原理和应用可以有效促进电子项目的发展。

#### 4.3 IO分配

- IO分配原则
  - 优先分配特定外设IO：如UATR1的PA9、PA10
  - 然后分配通用外设IO
  - 最后微调，根据原理图的布局合理性进行IO口的微调
    - 微调原则：就近原则，线路布局合理

### 5. 搭建开发环境

#### 5.1 常用开发工具简介

- 常用工具简介
  - ![image-20240815095722604](./images/stm32/image-20240815095722604.png)
    - 推荐使用标红的
- MDK简介
  - ![image-20240815100400574](./images/stm32/image-20240815100400574.png)
    - 这里直接下载破解版
- MDK安装：MDK软件安装+器件支持包
  - 安装路径不能有中文，且安装路径越短越好

#### 5.2 安装仿真器驱动 安装USB虚拟串口驱动

- ST LINK驱动安装

  - 软件位置：E:\WWW\stm32F1\【正点原子】精英STM32F103开发板 V2-资料盘(A盘)\6，软件资料\6，软件资料\1，软件\5，其他软件\5，其他软件\ST LINK驱动及教程\ST-LINK官方驱动\ST-LINK官方驱动\dpinst_amd64.exe
  - 电脑是64位的，就安装64位的驱动
  - ![image-20240815104706206](./images/stm32/image-20240815104706206.png)

- ST LINK下载调试

  - ![image-20240815104825161](./images/stm32/image-20240815104825161.png)

- 使用ST LINK下载程序

  - 打开一个跑马灯例程

  - 编译无误后，点击

    - ![image-20240815104955327](../../Personal%20notes/0/yqnodes/docs/notes/embedded/images/%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D/image-20240815104955327.png)

    - 进入设置界面

      - ![image-20240815105142640](./images/stm32/image-20240815105142640.png)
      - 根据上图内标号，第二步选择ST LINK驱动，然后点击设置
      - ![image-20240815105313402](./images/stm32/image-20240815105313402.png)
      - 开发板通电，接上ST LINK，并且下载好驱动后，标号2这会显示ST LINK驱动
      - 标号3处选SW，因为占用IO少，下面的频率一般是4MHZ，通常手动设置为10MHZ，MDK会自动匹配
      - 标号3 表示 MDK 通过仿真器的 SW 接口找到了目标芯片，ID 为0x1BA01477。如果这里显示：No target connected，则表示没找到任何器件，请检查仿真器和开发板连接是否正常？开发板是否供电了？
      - 其他部分使用默认就好

      - 然后点击Flash Download，进入FLASH算法设置
        - ![image-20240815105912449](./images/stm32/image-20240815105912449.png)
        - 这里 MDK5 会根据我们新建工程时选择的目标器件，自动设置 flash 算法。我们使用的是 STM32F103ZET6，FLASH 容量为 512K 字节，所以 Programming Algorithm 里面默认会有 512K 型号的 STM32F10x High-density Flash 算法。另外，如果这里没有 flash 算法，大家可以点击 Add 按钮，自行添加即可。最后，选中 Reset and Run 选项，以实现在编程后自动运行，其他默认设置即可。

- 程序下载
  - 设置好一系列参数后，点击下载按钮，ST LINK连接上时常亮，下载时闪烁，下载完成后恢复常亮

### 6. MDK5使用技巧

#### 6.1 文本美化

##### 6.1.1 编辑器设置

- ![image-20240815154400897](./images/stm32/image-20240815154400897.png)
- 点击设置，进入设置界面
- 标号2处设置语言为这个，可以使用中文，然后勾选上3，即显示空白
- 标号4、5、6分别是不同文件使用tab键替代4个空格 

##### 6.1.2 字体和颜色设置

- ![image-20240815160245423](./images/stm32/image-20240815160245423.png)
- 

##### 6.1.3 用户关键字设置

- ![image-20240815160717241](./images/stm32/image-20240815160717241.png)
- 这里添加关键字，比如数据类型，关键字使用时会标红

##### 6.1.4 代码提示&语法检测

- ![image-20240815160820043](./images/stm32/image-20240815160820043.png)

- 代码提示，即写3个字符就会提示

  - ![image-20240815160912942](./images/stm32/image-20240815160912942.png)

  

##### 6.1.5 global.prop文件妙用

- global.prop文件是上面4设置的参数的一个保存文件，就是参数文件，一旦软件被卸载重装，这些参数就会恢复初始化，这是把已经有的参数文件复制到指定路径，即可获取已经设置的参数效果
- global.prop文件存放路径：例 D:\MDK5.36\UV4\global.prop
  - 设置好各项参数后，可以把这个文件复制出来保存，等之后重新下载的时候复制到指定路径即可使用

#### 6.2 代码编辑技巧

##### 6.2.1 Tab键的妙用

- ![image-20240815164253462](./images/stm32/image-20240815164253462.png)

##### 6.2.2 快速定位函数或变量被定义的地方

- ![image-20240815164309537](./images/stm32/image-20240815164309537.png)

##### 6.2.3 快速注释&快速取消注释

- ![image-20240815164414916](./images/stm32/image-20240815164414916.png)

#### 6.3 查找&替换技巧

##### 6.3.1 快速打开头文件

- ![image-20240815164703158](./images/stm32/image-20240815164703158.png)

##### 6.3.2 查找功能

- ![image-20240815164724346](./images/stm32/image-20240815164724346.png)
- 查找选项里，上图中有三项，
  - 勾选的匹配大小写，就会查找大写或小写
  - 勾选匹配整个词，只有输入整个词才能查找到
- 根据实时情况进行勾选，可提高编程效率

##### 6.3.3 查找替换功能

- ![image-20240815165529201](./images/stm32/image-20240815165529201.png)
- Look in：替换范围在当前文件内
- search up：向上搜，不勾选的话默认向下搜查

#### 6.4 工程编译问题定位

- ![image-20240815165717580](./images/stm32/image-20240815165717580.png)

#### 6.5 窗口视图管理

- ![image-20240815165756830](./images/stm32/image-20240815165756830.png)

### 7. STM32初体验

- ![image-20240815171234357](./images/stm32/image-20240815171234357.png)
  - 标号1串口通过USB线连接电脑，实现USB转串口，同时也支持给开发板供电
  - 串口处接USB线与PC端连接，同时保证PC内已经下载了CH340的驱动，连接成功后，板子上电，电源蓝色灯常亮，可使用软件下载程序
  - ![image-20240815172223225](./images/stm32/image-20240815172223225.png)
    - 打开串口下载软件，按如上步骤操作即可成功编程

### 8. STM32基础知识

#### 8.1 STM32系统框架

##### 8.1.1 Cortex M内核&芯片

![image-20240820153955925](./images/stm32/image-20240820153955925.png)

- ARM公司负责芯片内核（简称CM3）和系统调试，即内核设计与IP授权
- 芯片设计由合作伙伴完成
  - 芯片公司得到CM3内核授权后，就可以把CM3内核用到自己的硅片设计中，比如添加存储器、外设、IO以及其他功能块，不同的厂家设计出的单片机会有不同的配置，因此市面上有各种不同配置的ARM芯片

##### 8.1.2 F1系统架构

- ![image-20240820154202758](./images/stm32/image-20240820154202758.png)
  - 主动单元可以驱动被动单元，反之不行
- F1系统架构
  - ![image-20240820155701392](./images/stm32/image-20240820155701392.png)
  - 总线矩阵左边连接的四条信号线分别是4主动单元，右边连接的4个分别是4个被动单元，没接到总线矩阵的都不算事驱动单元
  - 单元介绍
    - **I Code 总线（I-Bus）**：这是Cortex M3的内核指令总线，连接闪存指令接口（如Flash），用于获取指令
    - **D Code 总线（D-Bus）**：Cortex M3内核的数据总线，连接闪存存储器FLASH接口，用于各种数据的访问，如常量和变量
    - **系统总线（S-Bus）**：CM3的系统总线，连接所有外设，用于控制各种外设，如配置各种外设相关的寄存器
    - **DMA总线**：DMA是直接存储访问控制器，可以实现数据的自动搬运，整个过程不需要CPU处理。可以节省CPU支出，提高CPU处理效率
    - **内部FLASH**：单片机的硬盘，用于代码/数据存储
    - **内部SRAM**：单片机的内存，用于数据存储，直接挂载在总线矩阵上，CPU通过DCode总线实现0等待延时访问SRAM，最快总线频率可达72Mhz，从而保证高效高速的访问内存
    - **FSMC**：灵活的静态存储控制器，实际上就是一个外部总线接口，可以用来访问外部SRAM、NAND/NOR FLASH、LCD等。它也是直接挂在总线矩阵上面的，以方便CPU快速访问外挂器件
    - **AHB/APB桥**：AHB总线连接总线矩阵，同时通过2个APB桥连接APB1和APB2，AHB总线速度最大为72Mhz，APB2总线速度最大也是72Mhz，但是APB1总线速度最大只能是36Mhz。这三个总线上面挂载了STM32内部绝大部分外设
    - **总线矩阵**：总线矩阵协调内核系统总线和DMA主控总线之间的访问仲裁，仲裁利用轮换算法，保证各个总线之间的有序访问，从而确保工作正常

##### 8.1.3 F4系统架构

##### 8.1.4 F7系统架构

##### 8.1.5 H7系统架构

#### 8.2 STM32的寻址范围

- ![image-20240820162359461](./images/stm32/image-20240820162359461.png)
- 首先明确寻址范围需要知道：地址大小，寻址单位
  - 32位的单片机，有32跟地址线，每根地址线都有两种状态，就是每根地址线都有两个地址编号，那么32位就有2^32个地址编号
    - 2^32 = 4GB
    - STM32寻址范围:0x0000 0000~0xFFFF FFFF，16进制的寻址范围转为10进制就是0~2^32-1
  - 寻址单位是字节，所以STM32有2^32个字节地址

#### 8.3 存储器映射

- 存储器映射：对存储器地址分配的过程就是存储器映射

- 存储器：FLASH、SRAM 

- ![image-20240820170427878](./images/stm32/image-20240820170427878.png)

  - 重点是前三块的学习

  - ![image-20240820170503277](./images/stm32/image-20240820170503277.png)

  - ![image-20240820170528125](./images/stm32/image-20240820170528125.png)

  - ![image-20240820170535956](./images/stm32/image-20240820170535956.png)

    

#### 8.4 寄存器映射

- 寄存器是单片机内部一种特殊的内存，可以实现对单片机各个功能的控制
- 简单来说：寄存器是单片机内部的控制机构
  - 例如一个8位的寄存器，每一位的状态分别对应不同的功能

##### 8.4.1 寄存器分类

- ![image-20240821091752412](./images/stm32/image-20240821091752412.png)
  - 重点是外设寄存器

##### 8.4.2 寄存器映射

- 寄存器是特殊的存储器，给寄存器地址命名的过程就是寄存器映射
- ![image-20240821092138780](./images/stm32/image-20240821092138780.png)

##### 8.4.3 寄存器描述

- ![image-20240821093154235](./images/stm32/image-20240821093154235.png)
  - 寄存器偏移量：根据数据手册
  - ![image-20240821093657674](./images/stm32/image-20240821093657674.png)
  - 寄存器起始地址如上，偏移量是在起始地址的基础上往后偏移
- 寄存器映射举例
  - ![image-20240821093836047](./images/stm32/image-20240821093836047.png)
  - 一个寄存器地址本身是一个指针，然后对指针类型强转，之后用解引用运算符获取地址内的值，同时也可以修改寄存器内的值，这个过程就是寄存器映射

##### 8.4.4 寄存器地址的计算

- 计算公式：总线基地址+外设偏移量+寄存器偏移量

- ![image-20240821101259970](./images/stm32/image-20240821101259970.png)

- 查参考手册，寄存器地址=寄存器外设的基地址+寄存器偏移量

- 举例 GPIOA_ODR寄存器地址计算

  - 查数据手册得GPIOA外设的地址：0x40010800![image-20240821101633492](./images/stm32/image-20240821101633492.png)

  - 寄存器偏移量

    ![image-20240821101735829](./images/stm32/image-20240821101735829.png)

  - 寄存器地址：0x40010800+0x0C=0x4001080C

##### 8.4.5 寄存器映射方法

- 使用结构体定义每个外设涉及到的所有结构体
  - ![image-20240821104638302](./images/stm32/image-20240821104638302.png)
  - 在结构体里，每个寄存器占4个字节，跟参考手册里的寄存器偏移量一致
  - 将GPIOA强制转换为结构体指针类型
  - &GPIOA->CRL：就是取结构体指针GPIOA内CRL元素的地址
  - GPIOA->CRL：获取该元素的值

### 9. 认识HAL库

#### 9.1 初始HAL库

##### 9.1.1 CMSIS简介

- ![image-20240821113714170](./images/stm32/image-20240821113714170.png)
- 

##### 9.1.2 HAL库简介

- ![image-20240821113733775](./images/stm32/image-20240821113733775.png)

  

#### 9.2 STM32Cube固件包浅析

##### 9.2.1 如何获取STM32Cube固件包

- 进入ST官网：https://www.st.com/
- 然后直接搜索STM32Cube
- 进入界面后选择STM32F1
- 然后进入下载界面，需要填邮箱信息，然后点击下载
  - ![image-20240821114834623](./images/stm32/image-20240821114834623.png)
  - 然后会发邮件确认下载，打开邮件点下载后，网页会下载固件包

##### 9.2.2 固件包文件夹简介

- ![image-20240821144458680](./images/stm32/image-20240821144458680.png)
- 重点是框出的三个文件
- ![image-20240821144546246](./images/stm32/image-20240821144546246.png)
- 

##### 9.2.3 CMSIS文件夹关键文件

- ![image-20240821144651658](./images/stm32/image-20240821144651658.png)

- ![image-20240821144726227](./images/stm32/image-20240821144726227.png)

- ![image-20240821144741369](./images/stm32/image-20240821144741369.png)

  

#### 9.3 HAL库框架结构

##### 9.3.1 HAL库文件夹结构

- ![image-20240821151050661](./images/stm32/image-20240821151050661.png)

  - 这里体现出HAL库跟LL库是捆绑发布的

    

##### 9.3.2 HAL库文件介绍

- ![image-20240821151142729](./images/stm32/image-20240821151142729.png)
  - 这些文件以后编程中都会用到

##### 9.3.3 HAL库API函数和变量命名规则

- ![image-20240821151235000](./images/stm32/image-20240821151235000.png)
- ![image-20240821151250372](./images/stm32/image-20240821151250372.png)
- ![image-20240821151338861](./images/stm32/image-20240821151338861.png)
  - _weak修饰的函数是弱函数，用时可以重新定义

#### 9.4 如何使用HAL库

- ![image-20240826130651900](./images/stm32/image-20240826130651900.png)
- ![image-20240826130643354](./images/stm32/image-20240826130643354.png)

- STM32开发文件结构分布

  - ![image-20240826130739240](./images/stm32/image-20240826130739240.png)

- ![image-20240826130823314](./images/stm32/image-20240826130823314.png)

- ![image-20240826130921971](./images/stm32/image-20240826130921971.png)

  

#### 9.5 HAL库使用注意事项

- ![image-20240826131005672](../../Personal%20notes/0/yqnodes/docs/notes/embedded/images/%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D/image-20240826131005672.png)

  

### 10. STM32启动过程浅析

#### 10.1 MAP文件浅析

- ![image-20240826134558772](./images/stm32/image-20240826134558772.png)
- map文件：对分析程存储占用情况很有用，起到全局调用分配的作用
  - 概念：包括了各种.c文件、函数、符号等的地址、大小、引用关系等信息
  - 作用：分析各.c文件占用FLASH和RAM的大小，方便优化代码
- map文件组成：
  - ![image-20240826134925081](./images/stm32/image-20240826134925081.png)
- map文件实操
  - 学会分析那个.c文件占用的flash和sram空间比较大
  - ![image-20240826135424599](./images/stm32/image-20240826135424599.png)
    - 双击文件名即可打开.map文件，然后可以分析各个函数占用的空间

#### 10.2 STM32启动过程

##### 10.2.1 STM32启动模式

- 启动模式/自举模式
  - ![image-20240826140914692](./images/stm32/image-20240826140914692.png)
  - 地址映射
    - 地址映射是指将逻辑地址或虚拟地址转换为物理地址的过程。
    - 在STM32等微控制器中，CPU通过地址来访问内存和外设。
    - 逻辑地址是CPU的抽象地址，不直接对应物理内存中的位置
    - 物理地址则是实际的硬件地址，指向内存模块中的特定位置
  - 地址映射总结
    - STM32启动过程中的地址映射是确保CPU能够正确访问内部或外部存储器中数据和指令的关键步骤。通过地址映射，CPU可以将逻辑地址转换为物理地址，并访问存储在相应存储器中的数据和指令。这种机制使得STM32能够灵活地支持不同的启动模式和程序执行方式。
    - ![image-20240826142204259](./images/stm32/image-20240826142204259.png)
- STM32F1
  - ![image-20240826142306061](./images/stm32/image-20240826142306061.png)
- STM32F4
  - ![image-20240826142326847](./images/stm32/image-20240826142326847.png)

##### 10.2.2 STM32启动过程

- STM32启动过程，以内部FLASH启动为例
  - ![image-20240826143236775](./images/stm32/image-20240826143236775.png)
  - 0x00000000映射到0x08000000
  - MSP值：堆栈指针的值
  - PC值：复位向量的值

- 启动文件介绍

  - ![image-20240826145132751](./images/stm32/image-20240826145132751.png)

  - ![image-20240826145221664](./images/stm32/image-20240826145221664.png)

  - 堆栈大小可以在该文件内修改

    - ![image-20240826145938114](./images/stm32/image-20240826145938114.png)

    

- Reset_Handler函数介绍

  - ![image-20240826145516141](./images/stm32/image-20240826145516141.png)
  - 复位-中断函数是汇编函数

- 堆栈简介

  - ![image-20240826145554888](./images/stm32/image-20240826145554888.png)
    - 栈溢出会导致程序跑死、死机，这种情况下需要加大栈的大小
  - 堆栈大小在启动文件中设置
  - ![image-20240826145958435](./images/stm32/image-20240826145958435.png)
  - 

- STM32启动过程图解

  - ![image-20240826145633588](./images/stm32/image-20240826145633588.png)
  - 堆：向上生长，即低地址向高地址
  - 栈：向下生长，即高地址向低地址

### 11. STM32时钟树

#### 11.1 STM32时钟树

##### 11.1.1 STM32时钟数(F1)

- 时钟：单片机的脉搏，通常是50%的脉冲方波
- F1系列时钟树简图
  - ![image-20240827144437030](./images/stm32/image-20240827144437030.png)
  - 简单来说，就是时钟的选择、乘法、除法
    - 系统时钟最大要72MHz，使用HSI的话，需要先2分频，然后最大就是4*16 = 64MHz，达不到最大，所以这里选择HSE作为系统时钟来源
- F1系列时钟树
  - ![image-20240827144851902](./images/stm32/image-20240827144851902.png)
- F1系列CobeMX时钟树
  - ![image-20240827145125389](./images/stm32/image-20240827145125389.png)

##### 11.1.2 STM32时钟数(F4)

##### 11.1.3 STM32时钟数(F7)

##### 11.1.4 STM32时钟数(H7)

#### 11.2 系统时钟配置

##### 11.2.1 系统时钟配置步骤

- ![image-20240827160349100](./images/stm32/image-20240827160349100.png)

  

##### 11.2.2 外设时钟使能和失能

- ![image-20240827160405846](./images/stm32/image-20240827160405846.png)

  

##### 11.2.3 sys_stm32_clock_init 函数(F1) 

![image-20240827160439313](./images/stm32/image-20240827160439313.png)

- ![image-20240827160520277](./images/stm32/image-20240827160520277.png)

- ![image-20240827160550052](./images/stm32/image-20240827160550052.png)

  

##### 11.2.4 sys_stm32_clock_init 函数(F4/F7) 

##### 11.2.5 sys_stm32_clock_init 函数(H7) 

### 12. SYSTEM文件夹介绍

#### 12.1 sys文件夹介绍

- ![image-20240828102945083](./images/stm32/image-20240828102945083.png)
  - 主要是掌握系统时钟初始化函数

#### 12.2 delay文件夹介绍

##### 12.2.1 delay文件夹函数介绍

- ![image-20240828103324646](./images/stm32/image-20240828103324646.png)
  - 不使用OS指裸机开发

##### 12.2.2 Systick工作原理

- ![image-20240828104018909](./images/stm32/image-20240828104018909.png)
- 系统滴答定时器在内核里，是一个24位的递减计数器，来一个时钟脉冲，计数器-1，直到减为0，即VAL值从2^24减到0，代表一次延时完成，计数器标志位COUNTFLAG置1，之后VAL自动从LOAD重载，即LOAD是一个寄存器，0-2^24-1

##### 12.2.3 Systick寄存器介绍

- ![image-20240828104747048](./images/stm32/image-20240828104747048.png)
- ![image-20240828104757100](./images/stm32/image-20240828104757100.png)

##### 12.2.4 delay_init()函数

- ![image-20240828112606258](./images/stm32/image-20240828112606258.png)

  - 第一行是将寄存器的数值清0，将HAL库默认的配置清0

  - 第二行是将时钟源进行8分频

  - 第三行：

    - g_fac_us：全局变量，1us实际的来源

    - 1/1^6 = 1us，即频率是1M的话，1秒要数1^6次，一次要1/1^6s，刚搞数1次要1us

    - 那么频率是1M的倍数的话，1us就要数1/1^6*倍数

    - 这里将1M的倍数定义为全局变量**g_fac_us**

    - 在微秒延时函数里，传入的参数就要乘**g_fac_us**，代表数这么多次等于要延时的us数

      

##### 12.2.5 delay_us()函数

- ![image-20240828114211824](./images/stm32/image-20240828114211824.png)

  

##### 12.2.6 delay_ms()函数

- ![image-20240828114746734](./images/stm32/image-20240828114746734.png)
- 超频：这里计算128MHZ经8分频后是16M，16M可计数1.048576s，即1048576us，为了避免超频，这里将ms计算分为整秒数和秒数的余数，1s = 1000000us，这样就不会有超频的可能

#### 12.3 usart文件夹介绍

##### 12.3.1 printf函数输出流程

- ![image-20240828134731322](./images/stm32/image-20240828134731322.png)

- printf重定向：用户需要根据最终输出的硬件重新定义该函数

- 重定向：指的是改变数据或指令的正常流向

- ![image-20240828154027198](./images/stm32/image-20240828154027198.png)

  

##### 12.3.2 printf的使用

- ![image-20240828135633802](./images/stm32/image-20240828135633802.png)

- ![image-20240828135700633](./images/stm32/image-20240828135700633.png)

- ![image-20240828135716820](./images/stm32/image-20240828135716820.png)

  

##### 12.3.3 printf函数重定向（HAL库）

- 在 **stm32f4xx_hal.h中包含#include <stdio.h>

  ```c
  #include "stm32f4xx_hal.h"
  #include <stdio.h>  //剪切到stm32f4xx_hal.h
  extern UART_HandleTypeDef huart1; //声明串口
  ```

- 在 **stm32f4xx_hal.c** 中重写fget和fput函数

  ```c
  /**
  * 函数功能: 重定向c库函数printf到DEBUG_USARTx
  * 输入参数: 无
  * 返 回 值: 无
  * 说 明：无
  */
  int fputc(int ch, FILE *f)
  {
  HAL_UART_Transmit(&huart1, (uint8_t *)&ch, 1, 0xffff);
  return ch;
  }
  /**
  * 函数功能: 重定向c库函数getchar,scanf到DEBUG_USARTx
  * 输入参数: 无
  * 返 回 值: 无
  * 说 明：无
  */
  int fgetc(FILE *f)
  {
  uint8_t ch = 0;
  HAL_UART_Receive(&huart1, &ch, 1, 0xffff);
  return ch;
  }
  ```

- 在 **stm32f4xx_hal.c** 中重写fget和fput函数需要在**stm32f4xx_hal.h**中声明

- 上面操作配置好后，需要勾选下面的"Use MicroLIB"，否则printf不能正常运行

  - ![image-20240828154727395](./images/stm32/image-20240828154727395.png)

### 13. GPIO

#### 13.1 什么是GPIO

- GPIO：通用输入输出端口，即检测外部器件信息和控制外部器件工作
  - ![image-20240829101940621](./images/stm32/image-20240829101940621.png)
- GPIO特点
  - ![image-20240829102022137](./images/stm32/image-20240829102022137.png)
- GPIO电气特性
  - ![image-20240829102106537](./images/stm32/image-20240829102106537.png)
  - TTL端口，在数据手册里显示FT
    - ![image-20240829102413965](./images/stm32/image-20240829102413965.png)
- GPIO引脚分布
  - ![image-20240829102443574](./images/stm32/image-20240829102443574.png)

#### 13.2 IO端口基本结构介绍

- ![image-20240829105135793](./images/stm32/image-20240829105135793.png)

##### 13.2.1 保护二极管

- ![image-20240829104510339](./images/stm32/image-20240829104510339.png)
- 保护二极管起到保护作用，压降只有0.3V，超过IO输入电压范围的电压不能直接输入，需要接限流电阻，这样输入的电压就是3.6V，可以正常输入
- 若直接输入过高或过低的电压，会烧坏保护二极管，进而烧坏芯片内部电路
- 输入电压过低情况
  - ![image-20240829104902887](./images/stm32/image-20240829104902887.png)

##### 13.2.2 内部上拉、下拉电阻

- ![image-20240829110653850](./images/stm32/image-20240829110653850.png)
- 上拉、下拉电阻阻值在30k~50k
- 例如这里使用上拉电阻，阻值取40k，电流**I=3.3/40000=0.825mA**
  - 电流很小，所以驱动能力很弱，就称为弱上拉和弱下拉

##### 13.2.3 施密特触发器

- ![image-20240829111335919](./images/stm32/image-20240829111335919.png)
- 作用：整型，将输入的正弦波转为方波，存储在输入寄存器IDR中

##### 13.2.4 P-MOS & N-MOS管

- ![image-20240829111736825](./images/stm32/image-20240829111736825.png)

#### 13.3 GPIO八种工作模式分析

- ![image-20240829112645053](./images/stm32/image-20240829112645053.png)
- 四种输出模式下，施密特触发器都是打开的，这就意味着可以读取外部器件的输入

##### 13.3.1 输入浮空

- 输入状态不定，由外部器件决定，IO口空闲状态呈高阻态
  - 高阻态：指电路的一种输出状态，既不是高电平，也不是低电平
    - 表示方法：硬件表述语言，用"Z"表示；电路图中用"X"来表示其不确定的状态
    - 这里的高阻态表示外部不接任何器件
- ![image-20240829113759902](./images/stm32/image-20240829113759902.png)

##### 13.3.2 输入上拉

- ![image-20240829114358273](./images/stm32/image-20240829114358273.png)
- IO输入3.3V时，输入1
- IO输入0V时，输入0
- IO空闲时，由弱上拉输入1

##### 13.3.3 输入下拉

- ![image-20240829114605765](./images/stm32/image-20240829114605765.png)
- IO输入3.3V时，输入1
- IO输入0V时，输入0
- IO空闲时，由弱下拉输入0

##### 13.3.4 模拟功能

- ![image-20240829114944647](./images/stm32/image-20240829114944647.png)
- 专门用于模拟信号的输入或输出

##### 13.3.5 开漏输出

- ![image-20240829143246558](./images/stm32/image-20240829143246558.png)
  - **纠正：这里的施密特触发器打开也不能检测输入信号，只能将引脚切换到输入模式才可以检测输入信号**

- 只能输出低电平和高阻态
- 高阻态时，可以外接上拉电阻，实现输出1，电阻上接5V的话，就可以实现5V的输出
- 高阻态时，F4/F7/H7系列可以打开上拉电阻实现输出1

##### 13.3.6 推挽输出

- ![image-20240829143831172](./images/stm32/image-20240829143831172.png)
- 

##### 13.3.7 复用开漏输出

- ![image-20240829143557582](./images/stm32/image-20240829143557582.png)
- 与开漏输出的区别是输出控制源不一样，这里是来自片上外设

##### 13.3.8 复用推挽输出

- ![image-20240829150104133](./images/stm32/image-20240829150104133.png)

##### 13.3.9 F1与F4/F7/H7的IO区别

- ![image-20240829150933596](./images/stm32/image-20240829150933596.png)
- 唯一区别是上下拉电阻的位置
- ![image-20240829151011433](./images/stm32/image-20240829151011433.png)

#### 13.4 GPIO寄存器介绍

- ![image-20240829154400903](./images/stm32/image-20240829154400903.png)
- ![image-20240829154422718](./images/stm32/image-20240829154422718.png)
  - CRL配置GPIOx0~7的工作模式和输出速度
- ![image-20240829154437749](./images/stm32/image-20240829154437749.png)
  - CRH配置GPIOx8~15的工作模式和输出速度
- ![image-20240829154720981](./images/stm32/image-20240829154720981.png)
  - 配置上拉或下拉输入时，需要用ODR确定是上拉还是下拉
- ![image-20240829154821751](./images/stm32/image-20240829154821751.png)
- ![image-20240829154847494](./images/stm32/image-20240829154847494.png)
  - ODR可以用来设置IO的上下拉输出
  - 也可以设置对应的IO口0/1
- ![image-20240829155037345](./images/stm32/image-20240829155037345.png)

#### 13.5 通用外设驱动模型（4步法）

- ![image-20240829160615907](./images/stm32/image-20240829160615907.png)

#### 13.6 GPIO配置步骤

- ![image-20240829160636027](./images/stm32/image-20240829160636027.png)

#### 13.7 编程实战

- 点亮一个LED灯
- 通过按键控制一个LED灯亮灭

### 14. 中断

#### 14.1 什么是中断

- 中断：打断CPU正常执行的程序，转而进行更为紧急的程序，处理完之后返回之前暂停的原程序继续处理
  - ![image-20240830164454895](./images/stm32/image-20240830164454895.png)
- 中断的作用和意义：高效处理紧急程序，不会一直占用资源
  - ![image-20240830164557695](./images/stm32/image-20240830164557695.png)
- GPIO外部中断简图
  - ![image-20240830164748302](./images/stm32/image-20240830164748302.png)

##### 14.1.1 中断触发的基本原理--以WWDG举例

- 中断条件：WWDG中断的触发条件通常是通过硬件定时器计数来达成的，当WWDG计数器递减到设定的阈值时，会产生一个中断请求
- 中断请求生成：一旦条件满足，WWDG硬件会向NVIC发出中断请求，这会驱动控制芯片进入对应的中断处理函数
- 中断处理函数：在此情况下，CPU会中断当前执行的任务，保存上下文，跳转到WWDG中断服务函数，开始执行

#### 14.2 NVIC

##### 14.2.1 NVIC基本概念

- ![image-20240830171359857](./images/stm32/image-20240830171359857.png)
- NVIC：嵌套向量中断控制器，属于内核
- 什么是中断向量表
  - ![image-20240830171456590](./images/stm32/image-20240830171456590.png)
  - 定义一块固定内存，以4字节对齐，存放各个中断服务函数程序的首地址
  - 中断向量表存放在启动文件中，每次CPU启动都会先进入启动文件，若是有中断事件，就会先执行中断事件，然后才执行main()函数
    - 任意一个中断事件的优先级都高于main()函数

##### 14.2.2 NVIC相关寄存器介绍

- ![image-20240830172038686](./images/stm32/image-20240830172038686.png)
- 中断使能寄存器(ISER)
  - 共有8个，每个寄存器有32位，共有32*8=256位，外部有240个寄存器，这里一位对应一个中断，还有16位空闲
- ICER：跟ISER对应，也是240位分别对应240中断，有16位空闲

##### 14.2.3 NVIC工作原理

- ![image-20240830173250379](./images/stm32/image-20240830173250379.png)
- IPR与SHPR同级，进入的中断在一个级别内比较哪个优先级更高

##### 14.2.4 STM32中断优先级概念

- ![image-20240830174251803](./images/stm32/image-20240830174251803.png)

  

##### 14.2.5 STM32中断优先级分组

- ![image-20240830174340423](./images/stm32/image-20240830174340423.png)
- 一个工程内，只设置一次中断优先级分组
- AIRCR[10:8]：优先级分组0~4是按照111~011，是111逐次减1得到的
- 可参考STM32F1xx参考手册
  - ![image-20240830174824977](./images/stm32/image-20240830174824977.png)
- 举例说明
  - ![image-20240830174857292](./images/stm32/image-20240830174857292.png)
    - 编号：就是在向量表里的位置
    - 数值越小，优先级越高，包括自然优先级
    - 排序法则
      - 抢占优先级高的先执行
      - 抢占优先级相同，响应优先级高的先执行
      - 若抢占优先级和响应优先级都相同的话，自然优先级高的先执行

##### 14.2.6 STM32 NVIC的使用

- 经常使用的三个函数

  - ![image-20240902150815632](./images/stm32/image-20240902150815632.png)

  - 三个函数来源：

    - ![image-20240902151215636](./images/stm32/image-20240902151215636.png)

  - ```c
    /*
    函数功能:设置中断分组
    函数参数:中断分组数值，可选0~4
    */
    void HAL_NVIC_SetPriorityGrouping(uint32_t PriorityGroup);
    /*
    函数功能:设置中断优先级
    函数参数：
    	IRQn:中断名字
    	PreemptPriority:抢占优先级
    	SubPriority:响应优先级
    */
    void HAL_NVIC_SetPriority(IRQn_Type IRQn, uint32_t PreemptPriority, uint32_t SubPriority);
    /*
    函数功能:使能中断
    函数参数：
    	IRQn:中断名字
    */
    void HAL_NVIC_EnableIRQ(IRQn_Type IRQn);
    ```

    

#### 14.3 EXTI

##### 14.3.1 EXTI基本概念

![image-20240902153149305](./images/stm32/image-20240902153149305.png)

- ![image-20240902153306329](./images/stm32/image-20240902153306329.png)
  - EXTI作用：管理芯片外部、内部唤醒中断/事件

##### 14.3.2 EXTI主要特性

- ![image-20240902153958257](./images/stm32/image-20240902153958257.png)

  

##### 14.3.3 EXTI工作原理（F1/F4/F7）

- ![image-20240902155912512](./images/stm32/image-20240902155912512.png)
- 输入线实际上有19位，实际对应的寄存器也是19位
- 上升沿触发寄存器
  - ![image-20240902160149641](./images/stm32/image-20240902160149641.png)
  - 对应的位置1，就允许上升沿
  - 置0则禁止上升沿
- ![image-20240902160254363](./images/stm32/image-20240902160254363.png)
- ![image-20240902160411036](./images/stm32/image-20240902160411036.png)
- ![image-20240902160440671](./images/stm32/image-20240902160440671.png)

##### 14.3.4 EXTI工作原理（H7）

#### 14.4 EXTI与IO映射关系

##### 14.4.1 AFIO简介（F1）

![image-20240902161810737](./images/stm32/image-20240902161810737.png)

##### 14.4.2 SYSCFG简介（F4/F7/H7）

##### 14.4.3 EXTI与IO对应关系

- 如何确定EXTI0与Px0的对应关系，即如何确定EXTI0对应的是那一组的IO(A/B/C......)
- 通过配置4个寄存器实现
- ![image-20240902162125507](./images/stm32/image-20240902162125507.png)
- ![image-20240902162251436](./images/stm32/image-20240902162251436.png)
  - AFIO_EXTICR1的前16位分为4组，分别控制EXTI0-EXTI3的IO对应关系
    - ![image-20240902162500491](./images/stm32/image-20240902162500491.png)
- 同理AFIO_EXTICR2~4同理，分别对应EXTI4-EXTI7、EXTI8-EXTI11、EXTI12-EXTI15

#### 14.5 如何使用中断

- ![image-20240902165757368](./images/stm32/image-20240902165757368.png)
- STM32 EXTI配置步骤
  - ![image-20240902165825064](./images/stm32/image-20240902165825064.png)
- STM32 EXTI的HAL库设置步骤（外部中断）
  - ![image-20240902165847227](./images/stm32/image-20240902165847227.png)

#### 14.6 通用外设驱动模型

- ![image-20240902165931089](./images/stm32/image-20240902165931089.png)
  - EXTI用到了初始化和中断服务函数
  - 实操配置---NVIC分组设置函数路径
    - ![image-20240903000058537](./images/stm32/image-20240903000058537.png)
    - ![image-20240903000213600](./images/stm32/image-20240903000213600.png)

#### 14.7 HAL库中断回调处理机制介绍

- ![image-20240902170015546](./images/stm32/image-20240902170015546.png)
- 

#### 14.8 编程实战

- 使用外部中断控制一个灯的亮灭

  - 实现原理：中断在启动文件中设置，优先级高于main.c，一旦有中断发生，主函数就暂停，中断完成后，主函数继续

- 中断服务函数编写

  - HAL库中，中断服务函数在**stm32f1xx_it.c**中编写

  - ```c
    /**
      * @brief This function handles EXTI line3 interrupt.
      */
    void EXTI3_IRQHandler(void)
    {
      /* USER CODE BEGIN EXTI3_IRQn 0 */
        
      /* USER CODE END EXTI3_IRQn 0 */
      HAL_GPIO_EXTI_IRQHandler(KEY1_Pin);  //自带，HAL库中断处理共用函数，检测到中断后，调用对应的中断回调函数
      /* USER CODE BEGIN EXTI3_IRQn 1 */
        __HAL_GPIO_EXTI_CLEAR_IT(KEY1_Pin);   //清中断
      /* USER CODE END EXTI3_IRQn 1 */
    }
    
    /* USER CODE BEGIN 1 */
    //中断回调函数，该函数在stm32f1xx_hal_gpio.c中若定义，函数声明也在其他函数中声明过了，这里只需要重新定义，不需要额外声明
    void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
    {
        HAL_Delay(20);   //按键消抖
        if(GPIO_Pin == KEY1_Pin)   //判断是否是检测到的引脚
        {
            if(HAL_GPIO_ReadPin(KEY1_GPIO_Port,KEY1_Pin) == GPIO_PIN_RESET)
            {
                HAL_GPIO_TogglePin(LED1_GPIO_Port,LED1_Pin);  //KEY1按下，LED1状态翻转
                HAL_Delay(500); //延时，使LED1状态更稳定
            }
        }
    }
    /* USER CODE END 1 */
    
    ```

- GPIO的配置在gpio.c中

- main.c

  - ```c
    int main(void)
    {
      /* USER CODE BEGIN 1 */
    
      /* USER CODE END 1 */
    
      /* MCU Configuration--------------------------------------------------------*/
    
      /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
      HAL_Init();  //包含NVIC 时钟使能、分组
    
      /* USER CODE BEGIN Init */
    
      /* USER CODE END Init */
    
      /* Configure the system clock */
      SystemClock_Config();
    
      /* USER CODE BEGIN SysInit */
    
      /* USER CODE END SysInit */
    
      /* Initialize all configured peripherals */
      MX_GPIO_Init();   //包括中断优先级、中断使能设置
      /* USER CODE BEGIN 2 */
    
      /* USER CODE END 2 */
    
      /* Infinite loop */
      /* USER CODE BEGIN WHILE */
      while (1)
      {
          HAL_GPIO_TogglePin(BEEP_GPIO_Port,BEEP_Pin);   //BEEP状态翻转
          HAL_Delay(500);
        /* USER CODE END WHILE */
    
        /* USER CODE BEGIN 3 */
      }
      /* USER CODE END 3 */
    }
    
    ```

    

- NVIC软件开发设置

  - ![image-20240903113026001](./images/stm32/image-20240903113026001.png)

    - 在3处选择分组模式
    - 4处是系统滴答定时器的优先级，与delay函数有关，这里的优先级应该高于中断的
    - 5处使能中断，第一个抢占优先级，第二个是响应优先级

  - NVIC在代码中设置位置  **stm32f1xx_hal_msp.c**

    - ```c
      void HAL_MspInit(void)  //底层硬件初始化
      {
        /* USER CODE BEGIN MspInit 0 */
      
        /* USER CODE END MspInit 0 */
      
        __HAL_RCC_AFIO_CLK_ENABLE();  //使能时钟
        __HAL_RCC_PWR_CLK_ENABLE();
      
        HAL_NVIC_SetPriorityGrouping(NVIC_PRIORITYGROUP_2);  //设置中断分组
      
        /* System interrupt init*/
      
        /** NOJTAG: JTAG-DP Disabled and SW-DP Enabled
        */
        __HAL_AFIO_REMAP_SWJ_NOJTAG();
      
        /* USER CODE BEGIN MspInit 1 */
      
        /* USER CODE END MspInit 1 */
      }
      
      ```

#### 14.9 中断总结

### 15. 串口

#### 15.1 数据通信的基础概念

##### 15.1.1 串行/并行通信

![image-20240903151001562](./images/stm32/image-20240903151001562.png)

##### 15.1.2 单工/半双工/全双工通信

- ![image-20240903151052981](./images/stm32/image-20240903151052981.png)

##### 15.1.3 同步/异步通信

- ![image-20240903151127412](./images/stm32/image-20240903151127412.png)
- 同步和异步的区别就是有无**时钟信号**
- 异步通信，没有时钟信号，通过加入起始位和停止位等来同步信息

##### 15.1.4 波特率

- ![image-20240903152338640](./images/stm32/image-20240903152338640.png)
- 码元：指每秒钟传输的离散信号的基本单元，这些单位可以是二进制、四进制、八进制、十六进制等
  - 多进制码元：在高级通信系统中，为了提高通信效率，可能会采用**多进制调制技术**，这种情况下，码元可以代表多个进制的信息
- 二进制时波特率等于比特率，在多进制通信时，波特率将低于比特率

##### 15.1.5 常见的串口通信接口

- ![image-20240903153106929](./images/stm32/image-20240903153106929.png)
  - 不是只要有两根信号线就是全双工，而是两根信号线分别是输入输出线才是全双工

#### 15.2 串口（RS-232）

- ![image-20240903154622384](./images/stm32/image-20240903154622384.png)
- ![image-20240903154636798](./images/stm32/image-20240903154636798.png)
- ![image-20240903154706670](./images/stm32/image-20240903154706670.png)
- ![image-20240903154719449](./images/stm32/image-20240903154719449.png)
- RS-232异步通信有两个接口
  - DB9接口：现在已经不常用
  - Type_C接口：比较流行
- RS-232异步通信协议
  - ![image-20240903155019165](./images/stm32/image-20240903155019165.png)

#### 15.3 STM32的USART

##### 15.3.1 USART简介

- ![image-20240903160311357](./images/stm32/image-20240903160311357.png)

##### 15.3.2 USART主要特性

- ![image-20240903160421974](./images/stm32/image-20240903160421974.png)
- 如何快速查看STM32某个外设的数量和对应的引脚
  - 查看ST MCU最新选型手册可以查到外设数量，ST官网可下载
    - ![image-20240903160725715](./images/stm32/image-20240903160725715.png)
    - 在文档中搜索芯片型号就可以找到
    - 表格最上面是芯片对应的外设种类，然后根据每个芯片，有对应的外设数量
  - 查具体外设对应哪个引脚需要查STM32F1xx数据手册
    - ![image-20240903161054792](./images/stm32/image-20240903161054792.png)
    - 在数据手册里搜索外设名字就可以查到对应的引脚号

##### 15.3.3 STM32F1/F7/F4的USART框图

- ![image-20240904100705364](./images/stm32/image-20240904100705364.png)
  - 发送移位寄存器：等带上一个字节发送完之后，再发送下一个字节，移位寄存器内只能存储一个字节
  - 接收移位寄存器：同上，一个时间内只能操作一个字节
- USART框图简图
  - ![image-20240904101000595](./images/stm32/image-20240904101000595.png)

###### 15.3.3.1 HAL库系统复位函数

- 外部复位函数举例

  - ```c
    int main(void)
    {
      
      HAL_Init();
    
     
      SystemClock_Config();
    
      /* USER CODE BEGIN SysInit */
    
      /* USER CODE END SysInit */
    
      /* Initialize all configured peripherals */
      MX_GPIO_Init();
      MX_USART1_UART_Init();
      /* USER CODE BEGIN 2 */
      if(__HAL_RCC_GET_FLAG(RCC_FLAG_PINRST))   //检测外部复位来源，复位返回1，否则返回0
      {
          printf("复位按键复位\r\n");
          __HAL_RCC_CLEAR_RESET_FLAGS();
      }
        
      /* USER CODE END 2 */
    
      /* Infinite loop */
      /* USER CODE BEGIN WHILE */
      while (1)
      {
          printf("hello\r\n");
          HAL_Delay(500);
        /* USER CODE END WHILE */
    
        /* USER CODE BEGIN 3 */
      }
      /* USER CODE END 3 */
    }
    ```

    

###### 15.3.3.2 HAL库串口配置重映射

- 实现printf()函数使用自由

  - 该函数主要用于输出标志性语句，不能依赖该函数输出所有，printf()函数是阻塞输出，有时间阻塞，所以只能输出标志性的语句

- 配置完串口重映射后，一定要使用微库

  - ![image-20240918100754794](./images/stm32/image-20240918100754794.png)

  - 不勾选微库的话，printf函数不能正常使用，并且会造成程序其他功能也不能正常使用

- HAL库--usart.c配置

  - ```c
    
    
    #include "usart.h"
    
    /* USER CODE BEGIN 0 */
    
    /* USER CODE END 0 */
    
    UART_HandleTypeDef huart1;
    
    /* USART1 init function */
    
    void MX_USART1_UART_Init(void)
    {
    
      /* USER CODE BEGIN USART1_Init 0 */
    
      /* USER CODE END USART1_Init 0 */
    
      /* USER CODE BEGIN USART1_Init 1 */
    
      /* USER CODE END USART1_Init 1 */
      huart1.Instance = USART1;
      huart1.Init.BaudRate = 115200;
      huart1.Init.WordLength = UART_WORDLENGTH_8B;
      huart1.Init.StopBits = UART_STOPBITS_1;
      huart1.Init.Parity = UART_PARITY_NONE;
      huart1.Init.Mode = UART_MODE_TX_RX;
      huart1.Init.HwFlowCtl = UART_HWCONTROL_NONE;
      huart1.Init.OverSampling = UART_OVERSAMPLING_16;
      if (HAL_UART_Init(&huart1) != HAL_OK)
      {
        Error_Handler();
      }
      /* USER CODE BEGIN USART1_Init 2 */
    
      /* USER CODE END USART1_Init 2 */
    
    }
    
    void HAL_UART_MspInit(UART_HandleTypeDef* uartHandle)
    {
    
      GPIO_InitTypeDef GPIO_InitStruct = {0};
      if(uartHandle->Instance==USART1)
      {
      /* USER CODE BEGIN USART1_MspInit 0 */
    
      /* USER CODE END USART1_MspInit 0 */
        /* USART1 clock enable */
        __HAL_RCC_USART1_CLK_ENABLE();
    
        __HAL_RCC_GPIOA_CLK_ENABLE();
        /**USART1 GPIO Configuration
        PA9     ------> USART1_TX
        PA10     ------> USART1_RX
        */
        GPIO_InitStruct.Pin = GPIO_PIN_9;
        GPIO_InitStruct.Mode = GPIO_MODE_AF_PP;
        GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_HIGH;
        HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
    
        GPIO_InitStruct.Pin = GPIO_PIN_10;
        GPIO_InitStruct.Mode = GPIO_MODE_INPUT;
        GPIO_InitStruct.Pull = GPIO_NOPULL;
        HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
    
        /* USART1 interrupt Init */
        HAL_NVIC_SetPriority(USART1_IRQn, 2, 3);
        HAL_NVIC_EnableIRQ(USART1_IRQn);
      /* USER CODE BEGIN USART1_MspInit 1 */
    
      /* USER CODE END USART1_MspInit 1 */
      }
    }
    
    void HAL_UART_MspDeInit(UART_HandleTypeDef* uartHandle)
    {
    
      if(uartHandle->Instance==USART1)
      {
      /* USER CODE BEGIN USART1_MspDeInit 0 */
    
      /* USER CODE END USART1_MspDeInit 0 */
        /* Peripheral clock disable */
        __HAL_RCC_USART1_CLK_DISABLE();
    
        /**USART1 GPIO Configuration
        PA9     ------> USART1_TX
        PA10     ------> USART1_RX
        */
        HAL_GPIO_DeInit(GPIOA, GPIO_PIN_9|GPIO_PIN_10);
    
        /* USART1 interrupt Deinit */
        HAL_NVIC_DisableIRQ(USART1_IRQn);
      /* USER CODE BEGIN USART1_MspDeInit 1 */
    
      /* USER CODE END USART1_MspDeInit 1 */
      }
    }
    
    /* USER CODE BEGIN 1 */
    int fputc(int ch, FILE *f)
    {
    HAL_UART_Transmit(&huart1, (uint8_t *)&ch, 1, 0xffff);
    return ch;
    }
    /**
    * 函数功能: 重定向c库函数getchar,scanf到DEBUG_USARTx
    * 输入参数: 无
    * 返 回 值: 无
    * 说 明：无
    */
    int fgetc(FILE *f)
    {
    uint8_t ch = 0;
    HAL_UART_Receive(&huart1, &ch, 1, 0xffff);
    return ch;
    }
    /* USER CODE END 1 */
    
    ```

  - usart.h

    - ```c
      /* USER CODE BEGIN Header */
      /**
        ******************************************************************************
        * @file    usart.h
        * @brief   This file contains all the function prototypes for
        *          the usart.c file
        ******************************************************************************
        * @attention
        *
        * Copyright (c) 2024 STMicroelectronics.
        * All rights reserved.
        *
        * This software is licensed under terms that can be found in the LICENSE file
        * in the root directory of this software component.
        * If no LICENSE file comes with this software, it is provided AS-IS.
        *
        ******************************************************************************
        */
      /* USER CODE END Header */
      /* Define to prevent recursive inclusion -------------------------------------*/
      #ifndef __USART_H__
      #define __USART_H__
      
      #ifdef __cplusplus
      extern "C" {
      #endif
      
      /* Includes ------------------------------------------------------------------*/
      #include "main.h"
      
      /* USER CODE BEGIN Includes */
      #include "stdio.h"
      /* USER CODE END Includes */
      
      extern UART_HandleTypeDef huart1;
      
      /* USER CODE BEGIN Private defines */
      
      /* USER CODE END Private defines */
      
      void MX_USART1_UART_Init(void);
      
      /* USER CODE BEGIN Prototypes */
      int fputc(int ch, FILE *f);
      int fgetc(FILE *f);
      /* USER CODE END Prototypes */
      
      #ifdef __cplusplus
      }
      #endif
      
      #endif /* __USART_H__ */
      
      
      ```

      

##### 15.3.4 STM32H7的USART框图

- ![image-20240904100948147](./images/stm32/image-20240904100948147.png)

##### 15.3.5 设置波特率 F1

- 寄存器版本如何设置波特率
  - ![image-20240904102455200](./images/stm32/image-20240904102455200.png)
    - baud是波特率已知，USARTDIV的值可以计算出来，然后将USARTDIV的值的整数、小数部分分别存入寄存器BRR中，就完成波特率设置
    - ![image-20240904102721114](./images/stm32/image-20240904102721114.png)
    - ![image-20240904103339592](./images/stm32/image-20240904103339592.png)
    - 公式简化
      - ![image-20240904103749778](./images/stm32/image-20240904103749778.png)
  - HAL库设置波特率
    - 直接在CobeMX中设置波特率就好，一步到位

##### 15.3.6 设置波特率 F4

##### 15.3.7 设置波特率 F7

##### 15.3.8 设置波特率 H7

##### 15.3.9 USART寄存器介绍

- 控制寄存器CR1
  - ![image-20240904105358571](./images/stm32/image-20240904105358571.png)
- 控制寄存器CR2
  - ![image-20240904105425239](./images/stm32/image-20240904105425239.png)
- 控制寄存器CR3
  - ![image-20240904105447038](./images/stm32/image-20240904105447038.png)
- 数据寄存器DR
  - ![image-20240904105504564](./images/stm32/image-20240904105504564.png)
- 状态寄存器SR
  - ![image-20240904105525995](./images/stm32/image-20240904105525995.png)
- 时序总结
  - ![image-20240904105602144](./images/stm32/image-20240904105602144.png)



#### 15.4 HAL库外设初始化MSP回调机制

- ![image-20240904111013510](./images/stm32/image-20240904111013510.png)
- ![image-20240904111109820](./images/stm32/image-20240904111109820.png)

#### 15.5 HAL库中断回调机制

- ![image-20240904113055849](./images/stm32/image-20240904113055849.png)
- ![image-20240904113140261](./images/stm32/image-20240904113140261.png)
- HAL库UART 中断共用处理函数
  - ![image-20240904113221883](./images/stm32/image-20240904113221883.png)

#### 15.6 USART/UART异步通信配置步骤

- ![image-20240904115613498](./images/stm32/image-20240904115613498.png)
- HAL库相关函数介绍
  - ![image-20240904115644428](./images/stm32/image-20240904115644428.png)
  - ![image-20240904115659080](./images/stm32/image-20240904115659080.png)
  - ![image-20240904115708046](./images/stm32/image-20240904115708046.png)

#### 15.7 IO引脚复用功能

##### 15.7.1 何为复用

- ![image-20240904145924416](./images/stm32/image-20240904145924416.png)

##### 15.7.2 STM32F1的引脚复用

- ![image-20240904150040252](./images/stm32/image-20240904150040252.png)

#### 15.8 编程实战

- 通过串口接收或发送一个字符

- 使用CubeMX生成USART1的代码

  - HAL库中所有的中断函数都定义在**stm32f1xx_it.c**中，这看起来不太方便，这里是将对应外设的中断定义在对应外设的.c文件中，不需要再声明，当然函数名要与stm32f1xx_it.c中的保持一致，并将stm32f1xx_it.c中对应的函数屏蔽掉

- 已知的usart.c中

  - ```c
    /* USER CODE BEGIN Header */
    /**
      ******************************************************************************
      * @file    usart.c
      * @brief   This file provides code for the configuration
      *          of the USART instances.
      ******************************************************************************
      * @attention
      *
      * Copyright (c) 2024 STMicroelectronics.
      * All rights reserved.
      *
      * This software is licensed under terms that can be found in the LICENSE file
      * in the root directory of this software component.
      * If no LICENSE file comes with this software, it is provided AS-IS.
      *
      ******************************************************************************
      */
    /* USER CODE END Header */
    /* Includes ------------------------------------------------------------------*/
    #include "usart.h"
    
    /* USER CODE BEGIN 0 */
    //全局变量需要声明
    uint8_t g_rx_buffer[1] = {0};   //定义全局接收数组
    uint8_t g_usart1_rx_flag = 0;   //串口接收完成标志，0表示未完成，1表示完成
    /* USER CODE END 0 */
    
    UART_HandleTypeDef huart1;
    
    /* USART1 init function */
    
    void MX_USART1_UART_Init(void)
    {
    
      /* USER CODE BEGIN USART1_Init 0 */
    
      /* USER CODE END USART1_Init 0 */
    
      /* USER CODE BEGIN USART1_Init 1 */
    
      /* USER CODE END USART1_Init 1 */
      huart1.Instance = USART1;
      huart1.Init.BaudRate = 115200;
      huart1.Init.WordLength = UART_WORDLENGTH_8B;
      huart1.Init.StopBits = UART_STOPBITS_1;
      huart1.Init.Parity = UART_PARITY_NONE;
      huart1.Init.Mode = UART_MODE_TX_RX;
      huart1.Init.HwFlowCtl = UART_HWCONTROL_NONE;
      huart1.Init.OverSampling = UART_OVERSAMPLING_16;
      if (HAL_UART_Init(&huart1) != HAL_OK)
      {
        Error_Handler();
      }
      /* USER CODE BEGIN USART1_Init 2 */
        HAL_UART_Receive_IT(&huart1,g_rx_buffer,1);   //开启串口异步接收中断
      /* USER CODE END USART1_Init 2 */
    
    }
    //MSP回调函数
    void HAL_UART_MspInit(UART_HandleTypeDef* uartHandle)
    {
    
      GPIO_InitTypeDef GPIO_InitStruct = {0};
      if(uartHandle->Instance==USART1)
      {
      /* USER CODE BEGIN USART1_MspInit 0 */
    
      /* USER CODE END USART1_MspInit 0 */
        /* USART1 clock enable */
        __HAL_RCC_USART1_CLK_ENABLE();
    
        __HAL_RCC_GPIOA_CLK_ENABLE();
        /**USART1 GPIO Configuration
        PA9     ------> USART1_TX
        PA10     ------> USART1_RX
        */
        GPIO_InitStruct.Pin = GPIO_PIN_9;
        GPIO_InitStruct.Mode = GPIO_MODE_AF_PP;
        GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_HIGH;
        HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
    
        GPIO_InitStruct.Pin = GPIO_PIN_10;
        GPIO_InitStruct.Mode = GPIO_MODE_INPUT;
        GPIO_InitStruct.Pull = GPIO_NOPULL;
        HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
    
        /* USART1 interrupt Init */
        HAL_NVIC_SetPriority(USART1_IRQn, 2, 0);
        HAL_NVIC_EnableIRQ(USART1_IRQn);
      /* USER CODE BEGIN USART1_MspInit 1 */
    
      /* USER CODE END USART1_MspInit 1 */
      }
    }
    
    void HAL_UART_MspDeInit(UART_HandleTypeDef* uartHandle)
    {
    
      if(uartHandle->Instance==USART1)
      {
      /* USER CODE BEGIN USART1_MspDeInit 0 */
    
      /* USER CODE END USART1_MspDeInit 0 */
        /* Peripheral clock disable */
        __HAL_RCC_USART1_CLK_DISABLE();
    
        /**USART1 GPIO Configuration
        PA9     ------> USART1_TX
        PA10     ------> USART1_RX
        */
        HAL_GPIO_DeInit(GPIOA, GPIO_PIN_9|GPIO_PIN_10);
    
        /* USART1 interrupt Deinit */
        HAL_NVIC_DisableIRQ(USART1_IRQn);
      /* USER CODE BEGIN USART1_MspDeInit 1 */
    
      /* USER CODE END USART1_MspDeInit 1 */
      }
    }
    
    /* USER CODE BEGIN 1 */
    //编写串口中断函数
    void USART1_IRQHandler(void)
    {
        HAL_UART_IRQHandler(&huart1);   //HAL库中断公共函数
        HAL_UART_Receive_IT(&huart1,g_rx_buffer,1);   //开启串口异步接收中断
    }
    //串口数据接收完成回调函数，即串口接收完成后回来调用此函数，该函数本来是弱定义，可以重新定义，且不用再声明
    void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart)
    {
        if(huart->Instance == USART1)   //判断外设寄存器基地址
        {
            g_usart1_rx_flag = 1;    //接收完成标志
        }
    }
    /* USER CODE END 1 */
    
    
    ```

- usart.h

  - ```c
    /* USER CODE BEGIN Header */
    /**
      ******************************************************************************
      * @file    usart.h
      * @brief   This file contains all the function prototypes for
      *          the usart.c file
      ******************************************************************************
      * @attention
      *
      * Copyright (c) 2024 STMicroelectronics.
      * All rights reserved.
      *
      * This software is licensed under terms that can be found in the LICENSE file
      * in the root directory of this software component.
      * If no LICENSE file comes with this software, it is provided AS-IS.
      *
      ******************************************************************************
      */
    /* USER CODE END Header */
    /* Define to prevent recursive inclusion -------------------------------------*/
    #ifndef __USART_H__
    #define __USART_H__
    
    #ifdef __cplusplus
    extern "C" {
    #endif
    
    /* Includes ------------------------------------------------------------------*/
    #include "main.h"
    
    /* USER CODE BEGIN Includes */
    
    /* USER CODE END Includes */
    
    extern UART_HandleTypeDef huart1;
    
    /* USER CODE BEGIN Private defines */
    extern uint8_t g_rx_buffer[1];   //定义全局接收数组
    extern uint8_t g_usart1_rx_flag;   //串口接收完成标志，0表示未完成，1表示完成
    /* USER CODE END Private defines */
    
    void MX_USART1_UART_Init(void);
    
    /* USER CODE BEGIN Prototypes */
    
    /* USER CODE END Prototypes */
    
    #ifdef __cplusplus
    }
    #endif
    
    #endif /* __USART_H__ */
    
    
    ```

- main.c

  - ```c
    int main(void)
    {
      /* USER CODE BEGIN 1 */
    
      /* USER CODE END 1 */
    
      /* MCU Configuration--------------------------------------------------------*/
    
      /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
      HAL_Init();
    
      /* USER CODE BEGIN Init */
    
      /* USER CODE END Init */
    
      /* Configure the system clock */
      SystemClock_Config();
    
      /* USER CODE BEGIN SysInit */
    
      /* USER CODE END SysInit */
    
      /* Initialize all configured peripherals */
      MX_GPIO_Init();
      MX_USART1_UART_Init();
      /* USER CODE BEGIN 2 */
    
      /* USER CODE END 2 */
    
      /* Infinite loop */
      /* USER CODE BEGIN WHILE */
      while (1)
      {
          if(g_usart1_rx_flag == 1)  //判断是否接收完成
          {
              HAL_UART_Transmit(&huart1,g_rx_buffer,1,100);
              while(__HAL_UART_GET_FLAG(&huart1,UART_FLAG_TC) != 1);    //等待发送完成，这里是发送完成标志
              g_usart1_rx_flag = 0;
          }
          else
          {
              HAL_Delay(50);
          }
        /* USER CODE END WHILE */
    
        /* USER CODE BEGIN 3 */
      }
      /* USER CODE END 3 */
    }
    
    ```

##### 15.8.1 正点原子串口例程

- ![image-20240904175205567](../../Personal%20notes/0/yqnodes/docs/notes/embedded/images/%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D/image-20240904175205567.png)
- ![image-20240904175221741](./images/stm32/image-20240904175221741.png)
- ![image-20240904175400881](./images/stm32/image-20240904175400881.png)

##### 15.8.2 编程实战

- usart.c

  - ```c
    
    #include "usart.h"
    
    /* USER CODE BEGIN 0 */
    uint8_t g_usart_recv_buf[RECV_MAX_DATA] = {0};    //存储接收字节数
    uint8_t g_usart_recv_one_data[1] = {0};
    uint16_t g_usart_recv_sta = 0;       //位15是1，说明接收完成；位14是1，说明接收到0x0a，位13-0是接收字节个数
    /* USER CODE END 0 */
    
    UART_HandleTypeDef huart1;
    
    /* USART1 init function */
    
    void MX_USART1_UART_Init(void)
    {
    
      /* USER CODE BEGIN USART1_Init 0 */
    
      /* USER CODE END USART1_Init 0 */
    
      /* USER CODE BEGIN USART1_Init 1 */
    
      /* USER CODE END USART1_Init 1 */
      huart1.Instance = USART1;
      huart1.Init.BaudRate = 115200;
      huart1.Init.WordLength = UART_WORDLENGTH_8B;
      huart1.Init.StopBits = UART_STOPBITS_1;
      huart1.Init.Parity = UART_PARITY_NONE;
      huart1.Init.Mode = UART_MODE_TX_RX;
      huart1.Init.HwFlowCtl = UART_HWCONTROL_NONE;
      huart1.Init.OverSampling = UART_OVERSAMPLING_16;
      if (HAL_UART_Init(&huart1) != HAL_OK)
      {
        Error_Handler();
      }
      /* USER CODE BEGIN USART1_Init 2 */
        HAL_UART_Receive_IT(&huart1, g_usart_recv_one_data, 1);
      /* USER CODE END USART1_Init 2 */
    
    }
    
    void HAL_UART_MspInit(UART_HandleTypeDef* uartHandle)
    {
    
      GPIO_InitTypeDef GPIO_InitStruct = {0};
      if(uartHandle->Instance==USART1)
      {
      /* USER CODE BEGIN USART1_MspInit 0 */
    
      /* USER CODE END USART1_MspInit 0 */
        /* USART1 clock enable */
        __HAL_RCC_USART1_CLK_ENABLE();
    
        __HAL_RCC_GPIOA_CLK_ENABLE();
        /**USART1 GPIO Configuration
        PA9     ------> USART1_TX
        PA10     ------> USART1_RX
        */
        GPIO_InitStruct.Pin = GPIO_PIN_9;
        GPIO_InitStruct.Mode = GPIO_MODE_AF_PP;
        GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_HIGH;
        HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
    
        GPIO_InitStruct.Pin = GPIO_PIN_10;
        GPIO_InitStruct.Mode = GPIO_MODE_INPUT;
        GPIO_InitStruct.Pull = GPIO_NOPULL;
        HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
    
        /* USART1 interrupt Init */
        HAL_NVIC_SetPriority(USART1_IRQn, 2, 3);
        HAL_NVIC_EnableIRQ(USART1_IRQn);
      /* USER CODE BEGIN USART1_MspInit 1 */
    
      /* USER CODE END USART1_MspInit 1 */
      }
    }
    
    void HAL_UART_MspDeInit(UART_HandleTypeDef* uartHandle)
    {
    
      if(uartHandle->Instance==USART1)
      {
      /* USER CODE BEGIN USART1_MspDeInit 0 */
    
      /* USER CODE END USART1_MspDeInit 0 */
        /* Peripheral clock disable */
        __HAL_RCC_USART1_CLK_DISABLE();
    
        /**USART1 GPIO Configuration
        PA9     ------> USART1_TX
        PA10     ------> USART1_RX
        */
        HAL_GPIO_DeInit(GPIOA, GPIO_PIN_9|GPIO_PIN_10);
    
        /* USART1 interrupt Deinit */
        HAL_NVIC_DisableIRQ(USART1_IRQn);
      /* USER CODE BEGIN USART1_MspDeInit 1 */
    
      /* USER CODE END USART1_MspDeInit 1 */
      }
    }
    
    /* USER CODE BEGIN 1 */
    int fputc(int ch, FILE *f)
    {
    HAL_UART_Transmit(&huart1, (uint8_t *)&ch, 1, 0xffff);
    return ch;
    }
    /**
    * 函数功能: 重定向c库函数getchar,scanf到DEBUG_USARTx
    * 输入参数: 无
    * 返 回 值: 无
    * 说 明：无
    */
    int fgetc(FILE *f)
    {
    uint8_t ch = 0;
    HAL_UART_Receive(&huart1, &ch, 1, 0xffff);
    return ch;
    }
    /* 中断服务函数 */
    
    void USART1_IRQHandler(void)
    {
      /* USER CODE BEGIN USART1_IRQn 0 */
      //  printf("-----------------进入中断---------------------------\r\n");
      /* USER CODE END USART1_IRQn 0 */
      HAL_UART_IRQHandler(&huart1);
      /* USER CODE BEGIN USART1_IRQn 1 */
        HAL_UART_Receive_IT(&huart1, g_usart_recv_one_data,1);
       // MX_USART_RX(RX,strlen(RX));
      /* USER CODE END USART1_IRQn 1 */
    }
    /* 串口接收完成回调函数 */
    void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart)
    {
     //   printf("-----------------进入接收完成回调---------------------------\r\n");
        if(huart->Instance == USART1)
        {
            if((g_usart_recv_sta & 0x8000) == 0)   //没有接收完成
            {
                if(g_usart_recv_sta & 0x4000)    //接收到0x0d
                {
                    if(g_usart_recv_one_data[0] == 0x0a)  //接收完成
                    {
                        g_usart_recv_sta |= 0x8000;
                        g_usart_recv_buf[g_usart_recv_sta & 0x3FFF] = '\0';
                    }
                    else
                    {
                        g_usart_recv_sta = 0;    //接收失败，重新接收
                        printf("接收数据失败，重新接收\r\n");
                    }
                }
                else
                {
                    if(g_usart_recv_one_data[0] == 0x0d)
                    {
                        g_usart_recv_sta |= 0x4000;
                    }
                    else     //按字节存储接收数据 
                    {
                        g_usart_recv_buf[g_usart_recv_sta & 0x3FFF] = g_usart_recv_one_data[0];
                        g_usart_recv_sta++;
                        if(g_usart_recv_sta > (RECV_MAX_DATA - 1))   //接收数据过长
                        {
                            g_usart_recv_sta = 0;    //接收失败，重新接收
                            printf("接收数据过长，重新接收\r\n");
                        }
                    }
                }
            }
        HAL_UART_Receive_IT(&huart1, g_usart_recv_one_data, 1);
        }
    }
    /* 发送字符串 */
    
    /* USER CODE END 1 */
    
    
    ```

  - main.c

    - ```c
      /* USER CODE BEGIN Header */
      /**
        ******************************************************************************
        * @file           : main.c
        * @brief          : Main program body
        ******************************************************************************
        * @attention
        *
        * Copyright (c) 2024 STMicroelectronics.
        * All rights reserved.
        *
        * This software is licensed under terms that can be found in the LICENSE file
        * in the root directory of this software component.
        * If no LICENSE file comes with this software, it is provided AS-IS.
        *
        ******************************************************************************
        */
      /* USER CODE END Header */
      /* Includes ------------------------------------------------------------------*/
      #include "main.h"
      #include "usart.h"
      #include "gpio.h"
      
      /* Private includes ----------------------------------------------------------*/
      /* USER CODE BEGIN Includes */
      
      /* USER CODE END Includes */
      
      /* Private typedef -----------------------------------------------------------*/
      /* USER CODE BEGIN PTD */
      
      /* USER CODE END PTD */
      
      /* Private define ------------------------------------------------------------*/
      /* USER CODE BEGIN PD */
      
      /* USER CODE END PD */
      
      /* Private macro -------------------------------------------------------------*/
      /* USER CODE BEGIN PM */
      
      /* USER CODE END PM */
      
      /* Private variables ---------------------------------------------------------*/
      
      /* USER CODE BEGIN PV */
      
      /* USER CODE END PV */
      
      /* Private function prototypes -----------------------------------------------*/
      void SystemClock_Config(void);
      /* USER CODE BEGIN PFP */
      
      /* USER CODE END PFP */
      
      /* Private user code ---------------------------------------------------------*/
      /* USER CODE BEGIN 0 */
      
      /* USER CODE END 0 */
      
      /**
        * @brief  The application entry point.
        * @retval int
        */
      int main(void)
      {
        /* USER CODE BEGIN 1 */
          uint8_t t = 0;
        /* USER CODE END 1 */
      
        /* MCU Configuration--------------------------------------------------------*/
      
        /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
        HAL_Init();
      
        /* USER CODE BEGIN Init */
      
        /* USER CODE END Init */
      
        /* Configure the system clock */
        SystemClock_Config();
      
        /* USER CODE BEGIN SysInit */
      
        /* USER CODE END SysInit */
      
        /* Initialize all configured peripherals */
        MX_GPIO_Init();
        MX_USART1_UART_Init();
        /* USER CODE BEGIN 2 */
        if(__HAL_RCC_GET_FLAG(RCC_FLAG_PINRST))
        {
            printf("复位按键复位\r\n");
            __HAL_RCC_CLEAR_RESET_FLAGS();
        }
          printf("请输入一个英文字符或字符串：\r\n");
        //  MX_USART_TX(TX);
        /* USER CODE END 2 */
      
        /* Infinite loop */
        /* USER CODE BEGIN WHILE */
        while (1)
        { 
            if(g_usart_recv_sta & 0x80)    //接收成功
            {
                printf("接收到的字符串为:");
                HAL_UART_Transmit(&huart1,g_usart_recv_buf,sizeof(g_usart_recv_buf),100);
                while(__HAL_UART_GET_FLAG(&huart1, UART_FLAG_TC) != 1);
                printf("\r\n");
                g_usart_recv_sta = 0;  //状态标志清0
                memset(g_usart_recv_buf,0,RECV_MAX_DATA);  //存储空间清0
            }
          t++;
          if(t>10)
          {
              HAL_GPIO_TogglePin(LED0_GPIO_Port,LED0_Pin); 
              t = 0;
          }
         HAL_Delay(10);
          /* USER CODE END WHILE */
      
          /* USER CODE BEGIN 3 */
        }
        /* USER CODE END 3 */
      }
      
      /**
        * @brief System Clock Configuration
        * @retval None
        */
      void SystemClock_Config(void)
      {
        RCC_OscInitTypeDef RCC_OscInitStruct = {0};
        RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};
      
        /** Initializes the RCC Oscillators according to the specified parameters
        * in the RCC_OscInitTypeDef structure.
        */
        RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
        RCC_OscInitStruct.HSEState = RCC_HSE_ON;
        RCC_OscInitStruct.HSEPredivValue = RCC_HSE_PREDIV_DIV1;
        RCC_OscInitStruct.HSIState = RCC_HSI_ON;
        RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;
        RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSE;
        RCC_OscInitStruct.PLL.PLLMUL = RCC_PLL_MUL9;
        if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
        {
          Error_Handler();
        }
      
        /** Initializes the CPU, AHB and APB buses clocks
        */
        RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                                    |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
        RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;
        RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
        RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV2;
        RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;
      
        if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_2) != HAL_OK)
        {
          Error_Handler();
        }
      }
      
      /* USER CODE BEGIN 4 */
      
      /* USER CODE END 4 */
      
      /**
        * @brief  This function is executed in case of error occurrence.
        * @retval None
        */
      void Error_Handler(void)
      {
        /* USER CODE BEGIN Error_Handler_Debug */
        /* User can add his own implementation to report the HAL error return state */
        __disable_irq();
        while (1)
        {
        }
        /* USER CODE END Error_Handler_Debug */
      }
      
      #ifdef  USE_FULL_ASSERT
      /**
        * @brief  Reports the name of the source file and the source line number
        *         where the assert_param error has occurred.
        * @param  file: pointer to the source file name
        * @param  line: assert_param error line source number
        * @retval None
        */
      void assert_failed(uint8_t *file, uint32_t line)
      {
        /* USER CODE BEGIN 6 */
        /* User can add his own implementation to report the file name and line number,
           ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
        /* USER CODE END 6 */
      }
      #endif /* USE_FULL_ASSERT */
      
      ```

      

### 16. IWDG

#### 16.1 IWDG简介

- IWDG：独立看门狗，实际上是一个能产生复位信号的递减计数器，减到0时发生复位
- 时钟来源：有独立的RC振荡器提供，可在待机或停运模式下运行，开启看门狗后，递减计数器开始工作，减到0x00时产生复位
- 喂狗：重装载计数器，在IWDG递减到0之前给计数器重载，及时喂狗，防止复位
- 实现场景：外界电磁干扰或由于硬件问题程序跑飞的时候，系统没有及时喂狗会产生复位，从而表面上解决问题，但实际上问题依然存在，需要设计师设计严谨
- 应用：在一些需要高稳定性，并且对时间精度要求较低的场合（因为RC振荡器自身就不太稳定，提供的时钟也不太稳定）

#### 16.2 IWDG工作原理

- ![image-20240905095408358](./images/stm32/image-20240905095408358.png)

#### 16.3 IWDG框图

- ![image-20240905095543300](./images/stm32/image-20240905095543300.png)

#### 16.4 IWDG寄存器

- ![image-20240905101543851](./images/stm32/image-20240905101543851.png)
- ![image-20240905101553515](../../Personal%20notes/0/yqnodes/docs/notes/embedded/images/%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D/image-20240905101553515.png)
- ![image-20240905101607655](./images/stm32/image-20240905101607655.png)
- ![image-20240905101617713](./images/stm32/image-20240905101617713.png)
- 寄存器配置步骤，主要是使用寄存器开发要熟悉，使用HAL库就了解
  - ![image-20240905102354909](./images/stm32/image-20240905102354909.png)

#### 16.5 IWDG溢出时间

- ![image-20240905102557344](./images/stm32/image-20240905102557344.png)
- ![image-20240905102831098](./images/stm32/image-20240905102831098.png)
  - IWDG最短最长时间可以给溢出时间做参考，例如要溢出1s，那么这里就不能选4/8分频，因为它俩最长时间也没有1s

#### 16.6 IWDG配置步骤

- 主要是两步
  - 使能IWDG_PR 、IWDG_RLR寄存器，设置IWDG_PR 、IWDG_RLR寄存器的值，启动IWDG
  - 及时喂狗
- ![image-20240905104154536](./images/stm32/image-20240905104154536.png)
- ![image-20240905104308253](./images/stm32/image-20240905104308253.png)

#### 16.7 编程实战

- 验证不及时喂狗，系统会自动重启
- ![image-20240905115108371](./images/stm32/image-20240905115108371.png)

- iwdg.c

  - ```c
    /* USER CODE BEGIN Header */
    /**
      ******************************************************************************
      * @file    iwdg.c
      * @brief   This file provides code for the configuration
      *          of the IWDG instances.
      ******************************************************************************
      * @attention
      *
      * Copyright (c) 2024 STMicroelectronics.
      * All rights reserved.
      *
      * This software is licensed under terms that can be found in the LICENSE file
      * in the root directory of this software component.
      * If no LICENSE file comes with this software, it is provided AS-IS.
      *
      ******************************************************************************
      */
    /* USER CODE END Header */
    /* Includes ------------------------------------------------------------------*/
    #include "iwdg.h"
    
    /* USER CODE BEGIN 0 */
    
    /* USER CODE END 0 */
    
    IWDG_HandleTypeDef hiwdg;
    
    /* IWDG init function */
    void MX_IWDG_Init(uint8_t psc,uint16_t arr)  //有形参更容易修改
    {
    
      /* USER CODE BEGIN IWDG_Init 0 */
    
      /* USER CODE END IWDG_Init 0 */
    
      /* USER CODE BEGIN IWDG_Init 1 */
    
      /* USER CODE END IWDG_Init 1 */
      hiwdg.Instance = IWDG;
      hiwdg.Init.Prescaler = psc;   //这里不能写数学，有对应的宏定义
      hiwdg.Init.Reload = arr;   //这里写数字
      if (HAL_IWDG_Init(&hiwdg) != HAL_OK)
      {
        Error_Handler();
      }
      /* USER CODE BEGIN IWDG_Init 2 */
    
      /* USER CODE END IWDG_Init 2 */
    
    }
    
    /* USER CODE BEGIN 1 */
    void IWDG_Feed(void)   //喂狗函数
    {
        HAL_IWDG_Refresh(&hiwdg);      //喂狗
    }
    /* USER CODE END 1 */
    
    ```

  - iwdg.h

    - ```c
      /* USER CODE BEGIN Header */
      /**
        ******************************************************************************
        * @file    iwdg.h
        * @brief   This file contains all the function prototypes for
        *          the iwdg.c file
        ******************************************************************************
        * @attention
        *
        * Copyright (c) 2024 STMicroelectronics.
        * All rights reserved.
        *
        * This software is licensed under terms that can be found in the LICENSE file
        * in the root directory of this software component.
        * If no LICENSE file comes with this software, it is provided AS-IS.
        *
        ******************************************************************************
        */
      /* USER CODE END Header */
      /* Define to prevent recursive inclusion -------------------------------------*/
      #ifndef __IWDG_H__
      #define __IWDG_H__
      
      #ifdef __cplusplus
      extern "C" {
      #endif
      
      /* Includes ------------------------------------------------------------------*/
      #include "main.h"
      
      /* USER CODE BEGIN Includes */
      
      /* USER CODE END Includes */
      
      extern IWDG_HandleTypeDef hiwdg;    //全局变量，iwdg操作句柄
      
      /* USER CODE BEGIN Private defines */
      
      /* USER CODE END Private defines */
      
      
      
      /* USER CODE BEGIN Prototypes */
      void MX_IWDG_Init(uint8_t psc,uint16_t arr);
      void IWDG_Feed(void);
          
      /* USER CODE END Prototypes */
      
      #ifdef __cplusplus
      }
      #endif
      
      #endif /* __IWDG_H__ */
      
      
      ```

  - main.c

    - ```c
      /* USER CODE BEGIN Header */
      /**
        ******************************************************************************
        * @file           : main.c
        * @brief          : Main program body
        ******************************************************************************
        * @attention
        *
        * Copyright (c) 2024 STMicroelectronics.
        * All rights reserved.
        *
        * This software is licensed under terms that can be found in the LICENSE file
        * in the root directory of this software component.
        * If no LICENSE file comes with this software, it is provided AS-IS.
        *
        ******************************************************************************
        */
      /* USER CODE END Header */
      /* Includes ------------------------------------------------------------------*/
      #include "main.h"
      #include "iwdg.h"
      #include "usart.h"
      #include "gpio.h"
      
      /* Private includes ----------------------------------------------------------*/
      /* USER CODE BEGIN Includes */
      
      /* USER CODE END Includes */
      
      /* Private typedef -----------------------------------------------------------*/
      /* USER CODE BEGIN PTD */
      
      /* USER CODE END PTD */
      
      /* Private define ------------------------------------------------------------*/
      /* USER CODE BEGIN PD */
      
      /* USER CODE END PD */
      
      /* Private macro -------------------------------------------------------------*/
      /* USER CODE BEGIN PM */
      
      /* USER CODE END PM */
      
      /* Private variables ---------------------------------------------------------*/
      
      /* USER CODE BEGIN PV */
      
      /* USER CODE END PV */
      
      /* Private function prototypes -----------------------------------------------*/
      void SystemClock_Config(void);
      /* USER CODE BEGIN PFP */
      
      /* USER CODE END PFP */
      
      /* Private user code ---------------------------------------------------------*/
      /* USER CODE BEGIN 0 */
      
      /* USER CODE END 0 */
      
      /**
        * @brief  The application entry point.
        * @retval int
        */
      int main(void)
      {
        /* USER CODE BEGIN 1 */
      
        /* USER CODE END 1 */
      
        /* MCU Configuration--------------------------------------------------------*/
      
        /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
        HAL_Init();
      
        /* USER CODE BEGIN Init */
      
        /* USER CODE END Init */
      
        /* Configure the system clock */
        SystemClock_Config();
      
        /* USER CODE BEGIN SysInit */
      
        /* USER CODE END SysInit */
      
        /* Initialize all configured peripherals */
        MX_GPIO_Init(); 
        MX_USART1_UART_Init();
        /* USER CODE BEGIN 2 */
          printf("您还没喂狗，请及时喂狗\r\n");
          MX_IWDG_Init(IWDG_PRESCALER_32,1250);    //溢出时间是1s
        /* USER CODE END 2 */
      
        /* Infinite loop */
        /* USER CODE BEGIN WHILE */
        while (1)
        {
            HAL_Delay(1050);
            IWDG_Feed();    //喂狗
            printf("狗子已喂\r\n");
          /* USER CODE END WHILE */
      
          /* USER CODE BEGIN 3 */
        }
        /* USER CODE END 3 */
      }
      
      /**
        * @brief System Clock Configuration
        * @retval None
        */
      void SystemClock_Config(void)
      {
        RCC_OscInitTypeDef RCC_OscInitStruct = {0};
        RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};
      
        /** Initializes the RCC Oscillators according to the specified parameters
        * in the RCC_OscInitTypeDef structure.
        */
        RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_LSI|RCC_OSCILLATORTYPE_HSE;
        RCC_OscInitStruct.HSEState = RCC_HSE_ON;
        RCC_OscInitStruct.HSEPredivValue = RCC_HSE_PREDIV_DIV1;
        RCC_OscInitStruct.HSIState = RCC_HSI_ON;
        RCC_OscInitStruct.LSIState = RCC_LSI_ON;
        RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;
        RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSE;
        RCC_OscInitStruct.PLL.PLLMUL = RCC_PLL_MUL9;
        if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
        {
          Error_Handler();
        }
      
        /** Initializes the CPU, AHB and APB buses clocks
        */
        RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                                    |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
        RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;
        RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
        RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV2;
        RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;
      
        if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_2) != HAL_OK)
        {
          Error_Handler();
        }
      }
      
      /* USER CODE BEGIN 4 */
      
      /* USER CODE END 4 */
      
      /**
        * @brief  This function is executed in case of error occurrence.
        * @retval None
        */
      void Error_Handler(void)
      {
        /* USER CODE BEGIN Error_Handler_Debug */
        /* User can add his own implementation to report the HAL error return state */
        __disable_irq();
        while (1)
        {
        }
        /* USER CODE END Error_Handler_Debug */
      }
      
      #ifdef  USE_FULL_ASSERT
      /**
        * @brief  Reports the name of the source file and the source line number
        *         where the assert_param error has occurred.
        * @param  file: pointer to the source file name
        * @param  line: assert_param error line source number
        * @retval None
        */
      void assert_failed(uint8_t *file, uint32_t line)
      {
        /* USER CODE BEGIN 6 */
        /* User can add his own implementation to report the file name and line number,
           ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
        /* USER CODE END 6 */
      }
      #endif /* USE_FULL_ASSERT */
      
      ```

### 17. WWDG

#### 17.1 WWDG 简介

- ![image-20240905144049867](./images/stm32/image-20240905144049867.png)
- WWDG有什么用？
  - ![image-20240905144137637](./images/stm32/image-20240905144137637.png)
  - WWDG时钟来源于总线，时钟精准，所以应用于精准检测程序运行时间的场合

#### 17.2 WWDG 工作原理

- ![image-20240905144408724](./images/stm32/image-20240905144408724.png)

  

#### 17.3 WWDG框图

- 计数器从0x40减到0x3f产生复位
  - ![image-20240905144742516](./images/stm32/image-20240905144742516.png)
- 计数器T[6,0] > W[6,0]时喂狗，产生复位
  - ![image-20240905144953100](./images/stm32/image-20240905144953100.png)
  - 最终预分频系数是4096*2^WDGTB
    - 4096是固定的
    - 2^WDGTB是通过寄存器设置的

#### 17.4 WWDG寄存器

- ![image-20240905145756320](./images/stm32/image-20240905145756320.png)
- ![image-20240905145926964](./images/stm32/image-20240905145926964.png)
- ![image-20240905150005948](./images/stm32/image-20240905150005948.png)
- 

#### 17.5 WWDG超时时间计算

- ![image-20240905152828491](./images/stm32/image-20240905152828491.png)
- WWDG通过一个递减的计数器来监测系统的运行状态。当计数器递减到某个特定值时（通常是窗口下限值），如果在此之前没有通过“喂狗”操作来重载计数器的值，那么WWDG将触发系统复位。超时时间就是计数器从初始值递减到触发复位所需的时间
- ![image-20240905153042446](./images/stm32/image-20240905153042446.png)
  - 带入超时时间公式即可算出

#### 17.6 WWDG配置步骤

- ![image-20240905154859118](./images/stm32/image-20240905154859118.png)
- ![image-20240905154942561](./images/stm32/image-20240905154942561.png)

#### 17.7 编程实战-验证窗口狗功能

- WWDG初始化函数里，开启提前中断使能，才能在计数器减到0x40时进入中断函数

  - ```c
    void MX_WWDG_Init(uint8_t win,uint8_t cont,uint32_t psc)
    {
    
      /* USER CODE BEGIN WWDG_Init 0 */
    
      /* USER CODE END WWDG_Init 0 */
    
      /* USER CODE BEGIN WWDG_Init 1 */
    
      /* USER CODE END WWDG_Init 1 */
      hwwdg.Instance = WWDG;
      hwwdg.Init.Prescaler = psc;
      hwwdg.Init.Window = win;
      hwwdg.Init.Counter = cont;
      hwwdg.Init.EWIMode = WWDG_EWI_ENABLE;   //使能提前唤醒中断
      if (HAL_WWDG_Init(&hwwdg) != HAL_OK)
      {
        Error_Handler();
      }
      /* USER CODE BEGIN WWDG_Init 2 */
    
      /* USER CODE END WWDG_Init 2 */
    
    }
    ```

    

- 进入中断函数就是达到了中断标志

- 由于HAL库系统滴答定时器是以ms为单位，而设置8分频时，WWDG从0x40减到0x3F只要0.91ms，小于最小时间单位，导致没办法在0.91ms内喂狗，导致复位

  - 问题：目前时间精度不够
  - 解决方案
    - 1、提高系统滴答定时器精度
    - 2、将WWDG优先级高于系统滴答定时器
  - 最终状态
    - 兜兜转转，又回到了原点，WWDG的优先级低于系统滴答定时器，然后使用中断回调函数，居然实现了喂狗！

#### 17.8 IWDG与WWDG的区别

- ![image-20240909141432937](./images/stm32/image-20240909141432937.png)

### 18. 定时器

#### 18.1 定时器概述

- ![image-20240909143645073](./images/stm32/image-20240909143645073.png)

- ![image-20240909143700907](./images/stm32/image-20240909143700907.png)

- ![image-20240909143711309](./images/stm32/image-20240909143711309.png)

- ![image-20240909143720903](./images/stm32/image-20240909143720903.png)

- ![image-20240909143730936](./images/stm32/image-20240909143730936.png)

  

#### 18.2 基本定时器

##### 18.2.1 基本定时器简介

- ![image-20240909145747764](./images/stm32/image-20240909145747764.png)

##### 18.2.2 基本定时器框图

- ![image-20240909145808253](./images/stm32/image-20240909145808253.png)
  - 影子寄存器是真正起作用的寄存器，CPU不可以直接访问，PSC、ARR寄存器实际上起缓冲/缓存的作用，当发生更新事件时，再将内容移植到对应的影子寄存器
  - 自动重装载寄存器：通过设置ARPE位决定是否有影子寄存器
    - ARPE置1，有缓冲作用，有影子寄存器
    - ARPE置0，无缓冲作用，无影子寄存器
  - 没有ARR影子寄存器的话，会导致错过溢出事件，不急及时进行重装载
    - 例如，本来ARR = 5000，现在计数器已经加到4500，此刻司改ARR = 4000，那现在已经错过溢出事件节点，计数器不会重装载，直到加到65535之后就会溢出，然后才会重载，这样就会错过一次溢出事件
  - 时钟源：来自总线时钟APB2 72M，但不是一直是72M，跟时钟分频系数有关，查参考手册得
    - ![image-20240909151533559](./images/stm32/image-20240909151533559.png)
  - 产生溢出事件时，会产生触发输出信号TRGO，产生一次DAC

##### 18.2.3 基本定时器计数模式及溢出条件

- ![image-20240909161546800](./images/stm32/image-20240909161546800.png)
  - 注意三种模式的溢出条件，都是等于设定值时，产生溢出事件
- ![image-20240909161604616](./images/stm32/image-20240909161604616.png)
  - 二分频，就是进来两个时钟周期，定时器才计数一次
    - 分频系数越大，计数一次用的时间越长
- ![image-20240909161736589](./images/stm32/image-20240909161736589.png)
  - 减到0产生溢出时间，不是减到0产生，而是0再减才会产生更新事件
- ![image-20240909162025690](./images/stm32/image-20240909162025690.png)

##### 18.2.4 基本定时器中断实验相关寄存器

- ![image-20240909163110534](./images/stm32/image-20240909163110534.png)

- ![image-20240909163151330](./images/stm32/image-20240909163151330.png)

- ![image-20240909163208142](./images/stm32/image-20240909163208142.png)

- ![image-20240909163220982](./images/stm32/image-20240909163220982.png)

- ![image-20240909163249160](./images/stm32/image-20240909163249160.png)

- ![image-20240909163359474](./images/stm32/image-20240909163359474.png)

  

##### 18.2.5 定时器溢出时间计算方法

- ![image-20240909163827119](./images/stm32/image-20240909163827119.png)
  - 推导原理：分频后的时钟就是1s内计数的多少次，那么计数1次需要**1/(分频后的时钟)**秒，那么溢出时间就是计数一次的时间*(ARR+1)
    - ARR+1：ARR设置为0时，不能直接用0，也需要一个时钟周期来，才会溢出，所以这里要+1

##### 18.2.6 定时器中断实验配置步骤

- ![image-20240909165453841](./images/stm32/image-20240909165453841.png)
- ![image-20240909165506661](./images/stm32/image-20240909165506661.png)
- ![image-20240909165530122](./images/stm32/image-20240909165530122.png)

##### 18.2.7 编程实战-定时器中断实验

- 定时器计数500ms，LED0状态翻转一次

- ```c
  //更新中断回调函数
  uint8_t second = 0;    //在头文件里全局声明
  void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
  {
     
      if( htim->Instance == TIM6 )
      {
          second++;   //500ms加一次
          if(second%2 == 0)   
          {
              HAL_GPIO_TogglePin(LED1_GPIO_Port,LED1_Pin); //LED1翻转
          }
          else   //每500ms LED1翻转一次
          {
              HAL_GPIO_TogglePin(LED0_GPIO_Port,LED0_Pin); //LED0翻转
          }      
      }
  }
  ```

  

#### 18.3 通用定时器

##### 18.3.1 通用定时器简介

- ![image-20240910150120032](./images/stm32/image-20240910150120032.png)

##### 18.3.2 通用定时器框图

- ![image-20240910151717212](./images/stm32/image-20240910151717212.png)
  - 输入捕获和输出比较是分时复用的，不能一个通道即是输入捕获，又是输出比较

##### 18.3.3 计时器时钟源

- ![image-20240910152609026](./images/stm32/image-20240910152609026.png)

- 计数器时钟源寄存器设置方法

  - TIMx_SMCR：从模式控制寄存器
  - ![image-20240910152725828](./images/stm32/image-20240910152725828.png)

- 外部时钟模式1

  - ![image-20240910153908594](./images/stm32/image-20240910153908594.png)
  - TIMx_CCMR1  捕获/比较寄存器1
    - ![image-20240910154311351](./images/stm32/image-20240910154311351.png)
    - 滤波原理，进来一个波形，根据N的值，进行滤波
      - 例如N = 8，当进入第一个事件是1时，而第七和第八事件都是0时，这是实际进入的波形就是高电平1，并且持续8个事件的时间，之后的波形也是同理
      - 从而实现滤掉电平毛刺的效果，简称滤波
  - TIMx_CCER  捕获比较使能寄存器
    - ![image-20240910155054610](./images/stm32/image-20240910155054610.png)
    - ![image-20240910155122067](./images/stm32/image-20240910155122067.png)
      - 当输入捕获的是上升沿时，当用作外部触发时不反相
      - 当输入捕获的是下降沿时，当用作外部触发时要反相，因为触发信号需要上升沿
  - TIMx_SMCR  从模式控制寄存器
    - ![image-20240910155426969](./images/stm32/image-20240910155426969.png)

- 外部时钟模式2——IO引脚重映射输入

  - ![image-20240910161058544](./images/stm32/image-20240910161058544.png)

- 内部触发输入 -- 级联

  - ![image-20240910161147057](./images/stm32/image-20240910161147057.png)

- 使用通用定时器TIM2 实现500ms中断一次

  - ![image-20240910161314878](./images/stm32/image-20240910161314878.png)

  - 跟基本定时器的操作基本一致，只是计数模式需要选择

  - tim2.c

    - ```c
      
      /* Includes ------------------------------------------------------------------*/
      #include "tim.h"
      
      /* USER CODE BEGIN 0 */
      
      /* USER CODE END 0 */
      
      TIM_HandleTypeDef htim2;
      
      /* TIM2 init function */
      void MX_TIM2_Init(void)
      {
      
        /* USER CODE BEGIN TIM2_Init 0 */
      
        /* USER CODE END TIM2_Init 0 */
      
        TIM_ClockConfigTypeDef sClockSourceConfig = {0};
        TIM_MasterConfigTypeDef sMasterConfig = {0};
      
        /* USER CODE BEGIN TIM2_Init 1 */
      
        /* USER CODE END TIM2_Init 1 */
        htim2.Instance = TIM2;
        htim2.Init.Prescaler = 7200-1;
        htim2.Init.CounterMode = TIM_COUNTERMODE_UP;
        htim2.Init.Period = 5000-1;
        htim2.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
        htim2.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_ENABLE;
        if (HAL_TIM_Base_Init(&htim2) != HAL_OK)
        {
          Error_Handler();
        }
        sClockSourceConfig.ClockSource = TIM_CLOCKSOURCE_INTERNAL;
        if (HAL_TIM_ConfigClockSource(&htim2, &sClockSourceConfig) != HAL_OK)
        {
          Error_Handler();
        }
        sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
        sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
        if (HAL_TIMEx_MasterConfigSynchronization(&htim2, &sMasterConfig) != HAL_OK)
        {
          Error_Handler();
        }
        /* USER CODE BEGIN TIM2_Init 2 */
          HAL_TIM_Base_Start_IT(&htim2);     //启动定时器
        /* USER CODE END TIM2_Init 2 */
      
      }
      
      void HAL_TIM_Base_MspInit(TIM_HandleTypeDef* tim_baseHandle)
      {
      
        if(tim_baseHandle->Instance==TIM2)
        {
        /* USER CODE BEGIN TIM2_MspInit 0 */
      
        /* USER CODE END TIM2_MspInit 0 */
          /* TIM2 clock enable */
          __HAL_RCC_TIM2_CLK_ENABLE();
      
          /* TIM2 interrupt Init */
          HAL_NVIC_SetPriority(TIM2_IRQn, 2, 3);
          HAL_NVIC_EnableIRQ(TIM2_IRQn);
        /* USER CODE BEGIN TIM2_MspInit 1 */
      
        /* USER CODE END TIM2_MspInit 1 */
        }
      }
      
      void HAL_TIM_Base_MspDeInit(TIM_HandleTypeDef* tim_baseHandle)
      {
      
        if(tim_baseHandle->Instance==TIM2)
        {
        /* USER CODE BEGIN TIM2_MspDeInit 0 */
      
        /* USER CODE END TIM2_MspDeInit 0 */
          /* Peripheral clock disable */
          __HAL_RCC_TIM2_CLK_DISABLE();
      
          /* TIM2 interrupt Deinit */
          HAL_NVIC_DisableIRQ(TIM2_IRQn);
        /* USER CODE BEGIN TIM2_MspDeInit 1 */
      
        /* USER CODE END TIM2_MspDeInit 1 */
        }
      }
      
      /* USER CODE BEGIN 1 */
      void TIM2_IRQHandler(void)
      {
        /* USER CODE BEGIN TIM2_IRQn 0 */
      
        /* USER CODE END TIM2_IRQn 0 */
        HAL_TIM_IRQHandler(&htim2);
        /* USER CODE BEGIN TIM2_IRQn 1 */
      
        /* USER CODE END TIM2_IRQn 1 */
      }
      void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
      {
          if(htim->Instance == TIM2)
          {
              HAL_GPIO_TogglePin(LED0_GPIO_Port,LED0_Pin);   //每500ms翻转一次
          }
      }
      /* USER CODE END 1 */
      
      ```

      

##### 18.3.4 通用定时器PWM输出实验

###### 18.3.4.1 通用定时器输出比较部分框图介绍

- ![image-20240911100051422](./images/stm32/image-20240911100051422.png)
- 捕获/比较通道1的主电路
  - ![image-20240911100209384](./images/stm32/image-20240911100209384.png)
    - 当禁止预装载使能时，CCR1写完，并且CC1S通道一设置为输出时，设置的预装载值就可移植到影子寄存器中
    - 当使能预装载时，CCR1写完，并且CC1S通道一设置为输出时，还要有UEV更新事件，设置的预装载值才可移植到影子寄存器中
- 捕获/比较通道1的输出部分
  - ![image-20240911102637880](./images/stm32/image-20240911102637880.png)

###### 18.3.4.2 通用定时器输出PWM原理

- ![image-20240911102655735](./images/stm32/image-20240911102655735.png)
- 在时钟不变的情况下
  - PWM的周期由ARR，ARR增加，PWM周期就增大
  - PWM波的占空比由CCRx决定

###### 18.3.4.3 PWM模式

- 在捕获/比较模式寄存器1(TIMx_CCMR1)中，OC1M[2,0]配置输出比较1模式
  - ![image-20240911104519669](./images/stm32/image-20240911104519669.png)
  - 输入/捕获1输出极性由CC1P位控制，设为0则高电平有效，设为1则低电平有效，对应PWM模式下的有效电平
    - ![image-20240911104727456](./images/stm32/image-20240911104727456.png)
  - ![image-20240911105124753](./images/stm32/image-20240911105124753.png)

###### 18.3.4.4 通用定时器PWM输出配置步骤

- ![image-20240911105159894](./images/stm32/image-20240911105159894.png)
- ![image-20240911105214102](./images/stm32/image-20240911105214102.png)
- ![image-20240911105239424](./images/stm32/image-20240911105239424.png)

###### 18.3.4.5 编程实战--通用定时器输出PWM

- ![image-20240911150824034](./images/stm32/image-20240911150824034.png)

##### 18.3.5 通用定时器输入捕获实验

###### 18.3.5.1 通用定时器输入捕获部分框图介绍

- ![image-20240911150838613](./images/stm32/image-20240911150838613.png)

- ![image-20240911150905750](./images/stm32/image-20240911150905750.png)

- ![image-20240911150944746](./images/stm32/image-20240911150944746.png)

  

###### 18.3.5.2 通用定时器输入捕获脉宽测量原理

- ![image-20240911151926364](./images/stm32/image-20240911151926364.png)
- 向上计数的溢出条件：CNT = ARR ，但如果ARR = 0时，也最少需要一个时钟，所以这里要ARR + 1

###### 18.3.5.3 通用定时器输入捕获配置步骤

- ![image-20240911152857254](./images/stm32/image-20240911152857254.png)

- ![image-20240911152912020](./images/stm32/image-20240911152912020.png)

- ![image-20240911152921209](./images/stm32/image-20240911152921209.png)

  

###### 18.3.5.4 编程实战-通用定时器输入捕获实验

- 捕获UP按键按下的时间，UP按键按下是高电平，然后将时间打印出来
- PA0接UP键，GPIO设置为TIM5_CH1，设置模式为复用推挽
- CobeMX配置过程--TIM5

  - ![image-20240919092027257](./images/stm32/image-20240919092027257.png)
  - 

- tim.c

  - ```c
    /* USER CODE BEGIN Header */
    /**
      ******************************************************************************
      * @file    tim.c
      * @brief   This file provides code for the configuration
      *          of the TIM instances.
      ******************************************************************************
      * @attention
      *
      * Copyright (c) 2024 STMicroelectronics.
      * All rights reserved.
      *
      * This software is licensed under terms that can be found in the LICENSE file
      * in the root directory of this software component.
      * If no LICENSE file comes with this software, it is provided AS-IS.
      *
      ******************************************************************************
      */
    /* USER CODE END Header */
    /* Includes ------------------------------------------------------------------*/
    #include "tim.h"
    
    /* USER CODE BEGIN 0 */
    uint8_t tim5_cap_sta = 0;    //捕获状态标志
    uint16_t tim5_cap_val = 0;    //捕获计数器的值
    /* USER CODE END 0 */
    
    TIM_HandleTypeDef htim5;
    
    /* TIM5 init function */
    void MX_TIM5_Init(void)
    {
    
      /* USER CODE BEGIN TIM5_Init 0 */
    
      /* USER CODE END TIM5_Init 0 */
    
      TIM_ClockConfigTypeDef sClockSourceConfig = {0};
      TIM_MasterConfigTypeDef sMasterConfig = {0};
      TIM_IC_InitTypeDef sConfigIC = {0};
    
      /* USER CODE BEGIN TIM5_Init 1 */
    
      /* USER CODE END TIM5_Init 1 */
      htim5.Instance = TIM5;
      htim5.Init.Prescaler = 72-1;
      htim5.Init.CounterMode = TIM_COUNTERMODE_UP;
      htim5.Init.Period = 65535;
      htim5.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
      htim5.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_ENABLE;
      if (HAL_TIM_Base_Init(&htim5) != HAL_OK)
      {
        Error_Handler();
      }
      sClockSourceConfig.ClockSource = TIM_CLOCKSOURCE_INTERNAL;
      if (HAL_TIM_ConfigClockSource(&htim5, &sClockSourceConfig) != HAL_OK)
      {
        Error_Handler();
      }
      if (HAL_TIM_IC_Init(&htim5) != HAL_OK)
      {
        Error_Handler();
      }
      sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
      sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
      if (HAL_TIMEx_MasterConfigSynchronization(&htim5, &sMasterConfig) != HAL_OK)
      {
        Error_Handler();
      }
      sConfigIC.ICPolarity = TIM_INPUTCHANNELPOLARITY_RISING;
      sConfigIC.ICSelection = TIM_ICSELECTION_DIRECTTI;
      sConfigIC.ICPrescaler = TIM_ICPSC_DIV1;
      sConfigIC.ICFilter = 0;
      if (HAL_TIM_IC_ConfigChannel(&htim5, &sConfigIC, TIM_CHANNEL_1) != HAL_OK)
      {
        Error_Handler();
      }
      /* USER CODE BEGIN TIM5_Init 2 */
        __HAL_TIM_ENABLE_IT(&htim5, TIM_IT_UPDATE);         //使能更新中断
        HAL_TIM_IC_Start_IT(&htim5, TIM_CHANNEL_1);  //使能捕获、捕获中断及计数器
        
      /* USER CODE END TIM5_Init 2 */
    
    }
    
    void HAL_TIM_Base_MspInit(TIM_HandleTypeDef* tim_baseHandle)
    {
    
      GPIO_InitTypeDef GPIO_InitStruct = {0};
      if(tim_baseHandle->Instance==TIM5)
      {
      /* USER CODE BEGIN TIM5_MspInit 0 */
    
      /* USER CODE END TIM5_MspInit 0 */
        /* TIM5 clock enable */
        __HAL_RCC_TIM5_CLK_ENABLE();
    
        __HAL_RCC_GPIOA_CLK_ENABLE();
        /**TIM5 GPIO Configuration
        PA0-WKUP     ------> TIM5_CH1
        */
        GPIO_InitStruct.Pin = KEY_UP_Pin;
        GPIO_InitStruct.Mode = GPIO_MODE_AF_PP;
        GPIO_InitStruct.Pull = GPIO_PULLDOWN;
        GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_HIGH;
        HAL_GPIO_Init(KEY_UP_GPIO_Port, &GPIO_InitStruct);
    
        /* TIM5 interrupt Init */
        HAL_NVIC_SetPriority(TIM5_IRQn, 2, 3);
        HAL_NVIC_EnableIRQ(TIM5_IRQn);
      /* USER CODE BEGIN TIM5_MspInit 1 */
    
      /* USER CODE END TIM5_MspInit 1 */
      }
    }
    
    void HAL_TIM_Base_MspDeInit(TIM_HandleTypeDef* tim_baseHandle)
    {
    
      if(tim_baseHandle->Instance==TIM5)
      {
      /* USER CODE BEGIN TIM5_MspDeInit 0 */
    
      /* USER CODE END TIM5_MspDeInit 0 */
        /* Peripheral clock disable */
        __HAL_RCC_TIM5_CLK_DISABLE();
    
        /**TIM5 GPIO Configuration
        PA0-WKUP     ------> TIM5_CH1
        */
        HAL_GPIO_DeInit(KEY_UP_GPIO_Port, KEY_UP_Pin);
    
        /* TIM5 interrupt Deinit */
        HAL_NVIC_DisableIRQ(TIM5_IRQn);
      /* USER CODE BEGIN TIM5_MspDeInit 1 */
    
      /* USER CODE END TIM5_MspDeInit 1 */
      }
    }
    
    /* USER CODE BEGIN 1 */
    void HAL_TIM_IC_MspInit(TIM_HandleTypeDef *htim)
    {
        GPIO_InitTypeDef GPIO_InitStruct = {0};
      if(htim->Instance==TIM5)
      {
      /* USER CODE BEGIN TIM5_MspInit 0 */
    
      /* USER CODE END TIM5_MspInit 0 */
        /* TIM5 clock enable */
        __HAL_RCC_TIM5_CLK_ENABLE();
    
        __HAL_RCC_GPIOA_CLK_ENABLE();
        /**TIM5 GPIO Configuration
        PA0-WKUP     ------> TIM5_CH1
        */
        GPIO_InitStruct.Pin = KEY_UP_Pin;
        GPIO_InitStruct.Mode = GPIO_MODE_AF_PP;   //复用推挽
        GPIO_InitStruct.Pull = GPIO_PULLDOWN;
        GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_HIGH;
        HAL_GPIO_Init(KEY_UP_GPIO_Port, &GPIO_InitStruct);
    
        /* TIM5 interrupt Init */
        HAL_NVIC_SetPriority(TIM5_IRQn, 2, 3);
        HAL_NVIC_EnableIRQ(TIM5_IRQn);
      /* USER CODE BEGIN TIM5_MspInit 1 */
    
      /* USER CODE END TIM5_MspInit 1 */
      }
    }
    /* 中断服务函数 */
    void TIM5_IRQHandler(void)
    {
      /* USER CODE BEGIN TIM5_IRQn 0 */
    
      /* USER CODE END TIM5_IRQn 0 */
      HAL_TIM_IRQHandler(&htim5);
      /* USER CODE BEGIN TIM5_IRQn 1 */
    
      /* USER CODE END TIM5_IRQn 1 */
    }
    /* 捕获回调函数 */
    void HAL_TIM_IC_CaptureCallback(TIM_HandleTypeDef *htim)
    {
        if( htim->Instance == TIM5)
        {
            if((tim5_cap_sta & 0x80) == 0)   //没有捕获完成
            {
                if(tim5_cap_sta & 0x40)    //已经捕获到一个上升沿，现在是捕获到了下降沿才能进入函数体
                {
                    tim5_cap_sta |= 0x80;    //标记成功捕获
                    tim5_cap_val = HAL_TIM_ReadCapturedValue(&htim5, TIM_CHANNEL_1);
                    TIM_RESET_CAPTUREPOLARITY(&htim5, TIM_CHANNEL_1);    //清除设置
                    TIM_SET_CAPTUREPOLARITY(&htim5, TIM_CHANNEL_1, TIM_INPUTCHANNELPOLARITY_RISING);
                }
                else   //第一次捕获到上升沿
                {
                    tim5_cap_sta = 0;
                    tim5_cap_val = 0;
                    tim5_cap_sta |= 0x40;   //标记捕获到了一次上升沿
                    __HAL_TIM_DISABLE(&htim5);
                    __HAL_TIM_SET_COUNTER(&htim5, 0);
                    TIM_RESET_CAPTUREPOLARITY(&htim5, TIM_CHANNEL_1);    //清除设置
                    TIM_SET_CAPTUREPOLARITY(&htim5, TIM_CHANNEL_1, TIM_INPUTCHANNELPOLARITY_FALLING);
                    __HAL_TIM_ENABLE(&htim5);
                }
            }
        }
        
    }
    /* 更新回调函数 */
    void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
    {
        if( htim->Instance == TIM5)
        {
            if((tim5_cap_sta & 0x80) == 0)
            {
                if(tim5_cap_sta & 0x40)
                {
                    if((tim5_cap_sta & 0x3F) == 0x3F)     //高电平时间超长，计数到存储最大值就自动捕获
                    {
                        
                        TIM_RESET_CAPTUREPOLARITY(&htim5, TIM_CHANNEL_1);    //清除设置
                        TIM_SET_CAPTUREPOLARITY(&htim5, TIM_CHANNEL_1, TIM_INPUTCHANNELPOLARITY_RISING);
                        tim5_cap_sta |= 0x80;
                        tim5_cap_val = 0xFFFF;
                    }
                    else
                    {
                        tim5_cap_sta++;
                    }
                }
            }
        }
        
    }
    /* USER CODE END 1 */
    
    ```

  - main.c

    - ```c
      /* USER CODE BEGIN Header */
      /**
        ******************************************************************************
        * @file           : main.c
        * @brief          : Main program body
        ******************************************************************************
        * @attention
        *
        * Copyright (c) 2024 STMicroelectronics.
        * All rights reserved.
        *
        * This software is licensed under terms that can be found in the LICENSE file
        * in the root directory of this software component.
        * If no LICENSE file comes with this software, it is provided AS-IS.
        *
        ******************************************************************************
        */
      /* USER CODE END Header */
      /* Includes ------------------------------------------------------------------*/
      #include "main.h"
      #include "tim.h"
      #include "usart.h"
      #include "gpio.h"
      
      /* Private includes ----------------------------------------------------------*/
      /* USER CODE BEGIN Includes */
      
      /* USER CODE END Includes */
      
      /* Private typedef -----------------------------------------------------------*/
      /* USER CODE BEGIN PTD */
      
      /* USER CODE END PTD */
      
      /* Private define ------------------------------------------------------------*/
      /* USER CODE BEGIN PD */
      
      /* USER CODE END PD */
      
      /* Private macro -------------------------------------------------------------*/
      /* USER CODE BEGIN PM */
      
      /* USER CODE END PM */
      
      /* Private variables ---------------------------------------------------------*/
      
      /* USER CODE BEGIN PV */
      
      /* USER CODE END PV */
      
      /* Private function prototypes -----------------------------------------------*/
      void SystemClock_Config(void);
      /* USER CODE BEGIN PFP */
      
      /* USER CODE END PFP */
      
      /* Private user code ---------------------------------------------------------*/
      /* USER CODE BEGIN 0 */
      
      /* USER CODE END 0 */
      
      /**
        * @brief  The application entry point.
        * @retval int
        */
      int main(void)
      {
        /* USER CODE BEGIN 1 */
          uint32_t temp = 0;    //存储计数值
          uint8_t t = 0;
        /* USER CODE END 1 */
      
        /* MCU Configuration--------------------------------------------------------*/
      
        /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
        HAL_Init();
      
        /* USER CODE BEGIN Init */
      
        /* USER CODE END Init */
      
        /* Configure the system clock */
        SystemClock_Config();
      
        /* USER CODE BEGIN SysInit */
      
        /* USER CODE END SysInit */
      
        /* Initialize all configured peripherals */
        MX_GPIO_Init();
        MX_TIM5_Init();
        MX_USART1_UART_Init();
        /* USER CODE BEGIN 2 */
      
        /* USER CODE END 2 */
      
        /* Infinite loop */
        /* USER CODE BEGIN WHILE */
        while (1)
        {
           
            if(tim5_cap_sta & 0x80)   //成功捕获高电平
            {
                
                temp = tim5_cap_sta & 0x3F;
                temp *= 65536;
                temp += tim5_cap_val;
                printf("HIGH：%d us\r\n",temp);
                tim5_cap_sta = 0;
            }
            
            t++;
           
            if(t > 20)
            {
                
                HAL_GPIO_TogglePin(LED1_GPIO_Port,LED1_Pin);
                t = 0;
                
            }
            
            HAL_Delay(10);
            
          /* USER CODE END WHILE */
      
          /* USER CODE BEGIN 3 */
        }
        /* USER CODE END 3 */
      }
      
      /**
        * @brief System Clock Configuration
        * @retval None
        */
      void SystemClock_Config(void)
      {
        RCC_OscInitTypeDef RCC_OscInitStruct = {0};
        RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};
      
        /** Initializes the RCC Oscillators according to the specified parameters
        * in the RCC_OscInitTypeDef structure.
        */
        RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
        RCC_OscInitStruct.HSEState = RCC_HSE_ON;
        RCC_OscInitStruct.HSEPredivValue = RCC_HSE_PREDIV_DIV1;
        RCC_OscInitStruct.HSIState = RCC_HSI_ON;
        RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;
        RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSE;
        RCC_OscInitStruct.PLL.PLLMUL = RCC_PLL_MUL9;
        if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
        {
          Error_Handler();
        }
      
        /** Initializes the CPU, AHB and APB buses clocks
        */
        RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                                    |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
        RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;
        RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
        RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV2;
        RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;
      
        if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_2) != HAL_OK)
        {
          Error_Handler();
        }
      }
      
      /* USER CODE BEGIN 4 */
      
      /* USER CODE END 4 */
      
      /**
        * @brief  This function is executed in case of error occurrence.
        * @retval None
        */
      void Error_Handler(void)
      {
        /* USER CODE BEGIN Error_Handler_Debug */
        /* User can add his own implementation to report the HAL error return state */
        __disable_irq();
        while (1)
        {
        }
        /* USER CODE END Error_Handler_Debug */
      }
      
      #ifdef  USE_FULL_ASSERT
      /**
        * @brief  Reports the name of the source file and the source line number
        *         where the assert_param error has occurred.
        * @param  file: pointer to the source file name
        * @param  line: assert_param error line source number
        * @retval None
        */
      void assert_failed(uint8_t *file, uint32_t line)
      {
        /* USER CODE BEGIN 6 */
        /* User can add his own implementation to report the file name and line number,
           ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
        /* USER CODE END 6 */
      }
      #endif /* USE_FULL_ASSERT */
      
      ```

      


##### 18.3.6 通用定时器脉冲计数实验

- 通用定时器有四种时钟来源
- ![image-20240912165259448](./images/stm32/image-20240912165259448.png)
- ![image-20240912165358452](./images/stm32/image-20240912165358452.png)
- ![image-20240912165433089](./images/stm32/image-20240912165433089.png)
- ![image-20240912165450768](./images/stm32/image-20240912165450768.png)
- ![image-20240912165508752](./images/stm32/image-20240912165508752.png)
- 编程实战：通用定时器脉冲计数
  - 将定时器2通道1输入的高电平脉冲作为定时器2的时钟，并通过串口打印脉冲数
  - 实现原理，PSC = 0，ARR = 65536
    - 从模式设置：使用外部时钟1
    - 向上计数模式：按键UP按下一次，来一个上升沿，计数+1
    - 双边沿计数模式：按键UP按下一次，来一个上升沿和一个下降沿，计数+2
    - psc不分频，才可以实现来一个脉冲，计数一次
    - 不加溢出更新中断时，最多只能计数65536个，加了更新中断，可以存储溢出次数，从而计数更多
  
- tim.c

  - ```c
    /* USER CODE BEGIN Header */
    /**
      ******************************************************************************
      * @file    tim.c
      * @brief   This file provides code for the configuration
      *          of the TIM instances.
      ******************************************************************************
      * @attention
      *
      * Copyright (c) 2024 STMicroelectronics.
      * All rights reserved.
      *
      * This software is licensed under terms that can be found in the LICENSE file
      * in the root directory of this software component.
      * If no LICENSE file comes with this software, it is provided AS-IS.
      *
      ******************************************************************************
      */
    /* USER CODE END Header */
    /* Includes ------------------------------------------------------------------*/
    #include "tim.h"
    
    /* USER CODE BEGIN 0 */
    
    /* USER CODE END 0 */
    
    TIM_HandleTypeDef htim2;
    
    /* TIM2 init function */
    void MX_TIM2_Init(void)
    {
    
      /* USER CODE BEGIN TIM2_Init 0 */
    
      /* USER CODE END TIM2_Init 0 */
    
      TIM_SlaveConfigTypeDef sSlaveConfig = {0};
      TIM_MasterConfigTypeDef sMasterConfig = {0};
    
      /* USER CODE BEGIN TIM2_Init 1 */
    
      /* USER CODE END TIM2_Init 1 */
      htim2.Instance = TIM2;
      htim2.Init.Prescaler = 0;
      htim2.Init.CounterMode = TIM_COUNTERMODE_UP;
      htim2.Init.Period = 65535;
      htim2.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
      htim2.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
      if (HAL_TIM_Base_Init(&htim2) != HAL_OK)
      {
        Error_Handler();
      }
      sSlaveConfig.SlaveMode = TIM_SLAVEMODE_EXTERNAL1;
      sSlaveConfig.InputTrigger = TIM_TS_TI1F_ED;
      sSlaveConfig.TriggerPolarity = TIM_TRIGGERPOLARITY_RISING;
      sSlaveConfig.TriggerFilter = 0;
      if (HAL_TIM_SlaveConfigSynchro(&htim2, &sSlaveConfig) != HAL_OK)
      {
        Error_Handler();
      }
      sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
      sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
      if (HAL_TIMEx_MasterConfigSynchronization(&htim2, &sMasterConfig) != HAL_OK)
      {
        Error_Handler();
      }
      /* USER CODE BEGIN TIM2_Init 2 */
        if(HAL_TIM_IC_Init(&htim2) != HAL_OK)
        {
            Error_Handler();
        }
        HAL_TIM_IC_Start(&htim2, TIM_CHANNEL_1);
      /* USER CODE END TIM2_Init 2 */
    
    }
    
    void HAL_TIM_Base_MspInit(TIM_HandleTypeDef* tim_baseHandle)
    {
    
      GPIO_InitTypeDef GPIO_InitStruct = {0};
      if(tim_baseHandle->Instance==TIM2)
      {
      /* USER CODE BEGIN TIM2_MspInit 0 */
    
      /* USER CODE END TIM2_MspInit 0 */
        /* TIM2 clock enable */
        __HAL_RCC_TIM2_CLK_ENABLE();
    
        __HAL_RCC_GPIOA_CLK_ENABLE();
        /**TIM2 GPIO Configuration
        PA0-WKUP     ------> TIM2_CH1
        */
        GPIO_InitStruct.Pin = KEY_UP_Pin;
        GPIO_InitStruct.Mode = GPIO_MODE_AF_PP;
        GPIO_InitStruct.Pull = GPIO_PULLDOWN;
        GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_HIGH;
        HAL_GPIO_Init(KEY_UP_GPIO_Port, &GPIO_InitStruct);
    
      /* USER CODE BEGIN TIM2_MspInit 1 */
    
      /* USER CODE END TIM2_MspInit 1 */
      }
    }
    
    void HAL_TIM_Base_MspDeInit(TIM_HandleTypeDef* tim_baseHandle)
    {
    
      if(tim_baseHandle->Instance==TIM2)
      {
      /* USER CODE BEGIN TIM2_MspDeInit 0 */
    
      /* USER CODE END TIM2_MspDeInit 0 */
        /* Peripheral clock disable */
        __HAL_RCC_TIM2_CLK_DISABLE();
    
        /**TIM2 GPIO Configuration
        PA0-WKUP     ------> TIM2_CH1
        */
        HAL_GPIO_DeInit(KEY_UP_GPIO_Port, KEY_UP_Pin);
    
      /* USER CODE BEGIN TIM2_MspDeInit 1 */
    
      /* USER CODE END TIM2_MspDeInit 1 */
      }
    }
    
    /* USER CODE BEGIN 1 */
    void HAL_TIM_IC_MspInit(TIM_HandleTypeDef *htim)
    {
        GPIO_InitTypeDef GPIO_InitStruct = {0};
      if(htim->Instance==TIM2)
      {
      /* USER CODE BEGIN TIM2_MspInit 0 */
    
      /* USER CODE END TIM2_MspInit 0 */
        /* TIM2 clock enable */
        __HAL_RCC_TIM2_CLK_ENABLE();
    
        __HAL_RCC_GPIOA_CLK_ENABLE();
        /**TIM2 GPIO Configuration
        PA0-WKUP     ------> TIM2_CH1
        */
        GPIO_InitStruct.Pin = KEY_UP_Pin;
        GPIO_InitStruct.Mode = GPIO_MODE_AF_PP;
        GPIO_InitStruct.Pull = GPIO_PULLDOWN;
        GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_HIGH;
        HAL_GPIO_Init(KEY_UP_GPIO_Port, &GPIO_InitStruct);
    
      /* USER CODE BEGIN TIM2_MspInit 1 */
    
      /* USER CODE END TIM2_MspInit 1 */
      }
    }
    /* USER CODE END 1 */
    
    ```

  - main.c

    - ```c
      /* USER CODE BEGIN Header */
      /**
        ******************************************************************************
        * @file           : main.c
        * @brief          : Main program body
        ******************************************************************************
        * @attention
        *
        * Copyright (c) 2024 STMicroelectronics.
        * All rights reserved.
        *
        * This software is licensed under terms that can be found in the LICENSE file
        * in the root directory of this software component.
        * If no LICENSE file comes with this software, it is provided AS-IS.
        *
        ******************************************************************************
        */
      /* USER CODE END Header */
      /* Includes ------------------------------------------------------------------*/
      #include "main.h"
      #include "tim.h"
      #include "usart.h"
      #include "gpio.h"
      
      /* Private includes ----------------------------------------------------------*/
      /* USER CODE BEGIN Includes */
      
      /* USER CODE END Includes */
      
      /* Private typedef -----------------------------------------------------------*/
      /* USER CODE BEGIN PTD */
      
      /* USER CODE END PTD */
      
      /* Private define ------------------------------------------------------------*/
      /* USER CODE BEGIN PD */
      
      /* USER CODE END PD */
      
      /* Private macro -------------------------------------------------------------*/
      /* USER CODE BEGIN PM */
      
      /* USER CODE END PM */
      
      /* Private variables ---------------------------------------------------------*/
      
      /* USER CODE BEGIN PV */
      
      /* USER CODE END PV */
      
      /* Private function prototypes -----------------------------------------------*/
      void SystemClock_Config(void);
      /* USER CODE BEGIN PFP */
      
      /* USER CODE END PFP */
      
      /* Private user code ---------------------------------------------------------*/
      /* USER CODE BEGIN 0 */
      
      /* USER CODE END 0 */
      
      /**
        * @brief  The application entry point.
        * @retval int
        */
      int main(void)
      {
        /* USER CODE BEGIN 1 */
          uint8_t key = 0;    //扫描键值
          uint8_t t = 0;
          uint16_t oldCnt = 0;
          uint16_t newCnt = 0;
        /* USER CODE END 1 */
      
        /* MCU Configuration--------------------------------------------------------*/
      
        /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
        HAL_Init();
      
        /* USER CODE BEGIN Init */
      
        /* USER CODE END Init */
      
        /* Configure the system clock */
        SystemClock_Config();
      
        /* USER CODE BEGIN SysInit */
      
        /* USER CODE END SysInit */
      
        /* Initialize all configured peripherals */
        MX_GPIO_Init();
        MX_TIM2_Init();
        MX_USART1_UART_Init();
        /* USER CODE BEGIN 2 */
      
        /* USER CODE END 2 */
      
        /* Infinite loop */
        /* USER CODE BEGIN WHILE */
        while (1)
        {
            key = key_scan(0);
            if(key == KEY0_PRES)
            {
                __HAL_TIM_SET_COUNTER(&htim2, 0);
            }
            newCnt = __HAL_TIM_GET_COUNTER(&htim2);
            if( oldCnt != newCnt)
            {
                oldCnt = newCnt;
                printf("CNT = %d\r\n",oldCnt);
            }
            t++;
            if(t > 20)
            {
                HAL_GPIO_TogglePin(LED0_GPIO_Port,LED0_Pin);
                t = 0;
            }
            HAL_Delay(10);
          /* USER CODE END WHILE */
      
          /* USER CODE BEGIN 3 */
        }
        /* USER CODE END 3 */
      }
      
      /**
        * @brief System Clock Configuration
        * @retval None
        */
      void SystemClock_Config(void)
      {
        RCC_OscInitTypeDef RCC_OscInitStruct = {0};
        RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};
      
        /** Initializes the RCC Oscillators according to the specified parameters
        * in the RCC_OscInitTypeDef structure.
        */
        RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
        RCC_OscInitStruct.HSEState = RCC_HSE_ON;
        RCC_OscInitStruct.HSEPredivValue = RCC_HSE_PREDIV_DIV1;
        RCC_OscInitStruct.HSIState = RCC_HSI_ON;
        RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;
        RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSE;
        RCC_OscInitStruct.PLL.PLLMUL = RCC_PLL_MUL9;
        if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
        {
          Error_Handler();
        }
      
        /** Initializes the CPU, AHB and APB buses clocks
        */
        RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                                    |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
        RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;
        RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
        RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV2;
        RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;
      
        if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_2) != HAL_OK)
        {
          Error_Handler();
        }
      }
      
      /* USER CODE BEGIN 4 */
      
      /* USER CODE END 4 */
      
      /**
        * @brief  This function is executed in case of error occurrence.
        * @retval None
        */
      void Error_Handler(void)
      {
        /* USER CODE BEGIN Error_Handler_Debug */
        /* User can add his own implementation to report the HAL error return state */
        __disable_irq();
        while (1)
        {
        }
        /* USER CODE END Error_Handler_Debug */
      }
      
      #ifdef  USE_FULL_ASSERT
      /**
        * @brief  Reports the name of the source file and the source line number
        *         where the assert_param error has occurred.
        * @param  file: pointer to the source file name
        * @param  line: assert_param error line source number
        * @retval None
        */
      void assert_failed(uint8_t *file, uint32_t line)
      {
        /* USER CODE BEGIN 6 */
        /* User can add his own implementation to report the file name and line number,
           ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
        /* USER CODE END 6 */
      }
      #endif /* USE_FULL_ASSERT */
      
      ```

      


#### 18.4 高级定时器

##### 18.4.1 高级定时器简介

- ![image-20240912174141146](./images/stm32/image-20240912174141146.png)
  - 在通用定时器的基础上，多了三点
    - 有重复计数器，设置计数器溢出多少次才产生一次更新事件
    - 死区时间带可编程的互补输出
    - 断路输入，用于将定时器的输入信号置于用户可选的安全配置中

##### 18.4.2 高级定时器框图

- ![image-20240912174445051](./images/stm32/image-20240912174445051.png)

##### 18.4.3 高级定时器输出指定个数PWM实验

###### 18.4.3.1 重复计数器特性

- ![image-20240913093639750](./images/stm32/image-20240913093639750.png)
  - 要点：如果设置RCR为N，更新事件将在N+1次溢出事件发生

###### 18.4.3.2 高级定时器输出指定个数PWM实验原理

- ![image-20240913094828427](./images/stm32/image-20240913094828427.png)

- 更新中断内，关闭计数器？

  - 进入中断就说明已经产生了N个PWM，即N个溢出事件，这里关闭计数器，就代表停止计数

  - PC6设置为TIM8_CH1，然后用杜邦线将PC6接到PE5上，即接到LED1上，输出几个PWM，LED1就闪几次

    - 这里PC6设为上拉复用推挽输出
    - PE5设为上拉输入，避免与PC6产生冲突

  - 设置N个PWM

    - ```c
      /* 高级定时器TIMX NPWM设置PWM个数函数 */
      void atim_timx_npwm_chy_set(uint8_t npwm)
      {
          if(npwm == 0) return;
          
          g_npwm_remain = npwm;
          HAL_TIM_GenerateEvent(&g_timx_npwm_chy_handle, TIM_EVENTSOURCE_UPDATE); //软件更新，就是重新开始计数
          __HAL_TIM_ENABLE(&g_timx_npwm_chy_handle);  //使能定时器，就会产生一次溢出事件而产生更新中断
      }
      
      ```

  - 更新中断回调函数

    - ```c
      /* 定时器更新中断回调函数 */
      void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
      {
          if (htim->Instance == TIM8)
          {
              if(g_npwm_remain)   //第一次进入回调，按RCR初始值是0，产生了一次溢出事件就进入回调
              {
                  TIM8->RCR = g_npwm_remain - 1;  //重新设置RCR
                  HAL_TIM_GenerateEvent(&g_timx_npwm_chy_handle, TIM_EVENTSOURCE_UPDATE);   //软件更新，重新开始计数
                  __HAL_TIM_ENABLE(&g_timx_npwm_chy_handle);   //启动定时器
                  g_npwm_remain = 0;  //数值归0
              }
              else    //第2次进入回调，说明已经产生了g_npwm_remain次溢出事件
              {
                  TIM8->CR1 &= ~(1 << 0);  //关闭计数器，等待下一次设置RCR值
              }
          }
      }
      ```

  - main.c

    - ```c
      /**
       ****************************************************************************************************
       * @file        main.c
       * @author      正点原子团队(ALIENTEK)
       * @version     V1.0
       * @date        2020-04-20
       * @brief       跑马灯 实验
       * @license     Copyright (c) 2020-2032, 广州市星翼电子科技有限公司
       ****************************************************************************************************
       * @attention
       *
       * 实验平台:正点原子 STM32F103开发板
       * 在线视频:www.yuanzige.com
       * 技术论坛:www.openedv.com
       * 公司网址:www.alientek.com
       * 购买地址:openedv.taobao.com
       *
       ****************************************************************************************************
       */
      
      #include "./SYSTEM/sys/sys.h"
      #include "./SYSTEM/usart/usart.h"
      #include "./SYSTEM/delay/delay.h"
      #include "./BSP/LED/led.h"
      #include "./BSP/KEY/key.h"
      #include "./BSP/TIMER/atim.h"
      
      
      int main(void)
      {
          uint8_t key;
          uint8_t t = 0;
          GPIO_InitTypeDef gpio_init_struct;
          
          HAL_Init();                         /* 初始化HAL库 */
          sys_stm32_clock_init(RCC_PLL_MUL9); /* 设置时钟, 72Mhz */
          delay_init(72);                     /* 延时初始化 */
          usart_init(115200);                 /* 串口初始化为115200 */
          led_init();                         /* 初始化LED */
          key_init();                         /* 初始化按键 */
          
          /* 把PE5设置为输入，避免与PC6冲突 */
          __HAL_RCC_GPIOE_CLK_ENABLE();
          gpio_init_struct.Pin = GPIO_PIN_5;
          gpio_init_struct.Mode = GPIO_MODE_INPUT;                /* 输入 */
          gpio_init_struct.Pull = GPIO_PULLUP;                    /* 上拉 */
          gpio_init_struct.Speed = GPIO_SPEED_FREQ_HIGH;          /* 高速 */
          HAL_GPIO_Init(GPIOE, &gpio_init_struct);
          
          atim_timx_npwm_chy_init(5000 - 1, 7200 - 1);  //设置定时器参数
          
          atim_timx_npwm_chy_set(5);   //设置输出PWM个数
          
          while (1)
          {
              key = key_scan(0);
              if(key == KEY0_PRES)
              {
                  atim_timx_npwm_chy_set(6);   //重新设置PWM个数
              }
       
              t++;
              delay_ms(10);
      
              if (t > 50)                    /* 控制LED0闪烁, 提示程序运行状态 */
              {
                  t = 0;
                  LED0_TOGGLE();
              }
          }
      }
      
      ```

      

- 注意：高级定时器通道输出必须把MOE位置1

  - ![image-20240913095308134](./images/stm32/image-20240913095308134.png)

###### 18.4.3.3 高级定时器输出指定个数PWM实验配置步骤

- ![image-20240913095928588](./images/stm32/image-20240913095928588.png)
- ![image-20240913100023312](./images/stm32/image-20240913100023312.png)

###### 18.4.3.4 编程实战：高级定时器输出指定个数PWM实验

- 输出6个PWM，对应的LED1状态翻转6次
- ![image-20240913102751570](./images/stm32/image-20240913102751570.png)
- CobeMX配置步骤

  - ![image-20240919092427114](./images/stm32/image-20240919092427114.png)

- tim.c

  - ```c
    /* USER CODE BEGIN Header */
    /**
      ******************************************************************************
      * @file    tim.c
      * @brief   This file provides code for the configuration
      *          of the TIM instances.
      ******************************************************************************
      * @attention
      *
      * Copyright (c) 2024 STMicroelectronics.
      * All rights reserved.
      *
      * This software is licensed under terms that can be found in the LICENSE file
      * in the root directory of this software component.
      * If no LICENSE file comes with this software, it is provided AS-IS.
      *
      ******************************************************************************
      */
    /* USER CODE END Header */
    /* Includes ------------------------------------------------------------------*/
    #include "tim.h"
    
    /* USER CODE BEGIN 0 */
    static uint8_t tim8_rcr_set= 0;
    /* USER CODE END 0 */
    
    TIM_HandleTypeDef htim8;
    
    /* TIM8 init function */
    void MX_TIM8_Init(void)
    {
    
      /* USER CODE BEGIN TIM8_Init 0 */
    
      /* USER CODE END TIM8_Init 0 */
    
      TIM_ClockConfigTypeDef sClockSourceConfig = {0};
      TIM_MasterConfigTypeDef sMasterConfig = {0};
      TIM_OC_InitTypeDef sConfigOC = {0};
      TIM_BreakDeadTimeConfigTypeDef sBreakDeadTimeConfig = {0};
    
      /* USER CODE BEGIN TIM8_Init 1 */
    
      /* USER CODE END TIM8_Init 1 */
      htim8.Instance = TIM8;
      htim8.Init.Prescaler = 7200-1;
      htim8.Init.CounterMode = TIM_COUNTERMODE_UP;
      htim8.Init.Period = 5000-1;
      htim8.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
      htim8.Init.RepetitionCounter = 0;
      htim8.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
      if (HAL_TIM_Base_Init(&htim8) != HAL_OK)
      {
        Error_Handler();
      }
      sClockSourceConfig.ClockSource = TIM_CLOCKSOURCE_INTERNAL;
      if (HAL_TIM_ConfigClockSource(&htim8, &sClockSourceConfig) != HAL_OK)
      {
        Error_Handler();
      }
      if (HAL_TIM_PWM_Init(&htim8) != HAL_OK)
      {
        Error_Handler();
      }
      sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
      sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
      if (HAL_TIMEx_MasterConfigSynchronization(&htim8, &sMasterConfig) != HAL_OK)
      {
        Error_Handler();
      }
      sConfigOC.OCMode = TIM_OCMODE_PWM1;
      sConfigOC.Pulse = (5000-1)/2;
      sConfigOC.OCPolarity = TIM_OCPOLARITY_HIGH;
      sConfigOC.OCNPolarity = TIM_OCNPOLARITY_HIGH;
      sConfigOC.OCFastMode = TIM_OCFAST_DISABLE;
      sConfigOC.OCIdleState = TIM_OCIDLESTATE_RESET;
      sConfigOC.OCNIdleState = TIM_OCNIDLESTATE_RESET;
      if (HAL_TIM_PWM_ConfigChannel(&htim8, &sConfigOC, TIM_CHANNEL_1) != HAL_OK)
      {
        Error_Handler();
      }
      sBreakDeadTimeConfig.OffStateRunMode = TIM_OSSR_DISABLE;
      sBreakDeadTimeConfig.OffStateIDLEMode = TIM_OSSI_DISABLE;
      sBreakDeadTimeConfig.LockLevel = TIM_LOCKLEVEL_OFF;
      sBreakDeadTimeConfig.DeadTime = 0;
      sBreakDeadTimeConfig.BreakState = TIM_BREAK_DISABLE;
      sBreakDeadTimeConfig.BreakPolarity = TIM_BREAKPOLARITY_HIGH;
      sBreakDeadTimeConfig.AutomaticOutput = TIM_AUTOMATICOUTPUT_DISABLE;
      if (HAL_TIMEx_ConfigBreakDeadTime(&htim8, &sBreakDeadTimeConfig) != HAL_OK)
      {
        Error_Handler();
      }
      /* USER CODE BEGIN TIM8_Init 2 */
        __HAL_TIM_ENABLE_IT(&htim8, TIM_IT_UPDATE);
        HAL_TIM_PWM_Start(&htim8, TIM_CHANNEL_1);
      /* USER CODE END TIM8_Init 2 */
      HAL_TIM_MspPostInit(&htim8);
    
    }
    
    void HAL_TIM_Base_MspInit(TIM_HandleTypeDef* tim_baseHandle)
    {
    
      if(tim_baseHandle->Instance==TIM8)
      {
      /* USER CODE BEGIN TIM8_MspInit 0 */
    
      /* USER CODE END TIM8_MspInit 0 */
        /* TIM8 clock enable */
        __HAL_RCC_TIM8_CLK_ENABLE();
    
        /* TIM8 interrupt Init */
        HAL_NVIC_SetPriority(TIM8_UP_IRQn, 2, 3);
        HAL_NVIC_EnableIRQ(TIM8_UP_IRQn);
      /* USER CODE BEGIN TIM8_MspInit 1 */
    
      /* USER CODE END TIM8_MspInit 1 */
      }
    }
    /* 在基本的初始化后进行额外的设置 */
    void HAL_TIM_MspPostInit(TIM_HandleTypeDef* timHandle)
    {
    
      GPIO_InitTypeDef GPIO_InitStruct = {0};
      if(timHandle->Instance==TIM8)
      {
      /* USER CODE BEGIN TIM8_MspPostInit 0 */
    
      /* USER CODE END TIM8_MspPostInit 0 */
    
        __HAL_RCC_GPIOC_CLK_ENABLE();
        /**TIM8 GPIO Configuration
        PC6     ------> TIM8_CH1
        */
        GPIO_InitStruct.Pin = GPIO_PIN_6;
        GPIO_InitStruct.Mode = GPIO_MODE_AF_PP;
        GPIO_InitStruct.Pull = GPIO_PULLUP;
        GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_HIGH;
        HAL_GPIO_Init(GPIOC, &GPIO_InitStruct);
    
      /* USER CODE BEGIN TIM8_MspPostInit 1 */
    
      /* USER CODE END TIM8_MspPostInit 1 */
      }
    
    }
    
    void HAL_TIM_Base_MspDeInit(TIM_HandleTypeDef* tim_baseHandle)
    {
    
      if(tim_baseHandle->Instance==TIM8)
      {
      /* USER CODE BEGIN TIM8_MspDeInit 0 */
    
      /* USER CODE END TIM8_MspDeInit 0 */
        /* Peripheral clock disable */
        __HAL_RCC_TIM8_CLK_DISABLE();
    
        /* TIM8 interrupt Deinit */
        HAL_NVIC_DisableIRQ(TIM8_UP_IRQn);
      /* USER CODE BEGIN TIM8_MspDeInit 1 */
    
      /* USER CODE END TIM8_MspDeInit 1 */
      }
    }
    
    /* USER CODE BEGIN 1 */
    //设置RCR值
    void tim8_set_pwm(uint8_t npwm)
    {
        if(npwm == 0)
        {
            return;
        }
        tim8_rcr_set = npwm;
        HAL_TIM_GenerateEvent(&htim8,TIM_EVENTSOURCE_UPDATE);  //软件更新中断
        __HAL_TIM_ENABLE(&htim8);  //定时器使能
    }
    /* 定时器8中断服务函数 */
    void TIM8_UP_IRQHandler(void)
    {
      /* USER CODE BEGIN TIM8_UP_IRQn 0 */
    
      /* USER CODE END TIM8_UP_IRQn 0 */
      HAL_TIM_IRQHandler(&htim8);
      /* USER CODE BEGIN TIM8_UP_IRQn 1 */
    
      /* USER CODE END TIM8_UP_IRQn 1 */
    }
    void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
    {
        if(htim->Instance == TIM8)
        {
            if(tim8_rcr_set)
            {
                TIM8->RCR = tim8_rcr_set-1;
                HAL_TIM_GenerateEvent(&htim8,TIM_EVENTSOURCE_UPDATE);
                __HAL_TIM_ENABLE(&htim8);
                tim8_rcr_set = 0;
            }
            else
            {
                TIM8->CR1 &= ~(1 << 0);
            }
        }
    }
    /* USER CODE END 1 */
    
    ```
  - main.c

    - ```c
      /* USER CODE BEGIN Header */
      /**
        ******************************************************************************
        * @file           : main.c
        * @brief          : Main program body
        ******************************************************************************
        * @attention
        *
        * Copyright (c) 2024 STMicroelectronics.
        * All rights reserved.
        *
        * This software is licensed under terms that can be found in the LICENSE file
        * in the root directory of this software component.
        * If no LICENSE file comes with this software, it is provided AS-IS.
        *
        ******************************************************************************
        */
      /* USER CODE END Header */
      /* Includes ------------------------------------------------------------------*/
      #include "main.h"
      #include "tim.h"
      #include "usart.h"
      #include "gpio.h"
      
      /* Private includes ----------------------------------------------------------*/
      /* USER CODE BEGIN Includes */
      
      /* USER CODE END Includes */
      
      /* Private typedef -----------------------------------------------------------*/
      /* USER CODE BEGIN PTD */
      
      /* USER CODE END PTD */
      
      /* Private define ------------------------------------------------------------*/
      /* USER CODE BEGIN PD */
      
      /* USER CODE END PD */
      
      /* Private macro -------------------------------------------------------------*/
      /* USER CODE BEGIN PM */
      
      /* USER CODE END PM */
      
      /* Private variables ---------------------------------------------------------*/
      
      /* USER CODE BEGIN PV */
      
      /* USER CODE END PV */
      
      /* Private function prototypes -----------------------------------------------*/
      void SystemClock_Config(void);
      /* USER CODE BEGIN PFP */
      
      /* USER CODE END PFP */
      
      /* Private user code ---------------------------------------------------------*/
      /* USER CODE BEGIN 0 */
      
      /* USER CODE END 0 */
      
      /**
        * @brief  The application entry point.
        * @retval int
        */
      int main(void)
      {
        /* USER CODE BEGIN 1 */
          uint8_t key = 0;
        /* USER CODE END 1 */
      
        /* MCU Configuration--------------------------------------------------------*/
      
        /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
        HAL_Init();
      
        /* USER CODE BEGIN Init */
      
        /* USER CODE END Init */
      
        /* Configure the system clock */
        SystemClock_Config();
      
        /* USER CODE BEGIN SysInit */
      
        /* USER CODE END SysInit */
      
        /* Initialize all configured peripherals */
        MX_GPIO_Init();
        MX_TIM8_Init();
        MX_USART1_UART_Init();
        /* USER CODE BEGIN 2 */
          tim8_set_pwm(5);
        /* USER CODE END 2 */
      
        /* Infinite loop */
        /* USER CODE BEGIN WHILE */
        while (1)
        {
            key = key_scan(0);
            if(key == KEY0_PRES)
            {
                tim8_set_pwm(3);
            }
          /* USER CODE END WHILE */
      
          /* USER CODE BEGIN 3 */
        }
        /* USER CODE END 3 */
      }
      
      /**
        * @brief System Clock Configuration
        * @retval None
        */
      void SystemClock_Config(void)
      {
        RCC_OscInitTypeDef RCC_OscInitStruct = {0};
        RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};
      
        /** Initializes the RCC Oscillators according to the specified parameters
        * in the RCC_OscInitTypeDef structure.
        */
        RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
        RCC_OscInitStruct.HSEState = RCC_HSE_ON;
        RCC_OscInitStruct.HSEPredivValue = RCC_HSE_PREDIV_DIV1;
        RCC_OscInitStruct.HSIState = RCC_HSI_ON;
        RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;
        RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSE;
        RCC_OscInitStruct.PLL.PLLMUL = RCC_PLL_MUL9;
        if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
        {
          Error_Handler();
        }
      
        /** Initializes the CPU, AHB and APB buses clocks
        */
        RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                                    |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
        RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;
        RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
        RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV2;
        RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;
      
        if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_2) != HAL_OK)
        {
          Error_Handler();
        }
      }
      
      /* USER CODE BEGIN 4 */
      
      /* USER CODE END 4 */
      
      /**
        * @brief  This function is executed in case of error occurrence.
        * @retval None
        */
      void Error_Handler(void)
      {
        /* USER CODE BEGIN Error_Handler_Debug */
        /* User can add his own implementation to report the HAL error return state */
        __disable_irq();
        while (1)
        {
        }
        /* USER CODE END Error_Handler_Debug */
      }
      
      #ifdef  USE_FULL_ASSERT
      /**
        * @brief  Reports the name of the source file and the source line number
        *         where the assert_param error has occurred.
        * @param  file: pointer to the source file name
        * @param  line: assert_param error line source number
        * @retval None
        */
      void assert_failed(uint8_t *file, uint32_t line)
      {
        /* USER CODE BEGIN 6 */
        /* User can add his own implementation to report the file name and line number,
           ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
        /* USER CODE END 6 */
      }
      #endif /* USE_FULL_ASSERT */
      
      ```

      


##### 18.4.4 高级定时器输出比较模式实验

###### 18.4.4.1 实验原理

- cnt = CCRx时，IO电平翻转
- 周期为2(ARR+1)，因为是从0开始计数，到等于ARR溢出
- 占空比固定在50%
- 相位：CCRx在循环波形中的位置，在0的时候就是**初相**
- ![image-20240913111613239](./images/stm32/image-20240913111613239.png)

###### 18.4.4.2 实验配置步骤

- ![image-20240913112219520](./images/stm32/image-20240913112219520.png)
- ![image-20240913112232419](./images/stm32/image-20240913112232419.png)
- ![image-20240913112252346](./images/stm32/image-20240913112252346.png)

###### 18.4.4.3 编程实战

- ![image-20240913114245934](./images/stm32/image-20240913114245934.png)
- CobeMX

  - ![image-20240919093417882](./images/stm32/image-20240919093417882.png)

  - ![image-20240919093500317](./images/stm32/image-20240919093500317.png)

    - 模式是触发模式

  - tim.c

    - ```c
      /* USER CODE BEGIN Header */
      /**
        ******************************************************************************
        * @file    tim.c
        * @brief   This file provides code for the configuration
        *          of the TIM instances.
        ******************************************************************************
        * @attention
        *
        * Copyright (c) 2024 STMicroelectronics.
        * All rights reserved.
        *
        * This software is licensed under terms that can be found in the LICENSE file
        * in the root directory of this software component.
        * If no LICENSE file comes with this software, it is provided AS-IS.
        *
        ******************************************************************************
        */
      /* USER CODE END Header */
      /* Includes ------------------------------------------------------------------*/
      #include "tim.h"
      
      /* USER CODE BEGIN 0 */
      
      /* USER CODE END 0 */
      
      TIM_HandleTypeDef htim8;
      
      /* TIM8 init function */
      void MX_TIM8_Init(void)
      {
      
        /* USER CODE BEGIN TIM8_Init 0 */
      
        /* USER CODE END TIM8_Init 0 */
      
        TIM_ClockConfigTypeDef sClockSourceConfig = {0};
        TIM_MasterConfigTypeDef sMasterConfig = {0};
        TIM_OC_InitTypeDef sConfigOC = {0};
        TIM_BreakDeadTimeConfigTypeDef sBreakDeadTimeConfig = {0};
      
        /* USER CODE BEGIN TIM8_Init 1 */
      
        /* USER CODE END TIM8_Init 1 */
        htim8.Instance = TIM8;
        htim8.Init.Prescaler = 72-1;
        htim8.Init.CounterMode = TIM_COUNTERMODE_UP;
        htim8.Init.Period = 1000-1;
        htim8.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
        htim8.Init.RepetitionCounter = 0;
        htim8.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
        if (HAL_TIM_Base_Init(&htim8) != HAL_OK)
        {
          Error_Handler();
        }
        sClockSourceConfig.ClockSource = TIM_CLOCKSOURCE_INTERNAL;
        if (HAL_TIM_ConfigClockSource(&htim8, &sClockSourceConfig) != HAL_OK)
        {
          Error_Handler();
        }
        if (HAL_TIM_OC_Init(&htim8) != HAL_OK)
        {
          Error_Handler();
        }
        sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
        sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
        if (HAL_TIMEx_MasterConfigSynchronization(&htim8, &sMasterConfig) != HAL_OK)
        {
          Error_Handler();
        }
        sConfigOC.OCMode = TIM_OCMODE_TOGGLE;
        sConfigOC.Pulse = 0;
        sConfigOC.OCPolarity = TIM_OCPOLARITY_HIGH;
        sConfigOC.OCNPolarity = TIM_OCNPOLARITY_HIGH;
        sConfigOC.OCFastMode = TIM_OCFAST_DISABLE;
        sConfigOC.OCIdleState = TIM_OCIDLESTATE_RESET;
        sConfigOC.OCNIdleState = TIM_OCNIDLESTATE_RESET;
        if (HAL_TIM_OC_ConfigChannel(&htim8, &sConfigOC, TIM_CHANNEL_1) != HAL_OK)
        {
          Error_Handler();
        }
        __HAL_TIM_ENABLE_OCxPRELOAD(&htim8, TIM_CHANNEL_1);
        if (HAL_TIM_OC_ConfigChannel(&htim8, &sConfigOC, TIM_CHANNEL_2) != HAL_OK)
        {
          Error_Handler();
        }
        __HAL_TIM_ENABLE_OCxPRELOAD(&htim8, TIM_CHANNEL_2);
        if (HAL_TIM_OC_ConfigChannel(&htim8, &sConfigOC, TIM_CHANNEL_3) != HAL_OK)
        {
          Error_Handler();
        }
        __HAL_TIM_ENABLE_OCxPRELOAD(&htim8, TIM_CHANNEL_3);
        if (HAL_TIM_OC_ConfigChannel(&htim8, &sConfigOC, TIM_CHANNEL_4) != HAL_OK)
        {
          Error_Handler();
        }
        __HAL_TIM_ENABLE_OCxPRELOAD(&htim8, TIM_CHANNEL_4);
        sBreakDeadTimeConfig.OffStateRunMode = TIM_OSSR_DISABLE;
        sBreakDeadTimeConfig.OffStateIDLEMode = TIM_OSSI_DISABLE;
        sBreakDeadTimeConfig.LockLevel = TIM_LOCKLEVEL_OFF;
        sBreakDeadTimeConfig.DeadTime = 0;
        sBreakDeadTimeConfig.BreakState = TIM_BREAK_DISABLE;
        sBreakDeadTimeConfig.BreakPolarity = TIM_BREAKPOLARITY_HIGH;
        sBreakDeadTimeConfig.AutomaticOutput = TIM_AUTOMATICOUTPUT_DISABLE;
        if (HAL_TIMEx_ConfigBreakDeadTime(&htim8, &sBreakDeadTimeConfig) != HAL_OK)
        {
          Error_Handler();
        }
        /* USER CODE BEGIN TIM8_Init 2 */
          HAL_TIM_OC_Start(&htim8, TIM_CHANNEL_1);
          HAL_TIM_OC_Start(&htim8, TIM_CHANNEL_2);
          HAL_TIM_OC_Start(&htim8, TIM_CHANNEL_3);
          HAL_TIM_OC_Start(&htim8, TIM_CHANNEL_4);
        /* USER CODE END TIM8_Init 2 */
        HAL_TIM_MspPostInit(&htim8);
      
      }
      
      void HAL_TIM_Base_MspInit(TIM_HandleTypeDef* tim_baseHandle)
      {
      
        if(tim_baseHandle->Instance==TIM8)
        {
        /* USER CODE BEGIN TIM8_MspInit 0 */
      
        /* USER CODE END TIM8_MspInit 0 */
          /* TIM8 clock enable */
          __HAL_RCC_TIM8_CLK_ENABLE();
        /* USER CODE BEGIN TIM8_MspInit 1 */
      
        /* USER CODE END TIM8_MspInit 1 */
        }
      }
      void HAL_TIM_MspPostInit(TIM_HandleTypeDef* timHandle)
      {
      
        GPIO_InitTypeDef GPIO_InitStruct = {0};
        if(timHandle->Instance==TIM8)
        {
        /* USER CODE BEGIN TIM8_MspPostInit 0 */
      
        /* USER CODE END TIM8_MspPostInit 0 */
      
          __HAL_RCC_GPIOC_CLK_ENABLE();
          /**TIM8 GPIO Configuration
          PC6     ------> TIM8_CH1
          PC7     ------> TIM8_CH2
          PC8     ------> TIM8_CH3
          PC9     ------> TIM8_CH4
          */
          GPIO_InitStruct.Pin = GPIO_PIN_6|GPIO_PIN_7|GPIO_PIN_8|GPIO_PIN_9;
          GPIO_InitStruct.Mode = GPIO_MODE_AF_PP;
          GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_HIGH;
          HAL_GPIO_Init(GPIOC, &GPIO_InitStruct);
      
        /* USER CODE BEGIN TIM8_MspPostInit 1 */
      
        /* USER CODE END TIM8_MspPostInit 1 */
        }
      
      }
      
      void HAL_TIM_Base_MspDeInit(TIM_HandleTypeDef* tim_baseHandle)
      {
      
        if(tim_baseHandle->Instance==TIM8)
        {
        /* USER CODE BEGIN TIM8_MspDeInit 0 */
      
        /* USER CODE END TIM8_MspDeInit 0 */
          /* Peripheral clock disable */
          __HAL_RCC_TIM8_CLK_DISABLE();
        /* USER CODE BEGIN TIM8_MspDeInit 1 */
      
        /* USER CODE END TIM8_MspDeInit 1 */
        }
      }
      
      /* USER CODE BEGIN 1 */
      
      /* USER CODE END 1 */
      
      ```

    - main.c

      - ```c
        /* USER CODE BEGIN Header */
        /**
          ******************************************************************************
          * @file           : main.c
          * @brief          : Main program body
          ******************************************************************************
          * @attention
          *
          * Copyright (c) 2024 STMicroelectronics.
          * All rights reserved.
          *
          * This software is licensed under terms that can be found in the LICENSE file
          * in the root directory of this software component.
          * If no LICENSE file comes with this software, it is provided AS-IS.
          *
          ******************************************************************************
          */
        /* USER CODE END Header */
        /* Includes ------------------------------------------------------------------*/
        #include "main.h"
        #include "tim.h"
        #include "gpio.h"
        
        /* Private includes ----------------------------------------------------------*/
        /* USER CODE BEGIN Includes */
        
        /* USER CODE END Includes */
        
        /* Private typedef -----------------------------------------------------------*/
        /* USER CODE BEGIN PTD */
        
        /* USER CODE END PTD */
        
        /* Private define ------------------------------------------------------------*/
        /* USER CODE BEGIN PD */
        
        /* USER CODE END PD */
        
        /* Private macro -------------------------------------------------------------*/
        /* USER CODE BEGIN PM */
        
        /* USER CODE END PM */
        
        /* Private variables ---------------------------------------------------------*/
        
        /* USER CODE BEGIN PV */
        
        /* USER CODE END PV */
        
        /* Private function prototypes -----------------------------------------------*/
        void SystemClock_Config(void);
        /* USER CODE BEGIN PFP */
        
        /* USER CODE END PFP */
        
        /* Private user code ---------------------------------------------------------*/
        /* USER CODE BEGIN 0 */
        
        /* USER CODE END 0 */
        
        /**
          * @brief  The application entry point.
          * @retval int
          */
        int main(void)
        {
          /* USER CODE BEGIN 1 */
            uint8_t t = 0;
          /* USER CODE END 1 */
        
          /* MCU Configuration--------------------------------------------------------*/
        
          /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
          HAL_Init();
        
          /* USER CODE BEGIN Init */
        
          /* USER CODE END Init */
        
          /* Configure the system clock */
          SystemClock_Config();
        
          /* USER CODE BEGIN SysInit */
        
          /* USER CODE END SysInit */
        
          /* Initialize all configured peripherals */
          MX_GPIO_Init();
          MX_TIM8_Init();
          /* USER CODE BEGIN 2 */
            __HAL_TIM_SET_COMPARE(&htim8, TIM_CHANNEL_1, 250-1);
            __HAL_TIM_SET_COMPARE(&htim8, TIM_CHANNEL_2, 500-1);
            __HAL_TIM_SET_COMPARE(&htim8, TIM_CHANNEL_3, 750-1);
            __HAL_TIM_SET_COMPARE(&htim8, TIM_CHANNEL_4, 1000-1);
          /* USER CODE END 2 */
        
          /* Infinite loop */
          /* USER CODE BEGIN WHILE */
          while (1)
          {
              t++;
              if(t > 20)
              {
                  HAL_GPIO_TogglePin(LED0_GPIO_Port,LED0_Pin);
                  t = 0;
              }
              HAL_Delay(10);
            /* USER CODE END WHILE */
            
            /* USER CODE BEGIN 3 */
          }
          /* USER CODE END 3 */
        }
        
        /**
          * @brief System Clock Configuration
          * @retval None
          */
        void SystemClock_Config(void)
        {
          RCC_OscInitTypeDef RCC_OscInitStruct = {0};
          RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};
        
          /** Initializes the RCC Oscillators according to the specified parameters
          * in the RCC_OscInitTypeDef structure.
          */
          RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
          RCC_OscInitStruct.HSEState = RCC_HSE_ON;
          RCC_OscInitStruct.HSEPredivValue = RCC_HSE_PREDIV_DIV1;
          RCC_OscInitStruct.HSIState = RCC_HSI_ON;
          RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;
          RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSE;
          RCC_OscInitStruct.PLL.PLLMUL = RCC_PLL_MUL9;
          if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
          {
            Error_Handler();
          }
        
          /** Initializes the CPU, AHB and APB buses clocks
          */
          RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                                      |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
          RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;
          RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
          RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV2;
          RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;
        
          if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_2) != HAL_OK)
          {
            Error_Handler();
          }
        }
        
        /* USER CODE BEGIN 4 */
        
        /* USER CODE END 4 */
        
        /**
          * @brief  This function is executed in case of error occurrence.
          * @retval None
          */
        void Error_Handler(void)
        {
          /* USER CODE BEGIN Error_Handler_Debug */
          /* User can add his own implementation to report the HAL error return state */
          __disable_irq();
          while (1)
          {
          }
          /* USER CODE END Error_Handler_Debug */
        }
        
        #ifdef  USE_FULL_ASSERT
        /**
          * @brief  Reports the name of the source file and the source line number
          *         where the assert_param error has occurred.
          * @param  file: pointer to the source file name
          * @param  line: assert_param error line source number
          * @retval None
          */
        void assert_failed(uint8_t *file, uint32_t line)
        {
          /* USER CODE BEGIN 6 */
          /* User can add his own implementation to report the file name and line number,
             ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
          /* USER CODE END 6 */
        }
        #endif /* USE_FULL_ASSERT */
        
        ```

        


##### 18.4.5 高级定时器互补输出比较模式实验

###### 18.4.5.1 互补输出，还带死区控制，什么意思

- ![image-20240913143438847](./images/stm32/image-20240913143438847.png)
- 由于元器件是有延迟性的，要加死区时间控制，避免元器件短路
  - ![image-20240913154645551](./images/stm32/image-20240913154645551.png)
  - 死区时间两通道都是低电平

###### 18.4.5.2 带死区控制的互补输出应用之H桥

- ![image-20240913143502722](./images/stm32/image-20240913143502722.png)
- 有效电平为高电平
  - OC1有效时，Q1和Q4通，电机正转
  - OC1N有效时，Q2和Q3通，电机反转

###### 18.4.5.3 捕获/比较通道的输出部分（通道1至3）

- ![image-20240913144202824](./images/stm32/image-20240913144202824.png)

###### 18.4.5.4 死区时间计算

- ![image-20240913145753440](./images/stm32/image-20240913145753440.png)
  - 上面举例，分频系数是4，带入公式就可以算出死区时间
  - tDTS：计数一次需要的时间

###### 18.4.5.5 刹车（断路）功能

- ![image-20240913145851969](./images/stm32/image-20240913145851969.png)
  - 刹车信号：外界手动输入一个有效电平，然后PWM波形进入空闲模式，空闲状态由相应寄存器控制
    - 将PE15接一个高电平就是外界手动输入一个高电平，低电平同理
  - TIM1、TIM8都有对应的刹车输入引脚
    - ![image-20240913162005720](./images/stm32/image-20240913162005720.png)
    - 这里使用IO重映射，用完全映像11
- ![image-20240913145950900](./images/stm32/image-20240913145950900.png)

###### 18.4.5.6 高级定时器互补输出带死区控制实验配置步骤

- ![image-20240913150509702](./images/stm32/image-20240913150509702.png)
- ![image-20240913150520927](./images/stm32/image-20240913150520927.png)
- ![image-20240913150557848](./images/stm32/image-20240913150557848.png)
- ![image-20240913150627963](./images/stm32/image-20240913150627963.png)

###### 18.4.5.7 编程实战：高级定时器互补输出带死区控制实验

- ![image-20240913150647127](./images/stm32/image-20240913150647127.png)

##### 18.4.6 高级定时器PWM输入模式实验

###### 18.4.6.1 PWM输入模式工作原理

- PWM输入模式作用：测量PWM周期、频率、占空比

- ![image-20240913174017581](./images/stm32/image-20240913174017581.png)

  

###### 18.4.6.2 PWM输入模式时序

- ![image-20240913174107768](./images/stm32/image-20240913174107768.png)

###### 18.4.6.3 高级定时器PWM输入模式实验配置步骤

- ![image-20240913175140137](./images/stm32/image-20240913175140137.png)
- ![image-20240913175149619](./images/stm32/image-20240913175149619.png)
- ![image-20240913175159222](./images/stm32/image-20240913175159222.png)

###### 18.4.6.4 编程实战：高级定时器PWM输入实验模式

- ![image-20240913181138882](./images/stm32/image-20240913181138882.png)

- tim.c

  - ```c
    
    ```

### 19. 电容触摸按键

#### 19.1 电容触摸按键原理介绍

- ![image-20240919095141231](./images/stm32/image-20240919095141231.png)
  - 电容结构：两个金属块中间间隔一层绝缘体
- ![image-20240919100058595](./images/stm32/image-20240919100058595.png)
  - 电容储能，就是充放电的过程，IO口根据阈值有低电平到高电平的跳变，可以用定时器捕获中断检测
- 手指没有按下按键时，电容充放电的过程
  - ![image-20240919100325293](./images/stm32/image-20240919100325293.png)
- ![image-20240919100931152](./images/stm32/image-20240919100931152.png)
  - 电容值与时间成正比关系，电容越大，充电时间越长
- ![image-20240919101628157](./images/stm32/image-20240919101628157.png)
  - 获取无触摸的充电时间Tcs，然后定时循环测量触摸充电时间T，T-Tcs的值大于一定阈值时，就认为有手指触摸

#### 19.2 检测电容触摸按键原理

- ![image-20240919102353821](./images/stm32/image-20240919102353821.png)
- ![image-20240919103243433](./images/stm32/image-20240919103243433.png)
  - TPAD电容按键没有直接连IO口，而是通过跳线帽来连接的，当然也可以用杜邦线接到其他有输入捕获功能的IO口

#### 19.3 编程实战

- ![image-20240919103656158](./images/stm32/image-20240919103656158.png)
- ![image-20240919104000733](./images/stm32/image-20240919104000733.png)
- ![image-20240919144921949](./images/stm32/image-20240919144921949.png)
- 功能
  - 用手指按压电容按键，LED1状态翻转
    - 函数需要
      - 电容按键扫描函数
        - 获取手指按压下的电容充电计数，取3次的最大值
        - 对比无按压+阈值，大于就代表按压有效
      - 电容按键初始化
        - 定时器初始化函数
          - 包括开启捕获
        - 无按压下，获取正常电容充电计数
          - 取10次，然后取中间6次的平均值
          - 电容先放电到0，再开始充电

##### 19.3.1 编程实战源码

###### 19.3.1.1 tpad.c

```c
#include "tpad.h"
#include "tim.h"
#include "usart.h"

volatile uint16_t g_tpad_default_val = 0;   //该值可能会被任何外部修改而改变
/* 电容按键初始化 */
void tpad_init(void)
{
    /* 变量定义 */
    uint16_t buf[10] = {0};  //存储捕获到的电容充电时间
    uint16_t temp = 0;
    /* 定时器初始化 */
    MX_TIM5_Init();
    /* 10次调用 */
    for(int i = 0;i < 10;i++)
    {
        buf[i] = tpad_get_val();
        HAL_Delay(10);
    }
    /* 对数组进行冒泡排序-升序 */
    for(int i = 0;i < 10-1;i++)
    {
        for(int j = 0;j < 10-1-i;j++)
        {
            if(buf[j] > buf[j+1])
            {
                temp = buf[j];
                buf[j] = buf[j+1];
                buf[j+1] = temp;
            }
        }
    }
    temp = 0;   
    /* 取中间6个数的平均值 */
    for(int i = 2;i < 8;i++)
    {
        temp += buf[i];
    }
    /* 获取平均值 */
    g_tpad_default_val = temp/6;
    if(g_tpad_default_val > TPAD_ARR_MAX_VAL/2)
    {
        printf("获取的电容充电值不正常！\r\n");
    }
    printf("g_tpad_default_val:%d\r\n",g_tpad_default_val);
    
}
/* 获取电容充电计数值 */
static uint16_t tpad_get_val(void)
{
    tpad_reset();   //电容充放电重置
    /* 等待一次捕获,捕获成功则跳出while循环 */
    while(__HAL_TIM_GET_FLAG(&htim5, TIM_FLAG_CC2) != GPIO_PIN_SET )
    {
        if(htim5.Instance->CNT > TPAD_ARR_MAX_VAL - 500 )   
        {
            return htim5.Instance->CNT;  //超时了，直接返回cnt的值
        }
    }
    return htim5.Instance->CCR2;    //返回捕获寄存器内的值，捕获到之后，cnt的值转移到CCR2中
}
/* 电容充放电初始化 */
static void tpad_reset(void)
{
    GPIO_InitTypeDef GPIO_InitStruct = {0};
    
    __HAL_RCC_GPIOA_CLK_ENABLE();
    /* 先配置为推挽输出，上拉模式 */
    GPIO_InitStruct.Pin = GPIO_PIN_1;
    GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
    GPIO_InitStruct.Pull = GPIO_PULLUP;
    GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_MEDIUM;
    HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
    /* 输出0，使电容放电 */
    HAL_GPIO_WritePin(GPIOA,GPIO_PIN_1,GPIO_PIN_RESET);
    
    HAL_Delay(5);
    
    htim5.Instance->SR = 0;    //标记清零
    htim5.Instance->CNT = 0;   //计数器清零
    
    /* 再配置为浮空输入 */
    GPIO_InitStruct.Pin = GPIO_PIN_1;
    GPIO_InitStruct.Mode = GPIO_MODE_INPUT;
    GPIO_InitStruct.Pull = GPIO_NOPULL;
    GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_MEDIUM;
    HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
}

/* 检测到按压时，连续捕获N次，取最大值 */
static uint16_t tpad_get_maxval(uint8_t n)
{
    uint16_t temp = 0;
    uint16_t maxval = 0;
    while(n--)
    {
        temp = tpad_get_val();
        if(temp > maxval)
        {
            maxval = temp;
        }
    }
    return maxval;
}

/* 
电容按键扫描函数 
mode:0不支持连按
     1支持连按
*/

uint8_t tpad_scan(uint8_t mode)
{
    static uint8_t keyen = 0;  //0开始检测，非0无效
    uint8_t res = 0;    //检测成功标志，1成功，0失败
    uint8_t sample = 3;   //默认采样三次
    uint16_t val = 0;   //采样值
    
    if(mode)   //支持连按
    {
        sample = 6;
        keyen = 0;
    }
    val = tpad_get_maxval(sample);
    if(val > g_tpad_default_val + TPAD_GATE_VAL )
    {
        if(keyen == 0)
        {
            res = 1;    //keyen==0,采样才有效
        }
        keyen = 3;        
    }
    if(keyen)
    {
        keyen--;  //成功采样一次之后，至少经过3次之后按键才有效
    }
    return res;
}


```



###### 19.3.1.2 tpad.h

```c
#ifndef __TPAD_H__
#define __TPAD_H__

#include "main.h"

/* 宏定义 */
#define TPAD_ARR_MAX_VAL 0xFFFF
#define TPAD_GATE_VAL 100

extern volatile uint16_t g_tpad_default_val;   //该值可能会被任何外部修改而改变

/* 函数声明 */
void tpad_init(void);   //按键初始化
uint8_t tpad_scan(uint8_t mode);    //按键扫描
static uint16_t tpad_get_val(void);   //获取一次捕获的值
static void tpad_reset(void);      //按键重置，即放电到0，再开始计数充电
static uint16_t tpad_get_maxval(uint8_t n);       //采样n次取最大值

#endif


```

###### 19.3.1.3 TIM5_CH2配置为输入捕获

- ```c
  /* USER CODE BEGIN Header */
  /**
    ******************************************************************************
    * @file    tim.c
    * @brief   This file provides code for the configuration
    *          of the TIM instances.
    ******************************************************************************
    * @attention
    *
    * Copyright (c) 2024 STMicroelectronics.
    * All rights reserved.
    *
    * This software is licensed under terms that can be found in the LICENSE file
    * in the root directory of this software component.
    * If no LICENSE file comes with this software, it is provided AS-IS.
    *
    ******************************************************************************
    */
  /* USER CODE END Header */
  /* Includes ------------------------------------------------------------------*/
  #include "tim.h"
  
  /* USER CODE BEGIN 0 */
  
  /* USER CODE END 0 */
  
  TIM_HandleTypeDef htim5;
  
  /* TIM5 init function */
  void MX_TIM5_Init(void)
  {
  
    /* USER CODE BEGIN TIM5_Init 0 */
  
    /* USER CODE END TIM5_Init 0 */
  
    TIM_ClockConfigTypeDef sClockSourceConfig = {0};
    TIM_MasterConfigTypeDef sMasterConfig = {0};
    TIM_IC_InitTypeDef sConfigIC = {0};
  
    /* USER CODE BEGIN TIM5_Init 1 */
  
    /* USER CODE END TIM5_Init 1 */
    htim5.Instance = TIM5;
    htim5.Init.Prescaler = 5;
    htim5.Init.CounterMode = TIM_COUNTERMODE_UP;
    htim5.Init.Period = 65535;
    htim5.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
    htim5.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
    if (HAL_TIM_Base_Init(&htim5) != HAL_OK)
    {
      Error_Handler();
    }
    sClockSourceConfig.ClockSource = TIM_CLOCKSOURCE_INTERNAL;
    if (HAL_TIM_ConfigClockSource(&htim5, &sClockSourceConfig) != HAL_OK)
    {
      Error_Handler();
    }
    if (HAL_TIM_IC_Init(&htim5) != HAL_OK)
    {
      Error_Handler();
    }
    sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
    sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
    if (HAL_TIMEx_MasterConfigSynchronization(&htim5, &sMasterConfig) != HAL_OK)
    {
      Error_Handler();
    }
    sConfigIC.ICPolarity = TIM_INPUTCHANNELPOLARITY_RISING;
    sConfigIC.ICSelection = TIM_ICSELECTION_DIRECTTI;
    sConfigIC.ICPrescaler = TIM_ICPSC_DIV1;
    sConfigIC.ICFilter = 0;
    if (HAL_TIM_IC_ConfigChannel(&htim5, &sConfigIC, TIM_CHANNEL_2) != HAL_OK)
    {
      Error_Handler();
    }
    /* USER CODE BEGIN TIM5_Init 2 */
      __HAL_TIM_ENABLE(&htim5);
      HAL_TIM_IC_Start(&htim5, TIM_CHANNEL_2);
    /* USER CODE END TIM5_Init 2 */
  
  }
  
  void HAL_TIM_Base_MspInit(TIM_HandleTypeDef* tim_baseHandle)
  {
  
    GPIO_InitTypeDef GPIO_InitStruct = {0};
    if(tim_baseHandle->Instance==TIM5)
    {
    /* USER CODE BEGIN TIM5_MspInit 0 */
  
    /* USER CODE END TIM5_MspInit 0 */
      /* TIM5 clock enable */
      __HAL_RCC_TIM5_CLK_ENABLE();
  
      __HAL_RCC_GPIOA_CLK_ENABLE();
      /**TIM5 GPIO Configuration
      PA1     ------> TIM5_CH2
      */
      GPIO_InitStruct.Pin = GPIO_PIN_1;
      GPIO_InitStruct.Mode = GPIO_MODE_INPUT;
      GPIO_InitStruct.Pull = GPIO_NOPULL;
      HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
  
      /* TIM5 interrupt Init */
      HAL_NVIC_SetPriority(TIM5_IRQn, 2, 3);
      HAL_NVIC_EnableIRQ(TIM5_IRQn);
    /* USER CODE BEGIN TIM5_MspInit 1 */
  
    /* USER CODE END TIM5_MspInit 1 */
    }
  }
  
  void HAL_TIM_Base_MspDeInit(TIM_HandleTypeDef* tim_baseHandle)
  {
  
    if(tim_baseHandle->Instance==TIM5)
    {
    /* USER CODE BEGIN TIM5_MspDeInit 0 */
  
    /* USER CODE END TIM5_MspDeInit 0 */
      /* Peripheral clock disable */
      __HAL_RCC_TIM5_CLK_DISABLE();
  
      /**TIM5 GPIO Configuration
      PA1     ------> TIM5_CH2
      */
      HAL_GPIO_DeInit(GPIOA, GPIO_PIN_1);
  
      /* TIM5 interrupt Deinit */
      HAL_NVIC_DisableIRQ(TIM5_IRQn);
    /* USER CODE BEGIN TIM5_MspDeInit 1 */
  
    /* USER CODE END TIM5_MspDeInit 1 */
    }
  }
  
  /* USER CODE BEGIN 1 */
  
  /* USER CODE END 1 */
  
  ```

  

#### 19.4 课堂总结

- 电容：可以容纳电荷的器件，两个金属块中间隔一层绝缘体就可以构成一个最简单的电容
- 电容触摸按键：等效为一个电容
  - 优点
    - 无机械设置，使用寿命长
    - 非接触式感应，面板不用开孔，简洁美观
    - 防水性好
- 电容储能就是充放电的过程，利用这个特性，可以得到充电时间
  - ![image-20240919175259799](./images/stm32/image-20240919175259799.png)
  - 电容按键本身等效为一个电容，充电时间为Tcs
  - 当有手指按压按键时，等同于又并入一个电容，电容值加大，充电时间就增长到Tcx
  - Tcx-Tcs作为一个阈值，也是按压电容按键的一个门槛，即按压电容按键时，电容充电时间计数值CNTx > 阈值 + Tcs
- 电容按键不直接连IO，而是通过跳线帽接到PA1上
- IO口使用输入捕获，就会将TIM5_CH1重映射到IO口
- 使用电容按时，先通过外接IO输入0，使电容值放电到0，然后IO再设置为浮空输入，等待捕获
- 采样捕获，多次采样取最大值，这里不连按是采样3次，连按是采样6次

### 20. OLED

#### 20.1 OLED显示屏介绍

- ![image-20240920093324575](./images/stm32/image-20240920093324575.png)

  - OLED就是由很多个发光二极管有序排列组成的，发光二极管两端给正确的电压就会亮

- ![image-20240920093509733](./images/stm32/image-20240920093509733.png)

  - 分辨率128*64，可以理解为长有128个小灯泡，宽有64个小灯泡，通过控制这些灯泡的亮灭来显示字符

- 8位8080并口说明

  ![image-20240920093828843](./images/stm32/image-20240920093828843.png)

- ![image-20240920093848078](./images/stm32/image-20240920093848078.png)

- 

#### 20.2 OLED驱动原理

- ![image-20240920094758427](./images/stm32/image-20240920094758427.png)

  - OLED驱动核心就是驱动OLED的驱动芯片SSD1306

    - 单片机向SSD1306写数据，实现OLED屏幕显示

  - SSD1306驱动工作时序

    - ![image-20240920095023716](./images/stm32/image-20240920095023716.png)

    - ![image-20240920095156040](./images/stm32/image-20240920095156040.png)
      - WR拉低，写数据，数据写完之后，将WR拉高，在WR上升沿数据写入SSD1306

#### 20.3 OLED驱动芯片简介

- ![image-20240920162724877](./images/stm32/image-20240920162724877.png)

- 什么是GRAM

  - ![G](./images/stm32/image-20240920163805916.png)
  - GRAM：图形显示数据RAM，是一个位映射静态RAM，保存要显示的位模式

- 什么是页地址

  - ![image-20240920164039879](./images/stm32/image-20240920164039879.png)

- ![image-20240920164142290](./images/stm32/image-20240920164142290.png)

  - 如何解决点覆盖问题
    - 单片机内新建一个GRAM二维数组，每次修改时修改单片机内的GRAM，修改完之后，再将GRAM整体写入OLED的GRAM，可以节省内存

- ![image-20240920165329704](./images/stm32/image-20240920165329704.png)

  - 上述代码里，for(n=0;n<128;n++)循环里就是每一页内，循环写入128个字节

- ![image-20240920165647612](./images/stm32/image-20240920165647612.png)

  - ```c
    /*
    OLED_GRAM[x][y/8] 对应一个字节;
    该字节的八位，按一列排，顶部是低位，最下面是高位;
    每一页有128个列字节;
    */
    OLED_GRAM[x][y/8] |= 1<<y%8  //假设这里y为4，x为0，那这一行就是设置第0页内的第0列的一个字节为4，就是该列字节的第3位置1
    
    ```

    

- ![image-20240920165939445](./images/stm32/image-20240920165939445.png)

  - GRAM实际上是一个二维数组，存放图形显示的位模式
    - 64行分为8页，每一页就是128*8个点，那么对应每一页的每一列就是一个字节，通过设置每一列字节对应的位置1，就可以设置对应的点亮

#### 20.4 字符显示原理

- ![image-20240920174840631](./images/stm32/image-20240920174840631.png)
- ![image-20240920174905582](./images/stm32/image-20240920174905582.png)
- ![image-20240920175239671](./images/stm32/image-20240920175239671.png)
- ![image-20240920175333431](./images/stm32/image-20240920175333431.png)
- ![image-20240920175343428](./images/stm32/image-20240920175343428.png)
- ![image-20240920175400808](./images/stm32/image-20240920175400808.png)
  - 将95个显示字符都进行编码换算，做成字库，之后用起来方便

#### 20.5 OLED基本驱动步骤

- ![image-20240920175606450](./images/stm32/image-20240920175606450.png)

#### 20.6 编程实战

- ![image-20240920175620934](./images/stm32/image-20240920175620934.png)

- oled.c

  -  

    ```c
    /**
     ****************************************************************************************************
     * @file        oled.c
     * @author      正点原子团队(ALIENTEK)
     * @version     V1.0
     * @date        2020-04-22
     * @brief       OLED 驱动代码
     * @license     Copyright (c) 2020-2032, 广州市星翼电子科技有限公司
     ****************************************************************************************************
     * @attention
     *
     * 实验平台:正点原子 STM32F103开发板
     * 在线视频:www.yuanzige.com
     * 技术论坛:www.openedv.com
     * 公司网址:www.alientek.com
     * 购买地址:openedv.taobao.com
     *
     * 修改说明
     * V1.0 20200421
     * 第一次发布
     *
     ****************************************************************************************************
     */
    
    #include "stdlib.h"
    #include "./BSP/OLED/oled.h"
    #include "./BSP/OLED/oledfont.h"
    #include "./SYSTEM/delay/delay.h"
    
    static uint8_t g_oled_gram[128][8];
    
    void oled_refresh_gram(void)
    {
        uint8_t i,n;
    
        for (i = 0; i < 8; i++)
        {
            oled_wr_byte(0xb0 + i, OLED_CMD) ;  /* 设置页地址（0~7）*/
            oled_wr_byte(0x00, OLED_CMD) ;      /* 设置显示位置-列低地址 */ 
            oled_wr_byte(0x10, OLED_CMD) ;      /* 设置显示位置-列高地址 */
            
            for (n = 0; n < 128; n++)
            {
                oled_wr_byte( g_oled_gram[ n ][ i ], OLED_DATA) ;
            }
        }
    }
    
    void oled_draw_point(uint8_t  x, uint8_t  y, uint8_t  dot) 
    {
        uint8_t pos, bx, temp = 0;
        
        if (x > 127 || y > 63)  return;    /* 超出范围了 */
        
        pos = y / 8;    /*  页地址 */
        bx = y % 8;     /*  计算y在对应字节里面的位置 */
        temp = 1 << bx; /*  转换后y对应的bit位置 */
    
        if ( dot )  /*  画实心点 */
            g_oled_gram[ x ][ pos ] |= temp;
        else
            g_oled_gram[ x ][ pos ] &= ~temp;
    }
    
    /* 16*16大小，字符A的点阵数据数组：*/
    uint8_t oled_ascii_1608[]=
    {
      0x00,0x04,0x00,0x3C,0x03,0xC4,0x1C,0x40,
      0x07,0x40,0x00,0xE4,0x00,0x1C,0x00,0x04
    } ;
    
    void oled_show_char_test(uint8_t  x, uint8_t  y, uint8_t mode)
    {
        uint8_t temp, t1, t;
        uint8_t y0 = y;                 /* 保存y的初值 */
    
        for(t = 0; t < 16; t++)         /* 总共16个字节，要遍历一遍 */
        {
            temp = oled_ascii_1608[t];  /* 依次获取点阵数据 */
    
            for(t1 = 0; t1 < 8; t1++)
            {
                if(temp & 0X80)     /* 这个点有效，需要画出来 */
                    oled_draw_point(x, y, mode);
                else                /* 这个点无效，不需要画出来 */
                    oled_draw_point(x, y, !mode);
    
                temp <<= 1;         /* 低位数据往高位移位，最高位数据直接丢弃 */
                y++;                /* y坐标自增 */
    
                if((y - y0) == 16)  /* 显示完一列了 */
                {
                    y = y0;         /* y坐标复位 */
                    x++;            /* x坐标递增 */
                    break;          /* 跳出 for循环 */
                }
            }
        }
    }
    
    
    /**
     * @brief       初始化OLED(SSD1306)
     * @param       无
     * @retval      无
     */
    void oled_init(void)
    {
        GPIO_InitTypeDef gpio_init_struct;
        
        __HAL_RCC_GPIOC_CLK_ENABLE();     /* 使能PORTC时钟 */
        __HAL_RCC_GPIOD_CLK_ENABLE();     /* 使能PORTD时钟 */
        __HAL_RCC_GPIOG_CLK_ENABLE();     /* 使能PORTG时钟 */
        
        /* PC0 ~ 7 设置 */
        gpio_init_struct.Pin = GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3|GPIO_PIN_4|GPIO_PIN_5|GPIO_PIN_6|GPIO_PIN_7;                
        gpio_init_struct.Mode = GPIO_MODE_OUTPUT_PP;            /* 推挽输出 */
        gpio_init_struct.Pull = GPIO_PULLUP;                    /* 上拉 */
        gpio_init_struct.Speed = GPIO_SPEED_FREQ_MEDIUM;        /* 中速 */
        HAL_GPIO_Init(GPIOC, &gpio_init_struct);                /* PC0 ~ 7 设置 */
    
        gpio_init_struct.Pin = GPIO_PIN_3|GPIO_PIN_6;           /* PD3, PD6 设置 */
        gpio_init_struct.Mode = GPIO_MODE_OUTPUT_PP;            /* 推挽输出 */
        gpio_init_struct.Pull = GPIO_PULLUP;                    /* 上拉 */
        gpio_init_struct.Speed = GPIO_SPEED_FREQ_MEDIUM;        /* 中速 */
        HAL_GPIO_Init(GPIOD, &gpio_init_struct);                /* PD3, PD6 设置 */
        
        gpio_init_struct.Pin = GPIO_PIN_13|GPIO_PIN_14|GPIO_PIN_15;
        gpio_init_struct.Mode = GPIO_MODE_OUTPUT_PP;            /* 推挽输出 */
        gpio_init_struct.Pull = GPIO_PULLUP;                    /* 上拉 */
        gpio_init_struct.Speed = GPIO_SPEED_FREQ_MEDIUM;        /* 中速 */
        HAL_GPIO_Init(GPIOG, &gpio_init_struct);                /* WR/RD/RST引脚模式设置 */
    
        OLED_WR(1);
        OLED_RD(1);
    
        OLED_CS(1);
        OLED_RS(1);
        
        /* 复位时序 */
        OLED_RST(0);
        delay_ms(100);
        OLED_RST(1);
    
        oled_wr_byte(0xAE, OLED_CMD);   /* 关闭显示 */
        oled_wr_byte(0xD5, OLED_CMD);   /* 设置时钟分频因子,震荡频率 */
        oled_wr_byte(80, OLED_CMD);     /* [3:0],分频因子;[7:4],震荡频率 */
        oled_wr_byte(0xA8, OLED_CMD);   /* 设置驱动路数 */
        oled_wr_byte(0X3F, OLED_CMD);   /* 默认0X3F(1/64) */
        oled_wr_byte(0xD3, OLED_CMD);   /* 设置显示偏移 */
        oled_wr_byte(0X00, OLED_CMD);   /* 默认为0 */
    
        oled_wr_byte(0x40, OLED_CMD);   /* 设置显示开始行 [5:0],行数. */
    
        oled_wr_byte(0x8D, OLED_CMD);   /* 电荷泵设置 */
        oled_wr_byte(0x14, OLED_CMD);   /* bit2，开启/关闭 */
        oled_wr_byte(0x20, OLED_CMD);   /* 设置内存地址模式 */
        oled_wr_byte(0x02, OLED_CMD);   /* [1:0],00，列地址模式;01，行地址模式;10,页地址模式;默认10; */
        oled_wr_byte(0xA1, OLED_CMD);   /* 段重定义设置,bit0:0,0->0;1,0->127; */
        oled_wr_byte(0xC8, OLED_CMD);   /* 设置COM扫描方向;bit3:0,普通模式;1,重定义模式 COM[N-1]->COM0;N:驱动路数 */
        oled_wr_byte(0xDA, OLED_CMD);   /* 设置COM硬件引脚配置 */
        oled_wr_byte(0x12, OLED_CMD);   /* [5:4]配置 */
    
        oled_wr_byte(0x81, OLED_CMD);   /* 对比度设置 */
        oled_wr_byte(0xEF, OLED_CMD);   /* 1~255;默认0X7F (亮度设置,越大越亮) */
        oled_wr_byte(0xD9, OLED_CMD);   /* 设置预充电周期 */
        oled_wr_byte(0xf1, OLED_CMD);   /* [3:0],PHASE 1;[7:4],PHASE 2; */
        oled_wr_byte(0xDB, OLED_CMD);   /* 设置VCOMH 电压倍率 */
        oled_wr_byte(0x30, OLED_CMD);   /* [6:4] 000,0.65*vcc;001,0.77*vcc;011,0.83*vcc; */
    
        oled_wr_byte(0xA4, OLED_CMD);   /* 全局显示开启;bit0:1,开启;0,关闭;(白屏/黑屏) */
        oled_wr_byte(0xA6, OLED_CMD);   /* 设置显示方式;bit0:1,反相显示;0,正常显示 */
        oled_wr_byte(0xAF, OLED_CMD);   /* 开启显示 */
        
    }
    
    void oled_data_out(uint8_t data)
    {
        GPIOC->ODR = (GPIOC->ODR & 0XFF00) | (data & 0X00FF);
    }
    
    
    static void oled_wr_byte(uint8_t data, uint8_t cmd)
    {
        OLED_RS (cmd);  /* 数据类型，由传参决定 */
        OLED_CS ( 0 );  /* 拉低片选线，选中SSD1306 */
        OLED_WR ( 0 );  /* 拉低WR线，准备数据 */
        oled_data_out(data); /* WR低电平期间，准备数据 */
        OLED_WR ( 1 );  /* 在WR上升沿，数据发出 */
        OLED_CS ( 1 );  /* 取消片选 */
        OLED_RS ( 1 );  /* 释放RS线，恢复默认 */
    }
    
    void oled_draw_point_test(uint8_t x, uint8_t y)
    {
        /* 页地址模式 */
        uint8_t page_num = y / 8;
        
        /* 1、发送页地址 */
        oled_wr_byte(0xB0 | page_num, OLED_CMD);
        
        /* 2、发送列地址 */
        oled_wr_byte((x & 0x0F) | 0x00, OLED_CMD);      /* 列地址低四位 */
        oled_wr_byte((x & 0xF0) >> 4 | 0x10, OLED_CMD); /* 列地址高四位 */
        
        /* 3、发送1字节数据 */
        oled_wr_byte(1 << (y % 8), OLED_DATA);
    }
    
    void oled_clear(void)
    {
        uint8_t i, n;
    
        for (i = 0; i < 8; i++)
        {
            oled_wr_byte (0xb0 + i, OLED_CMD); /* 设置页地址（0~7） */
            oled_wr_byte (0x00, OLED_CMD);     /* 设置显示位置—列低地址 */
            oled_wr_byte (0x10, OLED_CMD);     /* 设置显示位置—列高地址 */
    
            for (n = 0; n < 128; n++)
            {
                oled_wr_byte(0x00, OLED_DATA);
            }
        }
    }
    
    
    ```

- oled.h

  - ```c
    /**
     ****************************************************************************************************
     * @file        oled.h
     * @author      正点原子团队(ALIENTEK)
     * @version     V1.0
     * @date        2020-04-21
     * @brief       OLED 驱动代码
     * @license     Copyright (c) 2020-2032, 广州市星翼电子科技有限公司
     ****************************************************************************************************
     * @attention
     *
     * 实验平台:正点原子 STM32F103开发板
     * 在线视频:www.yuanzige.com
     * 技术论坛:www.openedv.com
     * 公司网址:www.alientek.com
     * 购买地址:openedv.taobao.com
     *
     * 修改说明
     * V1.0 20200421
     * 第一次发布
     *
     ****************************************************************************************************
     */
     
    #ifndef __OLED_H
    #define __OLED_H
    
    #include "stdlib.h" 
    #include "./SYSTEM/sys/sys.h"
    
    
    /******************************************************************************************/
    /* OLED 8080 模式引脚 定义 */
    
    /* 片选脚 */
    #define OLED_CS_PORT                GPIOD
    #define OLED_CS_PIN                 GPIO_PIN_6
    #define OLED_CS_CLK_ENABLE()        do{ __HAL_RCC_GPIOD_CLK_ENABLE(); }while(0)   /* PD口时钟使能 */
    
    /* 数据类型脚 命令/数据*/
    #define OLED_RS_PORT                GPIOD
    #define OLED_RS_PIN                 GPIO_PIN_3
    #define OLED_RS_CLK_ENABLE()        do{ __HAL_RCC_GPIOD_CLK_ENABLE(); }while(0)   /* PD口时钟使能 */
    
    /* 向OLED读取数据脚 */
    #define OLED_RD_PORT                GPIOG
    #define OLED_RD_PIN                 GPIO_PIN_13
    #define OLED_RD_CLK_ENABLE()        do{ __HAL_RCC_GPIOG_CLK_ENABLE(); }while(0)   /* PG口时钟使能 */
    
    /* 向OLED写入数据脚 */
    #define OLED_WR_PORT                GPIOG
    #define OLED_WR_PIN                 GPIO_PIN_14
    #define OLED_WR_CLK_ENABLE()        do{ __HAL_RCC_GPIOG_CLK_ENABLE(); }while(0)   /* PG口时钟使能 */
    
    /* 复位脚 */
    #define OLED_RST_PORT               GPIOG
    #define OLED_RST_PIN                GPIO_PIN_15
    #define OLED_RST_CLK_ENABLE()       do{ __HAL_RCC_GPIOG_CLK_ENABLE(); }while(0)   /* PG口时钟使能 */
    
    /* 数据脚 */
    #define OLED_DATA_PORT               GPIOC
    #define OLED_DATA_PIN                GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3|GPIO_PIN_4|GPIO_PIN_5|GPIO_PIN_6|GPIO_PIN_7
    #define OLED_DATA_CLK_ENABLE()       do{ __HAL_RCC_GPIOC_CLK_ENABLE(); }while(0)   /* PC口时钟使能 */
    /******************************************************************************************/
    
    /* OLED 8080模式相关端口控制函数 定义 */
    #define OLED_RST(x)     do{ x ? \
                                      HAL_GPIO_WritePin(OLED_RST_PORT, OLED_RST_PIN, GPIO_PIN_SET) : \
                                      HAL_GPIO_WritePin(OLED_RST_PORT, OLED_RST_PIN, GPIO_PIN_RESET); \
                            }while(0)       /* 设置RST引脚 */
    
    #define OLED_CS(x)      do{ x ? \
                                      HAL_GPIO_WritePin(OLED_CS_PORT, OLED_CS_PIN, GPIO_PIN_SET) : \
                                      HAL_GPIO_WritePin(OLED_CS_PORT, OLED_CS_PIN, GPIO_PIN_RESET); \
                            }while(0)       /* 设置CS引脚 */
    #define OLED_RS(x)      do{ x ? \
                                      HAL_GPIO_WritePin(OLED_RS_PORT, OLED_RS_PIN, GPIO_PIN_SET) : \
                                      HAL_GPIO_WritePin(OLED_RS_PORT, OLED_RS_PIN, GPIO_PIN_RESET); \
                            }while(0)       /* 设置RS引脚 */
                                  
    #define OLED_WR(x)      do{ x ? \
                                      HAL_GPIO_WritePin(OLED_WR_PORT, OLED_WR_PIN, GPIO_PIN_SET) :  \
                                      HAL_GPIO_WritePin(OLED_WR_PORT, OLED_WR_PIN, GPIO_PIN_RESET); \
                            } while (0)     /* 设置WR引脚 */
    
    #define OLED_RD(x)      do{ x ? \
                                      HAL_GPIO_WritePin(OLED_RD_PORT, OLED_RD_PIN, GPIO_PIN_SET) : \
                                      HAL_GPIO_WritePin(OLED_RD_PORT, OLED_RD_PIN, GPIO_PIN_RESET); \
                            }while(0)       /* 设置RD引脚 */
    
    /* 命令/数据 定义 */
    #define OLED_CMD        0       /* 写命令 */
    #define OLED_DATA       1       /* 写数据 */
    
    /******************************************************************************************/
        
    static void oled_wr_byte(uint8_t data, uint8_t cmd);    /* 写一个字节到OLED */
    void oled_init(void);           /* OLED初始化 */
    void oled_refresh_gram(void);
    void oled_draw_point(uint8_t  x, uint8_t  y, uint8_t  dot);
    void oled_show_char_test(uint8_t  x, uint8_t  y, uint8_t mode);
    void oled_data_out(uint8_t data);
    void oled_draw_point_test(uint8_t x, uint8_t y);
    void oled_clear(void);                        
    
    #endif
    
    ```

    

#### 20.7 课堂总结

### 21 MPU内存保护

#### 21.1 内存保护单元(MPU)介绍

- ![image-20240923152547590](./images/stm32/image-20240923152547590.png)
  - MPU就是内核管理员，可以设置内核内不同区域的存储器访问权限，设置存储器的属性，提高STM32芯片性能
- ![image-20240923154558366](./images/stm32/image-20240923154558366.png)
- ![image-20240923154634613](./images/stm32/image-20240923154634613.png)
- ![image-20240923154717944](./images/stm32/image-20240923154717944.png)
- ![image-20240923154729235](./images/stm32/image-20240923154729235.png)

- ![image-20240923154801096](./images/stm32/image-20240923154801096.png)
- ![image-20240923154919678](./images/stm32/image-20240923154919678.png)
  - C：对应使用缓存
  - B：对应是否缓冲

#### 21.2 Cache简介

- ![image-20240923155204057](./images/stm32/image-20240923155204057.png)

#### 21.3 MPU相关寄存器介绍

#### 21.4 MPU相关HAL库驱动介绍

#### 21.5 MPU基本配置步骤

#### 21.6 编程实战

#### 21.7 课堂总结

### 22. LCD实验

#### 22.1 显示器分类

- ![image-20240923163045696](./images/stm32/image-20240923163045696.png)

##### 22.1.1 LCD与TFTLCD的区别

TFT LCD（**薄膜晶体管液晶显示器**）和传统LCD（液晶显示器）之间有一些关键的区别，主要体现在以下几个方面：

1. **显示技术**：
   - **LCD**：传统的LCD通常采用复合色彩过滤器和较简单的驱动方式，显示效果较为基础。
   - **TFT LCD**：TFT LCD使用薄膜晶体管技术，每个像素都有独立的晶体管控制，因此能够提供更好的显示质量和更快的响应速度。
2. **显示质量**：
   - **TFT LCD**：具有更高的分辨率、更广的视角和更准确的颜色再现能力。其对比度和亮度也通常更高。
   - **LCD**：显示效果相对较弱，特别是在颜色饱和度和视角方面。
3. **刷新率**：
   - **TFT LCD**：能够实现更高的刷新率，适合于动态画面显示，如游戏和视频播放。
   - **LCD**：由于结构的限制，刷新率较低，可能会出现拖影现象。
4. **响应时间**：
   - **TFT LCD**：相对较快，能有效减少模糊和拖影。
   - **LCD**：响应时间较长，表现较差。
5. **价格**：
   - **TFT LCD**：由于其更复杂的制造工艺，成本相对较高，价格也更贵。
   - **LCD**：因技术较简单，成本较低，价格也更便宜。

总之，TFT LCD在性能和显示效果上优于传统LCD，适合对视觉质量有较高要求的应用场景。

#### 22.2 LCD简介

- ![image-20240923163200495](./images/stm32/image-20240923163200495.png)
- ![image-20240923163215347](./images/stm32/image-20240923163215347.png)
  - 驱动IC：ILI9341
- ![image-20240923163232619](./images/stm32/image-20240923163232619.png)
- ![image-20240923163306854](./images/stm32/image-20240923163306854.png)
- ![image-20240923163322213](./images/stm32/image-20240923163322213.png)
- 三基色原理--RGB
  - ![image-20240923163536226](./images/stm32/image-20240923163536226.png)

#### 22.3 LCD驱动原理

- 单片机驱动LCD实际就是驱动LCD的驱动IC
  - ![image-20240923164828070](./images/stm32/image-20240923164828070.png)
  - ![image-20240923164841805](./images/stm32/image-20240923164841805.png)
  - ![image-20240923164902634](./images/stm32/image-20240923164902634.png)
  - ![image-20240923164918888](./images/stm32/image-20240923164918888.png)
  - ![image-20240923164927402](./images/stm32/image-20240923164927402.png)
  - ![image-20240923165009292](./images/stm32/image-20240923165009292.png)

#### 22.4 LCD驱动芯片简介

- ![image-20240923172137980](./images/stm32/image-20240923172137980.png)

- ![image-20240923172206915](./images/stm32/image-20240923172206915.png)

- ![image-20240923172252528](./images/stm32/image-20240923172252528.png)

  - ![image-20240923231721847](./images/stm32/image-20240923231721847.png)
    - ![image-20240923234717853](./images/stm32/image-20240923234717853.png)
    - 这里参数1是0，即BGR是0，表示是按BGR顺序；RGB顺序是置1(代码中理解，但7789数据手册刚好相反)
  - ![image-20240923172314079](./images/stm32/image-20240923172314079.png)

- X坐标设置

  - ![image-20240923172332445](./images/stm32/image-20240923172332445.png)

    - 代码设置

      - ```c
        /* 设置坐标 */
        void lcd_set_cursor(uint16_t x, uint16_t y)
        {
            lcd_wr_regno(lcddev.setxcmd);  //设置x坐标命令
            /* 发送一个x坐标的参数 */
            lcd_wr_data(x >> 8);    //发送x的高8位
            lcd_wr_data(x & 0xFF);   //发送x的低8位
            lcd_wr_regno(lcddev.setycmd);   //设置y坐标命令
            /* 发送一个y坐标的参数 */
            lcd_wr_data(y >> 8);
            lcd_wr_data(y & 0xFF);
        }
        ```

        

- Y坐标设置

  - ![image-20240923172419438](./images/stm32/image-20240923172419438.png)

- ![image-20240923172523541](./images/stm32/image-20240923172523541.png)

  - GRAM指令：控制像素点自增方向，以及像素点的颜色设置
  - 

- ![image-20240923173031162](./images/stm32/image-20240923173031162.png)

- ![image-20240923173431409](./images/stm32/image-20240923173431409.png)

#### 22.5 LCD基本驱动步骤

- ![image-20240923174258434](./images/stm32/image-20240923174258434.png)
- ![image-20240923174309266](./images/stm32/image-20240923174309266.png)
- ![image-20240923174320228](./images/stm32/image-20240923174320228.png)
- 

#### 22.6 编程实战1

- ![image-20240923174347894](./images/stm32/image-20240923174347894.png)

#### 22.7 FSMC介绍

##### 22.7.1 FSMC简介

- ![image-20240923235123659](./images/stm32/image-20240923235123659.png)

##### 22.7.2 FSMC框图介绍

- ![image-20240925094327667](./images/stm32/image-20240925094327667.png)
- ![image-20240925094355557](./images/stm32/image-20240925094355557.png)
- ![image-20240925094406336](./images/stm32/image-20240925094406336.png)
- 

##### 22.7.3 FSMC时序介绍

- ![image-20240925095413890](./images/stm32/image-20240925095413890.png)
  - FSMC控制单元的NOR/PSRAM控制其可以产生5种异步时序
  - ![image-20240925100122918](./images/stm32/image-20240925100122918.png)
    - 模式A时序跟8080时序可以对得上
- 重点时序--读写ID/FM时间
  - ![image-20240925100409170](./images/stm32/image-20240925100409170.png)

##### 22.7.4 FSMC地址映射

- ![image-20240925101119433](./images/stm32/image-20240925101119433.png)
- 地址偏移
  - ![image-20240925101309169](./images/stm32/image-20240925101309169.png)
- ![image-20240925101931007](./images/stm32/image-20240925101931007.png)

##### 22.7.5 FSMC相关寄存器介绍

- ![image-20241017165409940](./images/stm32/image-20241017165409940.png)
- ![image-20241017165426497](./images/stm32/image-20241017165426497.png)
- ![image-20241017165443357](./images/stm32/image-20241017165443357.png)
- ![image-20241017165501912](./images/stm32/image-20241017165501912.png)
- ![image-20241017165513869](./images/stm32/image-20241017165513869.png)
- 

##### 22.7.6 FSMC相关HAL库函数介绍

- ![image-20241017165530460](./images/stm32/image-20241017165530460.png)
- ![image-20241017165541511](./images/stm32/image-20241017165541511.png)
- ![image-20241017165555141](./images/stm32/image-20241017165555141.png)
- ![image-20241017165606532](./images/stm32/image-20241017165606532.png)
- ![image-20241017165617368](./images/stm32/image-20241017165617368.png)
- 

#### 22.8 编程实战2

- <img src="./images/stm32/image-20241017165654883.png" alt="image-20241017165654883" style="zoom:80%;" />
  
  - 直接用课堂例程中的 lcd.c lcd.h lcd_ex.c文件，主打一个会用
  
- CobeMX配置

  - ![image-20241101124809220](./images/stm32/image-20241101124809220.png)

  - RCC、LED、USART1配置完毕之后，生成代码

  - 将HAL库的TFT LCD例程代码中的LCD文件夹内的文件分别移植到src和inc中

  - lcd.c--注意事项

    - 头文件

    - 延时都替换成HAL_Delay()

    - ```c
      #include "lcd.h"
      #include "lcdfont.h"
      #include "main.h"
      #include "usart.h"
      
      /* lcd_ex.c存放各个LCD驱动IC的寄存器初始化部分代码,以简化lcd.c,该.c文件
       * 不直接加入到工程里面,只有lcd.c会用到,所以通过include的形式添加.(不要在
       * 其他文件再包含该.c文件!!否则会报错!)
       */
      #include "lcd_ex.c"
      ```

  - lcd.h里只需要包含 main.h头文件

  - fsmc.c内，屏蔽HAL_SRAM_MspInit(SRAM_HandleTypeDef* sramHandle)函数

  - main.c内，包含lcd.h

    - ```c
      int main(void)
      {
        /* USER CODE BEGIN 1 */
      
        /* USER CODE END 1 */
      
        /* MCU Configuration--------------------------------------------------------*/
      
        /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
        HAL_Init();
      
        /* USER CODE BEGIN Init */
      
        /* USER CODE END Init */
      
        /* Configure the system clock */
        SystemClock_Config();
      
        /* USER CODE BEGIN SysInit */
      
        /* USER CODE END SysInit */
      
        /* Initialize all configured peripherals */
        MX_GPIO_Init();
        MX_FSMC_Init();
        MX_USART1_UART_Init();
        /* USER CODE BEGIN 2 */
        lcd_init();
        lcd_draw_point(5, 5, BLACK);
      
        /* USER CODE END 2 */
      
        /* Infinite loop */
        /* USER CODE BEGIN WHILE */
        while (1)
        {
            HAL_Delay(500);
            HAL_GPIO_TogglePin(LED0_GPIO_Port,LED0_Pin);
          /* USER CODE END WHILE */
      
          /* USER CODE BEGIN 3 */
        }
        /* USER CODE END 3 */
      }
      
      ```

      - 特别注意：自己编写的函数要点击使用微库
        -  ![image-20241101170627121](./images/stm32/image-20241101170627121.png)




#### 22.9 课堂总结

### 23. RTC

#### 23.1 RTC简介

- ![image-20240925151410560](./images/stm32/image-20240925151410560.png)
  - RTC本质就是一个计数器，板子掉电之后仍会运行
- ![image-20240925151532258](./images/stm32/image-20240925151532258.png)
  - 常用RTC方案
    - 片上RTC
    - 独立RTC

#### 23.2 STM32 RTC框图介绍

- ![image-20240925152443158](./images/stm32/image-20240925152443158.png)

#### 23.3 RTC相关寄存器介绍

- ![image-20240925152803206](./images/stm32/image-20240925152803206.png)
- ![image-20240925153714149](./images/stm32/image-20240925153714149.png)
- ![image-20240925153732223](./images/stm32/image-20240925153732223.png)
- ![image-20240925153750842](./images/stm32/image-20240925153750842.png)
- ![image-20240925153809774](./images/stm32/image-20240925153809774.png)
- ![image-20240925153831242](./images/stm32/image-20240925153831242.png)
- ![image-20240925153958297](./images/stm32/image-20240925153958297.png)
  - LSE是32.768KHZ，分频32768次就是1HZ，等于1s计数次
- ![image-20240925154024727](./images/stm32/image-20240925154024727.png)
- 

#### 23.4 RTC相关HAL库驱动介绍

- ![image-20240925154337780](./images/stm32/image-20240925154337780.png)
- 

#### 23.5 RTC基本驱动步骤

- ![image-20240925154909028](./images/stm32/image-20240925154909028.png)
- 

#### 23.6 时间设置和读取

- ![image-20240925155025030](./images/stm32/image-20240925155025030.png)

#### 23.7 编程实战

- ![image-20240925155039740](./images/stm32/image-20240925155039740.png)

- 时间设置好之后，往后备寄存器写入值，该值掉电不丢失，复位后会判断该值是否改变，若不变，时间将会继续计数，而不会重置

- rtc.c

  - ```c
    /**
     ****************************************************************************************************
     * @file        rtc.c
     * @author      正点原子团队(ALIENTEK)
     * @version     V1.0
     * @date        2020-04-24
     * @brief       RTC 驱动代码
     * @license     Copyright (c) 2020-2032, 广州市星翼电子科技有限公司
     ****************************************************************************************************
     * @attention
     *
     * 实验平台:正点原子 STM32F103开发板
     * 在线视频:www.yuanzige.com
     * 技术论坛:www.openedv.com
     * 公司网址:www.alientek.com
     * 购买地址:openedv.taobao.com
     *
     * 修改说明
     * V1.1 20210823
     * HAL库F1的RTC时间设置的BUG仍未修复,时间设置函数我们直接用寄存器配置
     * V1.0 20200422
     * 第一次发布
     *
     ****************************************************************************************************
     */
    
    #include "./BSP/RTC/rtc.h"
    #include "./BSP/LED/led.h"
    #include "./SYSTEM/usart/usart.h"
    #include "./SYSTEM/delay/delay.h"
    
    
    RTC_HandleTypeDef g_rtc_handle; /* RTC控制句柄 */
    _calendar_obj calendar;         /* 时间结构体 */
    
    /**
     * @brief       RTC写入后备区域SRAM
     * @param       bkrx : 后备区寄存器编号,范围:0~41
                            对应 RTC_BKP_DR1~RTC_BKP_DR42
     * @param       data : 要写入的数据,16位长度
     * @retval      无
     */
    void rtc_write_bkr(uint32_t bkrx, uint16_t data)
    {
        HAL_PWR_EnableBkUpAccess(); /* 取消备份区写保护 */
        HAL_RTCEx_BKUPWrite(&g_rtc_handle, bkrx + 1, data);
    }
    
    /**
     * @brief       RTC读取后备区域SRAM
     * @param       bkrx : 后备区寄存器编号,范围:0~41
                    对应 RTC_BKP_DR1~RTC_BKP_DR42
     * @retval      读取到的值
     */
    uint16_t rtc_read_bkr(uint32_t bkrx)
    {
        uint32_t temp = 0;
        temp = HAL_RTCEx_BKUPRead(&g_rtc_handle, bkrx + 1);
        return (uint16_t)temp; /* 返回读取到的值 */
    }
    
    /**
     * @brief       RTC初始化
     *   @note
     *              默认尝试使用LSE,当LSE启动失败后,切换为LSI.
     *              通过BKP寄存器0的值,可以判断RTC使用的是LSE/LSI:
     *              当BKP0==0X5050时,使用的是LSE
     *              当BKP0==0X5051时,使用的是LSI
     *              注意:切换LSI/LSE将导致时间/日期丢失,切换后需重新设置.
     *
     * @param       无
     * @retval      0,成功
     *              1,进入初始化模式失败
     */
    uint8_t rtc_init(void)
    {
        /* 检查是不是第一次配置时钟 */
        uint16_t bkpflag = 0;
    
        __HAL_RCC_PWR_CLK_ENABLE(); /* 使能PWR电源时钟 */
        __HAL_RCC_BKP_CLK_ENABLE(); /* 使能BKP备份时钟 */
        HAL_PWR_EnableBkUpAccess(); /* 取消备份区写保护 */
        
        g_rtc_handle.Instance = RTC;
        g_rtc_handle.Init.AsynchPrediv = 32767;     /* 时钟周期设置,理论值：32767, 这里也可以用 RTC_AUTO_1_SECOND */
        g_rtc_handle.Init.OutPut = RTC_OUTPUTSOURCE_NONE;
        if (HAL_RTC_Init(&g_rtc_handle) != HAL_OK)  /* 初始化RTC */
        {
            return 1;
        }
        
        bkpflag = rtc_read_bkr(0);  /* 读取BKP0的值 */
        if ((bkpflag != 0X5050) && (bkpflag != 0x5051))         /* 之前未初始化过，重新配置 */
        {
            rtc_set_time(2024, 9, 26, 16, 50, 35);              /* 设置时间 */
        }
    
        __HAL_RTC_ALARM_ENABLE_IT(&g_rtc_handle, RTC_IT_SEC);   /* 允许秒中断 */
        __HAL_RTC_ALARM_ENABLE_IT(&g_rtc_handle, RTC_IT_ALRA);  /* 允许闹钟中断 */
        
        HAL_NVIC_SetPriority(RTC_IRQn, 0x2, 0);                 /* 设置RTC中断 */
        HAL_NVIC_EnableIRQ(RTC_IRQn);                           /* 使能中断 */
        
        rtc_get_time(); /* 更新时间 */
        
        return 0;
    }
    
    /**
     * @brief       RTC初始化
     *   @note
     *              RTC底层驱动，时钟配置,此函数会被HAL_RTC_Init()调用
     * @param       hrtc:RTC句柄
     * @retval      无
     */
    void HAL_RTC_MspInit(RTC_HandleTypeDef *hrtc)
    {
        uint16_t retry = 200;
        
        __HAL_RCC_RTC_ENABLE();     /* RTC时钟使能 */
    
        RCC_OscInitTypeDef rcc_oscinitstruct;
        RCC_PeriphCLKInitTypeDef rcc_periphclkinitstruct;
        
        /* 使用寄存器的方式去检测LSE是否可以正常工作 */
        RCC->BDCR |= 1 << 0;    /* 开启外部低速振荡器LSE */
        
        while (retry && ((RCC->BDCR & 0X02) == 0))  /* 等待LSE准备好 */
        {
            retry--;
            delay_ms(5);
        }
    
        if (retry == 0)     /* LSE起振失败 使用LSI */
        {
            rcc_oscinitstruct.OscillatorType = RCC_OSCILLATORTYPE_LSI;  /* 选择要配置的振荡器 */
            rcc_oscinitstruct.LSIState = RCC_LSI_ON;                    /* LSI状态：开启 */
            rcc_oscinitstruct.PLL.PLLState = RCC_PLL_NONE;              /* PLL无配置 */
            HAL_RCC_OscConfig(&rcc_oscinitstruct);                      /* 配置设置的rcc_oscinitstruct */
    
            rcc_periphclkinitstruct.PeriphClockSelection = RCC_PERIPHCLK_RTC;   /* 选择要配置的外设 RTC */
            rcc_periphclkinitstruct.RTCClockSelection = RCC_RTCCLKSOURCE_LSI;   /* RTC时钟源选择 LSI */
            HAL_RCCEx_PeriphCLKConfig(&rcc_periphclkinitstruct);                /* 配置设置的rcc_periphClkInitStruct */
            rtc_write_bkr(0, 0X5051);
        }
        else
        {
            rcc_oscinitstruct.OscillatorType = RCC_OSCILLATORTYPE_LSE ; /* 选择要配置的振荡器 */
            rcc_oscinitstruct.LSEState = RCC_LSE_ON;                    /* LSE状态：开启 */
            rcc_oscinitstruct.PLL.PLLState = RCC_PLL_NONE;              /* PLL不配置 */
            HAL_RCC_OscConfig(&rcc_oscinitstruct);                      /* 配置设置的rcc_oscinitstruct */
    
            rcc_periphclkinitstruct.PeriphClockSelection = RCC_PERIPHCLK_RTC;   /* 选择要配置外设 RTC */
            rcc_periphclkinitstruct.RTCClockSelection = RCC_RTCCLKSOURCE_LSE;   /* RTC时钟源选择LSE */
            HAL_RCCEx_PeriphCLKConfig(&rcc_periphclkinitstruct);                /* 配置设置的rcc_periphclkinitstruct */
            rtc_write_bkr(0, 0X5050);
        }
    }
    
    /**
     * @brief       RTC时钟中断
     *   @note      秒钟中断 / 闹钟中断 共用同一个中断服务函数
     *              根据RTC_CRL寄存器的 SECF 和 ALRF 位区分是哪个中断
     * @param       无
     * @retval      无
     */
    void RTC_IRQHandler(void)
    {
        if (__HAL_RTC_ALARM_GET_FLAG(&g_rtc_handle, RTC_FLAG_SEC) != RESET)     /* 秒中断 */
        {
            rtc_get_time();     /* 更新时间 */
            __HAL_RTC_ALARM_CLEAR_FLAG(&g_rtc_handle, RTC_FLAG_SEC);            /* 清除秒中断 */
            //printf("sec:%d\r\n", calendar.sec);   /* 打印秒钟 */
        }
    
        if (__HAL_RTC_ALARM_GET_FLAG(&g_rtc_handle, RTC_FLAG_ALRAF) != RESET)   /* 闹钟中断 */
        {
            __HAL_RTC_ALARM_CLEAR_FLAG(&g_rtc_handle, RTC_FLAG_ALRAF);          /* 清除闹钟中断 */
            printf("Alarm Time:%d-%d-%d %d:%d:%d\n", calendar.year, calendar.month, calendar.date, calendar.hour, calendar.min, calendar.sec);
        }
    
        __HAL_RTC_ALARM_CLEAR_FLAG(&g_rtc_handle, RTC_FLAG_OW);                 /* 清除溢出中断标志 */
        
        while (!__HAL_RTC_ALARM_GET_FLAG(&g_rtc_handle, RTC_FLAG_RTOFF));       /* 等待RTC寄存器操作完成, 即等待RTOFF == 1 */
    }
    
    /**
     * @brief       判断年份是否是闰年
     *   @note      月份天数表:
     *              月份   1  2  3  4  5  6  7  8  9  10 11 12
     *              闰年   31 29 31 30 31 30 31 31 30 31 30 31
     *              非闰年 31 28 31 30 31 30 31 31 30 31 30 31
     * @param       year : 年份
     * @retval      0, 非闰年; 1, 是闰年;
     */
    static uint8_t rtc_is_leap_year(uint16_t year)
    {
        /* 闰年规则: 四年闰百年不闰，四百年又闰 */
        if ((year % 4 == 0 && year % 100 != 0) || (year % 400 == 0))
        {
            return 1;
        }
        else
        {
            return 0;
        }
    }
    
    /**
     * @brief       设置时间, 包括年月日时分秒
     *   @note      以1970年1月1日为基准, 往后累加时间
     *              合法年份范围为: 1970 ~ 2105年
                    HAL默认为年份起点为2000年
     * @param       syear : 年份
     * @param       smon  : 月份
     * @param       sday  : 日期
     * @param       hour  : 小时
     * @param       min   : 分钟
     * @param       sec   : 秒钟
     * @retval      0, 成功; 1, 失败;
     */
    uint8_t rtc_set_time(uint16_t syear, uint8_t smon, uint8_t sday, uint8_t hour, uint8_t min, uint8_t sec)
    {
        uint32_t seccount = 0;
    
        seccount = rtc_date2sec(syear, smon, sday, hour, min, sec); /* 将年月日时分秒转换成总秒钟数 */
    
        __HAL_RCC_PWR_CLK_ENABLE(); /* 使能电源时钟 */
        __HAL_RCC_BKP_CLK_ENABLE(); /* 使能备份域时钟 */
        HAL_PWR_EnableBkUpAccess(); /* 取消备份域写保护 */
        /* 上面三步是必须的! */
        
        RTC->CRL |= 1 << 4;         /* 进入配置模式 */
        
        RTC->CNTL = seccount & 0xffff;
        RTC->CNTH = seccount >> 16;
        
        RTC->CRL &= ~(1 << 4);      /* 退出配置模式 */
    
        while (!__HAL_RTC_ALARM_GET_FLAG(&g_rtc_handle, RTC_FLAG_RTOFF));       /* 等待RTC寄存器操作完成, 即等待RTOFF == 1 */
    
        return 0;
    }
    
    /**
     * @brief       设置闹钟, 具体到年月日时分秒
     *   @note      以1970年1月1日为基准, 往后累加时间
     *              合法年份范围为: 1970 ~ 2105年
     * @param       syear : 年份
     * @param       smon  : 月份
     * @param       sday  : 日期
     * @param       hour  : 小时
     * @param       min   : 分钟
     * @param       sec   : 秒钟
     * @retval      0, 成功; 1, 失败;
     */
    uint8_t rtc_set_alarm(uint16_t syear, uint8_t smon, uint8_t sday, uint8_t hour, uint8_t min, uint8_t sec)
    {
        uint32_t seccount = 0;
    
        seccount = rtc_date2sec(syear, smon, sday, hour, min, sec); /* 将年月日时分秒转换成总秒钟数 */
    
        __HAL_RCC_PWR_CLK_ENABLE(); /* 使能电源时钟 */
        __HAL_RCC_BKP_CLK_ENABLE(); /* 使能备份域时钟 */
        HAL_PWR_EnableBkUpAccess(); /* 取消备份域写保护 */
        /* 上面三步是必须的! */
        
        RTC->CRL |= 1 << 4;         /* 进入配置模式 */
        
        RTC->ALRL = seccount & 0xffff;
        RTC->ALRH = seccount >> 16;
        
        RTC->CRL &= ~(1 << 4);      /* 退出配置模式 */
    
        while (!__HAL_RTC_ALARM_GET_FLAG(&g_rtc_handle, RTC_FLAG_RTOFF));       /* 等待RTC寄存器操作完成, 即等待RTOFF == 1 */
    
        return 0;
    }
    
    /**
     * @brief       得到当前的时间
     *   @note      该函数不直接返回时间, 时间数据保存在calendar结构体里面
     * @param       无
     * @retval      无
     */
    void rtc_get_time(void)
    {
        static uint16_t daycnt = 0;
        uint32_t seccount = 0;
        uint32_t temp = 0;
        uint16_t temp1 = 0;
        const uint8_t month_table[12] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31}; /* 平年的月份日期表 */
    
        seccount = RTC->CNTH; /* 得到计数器中的值(秒钟数) */
        seccount <<= 16;
        seccount += RTC->CNTL;
    
        temp = seccount / 86400; /* 得到天数(秒钟数对应的) */
    
        if (daycnt != temp) /* 超过一天了 */
        {
            daycnt = temp;
            temp1 = 1970;   /* 从1970年开始 */
    
            while (temp >= 365)
            {
                if (rtc_is_leap_year(temp1)) /* 是闰年 */
                {
                    if (temp >= 366)
                    {
                        temp -= 366;    /* 闰年的秒钟数 */
                    }
                    else
                    {
                        break;
                    }
                }
                else
                {
                    temp -= 365;        /* 平年 */
                }
    
                temp1++;
            }
    
            calendar.year = temp1;      /* 得到年份 */
            temp1 = 0;
    
            while (temp >= 28)      /* 超过了一个月 */
            {
                if (rtc_is_leap_year(calendar.year) && temp1 == 1) /* 当年是不是闰年/2月份 */
                {
                    if (temp >= 29)
                    {
                        temp -= 29; /* 闰年的秒钟数 */
                    }
                    else
                    {
                        break;
                    }
                }
                else
                {
                    if (temp >= month_table[temp1])
                    {
                        temp -= month_table[temp1]; /* 平年 */
                    }
                    else
                    {
                        break;
                    }
                }
    
                temp1++;
            }
    
            calendar.month = temp1 + 1; /* 得到月份 */
            calendar.date = temp + 1;   /* 得到日期 */
        }
    
        temp = seccount % 86400;                                                    /* 得到秒钟数 */
        calendar.hour = temp / 3600;                                                /* 小时 */
        calendar.min = (temp % 3600) / 60;                                          /* 分钟 */
        calendar.sec = (temp % 3600) % 60;                                          /* 秒钟 */
        calendar.week = rtc_get_week(calendar.year, calendar.month, calendar.date); /* 获取星期 */
    }
    
    /**
     * @brief       将年月日时分秒转换成秒钟数
     *   @note      输入公历日期得到星期(起始时间为: 公元0年3月1日开始, 输入往后的任何日期, 都可以获取正确的星期)
     *              使用 基姆拉尔森计算公式 计算, 原理说明见此贴:
     *              https://www.cnblogs.com/fengbohello/p/3264300.html
     * @param       syear : 年份
     * @param       smon  : 月份
     * @param       sday  : 日期
     * @retval      0, 星期天; 1 ~ 6: 星期一 ~ 星期六
     */
    uint8_t rtc_get_week(uint16_t year, uint8_t month, uint8_t day)
    {
        uint8_t week = 0;
    
        if (month < 3)
        {
            month += 12;
            --year;
        }
    
        week = (day + 1 + 2 * month + 3 * (month + 1) / 5 + year + (year >> 2) - year / 100 + year / 400) % 7;
        return week;
    }
    
    /**
     * @brief       将年月日时分秒转换成秒钟数
     *   @note      以1970年1月1日为基准, 1970年1月1日, 0时0分0秒, 表示第0秒钟
     *              最大表示到2105年, 因为uint32_t最大表示136年的秒钟数(不包括闰年)!
     *              本代码参考只linux mktime函数, 原理说明见此贴:
     *              http://www.openedv.com/thread-63389-1-1.html
     * @param       syear : 年份
     * @param       smon  : 月份
     * @param       sday  : 日期
     * @param       hour  : 小时
     * @param       min   : 分钟
     * @param       sec   : 秒钟
     * @retval      转换后的秒钟数
     */
    static long rtc_date2sec(uint16_t syear, uint8_t smon, uint8_t sday, uint8_t hour, uint8_t min, uint8_t sec)
    {
        uint32_t Y, M, D, X, T;
        signed char monx = smon;    /* 将月份转换成带符号的值, 方便后面运算 */
    
        if (0 >= (monx -= 2))       /* 1..12 -> 11,12,1..10 */
        {
            monx += 12; /* Puts Feb last since it has leap day */
            syear -= 1;
        }
    
        Y = (syear - 1) * 365 + syear / 4 - syear / 100 + syear / 400; /* 公元元年1到现在的闰年数 */
        M = 367 * monx / 12 - 30 + 59;
        D = sday - 1;
        X = Y + M + D - 719162;                      /* 减去公元元年到1970年的天数 */
        T = ((X * 24 + hour) * 60 + min) * 60 + sec; /* 总秒钟数 */
        return T;
    }
    
    ```

- rtc.h

  - ```c
    /**
     ****************************************************************************************************
     * @file        rtc.h
     * @author      正点原子团队(ALIENTEK)
     * @version     V1.0
     * @date        2020-04-22
     * @brief       RTC 驱动代码
     * @license     Copyright (c) 2020-2032, 广州市星翼电子科技有限公司
     ****************************************************************************************************
     * @attention
     *
     * 实验平台:正点原子 STM32F103开发板
     * 在线视频:www.yuanzige.com
     * 技术论坛:www.openedv.com
     * 公司网址:www.alientek.com
     * 购买地址:openedv.taobao.com
     *
     * 修改说明
     * V1.0 20200422
     * 第一次发布
     *
     ****************************************************************************************************
     */
    
    #ifndef __RTC_H
    #define __RTC_H
    
    #include "./SYSTEM/sys/sys.h"
    
    
    /* 时间结构体, 包括年月日周时分秒等信息 */
    typedef struct
    {
        uint8_t hour;       /* 时 */
        uint8_t min;        /* 分 */
        uint8_t sec;        /* 秒 */
        /* 公历年月日周 */
        uint16_t year;      /* 年 */
        uint8_t  month;     /* 月 */
        uint8_t  date;      /* 日 */
        uint8_t  week;      /* 周 */
    } _calendar_obj;
    
    extern _calendar_obj calendar;                      /* 时间结构体 */
    
    /* 静态函数 */
    static uint8_t rtc_is_leap_year(uint16_t year);     /* 判断当前年份是不是闰年 */
    static long rtc_date2sec(uint16_t syear, uint8_t smon, uint8_t sday, uint8_t hour, uint8_t min, uint8_t sec);   /* 将年月日时分秒转换成秒钟数 */
    
    /* 接口函数 */
    uint8_t rtc_init(void);                             /* 初始化RTC */
    void rtc_get_time(void);                            /* 获取RTC时间信息 */
    uint16_t rtc_read_bkr(uint32_t bkrx);               /* 读取后备寄存器 */
    void rtc_write_bkr(uint32_t bkrx, uint16_t data);   /* 写后备寄存器 */ 
    uint8_t rtc_get_week(uint16_t year, uint8_t month, uint8_t day);    /* 根据年月日获取星期几 */
    uint8_t rtc_set_time(uint16_t syear, uint8_t smon, uint8_t sday, uint8_t hour, uint8_t min, uint8_t sec);   /* 设置时间 */
    uint8_t rtc_set_alarm(uint16_t syear, uint8_t smon, uint8_t sday, uint8_t hour, uint8_t min, uint8_t sec);  /* 设置闹钟时间 */
    
    #endif
    
    ```

### 24. 低功耗实验

#### 24.1 STM32 电源系统结构介绍

- ![image-20240925173418966](./images/stm32/image-20240925173418966.png)
- ![image-20240925173431055](./images/stm32/image-20240925173431055.png)
- 什么是低功耗--降低集成电路的能量消耗
  - ![image-20240925173513409](./images/stm32/image-20240925173513409.png)
  - 

#### 24.2 低功耗模式介绍

- STM32具有四种模式：运行、停止、待机、睡眠
  - 低功耗模式有三种：停止、待机、睡眠
  - 当内核不需要运行时，可以进入低功耗模式
  - 三种低功耗模式不同点：电源消耗、唤醒时间、唤醒源

- ![image-20240925173728487](./images/stm32/image-20240925173728487.png)
- 睡眠模式
  - ![image-20240925174040420](./images/stm32/image-20240925174040420.png)
- 停止模式
  - ![image-20240925174118367](./images/stm32/image-20240925174118367.png)
- 待机模式
  - ![image-20240925174440702](./images/stm32/image-20240925174440702.png)
- ![image-20240925174457110](./images/stm32/image-20240925174457110.png)
- ![image-20240925174512318](./images/stm32/image-20240925174512318.png)
- 

#### 24.3 低功耗相关寄存器介绍

- ![image-20240926155736946](./images/stm32/image-20240926155736946.png)

- ![image-20240926155809922](./images/stm32/image-20240926155809922.png)

- ![image-20240926155913367](./images/stm32/image-20240926155913367.png)

  - CWUF位置1：清除唤醒标志位
  - 清除唤醒标志位作用：等待KEY_UP按键按下，触发唤醒中断，唤醒标志WUF置1

- ![image-20240926160025219](./images/stm32/image-20240926160025219.png)

- 内核指令

  - ![image-20240926160112891](./images/stm32/image-20240926160112891.png)

    

#### 24.4 低功耗相关HAL库驱动介绍

- ![image-20240926160150430](./images/stm32/image-20240926160150430.png)
- 

#### 24.5 低功耗模式的使用步骤

- ![image-20240926161744440](./images/stm32/image-20240926161744440.png)
- ![image-20240926161837602](./images/stm32/image-20240926161837602.png)
- ![image-20240926161850787](./images/stm32/image-20240926161850787.png)
- ![image-20240926161909565](./images/stm32/image-20240926161909565.png)
- ![image-20240926161922759](./images/stm32/image-20240926161922759.png)
- ![image-20240926161944999](./images/stm32/image-20240926161944999.png)
- 

#### 24.6 编程实战

#### 24.7 课堂总结

### 25. DMA

#### 25.1 DMA介绍

- ![image-20241008161530911](./images/stm32/image-20241008161530911.png)
- DMA：直接存储器访问，简称数据搬运工，搬运数据的过程不需要CPU的参与，可以为CPU减负，提高CPU效率

#### 25.2 DMA结构框图介绍

- ![image-20241008161758330](./images/stm32/image-20241008161758330.png)

- DMA作用：建立数据传输通道

- DMA处理数据过程

  - ![image-20241008161904776](./images/stm32/image-20241008161904776.png)

  - 外设需要传输数据，都需要向DMA发送请求

  - DMA的每个通道用来管理来自一个或多个外设对存储器访问的请求，且都有一个仲裁器，用于处理DMA请求之间的优先级

  - DMA优先级

    - 多个请求通过逻辑或输入到DMA控制器，这里包括输入DMA1/DMA2的所有通道，只有一个请求有效

    - ![image-20241008162307772](./images/stm32/image-20241008162307772.png)

      - 仲裁器管理DMA请求有两个阶段

        - 第一阶段：软件阶段，可以通过软件寄存器DMA_CCRx配置每个通道的优先级，有4个等级：最高、高、中和低
        - 第二阶段：硬件阶段，如果两个请求的具有相同的软件优先级，较低编号的通道优先级更高

        【OS：这里DMA仲裁器处理的请求是多个通道同时有请求，不针对同一个通道的多的请求】

#### 25.3  DMA相关寄存器介绍

- ![image-20241008164657825](./images/stm32/image-20241008164657825.png)
- ![image-20241008164709187](./images/stm32/image-20241008164709187.png)
- ![image-20241008164722559](./images/stm32/image-20241008164722559.png)
- ![image-20241008164736559](./images/stm32/image-20241008164736559.png)
- ![image-20241008164832862](./images/stm32/image-20241008164832862.png)
- ![image-20241008164846823](./images/stm32/image-20241008164846823.png)
- 

#### 25.4 DMA相关HAL库驱动介绍

- ![image-20241008164912702](./images/stm32/image-20241008164912702.png)
- ![image-20241008170039417](./images/stm32/image-20241008170039417.png)
- 

#### 25.5 DMA配置步骤

- ![image-20241009155410354](./images/stm32/image-20241009155410354.png)

#### 25.6 编程实战

- ![image-20241009155436575](./images/stm32/image-20241009155436575.png)

##### 25.6.1 传输方向--内存到内存

###### dma.c

```c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file    dma.c
  * @brief   This file provides code for the configuration
  *          of all the requested memory to memory DMA transfers.
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2024 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
/* USER CODE END Header */

/* Includes ------------------------------------------------------------------*/
#include "dma.h"

/* USER CODE BEGIN 0 */
uint8_t src_buf[10] = {0x01,0x02,0x03,0x04,0x05,0x06,0x07,0x08,0x09,0x0a};
uint8_t dest_buf[10] = {0};
/* USER CODE END 0 */

/*----------------------------------------------------------------------------*/
/* Configure DMA                                                              */
/*----------------------------------------------------------------------------*/

/* USER CODE BEGIN 1 */

/* USER CODE END 1 */
DMA_HandleTypeDef hdma_memtomem_dma1_channel1;

/**
  * Enable DMA controller clock
  * Configure DMA for memory to memory transfers
  *   hdma_memtomem_dma1_channel1
  */
void MX_DMA_Init(void)
{

  /* DMA controller clock enable */
  __HAL_RCC_DMA1_CLK_ENABLE();

  /* Configure DMA request hdma_memtomem_dma1_channel1 on DMA1_Channel1 */
  hdma_memtomem_dma1_channel1.Instance = DMA1_Channel1;
  hdma_memtomem_dma1_channel1.Init.Direction = DMA_MEMORY_TO_MEMORY;
  hdma_memtomem_dma1_channel1.Init.PeriphInc = DMA_PINC_ENABLE;
  hdma_memtomem_dma1_channel1.Init.MemInc = DMA_MINC_ENABLE;
  hdma_memtomem_dma1_channel1.Init.PeriphDataAlignment = DMA_PDATAALIGN_BYTE;
  hdma_memtomem_dma1_channel1.Init.MemDataAlignment = DMA_MDATAALIGN_BYTE;
  hdma_memtomem_dma1_channel1.Init.Mode = DMA_NORMAL;
  hdma_memtomem_dma1_channel1.Init.Priority = DMA_PRIORITY_HIGH;
  if (HAL_DMA_Init(&hdma_memtomem_dma1_channel1) != HAL_OK)
  {
    Error_Handler();
  }
  HAL_DMA_Start(&hdma_memtomem_dma1_channel1,(uint32_t)src_buf,(uint32_t)dest_buf,10);
}

/* USER CODE BEGIN 2 */
void dma_enable_transmit(uint8_t cnt)
{
    __HAL_DMA_DISABLE(&hdma_memtomem_dma1_channel1);
    hdma_memtomem_dma1_channel1.Instance->CNDTR = cnt;
    __HAL_DMA_ENABLE(&hdma_memtomem_dma1_channel1);
}
/* USER CODE END 2 */


```

###### main.c

```c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2024 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"
#include "dma.h"
#include "gpio.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "key.h"
#include "string.h"
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */

/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/

/* USER CODE BEGIN PV */

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */

/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{
  /* USER CODE BEGIN 1 */
    uint8_t t = 0;
    uint8_t key = 0;
  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_DMA_Init();
  /* USER CODE BEGIN 2 */

  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
      key = key_scan(0);
      switch(key)
      {
          case KEY0_PRES:
          {
              memset(dest_buf,0,10);
              dma_enable_transmit(10);
              while(1)
              {
                  if(__HAL_DMA_GET_FLAG(&hdma_memtomem_dma1_channel1,DMA_FLAG_TC1)) //判断是否发送完成
                  {
                      __HAL_DMA_CLEAR_FLAG(&hdma_memtomem_dma1_channel1,DMA_FLAG_TC1);  //清除发送完成标志
                      HAL_GPIO_TogglePin(LED1_GPIO_Port,LED1_Pin);
                  }
                  break;
              }
          }break;
          case KEY1_PRES:
          {
              HAL_GPIO_TogglePin(LED1_GPIO_Port,LED1_Pin);
          }break;
          
      }
      t++;
      if(t > 20)
      {
          HAL_GPIO_TogglePin(LED0_GPIO_Port,LED0_Pin);
          t = 0;
      }
      HAL_Delay(10);
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
  RCC_OscInitStruct.HSEState = RCC_HSE_ON;
  RCC_OscInitStruct.HSEPredivValue = RCC_HSE_PREDIV_DIV1;
  RCC_OscInitStruct.HSIState = RCC_HSI_ON;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;
  RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSE;
  RCC_OscInitStruct.PLL.PLLMUL = RCC_PLL_MUL9;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV2;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_2) != HAL_OK)
  {
    Error_Handler();
  }
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */

```

#### 25.7 总结

##### 25.7.1 编程总结

- 自定义的.c文件，项目更新之后，需要重新添加到项目里
- 内存到内存的数据传输中，数据宽度定义的是一个字节，那么定义内存的数组数据类型一定是1个字节，需要与数据宽度保持一致



### 26. DAC

#### 26.1 DAC 简介

##### 26.1.1 什么是DAC

![image-20241012150852653](./images/stm32/image-20241012150852653.png)



##### 26.1.2 DAC的特性参数

- ![image-20241012150943418](./images/stm32/image-20241012150943418.png)
- 

##### 26.1.3 STM32各系列DAC的主要特性

- ![image-20241012151052416](./images/stm32/image-20241012151052416.png)
- DAC输出通道固定有两个，两个通道间有单独的转换器，各通道可独立转换
  - 两个通道的引脚为了避免寄生电流消耗，必须配置为模拟功能

#### 26.2 DAC工作原理

##### 26.2.1 DAC框图简介

- ![image-20241012152615311](./images/stm32/image-20241012152615311.png)

##### 26.2.2 参考电压/模拟部分电压

- ![image-20241012163145868](./images/stm32/image-20241012163145868.png)

##### 26.2.3 DAC数据格式

- ![image-20241012163203076](./images/stm32/image-20241012163203076.png)
- 通过配置寄存器DHRyyy，来给到DHRx寄存器值，然后通过触发事件将DHRx的值转移到DORx中，通过数模转换器输出

##### 26.2.4 触发源

- ![image-20241012163815583](./images/stm32/image-20241012163815583.png)
- ![image-20241012163916329](./images/stm32/image-20241012163916329.png)
  - tSETTLING：建立时间

##### 26.2.5 DMA请求

- 只有外部触发事件有DMA请求
- ![image-20241012164342399](./images/stm32/image-20241012164342399.png)

##### 26.2.6 DAC输出电压

- <img src="./images/stm32/image-20241012164503951.png" alt="image-20241012164503951" style="zoom:67%;" />

#### 26.3 DAC输出实验

###### 26.3.1 实验简要

- ![image-20241012165837534](./images/stm32/image-20241012165837534.png)

###### 26.3.2 DAC寄存器介绍

- ![image-20241012165852147](./images/stm32/image-20241012165852147.png)
- ![image-20241012165903723](./images/stm32/image-20241012165903723.png)
- ![image-20241012165916102](./images/stm32/image-20241012165916102.png)

###### 26.3.3 DAC输出实验配置步骤

- ![image-20241012165939285](./images/stm32/image-20241012165939285.png)
- ![image-20241012165954102](./images/stm32/image-20241012165954102.png)
- ![image-20241012170018630](./images/stm32/image-20241012170018630.png)

###### 26.3.4 编程实战：DAC输出实验

#### 26.4 DAC输出三角波实验

#### 26.5 DAC输出正弦波实验

#### 26.6 PWM DAC实验



### 27.  ADC

#### 27.1 ADC简介

##### 27.1.1 什么是ADC

- ![image-20241010142345560](./images/stm32/image-20241010142345560.png)
- ADC就是模拟量转换为数字量的转换器，模拟量指电压，数字量指二进制

##### 27.1.2 常见的ADC类型

- 并联比较型
- 逐次逼近型
- ![image-20241010142524472](./images/stm32/image-20241010142524472.png)

##### 27.1.3 并联比较型工作示意图

- ![image-20241010142601005](./images/stm32/image-20241010142601005.png)
- 工作原理--以分辨率为3位为例
  - 参考电压通过分压电阻分压分为7个档位
  - 输入的模拟电压与参考电压分压后比较，相同就将模拟电压输出到编码器，编码成二进制输出，D0是低位
- 优点
  - 转换速度快
- 缺点
  - 成本高、功耗高、分辨率低
  - 成本有分压电阻和比较器，分辨率越高，分压电阻和比较器就越高

##### 27.1.4 逐次逼近型工作示意图

- ![image-20241010143124310](./images/stm32/image-20241010143124310.png)
- 分辨率指D2、D1、D0有三位，增加位数，就是增加分辨率，采样速率就会下降
- 数码寄存器 分辨率从高位逐次加1，转换为模拟量，与模拟电压比较，若Vx>=V0，则高位保持，次高位再加1，循环工作，直到Vx<V 0，就可确定模拟量转换成数字量的值

##### 27.1.5 ADC特性参数

- ![image-20241010143558666](./images/stm32/image-20241010143558666.png)

##### 27.1.6 STM32各系列ADC的主要特性

- ![image-20241010143617929](./images/stm32/image-20241010143617929.png)
- STM32芯片内的ADC类型都是逐次逼近型，因为其结构简单，电路容易集成

#### 27.2 ADC工作原理

##### 27.2.1 ADC框图简介

- ![image-20241010144833655](./images/stm32/image-20241010144833655.png)
- ![image-20241010144851074](./images/stm32/image-20241010144851074.png)
- ![image-20241010144904745](./images/stm32/image-20241010144904745.png)
- ![image-20241010144918385](./images/stm32/image-20241010144918385.png)

##### 27.2.2 参考电压/模拟部分电压

- ![image-20241010151233384](./images/stm32/image-20241010151233384.png)
  - 参考电压正极这里默认是用跳线帽接VDDA

##### 27.2.3 输入通道

- ![image-20241010151326742](./images/stm32/image-20241010151326742.png)
- 

##### 27.2.4 转换序列

- ![image-20241010151356918](./images/stm32/image-20241010151356918.png)
- 注入组可以打断规则组
  - ![image-20241010151514069](./images/stm32/image-20241010151514069.png)
- 规则序列
  - ![image-20241010151601653](./images/stm32/image-20241010151601653.png)
  - 例如：SQL[3:0] = 2，表示设置规则转换的通道数为3
  - 规则序列寄存器有3个，因为规则序列最多可转换16个通道
- 注入序列
  - ![image-20241010151949959](./images/stm32/image-20241010151949959.png)
  - 这里开始的寄存器位通过公式计算得来，即从倒数开始加几个通道
    - 例如需要注入序列转换的通道数是2，那么注入序列转换的顺序就是从JSQ3[4:0]开始

##### 27.2.5 触发源

- ![image-20241010154252561](./images/stm32/image-20241010154252561.png)
  - 通常使用外部事件触发转换
- ![image-20241010154338003](./images/stm32/image-20241010154338003.png)
- ![image-20241010154408777](./images/stm32/image-20241010154408777.png)
- ![image-20241010154417872](./images/stm32/image-20241010154417872.png)
- 

##### 27.2.6 转换时间

- ADC时钟来源--APB2总线
- ![image-20241010155048629](./images/stm32/image-20241010155048629.png)
- 如何设置ADC转换时间
  - ![image-20241010155235120](./images/stm32/image-20241010155235120.png)

##### 27.2.7 数据寄存器

- ![image-20241010160116748](./images/stm32/image-20241010160116748.png)
- ADC的分辨率只有12位，数据寄存器都是16位，需要设置数据存储的对齐方式，常采用右对齐

##### 27.2.8 中断

- ![image-20241010160518510](./images/stm32/image-20241010160518510.png)
  - 使能控制位置1，事件标志置1时就会产生中断

##### 27.2.9 单次转换模式和连续转换模式

- ![image-20241010161720689](./images/stm32/image-20241010161720689.png)

##### 27.2.10 扫描模式

- ![image-20241010161753896](./images/stm32/image-20241010161753896.png)
  - 开启扫描模式后，ADC会扫描被选中的所有通道
  - 开始连续模式和扫描模式后，ADC会扫描一遍所有被选中的通道，然后继续下一次循环扫描，直到扫描次数用完为止
- 不同模式搭配
  - ![image-20241010161826457](./images/stm32/image-20241010161826457.png)

#### 27.3 单通道ADC采集实验

##### 27.3.1 实验简要

- ![image-20241011161447819](./images/stm32/image-20241011161447819.png)

- ![image-20241010174038474](./images/stm32/image-20241010174038474.png)

##### 27.3.2 ADC寄存器介绍

- ![image-20241010174128213](./images/stm32/image-20241010174128213.png)
- ![image-20241010174204136](./images/stm32/image-20241010174204136.png)
- ![image-20241010174233228](./images/stm32/image-20241010174233228.png)
- ![image-20241010174301434](./images/stm32/image-20241010174301434.png)
- ![image-20241010174318783](./images/stm32/image-20241010174318783.png)
- ![image-20241010174343408](./images/stm32/image-20241010174343408.png)
- ![image-20241010174358440](./images/stm32/image-20241010174358440.png)
- ![image-20241010174412743](./images/stm32/image-20241010174412743.png)
- ![image-20241010174427982](./images/stm32/image-20241010174427982.png)
- 

##### 27.3.3 单通道ADC采集实验配置步骤

- ![image-20241010180124030](./images/stm32/image-20241010180124030.png)
- ![image-20241010180153472](./images/stm32/image-20241010180153472.png)
- ![image-20241010180214371](./images/stm32/image-20241010180214371.png)
- ![image-20241010180230361](./images/stm32/image-20241010180230361.png)

##### 27.3.4 编程实战：单通道ADC采集实验

- 采集ADC1通道1输入的电压信号，经过AD转换后的数字值，然后经过精度换算成电压值打印出来

- Cobe MX 配置步骤

  - ![image-20241010210736264](./images/stm32/image-20241010210736264.png)

- adc.c

  - ```c
    
    /* USER CODE BEGIN Header */
    /**
      ******************************************************************************
      * @file    adc.c
      * @brief   This file provides code for the configuration
      *          of the ADC instances.
      ******************************************************************************
      * @attention
      *
      * Copyright (c) 2024 STMicroelectronics.
      * All rights reserved.
      *
      * This software is licensed under terms that can be found in the LICENSE file
      * in the root directory of this software component.
      * If no LICENSE file comes with this software, it is provided AS-IS.
      *
      ******************************************************************************
      */
    /* USER CODE END Header */
    /* Includes ------------------------------------------------------------------*/
    #include "adc.h"
    
    /* USER CODE BEGIN 0 */
    
    /* USER CODE END 0 */
    
    ADC_HandleTypeDef hadc1;
    
    /* ADC1 init function */
    void MX_ADC1_Init(void)
    {
    
      /* USER CODE BEGIN ADC1_Init 0 */
    
      /* USER CODE END ADC1_Init 0 */
    
      ADC_ChannelConfTypeDef sConfig = {0};
    
      /* USER CODE BEGIN ADC1_Init 1 */
    
      /* USER CODE END ADC1_Init 1 */
    
      /** Common config
      */
      hadc1.Instance = ADC1;
      hadc1.Init.ScanConvMode = ADC_SCAN_DISABLE;
      hadc1.Init.ContinuousConvMode = DISABLE;
      hadc1.Init.DiscontinuousConvMode = DISABLE;
      hadc1.Init.ExternalTrigConv = ADC_SOFTWARE_START;
      hadc1.Init.DataAlign = ADC_DATAALIGN_RIGHT;
      hadc1.Init.NbrOfConversion = 1;
      if (HAL_ADC_Init(&hadc1) != HAL_OK)
      {
        Error_Handler();
      }
    
      /** Configure Regular Channel
      */
      sConfig.Channel = ADC_CHANNEL_1;
      sConfig.Rank = ADC_REGULAR_RANK_1;
      sConfig.SamplingTime = ADC_SAMPLETIME_239CYCLES_5;
      if (HAL_ADC_ConfigChannel(&hadc1, &sConfig) != HAL_OK)
      {
        Error_Handler();
      }
      /* USER CODE BEGIN ADC1_Init 2 */
        HAL_ADCEx_Calibration_Start(&hadc1);
      /* USER CODE END ADC1_Init 2 */
    
    }
    
    void HAL_ADC_MspInit(ADC_HandleTypeDef* adcHandle)
    {
    
      GPIO_InitTypeDef GPIO_InitStruct = {0};
      if(adcHandle->Instance==ADC1)
      {
      /* USER CODE BEGIN ADC1_MspInit 0 */
        RCC_PeriphCLKInitTypeDef adc_clk_init = {0};
      /* USER CODE END ADC1_MspInit 0 */
        /* ADC1 clock enable */
        __HAL_RCC_ADC1_CLK_ENABLE();
    
        __HAL_RCC_GPIOA_CLK_ENABLE();
        /**ADC1 GPIO Configuration
        PA1     ------> ADC1_IN1
        */
        GPIO_InitStruct.Pin = GPIO_PIN_1;
        GPIO_InitStruct.Mode = GPIO_MODE_ANALOG;
        HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
    
      /* USER CODE BEGIN ADC1_MspInit 1 */
        adc_clk_init.PeriphClockSelection = RCC_PERIPHCLK_ADC;
        adc_clk_init.AdcClockSelection = RCC_CFGR_ADCPRE_DIV6;
        HAL_RCCEx_PeriphCLKConfig(&adc_clk_init);
      /* USER CODE END ADC1_MspInit 1 */
      }
    }
    
    void HAL_ADC_MspDeInit(ADC_HandleTypeDef* adcHandle)
    {
    
      if(adcHandle->Instance==ADC1)
      {
      /* USER CODE BEGIN ADC1_MspDeInit 0 */
    
      /* USER CODE END ADC1_MspDeInit 0 */
        /* Peripheral clock disable */
        __HAL_RCC_ADC1_CLK_DISABLE();
    
        /**ADC1 GPIO Configuration
        PA1     ------> ADC1_IN1
        */
        HAL_GPIO_DeInit(GPIOA, GPIO_PIN_1);
    
      /* USER CODE BEGIN ADC1_MspDeInit 1 */
    
      /* USER CODE END ADC1_MspDeInit 1 */
      }
    }
    
    /* USER CODE BEGIN 1 */
    /* 获得ADC转换后的结果函数 */
    uint32_t adc_get_result(void)
    {
        HAL_ADC_Start(&hadc1);
        HAL_ADC_PollForConversion(&hadc1,10);
        return (uint16_t)HAL_ADC_GetValue(&hadc1);
    }
    /* USER CODE END 1 */
    
    ```

  - main.c 主要

    - ```c
      while (1)
        {
            adc_d = adc_get_result();
            printf("adc_d = %d\n",adc_d);
            adc_v = adc_d * (3.3/4096);  //分辨率12位
            printf("adc_v = %.2fV\n",adc_v);
            HAL_Delay(300);
            HAL_GPIO_TogglePin(LED0_GPIO_Port,LED0_Pin);
          /* USER CODE END WHILE */
      
          /* USER CODE BEGIN 3 */
        }
      ```

      

#### 27.4 单通道ADC采集（DMA读取）实验

- ![image-20241011094733230](./images/stm32/image-20241011094733230.png)

- ![image-20241011094745056](./images/stm32/image-20241011094745056.png)

  - **启动DMA，开启传输完成中断函数**与**触发ADC转换，DMA传输数据函数**必须写在ADC初始化函数里

  - adc.c

    - ```c
      /* USER CODE BEGIN Header */
      /**
        ******************************************************************************
        * @file    adc.c
        * @brief   This file provides code for the configuration
        *          of the ADC instances.
        ******************************************************************************
        * @attention
        *
        * Copyright (c) 2024 STMicroelectronics.
        * All rights reserved.
        *
        * This software is licensed under terms that can be found in the LICENSE file
        * in the root directory of this software component.
        * If no LICENSE file comes with this software, it is provided AS-IS.
        *
        ******************************************************************************
        */
      /* USER CODE END Header */
      /* Includes ------------------------------------------------------------------*/
      #include "adc.h"
      
      /* USER CODE BEGIN 0 */
      uint8_t g_adc_dma_sta;
      uint16_t adc_dma_buf[100] = {0};
      /* USER CODE END 0 */
      
      ADC_HandleTypeDef hadc1;
      DMA_HandleTypeDef hdma_adc1;
      
      /* ADC1 init function */
      void MX_ADC1_Init(void)
      {
      
        /* USER CODE BEGIN ADC1_Init 0 */
      
        /* USER CODE END ADC1_Init 0 */
      
        ADC_ChannelConfTypeDef sConfig = {0};
      
        /* USER CODE BEGIN ADC1_Init 1 */
      
        /* USER CODE END ADC1_Init 1 */
      
        /** Common config
        */
        hadc1.Instance = ADC1;
        hadc1.Init.ScanConvMode = ADC_SCAN_DISABLE;
        hadc1.Init.ContinuousConvMode = ENABLE;
        hadc1.Init.DiscontinuousConvMode = DISABLE;
        hadc1.Init.ExternalTrigConv = ADC_SOFTWARE_START;
        hadc1.Init.DataAlign = ADC_DATAALIGN_RIGHT;
        hadc1.Init.NbrOfConversion = 1;
        if (HAL_ADC_Init(&hadc1) != HAL_OK)
        {
          Error_Handler();
        }
      
        /** Configure Regular Channel
        */
        sConfig.Channel = ADC_CHANNEL_1;
        sConfig.Rank = ADC_REGULAR_RANK_1;
        sConfig.SamplingTime = ADC_SAMPLETIME_239CYCLES_5;
        if (HAL_ADC_ConfigChannel(&hadc1, &sConfig) != HAL_OK)
        {
          Error_Handler();
        }
        /* USER CODE BEGIN ADC1_Init 2 */
          HAL_ADCEx_Calibration_Start(&hadc1);   //adc校准
          HAL_DMA_Start_IT(&hdma_adc1,(uint32_t)&ADC1->DR,(uint32_t)&adc_dma_buf,0);
          HAL_ADC_Start_DMA(&hadc1,(uint32_t *)adc_dma_buf,0);
        /* USER CODE END ADC1_Init 2 */
      
      }
      
      void HAL_ADC_MspInit(ADC_HandleTypeDef* adcHandle)
      {
      
        GPIO_InitTypeDef GPIO_InitStruct = {0};
        if(adcHandle->Instance==ADC1)
        {
        /* USER CODE BEGIN ADC1_MspInit 0 */
          RCC_PeriphCLKInitTypeDef adc_clk_init = {0};
        /* USER CODE END ADC1_MspInit 0 */
          /* ADC1 clock enable */
          __HAL_RCC_ADC1_CLK_ENABLE();
      
          __HAL_RCC_GPIOA_CLK_ENABLE();
          /**ADC1 GPIO Configuration
          PA1     ------> ADC1_IN1
          */
          GPIO_InitStruct.Pin = GPIO_PIN_1;
          GPIO_InitStruct.Mode = GPIO_MODE_ANALOG;
          HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
      
          /* ADC1 DMA Init */
          /* ADC1 Init */
          hdma_adc1.Instance = DMA1_Channel1;
          hdma_adc1.Init.Direction = DMA_PERIPH_TO_MEMORY;
          hdma_adc1.Init.PeriphInc = DMA_PINC_DISABLE;
          hdma_adc1.Init.MemInc = DMA_MINC_ENABLE;
          hdma_adc1.Init.PeriphDataAlignment = DMA_PDATAALIGN_HALFWORD;
          hdma_adc1.Init.MemDataAlignment = DMA_MDATAALIGN_HALFWORD;
          hdma_adc1.Init.Mode = DMA_NORMAL;
          hdma_adc1.Init.Priority = DMA_PRIORITY_LOW;
          if (HAL_DMA_Init(&hdma_adc1) != HAL_OK)
          {
            Error_Handler();
          }
      
          __HAL_LINKDMA(adcHandle,DMA_Handle,hdma_adc1);   //DMA1与ADC1外设关联
      
        /* USER CODE BEGIN ADC1_MspInit 1 */
          adc_clk_init.PeriphClockSelection = RCC_PERIPHCLK_ADC;
          adc_clk_init.AdcClockSelection = RCC_CFGR_ADCPRE_DIV6;
          HAL_RCCEx_PeriphCLKConfig(&adc_clk_init);
          
          
        /* USER CODE END ADC1_MspInit 1 */
        }
      }
      
      void HAL_ADC_MspDeInit(ADC_HandleTypeDef* adcHandle)
      {
      
        if(adcHandle->Instance==ADC1)
        {
        /* USER CODE BEGIN ADC1_MspDeInit 0 */
      
        /* USER CODE END ADC1_MspDeInit 0 */
          /* Peripheral clock disable */
          __HAL_RCC_ADC1_CLK_DISABLE();
      
          /**ADC1 GPIO Configuration
          PA1     ------> ADC1_IN1
          */
          HAL_GPIO_DeInit(GPIOA, GPIO_PIN_1);
      
          /* ADC1 DMA DeInit */
          HAL_DMA_DeInit(adcHandle->DMA_Handle);
        /* USER CODE BEGIN ADC1_MspDeInit 1 */
      
        /* USER CODE END ADC1_MspDeInit 1 */
        }
      }
      
      /* USER CODE BEGIN 1 */
      /* 获得ADC转换后的结果函数 */
      uint32_t adc_get_result(void)
      {
          HAL_ADC_Start(&hadc1);
          HAL_ADC_PollForConversion(&hadc1,10);
          return (uint16_t)HAL_ADC_GetValue(&hadc1);
      }
      /* 使能一次ADC DMA传输函数 */
      void adc_dma_enable(uint16_t cndtr)
      {
          /* 寄存器版本 */
          ADC1->CR2 &= ~(1<<0);   //关闭ADC
          DMA1_Channel1->CCR &= ~(1<<0);  //关闭DMA
          while(DMA1_Channel1->CCR & (1<<0));    //等待DMA1关闭
          DMA1_Channel1->CNDTR = cndtr;   //传输数量赋值
          DMA1_Channel1->CCR |= 1<<0;  //开启DMA1
          ADC1->CR2 |= 1<<0;    //开启ADC1
          ADC1->CR2 |= 1<<22;    //开启规则转换通道
          
          /* HAL库版本 */
      //    __HAL_ADC_DISABLE(&hadc1);
      //    __HAL_DMA_DISABLE(&hdma_adc1);
      //    while (__HAL_DMA_GET_FLAG(&hdma_adc1, __HAL_DMA_GET_TC_FLAG_INDEX(&hdma_adc1)));
      //    DMA1_Channel1->CNDTR = cndtr;
      //    __HAL_ADC_ENABLE(&hadc1);
      //    __HAL_DMA_ENABLE(&hdma_adc1);
      //    HAL_ADC_Start(&hadc1);
      }
      
      /* ADC DMA 采集中断服务函数 */
      void DMA1_Channel1_IRQHandler(void)
      {
        /* USER CODE BEGIN DMA1_Channel1_IRQn 0 */
          if(DMA1->ISR & (1<<1))   //DMA传输设定次数完成才会触发中断
          {
              g_adc_dma_sta = 1;
              DMA1->IFCR |= 1<<1;
          }
        /* USER CODE END DMA1_Channel1_IRQn 0 */
        HAL_DMA_IRQHandler(&hdma_adc1);
        /* USER CODE BEGIN DMA1_Channel1_IRQn 1 */
      
        /* USER CODE END DMA1_Channel1_IRQn 1 */
      }
      /* USER CODE END 1 */
      
      
      
      
      
      
      
      
      
      
      
      
      
      ```

      

- ![image-20241011094756782](./images/stm32/image-20241011094756782.png)

- ![image-20241011094808812](./images/stm32/image-20241011094808812.png)

##### 27.4.1 CobeMX --ADC采集DMA传输配置

- ADC配置
  - ![image-20241011165708622](./images/stm32/image-20241011165708622.png)
- ADC配置完后，配置DMA，此时DMA的选项中会关联ADC1
  - ![image-20241011165952931](./images/stm32/image-20241011165952931.png)

#### 27.5 多通道ADC采集（DMA读取）实验

- 使用ADC1多个通道采集电压信号，经过AD转换为数字值，并打印出来，然后再将数字值换算成电压

- 多通道在单通道的基础上，增加了ADC通道，DMA部分不用动，ADC通道配置需要增加

  - ```c
      sConfig.Channel = ADC_CHANNEL_0;  //通道0
      sConfig.Rank = ADC_REGULAR_RANK_1;  //转换顺序1
      sConfig.SamplingTime = ADC_SAMPLETIME_239CYCLES_5;
      
      sConfig.Channel = ADC_CHANNEL_1;  //通道1
      sConfig.Rank = ADC_REGULAR_RANK_2;  //转换顺序2
      if (HAL_ADC_ConfigChannel(&hadc1, &sConfig) != HAL_OK)
      {
        Error_Handler();
      }
    ```

  - ADC通道GPIO配置也要增加

    - ```c
          GPIO_InitStruct.Pin = GPIO_PIN_0|GPIO_PIN_1;
          GPIO_InitStruct.Mode = GPIO_MODE_ANALOG;
          HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
      ```

  - main.c

    - ```c
      while (1)
        {
              if(g_adc_dma_sta == 1)
              {
                  
                  for(i = 0;i < 2;i++)
                  {
                      sum = 0;
                      for(j = 0;j < 50;j++)
                      {
                          sum += adc_dma_buf[j*2 + i];  //选中每次扫描的同一个通道进行数值累加
                      }
                      adc_d = sum/50;
                      printf("PA%d:adc_d = %d\n",i,adc_d);
                      adc_v = adc_d * (3.3/4096);
                      printf("PA%d:adc_v = %.2fV\n",i,adc_v);
                  }
                  g_adc_dma_sta = 0;
                  adc_dma_enable(100);
              }
      }
      ```

      

- ![image-20241011152817041](./images/stm32/image-20241011152817041.png)

#### 27.6 单通道ADC过采样实验

##### 27.6.1 如何使用过采样和求均值的方式提高采样率

- ![image-20241011161808053](./images/stm32/image-20241011161808053.png)

##### 27.6.2 实验简要

- ![image-20241011161908547](./images/stm32/image-20241011161908547.png)
  - 提高采样率到16位后，采样256次才能获取一次采样值，同理，转换时间也是原有基础的256倍

##### 27.6.3 编程实战：单通道ADC过采样（16位分辨率）实验

- 在ADC单通道采集用DMA传输的基础上修改

  - 注意采集存储数组容量，避免溢出导致程序卡死

- main.c

  - ```c
    while (1)
      {
            if(g_adc_dma_sta == 1)
            {
                sum = 0;
                for(i = 0;i < 256*10;i++)
                {
                    sum += adc_dma_buf[i];
                }
                adc_d = (sum/10)>>4;  //求采集的平均值，右移4位获取16位分辨率采集的平均值
                printf("adc_d = %d\n",adc_d);
                adc_v = adc_d * (3.3/65536); //2^16 = 65536
                printf("adc_v = %.2fV\n",adc_v);
                g_adc_dma_sta = 0;
                adc_dma_enable(256*10);
            }
          HAL_Delay(100);
          HAL_GPIO_TogglePin(LED0_GPIO_Port,LED0_Pin);
        /* USER CODE END WHILE */
    
        /* USER CODE BEGIN 3 */
      }
      /* USER CODE END 3 */
    }
    
    ```

    

#### 27.7 内部传感器实验

- 数据手册中可查到的电气参数
  - ![image-20241012095845459](./images/stm32/image-20241012095845459.png)



##### 27.7.1 STM32内部温度传感器简介

- ![image-20241012095949126](./images/stm32/image-20241012095949126.png)

##### 27.7.2 温度计算方法

- ![image-20241012100154727](./images/stm32/image-20241012100154727.png)

##### 27.7.3 实验简要

- 实验分3步
  - 上电
  - 打开ADC1_IN16
  - 带入公式计算温度，并显示
- ![image-20241012100456816](./images/stm32/image-20241012100456816.png)

##### 27.7.4 编程实战：内部温度传感器实验

- ADC时钟源配置
  - ![image-20241012104733239](./images/stm32/image-20241012104733239.png)
    - 在时钟树里配置好之后，就会生成代码，在main.c的SystemClock_Config();里

- CobeMX配置——ADC1_IN16

  - ![image-20241012104638777](./images/stm32/image-20241012104638777.png)

- adc.c

  - ```c
    /* USER CODE BEGIN Header */
    /**
      ******************************************************************************
      * @file    adc.c
      * @brief   This file provides code for the configuration
      *          of the ADC instances.
      ******************************************************************************
      * @attention
      *
      * Copyright (c) 2024 STMicroelectronics.
      * All rights reserved.
      *
      * This software is licensed under terms that can be found in the LICENSE file
      * in the root directory of this software component.
      * If no LICENSE file comes with this software, it is provided AS-IS.
      *
      ******************************************************************************
      */
    /* USER CODE END Header */
    /* Includes ------------------------------------------------------------------*/
    #include "adc.h"
    
    /* USER CODE BEGIN 0 */
    
    /* USER CODE END 0 */
    
    ADC_HandleTypeDef hadc1;
    
    /* ADC1 init function */
    void MX_ADC1_Init(void)
    {
    
      /* USER CODE BEGIN ADC1_Init 0 */
    
      /* USER CODE END ADC1_Init 0 */
    
      ADC_ChannelConfTypeDef sConfig = {0};
    
      /* USER CODE BEGIN ADC1_Init 1 */
    
      /* USER CODE END ADC1_Init 1 */
    
      /** Common config
      */
      hadc1.Instance = ADC1;
      hadc1.Init.ScanConvMode = ADC_SCAN_DISABLE;
      hadc1.Init.ContinuousConvMode = DISABLE;
      hadc1.Init.DiscontinuousConvMode = DISABLE;
      hadc1.Init.ExternalTrigConv = ADC_SOFTWARE_START;
      hadc1.Init.DataAlign = ADC_DATAALIGN_RIGHT;
      hadc1.Init.NbrOfConversion = 1;
      if (HAL_ADC_Init(&hadc1) != HAL_OK)
      {
        Error_Handler();
      }
    
      /** Configure Regular Channel
      */
      sConfig.Channel = ADC_CHANNEL_TEMPSENSOR;
      sConfig.Rank = ADC_REGULAR_RANK_1;
      sConfig.SamplingTime = ADC_SAMPLETIME_239CYCLES_5;
      if (HAL_ADC_ConfigChannel(&hadc1, &sConfig) != HAL_OK)
      {
        Error_Handler();
      }
      /* USER CODE BEGIN ADC1_Init 2 */
        HAL_ADCEx_Calibration_Start(&hadc1); //ADC校准
      /* USER CODE END ADC1_Init 2 */
    
    }
    
    void HAL_ADC_MspInit(ADC_HandleTypeDef* adcHandle)
    {
    
      if(adcHandle->Instance==ADC1)
      {
      /* USER CODE BEGIN ADC1_MspInit 0 */
    
      /* USER CODE END ADC1_MspInit 0 */
        /* ADC1 clock enable */
        __HAL_RCC_ADC1_CLK_ENABLE();
      /* USER CODE BEGIN ADC1_MspInit 1 */
    
      /* USER CODE END ADC1_MspInit 1 */
      }
    }
    
    void HAL_ADC_MspDeInit(ADC_HandleTypeDef* adcHandle)
    {
    
      if(adcHandle->Instance==ADC1)
      {
      /* USER CODE BEGIN ADC1_MspDeInit 0 */
    
      /* USER CODE END ADC1_MspDeInit 0 */
        /* Peripheral clock disable */
        __HAL_RCC_ADC1_CLK_DISABLE();
      /* USER CODE BEGIN ADC1_MspDeInit 1 */
    
      /* USER CODE END ADC1_MspDeInit 1 */
      }
    }
    
    /* USER CODE BEGIN 1 */
    /* 获取转换后的结果函数 */
    uint16_t adc_get_result(void)
    {
        HAL_ADC_Start(&hadc1);
        HAL_ADC_PollForConversion(&hadc1,10);
        return HAL_ADC_GetValue(&hadc1);
    }
    /* 获取内部温度传感器的温度 */
    double adc_get_temperature(void)
    {
        uint16_t adcx;
       // short result;
        double temperature;
        adcx = adc_get_result();
        temperature = adcx * (3.3/4096);
        temperature = (1.43 - temperature)/0.0043 + 25;
        return temperature;
    }
    /* USER CODE END 1 */
    
    ```

- main.c -- 主要部分

  - ```c
    double temp = 0;
    while (1)
      {
          temp = adc_get_temperature(); //获取温度值
          printf("temp = %.2lf℃\n",temp);
          HAL_Delay(500);
          HAL_GPIO_TogglePin(LED0_GPIO_Port,LED0_Pin);
        /* USER CODE END WHILE */
    
        /* USER CODE BEGIN 3 */
      }
    ```

    

#### 27.8 光敏传感器实验

##### 27.8.1 光敏二极管简介

- <img src="./images/stm32/image-20241012110056927.png" alt="image-20241012110056927" style="zoom:80%;" />
- 光敏二极管实际是一个PN结
  - 工作时需要加反向电压，无光照时，反向电流很小，称为暗电流，小到PN结基本是断开状态
  - 有光照时，反向电流称为光电流，光照越强，光电流越强
  - <img src="./images/stm32/image-20241012110403520.png" alt="image-20241012110403520" style="zoom: 50%;" />
    - ADC是电压型采集，这里采集PF8点处的电压值，光照越强，R34电阻两端的电越大，采集到的电压值越小

##### 27.8.2实验简要

- <img src="./images/stm32/image-20241012110640018.png" alt="image-20241012110640018" style="zoom: 67%;" />

##### 27.8.3 编程实战：光敏传感器实验

- 编程步骤

  - 配置ADC3_IN6

  - 开启ADC3校准

    - ```c
       HAL_ADCEx_Calibration_Start(&hadc3); //ADC校准
      ```

  - 编写ADC取值函数和光感函数

    - ```c
      /* 获取转换后的结果函数 */
      uint16_t adc_get_result(void)
      {
          HAL_ADC_Start(&hadc3);
          HAL_ADC_PollForConversion(&hadc3,10);
          return HAL_ADC_GetValue(&hadc3);
      }
      /* 获取光照强度 */
      uint8_t lsensor_get_val(void)
      {
          uint16_t temp_val = 0;
          uint8_t temp = 0;
          temp_val = adc_get_result();
          temp = temp_val/40;   //temp_val最大为4095
          if(temp > 100) 
          {
              temp = 100;   //消掉4095后两位
          }
          temp = 100-temp;  //因为temp与光照成反比，这里用100减temp，就可以实现temp与光照正相关
          return temp;
      }
      ```

  - 在main.c中调用

    - ```c
      uint8_t light = 0;
      while (1)
        {
            light = lsensor_get_val();
            printf("light = %d\n",light);
            HAL_Delay(200);
            HAL_GPIO_TogglePin(LED0_GPIO_Port,LED0_Pin);
          /* USER CODE END WHILE */
      
          /* USER CODE BEGIN 3 */
        }
      ```

      

#### 27.9 课堂总结

### 28. IIC 

### 29. SPI 

- 主要使用硬件SPI

#### 29.1 SPI介绍

- ![image-20241106100653169](./images/stm32/image-20241106100653169.png)
- SPI是一种高速的、串行的、全双工的同步通信总线
- 主机指MCU，从机指各种外设芯片，主机跟从机的连线如上，时钟信号和片选信号都由主机发出
- SPI数据格式可设置8位或16位，传输顺序也可设置MSB或LSB
  - MSB：一字节的高位在前
  - LSB：一字节的低位在前

#### 29.2 SPI结构框图介绍

- ![image-20241106101732055](./images/stm32/image-20241106101732055.png)
- ![image-20241106101956121](./images/stm32/image-20241106101956121.png)
  - F1/4/7SPI结构基本一致，H7略有不同
- STM32硬件SPI外设对应引脚
  - ![image-20241106102130858](./images/stm32/image-20241106102130858.png)
- 主机模式下的数据发送和接收
  - ![image-20241106102641084](./images/stm32/image-20241106102641084.png)
  - SPI工作原理
    - ![image-20241106103043109](./images/stm32/image-20241106103043109.png)
    - 一个时钟的上升沿，主机的最高位给给到MOSI，从机的最高位给到MISO
    - 该时钟的下降沿时，MOSI的数据给到从机的最低位，MISO的数据给到主机的最低位
    - 然后主机会发送八个时钟，这样主机和从机的数据就会进行一个交换 

#### 29.3 SPI工作模式介绍

- 决定数据有效性
- ![image-20241106110314353](./images/stm32/image-20241106110314353.png)
- 时钟极性CPOL、时钟相位CPHA，组合起来有四种SPI工作模式
- ![image-20241106110632559](./images/stm32/image-20241106110632559.png)

#### 29.4 SPI相关寄存器介绍

- ![image-20241106110755731](./images/stm32/image-20241106110755731.png)
- SPI_CR1
  - ![image-20241106111128713](./images/stm32/image-20241106111128713.png)
  - ![image-20241106111157752](./images/stm32/image-20241106111157752.png)
- SPI_SR
  - ![image-20241106111224035](./images/stm32/image-20241106111224035.png)
- SPI_DR
  - ![image-20241106111252463](./images/stm32/image-20241106111252463.png)

#### 29.5 SPI相关HAL库介绍

- ![image-20241106111355461](./images/stm32/image-20241106111355461.png)
- 

#### 29.6 NOR FLASH介绍

#### 29.7 NOR FLASH基本驱动步骤

#### 29.8 编程实战

### 30. RS485

- ![image-20241024150022464](./images/stm32/image-20241024150022464.png)
- RS485是串行通信的一种接口标准，接口标准就是逻辑1和逻辑0的电平表示不同

#### 30.1 RS485介绍

- ![image-20241024150246362](./images/stm32/image-20241024150246362.png)
  - RS485使用两根差分信号线传输，差分就是两根信号线的电压差是传输的信号量
  - RS485接口电平低，不易损伤芯片，且传输效率高，传输距离远，支持多节点传输
- RS485总线连接图
  - ![image-20241024150522562](./images/stm32/image-20241024150522562.png)
  - 各节点之间都是A点对A点，B点对B点，使用AB双绞线连接各节点
  - AB两线之间的匹配电阻作用：确保RS485总线的稳定性，抑制噪声
- ![image-20241024151142780](./images/stm32/image-20241024151142780.png)
  - 接收器输出端RO接串口的输入
    - 输入信号传输方向：RO——>UART RX，即接收器的输出端输出的内容是另一个主板传输过来的内容，需要再给到串口的输入，然后主板才会收到另一个主板发来的数据
    - 输出信号传输方向：UART TX——>DI，串口的输出接驱动器的输入，然后信号通过AB线差分信号传输到另一块主板
  - DI驱动器输入端，接收的是主板要发送的信号
  - RO接收器输出端，输出的是主板接收的信号
- RS485通信波形图
  - ![image-20241024151226290](./images/stm32/image-20241024151226290.png)
- 

#### 30.2 RS485相关HAL库驱动介绍

- ![image-20241024151252324](./images/stm32/image-20241024151252324.png)
- HAL库驱动函数同USART基本一致

#### 30.3 RS485配置步骤

- ![image-20241024152156126](./images/stm32/image-20241024152156126.png)

#### 30.4 编程实战

- 目前无法实现，需要两块开发板，先搁置

### 31. CAN

#### 31.1 CAN基础知识介绍

##### 31.1.1 CAN介绍

###### 31.1.1.1 什么是CAN

- ![image-20241024160859666](./images/stm32/image-20241024160859666.png)
  - CAN就是一种ISO国际标准化的串行通信协议
  - CAN通信分为两种
    - 低速CAN，开环总线，最多可挂载20个设备节点
    - 高速CAN，闭环总线，最多可挂载30个设备节点
    - ![image-20241024161047481](./images/stm32/image-20241024161047481.png)

###### 31.1.1.2 CAN总线特点

- ![image-20241024161214901](./images/stm32/image-20241024161214901.png)

###### 31.1.1.3 CAN应用场景

- ![image-20241024161230573](./images/stm32/image-20241024161230573.png)

##### 31.1.2 CAN物理层

###### 31.1.2.1 CAN物理层特性

- ![image-20241024161524588](./images/stm32/image-20241024161524588.png)
- 显性电平：CAN_H>CAN_L，对应的是逻辑电平0
- 隐性电平：CAN_H<CAN_L，对应的是逻辑电平1

###### 31.1.2.2 CAN收发器芯片介绍

- ![image-20241024161901117](./images/stm32/image-20241024161901117.png)

##### 31.1.3 CAN协议层

###### 31.1.3.1 CAN帧种类介绍

- ![image-20241024171705351](./images/stm32/image-20241024171705351.png)

###### 31.1.3.2 CAN数据帧介绍

- ![image-20241024171740742](./images/stm32/image-20241024171740742.png)

###### 31.1.3.3 CAN位时序介绍

- ![image-20241024173441751](./images/stm32/image-20241024173441751.png)
  - 节点是接收端，接收到发送端总线上的信号的跳变在SS段范围内，表示节点和总线的时序是同步的，此时采样点的电平即该位的电平
  - ![image-20241024173729865](./images/stm32/image-20241024173729865.png)
    - 节点检测到的总线跳变不在自己的SS段内时，会通过硬件同步的方式，将自己的SS段平移到检测的边沿的地方，获得同步
  - 再同步
    - ![image-20241024174142498](./images/stm32/image-20241024174142498.png)

###### 31.1.3.4 CAN总线仲裁

- ![image-20241024174342541](./images/stm32/image-20241024174342541.png)
- CAN总线空闲时，最先向CAN总线发送信息的单元获得发送权，同时后续有其他单元也可以向CAN发送信息，但是连续输出显性电平最多的单元可以继续发送，即首先出现隐性电平的单元失去对总线的占有权即发送权而变为接收
  - 显性电平：对应逻辑0，即单元电平与总线电平的差值，是0就是显性电平，否则就是隐性电平

#### 31.2 STM32 CAN控制器介绍

##### 31.2.1 CAN控制器介绍

##### 31.2.2 CAN控制器模式

##### 31.2.3 CAN控制器框图

##### 31.2.4 CAN控制器位时序

#### 31.3 CAN相关寄存器介绍

#### 31.4 CAN相关HAL库驱动介绍

#### 31.5 CAN基本驱动步骤

#### 31.6 编程实战

