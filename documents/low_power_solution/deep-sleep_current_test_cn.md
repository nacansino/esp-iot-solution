[[EN]](./deep-sleep_current_test_en.md) 

# ESP32 深度睡眠模式功耗测试

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
*ESP32 拥有 18 个 RTC IO 和 10 个 TouchPad, 每一个 RTC IO 和 TouchPad 经过配置都可以将芯片从 deep_sleep 模式中唤醒, 从而可以实现低功耗方案*. 

## 概述
该项测试基于 ESP32 电流测试板, 开发板使用和测试程序配置请参考 deep_sleep 测试板使用简介 [电流测试板使用简介](../evaluation_boards/esp32_ulp_eb_cn.md). 下面介绍的是通过配置不同唤醒方式, deep_sleep 期间芯片的工作电流. 所有的 RTC IO 和 TouchPad 都进行了测试.


外部事件唤醒
-
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
外部事件可以通过在 RTC IO 上产生一个电平信号, 从而唤醒正处于 deep_sleep 状态的芯片. 该事件可以是 EXT0 或 EXT1.

* EXT0 方式是当某个 RTC IO 上检测到有唤醒芯片的电平信号时, 芯片就被会唤醒, 在程序中配置是高电平还是低电平唤醒.
* EXT1 方式下, 我们可以采用多个 RTC IO 组合触发方式, 当几个 RTC IO 上电平信号满足一定条件时，芯片将被唤醒. 如果只有一个 RTC IO 进行了配置, 效果将与 EXT0 类似.

***具体程序配置请参考 [电流测试板使用简介](../evaluation_boards/esp32_ulp_eb_cn.md#testcase) 第四章.***

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
测试配置如下 : RTC IO 配置成输入 floating 模式. 程序配置某一个 RTC IO 为 wake_up IO. 当采用高电平唤醒时, 外部采用 10K 欧电阻下拉, 当采用低电平唤醒时, 外部采用 10K 欧电阻上拉. EXT0 唤醒方式测试结果中, 配置 GPIO_0 为 wake_up IO 时, 如果是配置低电平唤醒, deep_sleep 期间工作电流是 6.3uA, 配置成高电平唤醒时, deep_sleep 期间工作电流是 6.2uA.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
_`注 : RTC_IO37 和 RTC_IO38 在测试版上没有引脚引出, 没有进行测试.`_


---

__EXT0 唤醒方式测试结果__

| GPIO_NUM | low leval wakeup | high level wakeup |
| :----:   |       :----:     |       :----:      |
|GPIO_0    |        6.3uA     |        6.2uA      |
|GPIO_2    |        6.3uA     |        6.2uA      |
|GPIO_4    |        6.4uA     |        6.2uA      |
|GPIO_12   |        6.4uA     |        6.4uA      |
|GPIO_13   |        6.3uA     |        6.3uA      |
|GPIO_14   |        6.3uA     |        6.3uA      |
|GPIO_15   |        6.4uA     |        6.4uA      |
|GPIO_25   |        6.3uA     |        6.5uA      |
|GPIO_26   |        6.6uA     |        6.3uA      |
|GPIO_27   |        6.4uA     |        6.4uA      |
|GPIO_32   |        6.4uA     |        6.4uA      |
|GPIO_33   |        6.4uA     |        6.4uA      |
|GPIO_34   |        6.4uA     |        6.2uA      |
|GPIO_35   |        6.4uA     |        6.3uA      |
|GPIO_36   |        6.4uA     |        6.3uA      |
|GPIO_37   |         \        |          \        |
|GPIO_38   |         \        |          \        |
|GPIO_39   |        6.4uA     |        6.3uA      |

<br/>

---

__EXT1 唤醒方式测试结果__

| GPIO_NUM | low leval wakeup |  high level wakeup |
| :----:   |       :----:     |        :----:      |
|GPIO_0    |        5.2uA     |         5.3uA      |
|GPIO_2    |        5.2uA     |         5.2uA      |
|GPIO_4    |        5.2uA     |         5.2uA      |
|GPIO_12   |        5.2uA     |         5.3uA      |
|GPIO_13   |        5.3uA     |         5.2uA      |
|GPIO_14   |        5.3uA     |         5.3uA      |
|GPIO_15   |        5.3uA     |         5.2uA      |
|GPIO_25   |        5.2uA     |         5.3uA      |
|GPIO_26   |        5.3uA     |         5.2uA      |
|GPIO_27   |        5.3uA     |         5.3uA      |
|GPIO_32   |        5.3uA     |         5.3uA      |
|GPIO_33   |        5.3uA     |         5.3uA      |
|GPIO_34   |        5.3uA     |         5.7uA      |
|GPIO_35   |        5.3uA     |         5.7uA      |
|GPIO_36   |        5.3uA     |         5.3uA      |
|GPIO_37   |         \        |          \         |
|GPIO_38   |         \        |          \         |
|GPIO_39   |        5.4uA     |         5.5uA      |

<br/>


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
**从表格可以看出, deep_sleep 期间, 配置不同 RTC IO 唤醒时, 工作电流基本相同, 芯片的工作电流非常低. 采用 EXT1 唤醒方式时电流会比 EXT0 唤醒方式下少 1uA 左右.**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
_`注: RTC IO 请尽量采用外部上下拉方式, 在进入 deep_sleep 之前将其配置成 input floating 模式, 与采用内部上下拉方式相比, 这样可以有效降低 deep_sleep 期间电流.`_

---


Touch Pad 唤醒方式
-

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
Touch Pad 唤醒方式下, 芯片进入 deep_sleep 之前, 对 Touch Pad 进行初始化，然后配置触发阈值. 进入 deep_sleep 后测量 sleep 期间电流. 测试配置程序可以参考文档[电流测试板使用简介](../evaluation_boards/esp32_ulp_eb_cn.md#testcase)第四章.

__TouchPad 唤醒方式测试结果__

|    Pad Num      | Current |
|    :----        |  :----  |
|  Pad0 (GPIO_4)  |  37.3 uA|
|  Pad1 (GPIO_0)  |  35.7 uA|
|  Pad2 (GPIO_2)  |  36.6 uA|
|  Pad3 (GPIO_15) |  35.6 uA|
|  Pad4 (GPIO_13) |  36.5 uA|
|  Pad5 (GPIO_12) |  36.1 uA|
|  Pad6 (GPIO_14) |  36.7 uA|
|  Pad7 (GPIO_27) |  35.7 uA|
|  Pad8 (GPIO_33) |  36.7 uA|
|  Pad9 (GPIO_32) |  36.3 uA|

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
_`注： 可以使用 touch_pad_set_meas_time 接口来调整 touch sensor 充放电时间和充放电检测间隔，从而在合适的响应时间下，获得更好的低功耗表现。`_
