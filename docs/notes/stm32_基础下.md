# STM32入门-下

## 1. 触摸屏

### 1.1 触摸屏介绍

- 触摸屏：又称触控面板，就是将触摸的位置转换为坐标数据的输入设备
- 触摸屏本质上与液晶屏是分离的，触摸屏在上。触摸屏负责检测触摸点，液晶屏负责显示。
- 触摸屏按工作原理和传输介质可分为：红外线式、表面式声波、电阻式和电容式
- 触摸屏分类
  - ![image-20241101091807954](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101091807954.png)
  - ![image-20241101092031425](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101092031425.png)

### 1.2 触摸屏原理介绍

#### 1.2.1 电阻触摸屏原理

- ![image-20241101092823804](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101092823804.png)
  - X- 与 X+在内层ITO，Y- 与 Y+在外层ITO
  - 获取坐标原理，通过获取触摸点的X轴电压和Y轴电压，获取触摸点的XY电压与总电压的比例，从而可以获取(x,y)坐标对应的比例

#### 1.2.2 电容触摸屏原理

- ![image-20241101093534590](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101093534590.png)
  - 利用充电时间检测电容大小，进而通过检测出的电容值变化来获取触摸坐标
  - 手指触摸的位置，电流会流向手指方向，导致触摸点的电容值变小

### 1.3 触摸IC介绍

- ![image-20241101093950336](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101093950336.png)

#### 1.3.1 电阻式 

- ![image-20241101095613839](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101095613839.png)

  - 这里的XPT2046是一块电阻触摸IC芯片，是从机
    - DIN：等于主机发送从机接收，即MOSI发送的信号在DIN口输入
    - DOUT：等于从机发送主机接收，即MISO，DOUT端发送数据，主机接收

- ![image-20241101095703410](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101095703410.png)

  - 看图写时序

  - ```c
    /* MCU向XTP2046发送命令，XTP2046返回数据 */
    uint16_t tp_write_and_read_ad(uint8_t cmd_data)
    {
        uint16_t rd_data = 0;
        
        /* 开始状态 */
        T_CLK(0);
        T_MOSI(0);
        T_CS(0);    /* 选中触摸IC */
        
        /* MCU向XTP2046发送数据 */
        for(uint8_t i = 0; i < 8; i++ )
        {
            T_CLK(0);   /* MCU开始准备数据 */
            
            if (cmd_data & 0x80)    /* 数据要取最高位发送，MSB */
            {
                T_MOSI(1);
            }
            else
            {
                T_MOSI(0);
            }
            HAL_Delay(1);    /* MCU准备数据完成 */
            
            T_CLK(1);       /* MCU发送数据，XTP2046开始读取 */
            HAL_Delay(1);    /* XTP2046读取数据完成 */
            
            cmd_data <<= 1; /* 将次高位变为最高位，用于下次取最高位 */
        }
        
        /* 过滤忙信号 */
        T_CLK(0);
        HAL_Delay(1);
        T_CLK(1);
        HAL_Delay(1);
        
        /* MCU读取XTP2046返回数据 */
        for(uint8_t i = 0; i < 16; i++ )
        {
            T_CLK(0);   /* XTP2046开始准备数据 */
            HAL_Delay(1);
            T_CLK(1);   /* MCU开始读取数据 */
            
            rd_data <<= 1;  /* 空出最低位用来保存读取到的数据 */
            rd_data |= T_MISO;  /* MCU读取数据 */
            HAL_Delay(1);
        }
    
        /* 结束状态 */    
        T_CLK(0);   /* 完整的周期 */
        T_CS(1);    /* 取消选中触摸IC */
        
        return  (rd_data >>= 4);
    }
    ```

    

- ![image-20241101095947028](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101095947028.png)

- 如何做到电阻触摸屏的精确触摸

  - ![image-20241101100240078](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101100240078.png)

- 电阻触摸屏触摸校准

  - ![image-20241101101036515](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101101036515.png)

#### 1.3.2 电容式

- ![image-20241101101109316](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101101109316-17304270700021.png)
- ![image-20241101101151193](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101101151193.png)
- ![image-20241101101221019](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101101221019.png)
- ![image-20241101101321207](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101101321207.png)
  - ![image-20241101101628645](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101101628645.png)
  - ![image-20241101101640890](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101101640890.png)
  - ![image-20241101101655583](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101101655583.png)
- ![image-20241101101825723](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101101825723.png)

### 1.4 触摸屏驱动步骤

- ![image-20241101102433469](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101102433469.png)

### 1.5 编程实战

- ![image-20241101102652660](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101102652660.png)

#### 1.5.1 电阻式触摸屏编程

- 我的是电阻触摸屏

