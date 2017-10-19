---
title: STM32 ADC使用（基础篇）
date: 2016-07-11 17:39:14
categories: ARM学习
tags: [stm32,ADC]
---
# STM32 ADC

STM32上集成的ADC（Analog to Digital Convert）外设很强大。在以前用89C51时，还需要芯片来处理AD/DA,而现在用stm32显然方便很多。


STM32（F10x系列）有3个12位的ADC，每个ADC有12个通道。各个通道的A/D转换可以单次也可以多次，连续，扫描，间断执行。ADC的结果可以左对齐或者右对齐的方式进行存储在16位的数据寄存器中。模拟看门狗允许应用程序检测电压是否超出规定阈值。

ADC基本配置可以参考《STM32库开发实战指南》,至于在ADC转换的过程中使用注入、中断、看门狗等在进阶篇再介绍，在此只详细记录三种AD转换。

>  1. ADC单通道转换。
>  2. ADC单通道转换（DMA方式）。
>  3. ADC多通道转换（DMA方式）。



## ADC单通道转换

#### 直接贴代码叙述

```c

    void  Adc_Init(void)
        {   
            ADC_InitTypeDef ADC_InitStructure;
            GPIO_InitTypeDef GPIO_InitStructure;

            RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA |RCC_APB2Periph_ADC1, ENABLE );  
            //使能ADC1通道时钟，并使能接受ADC转换的GPIO
            RCC_ADCCLKConfig(RCC_PCLK2_Div6);   
            //设置ADC分频因子6 72M/6=12,ADC最大时间不能超过14M，（可设置的分频系数为2、4、6、8）

             //PA1 作为模拟通道输入引脚                         
            GPIO_InitStructure.GPIO_Pin = GPIO_Pin_1;
            GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AIN;                   //模拟输入引脚
            GPIO_Init(GPIOA, &GPIO_InitStructure);  

            ADC_DeInit(ADC1);  //复位ADC1
            ADC_InitStructure.ADC_Mode = ADC_Mode_Independent;  //ADC工作模式:STM32有多种工作模式，而不同的ADC又是共用通道，当两个ADC采集同一个通道的先后顺序和时间间隔不同，就有不同的方式。具体的各个方式可在手册中查询。
            ADC_InitStructure.ADC_ScanConvMode = DISABLE;   //模数转换工作在单通道模式。当多通道需要ADC采集时，可把ADC配置为按一定顺序对各个通道进行扫描转换，即进行轮流采集各通道的值。若采集多个通道，则必须开启此模式。
            ADC_InitStructure.ADC_ContinuousConvMode = DISABLE; //模数转换工作在单次转换模式。可设置为连续和单次。
            ADC_InitStructure.ADC_ExternalTrigConv = ADC_ExternalTrigConv_None; //转换由软件而不是外部触发启动
            ADC_InitStructure.ADC_DataAlign = ADC_DataAlign_Right;  //ADC数据右对齐（ADC的转换精度为12位，而ADC的数据存储器-ADC_DR为16位，所以就有了左对齐和右对齐）
            ADC_InitStructure.ADC_NbrOfChannel = 1; //顺序进行规则转换的ADC通道的数目（1-16)
            ADC_Init(ADC1, &ADC_InitStructure); //根据ADC_InitStruct中指定的参数初始化外设ADCx的寄存器   


            ADC_Cmd(ADC1, ENABLE);  //使能指定的ADC1
            ADC_ResetCalibration(ADC1); //使能复位校准  
            while(ADC_GetResetCalibrationStatus(ADC1)); //等待复位校准结束
            ADC_StartCalibration(ADC1);  //开启AD校准
            while(ADC_GetCalibrationStatus(ADC1));   //等待校准结束
        //  ADC_SoftwareStartConvCmd(ADC1, ENABLE);     //使能指定的ADC1的软件转换启动功能
         }


        //获得ADC值
        //ch:通道值 0~3 ADC_Channel_0(0-17)
        u16 Get_Adc(u8 ch)   
        {
            //设置指定ADC的规则组通道，一个序列，采样时间
            ADC_RegularChannelConfig(ADC1, ch, 1, ADC_SampleTime_239Cycles5 );  //ADC1,ADC通道,采样时间为239.5周期（T=采样周期+12.5个周期）
            ADC_SoftwareStartConvCmd(ADC1, ENABLE);     //使能指定的ADC1的软件转换启动功能    
            while(!ADC_GetFlagStatus(ADC1, ADC_FLAG_EOC ));//等待转换结束

            return ADC_GetConversionValue(ADC1);    //返回最近一次ADC1规则组的转换结果
        }

        u16 Get_Adc_Average(u8 ch,u8 times)
        {
            u32 temp_val=0;
            u8 t;
            for(t=0;t<times;t++)
            {
                temp_val+=Get_Adc(ch);
                delay_ms(5);
            }
            return temp_val/times;
        }    
```
在`main`函数中只要调用`adcx=Get_Adc_Average(ADC_Channel_1,10);`之类的就可以读取转换的值。



