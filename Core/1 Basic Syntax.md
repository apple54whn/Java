# 1 Basic Syntax

## 1.1 introduce

* **命令提示符（其实没啥用👿，还是用类Unix吧👌）**

    | 命令含义         | 命令提示符（cmd）          | Windows PowerShell |
    | ---------------- | -------------------------- | ------------------ |
    | 切换盘符         | c: **或** C:               | 同左               |
    | 进入文件夹       | cd 文件夹名称              | 同左               |
    | 进入多级文件夹   | cd 文件夹1\文件夹2\文件夹3 | 同左               |
    | 返回上一级       | cd ..                      | 同左               |
    | 直接回根路径     | cd \ **或** cd /           | 同左               |
    | 查看当前目录内容 | dir                        | dir **或** ls      |
    | 清屏             | cls                        | cls **或** clear   |
    | 删除文件         | del                        | del **或** rm      |
    | 退出             | exit                       | exit               |

* **JVM**（Java Virtual Machine ） 

    JVM是一个虚拟的计算机，具有指令集并使用不同的存储区域。负责执行指令，管理数据、内存、寄存器。

    **跨平台**：任何软件的运行，都必须要运行在操作系统之上，而我们用Java编写的软件可以运行在任何的操作系 统上，这个特性称为**Java语言的跨平台特性**，该特性是**由JVM实现的**，我们编写的程序运行在JVM上，而**JVM** 运行在操作系上，**JVM不具有跨平台特性**，但是每个操作系统下都有不同版本的JVM。

* **JRE 和 JDK** 

    * JRE(Java Runtime Environment)：是Java程序的运行时环境，包含 **JVM** 和**运行时所需要的核心类库** 。
    * JDK (Java Development Kit)：是Java程序开发工具包，包含 **JRE** 和**开发人员使用的工具**。

* 安装及环境变量配置

    * JAVA_HOME
    * Path

* 编译与运行

    * **编译**：**javac** Test.java
    * **运行**：**java** Test

## 1.2 keyword & reserved word

被Java语言赋予了特殊含义，用做专门用途的字符串(单词)，关键字中**所有字母都为小写（main不是）**，详细可[查看官网文档](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/_keywords.html)

* 用于定义数据类型的关键字

    class、interface、enum、byte、short、int、long、float、double、char、boolean、void

* 用于定义流程控制的关键字

    if、else、switch、case、default、while、do、for、break、continue、return

* 用于定义访问权限修饰符的关键字

    private、protected、public

* 用于定义类、函数、变量修饰符的关键字

    abstract、final、static、synchronized

* 用于定义类与类之间关系的关键字

    extends、implements

* 用于定义建立实例及引用实例，判断实例的关键字

    new、this、super、instanceof

* 用于异常处理的关键字

    try、catch、finally、throw、throws

* 用于包的关键字

    package、import

* 其他修饰符关键字

    native、strictfp、transient、volatile、assert

* （不是关键字，但也当关键字来用）用于定义数据类型值的字面值

    true、false、null

Java保留字：现有Java版本尚未使用，但以后版本可能会作为关键字使用。goto 、const

## 1.3 Identifier

* Java 对各种**变量、方法和类等要素命名时使用的字符序列称为标识符**。凡是自己可以起名字的地方都叫标识符，比如类的名字、方法的名字和变量的名字等
* **命名规则**： 硬性要求
    * 标识符可以包含 英文字母26个(区分大小写) 、 0-9 、 $（美元符号） 和 _（下划线） 
    * 标识符不能以数字开头
    * 标识符不能是关键字或保留字，但能包含
    * 标识符不能包含空格
    * 标识符严格区分大小写
* **命名规范**： 软性建议
    * 包名：多单词组成时所有字母都小写:xxxyyyzzz
    * 类名、接口名：所有单词的首字母大写（大驼峰式）。 
    * 方法名、变量名： 首字母小写，后面每个单词首字母大写（小驼峰式）。 
    * 常量名：所有字母都大写。多单词时每个单词用下划线连接:XXX_YYY_ZZZ





## 1.4 constant

- **整数**常量、**浮点数**常量、**字符**常量、**字符串**常量、**布尔**常量、**空**常量

## 1.5 variable

- 变量是**内存中的一个存储区域**，该区域的数据可以在同一类型范围内不断变化，变量是程序中最基本的存储单元。

- 格式： 数据类型 变量名 = 数据值;

- **注意**：

    - Java中每个变量必须**先声明，后使用，不赋值不能使用**
    - 在同一个大括号范围内（**作用域内**），变量的名字不可以相同
    - 对于**byte或者short**类型变量，注意其**取值范围**
    - 对于float或者long类型变量，**后缀F、L**不能丢

    