- GPIO配置，使用的是SPI协议

  - ![image-20241101170837469](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241101170837469.png)
  - 使用的是原理图中的这五个GPIO：
    - T_MISO和T_PEN配置上拉输入
    - T_SCK、T_MOSI、T_CS配置为推挽上拉输出

  - touch.c

    - 主要根据通讯时序，编写MCU与STP4096的读写函数

    - ```c
      #include "stdio.h"
      #include "stdlib.h"
      #include "lcd.h"
      #include "touch.h"
      
      
      /* MCU向XTP2046发送命令，XTP2046返回数据 */
      uint16_t tp_write_and_read_ad(uint8_t cmd)
      {
          uint16_t read_data = 0;   //存储读取到的XTP2046返回的数据
          /* XTP2046 起始状态 */
          T_CS(0);      //片选拉低
          T_CLK(0);
          T_MOSI(0);
          /* MCU向IC发送命令，1字节命令，按位循环发送，高位先发 */
          for(uint8_t i = 0;i < 8;i++)
          {
              T_CLK(0);
              if(cmd & 0x80)    //取最高位值，若是1则发送1，反之发送0
              {
                  T_MOSI(1);
              }
              else
              {
                  T_MOSI(0);
              }
              HAL_Delay(1);     //等待数据传输完成
              T_CLK(1);         //MCU将数据发送给IC
              HAL_Delay(1); 
              cmd <<= 1;         //cmd左移一位，保证下一次发送次高位
          }
          /* 清除空闲，需要一个时钟周期 */
          T_CLK(0);
          HAL_Delay(1);
          T_CLK(1);
          HAL_Delay(1);
          /* MCU从IC获取AD值 */
          for(uint8_t i = 0;i < 16;i++)
          {
              T_CLK(0);
              HAL_Delay(1);
              T_CLK(1);
              read_data <<= 1;
              read_data |= T_MISO;
              HAL_Delay(1);
          }
          /* 结束信号 */
          T_CLK(0);
          T_CS(1);
          return (read_data >> 4);
      }
      
      ```

    - touch.h

      - ```c
        
        #ifndef __TOUCH_H__
        #define __TOUCH_H__
        
        #include "main.h"
        
        /******************************************************************************************/
        
        /* 电阻触摸屏控制引脚 */
        #define T_PEN           HAL_GPIO_ReadPin(GPIOF, T_PEN_Pin)           /* T_PEN */
        #define T_MISO          HAL_GPIO_ReadPin(GPIOB, T_MISO_Pin)         /* T_MISO */
        
        #define T_MOSI(x)     do{ x ? \
                                  HAL_GPIO_WritePin(GPIOF, T_MOSI_Pin, GPIO_PIN_SET) : \
                                  HAL_GPIO_WritePin(GPIOF, T_MOSI_Pin, GPIO_PIN_RESET); \
                              }while(0)     /* T_MOSI */
        
        #define T_CLK(x)      do{ x ? \
                                  HAL_GPIO_WritePin(GPIOB, T_SCK_Pin, GPIO_PIN_SET) : \
                                  HAL_GPIO_WritePin(GPIOB, T_SCK_Pin, GPIO_PIN_RESET); \
                              }while(0)     /* T_CLK */
        
        #define T_CS(x)       do{ x ? \
                                  HAL_GPIO_WritePin(GPIOF, T_CS_Pin, GPIO_PIN_SET) : \
                                  HAL_GPIO_WritePin(GPIOF, T_CS_Pin, GPIO_PIN_RESET); \
                              }while(0)     /* T_CS */
        
        
        /* 电阻屏函数 */
        /* MCU向XTP2046发送命令，XTP2046返回数据 */
        uint16_t tp_write_and_read_ad(uint8_t cmd);
        
        
        
        
        #endif
        
        
        ```

        

#### 1.5.2 电容式触摸屏编程

- 没有设备，忽略

## 2. 红外遥控实验

### 2.1 红外遥控介绍

- ![image-20241104094704120](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104094704120.png)
  - 红外遥控就是一种无线的、非接触型的控制技术，具有成本低、功耗低、传输信息可靠等优点
    - 红外遥控通信需要具备红外发射头和红外接收头两个设备
- 太阳光光谱图
  - ![image-20241104095012600](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104095012600.png)
  - 人类可见光线只有：红橙黄绿青蓝紫

- ![image-20241104095220404](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104095220404.png)
  - 发射器和接收器要波长和频率一致才可建立通信
- ![image-20241104095449655](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104095449655.png)

### 2.2 红外编码协议介绍

- ![image-20241104095707521](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104095707521-17306854283543.png)
- 接收器输出逻辑0和逻辑1的时序
  - ![image-20241104095935121](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104095935121.png)
- ![image-20241104100419073](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104100419073.png)
  - 地址码、地址反码、控制码、控制反码都是低位先发，LSB型，读数据的时候要倒着读，反码的作用是校验，提高数据的准确性
  - 例如上协议中：
    - 控制码：    00010101 = 0x15
    - 控制反码：11101010 = 0xEA

### 2.3 编程实战

- 驱动步骤
  - ![image-20241104111421343](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104111421343.png)

