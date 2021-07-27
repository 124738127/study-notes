# java的加载与执行

*.java --> *.class --> 类装载器 --> JVM（java虚拟机） --> 操作系统 --> 硬件平台

- **java编译**：

  *.java(源文件) --> *.class(字节码文件)

  * .java源文件中编写的是java源程序/源代码。
  * java编译便是把java源文件编译成可执行的.class字节码文件。
  * 编译阶段主要的任务是检查java源程序是否符合java语法。



- **java运行**：

  *.class --> 类装载器 --> JVM（java虚拟机） --> 操作系统 --> 硬件平台

  * java运行的是.class字节码文件，.java文件不会影响java程序的运行。
  * .class字节码文件中不是纯粹的二进制，这种文件无法在操作系统当中直接执行，由java虚拟机执行。
  * .class文件的执行过程如下：
    * DOS窗口输入 java classname(类名距离)
    * java.exe 命令会启动java虚拟机（JVM），JVM会启动类装载器ClassLoader
    * ClassLoader会去硬盘上搜索classname.class的字节码文件，找到该文件则将该字节码文件装在到JVM当中
    * JVM将classname.class字节码文件解释成二进制数据100101010001....
    * 操作系统执行二进制和底层硬件平台进行交互



# java变量

本质上来说，变量是内存中的一块空间。

* 变量包含三部分：数据类型、变量名、数据值。
* 数据类型指导JVM在程序运行阶段应该分配多大的内存空间。
* java语法中变量中存储的数据值必须要和变量的数据类型一致，否则编译时会报错。
* java中只有变量可以存储数据。
* 有了变量的概念之后，内存空间得到了重复的使用。



### 变量声明的语法结构

```java
数据类型 变量名;
```

* 变量名可以为任意符合java语法的标识符
* 在同一个作用域当中，同一个变量名仅能声明一次，不允许重复声明
* 在不同的作用域当中，变量名可以重名



**变量赋值**：

```text
变量名 = 数据值;
```



* =：赋值运算符，先执行赋值运算符右边的表达式，表达式执行结束之后将执行结果赋值给左边的变量

* 变量赋值时会在内存空间内开辟一份空间，存储变量
* java中的变量必须赋值后才能访问，仅声明是无法访问的



**变量的作用域**：变量的有效范围

* 通俗记忆：大括号内可以访问，出了大括号就不认识了

  * 注意，实例变量的访问比较特殊，例如：

  ```java
  public class instanceVriables {
      int num = 10;
      
      public static void main(String[] args) {
          // 编译报错
          // num是一个实例变量，而static方法没有当前对象this，因而无法访问
          System.out.println(num);
      }
  }
  ```

  

* java遵循就近原则，优先在更小的作用域内寻找变量，举例如下：

  ```java
  public class variablesTest01{
      public static void main(String[] args){
          int i = 1;
          System.out.println(i);//控制台输出 1
      }
      static int i = 100;
  }
  ```

  ```java
  public class variablesTest02{
      public static void main(String[] args){
          //int i = 1;
          System.out.println(i);//控制台输出 100
      }
      static int i = 100;
  }
  ```

  static 静态修饰符，程序先执行静态，再自上而下执行。



### 变量的分类

```text
|-- 局部变量
|-- 成员变量
|   |-- 实例变量
|   |-- 静态变量
```



#### 局部变量和成员变量

根据变量声明的位置，可以分为 局部变量 和 成员变量

* **局部变量**：在方法体当中声明的变量叫做局部变量
  * 局部变量在声明后必须显式赋值，否则会编译报错
  * 局部变量在栈内存中存储
* **成员变量**：在方法体之外，类体之内，声明的变量叫做成员变量，或称全局变量。
  * 成员变量在堆内存中存储



#### 实例变量和静态变量

* 成员变量根据修饰符关键字，可以分为 实例变量 和 静态变量
* **实例变量**：没有使用关键字static修饰的成员变量
  * 实例变量需要通过构造方法实例化对象才能访问
  * 构造方法创建实例时为实例变量开辟内存空间并赋默认值
  * **实例变量只能通过"引用.实例变量名"的方式访问**
* **静态变量**：使用关键字static修饰的成员变量
  * 静态变量在类加载的时候初始化，存储在方法区内存

  * 静态变量的使用不需要创建对象

  * 静态变量也可以是一个引用，例如

    ```java
    // ref引用Reference类的一个实例，可以访问实例的内部方法
    static Reference ref = new Reference();
    ```

    

  * 静态变量可以通过"类名.静态变量名"访问，建议使用这种方式访问

  * 静态变量也可以通过"引用.静态变量名"访问，但不建议这样访问，通过引用映射到类，增加了程序的开销
    
    * 后续不再赘述，默认static变量不使用引用的方式访问



### 变量调用的语法结构

```java
// 局部变量可以在作用域内直接使用
public class variables {
    // 实例变量
    int instanceVariables;
    
    // 静态变量
    static int staticVariables;
    
    public static void main(String[] args) {
        // 局部变量
        int localVariables;
        
        // 局部变量在作用域内直接访问
        localVariables = 10;
        
        // 静态变量使用"类名.静态变量名"访问
        variables.staticVariables = 20;
        
        // 实例变量首先要创建/获取实例，而后通过"引用.实例变量名"访问
        variables var = new variables();
        var.instanceVariables = 30;
        
        System.out.println(localVariables); // 10
        System.out.println(variables.staticVariables); // 20
        System.out.println(var.instanceVariables); // 30
    }
}
```





# java数据类型

数据类型指导JVM在程序运行阶段应该为变量分配多大的内存空间。

* 基本数据类型：四大类八小类，占用不同的内存空间[字节 byte]

   1 byte = 8 bit，bit表示一个二进制位：1/0

  * 整数型：byte[1], short[2], int[4], long[8]
  * 浮点型：float[4], double[8]
  * 布尔型：boolean[1]
  * 字符型：char[2]

* 引用数据类型：

  * 类：
  * 接口：
  * 数组：

java语言中所有的.class都属于引用数据类型，类型名就是类名



### 数据类型的默认值

默认向0看齐

* byte, short, int: 0
* long: 0L
* float, double: 0.0
* boolean: false (0否1是)
* char: '\u0000'(空字符)
* 引用数据类型: null（空值，不是空）



### 整数类型

java中整数型数据有三种表示方式：

* 十进制：十进制是缺省默认的表示方式
* 八进制：八进制整数型数据要以0开始，例如“x = 010”代表十进制的8
* 十六进制：十六进制整数型 数据要以0x开始，例如“x = 0x10”代表十进制的16



**java中整数类型的数据会被默认当作int类型来处理**，占用4 byte。

* 小容量类型可以自动转换成大容量类型，称为自动类型转换机制。
* 大容量类型不能直接赋值给小容量类型，需要进行强制类型转换。
  * 强制类型转换需要加强制类型转换符。
  * 强制类型转换后可以编译通过，但可能损失精度，需要谨慎使用。
  * 当数据不超过byte, short, char类型取值范围时，可以直接赋值，java默认执行强制类型转换，不会损失精度，不需要程序员手工执行。

```java
public class variablesType {
    public static void main(String[] args){
        
        //50这个整数型数据是int型
        //i变量在声明的时候也是int型
        //int型的数据50赋值给int型的变量i，不存在类型转换
        int i = 50;
        
        //2147482647被当作int类型，占用4个字节
        //x变量在声明时时long类型，占用8个字节
        //int类型的数据2147482647被赋值给long类型的x，存在类型转换，int类型转换成long类型
        //小容量类型可以自动转换成大容量类型，称为自动类型转换机制
        long x = 2147482647;
        
        //编译错误，2147483648超出int类型取值范围
        long y = 2147483648; 
        
        //编译通过，2147483648L 是一个long类型的数据，赋值过程不存在类型转换
        long z = 2147483648L; 
        
        //大容量类型不能直接赋值给小容量类型，需要进行强制类型转换
        //强制类型转换后可以编译通过，但可能损失精度
        /*
        强制转换原理：
        	原始数据long：00000000 00000000 00000000 00000000 01111111 11111111 11111111 11111111
        	强制转换后的数据int：01111111 11111111 11111111 11111111
        	将左边的二进制砍掉
        */
        //编译通过，但没有损失精度
        int xi = (int)x;
        
        //编译通过，但损失精度相当严重，结果变成了负数
        int zi = (int)z;
        
        /*
        	解析：
        	198(int): [00000000 00000000 00000000 11000110]补
        	198(byte):[11000110]补
        	198(byte):[10111010]原
        	198(byte):[-58]真
        */
        //编译通过，但损失精度相当严重
        byte m = (byte) 198;
        System.out.println(m); //-58
        
        //编译通过，不超过byte取值范围时，可以直接赋值，不需要做强制类型转换
        byte a = 50;
        
        //编译错误，i变量为int类型，int类型转换byte类型需要做强制类型转换
        byte ii = i;
        
        //编译通过，不超过char取值范围时，可以直接赋值，不需要做强制类型转换
        char ch = 65535;
        
        //编译错误，超过char取值范围，需要做强制类型转换
        ch = 65536;
        
    }
}
```



### 浮点类型

**java中浮点类型的数据会被默认当作double类型来处理**，占用8 byte。

* float类型的数据，需要在数据后面添加F/f，例如：3.0f



double和float在计算机内部二进制存储的时候，存储的都是近似值。