## 1.6 data type & 包装类

### data type

> **byte**是计算机中基本存储单元，bit是计算机中最小的存储单位。

* **基本数据类型**：数值型（整数型、浮点型）、字符型、布尔型

    | 数据类型     | 关键字                            | 内存占用 | 取值范围                       |
    | ------------ | --------------------------------- | -------- | ------------------------------ |
    | 整数类型     | byte                              | 1个字节  | -128~127                       |
    | 整数类型     | short                             | 2个字节  | -2^15~2^15-1 = -32768~32767    |
    | 整数类型     | int（默认）                       | 4个字节  | -2^31~2^31^-1（约21亿）        |
    | 整数类型     | long                              | 8个字节  | -2^63~2^63^-1                  |
    | 单精度浮点数 | ﬂoat（尾数可以精确到7位有效数字） | 4个字节  | -3.403E38~3.403E38             |
    | 双精度浮点数 | double（默认，精度是float的两倍） | 8个字节  | -1.798E308~1.798E308           |
    | 字符型       | char                              | 2个字节  | 2^16 = 0~65535                 |
    | 布尔类型     | boolean                           | 1个字节  | 只有true、false，不能用0等替代 |

    正无穷大、负无穷大、NaN（不是一个数字）都是表示溢出和出错情况的三个特殊浮点型数值

* **引用数据类型**

    * 类（包含字符串String）
    * 接口
    * 数组
    * Lambda（也属于类？）



* **整数类型**

    * **Java各整数类型有固定的表数范围和字段长度**，不受具体OS的影响，以保 证Java程序的可移植性。
    * 默认为 int 型，声明long型常量须后加‘l’或‘L’。通常声明为int型，除非不足以表示较大的数，才使用long。

* **浮点类型**

    * 与整数类型类似，**Java 浮点类型也有固定的表数范围和字段长度**，不受具体操作系统的影响。
    * **浮点类型**可能只是一个**近似值**，并非精确的数值。
    * 默认为double型，声明float型常量，须后加‘f’或‘F’。
    * 浮点型常量有两种表示形式
        * 十进制数形式：如5.12、512.0f、.512 （必须有小数点）
        * 科学计数法形式：如5.12e2、512E2、100E-2
    * float尾数可以精确到7位有效数字（精度很难满足需求），double精度是float的两倍，通常采用double。

* **字符型**

    * char 型数据用来表示通常意义上“字符”（2字节），**Java中的所有字符用来存储Unicode编码，读取到JVM中时会将字符编码转为UTF-16来存储，占两个字节**，可以存储大部分非生僻字（UTF-8位不定长，3个字节存储中文汉字。）。故一个字符可以存储一个字母，一个汉字，或其他书面语的一个字符。

    * 字符型变量的三种表现形式：

        * 字符常量是用单引号(‘ ’)括起来的**单个字符**。例如:char c1 = 'a'; char c2 = '中'; char c3 = '9';

        * Java中还允许使用**转义字符**‘\’来将其后的字符转变为特殊字符型常量。 

            例如`\n`换行符，`\'`单引，`\\`反斜杠，`\b`退格，`\r`回车、`\n`换行

        * 直接使用 **Unicode 值**来表示字符型常量:‘\uXXXX’。其中，XXXX代表 一个十六进制整数。如:\u000a 表示 \n。

    * char类型是可以进行**运算**的。因为它都对应有Unicode码。

    * 字符对应ASCII码（0~127）：48—'0'、65—'A'、97—'a'

* **布尔类型**

    * boolean类型数据只允许取值true和false，无null。

        不可以使用0或非 0 的整数替代false和true，这点和C语言不同。

    * **boolean 在《Java虚拟机规范》给出了4个字节（编译后用int替代），和boolean数组1个字节的定义（当byte数组处理）**

* 注意事项

    * **数据范围与字节数不一定相关**，如**float数据范围比long更加广泛**，但是float是4字节，long是8字节。