- 实现步骤原理

  - ![image-20241104111508648](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104111508648.png)

  - remote.c

    - ```c
      
      
      #include "./BSP/REMOTE/remote.h"
      #include "./SYSTEM/delay/delay.h"
      #include "./SYSTEM/usart/usart.h"
      
      
      TIM_HandleTypeDef g_tim4_handle;      /* 定时器4句柄 */
      
      
      /**
       * @brief       红外遥控初始化
       *   @note      设置IO以及定时器的输入捕获
       * @param       无
       * @retval      无
       */
      void remote_init(void)
      {
          TIM_IC_InitTypeDef tim_ic_init_handle;
      
          g_tim4_handle.Instance = REMOTE_IN_TIMX;                    /* 通用定时器4 */
          g_tim4_handle.Init.Prescaler = (72-1);                      /* 预分频器,1M的计数频率,1us加1 */
          g_tim4_handle.Init.CounterMode = TIM_COUNTERMODE_UP;        /* 向上计数器 */
          g_tim4_handle.Init.Period = 10000;                          /* 自动装载值 */
          g_tim4_handle.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
          HAL_TIM_IC_Init(&g_tim4_handle);
          
          /* 初始化TIM4输入捕获参数 */
          tim_ic_init_handle.ICPolarity = TIM_ICPOLARITY_RISING;      /* 上升沿捕获 */
          tim_ic_init_handle.ICSelection = TIM_ICSELECTION_DIRECTTI;  /* 映射到TI4上 */
          tim_ic_init_handle.ICPrescaler = TIM_ICPSC_DIV1;            /* 配置输入分频，不分频 */
          tim_ic_init_handle.ICFilter = 0x03;                         /* IC1F=0003 8个定时器时钟周期滤波 */
          HAL_TIM_IC_ConfigChannel(&g_tim4_handle, &tim_ic_init_handle, REMOTE_IN_TIMX_CHY);/* 配置TIM4通道4 */
          HAL_TIM_IC_Start_IT(&g_tim4_handle, REMOTE_IN_TIMX_CHY);    /* 开始捕获TIM的通道值 */
          __HAL_TIM_ENABLE_IT(&g_tim4_handle, TIM_IT_UPDATE);         /* 使能更新中断 */
      }
      
      /**
       * @brief       定时器4底层驱动，时钟使能，引脚配置
       * @param       htim:定时器句柄
       * @note        此函数会被HAL_TIM_IC_Init()调用
       * @retval      无
       */
      void HAL_TIM_IC_MspInit(TIM_HandleTypeDef *htim)
      {
          if(htim->Instance == REMOTE_IN_TIMX)
          {
              GPIO_InitTypeDef gpio_init_struct;
              
              REMOTE_IN_GPIO_CLK_ENABLE();            /* 红外接入引脚GPIO时钟使能 */
              REMOTE_IN_TIMX_CHY_CLK_ENABLE();        /* 定时器时钟使能 */
              __HAL_AFIO_REMAP_TIM4_DISABLE();        /* 这里用的是PB9/TIM4_CH4，参考AFIO_MAPR寄存器的设置 */
              
              gpio_init_struct.Pin = REMOTE_IN_GPIO_PIN;
              gpio_init_struct.Mode = GPIO_MODE_AF_INPUT;             /* 复用输入 */
              gpio_init_struct.Pull = GPIO_PULLUP;                    /* 上拉 */
              gpio_init_struct.Speed = GPIO_SPEED_FREQ_HIGH;          /* 高速 */
              HAL_GPIO_Init(REMOTE_IN_GPIO_PORT, &gpio_init_struct);  /* 初始化定时器通道引脚 */
      
              HAL_NVIC_SetPriority(REMOTE_IN_TIMX_IRQn, 1, 3);        /* 设置中断优先级，抢占优先级1，子优先级3 */
              HAL_NVIC_EnableIRQ(REMOTE_IN_TIMX_IRQn);                /* 开启ITM4中断 */
          }
      }
      
      /* 遥控器接收状态
       * [7]  : 收到了引导码标志
       * [6]  : 得到了一个按键的所有信息
       * [5]  : 保留
       * [4]  : 标记上升沿是否已经被捕获
       * [3:0]: 溢出计时器
       */
      uint8_t g_remote_sta = 0;
      uint32_t g_remote_data = 0; /* 红外接收到的数据 */
      uint8_t  g_remote_cnt = 0;  /* 按键按下的次数 */
      
      /**
       * @brief       定时器4中断服务函数
       * @param       无
       * @retval      无
       */
      void REMOTE_IN_TIMX_IRQHandler(void)
      {
          HAL_TIM_IRQHandler(&g_tim4_handle); /* 定时器共用处理函数 */
      }
      
      /**
       * @brief       定时器溢出中断服务函数
       * @param       htim:定时器句柄
       * @retval      无
       */
      void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
      {
          if (htim->Instance == REMOTE_IN_TIMX)
          {
              if (g_remote_sta & 0x80)      /* 上次有数据被接收到了 */
              {
                  g_remote_sta &= ~0X10;    /* 取消上升沿已经被捕获标记 */
      
                  if ((g_remote_sta & 0X0F) == 0X00)
                  {
                      g_remote_sta |= 1 << 6; /* 标记已经完成一次按键的键值信息采集 */
                  }
                  
                  if ((g_remote_sta & 0X0F) < 14)           /* 110ms */
                  {
                      g_remote_sta++;
                  }
                  else
                  {
                      g_remote_sta &= ~(1 << 7);    /* 清空引导标识 */
                      g_remote_sta &= 0XF0;         /* 清空计数器 */
                  }
              }
          }
      }
      
      /**
       * @brief       定时器输入捕获中断回调函数
       * @param       htim:定时器句柄
       * @retval      无
       */
      void HAL_TIM_IC_CaptureCallback(TIM_HandleTypeDef *htim)
      {
          if (htim->Instance == REMOTE_IN_TIMX)
          {
              uint16_t dval;  /* 下降沿时计数器的值 */
              
              if (RDATA)      /* 上升沿捕获 */
              {
                  __HAL_TIM_SET_CAPTUREPOLARITY(&g_tim4_handle, REMOTE_IN_TIMX_CHY, TIM_INPUTCHANNELPOLARITY_FALLING);    /* CC4P=1 设置为下降沿捕获 */
                  __HAL_TIM_SET_COUNTER(&g_tim4_handle, 0);  /* 清空定时器值 */
                  g_remote_sta |= 0X10;                      /* 标记上升沿已经被捕获 */
              }
              else           /* 下降沿捕获 */
              {
                  dval = HAL_TIM_ReadCapturedValue(&g_tim4_handle, REMOTE_IN_TIMX_CHY);   /* 读取CCR1也可以清CC1IF标志位 */
                  __HAL_TIM_SET_CAPTUREPOLARITY(&g_tim4_handle, REMOTE_IN_TIMX_CHY, TIM_INPUTCHANNELPOLARITY_RISING); /* 配置TIM4通道4上升沿捕获 */
      
                  if (g_remote_sta & 0X10)        /* 完成一次高电平捕获 */
                  {
                      if (g_remote_sta & 0X80)    /* 接收到了引导码 */
                      {
                          if (dval > 300 && dval < 800)   /* 560为标准值,560us */
                          {
                              g_remote_data >>= 1;            /* 右移一位，新增最高位，然后对其进行赋值，下一次再右移，最高位变成次高位，然后给新的最高位赋值，直到右移32次，就会获得一个32位的数据，并且接收的低位就在低位，高位就在高位 */
                              g_remote_data &= ~(0x80000000); /* 接收到0 */
                          }
                          else if (dval > 1400 && dval < 1800)    /* 1680为标准值,1680us */
                          {
                              g_remote_data >>= 1;            /* 右移一位 */
                              g_remote_data |= 0x80000000;    /* 接收到1 */
                          }
                          else if (dval > 2000 && dval < 3000)    /* 得到按键键值增加的信息 2250为标准值2.25ms */
                          {
                              g_remote_cnt++;         /* 按键次数增加1次 */
                              g_remote_sta &= 0XF0;   /* 清空计时器 */
                          }
                      }
                      else if (dval > 4200 && dval < 4700)    /* 4500为标准值4.5ms */
                      {
                          g_remote_sta |= 1 << 7; /* 标记成功接收到了引导码 */
                          g_remote_cnt = 0;       /* 清除按键次数计数器 */
                      }
                  }
      
                  g_remote_sta &= ~(1 << 4);
              }
          }
      }
      
      /**
       * @brief       处理红外按键(类似按键扫描)
       * @param       无
       * @retval      0   , 没有任何按键按下
       *              其他, 按下的按键键值
       */
      uint8_t remote_scan(void)
      {
          uint8_t sta = 0;
          uint8_t t1, t2;
      
          if (g_remote_sta & (1 << 6))    /* 得到一个按键的所有信息了 */
          {
              t1 = g_remote_data;                 /* 得到地址码 */
              t2 = (g_remote_data >> 8) & 0xff;   /* 得到地址反码 */
      
              if ((t1 == (uint8_t)~t2) && t1 == REMOTE_ID)    /* 检验遥控识别码(ID)及地址 */
              {
                  t1 = (g_remote_data >> 16) & 0xff;
                  t2 = (g_remote_data >> 24) & 0xff;
      
                  if (t1 == (uint8_t)~t2)
                  {
                      sta = t1;           /* 键值正确 */
                  }
              }
      
              if ((sta == 0) || ((g_remote_sta & 0X80) == 0)) /* 按键数据错误/遥控已经没有按下了 */
              {
                  g_remote_sta &= ~(1 << 6);  /* 清除接收到有效按键标识 */
                  g_remote_cnt = 0;           /* 清除按键次数计数器 */
              }
          }
      
          return sta;
      }
      
      
      ```

      

