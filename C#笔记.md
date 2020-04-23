[TOC]

---
# 初识C#

---

## 第一段程序Hello World

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
//外部的库，可以直接调用其中的类

namespace helloword//命名空间
{
    class Program//类
    {
        //主函数：每个程序都有一个主函数，主线程
        //主线程代表该程序的由生到死
        static void Main(string[] args)
        {
            Console.WriteLine("hello world");
            Console.ReadKey();//让控制台监听按键，无按键就一直监听
            Console.redekey(true);
            //Redekey中输入true表示控制台不显示输入的键盘内容
        }
    }
}
//语句要写在方法块中，方法写在类块中，花括号包括就叫块
```

**程序结构**

```csharp
using 引用的外部代码库 （里面有各种写好的工具方便我们编写程序）
namespace 命名空间 用来区分代码
{
    class类 
    {
        Main方法（函数）代表这个程序的生命（从生到死）
        {
            所有指令都在这里调用
        }
    }
}

//语句：由；结尾的指令就叫语句
//块:由一对花括号包裹起来的内容就叫块
//注释在一行文段开头用两个//标注，就是注释内联文档 ，对整个程序无任何影响
//方便我们标注自己的程序，去帮助我们理解程序的结构
//多行注释/*  ...... */
```

## 快捷键

| 快捷键        | 效果         | 快捷键         | 效果       |
| ------------- | ------------ | -------------- | ---------- |
| Ctrl+Y        | 重做操作     | Ctrl+S         | 保存项目   |
| Ctrl+Z        | 撤销操作     | Ctrl+K Ctrl+S  | 快速命令   |
| Ctrl+K Ctrl+C | 注释代码     | Ctrl+K Ctrl+U  | 取消注释   |
| Alt+Up Arrow  | 上移代码     | Alt+Down Arrow | 下移代码   |
| F5            | 运行         | Ctrl+F5        | 运行不调试 |
| Alt+Shift+F10 | 导入命名空间 | Ctrl+M Ctrl+O  | 折叠代码   |
>工具-选项-键盘可以更改快捷键

---

# C#基础

---

## 数据类型

```csharp
//SizeOf（） 可以通过这个方法获取一个值类型的大小
Console.WriteLine(sizeof(int));//输出值为4
```

- 计算机只能识别1/0，同理也只能存储1/0

- 一个二进制单位叫做位（ bit ）

- bit就是计算机最小得到储存单位

| 单位  | 换算     |
| ----- | -------- |
| 1Byte | 8bit     |
| 1KB   | 1024Byte |
| 1MB   | 1024KB   |
| 1GB   | 1024MB   |
| 1TB   | 1024GB   |

| **数据类型** | **类型描述**     | 补充                    |
| :----------- | :--------------- | :---------------------- |
| int          | 整型             | 4个字节32位             |
| float        | 单精度浮点型     | 4个字节32位7位有效数字  |
| double       | 双精度浮点型     | 8个字节64位15位有效数字 |
| char         | 字符型           | 2个字节                 |
| string       | 字符串型         | 可扩展类型              |
| bool         | 布尔型表示真与假 | ture & false            |

| 类型   | 有无符号 | 占据字节数 |
| ------ | -------- | ---------- |
| sbyte  | 有       | 1个字节    |
| short  | 有       | 2个字节    |
| int    | 有       | 4个字节    |
| long   | 有       | 8个字节    |
| byte   | 无       | 1个字节    |
| ushort | 无       | 2个字节    |
| uint   | 无       | 4个字节    |
| ulong  | 无       | 8个字节    |

>数据类型的取值范围是首位有符号存符号剩下几位就换算成十进制，无符号就按全部位数换算

---

## 变量与常量

**==常量声明==**：关键字const，常量必须赋初始值，且不能再次赋值即不能修改常量的值

`const float PI = 3.14159f;`

**==变量指得是装载数据的盒子==**:声明一个变量，其在内存中的存放如图

`string s = "Hello Wrold";`

![image](C1388E82CB1C416B91EB3DAB4FA92AF0)

| 值类型 | int、float等 | 存放在内存的栈中                                             |
| ---------- | ------------ | ------------------------------------------------------------ |
| 引用类型 | string等 | 存放在内存的堆中，当声明一个string变量的时候，就在堆中开辟一个空间用来存放数据|

---

## C#的命名规则

### **变量命名法则**

| 规则                                                         | 举例                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1.只能以英文或者_开头                                        | tag、_math等                                                 |
| 2.只能是英文，数字，_的组合                                  | key_int、my_Name、Chinese1等                                 |
| 3.不能和关键字重名                                           | 关键字：C#定义好了一些单词，赋予了含义如：namespace,class static void..... |
| 4.在同一作用域下，不能和已有的变量名，或者类名，方法名重名（因为C#不知道你调用的是哪一个） | 作用域：你的变量在哪个块中，就对哪个块有作用                 |

### **两大命名法**

| 术语             | 命名法 | 说明                                                         | 举例                     |
| ---------------- | ------ | ------------------------------------------------------------ | ------------------------ |
| **Camel 法**     | 驼峰法 | 首个单词的首字母小写，其余单词的首字母大写，在C#中一般变量名使用此方法命名 | 例：mvValue              |
| **Pascal命名法** | 帕斯卡 | 每个单词的首字母都大写，若是用缩写都用大写，一般类名、函数名用这个方法 | 例如：MyFunction、HP、MP |

---

## 运算符

```
Console.WriteLine("bool = " + sizeof(bool));//输出结果bool=1
//+ 号除了常用的加减乘除的用法还有链接字符串的作用

#region 第一题 //可以在外侧代码起标题，就可以收缩代码
    #endregion //外侧代码，可以起到整理作用