* **数据类型转换**

    有多种类型的数据混合运算时，系统首先自动将所有数据转换成容量最大的那种数据类型，然后再进行计算。**byte、char、short之间不会相互转换，他们三者在计算时首先转换为int类型。**

    **参与运算的有byte、char、short**——>**int**——>**long**——>**float**——>**double**（boolean不参与）

    当把任何基本数据类型的值和String进行连接运算时(+)，基本数据类型的值将自动转化为String类型。

    * **自动类型转换/提升**（隐式），范围**小**的类型向范围**大**的**类型提升**，如下：

        **编译器的常量优化**

        对于 byte、char、short 三种类型来说，若右侧赋值的**数值(不能为变量)**没有超过范围，那么javac编译器会自动隐含得帮我们**补上(byte) (short) (char)** ，否则编译报错。

        在给变量进行赋值时，若右侧的**表达式都是常量**，没有任何变量，那么javac编译器将会直接将若干个常量表达式计算得到结果。并根据是否超过范围决定编译成功与否。

        ```java
        byte a = 3;
        byte b = 4;
        byte c = a + b;//运算期间byte类型变量自动提升为int，但int类型不能赋值给byte类型，因此编译失败。
        byte d = 3 + 4;//常量在编译的时候（javac），已经确定了 3+4 的结果并没有超过byte类型的取值范围，可以赋值给d，成功
        
        char ch = 'A';
        System.out.println(ch + 1);//66
        char ch = '';//编译失败
        
        long a = 12345;//自动类型提升
        long b = 12313131312312313;//编译报错，过大的整数，转int失败
        
        float a = 12.3;//编译失败，double转float失败
        
        System.out .println(3+4+“Hello!”); //输出:7Hello!
        System.out.println(“Hello!”+3+4); //输出:Hello!34
        System.out.println(‘a’+1+“Hello!”); //输出:98Hello!
        System.out.println(“Hello”+‘a’+1); //输出:Helloa1，连接运算，不做数值运算！！若是俩char则做数值运算！！
        boolean b = true；
        System.out.println("Hello" + b);//输出：Hellotrue
        ```

    * **强制类型转换**（显式），是自动类型转换的**逆过程**，将容量大的数据类型转换为容量小的数据类型。

        一般不推荐使用，有可能发生**精度损失**（浮点转成整数，直接**丢掉小数**）

        ```java
        double a = 3.5;
        byte b = (byte)a;//3
        
        int a = 128;
        byte b = (byte)a;//-128
        //此处涉及进制转换
        //128的补码为（采用8位演示）：1000 0000
        //强转位byte：1就变为符号位了，代表-128
        ```

        **在使用+=、-=、*=、/=、%=运算符进行赋值时，强制类型转换会自动完成，不会改变原有数据类型；++、--也一样。**

    * 通常，字符串不能直接转换为基本类型，但通过基本类型对应的包装类则可以实现把字符串转换成基本类型。



### <span name="baozhuanglei">包装类（java.lang）</span>

-   针对八种基本数据类型定义相应的引用类型—包装类(封装类)，可以让让基本类型的数据**进行更多的操作**，此时Java才是真正的面向对象。如下其中6种数字类型都继承父类Number。

    | 基本数据类型 | 引用数据类型  |
    | :----------: | :-----------: |
    |     byte     |     Byte      |
    |    short     |     Short     |
    |     int      |  **Integer**  |
    |     long     |     Long      |
    |    float     |     Float     |
    |    double    |    Double     |
    |     char     | **Character** |
    |   boolean    |    Boolean    |

-   **装箱与拆箱**

    -   装箱：将基本数据类型的值转为引用数据类型，提供如下静态方法，其他包装类同理。

        -   `Integer.valueOf(int num/String str)`，字符串必须是基本类型字符串，否则`NumberFormatException`

        -   ~~利用其构造器，可传入基本类型、String。在JDK9时过时~~

            其中Boolean在传入字符串时底层方法为`return "true".equalsIgnoreCase(s)`

    -   拆箱：将引用数据类型的值转为基本数据类型，提供如下成员方法，其他包装类同理。

        -   `intValue()`该方法将Integer对象转为int

-   **自动拆装箱**：**JDK 5.0后支持**基本类型数据和包装类型数据之间可以自动互相转换

-   **基本类型与字符串转换**

    -   基本类型—>字符串：
        -   **`基本类型的值+""`**：最简单方法，常用
        -   **String类的静态方法`valueOf(参数)`**
        -   **包装类的静态方法`toString(参数)`**方法，不是Object类的`toString()`方法，重载
    -   字符串—>基本类型：
        -   **包装类的静态方法`parseXxx()`**
        -   **包装类的静态方法`valueOf(参数)`，转包装类后再转基本类型**

-   **进制转换**

    -   `static String toString(int num,int radix)`               十进制到其他进制`toString`
    -   `static int parseInt(String s,int radix)`                   其他进制到十进制`parseInt`

-   **基本类型的包装类（除浮点数）都有其缓存，Boolean为true、false，Character为0~127，其他为-128~127（其有内部类）**

-   **注意：**

    1.  包装类都重写Object类中的`toString()` 方法，以**字符串**形式返回包装类的基本数据类型的值
    2.  除了Character外，包装类都有`valueOf(String s)`方法，根据String类型参数创建包装类对象
    3.  除了Character外，包装类都有`parseXXX(String s)`的静态方法，将字符串转为基本类型数据