### 2.4 课堂总结

## 3. 游戏手柄实验

### 3.1 游戏手柄简介

- ![image-20241104113651592](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104113651592.png)
- ![image-20241104113838960](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104113838960.png)
  - 有5跟信号线，LATCH、CLOCK推挽输出，DATA输入
  - 时钟下降沿采集，DATA低电平有效
- 

### 3.2 FC手柄时序介绍

- ![image-20241104114119127](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104114119127.png)
  - DATA低电平有效，按键按下，对应键值赋1即可

### 3.3 编程实战

- 战舰版原理图部分
  - ![image-20241104114310290](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104114310290.png)

- joypad.c

  - ```c
    
    #include "./BSP/JOYPAD/joypad.h"
    
    /**
     * @brief       初始化手柄接口
     * @param       无
     * @retval      无
     */
    void joypad_init(void)
    {
        JOYPAD_CLK_GPIO_CLK_ENABLE();  /* CLK   所在IO时钟初始化 */
        JOYPAD_LAT_GPIO_CLK_ENABLE();  /* LATCH 所在IO时钟初始化 */
        JOYPAD_DATA_GPIO_CLK_ENABLE(); /* DATA  所在IO时钟初始化 */
    
        GPIO_InitTypeDef gpio_init_struct;
        gpio_init_struct.Pin = JOYPAD_CLK_GPIO_PIN;
        gpio_init_struct.Mode = GPIO_MODE_OUTPUT_PP;
        gpio_init_struct.Pull = GPIO_PULLUP;
        gpio_init_struct.Speed = GPIO_SPEED_FREQ_MEDIUM;
        HAL_GPIO_Init(JOYPAD_CLK_GPIO_PORT, &gpio_init_struct); /* JOYPAD_CLK  引脚模式设置 */
    
        gpio_init_struct.Pin = JOYPAD_LAT_GPIO_PIN;
        HAL_GPIO_Init(JOYPAD_LAT_GPIO_PORT, &gpio_init_struct); /* JOYPAD_LAT  引脚模式设置 */
    
        gpio_init_struct.Pin = JOYPAD_DATA_GPIO_PIN;
        gpio_init_struct.Mode = GPIO_MODE_INPUT;
        gpio_init_struct.Pull = GPIO_PULLUP;
        gpio_init_struct.Speed = GPIO_SPEED_FREQ_MEDIUM;
        HAL_GPIO_Init(JOYPAD_DATA_GPIO_PORT, &gpio_init_struct); /* JOYPAD_DATA 引脚模式设置 */
    }
    
    /**
     * @brief       手柄延迟函数
     * @param       t    : 要延时的时间
     * @retval      无
     */
    static void joypad_delay(uint16_t t)
    {
        while (t--);
    }
    
    /**
     * @brief       读取手柄按键值
     *   @note      FC手柄数据输出格式:
     *              每给一个脉冲,输出一位数据,输出顺序:
     *              A -> B -> SELECT -> START -> UP -> DOWN -> LEFT -> RIGHT.
     *              总共8位, 对于有C按钮的手柄, 按下C其实就等于 A + B 同时按下.
     *              按下是1,松开是0.
     * @param       无
     * @retval      按键结果, 格式如下:
     *              [7]:右
     *              [6]:左
     *              [5]:下
     *              [4]:上
     *              [3]:Start
     *              [2]:Select
     *              [1]:B
     *              [0]:A
     */
    uint8_t joypad_read(void)
    {
        volatile uint8_t temp = 0;
        uint8_t t;
        JOYPAD_LAT(1);          /* 锁存当前状态 */
        joypad_delay(80);
        JOYPAD_LAT(0);
    
        for (t = 0; t < 8; t++) /* 移位输出数据 */
        {
            temp >>= 1;
    
            if (JOYPAD_DATA == 0)
            {
                temp |= 0x80;   /* LOAD之后，就得到第一个数据 */
            }
    
            JOYPAD_CLK(1);      /* 每给一次脉冲，收到一个数据 */
            joypad_delay(80);
            JOYPAD_CLK(0);
            joypad_delay(80);
        }
    
        return temp;
    }
    
    ```