```

### **转义字符**

- 标志着在一个字符序列中出现在它之后的启读几个字符采取一种替代解释。
- 中 '\' 的转移字@在字符串前面表示取消字符串符的作用
- 保留原格式输出

| 转义符 | 字符名  |
| :----: | :-----: |
|   \\'   | 单引号  |
|   \\"   | 双引号  |
|  \\\   | 反斜杠  |
|   \0   | 空字符  |
|   \a   | 感叹号  |
|   \b   |  退格   |
|   \n   |  换行   |
|   \r   |  置换   |
|   \t   | 水平Tab |
|   \v   | 垂直Tab |

### **逻辑运算符**

- 逻辑运算符又叫条件运算符,即运算的结果只有两种, `true`或`false`
- 逻辑运算符的优先级比关系运算符低

| 运算符号 |          解释           |
| :------: | :---------------------: |
|    &     | 二进制与运算;判断运算符 |
|    \|    | 二进制或运算;判断运算符 |
|    &&    |         逻辑与          |
|   \|\|   |         逻辑或          |
|    ！    |         逻辑非          |

> &&：如果表达式1位false，就不会对表达式2进行判断，直接返回false；所以&&比&效率高
>
> |：如果表达式1位true，就不运算表达式2了，直接返回true；所以 || 比 | 效率高

![逻辑运算](E:\上课笔记\资料图\逻辑运算.bmp)

### **三目运算符**

表达式1 ? 表达式2 : 表达式3

- 表达式1、==必须是布尔表达式==
- 当表达式1为true时,执行表达式2,则整个结果为表达式2的结果
- 当表达式1为false时,执行表达式3,整个结果为表达式3的结果

```
int a = 3;
int b = 5;
string s = a > b ? "大于" : "小于" ;
//因为a<b,则表达式A>B为假，输出表达式3"小于"
```

---

## 数据类型的转换

### **隐式类型转换**

- 编译器自己完成的，不需要人工再加以说明，一般精度低完精度高的转是编译器会自行完成，例如字符串转整型，整型转浮点型，一般不会出现数据错误或者丢失

只能用于==数值==或者==父类对象和子类对象==的转换

> 下图就是低精度到高精度转换示意图

```
graph LR
char&short-->int
int-->unsigned无符号
unsigned无符号-->long
long-->double
float-->double
```



### **显示类型转换**

- 将精度高的类型转换成精度低的类型，例如 float 转 int 类型，需要强制转换，一般会有精度丢失

> 本质是高精度转低精度时，位数发生了变化，例如long型原本是8位转成int整型4位时，位数不足，所以编译器无法完成必须人工进行强制转化

​	1.其中一种转换方式，括号强制转换，仅限数值类型的转换，字符串不行

``` 
float a=3.14f;
int b=(int)a;
Console.WriteLine(b);//输出b=3,舍去小数部分，不进行四舍五入
```

​	2.Convert类型转换：Convert工具只能转换C#写好的数据类型即基本数据类型(int、float、string)

```
Consolo.WriteLine("请输入A的年龄");
string age = Console.ReadLine();
int age_int = Convert.ToInt32(age);
//使用ReadLine接收到的是字符串，如输入18
//输出的将会是ASCII值，所以必须进行强制转换成int类型
```

​	3.Parse转换：基本数据类型可以用Parse转换，且字符串的值必须为可转数字

``` 
Consolo.WriteLine("请输入A的年龄");
string age = Console.ReadLine();
int age_int = int.Parse(age);//与Convert类似
int a = int.Parse("32");//将字符串转换成你所需要的类型
```

---

## 条件语句

### **if语句**

- `if (布尔表达式) {if块}`

  + 先判断布尔表达式是否为true,为true则执行if块中语句序列,为false则跳过if块

- `if (布尔表达式) {if块]else {else块}`

  - 先判断布尔表达式是否为true,为true则执行if块中语句序列,为false则跳过if,执行else块中的语句

- `if (布尔表达式) {if块} else if (布尔表达式) {else if块}......`

  - 会一个一个的去判断是否满足条件，如果有一个满足,则执行对应块的语句序列,执行完毕后,就跳过整个if...else if

    如果以上情况都不满足,可以再跟一个else块,代表这是其他情况

### **switch语句**

``` 
switch(表达式)
{
    case 常量标号1:
        语句1;
        break;
    case 常量标号2:
        语句2;
        break;
    default:
        语句n;
        break;
}
```

- 表达式结果必须是int类型，string类型，char类型，枚举类型等常量类型或者bool型
- 常量标号的数据类型要和表达式的数据类型保持一致
- 常量标号具有唯一性，没有顺序
- switc可以用if重写，但是if不一定能用switch重写，当常量过多的时候，使用switch可以调高效率
- 语句块必须以break结尾
- default语句最多只能出现一次，可以不出现

---

## 循环语句

### **while循环**

`while(布尔表达式){循环块}`

- 当布尔表达式为true时,执行循环块的语句序列,执行完毕后,会再次询问布尔表达式直到为false时,才会跳过循环

### **do while循环**

`do{循环块}while(循环条件);`

- 无条件进入循环体;执行一次
- 循环体后判断是否满足条件,当条件满足时重复执行循环体，查到条件不满足时退出

```
//先输入用户信息和密码，如果错误则一直输入
//do while判断用户名密码
string y;
string p;
do
{
    Console.Clear();
    Console.WriteLine("请输入用户名：");
    y = Console.ReadLine();
    Console.WriteLine("请输入密码：");
    p = Console.ReadLine();
    
} while ("admin" != y && "888888" != p);
Console.WriteLine("密码正确");
```

### **for循环**

``` 
for(初始表达式; 循环表达式; 增量表达式)
{
    循环体
}
```

- 先求解初始表达式
- 求解循环表达式,若其值为true ,则执行for语句中指定的内嵌语句,然后执行增量表达式
- 再次判断循环表达式，重复上面一步操作直到循环表达式为false

```
//练习：求1-100之间所有的偶数和
int sum = 0;
for (int i = 2; i <= 100; i += 2)
{
    sum = sum + i;
}
Console.WriteLine("sum:" + sum);
```

```
//找出100-999之间的水仙花数
//例如：153=1*1*1+5*5*5+3*3*3
for(int i = 100; i < 999; i++)
{
    int hum = i / 100;
    int ten = (i % 100) / 10;
    int one = i % 10;
    if (hum * hum * hum + ten * ten * ten + one * one * one == i)
    {
        Console.WriteLine("水仙花数为：" + i);
    }
}
```

```
//打印出一个10行的等腰三角形
for (int i = 0; i < 10; i++)
{
    for (int k = 0; k < 9-i; k++)
    {
        Console.Write(" ");
    }
    for (int j = 0; j < 2*i+1; j++)
    {
        Console.Write("*");
    }
    Console.WriteLine();
}
```
![image](D2F1EBC39A6B4668AB6A06AB4DE13778)

```
//使用for循环控制选择格子移动
int select_x = 0;
int select_y = 0;
while (true)
{
    Console.SetCursorPosition(0, 0);
    for (int Y = 0; Y < 2; Y++)
    {
        for (int X = 0; X < 4; X++)
        {
            if (X == select_x && Y == select_y)
            {
                Console.Write("■");
            }
            else
            {
                Console.Write("□");
            }
        }
        Console.WriteLine();
    }
    char ch = Console.ReadKey(true).KeyChar;
    switch (ch)
    {
        case 'a':
            select_x--;
            if (select_x < 0)
                select_x = 0;
            break;
        case 's':
            select_y++;
            if (select_y > 1)
                select_y = 1;
            break;
        case 'd':
            select_x++;
            if (select_x > 3)
                select_x = 3;
            break;
        case 'w':
            select_y--;
            if (select_y < 0)
                select_y = 0;
            break;
    }
}
```
### **break&countinue关键字**
**break关键字**, 当执行到break时,会跳出break所在的循环块

**countinue关键字**, 当执行到countinue时,会跳过这一次countinue所在的循环,直接开始下一次

---

# 一些常用方法

## Console.Writ&Read

```
Console.WriteLine("你的名字是");
string input = Console.readLine();//ReadLing是从键盘读取一串字符
Console.WriteLine("你好" + input);

Console.WriteLine("你的数学成绩{0},英语成绩{1}",math,english);
//{}表示占位符，用于格式化输出
```

## 改变光标的位置

```
console.SetCursorPosition(Left,top);
//光标的位置，（列，行），在第几列第几行显示光标
```

## 从控制台获取一个输入的字符

```
char key = Console.ReadKey(true).KeyChar;

//练习：可以在控制台通过wasd移动光标
static void Main(string[] args)
{
    //操作 * 在控制台上移动
    //1.需要监听用户的位置 wasd 上下左右
    //2.根据方向，改变*的位置
    //3.消除*改变位置前的图像
    //4.绘制现在这个位置的*

    //数据初始化
    int x = 5, y = 5;
    
    //光标的位置，（10列，5行），在第10列第5行显示光标
    //因为行的宽度是列的两倍，所以10列5行光标的悬停位置就是正方形
    Console.SetCursorPosition(x*2, y);
    Console.Write('*');

    //回合开始
    while (true)
    {
        //监听用户按键
        char key = Console.ReadKey(true).KeyChar;
        //在x,y改变前，消除*
        Console.SetCursorPosition(x * 2, y);
        Console.Write(' ');

        switch (key)
        {
            case 'w':
                y += -1;
                break;
            case 's':
                y += 1;
                break;
            case 'a':
                x += -1;
                break;
            case 'd':
                x += 1;
                break;
            default:
                break;
        }
        //在新的位置上，打印*
        Console.SetCursorPosition(x * 2, y);
        Console.Write('*');
    }
}
```

## 随机数

```
//有一个随机数类 Random，它不是静态类，不能直接调用，需要先进行实例化
Random r = new Random();//实例化了一个骰子

int result = r.Next();//随机取得一个int的取值范围中的数字
int result = r.Next(101);//随机取得一个1~100的数字，即max-1;
int result = r.Next(-100,101);//随机取得一个-100~100的数字,即min~max-1;

//练习
//电脑随机一个数字，让玩家来猜
// 家只有10次机会
//电脑会根据玩家的输入,提示是猜大了,还是猜小了
//如果超过10次,则提示玩家,游戏失败
//使用随机数,循环语句,条件语句完成它
static void Main(string[] args)
{
    int Tag = 1;
    Random s = new Random();//实例化一个随机数变量
    int result = s.Next(101);//限制随机数的限定范围
    while (Tag <= 10)
    {
        string key = Console.ReadLine();
        int key_int = int.Parse(key);
        if(key_int < result)
        {
            Console.WriteLine("猜小了");
        }
        else if (key_int > result)
        {
            Console.WriteLine("猜大了");
        }
        else
        {
            Console.WriteLine("恭喜打开密码锁");
            break;
        }    
        Console.WriteLine("你还剩下{0}次机会",10-Tag++);
    }
    Console.WriteLine("你一共使用了{0}次机会",Tag);
    Console.WriteLine("密码是："+result);
}
```

## 清空控制台

```
Console.Clear;//清空控制台
```

## 设置字体的颜色
```
Console.BankgroundColor = ConsoleColor.Red//设置字体的背景色

Console.ForegroundColor = ConsoleColor.Red//设置字体的颜色
```

---

# 练习

```
//找出100以内的素数
//素数 / 质数：只能被1和这个数字本身整除的数字，1不是质数，最小的质数是2
int num = 2;
bool isPrime = true;
while (num < 100)
{
    isPrime = true;
    int i = 2;
    while (num > i)
    {
        if (0 == num % i)
        {
            //这时候num就不是素数
            isPrime = false;
            break;
        }
        i++;
    }
    if (true == isPrime)
    {
        Console.WriteLine("num:" + num);
    }
    num++;
}
```

```
//限定光标的移动范围
switch (key)
{
    case 'w':
        y += -1;
        if(y < 0)
            y = 0;//这两句话用来判断光标的限定范围
        break;
    case 's':
        y += 1;
        break;
    case 'a':
        x += -1;
        break;
    case 'd':
        x += 1;
        break;
    default:
        break;
}
```
---

# 复杂数据类型

---

## 枚举类型

### **枚举类型的定义**

==关键字enum== 	

```
enum 枚举名
{
	枚举项，
	枚举项，
	枚举项
}
```

- 一种由一组称为枚举数列表的命名常量组成的额独特类型
- 枚举类型默认可以跟int类型转换，枚举类型跟int类型是兼容的
- 默认情况下，第一个枚举数的值位0，后面每个枚举数的值一次递增1
- 枚举数可以用初始值来重写

### **枚举变量的声明**

​	`枚举名 枚举变量名 = 枚举名.枚举项；`

### **Enum的转换**

> 构建一个枚举类型为State，声明一个枚举类型State st

- 枚举转换成int、int转换成枚举

  ​	枚举类型默认跟int类型相互兼容，可以通过强制类型转换；

  ​	`State st = State.OnLine;`

  ​	`st = (State)3;`

  ​	`int x=(int)st;`

- 枚举转换成字符串

  ​	 myEnum.Tostring();

  ​	`st.ToString()；`

- 字符串转换成而枚举

  ​	（要转换的枚举类型）Enum.Parse(typeOf(要转换的枚举类型),"要转换的字符串"）；

  ​	`(State)Enum.Parse(typeOf(State),"OnLine")`

枚举是一种需要我们自定义的**数据类型**，需要我们先定义出这个类型，才能去使用这个类型的数据。

枚举类型是描述事物的有限状态，枚举项可以用中文但不建议使用，每个枚举项用逗号隔开，最后一个枚举项可以不使用逗号

枚举项搭配 switch 语句使用更佳，在 switch 语句的括号里输入定义好的枚举类型，双击空白处会自动识别枚举类型中的几个枚举项

```
//定义一个枚举类型
enmu QQ
{
	Qme,
	OnLine,
	OffLine,
	Leave,
	Busy
}
//枚举的本质就是int，可以和int发生强制（显示）转换
static void Main(string[] args)
{
	//枚举的声明
    QQ myQ = QQ.Busy;
}
```

---

## 结构体
### **结构体的定义与声明**

==关键字 struck== 	

```
struct 结构名
{
	成员;
	成员;
}
```

- 是一种自定义复合数据类型，可以帮我们一次性声明多个不同类型的变量，当然多个相同数据类型的变量也可以
- 声明变量的时候不能赋初始值
- 当结构的成员为 public 时，我们可以通过结构的 ==对象.成员名== 来访问
- 结构体是值类型，存在内存的栈中
- 对象是指实体的，实实在在的存在；结构体值是一种抽象的描述

```
struct Person//定义一个身份证的结构体
{
    public int id;
    public string name;
    public string address;
    public short age;
}
class Program
{
    static void Main(string[] args)
    {
        //结构体的声明和使用
        Person p1 = new Person();
        p1.id = 100010;
        ....
    }
}
```

==结构体的成员可以是除了自身这个类型以外的所有数据类型，即可以使用其他结构体作为自己的成员==

---

## 访问修饰符
### **public**

public  指定一个变量、方法和类的访问作用域为公共访问，即外部类也可以通过指定对象来调用

​		公有的，==所有人都可以访问==

### **private**

private 指定一个变量、方法和类的访问作用域为私有访问，即只有内部成员才可以访问

​		私有的（默认都为private）,==只有自己才能使用==

### **protect**
protect 指定一个变量、方法和类的访问作用域为继承访问，即只有内部与子类成员才可以访问

​		继承的，==只有自己和自己的子类才可以使用==

---

# 数组

---

## 一维数组

​	**数据结构：数据与数据之间的关系称为数据结构**

​	**数组：一种相同类型的集合**

​	**遍历：把这个结构中所有的数据都访问一遍**

```
//数组的几种声明和赋值方式
//1. 数据类型[] 数组名 = new 数据类型[初始长度]
int[] nums = new int[3];
nums[0] = 1;
nums[1] = 2;
nums[2] = 3;