* 原因在于：现实世界中有一些数据是无限循环的，而计算机资源是有限的，有限的资源存储无限的数据只能存储近似值。
  * 例如：1/3：0.3333333333......

* 由于double类型精度相对较低，java提供了精确度更高的引用数据类型：java.math.BigDecimal

```java
public class variablesType {
    public static void main(String[] args){
        
        //编译通过，double类型数据赋值给double类型数据
        double f = 5.0；
        
        //编译错误，大容量类型不能直接赋值给小容量类型，需要进行强制类型转换。
        float f = 5.0;
        
        //编译通过，float类型数据直接赋值给float类型变量
        float f = 5.0f;
        
        //编译通过，强制类型转换
        float f = (float)5.0;
        
    }
}
```



### 布尔类型

java中boolean类型只有两个值：true和false。

* false底层存储是0
* true底层存储的是1



### 基本数据类型转换规则

* 除boolean类型外，其他7种基本数据类型之间均可以互相转换
* 小容量向大容量转换，称为自动类型转换：
  * byte[1] < short[2] = char[2] < int[4] < long[8] < float[4] < double[8]
    * 任意浮点类型数据，不管占用多少字节，都比整数型的容量大
    * char和short可表示的种类相同，但char可以表示更大的正整数
* 大容量向小容量转换，称为强制类型转换：
  * 强制类型转换可能损失精度，需要谨慎使用
* 当数据没有超出byte, short, char的取值范围时，java工程师可以直接赋值使用，java编译时默认执行强制转换，不损失精度
* byte, short, char混合运算时，各自先转换成int类型再做运算
* 多种数据类型混合运算，先转换成容量最大的那种类型再做运算

```java
public class variablesCompute {
    public static void main(String[] args){
        
        //java中int类型的运算，会直接抹除小数位后的，具体计算过程参考计算机原理
        //int类型的数据10，int类型的数据3
        //运算结果是int类型的数据3
        int a = 10/3; //3
        
        int a = 13/2; //6
        
        //int类型的数据10，int类型的数据3，计算结果是int类型的3
        //int类型的数据3为double类型的变量c赋值，执行自动类型转换，变量c的数据是3.0
        double c = 10/3; //3
        
        //double类型的数据10.0，int类型的数据3
        //int类型的数据转换为混合运算中容量最大的double类型，数据3.0
        //执行运算
        double d = 10.0/3; //3.3333333333333335
    }
}
```



# java字符编码

计算机只认识二进制，那么计算机时如何表示现实世界中的文字的呢？通过字符编码。

**字符编码**：人为指定的“文字”和“二进制”之间的对照转换关系。

* 最早出现的字符编码是：ASCII码（采用一个byte编码）。
  * 'a' --> 97 [0110 0001]
  * 'A' --> 65 [0100 0001]
  * '0' --> 48 [0011 0000]
* **解码和编码时采用同一套字典（对照表），不会出现乱码。当解码和编码的时候采用的不是同一套对照表，会出现乱码。**
  * 'a' --按照ASCII解码--> 0110 0001
  * 0110 0001 --按照ASCII编码--> 'a'
* 随着计算机的发展，后来出现了一种编码方式支持西欧预言，向上兼容ASCII码，仍然不支持中文，这种编码是：ISO-8859-1，又被称为latin-1
* 随着计算机向亚洲发展，计算机开始支持中文、日文、韩文等国家文字，其中支持简体中文的编码方式：GB2312 < GBK < GB18030
* 支持繁体中文：大五码 big5
* 后来出现了一种编码方式统一了全球所有的文字：unicode编码
  * unicode编码方式有多种具体的实现：UTF-8 < UTF-16 < UTF-32
  * 现在在实际开发中，一般使用UTF-8的方式开发



java语言源代码采用的是unicode编码方式，所以“标识符“可以使用中文。



# java运算符

java运算符分为如下几类：

* 算数运算符

* 关系运算符

* 布尔运算符/逻辑运算符



  * 位运算符：可以直接操作二进制的运算符，效率比较高
* 赋值类运算符：

* 字符串连接运算符：

* 三目运算符/三元运算符/条件运算符：
  
  * ?:
  
* 其他运算符：
  * instanceof:
  * new:



### 算数运算符

* +, -, *, /: 加、减、乘、除

* %: 求余数（取模）

* ++, --: 自加、自减
  * 以++举例，--相同：


```java
public class operate {
    public static void main(String[] args){
        
        int x = 10;
        //++运算符可以出现在变量之后，运算结束后，变量值自加1
        x++;
        System.out.println(x); //11
        
        int y = 10;
        //++运算符可以出现在变量之前，运算结束后，变量值自加1
        ++y;
        System.out.println(y); //11
        
        //自加运算符在变量之后时，赋值运算符的优先级要高于自加运算符
        //因此先执行变量b的赋值，后执行变量a自加
        int a = 10;
        int b = a ++;
        System.out.println(a); //11
        System.out.println(b); //10
        
        //自加运算符在变量之前时，自加运算符的优先级要高于赋值运算符
        //因此先执行变量c的自加运算，而后执行变量d的赋值
        int c = 10;
        int d = ++ c;
        System.out.println(c); //11
        System.out.println(d); //11      
        
        //根据println方法的源程序 public void println(int x){...}
        //因此执行的是：int x = e ++;
        //所以，控制台输出的是未执行自加运算之前的10，而不是11
        int e = 10；
        System.out.println(e++); //10
        System.out.println(e); //11
        //以此类推
        System.out.println(++e); //12
        System.out.println(e); //12
        
    }
}
```



### 关系型运算符

* <, <=, >, >=, ==, !=: 小于、小于等于、大于、大于等于、等于、不等于
* 关系运算符的结果一定是布尔类型：true或false
```java
public class operate {
    public static void main(String[] args){
        
        int a = 10;
        int b = 10;
        
        //比较的时候，比较的是：变量a存储的数据10和变量b存储的数据10，二者之间的大小
        a > b; //false
        
    }
}
```



### 布尔运算符/逻辑运算符

* &: 逻辑与：两边的算子都是true，结果才是true
* |: 逻辑或：两边的算子只要由一个是true，结果就是true
* !: 逻辑非：取反，!false就是true，!true就是false，是一个单目运算符
* ^: 逻辑异或：两边的算子只要不一样，结果就是true
  * true ^ false = true, false ^ true = true
  * true ^ true = false, false ^ false = false
* &&: 短路与：短路与和逻辑与&最终的运算结果是相同的，只不过短路与存在短路现象
* ||: 短路或：短路或和逻辑或|最终的运算结果是相同的，只不过短路或存在短路现象



* 逻辑运算符要求两边的算子必须是布尔类型，并且逻辑运算符最终的运算结果也是布尔类型

* 短路现象：
  * 短路与：表达式从左至右，当找到一个false的算子时，发生短路与，后面的表达式不再执行
  * 短路或：表达式从左至右，当找到一个true的算子时，发生短路或，后面的表达式不再执行
  * 以短路与&&和逻辑与&举例：
```java
public class operate {
    public static void main(String[] args){
        
        //逻辑与
        int a = 10;
        int b = 5;
        //两边算子确定后，执行逻辑与运算
        System.out.println(a<b & ++a<b); //false
        System.out.println(a); //11
        
        
        //短路与
        int c = 10;
        int d = 5;
        //由于左边的算子已经确定是false，因此结果一定是false
        //右边的算子不再执行运算，这种现象被称为短路现象
        //短路与才会有短路现象，逻辑与不存在短路现象
        System.out.println(c<d && ++c<d); //false
        System.out.println(c); //10
        
        /*
        总结：
        短路与更智能，执行效率更高，实际开发过程中短路与更常用
        但如果实际业务中要求两侧的表达式必须都执行，那么必须使用逻辑与，使用短可能导致部分业务逻辑不执行        	
        */
    }
}
```



### 赋值类运算符

* 基本的赋值运算符：
  * =: 先执行等号右边的表达式，将执行结果赋值给左边的变量。
* 扩展的赋值运算符：
  * +=: 追加/累加，变量当前的数据值自加右边表达式的数据值
  * -=: 变量当前的数据值自减右边表达式的数据值
  * *=: 变量当前的数据值自乘右边表达式的数据值
  * /=: 变量当前的数据值自除右边表达式的数据值
  * %=: 变量当前的数据值自除右边表达式的数据值取模

```java
public class operate {
    public static void main(String[] args) {
        
        byte a = 10;
        
        //编译错误，byte类型的数据10和int类型的数据5，计算得出int类型的数据15
        //int类型的数据15向byte类型的变量a赋值，属于大容量向小容量转换，必须有强制类型转换
        a = a + 5;
        
        //编译通过，int类型的数据15经过强制类型转换，转换为byte类型的数据15，向byte类型的变量赋值
        //
        a = (byte)(a + 5);
        
        //编译通过
        //由此可见，a += 5 等同于 a = （byte）（a + 5），而并不等同于a = a + 5
        a += 5;
        
        a = (byte)0;
        a += 128;
        System.out.println(a); //-128，强制转换类型之后损失精度
        
    }
}
```

扩展类赋值运算符不改变运算结果类型。

* 假设变量最初的类型是byte，使用扩展类运算符，无论后续如何运算，运算结果始终会是byte类型。



### 字符串连接运算符

java中的"+"运算符：

* 加法运算：当"+"运算符两边的数据都是数字的话，一定会进行加法运算
* 字符串的连接运算：当"+"运算符两边的数据只要有一个数据的类型是字符串，一定会进行字符串连接运算，运算结果是一个字符串类型