## 1.7 运算符

* **算术运算符**：`+`、`-`、`*`、`/`、`%`、`++`、`--`、`+`（连接）

    * 对于除号`/`，它的整数除和小数除是有区别的。**整数之间做除法时，只保留整数部分而舍弃小数部分**。

        例如：int x=3510;x=x/1000*1000，最终x值为300

    * `++`、`--`：混合运算时比较麻烦，在单独使用时没有区别，只有**变量**才能使用，常量不能使用。**不改变原类型**。

        * 变量前：先算后用

        * 变量后：先用后算

            实际上，不管是前置 ++，还是后置 ++，都是先将变量的值加 1，然后才继续计算的。二者之间真正的区别是：

            * 前置 ++ 是将变量的值加 1 后，使用增值后的变量进行运算的
            * 后置 ++ 是首先将变量赋值给一个临时变量，接下来对原有变量的值加 1，然后使用那个临时变量进行运算。
            * 都不是原子操作

        ```java
        int n = 10;
        n += (n++) + (++n);//32
        ```

    * `+`在字符串中的操作： 连接、拼接字符串

        ```java
        int a = 10;
        int b = 20;
        String str = "hello";
        System.out.println(a + b + str);//30hello
        System.out.println(str+a+b);//hello1020
        ```

    * **取余和取模**（C、C++、java、JavaScript中%是取余，Python中%是取模），只对于整数有意义

        区别在于第一步的商**趋于0(取余)**、**趋于负无穷(取模)**，**取余和取模同符号数结果相同**

        ```java
        取余(结果符号取决于被除数)				取模(结果符号取决于模数)
        5%3=2；					 			5%3=2；
        -5%-3=-2;							 -5%-3=-2；
        5%-3=2;								 5%-3=-1；
        -5%3=-2;							 -5%3=1；
        ```

* **赋值运算符（支持连续赋值）**

    * 基本赋值运算符：`=`

    * 复合赋值运算符：`+=`、`-=`、`*=`、`/=`、`%=`

        **在使用+=、-=、*=、/=、%=复合运算符进行赋值时，强制类型转换会自动完成，不会改变本身的数据类型**

* **比较运算符**：`==`、`!=`、`<`、`>`、`<=`、`>=`、`"Hello" instanceof String`，结果是布尔值，不能连写

* **逻辑运算符**：是用来连接两个布尔类型结果的运算符，结果是布尔值，可以连写

    * `!`：逻辑非，取反
    * `^`：逻辑异或
    * `&&`：短路与，符号左边是false，右边不再运算；若是`&`需运算完
    * `||`：短路或，符号左边是true，右边不再运算；若是`|`需运算完

* **三元运算符**

    * 数据类型 变量名 = 布尔类型表达式？结果1：结果2;
        * 必须同时保证结果1和结果2符合左侧数据类型要求（可以统一为一个类型）
        * 三元运算符的结果必须被使用。赋值或打印

* **位运算符**：不改变原变量数值。**对都是整数的二进制补码**进行的运算！如下解释以及画图时也应用补码来解释！

    * `<<`左移，被移除的高位丢弃，低位空缺位补0。当顶替掉符号位时数值正负改变。

    * `>>`右移，被移位的二进制最高位是0，右移后所有空缺位补0；最高位是1，所有空缺位补1。

    * `>>>`无符号右移，被移位二进制最高位无论是0或者是1，空缺位都用0补。

    * `&`：与运算，如`6 & 3 = 2`

    * `|`：或运算，如`6 | 3 =7`

    * `^`：异或运算，如`6 ^3 = 5`，**一个数异或同一个数2次，则还是这个数**，可用于简单加密

    * `~`：取反运算，如`~ 6 = -7`，正数取反，各二进制码按**补码**各位取反；负数取反，各二进制码按**补码**各位取反

        ```matlab
        00000000 00000000 00000000 00000110				6的原码、补码
        11111111 11111111 11111111 11111001				取反（符号位也要取反）后的补码
        10000000 00000000 00000000 00000111				利用补码求原码（-1，取反），结果为-7
        ```

        ```java
        int i = 21;//....10101，数值位有5个
        int i = -21;
        System.out.println("i << 2 :" + (i << 2));//84，-84
        System.out.println("i << 3 :" + (i << 3));//168，-168
        System.out.println("i << 27 :" + (i << 27));//-1476395008，1476395008
        
        
        int i = 21;//....10101，数值位有5个
        int i = -21;
        System.out.println("i >> 2 :" + (i >> 2));//5，-6
        System.out.println("i >> 3 :" + (i >> 3));//2，-3
        System.out.println("i >> 5 :" + (i >> 5));//0，-1 ，负数右移最小为-1
        
        int i = 21;//....10101，数值位有5个
        int i = -21;
        System.out.println("i >> 2 :" + (i >>> 2));//5，1073741818
        System.out.println("i >> 3 :" + (i >>> 3));//2，536870909
        System.out.println("i >> 5 :" + (i >>> 5));//0，134217727
        // 由此可得，无符号右移对于负数来说无意义
        ```

        

    交换两值

    ```java
    // 最通用
    int temp = num1;
    num1 = num2;
    num2 = temp;
    
    //1.只能用于数值类型；2.可能相加超出范围
    num1 = num1 + num2;
    num2 = num1 - num2;
    num1 = num1 - num2;
    
    //只能用于数值类型
    num1 = num1 ^ num2;
    num2 = num1 ^ num2;
    num1 = num1 ^ num2;
    ```

    程序输出

    ```java
    boolean x = true;
    boolean y = false;
    short z = 40;
    if ((z++ == 40) && (y = true)) {
      z++;
    }
    if ((x = false) || (++z == 43)) {
      z++;
    }
    System.out.println("z = " + z);// 44
    ```

    

    

    

    ![image-20190728142420590](images/image-20190728142420590.png)