//2. 数据类型[] 数组名 = new 数据类型[初始长度]{元素}
int[] nums_2 = new int[3] { 1, 2, 3 };

//访问数组：数组名[下标号]
num_s[0];
//获取数组长度：数组名.Length
num_s.Length;

//遍历数组
//for(int i=0;i<数组名.Length;i++){Console.Write(数组名[i])}
for(int i=0;i<nums_2.Length;i++)
{
    Console.Write(nums_2[i]);
}
```

### **冒泡排序** ：第一轮循环把最大的值放到最后一个数组位置

```
int[] arr = new int[10] { 60, 35, 99, 32, 88, 4, 3, 2, 1, 0 };
//打印出初始的数组
for (int i = 0; i < arr.Length; i++)
{
    Console.WriteLine("arr[{0}]=:{1}", i, arr[i]);
}
Console.WriteLine("------------------------------------");

//进行冒泡排序
for (int i = 0; i < arr.Length; i++)
{
    for (int j = 0; j < arr.Length - 1 - i; j++)
    {
        int temp;
        if (arr[j] > arr[j + 1])
        {
            temp = arr[j];
            arr[j] = arr[j + 1];
            arr[j + 1] = temp;
        }
    }
}

//打印出排序之后的数组
for (int i = 0; i < arr.Length; i++)
{
    Console.WriteLine("arr[{0}]=:{1}", i, arr[i]);
}
```

### **选择排序** ：选出一个最小值放在数组的最后

```
int[] arr = new int[10] { 60, 35, 99, 32, 88, 4, 3, 2, 1, 0 };
//打印出初始的数组
for (int i = 0; i < arr.Length; i++)
{
    Console.WriteLine("arr[{0}]=:{1}", i, arr[i]);
}
Console.WriteLine("------------------------------------");

//选择排序
for (int i = 0; i < arr.Length; i++)//{ 60, 35, 99, 32, 88, 4, 3, 2, 1, 0 };
{
    int min = arr[i];
    int minIndex = i;
    for (int j = i+1; j < arr.Length; j++)
    {
        if (arr[j] < min)
        {
            min = arr[j];
            minIndex = j;
        }
    }
    if (arr[i] != min)
    {
        arr[minIndex] = a[i];
        a[i] = min;
    }
}

//打印出排序之后的数组
for (int i = 0; i < arr.Length; i++)
{
    Console.WriteLine("arr[{0}]=:{1}", i, arr[i]);
}
```

### **Splint()方法** ：使用分割符输出字符串数组

```
string[] str = Console.ReadLine().Split(';');//输入字符，以 ；分割，回车结束输入
string[] arr = "10|108|99|5".Split('|');//以 | 为界分割字符串

for (int i = 0; i < arr.Length; i++)
{
    Console.WriteLine(arr[i]);
}
//输出 ：10 188 99 5
```

---

## 二维数组

二维数组的声明和赋值方式

- 数据类型[ , ] 数组名；

  `int[,] numbers;`

- 数据类型[ , ] 数组名 = 仅指定长度的初始值

  `int[,] numbers = new int[2,3];`

- 数据类型[ , ] 数组名 = 初始值

  `int[,] numbers = new int[,]{{1,2,3},{4,5,6}};`

​	当二维数组没有赋值的时候，但是声明的时候使用了 ==new== 关键字，这个时候已经开辟了内存空间，所以这个时侯二维数组是有初始值的

​	二维数组的长度访问是使用 **数组名.GetLength(x)**，x代表你获取的第几个维度的长度，0代表第一个维度（行），1代表第二个维度（列）

```
//将1到25赋值给一个二维数组
int x = 1;
int[,] arr = new int[5, 5];
for (int i = 0; i < arr.GetLength(0); i++)
{
    for (int j = 0; j < arr.GetLength(1); j++)
    {
        arr[i, j] = x;
        x++;
    }
}
//打印出这个数组
for (int i = 0; i < arr.GetLength(0); i++)
{
    for (int j = 0; j < arr.GetLength(1); j++)
    {
        Console.WriteLine("arr[{0},{1}]={2}", i, j, arr[i, j]);
    }
}
```

---

## 交错数组

- 多维数组只能构造一个矩形的数据结构，而使用交错数组可以设计出不规则的结构

```
//交错数组的声明
int[][] arr = new int[3][]
{
    new int[3] {1,2,3},
    new int[2] {1,2},
    new int[4] {5,6,7,8}
};

//交错数组的foreach遍历
foreach (int[] item in arr)
{
    foreach (int i in item)
    {
        Console.Write(i);
    }
    Console.WriteLine();
}

//for循环遍历
for (int i = 0; i < arr.GetLength(0); i++)
{
    for (int j = 0; j < arr[i].Length; j++)
    {
        Console.WriteLine("x:" + arr[i][j]);
    }
}
```
### **foreach**
==foreach好用，方便，但是在开发之中禁止使用这个方法，因为这个foreach迭代器遍历会产生内存垃圾，不好清理，所以一般要改回for循环，且foreach遍历中，无法修改数据项的值==

```
foreach (var item in collection)
{
	//ver表示数据类型
    //item表示要从collecttion中取出数据放在item中
    //cillection表示要取数据的数组
}
```

---

# 							练习

```
//以下全部基于4*4数组
int[,] arr = new int[4, 4];

//将二维数组（4行4列）的右上半部分置零
for (int i = 0; i < arr.GetLength(0); i++)
{
    for (int j = 0; j < arr.GetLength(1); j++)
    {
        if (j > i)
        {
            arr[i, j] = 0;
        }
    }
}

//求二维数组对角线的和
for (int i = 0; i < arr.GetLength(0); i++)
{
    for (int j = 0; j < arr.GetLength(1); j++)
    {
        if (i == j || 3 == i + j)
        {
            sum += arr[i, j];
        }
    }
}

//找出二维数组最大值且对应的行列号
int max = 0;
int maxIndexI = 0;
int maxIndexJ = 0;
for (int i = 0; i < arr.GetLength(0); i++)
{

    for (int j = 0; j < arr.GetLength(1); j++)
    {
        if (arr[i, j] > max && arr[i, j] != max)
        {
            max = arr[i, j];
            maxIndexI = i;
            maxIndexJ = j;
        }
    }
}

//输入9个数字，用 ；号分割，按方阵输出
string[] str = Console.ReadLine().Split(';');
int index = 0;
for (int i = 0; i < arr.GetLength(0); i++)
{

    for (int j = 0; j < arr.GetLength(1); j++)
    {
        arr[i, j] = int.Parse(str[index]);
        index++;
    }
}
//第二种方法：输入9个数字，用 ；号分割，按方阵输出
string[] str = Console.ReadLine().Split(';');
for (int i = 0; i < str.Length; i++)
{
    arr[i / arr.GetLength(1), i % arr.GetLength(1)] = int.Parse(str[i]);
}
```
---
# 函数

---

## 函数的定义

```
static 返回类型 函数名(参数)
{
    代码块(函数体);
    代码块(函数体);
    return(如果返回类型为void可省略)
}
```

- 函数也叫做方法，是将一堆代码封装进行重用的一种机制
- 函数体中是语句的序列
- 函数只能声明在类、结构体、接口中
- 我们可以通过形参给函数体传入相应类型值
- 可能会有返回值，函数通过返回类型，将对应的值返回给我们
- 函数有效的节约了代码量，提升了代码的可读性
- 函数便于管理，我们可以通过对象直接调用函数

---

## 函数关键字 ：out    ref     params

当一个函数需要返回多个类型的数据时，就不能单纯的使用 return 来返回

### **out关键字**

- 使用变量的地址
- 可以使用数组或结构体返回多个数据
- ==被out修饰的参数在函数内部是需要被复制的参数（即在方法内部需要对out参数再赋值，否则无法使用，而ref则不需要）==
- 在函数调用的时候，out参数前一定要加out

### **ref关键字**

- 使用变量的地址
- 直接修改实参的值
- ==变量如果使用ref进行传参，必须先赋值（即在调用方法时，必须传进去的是已经赋值的变量，而out则不需要）==
- 在函数调用的时候，ref参数前一定要加ref

> ref和out类似C语言的指针

```
//返回给用户一个登录结果，并且还要单独的返回给用户一个登录信息