### 条件运算符

又称三元运算符、三目运算符。

* 语法规则：
  * 变量 = 布尔表达式 ? 表达式1 : 表达式2;
* 运算规则：
  * 如果布尔表达式的值为真，选择表达式1作为整个表达式的执行结果，赋值给变量
  * 如果布尔表达式的值为假，选择表达式2作为整个表达式的执行结果，赋值给变量





### java运算符的优先级

| 优先级 | 运算符                | 结合性   |
| ------ | ----------------- | -------- |
| 1      | .、()、[]、{}                        | 从左向右 |
| 2      | !、+、-、~、++、--                     | 从右向左 |
| 3      | *、/、%                                | 从左向右 |
| 4      | +、-                                     | 从左向右 |
| 5      | «、»、>>>                                 | 从左向右 |
| 6      | <、<=、>、>=、instanceof                    | 从左向右 |
| 7      | ==、!=                                     | 从左向右 |
| 8      | &                                         | 从左向右 |
| 9      | ^                                          | 从左向右 |
| 10     | \|                                         | 从左向右 |
| 11     | &&                                         | 从左向右 |
| 12     | \|\|                                      | 从左向右 |
| 13     | ?:                                        | 从右向左 |
| 14     | =、+=、-=、*=、/=、&=、\|=、^=、~=、«=、»=、>>>= | 从右向左 |








# java注释

.java文件中的注释，不会被编译到.class文件中

* 单行注释

  ```java
  // 单行注释
  ```

  

* 多行注释

  ```java
  /* 
      多行注释
      多行注释
      多行注释
  */
  ```

  

* javadoc注释

  ```java
  /*
  *   javadoc注释
  *   javadoc注释
  *   javadoc注释
  */
  ```

  javadoc注释是比较专业的注释，该注释会被javadoc.exe工具解析并生成java说明文档。




# helloworld解析



```java
// public表示公开的
// class表示定义一个类
// HelloWorld表示一个类名
public class HelloWorld { // 表示定义一个公开的类，起名HelloWorld
    // 类体
    // 类体内不允许直接编写java语句，除变量声明之外
    
    /*
    	public表示公开的，在任何位置都时可以访问的
    	static表示静态的
    	void表示空，方法执行后不返回任何数据
    	main表示方法名是main
    	(String[] args)是一个main方法的形式参数列表
    	
    	需要记住的是：
    		以下的方法是一个程序的“主方法”，是程序执行的入口
    		是SUN公司规定的，固定编写方法。
    */
    
    public static void main(String[] args) { // 表示定义一个公开的静态的主方法
        // 方法体
        // 在方法体内编写java语句
        // java语句以";"终止，分号必须是半角分号
        // 方法体内的java代码是自上而下依顺序执行的，逐行执行，以";"为断句
        
        /*
        	以下代码的作用是向控制台输出一段消息
        */
        System.out.println("Hello World!")
    }
}
```



# public class 和 class 的区别



* 一个java源文件中可以定义多个class

* 一个class会定义生成一个 xxx.class 的字节码文件

* 一个java源文件中，public class 不是必须的

* 一个java源文件中定义 public class 只能有一个，并且该类名称必须和java源文件名称一致

* 每一个class中都可以编写main方法，都可以设定程序的入口

* 当在命令窗口中执行java X，那么要求X.class当中必须有main方法，没有main方法会出现运行错误



# java控制语句

* 选择语句

  * if, if ... else

  ```java
  if (布尔表达式) {
      java语句;
      ...
  } else if (布尔表达式) {
      java语句;
      ...
  } else {
      java语句;
      ...
  }
  ```

  

  * switch
    * switch后小括号内的数据和case后面的数据匹配，匹配成功的分支执行
    * 按照自上而下的顺序以此匹配
    * 匹配成功的分支执行，分支当中最后有break;语句的话，整个switch语句终止
    * 匹配成功的分支执行，分支当中最后没有有break;语句的话，直接进入下一个分支执行，不进行匹配，这种情况被称为“case穿透”
    * 所有分支都不匹配，默认执行default分支
    * switch后面和case后面只能是int或者String类型的数据
      * byte, short, char也可以直接写，因为它们可以执行自动类型转换，转换成int类型
    * case可以合并

  ```java
  switch (int | String) {
      case int | String :
          java语句;
          ...
          break;
      case int | String :
          java语句;
          ...
          break;
      case 1: case 2: case 3 : case 4 :
          System.out.println("case语句的合并");
          break;
      default :
          java语句;
          ...
  }
  ```

  

* 循环语句

  * for

    * 循环结构代表着重复执行某一段代码片段

    * 执行过程/执行原理：

      * 初始化表达式、布尔表达式、更新表达式都不是必须的，但两个分号必须的

      * 1. 初始化表达式最先执行，并且在整个for循环当中只执行一次

      * 2. 布尔表达式必须是true或false，不能是其他值

      * 3. 判断布尔表达式

        * 布尔表达式为true
          * 执行循环体
          * 执行更新表达式
          * 重新进入3，判断布尔表达式
        * 布尔表达式为false
          * for循环结束

  ```java
  for (初始表达式; 布尔表达式; 更新表达式) {
      //循环体：需要重复执行的代码片段
      java语句;
      ...
  };
  ```

  

  * while

    * 执行过程/执行原理：

      * 1. 判断布尔表达式：

        * 布尔表达式为true
          * 执行循环体
          * 重新进入1，判断布尔表达式
        * 布尔表达式为false
          * while循环结束

    * while的更新表达式要写在循环体之内，否则可能造成死循环

  ```java
  while (布尔表达式) {
      //循环体
      java语句;
      ...
  };
  ```

  

  * do  ... while

    * 执行过程/执行原理：

      * 1. 执行循环体

      * 2. 判断布尔表达式

        * 布尔表达式为true
          * 重新进入1，执行循环体
        * 布尔表达式为false
          * do while循环结束

    * do while的更新表达式要写在循环体之内，否则可能造成死循环

  ```java
  do {
      //循环体
      java语句;
      ...
  } while (布尔表达式);
  ```

  

* 控制循环语句

  * break
    * break语句在循环语句中使用，用来终止循环的执行
    * 在默认的情况下，break语句终止的是当前的循环；也可以指定终止某个循环，需要给循环起名，语法：break 循环名称;

  ```java
  for (int i = 0; i < 5; i++) {
      for (int j = 0; j < 5; j++) {
          if (j == 4) {
              break; // 终止的是内层的循环，不影响外层循环
          }
      }
  }
  
  // 以下这种方式可以为for循环命名，用的不多
  A: for (int i = 0; i < 5; i++) {
      B: for (int j = 0; j < 5; j++) {
          if (j == 4) {
              break A; // 终止的是外层循环
          }
      }
  }
  ```

  

  * continue
    * continue语句在循环语句中使用，用来控制循环的执行
    * continue忽略循环体的下文，直接进入下一个循环，执行循环体
    * continue也可以指定继续某个循环，与break类似，需要指定循环名称

  ```java
  for (int i = 0; i < 5; i++) {
      if (i == 3) {
          continue;
      }
      System.out.println(i); // 0 1 2 4
  }
  ```

  

# JVM内存管理

JVM内存划分上有三块主要的内存空间：

* **方法区内存**：JVM只有一个方法区内存，主要存储：
  * 代码片段
  * 静态变量
* **堆内存**：JVM只有一个堆内存，主要存储：
  * 实例
  * 实例变量
* **栈内存**：JVM每一个线程存在一个栈内存，主要存储：
  * 局部变量



* 类的源代码编译后生成.class字节码文件，在类加载的时候，.class文件被加载到方法区内存
  * 所以JVM中三块主要的内存空间中，方法区内存最先有数据，存放了代码片段
  * 代码片段虽然在方法区内存中只有一份，但是可以被重复调用
  * 方法代码片段是.class文件的一部分
* 方法调用的时候，需要给在栈内存中给该方法分配独立的运行空间
  * 如果方法只定义而不调用，JVM不会给该方法分配运行所属的内存空间，只有在调用的时候才会分配
  * 在方法调用的瞬间，栈内存分配内存空间，此时发生压栈动作
  * 方法运行结束的瞬间，在栈内存中占用的内存空间被释放，此时发生弹栈动作
  * 栈内存主要存储方法体中的局部变量
* 方法调用的时候，参数传递的是变量中存储的数据值
  * 即在栈区的方法运行空间内，首先要分配实际参数数据值存储的内存空间
  * 这是java语言没有C语言运行效率高的原因之一，C语言可以使用指针直接指向内存，不需要额外占用更多的内存空间
* 通过类实例化对象的时候，需要在堆内存中给该实例分配独立的运行空间
  * 堆内存中主要存储实例对象的实例变量
* 三块内存中变化最频繁的是栈内存
* JVM自动垃圾回收机制（GC机制）主要针对的是堆内存
  * 当堆内存中的java实例没有更多的引用指向它的时候，这个实例将无法被访问，此时会被GC回收释放





# java方法

方法就是完成特定功能的可重复使用的代码片段。

* 方法在java中叫做Method，在C语言中叫做函数Function，或是过程Procedure
* 方法定义在类体当中，在一个类中可以定义多个方法，方法的编写的位置没有先后顺序
* 方法体当中不能再定义方法，方法体由java语句构成
* 使用方法被称为“调用”/invoke