- joypad.h

  - ```c
    
    
    #ifndef __JOYPAD_H
    #define __JOYPAD_H
    
    #include "./SYSTEM/sys/sys.h"
    
    /******************************************************************************************/
    /* 引脚 定义 */
    
    #define JOYPAD_CLK_GPIO_PORT            GPIOD
    #define JOYPAD_CLK_GPIO_PIN             GPIO_PIN_3
    #define JOYPAD_CLK_GPIO_CLK_ENABLE()    do{ __HAL_RCC_GPIOD_CLK_ENABLE(); }while(0)   /* PD口时钟使能 */
    
    #define JOYPAD_LAT_GPIO_PORT            GPIOB
    #define JOYPAD_LAT_GPIO_PIN             GPIO_PIN_11
    #define JOYPAD_LAT_GPIO_CLK_ENABLE()    do{ __HAL_RCC_GPIOB_CLK_ENABLE(); }while(0)    /* PB口时钟使能 */
    
    #define JOYPAD_DATA_GPIO_PORT           GPIOB
    #define JOYPAD_DATA_GPIO_PIN            GPIO_PIN_10
    #define JOYPAD_DATA_GPIO_CLK_ENABLE()   do{ __HAL_RCC_GPIOB_CLK_ENABLE(); }while(0)    /* PB口时钟使能 */
    
    /******************************************************************************************/
    
    /* 手柄连接引脚 */
    #define JOYPAD_CLK(x)   do{ x ? \
                                  HAL_GPIO_WritePin(JOYPAD_CLK_GPIO_PORT, JOYPAD_CLK_GPIO_PIN, GPIO_PIN_SET) : \
                                  HAL_GPIO_WritePin(JOYPAD_CLK_GPIO_PORT, JOYPAD_CLK_GPIO_PIN, GPIO_PIN_RESET); \
                            }while(0)   /* JOYPAD_CLK */
    
    #define JOYPAD_LAT(x)   do{ x ? \
                                  HAL_GPIO_WritePin(JOYPAD_LAT_GPIO_PORT, JOYPAD_LAT_GPIO_PIN, GPIO_PIN_SET) : \
                                  HAL_GPIO_WritePin(JOYPAD_LAT_GPIO_PORT, JOYPAD_LAT_GPIO_PIN, GPIO_PIN_RESET); \
                            }while(0)   /* JOYPAD_LATCH */
    
    #define JOYPAD_DATA     HAL_GPIO_ReadPin(JOYPAD_DATA_GPIO_PORT, JOYPAD_DATA_GPIO_PIN)   /* JOYPAD_DATA */
    
    
    /* 静态函数 */
    static void joypad_delay(uint16_t t);   /* JOYPAD 延时 */
    
    /* 接口函数 */
    void joypad_init(void);     /* JOYPAD 初始化 */
    uint8_t joypad_read(void);  /* JOYPAD 读取数据 */
    
    #endif
    
    
    ```

- main.c

  - ```c
    
    
    #include "./SYSTEM/sys/sys.h"
    #include "./SYSTEM/usart/usart.h"
    #include "./SYSTEM/delay/delay.h"
    #include "./USMART/usmart.h"
    #include "./BSP/LED/led.h"
    #include "./BSP/LCD/lcd.h"
    #include "./BSP/KEY/key.h"
    #include "./BSP/JOYPAD/joypad.h"
    
    /* 手柄按键符号定义 */
    const char *JOYPAD_SYMBOL_TBL[8] = {"Right", "Left", "Down", "Up", "Start", "Select", "B", "A"};
    
    int main(void)
    {
        uint8_t key;
        uint8_t t = 0, i = 0;
    
        HAL_Init();                         /* 初始化HAL库 */
        sys_stm32_clock_init(RCC_PLL_MUL9); /* 设置时钟, 72Mhz */
        delay_init(72);                     /* 延时初始化 */
        usart_init(115200);                 /* 串口初始化为115200 */
        led_init();                         /* 初始化LED */
        lcd_init();                         /* 初始化LCD */
        joypad_init();                      /* 游戏手柄接口初始化 */
    
        lcd_show_string(30,  50, 200, 16, 16, "STM32", RED);
        lcd_show_string(30,  70, 200, 16, 16, "JOYPAD TEST", RED);
        lcd_show_string(30,  90, 200, 16, 16, "ATOM@ALIENTEK", RED);
        lcd_show_string(30, 110, 200, 16, 16, "KEYVAL:", RED);
        lcd_show_string(30, 130, 200, 16, 16, "SYMBOL:", RED);
    
        while (1)
        {
            key = joypad_read();
    
            if (key) /* 手柄 有按键按下 */
            {
                lcd_show_num(86, 110, key, 3, 16, BLUE); /* 显示键值 */
    
                for (i = 0; i < 8; i++)
                {
                    if (key & (0X80 >> i))    //key是1的话，对应键值获取并显示，0就跳过
                    {
                        lcd_fill(30 + 56, 130, 30 + 56 + 48, 130 + 16, WHITE);                          /* 清除之前的显示 */
                        lcd_show_string(30 + 56, 130, 200, 16, 16, (char *)JOYPAD_SYMBOL_TBL[i], BLUE); /* 显示符号 */
                    }
                }
            }
    
            delay_ms(10);
            t++;
    
            if (t == 20)
            {
                t = 0;
                LED0_TOGGLE(); /* LED0闪烁 */
            }
        }
    }
    ```

    