//用户信息结构体
struct UserInfo
{
    public string name;
    public int level;
}
class Program
{
	//传入用户名和密码，返回一个登录结果和登录信息
    static bool Login(string name,string password,out UserInfo info)
    {
        info = new UserInfo();
        if("admin" == name && "888888" == password)
        {
            info.level = 10;
            info.name = "张三";
            return true;
        }
        else
        {
            return false;
        }
    }
    static void Main(string[] args)
    {

        UserInfo info = new UserInfo();
        bool loginInfo = Login("admin", "888888", out info);
        if (loginInfo)
        {
            Console.WriteLine("欢迎{0}级的{1}", info.level, info.name);
        }
        else
        {
            Console.WriteLine("登录失败");
        }
        Console.ReadKey(true);
    }
    
}
```

### **params关键字：可变参数传参**

使用情景：目前不知道需要传进来几个参数

- 参数个数可变
- 将实参列表中跟可变参数数组类型一直的元素都当做数组的元素去处理
- 只能作为参数列表中，==最后一个参数==

> 一般在工程中使用params是为了在不确定该方法有几个参数的时候使用，这样后续就会少改代码

---

## 函数重载：在不同条件下实现相同的功能

当一个类或者结构体中存在两个或两个以上同名的函数，当这两个函数满足以下关系时，他们就构成了重载关系：

- 函数名一样
- 函数参数个数不一样；或者参数个数一样参数数据类型不一样；或者参数个数一样，数据类型一样但是前后顺序不一样

> 函数重载大大简化了类的调用者的工作，是我们类具有自动化完成的功能

---

## 函数递归：方法自己调用自己，解决重复逻辑行为的问题

- 找出递归问题的可满足条件（边界，变化条件）
- 把这个条件能交给自己（把变化条件交给自己）
- 这个条件无尽向可满足边界靠近

```
//递归调用求阶乘
static int Factorial(int n)
{
    if (n == 1)
    {
        return 1;
    }
    return n * Factorial(n - 1);
}
```

```
//求阶乘的和
static int SumFactorial(int n)
{
    if (n == 1)
    {
        return 1;
    }
    return Factorial(n) + SumFactorial(n - 1);
}
```

```
//一根竹竿长n米，每天砍一半，求第m天的长度是多少
static float Bamboo(float b,int t)
{
    b = b / 2;
    if (t == 1)
    {
        return b;
    }
    return Bamboo(b, t - 1);
}
```
```
//递归算出杨辉三角
namespace 杨辉三角
{
    class Program
    {
        static int Dispose(int m,int n )
        {
            m = 3;
            n = 5;
            if (n > 2 * m)
            {
                Console.WriteLine("输入的数据有误");
                return -1;
            }
            if (n > m)
            {
                n = n - m;
            }
            return ShowTriangleNum( m, n );
        }
        static int ShowTriangleNum(int m,int n )
        {
            if(m==0&& m == 0)
            {
                return 1;
            }
            if (m == 0 && n != 0)
            {
                return 0;
            }
            return ShowTriangleNum( m - 1, n - 1 ) + 
            ShowTriangleNum( m - 1, n ) + ShowTriangleNum( m - 1, n + 1 );
        }
        static void Main( string[] args )
        {
        
        }
    }
}
```
---

## 方法int.TryParse()

- 尝试将用户输入的字符串转换成int类型，如果转换成功返回true，并把字符串转换成int类型的数组赋值给age

```
int age;
if(int.TryParse(Console.ReadLine(),out age))
{
    Console.WriteLine("年龄是：" + age);
}
else
{
    Console.WriteLine("输入错误，请重新输入");
}
```
---

# 面向对象

---

## 什么是面向对象

**是一种软件开发思想，是一种程序设计思路，是一种程序结构的表述**

## 面向对象特点

- 封装
- 继承
- 多态

---

# 封装

`类{方法(函数)}`

## 类

### **什么是类**

- 一种复杂的数据类型

- 需要我们先定义出这个类型

- 我们把具有相同属性和相同方法（行为）[^1]的对象进行进一步的封装，抽象出类这个概念

  > 就是模板，我们通过这个模板，可以构建相应的对象，确定对象将会拥有的特征（属性）和行为（方法）

[^1]:方法（函数）：这个对象所拥有的行为

```
graph LR
类-->A[属性]
类-->行为
A[属性]-->字段
A[属性]-->B[属性]
行为-->方法
```

### **类的声明**

```
访问修饰符 class 类名
{
    成员;(字段、属性、方法)
}
```

- 成员可以是==任意==数据类型
- 成员也可以是方法
- 如果为非static的成员，可以通过对象名去访问

==我们尽可能将类定义在另一个类的外面==

### **将类实例化为一个对象**

- 实例化：new一个实例化对象

- 初始化：给实例化对象成员赋初始值

  > 为了保证对象成员的合理性，一般情况下我们实例化对象后，会对其初始化

1. 对象是类的实例，他具有类的属性和方法（非静态）
2. 因为静态的方法，通过类名调用

#### new关键字

 将一个类实例化为一个对象，申请对应的内存空间（堆中），并调用该类的构造函数（将对象初始化）

`类名 对象名 = new 类名();`

==对象作为一个真实存在的物体，就会具有生命周期==

| 出生（实例化一个对象时） | 活着（这个对象在被使用） | 死亡（当对象没人使用的时候） |
| :----------------------: | :----------------------: | :--------------------------: |
|         构造函数         |        对象被使用        |           析构函数           |

### **构造函数**

- 与类名相同，可以帮我们在实例化对象的时候完成初始化,是类进行构造一个对象的函数，是一个对象的起始，在堆中开辟区域，如果我们没有自己写，系统会自动添加
- 构造函数的声明：没有返回值，不能写void（其实固定好了，只返回你对象的堆空间的地址）
- 必须和类名一致
- 当我们类中没有写构造函数时，系统会默认添加一个无参构造函数；如果类中有有参构造，那么系统不会给添加一个默认无参构造函数  
- 构造函数一旦重新指定，则默认构造函数失效  
- 当你在通过new关键字创造对象时，则会调用构造函数或者构造函数的重载方法，对对象初始化  (即构造函数必须通过new关键字调用)[^2]

[^2]:本质是，你在通过类这个模板（协议）在创造对象（在内存空间(堆)中去开辟一片属于你这个对象的空间）

- 可以通过 ：this指向其他构造函数

#### this关键字

1.在所在类中，暂时代替这个类的实例化对象
2.可以在这个类的构造函数的参数列表后面用 `:this` 去指代本类的其他构造函数

```
class Student
{
    public string name;
    public string sex;
    public int age;
    private int id
    //构造函数，这样声明就可以在实例化的同时初始化
    public Student(string name,string sex,int age)
    {
        //this暂时代替该类的实例化对象
        this.name = name;
        this.sex = sex;
        this.age = age;
    }
    //使用this去指向其他构造函数
    public Student(string name,string sex,int age,int id):this(name,sex,age)
    {
        this.id = id;
    }
}
```

### **对象在被引用**

- 有变量直接或间接指向对象空间，则对象空间在被引用（活着）
- 若是没有任何东西指向对象空间，那么对象就会结束周期调用析构函数

### **析构函数**[^3]

[^3]:死亡，生命周期结束

- 波浪号加类名，这个对象销毁所执行的方法
- 没有访问修饰符，没有返回值，函数名必须是`~类名`
- 我们一般不写，系统会自动添加，当系统调用垃圾回收时，系统会调用析构函数回收内存区域
- 不可重载

```
class Student
{
    //构造函数
    public Student()
    {
        
    }
    //析构函数
    ~Student()
    {
        
    }
}
```

#### 垃圾回收

- 垃圾回收时CLR内存管理机制，他将会帮助我们有效的释放内存，在一定程度上保证了对象的安全性，及使用的便捷性

---

## 类字段的保护（属性）

- 类的成员不应当时所有人都可以访问的，因为这样做很不安全

我们应该使用方法，封装我们的数据成员（字段），以达到保护的作用

封装字段有两个方法

1.访问方法`get{}`：是得到的意思，在get方法中返回对应的值给调用处

2.写入方法`set{}`：我们可以在set方法中，对只读（私有）变量、属性进行赋值

```
//属性的使用
访问修饰符 数据类型 属性名
{
	get
    {
    
    }
    set
    {
    
    }
}
```

1.属性名应当和你要保护的字段的名字保持一致，只不过在大小写上做区分，一般属性名首字母大写

2.两个块可以只有一个

3.get块必须要有return

4.set块可以通过value关键字获取到外部传递的参数

```
class Student
{
    private int age;
    //属性（保护字段age）
    public int Age
    {
        get
        {
            return age;
        }
        set
        {
            if(value > 80)
                Console.WriteLine("你是不是输错了");
            else
                age = value;
        }
    }
}