```java
public class method {
    // 类体
    
    public static void main(String[] args) {
        sums(10, 20);
        subs(10, 20);
    }
    
    public static void sums(int a, int b) {
        // 方法体
        System.out.println(a + b);
    }
    
    public static void subs(int a, int b) {
        // 方法体
        System.out.println(a - b);
    }
    
}
```



### 方法声明的语法结构

```text
[修饰符列表] 返回值类型 方法名(形式参数列表) {
    方法体;
}
```

**修饰符列表**：

* 方法的修饰符列表是可选项，不是必须的，修饰符包括：
  * public
  * protected
  * private
  * abstract
  * static：
    * 没有static关键字修饰的方法被称为“实例方法”
    * 没有static关键字修饰的变量被称为“实例变量”
  * final
  * synchronized
* public, protected, private 不能同时存在



**返回值类型**：

* 返回值是方法执行后的返回数据

  * 返回值的语法

  ```java
  reture 返回值;
  ```

* 返回值类型时返回数据的数据类型，可能为java中的任意一种数据类型，需要在定义方法时事先定义。返回值要和返回值的数据类型相匹配，否则编译时或运行时会报错

* java语法规定，方法执行后没有返回值时，返回值类型要定义为void，代表方法没有返回值

  * 返回值类型为void时，方法体内不能出现"return 返回值;"语句，但可以编写"reture;"

* 返回值类型若不是void，当方法执行结束时没有返回任何数据的话，编译器会报错

  * 方法具有返回值的时候，可以选择接收或是不接收。如果选择接收，那么要选择和返回值数据类型相匹配变量

* 只要带有return关键字的语句执行，return语句所在的方法结束



**方法名**：

* 方法名可以为任意符合java语法的标识符



**形式参数列表**：

* 形参是局部变量，仅在方法内使用
* 形参可以是多个，多个形参之间使用","隔开
* 形参的定义方式：(数据类型 形参名, ...)
* 数据类型是java中的任意一种数据类型
* 形参名是任意符合java语法的标识符
* 实参列表和形参列表必须满足：
  * 数量相同
  * 数据类型对应相同



**方法体**：

* 方法体由java语句构成



### 方法的分类

```text
|-- 静态方法
|-- 实例方法
|-- 构造方法
```



* 静态方法：有关键字static修饰的方法
* 实例方法：没有关键字static修饰的方法
* 构造方法：特殊的方法，用于实例化对象



### 方法调用的语法结构

```java
public class method {
    public static void main(String[] args) {
        
        // 静态方法通过"类名.方法名(实际参数列表);"调用
        // 静态方法在方法区内存加载，不需要创建实例
        method.staticMethod();
        
        // 实例方法通过"引用.方法名(实际参数列表);"调用
        // 调用实例方法需要先创建实例
        method mtd = new method();
        mtd.instanceMethod();
        
        // 静态方法也可以使用引用的方式访问，但执行时与引用的对象无关
        // 通过引用映射到类，增加了程序的开销，因此不建议使用引用的方式访问
        // 后续不再赘述，默认静态方法不使用引用的方式访问
        mtd = null;
        mtd.staticMethod();
    }
    
    public static void staticMethod() {
        System.out.println("staticMethod");
    }
    public void instanceMethod() {
        System.out.println("instanceMethod");
    }
}
```



方法只定义不调用是不会执行的，只有在方法调用的时候才会执行。



### 方法调用的参数传递原理

* java语言中，方法调用中传递的永远是变量中保存的数据，而不是变量所在的内存地址



#### java参数传递_例1

```java
public class parameter {
    public static void main(String[] args) {
        int i = 10;
        add(i);
        System.out.println(i); // 10
    }
    public static add(int i) {
        i++;
        System.out.println(i); // 11
    }
}
```

可以参考methodPrameterTransfer_01.png



#### java参数传递_例2

```java
public class parameter {
    public static void main（String[] args） {
        User user = new User(20);
        add(user);
        System.out.println(user.age); // 21
    }
    
    public static void add(User u) {
        u.age++;
        System.out.println(u.age); //21
    }
}

class User {
    int age;
    
    public User(int i) {
        age = i;
    }
}
```

* main方法压栈
* 堆内存创建User实例，局部变量age赋值20
* main方法user变量引用User实例在堆内存中的内存地址
  * user变量存在内存地址
  * user变量所存储的数据，是一个内存地址
* add方法压栈
* add方法u变量赋值main方法user变量存储的数据，即User实例在堆内存中的内存地址
  * u变量也成为一个引用
* 因此，add方法中的u变量也可以操作User实例

可以参考methodPrameterTransfer_02.png



### 方法的重载

**在同一个类当中，方法名相同，参数列表不同，构成方法重载。**

以如下代码举例说明：

```java
// 没有使用方法重载的情况
public class notOverload{
    public static void main(String[] args) {
        int result_1 = sumInt(1, 2);
        System.out.println(result_1);
        
        double result_2 = sumDouble(1.0, 2.0);
        System.out.println(result_2);
        
        long result_3 = sumLong(1L, 2L);
        System.out.println(result_3);
    }
    
    // 以下三个方法，功能相似，调用时需要记住三个方法名
    public static int sumInt(int a, int b) {
        return a + b;
    }
    
    public static double sumDouble(double a, double b) {
        return a + b;
    }
    
    public static long sumLong(long a, long b) {
        return a + b;
    }
}
```



```java
// 使用方法重载的情况
public class overload{
    public static void main(String[] args) {
        System.out.println(sum(1, 2));
        System.out.println(sum(1.0, 2.0));
        System.out.println(sum(1L, 2L));
    }
    
    // 以下三个方法构成了方法重载机制，只需要记住一个方法名
    public static int sum(int a, int b) {
        return a + b;
    }
    
    public static double sum(double a, double b){
        return a + b;
    }
    
    public static long sum(long a, long b) {
        return a + b;
    }
}
```



方法重载（overload）帮助java工程师处理相似功能，可以合并相似的方法，便于后续调用。



#### 方法重载的实现机制

* **在同一个类当中，方法名相同，参数列表不同，构成方法重载**

  * 功能相似的时候，尽量使用方法重载
  * 功能不相似的时候，不要使用方法重载

* 参数列表不同的情况：

  * 数量不同

  ```java
  public class overload {
      // 名义参数列表的数量不同，以下方法可以构成方法重载
      public static void m1(){
          
      }
      
      public static void m1(int a) {
          
      }
  }
  ```

  

  * 顺序不同

  ```java
  public class overload {
      // 名义参数列表的顺序不同，以下方法可以构成方法重载
      public static void m2(int a, double b) {
          
      }
      
      public static void m2(double a, int b) {
          
      }
  }
  ```

  

  * 类型不同

  ```java
  public class overload{
      // 名义参数列表的类型不同，以下方法可以构成方法重载
      public static void m3(int a) {
          
      }
      
      public static void m3(double a) {
          
      }
  }
  ```

  

* 方法重载和名义参数的名称无关

  ```java
  public class overload {
      // 编译错误：方法重复
      // 方法重载和名义参数的名称无关，以下方法不构成方法重载
      public static void m4(int a) {
          
      }
      
      public static void m4(int variables) {
          
      }
  }
  ```

  

* 方法重载和修饰符列表无关

  ```java
  public class overload {
      // 编译错误：方法重复
      // 方法重载和修饰符列表无关，以下方法不构成方法重载
      void m6() {
          
      }
      
      public static void m6() {
          
      }
  }
  ```

  

* 方法重载和返回值类型无关

  ```java
  public class overload {
      // 编译错误：方法重复
      // 方法重载和返回值类型无关，以下方法不构成方法重载
      public static void m5(int a) {
          
      }
      
      public static int m5(int a) {
          return a;
      }
  }
  ```





### 方法递归

方法递归非常耗费栈区内存，递归算法可以不使用的时候，尽量不使用。

方法递归容易出现栈内存溢出错误（java.lang.StackOverflowError），错误发生无法挽回，只有一个结果，JVM停止工作。

方法递归必须设计好结束条件，但即使结束条件正确，也可能出现栈溢出，因为栈内存是有限的。



# 面向对象和面向过程

**面向对象的三大特征：封装、继承和多态**。所有面向对象的编程语言都有这三大特征。



### 面向对象

* **面向对象（Object Oriented）**：
  * 将现实世界分割成不同的单元，每个单元都是独立的对象，在环境的驱动下，各对象协作而形成系统
  * 对象之间的关系是低耦合的，对象增加或减少，不影响已有的系统
  * 万物皆对象，面向对象的核心是抽象出独立的对象，主要关注对象能完成哪些功能
  * 优点：
    * 耦合度低，扩展性强
    * 更容易解决现实世界中复杂的业务逻辑
    * 组件复用性强
  * 缺点：
    * 前期投入成本较高，需要进行对象的抽取，大量的系统分析与设计



面向对象可以分为三个阶段：

* 面向对象的分析（Object Oriented Analysis）
* 面向对象的设计（Object Oriented Design）
* 面向对象的编程（Object Oriented Progranmming）：对扩展开放，对修改关闭



### 面向过程

* 面向过程（Process Oriented）：
  * 核心是具体过程，是因果关系，没有独立体的概念，复杂的因果关系构成了整个系统
  * 因果关系发生改变，系统便需要改造
  * 优点：
    * 对于业务逻辑简单的程序，可以快速开发
  * 缺点：
    * 很难解决非常复杂的业务逻辑
    * 软件元素的耦合度非常高，软件扩展性很差
    * 由于没有独立体的概念，无法实现组件复用