## ADC单通道转换(DMA方式)

#### 为什么要用DMA方式

在上述的ADC转换中，CPU要处理由ADC外设采集回来的数据时，CPU首先要把数据从ADC外设的寄存器读取到CPU内存中，然后进行运算。但是用CPU来转换数据是有些**杀鸡用牛刀**，用DMA方式可以大大减轻CPU工作，从而提高运算效率。

#### 贴代码

```c

#define ADC1_DR_Address    ((u32)0x40012400+0x4c)//DMA传输的外设地址ADC1_DR_Address是一个自定义的宏ADC_DR保存了ADC转换的值，以它作为DMA传输的源地址。

__IO uint16_t ADC_ConvertedValue;//在传输地址中定义一个基地址
//__IO u16 ADC_ConvertedValueLocal;



static void ADC1_GPIO_Config(void)
{
    GPIO_InitTypeDef GPIO_InitStructure;

    /* 使能DMA时钟 */
    RCC_AHBPeriphClockCmd(RCC_AHBPeriph_DMA1, ENABLE);
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_ADC1 | RCC_APB2Periph_GPIOC, ENABLE);

    /* 输入的GPIO口定义*/
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_1;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AIN;//ADC必须为模拟输入（输入后ADC转换为二进制）
    GPIO_Init(GPIOC, &GPIO_InitStructure);              // PC1,输入时不用设置速率
}


static void ADC1_Mode_Config(void)
{
    DMA_InitTypeDef DMA_InitStructure;
    ADC_InitTypeDef ADC_InitStructure;

    /* DMA channel1 configuration */
    DMA_DeInit(DMA1_Channel1);
    DMA_InitStructure.DMA_PeripheralBaseAddr = ADC1_DR_Address;  //ADC地址
    DMA_InitStructure.DMA_MemoryBaseAddr = (u32)&ADC_ConvertedValue;//内存地址
    DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralSRC;
    DMA_InitStructure.DMA_BufferSize = 1;
    DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Disable;//外设地址固定
    DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Disable;  //内存地址固定
    DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_HalfWord; //半字
    DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_HalfWord;
    DMA_InitStructure.DMA_Mode = DMA_Mode_Circular;     //循环传输
    DMA_InitStructure.DMA_Priority = DMA_Priority_High;
    DMA_InitStructure.DMA_M2M = DMA_M2M_Disable;
    DMA_Init(DMA1_Channel1, &DMA_InitStructure);

    /* Enable DMA channel1 */
    DMA_Cmd(DMA1_Channel1, ENABLE);

    /* ADC1 configuration */

    ADC_InitStructure.ADC_Mode = ADC_Mode_Independent;  //独立ADC模式
    ADC_InitStructure.ADC_ScanConvMode = DISABLE ;   //禁止扫描模式，扫描模式用于多通道采集
    ADC_InitStructure.ADC_ContinuousConvMode = ENABLE;  //开启连续转换模式，即不停地进行ADC转换
    ADC_InitStructure.ADC_ExternalTrigConv = ADC_ExternalTrigConv_None; //不使用外部触发转换
    ADC_InitStructure.ADC_DataAlign = ADC_DataAlign_Right;  //采集数据右对齐
    ADC_InitStructure.ADC_NbrOfChannel = 1;     //要转换的通道数目1
    ADC_Init(ADC1, &ADC_InitStructure);

    /*配置ADC时钟，为PCLK2的8分频，即9Hz*/
    RCC_ADCCLKConfig(RCC_PCLK2_Div8);
    /*配置ADC1的通道11为55.   5个采样周期，序列为1 */
    ADC_RegularChannelConfig(ADC1, ADC_Channel_11, 1, ADC_SampleTime_55Cycles5);

    /* Enable ADC1 DMA */
    ADC_DMACmd(ADC1, ENABLE);

    /* Enable ADC1 */
    ADC_Cmd(ADC1, ENABLE);

    /*复位校准寄存器 */   
    ADC_ResetCalibration(ADC1);
    /*等待校准寄存器复位完成 */
    while(ADC_GetResetCalibrationStatus(ADC1));

    /* ADC校准 */
    ADC_StartCalibration(ADC1);
    /* 等待校准完成*/
    while(ADC_GetCalibrationStatus(ADC1));

    /* 由于没有采用外部触发，所以使用软件触发ADC转换 */
    ADC_SoftwareStartConvCmd(ADC1, ENABLE);
}

void ADC1_Init(void)
{
    ADC1_GPIO_Config();
    ADC1_Mode_Config();
}

```