## 1.8 流程控制

* **顺序结构**

* **判断语句**

    * If
    * if...else...
    * if...else if...else...
        * if else 和 三元运算符互换：取两数最大值

* **选择语句**

    * switch语句中**表达式数据类型可以是：byte、char、short、int；Enum（枚举）、String(JDK7及之后)**

        ```java
        switch (表达式) {
          case 常量值1:
            语句1；
            break;
          case 常量值2:
            语句2；
            break;
            ......
          default:
            默认语句
            break;//default的break可省略不写，但不建议
        }
        ```

    * case的**穿透性**

        在switch语句中，如果case的后面**不写break**，将出现穿透现象，也就是**不会再判断下一个case**的值，直接向后运行，直到**遇到break**，或者**整体switch结束**。

    * **case后面的值为不可重复的常量，否则编译失败**

    * switch语句格式可以很灵活，**前后顺序可以颠倒**，**break语句也可以省略（例如，季节）**

    * default位置可以很灵活，但推荐放最后！

    * JDK 1.5 中可以在 switch **表达式中使用Enum定义的枚举类的对象**作为表达式, **case 子句可以直接使用枚举值的名字，无需添加枚举类作为限定**

* **循环语句**

    * for：（三角形图形，上右，下左）

        ```java
        for(初始化表达式; 布尔表达式; 步进表达式){
            循环体; 
        }
        ```

        ```java
        int num = 1;
        for(System.out.print('a');num <= 3;System.out.print('c'),num++){
          System.out.print('b');
        }
        //输出结果：abcbcbc
        System.out.println(num);//4
        ```

    * while

        ```java
        初始化表达式;
        while(布尔表达式){
            循环体; 
            步进表达式;
        }
        ```

    * do...while

        ```java
        初始化表达式;     
        do {
        	循环体;
        	步进表达式;
        } while(布尔表达式);
        ```

    * **for 和 while 的区别**

        * 控制条件语句所控制的那个**变量**，在for循环结束后，就不能再被访问到了，而while循环结束还可以继续使用，如果你想**继续使用，就用while**，**否则推荐使用for**。原因是for循环结束，该变量就从内存中消失，能够提高内存的使用效率。 
        * 在**已知循环次数**的时候使用推荐使用**for**，**循环次数未知**的时推荐使用**while**。
        * do while 绝对会执行一次。

* **跳出语句**

    * **break**：**终止switch**或者**当前最近的一层循环**，用在if中没用。

    * **continue**：**结束本次最近的一层循环**，**继续下一次**的循环

    * 可以用label（任意名称）来指定指明要终止的是哪一层语句块，否则只终止一层，标号语句必须紧接在循环的头部。

        ```java
        lable: for(;;)
          ...
          break/continue lable;
        ```

        这两个语句后面不能声明其他语句，因为永远执行不了，编译报错！

* 扩展知识点

    * 死循环：循环中的条件永远为true，死循环的是永不结束的循环。（**死循环后语句执行不到，编译失败**）

        `while(true)`、`for(;;)`

    * 嵌套循环：一个循环的循环体是另一个循环



## 1.9 Array & Arrays

### Array

- **容器**：将多个数据存储到一起，每个数据称为该容器的元素

