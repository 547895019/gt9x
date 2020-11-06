# GT9X

## 简介
GT911、GT928、GT9147都属于GT9系列非单层多点触控芯片，他们支持的触控点数不同（GT928支持10个点、GT911支持5个点）、驱动和感应通道也可能不同。可是他们的寄存器和IIC通讯时序是相同的，也就是说驱动程序是兼容的。

与主机的接口共有6PIN，分别为：VDD、GND、SCL、SDA、INT、RESET。INT、RESET不需要接上下拉电阻，可与主机直连。SCL、SDA需要接上拉电阻4.7K，毕竟400KHz的通信频率，没有上拉可能导致SCL、SDA边沿不够陡峭。

RST是复位引脚，拉低100us以上，即可复位。正常工作时，应该保持拉高。

INT是GT9xx的触摸信号输出引脚，在正常工作时，主机端要设置为悬浮输入，即不上下拉（GT9xx的驱动能力有限，如果外部上下拉，GT9xx可能驱动不了）。当有触摸发生时，INT引脚会输出上升沿或下降沿（内部寄存器可以配置），主机端可以一直读取INT脚的电平信号，也可以用端口外部中断检测。

gt9x 软件包提供了使用触摸芯片 gt9x 基本功能，并且本软件包已经对接到了 Touch 框架，通过 Touch 框架，开发者可以快速的将此触摸芯片驱动起来。
## 支持情况

| 包含设备           | 触摸芯片 |  
| ----------------     | -------- | 
| **通讯接口**      |          |      
| IIC              | √        | 
| **工作模式**     |          |     
| 中断             | √        | 
| 轮询             |   √       |        

## 使用说明

### 依赖

- RT-Thread 4.0.0+
- Touch 组件
- I2C 驱动：gt9x 设备使用 I2C 进行数据通讯，需要系统 I2C 驱动支持；

### 获取软件包

使用 gt9x 软件包需要在 RT-Thread 的包管理中选中它，具体路径如下：

```
RT-Thread online packages  --->
  peripheral libraries and drivers  --->
    touch drivers  --->
      gt9x: touch ic gt9x for rt-thread
              Version (latest)  --->
```
**Version**：软件包版本选择

### 使用软件包

gt9x 软件包初始化函数如下所示：

```
int rt_hw_gt9x_init(const char *name, struct rt_touch_config *cfg)
```

该函数需要由用户调用，函数主要完成的功能有，

- 设备配置和初始化（根据传入的配置信息，配置接口设备和中断引脚）；
- 注册相应的传感器设备，完成 gt9x 设备的注册；

#### 初始化示例

```.c
int rt_hw_gt9x_port(void)
{
    struct rt_touch_config config;
    rt_uint8_t rst;
    
    rst = GT9X_RST_PIN;
    config.dev_name = "i2c1";
    config.irq_pin.pin  = GT9X_IRQ_PIN;
    config.irq_pin.mode = PIN_MODE_INPUT_PULLDOWN;
    config.user_data = &rst;

    rt_hw_gt9x_init("gt", &config);

    return 0;
}
INIT_ENV_EXPORT(rt_hw_gt9x_port);
```

## 注意事项

暂无

## 联系人信息

维护人:

- [tyustli](https://github.com/tyustli) 

- 主页：<https://github.com/RT-Thread-packages/gt9x>