//属性的使用
Console.WriteLine(stu.Age);//默认调用get块
stu.Age = 19;//默认调用set块
```



---

## 全局变量和局部变量

### **全局变量**

- 在类中直接声明的变量叫全局变量（即类中所有方法的外面），且无论声明在类的哪个位置，即使是最后一行其他方法都可以使用到

### **局部变量**

- 在一个代码块中声明的变量就是局部变量，他的周期只在这个代码块中

```
{
    string s = "张三";
    Console.WriteLine(s);
}
```

> 善用这个大括号代码块，可以提高效率（减少变量取名的困扰）

---

## 静态类

### **静态**

- 静态类型的变量是类==最先加载==到内存区域的 ==（静态方法不可以调用非静态方法，但是非静态方法可以调用静态方法）==
- 静态变量或者方法，不是对象所单独拥有的，是所有对象所共有的，通过 ==类名.属性或方法== 直接访问(单例：即只有一个实例化对象，共用) 
- 静态区域的内容，是整个类中==最后销毁==的，也就是说不能乱声明静态的内容

### **静态类**

- 在定义类的时候，在类名前加上static
- 静态类，只能包含静态的成员（该类下，所有的成员都得加上static）  
- 静态类是全局唯一,全局共享的，不允许构造（不能指定构造函数），不能实例化，当你需要一个工具类的时候，可以考虑静态的  
- 静态方法（在方法名前加上static），只能调用外部的静态成员（不能调用外部的非静态成员）

静态的成员调用

`类名.成员名;`

非静态的成员调用

`对象名.成员名;`

**静态的数据（字段）是根据着程序从出生到死亡的，会一直占用资源，所以静态的数据（字段）越少越好**

但有时候，我们为了方便开发 ，需要全局唯一的共享数据 如（怪物管理者），所以为了即拥有静态的优点，又避免了静态的缺点，开发者们就研究出一种设计模式——单例设计模式

### **单例(一种设计模式)**

通过唯一的一个静态实例对象，可以索引到所有我需要的非静态数据

什么类需要单例？
- 这个类不能有多个对象，比如游戏管理类
- 当我们想使用静态的特性，制作一个全局唯一的工具类时，会发现如果存放过多的静态数据，则会很危险（内存堆满，因为静态的东西是和程序同生共死不会自己提前释放掉）
- 使用单例模式只在静态区中存放一个静态对象的地址，然后通过这个对象的地址去堆中存放或使用数据，可以避免静态数据过多的问题，也保持了静态的方便的特性

单例的特点：

- 整个项目中这个类只有一个对象
- 单例类的写法：把构造函数私有化  

如何使用单例：
- 创造该类的静态对象
- 保证该对象有且只有一个（用属性封装，封闭构造函数）
- 直接通过`类名.对象名`，去获取这个静态对象下的所有公开的数据

```
class GameManage//像这种管理类一般只有一个对象，所以用单例实现
{
    private GameManage()//构造函数私有化
    {
        
    }
    private static GameManage instance;//全局唯一对象
    public static GameManage Instance//属性
    {
        get
        {
            if(instance == null)
                instance = new GameManage();//如果这个类没有实例化对象那就new一个对象
            return instance;
        }
    }
    public static int playerScore;
    public int playerVIPLevel;//当要访问这几个字段的时候如下
}

//方法访问
Console.WriteLine("VIP等级" + GameManage.Instance.playerVIPLevel);
```



---

## 总结类和结构体区别

### **类型的区别**

- 结构体对象是值类型[^4]，类对象是引用类型[^5]
[^4]:值类型在存在栈中，默认值为 0(每种编程语言不太一样)
[^5]:引用类型存在堆中，默认值是 null
- 当方法传值是引用类型的时候，不必使用 ==ref、out== 关键字

### **结构体的构造函数**

- 默认构造函数不允许重新制定，可以重载带参数的构造函数
- 一旦重载了带参的，那么在你函数制定完毕前，必须给所有的字段赋值
- 重载了有参的构造函数后，原来==无参的构造函数还存在==，与类的区别所在

---

# 继承

## 什么是继承

- 继承是面向对象程序设计中最重要的概念之一。继承允许我们根据一个类来定义另一个类，这使得创建和维护应用程序变得更容易。同时也有利于重用代码和节省开发时间。
- 当创建一个类时，程序员不需要完全重新编写新的数据成员和成员函数，只需要设计一个新的类，继承了已有的类的成员即可。这个已有的类被称为 ==基类/父类/超类==，这个新的类被称为 ==派生类/子类==。
- ==我们一般通过分析派生类的共有属性和行为抽象一个基类==

---

## 如何继承

```
访问修饰符 派生类名：基类名
{

}
```

- 一个类只能直接继承一个基类（即只有一个父类）
- 派生类保留了父类的所有属性和方法（包括私有的）
- 如果父类的访问域为不允许（私有），派生类无法访问其属性和方法
- 派生类的构造函数需要指向一个父类的构造，如果不指定将会指向父类的默认构造函数
- ==所有==类都直接或间接继承一个叫Object的基类（没有写继承的都是默认继承超类Object类）

```
class Person
    {
        public string name;
        public int age;
        public string sex;
        
        public Person(string name,int age)
        {
            this.name = name;
            this.age = age;
        }
        
        public Person(string name,int age,string sex) : this(name, age)
        {
            this.sex = sex;
        }
    }
class Student : Person
    {
        //base关键字，与this对应，base指向的是父类的构造函数
        public Student(string name,int age,string sex) : base(name, age, sex)
        {
    
        }
    }
```

### **关键字与修饰符**

#### base关键字

**与this对应，base指向的是父类的构造函数或者暂时指向父类的实例，指向的这个构造一定要存在**

- 子类拥有父类的所有成员
- 子类在构造的时候，是默认调用父类的默认构造函数
- 当父类的默认构造函数失效时，应当使用base关键字

#### new关键字

- 如果在子类中声明的一个与父类中同名和同签名的函数，将默认启用`new`关键字，将会为这个新的成员申请一个新的空间，而父类中的成员将会被隐藏
- 也可以自己使用`new`关键字声明一个与父类中同名和同签名的函数

#### protected和internal访问修饰符

`protected`：保护类型，本类和继承类可以访问

`internal`：当前程序集才能访问，即即使通过using引用了一个程序集（一个namespace[^6]），也不能访问用internal修饰的类

[^6]:命名空间

---

## 里氏转换

### **什么是里氏转换**

- 子类可以当做父类使用（甚至可以转换成父类去使用）(因为它承载了父类的所有成员，所以可以当父类使用)
- 如果父类中装的是子类，我们可以将他强制转换成子类，并且去使用

```
class Person
class Programmer：Person

//子类当父类使用（用子类声明的父类对象）
Person p = new Programmer();
//p虽然是Person类型的对象，但是装载的是Programmer的对象
//且p虽然内核是Progreammer，但是p无法使用Praogrammer类中的方法