- **数组（Array）**： 是多个相同类型数据按一定顺序排列的集合，并使用一个名字命名，并通过编号的方式 对这些数据进行统一管理。存储数据**长度固定**的容器，多个数据的**数据类型要一致（存在自动类型转换）**。是**引用数据类型**， 创建数组对象会在内存中开辟**一整块连续的空间**，而数组名中引用的是这块连续空间的**首地址**。它的元素相当于类的成员变量，因此数组一经分配空间，其中的每个元素也被按照成员变量同样的方式被隐式初始化。

- 注意：数组**对象**有**定长**特性，**长度一旦指定，不可更改（因为内存连续）**。**不要把变量名看成数组**

    ```java
    int[] arr1 = new int[3];
    System.out.println(arr1.length);//3
    arr1 = new int[5];
    System.out.println(arr1.length);//5
    ```

- **初始化**：

    - 动态初始化：数组声明且为数组元素分配空间与赋值的操作分开进行（**必须指定长度，二维数组可以只指定第一个**）

        - 元素**默认值**：整型—`0`；浮点—`0.0`；布尔—`false`；引用类型—`null`；

            char—`‘\u0000’`（或`0`），是一个不可打印字符，非`'0'`；

        ```java
        // 先分配空间
        int[] arr = new int[3];
        
        int[][] arr = new int[m][];// 每个一维数组都是默认初始化值null 
        int[][] arr = new int[m][n];// 一维数组的名称分别为arr[0], arr[1], arr[2]...
        
        // 后续赋值
        ```

    - 静态初始化：在定义数组的同时就为数组元素分配空间并赋值

        - 有元素默认值，但是之后又把大括号内容赋值给数组了

        ```java
        // 分配空间并赋值
        int[] arr = new int[]{1,2,3};
        int[] arr = {1,2,3};//上述的简化版
        
        int[][] arr = new int[][]{{...},{...}};
        int[][] arr = {{...},{...}};
        ```

- **【注意】字符串有length()方法，数组有length属性**；Java中多维数组不必都是规则矩阵形式

- 数组内存图

    ![](images\04-两个引用指向同一个数组的内存图.png)

- 数组中涉及的常见算法

    - 数组元素的赋值(杨辉三角、回形数等)
    - 求数值型数组中元素的最大值、最小值、平均数、总和等
    - 数组的复制、反转、查找(线性查找、二分法查找)
    - 数组元素的排序算法
    - 详见doc

- 数组中常见异常：

    - ArrayIndexOutOfBoundsException：索引范围从0～length-1，小或大都不行。（Python中可以）
    - NullPointerExcption



### java.util.Arrays

Arrays工具类是针对**数组进行操作**的工具类，包括排序和查找等大量**静态方法**，常用方法如下：

-   `boolean equals(int[] a,int[] b)`**判断两个数组是否相等**，比较的是内容，顺序也得一致

-   `toString(int[] arr)`将**数组转为字符串**，如[1, 2, 3]

-   `void fill(int[] a,int val)`将指定值填充到数组之中，全部替换为val！

-   `sort(int[] arr,[ T[] a, Comparator<? super T> c ])`    给数组**排序**，无返回值

    -   若是数值，默认按升序
    -   若是字符串，默认按字母升序，先大写后小写，和ASCII表一致
    -   若是**自定义类型**，这个自定义类型需要**`Comparable`或`Comparator`接口的支持**

-   `binarySearch(int[] arr,int key)`**二分查找**，需先排序

-   `asList(int/String等[] arr)`**数组转集合**，**长度不能变！！！**转换的为Arrays中内部类ArrayList！

    需要使用`List<Object> objectList = ArrayList<Object>(Arrays.asList(数组))；`注意：

    ```java
    List arr1 = Arrays.asList(123, 456);
    System.out.println(arr1.size());//2
    
    List arr1 = Arrays.asList(new int[]{123, 456});
    System.out.println(arr1.size());//1
    
    List arr2 = Arrays.asList(new Integer[]{123, 456});
    System.out.println(arr2.size());//2
    ```

-   `copyOf(int/String等[] original, int from, int to)`  **数组拷贝**，底层调用`System.arraycopy()`



## 1.10 main方法

* main()方法作为程序的入口
* main()方法也是一个普通的静态方法
* main()方法可以作为我们与控制台交互的方式。加不加引号都可以（之前：使用Scanner）


## 1.9  JShell脚本工具 

- JShell**脚本**工具是JDK9的新特性。当我们编写的代码非常少的时候，而又不愿意编写类，main方法，也不愿意去编译和运行，这个时候可以使用JShell工具，**一步一步运行代码**。
- 命令行直接输入`JShell`命令，退出`/exit`







## 习题

### 画图

```java
//*
//**
//***
//****
//*****

for (int i = 0; i < 5; i++) {
  for (int j = 0; j <= i; j++) {
    System.out.print("*");
  }
  System.out.println();
}
```