C语言是纯面向过程的，C++半面向对象，Java纯面向对象。现在新的编程语言多数都是面向对象的，面向对象更符合人的思维方式。



# java面向对象



### 类、对象和实例

对象是真实存在的，但是类是不存在的。



什么是类？

* 类在现实世界中是不存在的，是一个模板，是一个概念，是人类大脑思考抽象的结果。
* 类代表了一类事物
* 在现实世界中，对象A和对象B之间有共同特征，进行抽象总结出一个模板，这个模板被称为类



什么是对象？

* 对象是实际存在的个体，现实世界中实际存在
* java工程师观察现实世界中的对象，经过抽象形成思维中的模板，而后使用java语言创建java类，再由java类创建java对象
* java对象使用java方法实现特定功能，互相协作而形成系统



**类 -- 【实例化】 --> 对象（实例/instance）**

**对象 -- 【抽象】 --> 类**



类描述的是对象的共同特征，一个类主要描述的是对象的状态和动作

* 状态 --> 一个类的属性
* 动作 --> 一个类的方法



#### 类声明的语法结构

```java
[修饰符列表] class 类名 extends 父类名 implements 接口名 {
    // 属性;
    // 方法
    
    /*
        静态代码块 static{}
        实例代码块 {}
        
        静态变量： modifiersList static variablesType variable;
        实例变量; modifiersList variablesType variable;
        
        构造方法 modifiersList className(variablesList){}
        静态方法 modifiersList static methodName(variablesList){}
        实例方法 modifiersList methodName(variablesList){}
    */
}
```



#### 实例（对象）的创建

对象是客观存在的，因此我们把java内部的对象称为“实例”

```java
new 类名();
```

new运算符的作用是创建实例



### 实例（对象）的引用

* java实例及实例变量存储在堆内存中

* java实例在堆内存中的内存地址，赋值给一个变量，这个变量被称为“引用”

  ```java
  // SampleClass是一个类，且存在无参构造方法
  // new SampleClass();创建了一个实例
  // 实例的内存地址赋值给了变量sample
  // sample就是一个引用
  SampleClass sample = new SampleClass();
  ```

  

* java实例只能通过引用来访问

  * 通过引用访问实例方法和实例变量

  ```java
  SampleClass sample = new SampleClass();
  
  // 访问实例变量
  sample.sampleVarables;
  
  // 访问实例方法
  sample.sampleMethod();
  ```

  



#### java实例的引用_例1



```java
public class instance {
    public static void main(String[] args) {
        // 方法体 局部变量
        int i = 10;
        
        // Student是一个引用数据类型
        // std是变量名
        // new Student()是一个Student类型的实例
        Student std = new Student();
    }
}

class Student {
    //类体 成员变量
    short age;
    boolean sex;
    String name;
}
```

以上代码片段的运行过程：

* 方法区内存加载instance.class

* 栈内存加载main方法

  * 栈内存存储变量i，数据是10

* 方法区内存加载Student.class

* 堆内存创建一个Student实例

  * age默认赋值0，sex默认赋值false，

  * name默认赋值null
  * age，sex，name都属于实例变量

* 栈内存中存储变量std，std存储的是Student对象在堆内存中的内存地址

  * 变量std被称为引用，而不是对象

可以参考instanceCreated_01.png



java中没有指针，无法直接操作堆内存，只能通过“引用”去访问堆内存中实例的实例变量

* 引用：引用是一个变量，只不过这个变量中存储的是实例（java对象）的内存地址

访问实例变量的语法：

```java
// 读取实例变量
引用.变量名
// 修改实例变量
应用.变量名 = 数据值;
```



#### java实例的引用_例2



```java
public class instance {
    public static void main(String[] args) {
        Student std = new Student();
        
        std.age = 18;
        std.name = "zhangsan";
        std.score = new Score();
        
        std.score.literature = 100;
        std.score.overview = "优秀";
    }
}

class Student{
    short age;
    boolean sex;
    String name;
    Score score;
}

class Score{
    short literature;
    short math;
    short english;
    String overview;
}
```



以上代码片段的运行过程：

* 方法区内存加载instance.class
* 栈内存加载main方法
* 方法区加载Student.class
* 堆内存创建一个Student实例（以下简称stu实例），并默认赋值
* 栈内存创建一个变量stu，并赋值Student实例在堆内存中的内存地址
* 堆内存中stu实例的age变量赋值18
* 方法区加载String.class方法
  * String的main方法执行不做叙述
* 堆内存创建一个String实例，默认变量赋值"zhangsan"
* 堆内存中stu实例的name变量赋值String实例"zhangsan"在堆内存中的内存地址
* 方法区加载Score.class方法
* 堆内存创建一个Score实例（以下简称stu.score实例），并默认赋值
* 堆内存中stu实例的score变量赋值Score实例在堆内存中的内存地址
* 堆内存中stu.score实例的literature变量赋值100
* 堆内存创建另一个String实例，默认变量赋值"优秀"
* 堆内存中stu.score实例的overview变量赋值String实例"优秀"在堆内存中的内存地址

可以参考instanceCreated_02.png



引用数据类型默认赋值为null，如果需要使用或引用，需要首先实例化引用数据类型。



#### java实例的引用_例3



```java
public class instance{
    public static void main(String[] args){
        Student std = new Student();
        
        Score score = new Score();
        
        std.score = score;
    }
}

class Student{
    short age;
    boolean sex;
    String name;
    Score score;
}

class Score{
    short literature;
    short math;
    short english;
    String overview;
}
```



以上代码片段的运行过程中：

* main方法中创建Score实例，并将内存地址赋值给变量score
* 然后，将score变量的值，即Score实例的内存地址，赋值给std实例中的score变量
* 因此，std实例中的score变量，获得了Score实例的内存地址，可以操作Score实例



可以参考instanceCreated_03.png



按照指针的方式理解。



#### java实例的引用_例4



```java
public class instance{
    public static void main(String[] args){
        Husband husband = new Husband();
        Wife wife = new Wife();
        
        husband.wife = wife;
        wife.husband = husband;

    }
}

class Husband{
    String name;
    Wife wife;
}

class Wife{
    String name;
    Husband husband;
}
```



引用数据类型的默认值为null，而且必须手动进行实例化。



#### java.lang.NullPointerException

空引用访问“实例”相关数据，一定会出现java空指针异常

```java
public class nullpointer{
    public static void main(String[] args){
        Test test = new Test();
        System.out.println(test.id); //0
        
        test = null;

        // 以下程序编译可以通过，因为符合语法，但运行时会出现空指针异常
        // 空引用访问“实例”相关数据，一定会出现空指针异常
        // java.lang.NullPointerException
        System.out.println(test.id); 
    }
}

class Test{
    int id;
}
```





### 类的封装 Encapsulate

* 类在封装之前，可以随意实例化类和访问实例的属性，具有复杂性和不安全性
* 类在封装之后，只会提供简单的操作入口，不需要外部关心其内部的实现原理
  * 例如照相机，只提供拍摄功能，用户不需要关心其内部逻辑
* 类在封装之后，才会形成真正的对象，成为独立体，可以适应更多的场合，提高了安全性



#### 封装的步骤

* 类的所有属性私有化，使用private修饰符修饰

  * private表示私有的，在类中修饰的变量只能在当前类中使用

* 对外提供简单的操作入口

  * 一个属性通常的访问包括两种形式：读取属性的值、修改属性的值

  * 对外提供两个公开的方法，分别是getter方法和setter方法

    * 命名规范

    ```java
    public class Encapsulation {
        private int test;
        
        public 数据类型 get首字母大写的变量名全拼 {
            return 变量名;
            
            /*
                例如：
                public int getTest {
                    return test;
                }
            */
        }
        
        public void set首字母大写的变量名全拼(数据类型 不同的变量名) {
            /*
                例如：
                public void setTest(int t) {
                    test = t;
                }
            */
        }
    }
    ```

  * getter方法要有返回值

  * setter方法没有返回值，因为set方法只负责修改数据

* getter和setter方法定义是不加static修饰符

  ```java
  // 有static修饰符的方法调用方式：
  类名.方法名(实参);
  
  // 没有static修饰符的方法调用方式：
  引用.方法名(实参);
  ```

* 在getter和setter方法执行过程中，可以进行安全控制



### 构造方法

构造方法又被称为构造函数、构造器、Constructor



#### 构造方法声明的语法结构

```java
[修饰符列表] 类名(形式参数列表) {
    构造方法体;
}
```

* 构造方法可以使用的修饰符：
* * public
  * protected
  * private
* 构造方法不需要声明返回值类型，也不能加入void修饰符，加入void后便不再是构造方法
* 构造方法的方法名必须和类名保持一致



#### 构造方法调用的语法结构

```java
new 构造方法名(实参列表);

// java中new修饰符就是为了创建类的实例
// 对于默认的构造
```



#### 构造方法的运行分析

* **构造方法的作用：**
  * **构造方法可以创建实例**
  * **构造方法创建实例的同时，会初始化实例变量的内存空间，同时为实例变量默认赋值**
* 构造方法执行结束后实际上是存在返回值的，但是不需要写"return 数据值;"这样的语句
  * 构造方法运行结束时java程序自动返回值
  * 返回值的数据类型就是构造方法所在类的类型
  * 返回值可以理解为堆内存中该实例所在的内存地址
  * 由于构造方法的返回值类型就是类本身，所以构造方法在声明时不需要写返回值类型