## 4. 单总线-DHT11

- DHT11：温湿度传感器

### 4.1 DHT11介绍

-  ![image-20241104160139525](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104160139525.png)
- ![image-20241104160151095](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104160151095.png)
- ![image-20241104160222350](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104160222350.png)
- 

### 4.2 DHT11工作时序

- ![image-20241104161536876](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104161536876.png)

- ![image-20241104161602560](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104161602560.png)

- ![image-20241104161812229](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104161812229.png)

- ![image-20241104161826154](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104161826154.png)

  - ![image-20241104161851312](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104161851312.png)

- ![image-20241104161917731](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104161917731.png)

  

### 4.3 DHT11基本操作步骤

- ![image-20241104163443956](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104163443956.png)
- 

### 4.4 编程实战

- ![image-20241104163904939](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104163904939.png)

- dht11.c

  - ```c
    
    
    #include "./BSP/DHT11/dht11.h"
    #include "./SYSTEM/delay/delay.h"
    #include "./SYSTEM/usart/usart.h"
    
    /**
     * @brief       初始化DHT11的IO口 DHT11_DQ 同时检测DHT11的存在
     * @param       无
     * @retval      0, 正常
     *              1, 不存在/不正常
     */
    void dht11_init(void)
    {
        GPIO_InitTypeDef gpio_init_struct;
    
        DHT11_DQ_GPIO_CLK_ENABLE();     /* 开启DHT11_DQ引脚时钟 */
    
        gpio_init_struct.Pin = DHT11_DQ_GPIO_PIN;
        gpio_init_struct.Mode = GPIO_MODE_OUTPUT_OD;            /* 开漏输出 */
        gpio_init_struct.Pull = GPIO_PULLUP;                    /* 上拉 */
        gpio_init_struct.Speed = GPIO_SPEED_FREQ_HIGH;          /* 高速 */
        HAL_GPIO_Init(DHT11_DQ_GPIO_PORT, &gpio_init_struct);   /* 初始化DHT11_DHT11_DQ引脚 */
        /* DHT11_DHT11_DQ引脚模式设置,开漏输出,上拉, 这样就不用再设置IO方向了, 开漏输出的时候(=1), 也可以读取外部信号的高低电平 */
    }
    
    
    void dht11_reset(void)
    {
        DHT11_DQ_OUT(0);    /* 拉低DHT11_DQ */
        delay_ms(20);       /* 拉低至少18ms*/
        DHT11_DQ_OUT(1);    /* DHT11_DQ=1 */
        delay_us(13);       /* 主机拉高10~35us */
    }
    
    uint8_t dht11_check(void)
    {
        uint8_t retry, rval = 0;
    
        while (DHT11_DQ_IN && retry < 100)
        {
            retry++;
            delay_us(1);
        }       /* DHT11会拉低数据线约83us*/
    
        if (retry >= 100)   /* 超时 */
            rval = 1;
        else
        {
            retry = 0;
    
            while (! DHT11_DQ_IN && retry < 100)
            {
                retry++;
                delay_us(1);
            }   /* DHT11拉低后会再次拉高约87us*/
    
            if (retry >= 100)
                rval = 1;   /* 超时 */
        }
    
        return rval;
    }
    
    uint8_t dht11_read_bit(void)
    {
        uint8_t retry = 0;  /* 定义变量 */
    
        while (DHT11_DQ_IN && retry < 100)
        {
            retry++;
            delay_us(1);
        }   /* 等待变为低电平 */
    
        retry = 0;
    
        while (! DHT11_DQ_IN && retry < 100)
        {
            retry++;
            delay_us(1);
        }   /* 等待变为高电平*/
    
        delay_us(40);   /* 等待40us */
    
        if (DHT11_DQ_IN)
            return 1;
        else
            return 0;
    }
    
    uint8_t dht11_read_byte(void)
    {
        uint8_t i, data = 0;
    
        for (i = 0; i < 8; i++)
        {
            /* 高位数据先输出，先左移一位 */
            data <<= 1;
            /* 读取1bit数据*/
            data |= dht11_read_bit();
        }
    
        return data;
    }
    
    uint8_t dht11_read_data(void)   /* return 0:succeed 1:failed */
    {
        uint8_t i, buf[5] = {0};
        uint8_t t = 0;
        uint8_t h = 0;
        uint8_t ret = 1;
        
        /* 1、主机发出起始信号 */
        dht11_reset();
        
        /* 2、主机检测从机发出的响应信号 */
        dht11_check();
        
        /* 3、从机返回40bit数据，主机接收5byte数据 */
        for (i = 0; i < 5; i++)
        {
            buf[i] = dht11_read_byte();
        }
        
        /* 4、数据处理 */
        if (buf[4] == (buf[0] + buf[1] + buf[2] + buf[3]))
        {
            h = buf[0];
            t = buf[2];
            ret = 0;
        }
        
        printf("T:%d H:%d\r\n",t, h);
        
        return ret;
    }
    
    
    ```

  - dht11.h

    - ```c
      
      
      #ifndef __DHT11_H
      #define __DHT11_H 
      
      #include "./SYSTEM/sys/sys.h"
      
      /******************************************************************************************/
      /* DHT11 引脚 定义 */
      
      #define DHT11_DQ_GPIO_PORT                  GPIOG
      #define DHT11_DQ_GPIO_PIN                   GPIO_PIN_11
      #define DHT11_DQ_GPIO_CLK_ENABLE()          do{ __HAL_RCC_GPIOG_CLK_ENABLE(); }while(0)   /* PG口时钟使能 */
      
      /******************************************************************************************/
      
      /* IO操作函数 */
      #define DHT11_DQ_OUT(x)     do{ x ? \
                                      HAL_GPIO_WritePin(DHT11_DQ_GPIO_PORT, DHT11_DQ_GPIO_PIN, GPIO_PIN_SET) : \
                                      HAL_GPIO_WritePin(DHT11_DQ_GPIO_PORT, DHT11_DQ_GPIO_PIN, GPIO_PIN_RESET); \
                                  }while(0)                                                /* 数据端口输出 */
      #define DHT11_DQ_IN         HAL_GPIO_ReadPin(DHT11_DQ_GPIO_PORT, DHT11_DQ_GPIO_PIN)  /* 数据端口输入 */
      
      void dht11_init(void);
      uint8_t dht11_read_data(void);   /* return 0:succeed 1:failed */
      
      #endif
      
      
      ```

      