```java
//*****
//****
//***
//**
//*

for (int i = 0; i < 4; i++) {
  for (int j = 0; j < 4-i; j++) {
    System.out.print("*");
  }
  System.out.println();
}
```

```java
//*
//**
//***
//****
//*****
//****
//***
//**
//*

for (int i = 0; i < 5; i++) {
  for (int j = 0; j <=i ; j++) {
    System.out.print("*");
  }
  System.out.println();
}
for (int i = 0; i < 4; i++) {

  for (int k = 0; k < 4-i; k++) {
    System.out.print("*");
  }
  System.out.println();
}
```



### 对角线画圈

```java
//O***O
//*O*O*
//**O**
//*O*O*
//O***O

public static void printPic(int num){
    for (int i = 0; i < num; i++) {
        for (int j = 0; j < num; j++) {
            if(j==i||j==num-1-i){
                System.out.print("O");
            } else {
                System.out.print("*");
            }
        }
        System.out.println();
    }
}
```

### 九九乘法表

```java
public static void multiplicationTable(int num) {
  for (int i = 1; i <= num; i++) {
    for (int j = 1; j <= i; j++) {
      System.out.print(j+"*"+i+"="+j*i+"\t");
    }
    System.out.println();
  }
}
```

### 100以内的所有质数

从2开始，到这个数减1（优化为到`Math.sqrt()`），**都**不能被这个数整除。当有一个能被整除，跳出！

![image-20190811234455956](images/image-20190811234455956.png)

```java
public static void primeNumber(int num) {
  long startTime = System.currentTimeMillis();
  
  // 穷举num以内的质数（包括num）
  for (int i = 2; i <= num; i++) {
    boolean flag = true;
    // 从2到这个数减1
    // for (int j = 2; j < i; j++) {
    // 优化：对本身是质数的优化效果提升特别大
    for (int j = 2; j <= Math.sqrt(i); j++) {
      if (i % j == 0) {
        flag = false;
        break;// 优化：一旦被除尽，则跳出。对非质数有优化效果。
      }
    }
    if (flag) {
      System.out.println(i);
    }
  }
  
  long endTime = System.currentTimeMillis();
  System.out.println(endTime - startTime);
}
```

```java
public static void primeNumber(int num) {
  long startTime = System.currentTimeMillis();

  // 穷举num以内的质数（包括num）
  lable:for (int i = 2; i <= num; i++) {
    // 优化：对本身是质数的优化效果提升特别大
    for (int j = 2; j <= Math.sqrt(i); j++) {
      if (i % j == 0) {
        continue lable;// 优化：一旦被除尽，则继续下一个。对非质数有优化效果。
      }
    }
    System.out.println(i);
  }

  long endTime = System.currentTimeMillis();
  System.out.println(endTime - startTime);
}
```



### 最小公倍数和最大公约数

```java
//获取最大公约数
//1.获取两个数中的较小值
int min = (m <= n)? m : n;
//2.遍历
for(int i = min;i >= 1 ;i--){
  if(m % i == 0 && n % i == 0){
    System.out.println("最大公约数为：" + i);
    break;
  }
}

//获取最小公倍数
//1.获取两个数中的较大值
int max = (m >= n)? m : n;
//2.遍历
for(int i = max;i <= m * n;i++){
  if(i % m == 0 && i % n == 0){
    System.out.println("最小公倍数：" + i);
    break;
  }
}
```

### 水仙花数

输出所有的水仙花数，所谓水仙花数是指一个3位数，其各个位上数 字立方和等于其本身。`153 = 1*1*1 + 3*3*3 + 5*5*5`

```java
for (int i = 100; i < 1000; i++) {
  int bai = i/100%10; //%10可省略
  int shi = i/10%10;
  int ge = i/1%10;
  if(i==bai*bai*bai+shi*shi*shi+ge*ge*ge){
    System.out.println(i);
  }
}
```



### 完数

一个数恰好等于他的因子（除去这个数本身的其他约数）之和，这个数为完数，如：6=1+2+3。找出1000以内所有完数！

```java
public static void wanshu(int num){
  //int factor = 0;
  for(int i = 1;i <= num;i++){
    int factor = 0;
    for(int j = 1;j <= i/2;j++){
      if(i % j == 0){
        factor += j;
      }
    }

    if(i == factor){
      System.out.println(i);
    }
    //重置factor
    //factor = 0;
  }
}
```





### 实现四舍五入

定义round方法，接收一位小数，实现四舍五入运算，并返回结果（+0.5，强转int）

```java
public static int round(double d){
    return (int) (d+0.5);
}
```



