# AT24CXXX系列EEPROM驱动

WTR——欧阳俊源 2020/01/31

---

原理：使用I2C通信接口，向芯片内部写入和读取信息。

## 硬件原理图

* 2个I2C上拉4.7K电阻
* WP为写保护引脚
* A0A1A2为I2C硬件地址

<img src="C:\Users\70654\AppData\Roaming\Typora\typora-user-images\image-20200131002857858.png" alt="image-20200131002857858" style="zoom:50%;" />

## 软件配置

### CubeMx

<img src="C:\Users\70654\AppData\Roaming\Typora\typora-user-images\image-20200131003147057.png" alt="image-20200131003147057" style="zoom:67%;" />

### Code

勾选keil中的这个选项

<img src="C:\Users\70654\AppData\Roaming\Typora\typora-user-images\image-20200131004607850.png" alt="image-20200131004607850" style="zoom:67%;" />

在at24c256.h中配置

* STORAGE_I2C：配置使用hal的i2c句柄
* MAX_NUM_STORAGE_ONE：配置最大存储的变量数。即调用AT24Cx_REGISTER()的次数
* MEMORY_SIZE：该值为at24c256的bytes数。不同的芯片可以修改。

<img src="C:\Users\70654\AppData\Roaming\Typora\typora-user-images\image-20200131003501013.png" alt="image-20200131003501013" style="zoom:67%;" />



使用方法：

* AT24Cx_REGISTER(变量名) 进行注册存储变量。
* AT24Cx_STORAGE(变量名) 存储改变量在此语句的当前值。
* AT24Cx_RECOVER(变量名) 恢复出该变量名存储的值。







### 例子

 ![image-20200131005155424](C:\Users\70654\AppData\Roaming\Typora\typora-user-images\image-20200131005155424.png)![image-20200131004938858](C:\Users\70654\AppData\Roaming\Typora\typora-user-images\image-20200131004938858.png)

恢复前把strmy的a和b都置0

![image-20200131004926134](C:\Users\70654\AppData\Roaming\Typora\typora-user-images\image-20200131004926134.png)

恢复完毕![image-20200131005130257](C:\Users\70654\AppData\Roaming\Typora\typora-user-images\image-20200131005130257.png)