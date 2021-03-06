AT24C02资料

* I2C 400K和1M都适用
* VCC 1.7-5.5V
* WP-读写保护引脚接到GND可以读写，接到VCC全芯片写保护
* 地址![image-20191220210022575](C:\Users\70654\AppData\Roaming\Typora\typora-user-images\image-20191220210022575.png)



有两种读写方式

按字节读写和按页读写

## 按字节写

![image-20191220211531030](C:\Users\70654\AppData\Roaming\Typora\typora-user-images\image-20191220211531030.png)

写

1. 起始信号后发送I2C硬件地址7位和以为读写标值
2. 再发送2个字节的地址(对I2C来说是内容，内容表示位EEPROM中的数据存储地址)，然后IC发送0来应答。
3. 发送1字节数据。
4. IC以0应答。然后此时MCU需要结束通信。然后IC进入t_WR的读写时间5ms

## 按页写

 每页64字节



![image-20191220211856683](C:\Users\70654\AppData\Roaming\Typora\typora-user-images\image-20191220211856683.png)

写

1. 与前面差不多，只是在第一个字节data数据发送后，还可以再发送63字节数据。然后停止，IC以零来应答。进入t_WR
2. 注意16位的地址在页读写方法上低6位是页面内的地址，高10位是页的地址。低6位会再没一个数据到来后自动+1，当地址到达页的边界的时候，会从本页第一个地址开始。如果超过64个数据发送，则会地址翻转，从最后一个到第一个来覆盖。



## 读取当前

内部数据字地址计数器维护在上一次读或写操作期间访问的最后一个地址，该地址加一。 只要维持芯片电源，该地址在两次操作之间就保持有效。 读取期间的地址翻转是从最后一个内存页的最后一个字节到第一页的第一个字节.

一旦将读/写选择位设置为1的设备地址送入并由EEPROM确认，当前地址数据字就会被串行送出。 微控制器不响应输入零，但会产生以下停止条件

![image-20191220215423444](C:\Users\70654\AppData\Roaming\Typora\typora-user-images\image-20191220215423444.png)

## 随机读取



## 注：

因为有t_wr的存在，再大约5ms内是不能读写的，如何确定是否可以读写了呢，就是不断进行通信，看有没有收到ACK。