在`main`函数中读取`ADC_ConvertedValue`就为转换的到的值（一般来说转换为电压较好`ADC_ConvertedValueLocal =(float) ADC_ConvertedValue/4096*3.3;`)，记住必须在前面`extern`下。



## ADC多通道转换（ADC多通道转换必须为DMA方式）

```c
 u8 DMA1_MEM_LEN;
//DMA1的各通道配置
//这里的传输形式是固定的,这点要根据不同的情况来修改
//从外设模式->存储器/16位数据宽度/存储器增量模式
 //cmar:存储器地址（自己定义的存储地址AD_DATA[4]） cndtr:数据传输量,实际上就是ADC要转换的路数（N）

#define ADC1_DR_Address  ((u32)0x40012400+0x4c)//DMA传输的外设地址ADC1_DR_Address是一个自定义的宏ADC_DR保存了ADC转换的值，以它作为DMA传输的源地址。
void DMA_Config(u32 cmar,u16 cndtr)
{     
    DMA_InitTypeDef DMA_InitStructure;
    RCC_AHBPeriphClockCmd(RCC_AHBPeriph_DMA1, ENABLE);  //使能DMA时钟

    DMA_DeInit(DMA1_Channel1);   //使用DMA的通道1，stm32有两个DMA，每个有7个通道
    DMA_InitStructure.DMA_PeripheralBaseAddr = ADC1_DR_Address;  //DMA外设基地址
    DMA_InitStructure.DMA_MemoryBaseAddr = cmar;  //DMA内存基地址
    DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralSRC;  //数据传输方向，从外设发送到内存  DMA_CCRX位4
    DMA_InitStructure.DMA_BufferSize = cndtr;  //DMA通道的DMA缓存的大小
    DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Disable;  //外设地址寄存器不变
    DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Enable;  //内存地址寄存器递增
    DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_HalfWord;  //外设数据宽度为16位
    DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_HalfWord; //内存数据宽度为16位
    DMA_InitStructure.DMA_Mode = DMA_Mode_Circular;  //工作在循环缓存模式
    DMA_InitStructure.DMA_Priority = DMA_Priority_High; //DMA通道 x拥有高优先级
    DMA_InitStructure.DMA_M2M = DMA_M2M_Disable;  //DMA通道x没有设置为内存到内存传输
    DMA_Init(DMA1_Channel1, &DMA_InitStructure);  //根据DMA_InitStruct中指定的参数初始化DMA的通道USART1_Tx_DMA_Channel所标识的寄存器

}

#define N  4//ADC1的通道数
extern u16 AD_DATA[4];

void  Adc_Init(void)
{
  ADC_InitTypeDef ADC_InitStructure;
  GPIO_InitTypeDef GPIO_InitStructure;

  RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA |RCC_APB2Periph_ADC1,ENABLE );//使能端口1的时钟和ADC1的时钟，因为ADC1的通道1在PA1上
  RCC_ADCCLKConfig(RCC_PCLK2_Div6);   //设置ADC分频因子6 72M/6=12M,ADC最大时间不能超过14M，也就是ADC的时钟频率为12MHz

  //PAx 作为模拟通道输入引脚                         
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0|GPIO_Pin_1|GPIO_Pin_3|GPIO_Pin_2;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AIN;     //模拟输入引脚
  GPIO_Init(GPIOA, &GPIO_InitStructure);

  ADC_DeInit(ADC1);  //复位ADC1,将外设 ADC1 的全部寄存器重设为缺省值
  ADC_InitStructure.ADC_Mode = ADC_Mode_Independent;    //本次实验使用的是ADC1，并ADC1工作在独立模式ADC_CR1的位19:16,即这几位为0000
  ADC_InitStructure.ADC_ScanConvMode = ENABLE;  //ADC_ScanConvMode 用来设置是否开启扫描模式，本实验开启扫面模式.ADC_CR1的位8
  ADC_InitStructure.ADC_ContinuousConvMode = ENABLE;    //ADC_ContinuousConvMode 用来设置是否开启连续转换模式 模数转换工作在连续转换模式，ADC_CR2的位1
  ADC_InitStructure.ADC_ExternalTrigConv = ADC_ExternalTrigConv_None;   //转换由软件而不是外部触发启动 ADC_CR2的位19:17
  ADC_InitStructure.ADC_DataAlign = ADC_DataAlign_Right;    //ADC数据右对齐ADC_CR2的位11
  ADC_InitStructure.ADC_NbrOfChannel = N;   //顺序进行规则转换的ADC通道的数目ADC_SQR1位23:20
  ADC_Init(ADC1, &ADC_InitStructure);   //根据ADC_InitStruct中指定的参数初始化外设ADCx的寄存器   

  ADC_RegularChannelConfig(ADC1, ADC_Channel_0, 1, ADC_SampleTime_55Cycles5 );//ADC1；ADC1通道0；第1转换；采样时间为55周期
  ADC_RegularChannelConfig(ADC1, ADC_Channel_1, 2, ADC_SampleTime_55Cycles5 );//ADC1；ADC1通道1；第2转换；采样时间为55周期
  ADC_RegularChannelConfig(ADC1, ADC_Channel_2, 3, ADC_SampleTime_55Cycles5 );
  //ADC1；ADC1通道1；第3转换；采样时间为55周期
    ADC_RegularChannelConfig(ADC1, ADC_Channel_3, 4, ADC_SampleTime_55Cycles5 );//ADC1；ADC1通道3；第4转换；采样时间为55周期

/**
这里的ADC_SampleTime可配置1.5、7.5、13.5、28.5、41.5、55.5、71.5、239.5 其计算公式为T=采样周期+12.5个周期
本代码中转换时间T=（55.5+12.5）/12  ----12为ADC时钟配置
注意你要输入的进行规则转换的通道数N要全部进行通道配置
**/
  ADC_DMACmd(ADC1, ENABLE); //使能ADC1的DMA传输，ADC_CR2位8

  ADC_Cmd(ADC1, ENABLE);    //使能的ADC1,ADC_CR2位0

  ADC_ResetCalibration(ADC1);   //使能复位校准，ADC_CR2位3  
  while(ADC_GetResetCalibrationStatus(ADC1));   //等待复位校准结束

  ADC_StartCalibration(ADC1);    //开启AD校准，ADC_CR2位2
  while(ADC_GetCalibrationStatus(ADC1));     //等待校准结束
}

```

在`main`函数中先声明
```c
#define N  4//ADC1的通道数
u16 AD_DATA[N];//AD转换的数字量
float value[N];//AD转换的模拟量
```
然后启动DMA
```c
Adc_Init();
DMA_Config((u32)&AD_DATA,N)
DMA_Cmd(DMA1_Channel1, ENABLE);//启动DMA通道
ADC_SoftwareStartConvCmd(ADC1, ENABLE);//软件启动AD转换   
```
最后转换为电压即可
`value[i] =(float) AD_DATA[i]*(3.3/4095)`
