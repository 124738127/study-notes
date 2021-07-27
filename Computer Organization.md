# 原码、反码、补码



**机器数**：计算机使用二进制表示数据，这个数据被叫做机器数。



在长期的实践过程中，科学家们定义计算机使用byte为单位来表示数据，1 byte = 8 bit。

每一个bit代表一个二进制数据0或1，即计算机使用八位二进制为单位来表示数据。

由于数据是带正负符号的，因此规定机器数的最高位用于存放符号，正数为0，负数为1。



**真值**：带符号位的机器数对应的数值被称为机器数的真值。

由于第一位是符号位，因此机器数的形式值（不带符号位的机器数）不等于真值，例如：

* 机器数1000 0011的形式值为131，真值为-3
* 机器数0000 0011的形式值为3，真值为+3



**原码**：原码就是符号位加上真值的绝对值。

根据原码，1 byte的取值范围为：[1111 1111, 0111 1111]原 = [-127, 127]真

且存在两个0值：0000 0000 = +0，1000 0000 = -0



**反码**：

* 正数的反码与原码一致。
* 负数的反码是对原码按位取反，只是符号位不变。

例如：

127 = [0111 1111]原 = [0111 1111]反

-127 = [1111 1111]原 = [1000 0000]反



**补码**：

* 正数的补码与原码一致。
* 负数的补码是该数的反码加1。

例如：

127 = [0111 1111]原 = [0111 1111]反 = [0111 1111]补

-127 = [1111 1111]原 = [1000 0000]反 = [1000 0001]补



**计算机底层以补码的方式表示和存储数据，同样基于补码执行计算。**

因此，1 byte的取值范围[1111 1111, 0111 1111]补 = [-128, 127]真



#### 反码计算原码

* 正整数反码的原码：就是反码本身
* 负整数反码的原码：符号位不变，数值按位取反，例如：
  * [1111 1111]反
  * [1000 0000]原
  * [-0]真



#### 补码计算原码

* 正整数补码的原码：就是补码本身。
* 负整数补码的原码：符号位不变，数值按位取反，末位加1，即补码的补码等于原码。例如：
  * [1000 0000]补
  * [1 1000 0000]原
  * [-128]真



# 计算机运算基础



#### 基于补码的加法运算：



#### 基于补码的减法运算：



#### 基于补码的乘法运算：



#### 基于补码的除法运算：


