## 5. 单总线-DS18B20

- 温度传感器

### 5.1 DS18B20介绍

- ![image-20241104141557204](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104141557204.png)
- ![image-20241104141731531](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104141731531.png)
- ![image-20241104141747139](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104141747139.png)
- ![image-20241104141800596](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104141800596.png)

### 5.2 DS18B20工作时序

- ![image-20241104141843607](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104141843607.png)
- ![image-20241104142505560](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104142505560.png)
  - 复位脉冲
    - <img src="./images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104142534607-17307015350755.png" alt="image-20241104142534607" style="zoom:80%;" />
  - 应答脉冲
    - ![image-20241104143026045](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104143026045.png)
      - 应答脉冲有两个注意几点
        - DQ_IN检测到0，主机发送复位信号后，会释放DQ信号线，然后总线被上拉电阻拉高，等待从机应答低电平，这里最多等待200us，超时就代表从机应答失败
        - DQ_IN检测到1，从机应答0后，等待DQ变为1，最多等240us，超时也代表从机应答失败
  - 写时序
    - ![image-20241104143132682](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104143132682.png)
    - ![image-20241104143900850](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104143900850.png)
  - 读时序
    - ![image-20241104144818550](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104144818550.png)
    - ![image-20241104144853791](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104144853791.png)

### 5.3 DS18B20基本操作步骤

- <img src="./images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104145720720.png" alt="image-20241104145720720" style="zoom:67%;" />
  - 单总线上可以挂载多个从机，每个从机的操作命令都不同，类似于从机的身份证一样
- ![image-20241104150714189](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104150714189.png)
- ![image-20241104150756931](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104150756931.png)
- ![image-20241104150821142](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104150821142.png)
- ![image-20241104150849020](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104150849020.png)
  - ![image-20241104150904594](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104150904594.png)
  - 

### 5.4 编程实战

- ![image-20241104150945392](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241104150945392.png)

- ds18b20.c

  - ```c
    
    
    #include "./SYSTEM/delay/delay.h"
    #include "./BSP/DS18B20/ds18b20.h"
    
    /**
     * @brief       初始化DS18B20的IO口 DS18B20_DQ 同时检测DS18B20的存在
     * @param       无
     */
    void ds18b20_init(void)
    {
        GPIO_InitTypeDef gpio_init_struct;
    
        DS18B20_DQ_GPIO_CLK_ENABLE();   /* 开启DS18B20_DQ引脚时钟 */
    
        gpio_init_struct.Pin = DS18B20_DQ_GPIO_PIN;
        gpio_init_struct.Mode = GPIO_MODE_OUTPUT_OD;            /* 开漏输出 */
        gpio_init_struct.Pull = GPIO_PULLUP;                    /* 上拉 */
        gpio_init_struct.Speed = GPIO_SPEED_FREQ_HIGH;          /* 高速 */
        HAL_GPIO_Init(DS18B20_DQ_GPIO_PORT, &gpio_init_struct); /* 初始化DS18B20_DQ引脚 */
        /* DS18B20_DQ引脚模式设置,开漏输出,上拉, 这样就不用再设置IO方向了, 开漏输出的时候(=1), 也可以读取外部信号的高低电平 */
    }
    
    void ds18b20_reset(void)
    {
        DS18B20_DQ_OUT(0);  /* 拉低DS18B20_DQ, 复位 */
        delay_us(750);      /* 延时750us */
        DS18B20_DQ_OUT(1);  /* DS18B20_DQ=1, 释放总线 */
        delay_us(15);       /* 延时15us */
    }
    
    uint8_t  ds18b20_check(void)    /* return 0:succeed   1:fail */
    {
        uint8_t retry, rval = 0;    /* 定义变量 */
    
        while (DS18B20_DQ_IN && retry < 200)
        {
            retry++;
            delay_us(1);
        }   /* 等待DS18B20_DQ变低，等待200us*/
    
        if (retry >= 200)   /* 超时 */
        {
            rval = 1;
        }
        else
        {
            retry = 0;
    
            while (! DS18B20_DQ_IN && retry < 240)
            {
                retry++;
                delay_us(1);
            }   /* 等待DS18B20_DQ变高，等待240us */
    
            if (retry >= 240)   /* 超时 */
            {
                rval = 1;
            }
        }
    
        return rval;
    }
    
    void ds18b20_write_0(void)
    {
        DS18B20_DQ_OUT(0);  /* 拉低DS18B20_DQ */
        delay_us(60);       /* 延时60us */
        DS18B20_DQ_OUT(1);  /* 释放DS18B20_DQ */
        delay_us(2);        /* 延时2us */
    }
    
    void ds18b20_write_1(void)
    {
        DS18B20_DQ_OUT(0);  /* 拉低DS18B20_DQ */
        delay_us(2);        /* 延时2us */
        DS18B20_DQ_OUT(1);  /* 释放DS18B20_DQ */
        delay_us(60);       /* 延时60us */
    }
    
    void ds18b20_write_byte(uint8_t data)   //低位先发
    {
        uint8_t j;
    
        for (j = 0; j < 8; j++)
        {
            if (data & 0x01)
            {
                ds18b20_write_1();  /* Write 1*/
            }
            else
            {
                ds18b20_write_0();  /* Write 0*/
            }
    
            data >>= 1;     /* 右移，获取高一位数据 */
        }
    }
    
    uint8_t  ds18b20_read_bit(void)
    {
        uint8_t data = 0;
    
        DS18B20_DQ_OUT(0);  /* 拉低DS18B20_DQ */
        delay_us(2);        /* 延时2us */
        DS18B20_DQ_OUT(1);  /* 释放DS18B20_DQ */
        delay_us(12);       /* 延时12us */
    
        if (DS18B20_DQ_IN)
        {
            data = 1;
        }
        
        delay_us(50);
        return data;
    }
    
    uint8_t  ds18b20_read_byte(void)   //低位先读
    {
        uint8_t i, b, data = 0;
    
        for (i = 0; i < 8; i++)
        {
            /* 先输出低位数据 ,高位数据后输出 */
            b = ds18b20_read_bit();
            /* 填充data的每一位 */
            data |= b << i;
        }
    
        return data;
    }
    
    float ds18b20_get_temperature(void)
    {
        uint8_t TL, TH;
        uint16_t temp = 0;
        float temperature = 0;
        
        /* 1、初始化 */
        ds18b20_reset();
        ds18b20_check();
        
        /* 2、发送ROM命令 */
        ds18b20_write_byte(0xCC);
        
        /* 3、发送DS18B20操作命令，启动温度转换 */
        ds18b20_write_byte(0x44);
        
        /* 4、初始化 */
        ds18b20_reset();
        ds18b20_check();
        
        /* 5、发送ROM命令 */
        ds18b20_write_byte(0xCC);
        
        /* 6、发送DS18B20操作命令 */
        ds18b20_write_byte(0xBE);   /* 读取RAM命令 */
        
        /* 7、读取两个字节数据 */
        TL = ds18b20_read_byte();
        TH = ds18b20_read_byte();
        
        /* 8、温度数据运算 (正温度)*/
        temp = TL + (TH << 8);
        temperature = temp*0.0625;
        return temperature;
    }
    
    
    
    
    ```