* java中的每一个类都有构造方法，如果构造方法没有被显式定义（在源程序中显示），那么java编译时会默认提供一个无参构造方法
  * 默认的无参构造方法被称为缺省构造器
* 当一个java类的构造方法被显式定义，java编译时便不再为该类提供缺省构造器
* 建议开发过程中，都显式定义无参构造方法，因为无参构造方法非常常用
* 构造方法支持方法重载机制

```java
public class constructor {
    // 三个Test构造方法构成方法重载
    Test t1 = new Test();
    Test t2 = new Test(100);
    Test t3 = new Test("zhangsan");
}

class Test {
    // 无参构造方法
    // 建议每个类均提供无参构造方法，便于使用
    public Test() {
        
    }
    
    // 有参构造方法，一个int类型的参数
    public Test(int i) {
        
    }
    
    // 有参构造方法，一个String类型的参数
    public Test(String i) {
        
    }
}
```



#### 缺省构造器





### java继承 Inheritance

继承最基本的作用是：代码复用。继承最重要的作用是：有了继承，才有了方法覆盖（override）和多态。

java中继承包括类的继承、接口的继承。



#### 类继承声明的语法结构

此处仅描述java类的继承，接口单列章节描述

```java
[修饰符列表] class 类名 extends 父类名 {
    //属性;
    //方法
}
```



* java中的类只支持单继承，一个类不能同时继承很多类

  * C++支持多继承

* java中一个类可以间接继承其他类

  ```java
  class d extends c {}
  class c extends b {}
  class b extends a {}
  // d类直接继承c类
  // d类简介继承b类、a类
  ```

  

* java中如果一个类没有显式继承任何类，那么该类默认继承JavaSE中的java.lang.Object类

  * 继承的根源是java.lang.Object类
  * java中任何一个类都有Object类中的特征

* 术语，B类继承A类，其中：

  * A类被称为：父类、基类、超类、superclass
  * B类被称为：子类、派生类、subclass



#### 继承的对象

* 私有元素不支持继承

* 构造方法不支持继承

* 其他类型的元素都可以被继承

  * 详情参考“java关键字-访问控制权限”
  * 子类中可以定义与父类同名的变量
    * 如果要访问父类中的同名变量，要使用super修饰符，详情参考super关键字

  * 子类中可以定义与父类同名的方法
    * 注意：同名方法有可能构成方法重写
    * 如果要访问父类中的同名方法，要使用super修饰符，详情参考super关键字



#### 继承的内存分析

* 实例化子类时，子类的构造方法默认调用了父类的构造方法，但并不会生成实例，只是继承了父类的特征
* 详情参考super关键字



### 方法重写 override

当父类中的方法无法满足子类的需求时，可以在子类中重写父类方法，这种重写的方式称为方法重写或方法覆盖。



#### 方法重写的语法结构

* 子类继承了父类的方法
* 子类重写父类方法时，需要与父类方法的声明方式保持一致，抽象方法例外
  * 修饰符列表的访问权限不能更低，可以更高
    * private < default < protected < public
  * 抛出异常不能更多，可以更少
  * 返回值类型相同
  * 方法名相同
  * 形式参数列表相同
* 静态方法不存在重写
* 重写只针对方法，不含变量

```java
class Animal {
    public void move() {
        System.out.println("动物在移动");
    }
    protected void eat() throws Exception {
        System.out.println("动物要吃东西");
    }
}

class Cat extends Animal {
    public void move() {
        System.out.println("猫在移动");
    }
    // 访问权限可以更高，抛出的异常可以更少
    public void eat() {
        System.out.println("猫要吃东西");
    }
}

public class override {
    Cat cat = new Cat();
    cat.move();
    cat.eat();
}
/*
    输出：
    猫在移动
    猫要吃东西
*/
```



* 抽象方法的重写详情参考“抽象方法”



#### 方法重写的实现机制

todo



### java多态 Polymorphism

封装抽象了独立体

继承使对象和对象之间存在继承关系

多态使父类型的引用却可以指向一个子类型的对象，动态绑定要实现的方法



术语：

* 向上转型（upcasting）：子类型转换为父类型
  * 自动类型转换是一种向上转型
* 向下转型（downcasting）：父类型转换为子类型转型
  * 强制类型转换是一种向下转型，需要加强制类型转换符
* 无论是向上转型还是向下转型，两种类型间必须存在继承关系，否则会编译错误，接口除外
  * 接口之间类型互转，不需要存在继承关系也可以强制类型转换，详情参考java接口



#### 多态的语法结构

以如下代码举例说明：

```java
class Animal {
    public void move() {
        System.out.println("动物在移动");
    }
}

class Cat extends Animal {
    public void move() {
        System.out.println("猫在走猫步");
    }
    public void catchMouse() {
        System.out.println("猫会抓老鼠");
    }
}

class Bird extends Animal {
    public void move() {
        System.out.println("鸟在飞翔");
    }
    public void eat() {
        System.out.println("鸟在吃东西");
    }
}

public class override {
    
    public static void main(String[] args) {
        // Animal和Cat之间存在继承关系，Animal是父类，Cat是子类
        // new Cat();是一个Cat类型的子类对象，而后赋值给Animal类型的父类对象
        // 子类型转换为父类型，进行自动类型转换
        // java中允许这种语法：父类型引用指向子类型对象
        Animal cat = new Cat();
    
        // 编译错误：找不到catchMouse方法
        // 变量cat的数据类型是Animal类型，而编译器在Animal类中没有找到catchMouse方法，导致静态绑定失败
        cat.catchMouse();
    
        // 编译通过
        // java程序永远分为编译阶段和运行阶段
        // 在程序编译阶段，编译器检查使用Animal类型的数据找到了Animal类中move方法。这个过程称为：“静态绑定”或“编译阶段绑定”
        // 只有编译阶段静态绑定成功之后，才能有后续的运行阶段
        // 在程序运行阶段，由于变量cat底层引用的是JVM堆内存中的Cat实例，因此执行的是Cat类中的move方法，而不是Animal类中的move方法。这个过程称为：“动态绑定”或“运行阶段绑定”
        // 无论Cat类有没有重写move方法（Cat类继承Animal类中的move方法），运行阶段一定调用的是Cat对象的move方法
        // 父类型引用指向子类型对象的这种机制，导致程序存在编译阶段绑定和运行阶段绑定两种不同的形态，这种机制可以称为一种多态语法机制
        cat.move();
        
        
        // 当调用的方法或访问的属性是子类特有的，在父类中不存在时，此时需要进行向下转型
        // 父类型转换为子类型，进行强制类型转换，需要强制类型转换符
        // 转换之后编译可以通过，Cat类型的数据在Cat类中找到了catchMouse方法
        Cat cat2 = (Cat)cat;
        cat2.catchMouse();
        
        // 以下程序可以编译通过，但运行阶段会出现异常
        // 编译器检查Animal类和Cat类之间存在继承关系，因此认为Animal类型的数据bird可以强制类型转换为Cat类型的数据cat3，语法合格，不会报错
        // 但是运行阶段会出现异常，因为JVM堆内存中存在的是Bird类型的对象，Bird类型和Cat类型之间没有继承关系，此时便会出现java.lang.ClassCastException，类型转换异常
        // 这种异常只可能在“向下转型”时发生，其他情况不会出现这个异常
        Animal bird = new Bird();
        Cat cat3 = (Cat)bird;
        
        // 向下转型存在隐患：编译通过，但运行时出现异常
        // 向上转型只要编译通过，运行时不会出现异常
        // 使用instanceof运算符可以避免出现以上的异常
        if(bird instanceof Cat) {
            Cat cat3 = (Cat)bird;
            cat3.catchMouse();
        }else if(bird instanceof Bird) {
            bird.eat();
        } 
    }
}
```



java向下转型存在隐患：编译通过，但运行时出现异常

向上转型只要编译通过，运行时不会出现异常

使用instanceof运算符可以避免出现以上的异常



#### instanceof关键字

语法格式：

```java
引用 instanceof 数据类型名
```



以上运算符的执行结果类型时布尔类型，结果可能是true和false

例如 cat instanceof Animal

* true：引用cat指向的实例是Animal类型
* false：引用cat指向的实例不是Animal类型



java规范中要求，在进行强制类型转换之前，建议采用instanceof运算符进行判断，避免出现ClassCastException



#### 多态的实现机制





#### 多态的使用_例1

```java
class Master {
    public void feed (Pet pet){
        pet.eat();
    }
}
class Pet {
    public void eat() {
        
    }
}
class Cat extends Pet {
    public void eat() {
        System.out.println("猫爱吃鱼");
    }
}
public Dog extends Pet {
    public void eat() {
        System.out.println("狗爱吃肉");
    }
}

public class Test {
    public static void main(String[] args) {
        Master master = new Master();
        
        // 通过多态机制，父类型自动引用指向子类型的对象
        // 运行时，使用子类型的方法
        // 高度抽象，降低了程序耦合度
        master.feed(new Cat());
        master.feed(new Dog());
    }
}
```



### 抽象类

```text
抽象类
...
|-- 抽象类
|   |-- 类
|   |   |-- 对象
```



* 对象是现实世界中存在的，人在观察对象的过程中，对对象进行抽象而形成类

* 类和类之间可能具有共同给特征，对类再进一步抽象，形成的就是抽象类
  * 抽象类和抽象类之间可能还有共同特征，还可以进一步抽象，形成多层次的抽象类