### 统计字符次数

统计字符出现次数。利用容量为26的数组保存字符出现次数

```java
public static void showTimes(char[] chs){
    int[] count = new int[26];
    for (int i = 0; i < chs.length; i++) {
        count[chs[i] - 97]++;
    }
    for (int i=0,ch=97 ;i<count.length;i++,ch++){
        if(count[i]!=0){
            System.out.println("符号"+(char)ch+"出现的次数:"+count[i]);
        }
    }
}
```



### int[] x,y[]

以下选项允许通过编译的是：

* `x[0]=y;` no
* `y[0] = x;` yes
* `y[0][0] = x;` no
* ` x[0][0] = y;` no
* `y[0][0] = x[0];` yes
* `x = y;` no，类型需匹配！



### *随机不重复数组

创建一个长度为6的int型数组，要求数组元素的值都在1-30之间，且是随机赋值。同时，要求 元素的值各不相同。

doc



### *打印char数组的题

```java
int[] arr1 = {1,2,3};
System.out.println(arr1);// [I@2f7c7260

char[] arr2 = {'a','b','c'};
System.out.println(arr2);// abc
```



### *数组除首位

定义一个int型的数组:int[] arr = new int[]{12,3,3,34,56,77,432};让数组的每个位置上的值去除以首位置的元素，得到的结果，作为该位置上的 新值。遍历新的数组。

```java
// 错误写法
for(int i= 0;i < arr.length;i++){
  arr[i] = arr[i] / arr[0]; 
}

// 正确写法1，从后到前
for(int i = arr.length – 1;i >= 0;i--){
  arr[i] = arr[i] / arr[0]; 
}

//正确写法2，不如写法1
int temp = arr[0];
for(int i= 0;i < arr.length;i++){
  arr[i] = arr[i] / temp; 
}
```





### *数组元素的赋值

#### *杨辉三角

* 第一行有 1 个元素, 第 n 行有 n 个元素

* 每一行的第一个元素和最后一个元素都是 1

* 从第三行开始, 对于非第一个元素和最后一个元素的元素。

    `yanghui[i][j] = yanghui[i-1][j-1] + yanghui[i-1][j];`

    ```java
    //1.声明并初始化二维数组
    int[][] yangHui = new int[10][];
    
    //2.给数组的元素赋值
    for(int i = 0;i < yangHui.length;i++){
      yangHui[i] = new int[i + 1];
    
      //2.1 给首末元素赋值
      yangHui[i][0] = yangHui[i][i] = 1;
      //2.2 给每行的非首末元素赋值
      //if(i > 1){
      for(int j = 1;j < yangHui[i].length - 1;j++){
        yangHui[i][j] = yangHui[i-1][j-1] + yangHui[i-1][j];
      }
      //}
    }
    ```

#### *回形数

doc



### *数组的复制、反转、查找

#### 数组复制

需new新的

#### 数组反转

```java
public static void reverseArray(int[] arr){
    for (int i = 0; i < arr.length/2; i++) {
        int temp = arr[i];
        arr[i] = arr[arr.length-1-i];
        arr[arr.length-1-i]= temp;
    }
}
//或
public static void reverseArray(int[] arr){
    for (int min=0,max=arr.length-1;min<max;min++,max--) {
        int temp = arr[min];
        arr[min] = arr[max];
        arr[max]= temp;
    }
}
```



#### 二分查找

```java
//前提：所要查找的数组必须有序。
int[] arr = new int[]{-98,-34,2,34,54,66,79,105,210,333};

int dest = -34;
int head = 0;//初始的首索引
int end = arr.length - 1;//初始的末索引
boolean find = false;
while(head <= end){

  int middle = (head + end)/2;

  if(dest == arr[middle]){
    System.out.println("找到了指定的元素，位置为：" + middle);
    find = true;
    break;
  }else if(arr[middle] > dest){
    end = middle - 1;
  }else{//arr2[middle] < dest1
    head = middle + 1;
  }
}

if(find){
  System.out.println("很遗憾，没有找到的啦！");
}
```



### *数组的排序

ppt总结常考的

#### 选择排序

找剩余数组元素中最小的，放在首位

```java
public static void selectSort(int[] arr) {
    for (int i = 0; i < arr.length - 1; i++) {
        for (int j = i + 1; j < arr.length; j++) {
            if (arr[i] > arr[j]) {
                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
...
```

#### *冒泡排序

相邻中最大的。。。

```java
public static void bubbleSort(int[] arr){
    for (int i = 0; i < arr.length-1; i++) {
        for(int j=0;j<arr.length-1-i;j++){
            if (arr[j]>arr[j+1]){
                int temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
...
```



#### *快速排序