//强转为子类，可以使用子类中的方法，最好不要用这种强转方法
//但是前提是要是用子类声明的父类对象
((Programmer)p).子类方法();
```

### **里氏转换关键字is和as**

- `is`关键字：表示类型转换，如果能够转换成功，则返回`true`，否则返回一个`false`；即判断当前对象能不能进行转换
- `as`关键字：表示类型转换，如果额能够转换则返回对于的对象，否则返回`null`；即尝试堆当前对象进行转换，转换成功则返回对应的对象

```
//类型转换上述p这个对象
if(p is Programmer)//判断当前对象能不能进行转换，如果能则返回true
{
    (p as Programmer).子类Programmer中方法();//尝试对当前对象进行转换，如果转换成功则返回对应的对象，否则返回null
}
else
{
    Console.WriteLine("转换不成功");   
}
```

---

## 常用方法

### **ConsoleKeyInfo**

```
//读取到上下左右键
ConsoleKeyInfo info = Console.ReadKey();
switch (info.Key)
{
    case ConsoleKey.UpArrow:
        
        break;
    case ConsoleKey.DownArrow:
       
        break;
    case ConsoleKey.LeftArrow:
        
        break;
    case ConsoleKey.RightArrow:
        
        break;
    default:
        break;
}
```

---

# 多态

## 什么是多态

- 同一操作作用与不同的对象，可以有不同的解释，产生不同的执行结果。在运行时可以通过调用同一个方法，来实现派生类中的不同表现
- 多态的实现形式大致有四种
  - 虚方法
  - 抽象类与抽象方法
  - 接口

---

## 虚方法

- 在实例方法（非`static`）前，添加`vitrual`关键字，则可以称该方法为虚方法
- 继承了该类的子类，则可以通过`override`关键字去重写父类的虚方法（签名相同且同名）
- 当我们程序在调用虚方法时，程序会根据我们对象中实际装载的对象以及最后重写的虚方法（就是如果装载的子类对象没有重写虚方法，就会往上找父类有没有重写虚方法，最后执行的就是父类中那个重写的方法），去决定执行效果
- 注意：虚方法一定要`public`（即不能私有化），如果子类中没有用`override`但是重名，就是默认添加了`new`关键字：为重名方法重写开一片空间

```
class Person
{
    public string name;
    public string country;
    public Person(string name,string country)
    {
        this.name = name;
        this.country = country;
    }
    public virtual void Speak()//Person类中的虚方法
    {
        Console.WriteLine("人类说话方法");
    }
}
class Chinese : Person//继承Person类
{
    public Chinese(string name, string country):base(name,country)
    {
        
    }
    public override void Speak()//重写父类中的虚方法
    {
        Console.WriteLine("中文说话");
    }
}
class Amerece : Person
{
    public Amerece(string name, string country):base(name,country)
    {

    }
    public override void Speak()//重写父类中的虚方法
    {
        Console.WriteLine("英文说话");
    }
}
class Program
{
    static void Main(string[] args)
    {
        Person p = new Chinese("张三", "中国");//实际装载的时Chinese类的对象
        p.Speak();//-->实际调用的是Chinese类中的 Speak方法，输出的是 “中文说话”

        Console.ReadKey(true);
    }
}
```

### **几个关键字**

#### virtual

- 在基类中我们可以通过`virtual`关键字声明一个虚方法

#### override

- 在派生类中我们可以通过`overrede`关键字重写签名相同的虚方法

#### sealed关键字

- 封闭这个类，使用sealed关键字使类无法继承

`sealed class Boss`-->此时Boss类无法被继承

- 在派生类中我们可以通过sealed封闭一个虚方法的重写，与封闭类类似

`private sealed void Speak() `-->此时Speak方法无法被重写

### **多态与new关键字覆盖的区别**

- new是实现不了多态，就当用父类声明变量装载子类对象的时候是无法调用出子类中的方法

```
Person p = new Chinese("张三", "中国");
p.Speak();//-->实际调用的还是父类中的 Speak方法
```

---

## 抽象类与抽象方法

### **抽象类**

- 用`abstract`修饰过的类是抽象类
- 抽象类不允许实例化
- 抽象类具有构造函数，提供给子类使用
- 抽象类是可以包含非抽象成员
- 继承了抽象类的子类==必须==实现父类中的所有抽象(`abstract`)成员，通过`override`关键字 

### **抽象方法**

- 抽象方法是没有实现代码的虚方法，且必须不能实现（即不能有代码块）
- 抽象方法使用`abstract`修饰符进行声明，且只能在同样声明了`abstract`的类中使用
- 继承了抽象类的子类==必须==实现父类中的所有抽象方法

```
abstract class Person//定义一个抽象类
{
    public abstract void Show();//定义一个抽象方法，且不能实现
}
class Student : Person//继承抽象类Person
{
    public override void Show()//子类必须重写父类中的抽象方法
    {

    }
}
class Program
{
    static void Main(string[] args)
    {
        Person p = new Student();//抽象类是无法被实例化的，只能声明子类对象
        p.Show();//此时调用的就是子类Student中的show方法
    }
}
```

### **虚方法与抽象类抽象方法的选择**

- 如果父类中的方法有默认的实现，并且父类需要被实例化，这时可以考虑将父类定义成一个普通类，用虚方法来实现多态
- 如果父类中的方法没有默认实现，父类也不需要被实例化，则可以将该类定义为抽象类

### **ToString()方法**

- 基类`Object`中的虚方法，可以选择重写或者不重写 
- 在C#中常用于各种控制台输出，本质上它将返回一个字符串
- 我们经常会在自定义类型中重写它，以便输出我们的需要的数据

`string str = 1234.ToString();//输出的就是字符串形式的1234`

```C#
class Monster
{
    public override string ToString()//重写ToString方法
    {
        
    }
}
```

### **String.Format()方法**

- 将指定字符串中的一个或多个格式项替换为指定对象的字符串表示形式。

```C#
class Monster
{
    private string name;
    public Monster(string name)
    {
        this.name = name;
    }
    public override string ToString()
    {
        return String.Format("你好{0}", name);//Format方法，其实就是跟Console格式化输出一样
    }
}
class Program
{
    static void Main(string[] args)
    {
        Monster mon = new Monster("Goblin");
        Console.WriteLine(mon);//Console.WriteLine打印mon这个对象的时候其实就是调用了类对象中的ToString方法


        Console.ReadLine();
    }
}
```

---

## 重载运算符

### **如何重载运算符**

```
访问修饰符 static 返回类型 operator 运算符(参数列表)
{
    运算后返回的结果
}
```

### **可以重载的一些运算符**

| 运算符                                                       | 可重载性                                                     |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| `+` `-` `!` `~` `++` `--` `true` `false`                     | 这些一元运算符可以进行重载                                   |
| `+` `-` `*` `/` `%` `&` `|` `>>` `<<`                        | 这些二元运算符可以进行重载                                   |
| `==` `!=` `>` `<` `>=` `<=`                                  | 比较运算符可以进行重载                                       |
| `&&` `||`                                                    | 条件逻辑运算符无法进行重载，但是它们使用`&`和`|`（可以重载）来计算 |
| `[]`                                                         | 数组索引运算符无法进行重载，但是可以定义索引器               |
| `(T)x`                                                       | 强制转换运算符无法进行重载，但是可以定义新转换运算符         |
| `+=` `-=` `*=` `/=` `%=` `&=` `|=` `<<=` `>>=`               | 赋值运算符无法进行重载，但是`+=`使用`+`（可以重载）来计算    |
| `=` `.` `?:` `??` `->` `=>` `f(x)` `as` <br>`cheked` `unchecked` `default` `delegate` <br>`is` `new` `sizeof` `typeof` | 这些运算符无法进行重载                                       |

```
struct Vector2
{
    public int x, y;
    //重载加法运算
    public static Vector2 operator +(Vector2 v1,Vector2 v2)
    {
        return new Vector2(v1.x + v2.x, v1.y + v2.y);
    }
    //重载 == 运算符
    public static bool operator ==(Vector2 v1, Vector2 v2)
    {
        if (v1.x == v2.x && v1.y == v2.y)
        {
            return true;
        }
        else
            return false;
    }
    //重载 != 运算符
    public static bool operator !=(Vector2 v1, Vector2 v2)
    {
        if(!(v1.x == v2.x && v1.y == v2.y)) 
        {
            return true;
        }
        else
            return false;
    }
}
```

---

## 接口

### **什么是接口**

- 接口定义了可由类和结构实现的协定
- 接口可以包含方法、属性、事件、索引器
- 接口不提供所定义的成员的实现代码，仅指定必须由实现接口的==类或结构体==提供的成员，因此接口在声明是不能具体实现，它只能通过继承了它的类的成员实现
- 接口是==多重继承==的，一个接口可以继承另一个接口或者多个接口，一个类可以继承一个或者多个接口

### **接口的优势**

- 多继承，可以让类更灵活，并在减少代码重复的情况下降低代码关联性（耦合度）
- 多态：程序可扩展性，节约成本，提高效率

### **接口的声明**

#### 关键字interface

```
访问修饰符 interface 接口名
{
    属性
    方法
}
```

- 接口的声明与类基本一致
- 接口名一般是首字母是大写的`I`
- 接口的方法和属性均不允许实现
- 接口不能有构造函数，不能有字段
- 不能重载运算符
- 接口中成员必须为public(不特殊声明默认为public)

### **接口的使用**

```
访问修饰符 类名：基类，接口名，接口名
{
    属性;
    方法;
    //实现接口
    public 返回值 接口方法()
    {
        
    }
    //显示实现接口
    返回值 接口名.接口方法()
    {
        
    }
}
```
-若类中已有存在同名的方法则要使用显示实现接口
- 继承关系中如果有类必须是第一个
- 继承了接口的类必须实现该接口的所有成员接口
- 接口可以继承接口

#### 接口具体使用方法

```
public interface IFly//接口
{
    void Fly();
}
class Animal//动物基类
{

}
class Swan : Animal, IFly//天鹅继承基类和接口
{
    public void Fly()
    {
        Console.WriteLine("天鹅飞");
    }
}
class Helicopter :IFly//直升机继承接口
{
    public void Fly()
    {
        Console.WriteLine("直升机飞");
    }
}

//使用
class Program
{
    static void Main(string[] args)
    {
        IFly[] fly = new IFly[4];//使用接口定义变量
        
        //实现多态
        fly[0] = new Swan();
        fly[1] = new Helicopter();
        fly[2] = new Swan();
        fly[3] = new Helicopter();
        
        //此时可以直接通过接口访问方法
        for (int i = 0; i < 3; i++)
        {
            fly[i].Fly();//输出：天鹅飞 直升机飞 天鹅飞
        }
        Console.ReadLine();
    }
}
```

#### 显示实现接口

**当显示实现接口方法名称与自身类中方法==重名==的时候，调用时要看是用什么类型声明的对象，如果是接口声明则优先调用接口的方法，如果是自身声明的就优先调用自己的方法**

```
public interface IFly
{
    void Fly();
}
class Helicopter :IFly
{
    void IFly.Fly()//显示实现接口
    {
        Console.WriteLine( "垂直飞" );
    }
    public void Fly()
    {
        Console.WriteLine("直升机飞");
    }
}
```

```
{
    IFly fly = new Helicopter();//是用接口进行声明的对象
    
    (fly.Fly as Helicopter).Fly()//调用自身重名方法
    fly.Fly();//调用接口方法
}
```

```
{    
    Helicopter fly = new Helicopter();//是用自己声明的对象
    
    fly.Fly();//调用自身重名方法
    (fly.Fly as IFly).Fly()//调用接口方法
}
```

---

## 索引器（少用）

### **索引器**

- 索引器类似于属性
- 可以让我们通过索引值快速便捷的访问类对象的成员
- 很多时候，索引器使属性可以被索引，使用一个或多个参数索引属性，这些参数为某些值集合提供索引

### **索引器的声明**

```
访问修饰符 数据类型 this[索引值]
{
	get{return}
	set{value}
}
```

- 索引器适应于除自动属性以外的所有属性特征