* 由于类本身是不存在的，所以抽象类无法实例化，无法直接创建对象

* 抽象类也是类，属于引用数据类型



#### 抽象类声明的语法结构

```java
[修饰符列表] abstract class 类名 {
    java语句;
}
```



#### 抽象类的使用

* 抽象类是无法实例化的，所以抽象类是用来被子类继承的
  * abstract修饰符和final修饰符不可同时存在
    * abstract修饰的类只能被继承，final修饰的类不能被继承，存在矛盾
* 抽象类的子类可以是抽象类，也可以不是抽象类
  * 字类是非抽象类时，才可以实例化对象
* 抽象类虽然无法实例化，但抽象类有构造方法
  * 抽象类的构造方法是供子类使用的



### 抽象方法

没有方法体且被abstract关键字修饰的方法叫做抽象方法，只有一个行为意向，但没有具体的实现。

* 没有方法体的方法不一定是抽象方法
  * native修饰的是调用JVM本地C++动态链接库的方法
  * abstract修饰的是抽象方法
* 抽象类中不一定有抽象方法
* 但抽象方法的只能出现在抽象类中
  * 如果父类是抽象类且有抽象方法，子类也是抽象类，那么子类可以继承或重写父类的抽象方法
  * 如果父类是抽象类且有抽象方法，但子类不是抽象类，那么子类必须重写父类的抽象方法



#### 抽象方法声明的语法结构

```java
[修饰符列表] abstract 方法名();
// 抽象方法没有方法体{}，以分号结尾
```



#### 抽象方法的重写

抽象方法的重写，又称抽象方法的实现。

去掉abstract关键字，加上方法体{}即可。

```java
abstract class Animal {
    public abstract void move();
}

class Cat extends Animal {
    // 1、去掉abstract关键字
    // 2、加上方法体{}
    public void move() {
        // java语句;
    }
}
```



#### 面向抽象编程

抽象方法继承是java多态最巧妙的体现之一。

````java
public class abstractTest {
    public static void main(String[] args) {
        // 使用父类型指向子类对象，且无论指向哪一个子类对象，都可以使用某个方法
        // 子类可以随意扩展，扩展性极大地提升了
        Animal cat = new Cat();
        cat.move(); // 猫在地上走
        Animal bird = new Bird();
        bird.move(); // 鸟在天上飞
    }
}

abstract class Animal {
    public abstract void move();
}

class Cat extends Animal {
    public void move() {
        System.out.println("猫在地上走");
    }
}

class Bird extends Animal {
    public void move() {
        System.out.println("鸟在天上飞");
    }
}
````



# java关键字

### this关键字

this关键字指向当前调用的对象。

* this是一个变量，存储在JVM堆内存的java实例内部
* this变量中保存了java实例自身的内存地址，即指向自身
  * this是一个引用
  * 创建100个java实例，便会有100个this变量，每个实例内部有一个
* this代表的就是当前正在使用的实例



#### this关键字的使用

* **this关键字只能在实例方法和构造方法内使用**
  * this不能在静态方法内使用



* this关键字可以在局部变量和成员变量重名时，指定调用当前的成员变量

  ```java
  public class This {
      private int id;
      
      public void setId(int id) {
          // 但两个id访问的都是当前的局部变量id
          id  = id;
          
          // this.id访问的是实例变量id
          this.id = id;
      }
  }
  ```

  

* this关键字可以在在构造方法内调用另一个构造方法

  * 使用this调用另一个构造方法时，this必须在第一句执行，在它之前不能有其它代码，否则编译错误
    * 一个构造方法内只可能调用一次另一个构造方法

  ```java
  public class This {
      private int year;
      private int month;
      private int day;
      
      // 有参构造方法
      public This(int year, int month, int day) {
          this.year = year;
          this.month = month;
          this.day = day;
      }
      
      // 无参构造方法
      public This() {
          // 可以使用this调用当前实例中的另一个构造方法
          // 节省代码量
          /* 
          this.year = 1970;
          this.month = 1;
          this.day = 1;
          */
          this(1970, 1, 1);
      }
  }
  ```

  

* 在一个构造方法内，this关键字和super关键字不可同时存在

  * 二者都要求出现在构造方法第一行，彼此相斥

* this在多数情况下都是可以省略不写的

  * 为了便于理解，最好写上



#### this调用的语法结构

```java
// 调用实例变量：
this.实例变量

// 调用实例方法：
this.实例方法(实参);

// 构造方法内调用另一个构造方法：
this(实参);
```

 

### super关键字

**super代表的时“当前对象（this）”的“父类型特征”。**



#### super关键字的使用

* **super关键字只能在实例方法和构造方法内使用**
  * super不能在静态方法内使用

* super();只能出现在构造方法的第一行，通过当前的构造方法调用“父类”中的构造方法，目的是，代码复用
  * 在创建子类之前，先初始化父类特征
* 在一个构造方法内，super关键字和this关键字不可同时存在
  * 二者都要求出现在构造方法第一行，彼此相斥
* 在一个构造方法内，没有显式定义this();或super();的话，默认会有一个super();，表示通过当前子类的构造方法调用父类的无参构造方法
  * 必须保证父类的无参构造方法是存在的
  * 建议每一个类都手动创建无参构造方法，如果无参构造方法丢失，可能会影响子类的创建
* 在一个类的实例化过程中，一定会在该类的一个构造方法内执行一次super()，调用父类的构造方法
  * 无论这个构造方法是否被显式定义
  * java类的实例化过程中，Object类中的无参构造方法处于栈顶部，最后调用，但最先结束
    * 因为每个类在实例化时均会执行父类的构造方法



```java
public class Super {
    public static void main(String[] args) {
        // 创建子类实例
        new SubClass();
    }
}
class ParentClass {
    public ParentClass() {
        // 父类无参构造方法
        // 默认存在super();语句，如果类没有显式继承声明，那么默认继承Object类
        System.out.println("ParentClass Constructor!");
    }
}
class SubClass {
    public SubClass() {
        // 子类无参构造方法
        // 默认存在super();语句，默认调用父类的无参构造方法
        // 即创建子类实例之前，先初始化父类特征，但不是创建父类实例
        System.out.println("SubClass Constructor!")
    }
}
/*
    运行结果：
    ParentClass Constructor!
    SubClass Constructor!
*/
```



#### super调用的语法结构

```java
// 子类实例中访问父类的实例变量：
super.实例变量	

// 子类实例中访问父类的实例方法：
super.实例方法(实参);

// 子类的构造方法内调用父类的构造方法：
super(实参);
```



#### super()的作用

java中的类最终都由Object类继承而来，每一个类在实例化过程中都执行了super()，但是：

* super()的作用是继承父类型的特征，并不是实例化父对象，类的实例化过程中只创建了一个对象
  * 理解：我继承了我父亲的一部分特征，例如眼睛、皮肤等，但创建我的时候并不需要创建父亲，只要看到父亲便可以获取这些特征，这部分特征继承之后是属于我的
* 父类型特征包括一切可以继承的属性和方法
  * 修饰符列表中的访问控制权限决定了是否可以被继承
* * 



#### super和this的区别

* super不是引用，this是引用
  * super不保存内存地址，不指向任何实例
  * this保存了当前实例的内存地址，指向实例自身
* super代表的是实例内部的父类型特征，是实例所在内存空间中的一部分内存空间

* super代表当前对象的父类型特征，因此super是this的一部分
* super在继承父类型特征时，会根据访问控制权限继承，而this可以访问当前实例的所有特征
* super和this执行的优先级可以按照如下方式理解：

```java
// 仅供理解使用
this. = super. < this() < super()
```



### static关键字

static代表静态的，所有static修饰的元素都是静态元素。

静态元素是对象统一特征的抽象：对象存在某一个属性或动作，但所有的对象都有统一的属性或动作，例如所有中国人的国籍都是中国，蛋炒饭中都有蛋和饭。

因此静态元素描述的是类级别的特征，而与某一特定对象（实例）无关。



#### static关键字的使用

* static修饰的变量是静态变量
* static修饰的方法是静态方法
* static修饰的代码块是静态代码块

* 所有static修饰的元素，都可以通过"类名.静态元素"的方式访问
  * 也可以通过"引用.静态元素"的方式访问，通过引用映射类，增加了程序的开销
  * 后续不再赘述，默认静态元素不使用引用的方式访问
* static修饰的静态元素，自上而下依次执行



#### static静态代码块

```java
// 语法
static {
    java语句;
}
```



以如下代码举例说明：

```java
public class staticCodes  {
    static {
        System.out.println("static javacodes 1");
    }
    
    static {
        System.out.println("static javacodes 2")
    }
    
    public static void main(String[] args) {
        System.out.println("main method");
    }
}

/*
    运行结果：
    static javacodes 1
    static javacodes 2
    main method
*/
```

* 静态代码块在.class字节码文件JVM方法区内存加载时执行
* 静态代码块在类加载时都会执行一次
* 静态代码块执行的这个特殊时机叫做”类加载时机“
* 静态代码块在一个类中可以编写多个，自上而下依次执行



#### 实例代码块

```java
// 语法
{
    java语句;
}
```



以如下代码举例说明：

```java
public class instanceCodes {
    {
        System.out.println("instance javacodes 1");
    }
    
    {
        System.out.println("instance javacodes 2");
    }
    
    public instanceCodes() {
        System.out.println("instanceCodes constructor");
    }
    
    public static void main(String[] args) {
        instanceCodes insCds = new instanceCodes();
    }
}

/*
    运行结果：
    instance javacodes 1
    instance javacodes 2
    instanceCodes constructor
*/
```