- ds18b20.h

  - ```c
    
    
    #ifndef __DS18B20_H
    #define __DS18B20_H
    
    #include "./SYSTEM/sys/sys.h"
    
    
    /******************************************************************************************/
    /* DS18B20引脚 定义 */
    
    #define DS18B20_DQ_GPIO_PORT                GPIOG
    #define DS18B20_DQ_GPIO_PIN                 GPIO_PIN_11
    #define DS18B20_DQ_GPIO_CLK_ENABLE()        do{ __HAL_RCC_GPIOG_CLK_ENABLE(); }while(0)   /* PG口时钟使能 */
    
    /******************************************************************************************/
    
    /* IO操作函数 */
    #define DS18B20_DQ_OUT(x)   do{ x ? \
                                    HAL_GPIO_WritePin(DS18B20_DQ_GPIO_PORT, DS18B20_DQ_GPIO_PIN, GPIO_PIN_SET) : \
                                    HAL_GPIO_WritePin(DS18B20_DQ_GPIO_PORT, DS18B20_DQ_GPIO_PIN, GPIO_PIN_RESET); \
                                }while(0)                                                       /* 数据端口输出 */
    #define DS18B20_DQ_IN       HAL_GPIO_ReadPin(DS18B20_DQ_GPIO_PORT, DS18B20_DQ_GPIO_PIN)     /* 数据端口输入 */
    
    void ds18b20_init(void);
    float ds18b20_get_temperature(void);
    
    #endif
    
    ```

- main.c

   - ```c
     
     
     #include "./SYSTEM/sys/sys.h"
     #include "./SYSTEM/usart/usart.h"
     #include "./SYSTEM/delay/delay.h"
     #include "./USMART/usmart.h"
     #include "./BSP/LED/led.h"
     #include "./BSP/LCD/lcd.h"
     #include "./BSP/KEY/key.h"
     #include "./BSP/DS18B20/ds18b20.h"
     
     
     int main(void)
     {
         uint8_t t = 0;
         float T = 0;
         
         HAL_Init();                         /* 初始化HAL库 */
         sys_stm32_clock_init(RCC_PLL_MUL9); /* 设置时钟, 72Mhz */
         delay_init(72);                     /* 延时初始化 */
         usart_init(115200);                 /* 串口初始化为115200 */
         led_init();                         /* 初始化LED */
         ds18b20_init();
         
         while (1)
         {
             if (t % 10 == 0) /* 每100ms读取一次 */
             {
                 T = ds18b20_get_temperature();
                 printf("T:%.1f \r\n", T);
             }
     
             delay_ms(20);
             t++;
     
             if (t == 20)
             {
                 t = 0;
                 LED0_TOGGLE(); /* LED0闪烁 */
             }
         }
     }
     
     ```




##   6. 无线通信

- 无线通信种类：2.4G、ble、wifi、Zigbee、3/4/5G 

### 6.1 NRF24L01介绍

- ![image-20241106093638182](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241106093638182.png)
- ![image-20241106094114462](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241106094114462.png)
- ![image-20241106094127178](images/stm32_%E5%9F%BA%E7%A1%80%E4%B8%8B/image-20241106094127178.png)
- 

### 6.2 NRF24L01工作模式

### 6.3 NRF24L01寄存器介绍

### 6.4 编程实战