```
class Student
{
    private string name,sex,address,age;
    public Student(string name,string sex,string address,string age)
    {
        this.name = name;
        this.sex = sex;
        this.address = address;
        this.age = age;
    }
    public string this[int i]//索引器
    {
        get
        {
            switch(i)
            {
                case 0:
                    return name;
                case 1:
                    return sex;
                case 2:
                    return address;
                case 3:
                    return age;
                default:
                    return "索引越界"
            }
        }
    }
}

//函数中使用
{
    Student s = new Student("张三","男","中国","18");
    s[0] = "王五";//此时对象s中姓名就被赋值为王五
    for(int i =0; i < 5; i++)
    {
        Console.WriteLine(s[i]);//打印出s对象中的所有字段
    }
}
```
---
# 数据结构

- 用来描述数据与数据之间的关系

## string引用类型和数组引用类型传值的区别

- 当字符串在主函数中作为一个变量传给一个方法时，在方法中对字符串进行修改并不影响主函数中字符串变量的值（修改值在堆中重新开辟空间）

- 当一个数组在主函数中作为一个变量传给一个方法时，在方法中对数组进行修改会影响主函数中数组的值（修改值不会重新开辟空间，而是在原空间中进行赋值）

---

## 装箱和拆箱

### **装箱**

是值类型到`object ` 类型或到此值类型所实现的任何接口类型的隐式转换。对值类型装箱会在堆中分配一个对象实例，并将该值复制到新的对象中

- 装箱的内部操作：对值类型在堆中分配一个对象实例，并将该值复制到新的对象中。按三步进行
  - 第一步：新分配托管堆内存(大小为值类型实例大小加上一个方法表指针和一个`SyncBlockIndex`)
  - 第二步：将值类型的实例字段拷贝到新分配的内存中
  - 第三步：返回托管堆中新分配对象的地址。这个地址就是一个指向对象的引用了

```
//代码示例
int i = 13;
object ob = i;
```

### **拆箱**

- 是从 `object` 类型到值类型或从接口类型到实现该接口的值类型的显式转换
- 检查对象实例，确保它是给定值类型的一个装箱值。将该值从实例复制到值类型变量中

```
//代码示例
int i = 13;
object ob = i;
int j = (int)ob;
```

### **装箱/拆箱对执行效率的影响(如何优化效率)**

- 装箱时,生成的是全新的引用对象,这会有时间损耗,也就是造成效率降低

- 避免装箱的方法：

  - 通过重载函数来避免
  - 通过泛型来避免

  > 凡事并不能绝对，假设你想改造的代码为第三方程序集，你无法更改，那你只能是装箱了。对于装箱/拆箱代码的优化，由于C#中对装箱和拆箱都是隐式的，所以，根本的方法是对代码进行分析，而分析最直接的方式是了解原理结何查看反编译的IL代码。比如：在循环体中可能存在多余的装箱，你可以简单采用提前装箱方式进行优化。

---

## 数组的增删改查插入

- 定义一个`Person`类

```
 public class Person 
 {
     public string name;
     public int age;
     public Person(string name, int age) 
     {
         this.name = name;
         this.age = age;
     }
 }
//声明一个数组persons，和一个记录长度的count
private Person[] persons = new Person[5];
public int count; 
```

### **增Add**

```
public void AddPerson(Person p) 
{
    for (int i = 0; i < persons.Length; i++)//找一下数组里是否有空位置
    {
        if (persons[i] == null) //有空位置则进行插入
        {
            persons[i] = p;
            count++;
            return;
        }
    }
    else//没有则调用拓展数组的方法
    	ExtendPerson(p);//拓展数组
}
//拓展数组的方法
private void ExtendPerson(Person p)
{
    int num = persons.Length;//记录之前的数
    Person[] newPersons = new Person[num * 2];//生成一个新的数组  长度是之前的两倍
    //把之前数据拷贝到新数组
    for (int i = 0; i < persons.Length; i++)
    {
        newPersons[i] = persons[i];
    }
    persons = newPersons;//新数组引用(地址)给persons，就是对persons进行了长度扩展

    AddPerson(p); //重新把最后的数据加进来
}
```

### **删Deleted**

```
public void DeletePerson(Person p)
{
    for (int i = 0; i < persons.Length; i++)
    {
        //当查找到我们要删除的对象时
        if (persons[i] == p) 
        {
            persons[i] = null;//将当前数组置为null；
            count--;           //只有到了这个地方才说明真的删除了一个对象，拥有的数量减一
            //让被删除的元素后面的数据向前串一个位置
            for (int j = i; j < persons.Length - 1; j++) 
            {
                persons[j] = persons[j+1 ];
                //最后一个位置置为null
                if (j + 1 == persons.Length- 1) 
                {
                    persons[j + 1] = null;
                }
            }
        }
    }
}
```

### **改Change**

```
 public void ChangePerson(Person p)
 {
     for (int i = 0; i < persons.Length; i++)//遍历数组
     {
         if (persons[i] == p)//找到这个元素
         {
             p.name = "英雄";//修改元素neir
         }
     }
 }
```

### **查Search**

```
 public Person SearchPerson(Person p)
 {
     for (int i = 0; i < persons.Length; i++)//遍历数组
     {
         if (persons[i] == p)//找到这个元素
         {
             return persons[i];//返回这个元素
         }
     }
     Console.WriteLine("没有找到"+p.name);
     return null;
 }
```

### **插入Insert**

```
public void InsertPerson(Person p,int index)//在下标为index的地方插入元素p
{
    for (int i = count; i > index ; i--)//倒叙遍历，遍历到要插入的下标号停止
    {
        persons[i] = persons[i - 1];//将在下标index的元素及后面的所有元素往后移
    }
    persons[index] = p;//插入元素
    count++;
}
```

---

# 集合

- 由一系列相同类型的元素组成的==数据结构==
- 首先引入一个新的命名空间：`using System.Collections`，后面的几个集合都基于这个命名空间下
- 几个常用集合：
  - 线性表`ArrayList`
  - 哈希表`HashTable`
  - 栈`Stack`
  - 队列`Queue`

## 线性表ArrayList

- 是对数组进一步的封装，保持了数组连续结构的特性，又可以使数组可以动态添加和删除
- 是线性的集合，有顺序

### **声明**

`ArrayList list = new ArrayList();`

### **属性**

`Count`：`list`存的数据的个数

`Capacity`：当前`list`的数组长度

`list[int index]`：索引器，下标从0开始

### **方法**

`Add(value)`：增加元素`value`

`Contains(value)`：是否包含元素`value`，返回布尔值

`Remove(value)`：在`list`里删除指定元素`value`

`RemoveAt(index)`：按下标号`index`删除元素

`RemoveRange(index,n)`：从下标号`index`开始删除`n`个元素

`Reverse()`：将`list`翻转输出

`Insert(index,value)`：在下标号`index`处插入元素`value`

`IndexOf(value)`：找到第一个与`value`相同的元素并返回其下标号

`LastIndexOf(value)`：找到最后一个与`value`相同的元素并返回其下标号

`Sort()`：将`list`正序排序

---

## 哈希表HashTable

- 散列状的，无顺序，通过键进行索引
- 散列排布的结构，为了方便查找，存储的元素是一堆键（目录）值（内容）对
- 键-值是对应的，可以多对一，但是不能一对多，即多个键可以对应一个值，一个键不能对应多个值
- 键不能重复，值可以重复

### **声明**

`HashTable table = new HashTable();`

### **属性**

`Count`：实际存的数据的个数

`table[object key]`：索引器，通过`key`可以索引到对应的元素

### **方法**

`Add(key,value)`：括号内是<键  值>对，增加一个数据，数据包括健值和元素

`Remove(key)`：删除一个存在的键，只要没有任何键指向值内容时，值就无人引用自动调用析构函数垃圾回收

`ContainsKey(key)`：当前`HashTable`是否包含这个`key`值，返回布尔值

---

## 栈Stack

- 特殊的链式结构
- 只有一个出口
- 先进后出，后进先出
- 栈顶：最后进栈的数据
- 栈底：最先进栈的数据

### **声明**

`Stack s = new Srack();`

### **属性**

`Count`：实际存的数据的个数

### **方法**

`Push(value)`：压栈/进栈，在栈顶添加一个新的元素

`Pop()`：弹栈/出栈，将最后一个元素从栈中删除，`Count`减一

`Peek()`：获得栈顶的数据，且栈内数据不会减少

`Contains(value)`：栈内是否包含元素m，返回布尔值

---

## 队列Queue

- 特殊的链式结构
- 两个出口
- 先进先出，后进后出
- 队首：第一个进队的数据
- 队尾：最后一个进队的数据

### **声明**

`Queue q = new Queue();`

### **属性**

`Count`：实际存的数据的个数

### **方法**

`Enqueue(value)`：进队，在队的末尾添加一个新的元素

`Dequeue()`：出队，将队首元素从队列中删除

`Peek()`：获得队首数据，数据不减少

`Contains(value)`：队内是否包含元素m，返回布尔值

---

# 泛型

- 是`C#`推出的一种特性，当我们在定义方法、类的时候，想先不定义数据类型（这种逻辑和结构适用于所有数据类型），这个时候我们可以用类型替代符`<>`来暂时代替我们的数据类型，在编译的时候，会根据你调用时候填入的类型，去进行判别

## 自定义泛型

###  **泛型类和泛型方法的声明**

```
//泛型方法
访问修饰符 返回类型 方法名<类型替代T,U>(T参数,U参数)
{
    
}
//泛型类
class 类名<类型替代符T>
{
	泛型替代符T 成员;
}
```

- 我们可以在方法名/类名后使用 `<类型替代符>`来定义一个泛型方法/泛型类

### **泛型类的使用**

```
泛型类<数据类型> 对象名 = new 泛型类<数据类型>();
```

- 在定义对象的时候在类型替代符`< >`里传进数据类型，后面这个对象传进的参数只能是这个数据类型

### **泛型方法的使用**

```
对象名.方法名<数据类型>(对应数据类型的参数);
```