* 实例代码块在实例创建（堆内存创建）时执行
* 每次创建实例时，实例代码块都会执行一次
* 实例代码块在构造方法执行之前执行，这个特殊时机叫做”对象初始化时机“
* 实例代码块在一个类中可以编写多个，自上而下依次执行



#### static原理解析

static修饰的元素都会在方法区内存存储，类被加载时，方法区最先被加载，且static元素被加载，后续都可以使用。



### final关键字

final代表最终的，不可变的，不能封装、继承、赋值 



#### final关键字的使用

* final修饰的类无法被继承

* final修饰的方法无法被重写

* final修饰的变量一旦赋值之后，不可重新赋值

  * 即变量赋值后不可二次修改

* final修饰的实例变量

  * java语言规定final修饰的实例变量不支持默认赋值，必须显式赋值

  * 可以直接赋默认值，也可以使用构造方法赋值

    * 这两种方式的本质是一种，都是在构造方法执行时为final实例变量赋值

  * final修饰的实例变量是不可变的，重复创建实例浪费内存空间，因此这种变量一般和static联合使用，被称为“常量”

    * java规范种要求常量名大写，每个单词之间使用下划线连接

    * 常量的定义语法：

      ```java
      public static final 基本数据类型 常量名 = 数据值;
      ```

      

* final修饰的引用

  * final修饰的引用一旦指向一个实例，不可再指向另一个实例
  * 这个实例的内存空间是可以修改的
  * 这个实例无法被垃圾回收器回收，除非程序结束



### abstract 关键字

abstract关键字代表抽象的用于修饰类、方法

* abstract关键字修饰的类被称为抽象类
* abstract关键字修饰的方法被称为抽象方法



### 访问控制权限

访问控制权限通过访问控制权限修饰符来控制元素的访问范围



#### 访问控制权限修饰符

访问控制权限修饰符包括

* public：公开的，在任何位置都可以访问
* protected：受保护的，同包或子类内可以访问
* default：缺省的，不添加访问控制权限修饰符时的默认值，只能在同包内访问
* private：私有的，只能在本类的实例中访问



访问控制权限修饰符可以修饰：类、变量、方法、......

访问控制权限的范围：private < default < protected < public



### native关键字

* 当源码中一个程序以";"结尾，且修饰符列表中有native关键字，代表这个方法调用底层C++写的dll程序
  * *.dll：动态链接库文件



# java package和import



### java package

java包，package，使用package语法机制，主要是方便程序的管理。

不同功能的类被放在不同的包内，查找和管理比较方便。



#### package定义

* java源程序的第一行编写package语句

* package只能编写一个语句

* 语法结构

  ```java
  package 包名;
  ```

  

* java包名的命名规范：

  ```java
  公司域名倒序 + 项目名 + 模块名 + 功能名;
  包名要求全部小写，包名也是标识符，要遵守java标识符规范
  一个包对应一个目录，目录之间使用"."连接
  // 例如：
  // package com.apache.tomcat.core;
  // package cn.alibaba.java.project.core;
  ```



#### package使用

* 使用了package机制之后，类名发生改变，变为：包名.类名
  * java的包对应着文件目录
  * 在使用命令行运行java命令（JVM ClassLoader）时，要找到package目录下的原始类名.class文件
    * 由于类名此时已经发生改变，因此要在包的根目录下执行java命令，才能正确找到包目录下的原始类名.class文件
  * 因此在不适用IDE的情况下，javac编译后需要手动创建包目录，将原始类名.class文件手工转移到package目录下的位置
    * 此时新的类名与文件名保持一致，可以运行
* 一种简单的创建包目录的方式：

```java
// java编译并运行
javac -d 编译后存放路径 java源文件路径
```

* 在同一个包内的，类之间互相引用无需添加包名，否则都需要添加包名

```java
package com.user.scr.main;
public class UserName {
    public static void main(String[] args) {
        System.out.println("com.user.src.main.UserName");
    }
}
-------------------------------------------------
-------------------------------------------------
package com.user.src.main;
public class UserSex {
    public static void main(String[] args) {
        System.out.println("com.user.src.main.UserSex");
        
        // 编译通过
        // 这是完整写法，指定包名时，编译器会在指定的包下寻找UserName类
        // 即com.user.scr.main包内的UserName类，发现可以找到，编译通过
        com.user.scr.main.UserName user = new com.user.scr.main.UserName();
        
        // 编译通过
        // 当省略包名之后，java编译器会在当前包下寻找UserName类
        // 即com.user.scr.main包内的UserName类，发现可以找到，编译通过
        UserName username = new UserName();
        System.out.println(username);
    }
}
-------------------------------------------------
-------------------------------------------------
package com.user.test.main;
public class Test {
    public stati void main(String[] args) {
        // 编译通过
        // 这是完整写法，指定包名时，编译器会在指定的包下寻找UserName类
        // 即com.user.scr.main包内的UserName类，发现可以找到，编译通过
        com.user.scr.main.UserName user = new com.user.scr.main.UserName();
        
        // 编译错误
        // 省略包名之后，，java编译器会在当前包下寻找UserName类
        // 即com.user.test.main包内的UserName类，无法找到，编译错误
        UserName username = new UserName();
    }
}
```



### java import

* import语句用来导入其他类
  * 在相同的包内，不需要使用import导入
  * 在不同的包内，建议使用import导入，便于引用



#### import定义

import需要编写在package语句之下，class语句之上

```java
// 单独引入某一个类
import 类名;
// 引入某一个包内的所有类
// *代表所有
import 包名.*;
```



#### import使用

* java.lang.*; 不需要手动引入，系统自动引入
  * lang：language语言包，是java的核心语言类，不需要手动引入
* 不是java.lang包下，且不在同一个包下的时候，需要import手动引入



# System.out.println()解析

```java
System.out.println("HelloWorld!");
// System是一个类
// out后没有()，且无需System实例化，因此out是System类中的静态变量
// println()是out可以使用一个方法，证明out是一个静态的引用，println()是out引用实例的方法

// 查看源码：System类中定义了常量out
// public static final PrintStream out = null;
// out是PrintStrem类型的引用，赋值为null
// println()是PrintStrem类中的方法，且构成了方法重载
// 
```



# java接口

接口是一种特殊的类。



### 接口声明的语法结构

```java
[修饰符列表] interface 接口名 extends 父类接口名 {
    
}
```



### 接口的特征

* 接口是一种引用数据类型
* 接口是完全抽象的
  * 抽象类是半抽象的，所以也可以说接口是特殊的抽象类
  * 接口编译后也是.class字节码文件
* 接口中只包含两部分内容：
  * 常量
  * 抽象方法
* 接口中所有的元素都是public修饰的
  * 接口中声明常量时，public static final 修饰符可以省略不写，编译器缺省补充
  * 接口中声明抽象方法时，public abstract 修饰符可以省略不写，编译器缺省补充
* 接口不能实例化
  * 因此接口中的常量必须手工赋值，不支持默认赋值
* 接口和接口之间没有继承关系时，可以进行强制类型转换，编译可以通过
  * 但是除非引用是null，否则运行会出现ClassCastException异常
  * 异常描述：attempt to cast an object to a subclass of which it is not an instance（尝试将对象转换为其子类，但并不是它的实例）
  * 开发中不要使用非继承关系接口做类型强转



### 接口的继承

* 接口可以继承，且支持多继承，即一个接口同时继承多个接口
  * 类仅支持单继承

```java
interface Bird {}
// 接口的单继承
interface Cat extends Bird {}
// 接口的多继承
interface Animal extends Bird, Cat {}
```



### 接口的实现

* 类和接口之间叫做实现，使用implements关键字
  * 可以理解为子类继承接口
* 类和接口之间支持多实现

```java
interface Cat {}
interface Bird {}
// 单实现
class MyCat implements Cat {}
// 多实现
class Animal implements Cat, Bird {}
// java中的类仅支持单继承，但实际上 ，单继承只是为了简单而出现的
// 现实世界中存在多继承 ，java接口的多实现弥补了类的单继承带来的缺陷
```



* 接口的实现可以使用多态
  * 接口也是一种引用数据类型，可以使用接口的数据类型定义子类的实例，使用子类中的方法



### 面向接口编程

接口实现也是java多态最巧妙的体现之一。

```java
interface Cat {
    void move();
}
class MyCat implements Cat{
    public void move() {
        System.out.println("猫在地上走");
    }
}
class HerCat implements Cat{
    public void move() {
        System.out.println("她的猫也在地上走");
    }
}
public class interfaceTest {
    public static void main(String[] args) {
        Cat cat = new MyCat();
        cat.move(); // 猫在地上走
        Cat herCat = new HerCat();
        herCat.move(); // 她的猫也在地上走
    }
}
```



# java注解

java注解是为了更清晰的描述代码

java注解的源代码在java.lang.annotation包内



### 元注解

java中的元注解有3个，Documented、Retention、Target，其它的注解均为元注解的衍生



* @Documented：标记这些注解是否包含在javadoc中
* @Retention：标识这个注解保存多久
  * SOURCE：仅在源代码中保存，编译时丢弃
  * CLASS：被编译到.class字节码文件中，但是JVM运行时不保留
    * Retention没有声明时，缺省使用CLASS
  * RUNTIME：被编译到.class字节码文件中，且JVM运行时被保留
* @Target