- 在调用方法的时候在类型替代符`< >`里传进数据类型(可以不写，编译器会根据传的参数自行判断)，后面这个对象传进的参数只能是这个数据类型

---

# 泛型集合

- 在命名空间`System.Collections.Generic`里
- 泛型集合就是在原有的集合上增加了泛型的特性，可以更方便的使用

## 线性List

- 是一个连续性的集合
- 在`ArrayList`的基础上增加了泛型的特性，方法和属性和`ArrayList`相差无几

### **定义**

```
//自定义一个泛型
List<数据类型> list = new List<数据类型>();
List<数据类型> list = new List<数据类型>(初始长度);//一般不自己加，不够会自己增加长度
```

### **方法**

`CopyTo(arr);`：将整个`List`复制给一个数组`arr`，从下标0开始，还有其他两个重载可以自己F12看

`Add(value)`：将定义时对应的数据类型的数据`value`添加到泛型集合里

`RemoveAt(index)`：将下标号为`index`的数据删除

`Insert(index,value)`：在下标号`index`的位置插入数据`value`

`IndexOf(value)`：从头开始找数据`value`返回下标号

`LastIndexOf(value)`：从尾开始找数据`value`返回下标号

`Sort()`：对集合中的元素正向排序，有其他重载方法

### **遍历**

- 可以使用`for`循环和`foreach`进行遍历访问

### **接口**

- `Icomparable`接口：
  - `Sort方法`可以对默认数据类型进行排序 如 `int`,`float`，但不能对我们自己写的类型排序，但是微软考虑到我们自己书写的类型也需要排序，所以留下了`IComparable<T>`接口，可以让我们去重新定义比较器的逻辑，我们自己书写的类去继承`IComparable<T>`接口,重写排序逻辑，通过返回值去决定他们的顺序

```
int Compare( T x, T y );//x:要比较的第一个对象。y:要比较的第二个对象。
//返回结果:
//     一个有符号的整数，指示x和y的相对值，如下表所示。 返回值小于零：x小于y。 返回值等于零：x等于y。 返回值大于零：x大于y。
public int Compare( T x, T y )
{
    if (x < y)
        return -1;
    else if (x == y)
        return 0;
    else
        return 1;
}
//选择正向或者是反向排序只要更改返回值就行了
```

- 选择正序或者是倒序只要==更改返回值==就行了

---

## 字典Dictionary

- 字典在内存中为散列结构，使用`key`找到对应的值
- 对应`HashTable`的泛型结构

### **定义**

`Dictionary<Key值类型,Value值类型> dic;`

`Dictionary<Key值类型,Value值类型> dic = new Dictionary<Key值类型,Value值类型>();`

### **方法**

`Add(key,value)`：添加字典元素，存放的是一对健-值对

`dic[key] = value`：另一种添加字典元素的办法

`dic[key]`：索引器，用来访问字典元素

`ContainsKey(key)`：判断字典中有没有这个键，返回布尔值

### **遍历**

- 使用`foreach`进行遍历，但是一般==不会遍历字典元素==，只要通过目录`key`查找到元素`value`就行

```
foreach(KeyValuePair<key类型,value类型> item in 字典变量dic)
{
    //item可以去点到key或者value
    //item.key访问到key值
    //item.value访问到内容
}
```

### **练习**

```
//计算字符串中每个字母出现的个数"Welcome to HXSD!Welcome to Unity World!"
{
    string s = "Welcome to HXSD!Welcome to Unity World!";
    char[] chars = s.ToArray();
    Dictionary<char, int> charDic = new Dictionary<char, int>();
    for (int i = 0; i < chars.Length; i++)
    {

        if (charDic.ContainsKey( chars[i] ))
        {
            //当前字典中已经有了这个字符，则把value值加1
            charDic[chars[i]]++;
        }
        else
        {
            //当前字典中没有这个字符，新添加进字典，value等于1
            charDic.Add( chars[i], 1 );
        }
    }
    foreach (KeyValuePair<char,int> item in charDic)
    {
        Console.WriteLine("{0}:{1}",item.Key,item.Value );
    }
}
```

- 练习中的几个方法
  - `s = s.ToLower();//将字符串的所有字符转化为小写`
  - `s = s.ToUpper();//将字符串的所有字符转化为大写`

---

# 委托与事件

## 委托Delegate

- 委托也是一种数据类型，是存的方法的引用
- 委托变量可以承载一个或多个方法
- 委托(`Delegate`)是存有对某个方法或多个方法引用的一种引用类型
- 所有的委托(`Delegate`)都派生自`System.Delegate`类
- 委托本质上就是方法的列表（有顺序）

### **委托类型的声明**

`访问修饰符 delegate 返回类型 委托类型名(参数列表);`

- 由于委托是一个方法的引用，所以在声明委托的时候我们需要指定其方法的签名(即==**委托的返回类型和参数**要和委托的**方法的返回类型和参数**完全一致==)

### **委托的使用**

```
//委托的返回类型、参数要和委托的方法的返回类型、参数要完全一致，方法不需要括号
委托类型名 委托变量名 = new 委托类型名(方法);
委托类型名 委托变量名 = 方法;
//后续要继续委托其他方法
委托变量名 += 方法;//委托的注册
委托变量名 -= 方法;//委托的注销
//此时直接使用委托变量就可以同时调用这几个委托的方法
委托变量名(参数);
委托变量名.Invoke(参数);
-------------------------------------------------------------------------------------
//例如：定义一个委托类型Dele(),两个方法Ga();Te()
public delegate void Dele(int arg);
public void Ga(int arg){}
public void Te(int arg){}
//委托的使用
Dele del = new Dele(Ga);//此时方法Ga就委托给了del
del += Te;//注册Te方法
del -= Te;//注销Te方法
del(参数);//有几个方法委托给del，此时调用del就会同时执行者几个方法
del.Invoke(参数);//另一种调用方法
```

- 委托类型可以作为 一个参数传给一个方法
- 委托可以将方法当做一个变量去使用
- 一个委托变量，在另一个方法中去调用叫做回调

---

## 泛型委托

### **泛型委托的定义**

`访问修饰符 delegate U 委托名<T,U>(T a,U b);`

### **封装好的两个泛型委托类型**

**C#提供了两个做好的泛型委托类型供我们使用**

1.不带返回值的泛型委托，参数列表就是方法对应的参数

​	`Action<类型1，类型2...> 委托变量名 = 方法名`

2.带返回值的泛型委托，参数列表末尾是方法返回值类型

​	`Func<类型1，类型2..,返回值类型> 委托变量名 = 方法名`

### **匿名方法**

`委托变量名 = delegate(参数列表){方法体};`

- 匿名方法更像是一个临时变量，这个方法只会用这一次，不会被其他地方重复调用，除了这个委托变量。
- 参数列表、返回值要和委托变量的参数列表、返回值完全一致

### **匿名方法Lambda表达式**

`委托变量名 = (参数列表) =>{方法体};`

### **匿名方法的使用**

```
//例如定义一个委托变量 act
Func<int,int> act = delegate(a)
                        {
							a = 3;
    						return a;//有返回值类型就必须加return
                        }
//Lambda表达式
Action<int> = (a) => a=3;
```

---

## 事件
`public event 委托类型名 事件名;`

- 当我们的委托变量被保护起来，只允许外部注册或者注销的时候，则称这个委托变量为事件
- 因为每次我们都会写注册注销的接口给外部，过于麻烦，所以就有关键字`event`
- 加上这个关键字后，外部只能通过这个变量进行注册和注销，但外部不能使用这个委托方法
- 事件可以看做是一个委托类型的变量
- 通过+=为事件注册多个委托实例或多个方法
- 通过-=为事件注销多个委托实例或多个方法
- 事件只能自己调用自己，外部是不能调用的

### **观察者模式**
- 有一群人观察或者订阅某一对象，当这个对象的行为触发事件后，会通知这群观察者或者订阅者，而这群观察者就会像条件反射一样，去执行自己对应的行为
- 观察者模式就是使用事件来达成的，只要把事件对象，封装到自身身上，交由自己类触发执行，别人只能订阅或者观察我（注册），一旦我触发这个事件，就会触发这个观察者的行为（注册进来的方法）
```
namespace 事件_老师打铃
{
    public delegate void PlayTime();
    class Teacher {
        public string name;
        public event PlayTime playTimeEvent = null;
        public void CallClassOver() {
            playTimeEvent();
        }
    }

    class Student {
        public string name;
        public string doSth;

        public Student(string name,string dosth) {
            this.name = name;
            this.doSth = dosth;
        }

        public void DoSth() {
            Console.WriteLine("{0},在打铃的时候{1}",name,doSth);
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            Teacher t = new Teacher();
            t.name = "张三";


            Student xiaoming = new Student("小明", "买东西吃");
            t.playTimeEvent += xiaoming.DoSth;
            Student xiaozhang = new Student("小张", "打水");
            t.playTimeEvent += xiaozhang.DoSth;
            Student xiaohong = new Student("小红", "练习");
            t.playTimeEvent += xiaohong.DoSth;
            Student xiaohua = new Student("小花", "打羽毛球");
            t.playTimeEvent += xiaohua.DoSth;

            t.CallClassOver();

            Console.ReadLine();
        }
    }
}

```

---

## 多线程

- 引入命名空间`using System Threading`

### **多线程的声明**

- 新开一条线程运行其他方法

`Thread 变量名 = new Thread(无返回类型无参数的方法)`

### **多线程的方法**

`变量名.Start()`：开启这个线程

`Thread.Sleep(Time)`：这是一个静态方法，会暂停这个方法所在的线程多少毫秒，`Time`是毫秒单位

`变量名.Abort()`：关闭此线程

