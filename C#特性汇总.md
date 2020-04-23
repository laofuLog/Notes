# C#特性汇总

## 3.0特性

### 1.var隐式类型

```csharp
var i = 5;

var s = "Hello";

var a = new[] { 0, 1, 2 };

var expr =
    from c in customers
    where c.City == "London"
    select c;

var anon = new { Name = "Terry", Age = 34 };
                           
var list = new List<int>();
```

- 只有在同一语句中声明和初始化局部变量时，才能使用 `var`；不能将该变量初始化为 `null`、方法组或匿名函数。
- 不能将 `var` 用于类范围的域
- 不能在同一语句中初始化多个隐式类型的变量
- 如果范围中有一个名为 `var `的类型，则 `var `关键字将解析为该类型名称，而不作为隐式类型局部变量声明的一部分进行处理
- 在很多情况下，`var `是可选的，它只是提供了语法上的便利。但是，在使用匿名类型初始化变量时，如果需要在以后访问对象的属性，则必须将该变量声明为 `var`。这在` LINQ` 查询表达式中很常见
- 由` var `声明的变量不能用在初始化表达式中。换句话说，表达式 `int i = (i = 20) `是合法的；但表达式` var i = (i = 20) `则会产生编译时错误。

### 2.设置对象初始值

```csharp
public class Person
{ 
    public string Name { get; set; }
    public int Age { get; set; }
}

class Program
{
    static void Main(string[] args)
    {
        Person p = new Person { Name="sandy",Age=20};
    }
}
```

### 3.设置集合初始值

```csharp
List<int> listInt = new List<int> { 0, 1, 2, 3, 4, 5  };

List<Person> listP = new List<Person> 
{ 
    new Person(){Name="robbin",Age=10},
    new Person(){Name="sandy",Age= 20}
};
```

### 4.拓展方法

```csharp
public static class StrExtensions
{
    public static int WordCount(this String str)
    {
        return str.Split(new char[] { ' ', '.', '?' },StringSplitOptions.RemoveEmptyEntries).Length;
    }
}
class Program
{
    static void Main(string[] args)
    {
        string s = "Hello Extension Methods";
        int i = s.WordCount();
        Console.WriteLine(i);
    }
}
```

- 拓展方法只能在静态类中定义，且必须也是静态方法

### 5.匿名类型

```csharp
var v = new { Amount = 108, Message = "Hello" };
```

### 6.匿名方法Lambda

- 详细参照百度o.o

### 7.LINQ

- 详细参照百度o.o

### 8.自动实现属性

```csharp
class LightweightCustomer
{
    public double TotalPurchases { get; set; }
    public string Name { get; private set; } 
    public int CustomerID { get; private set; } 
}
```

### 9.分部方法定义

- 可以将类或结构、接口或方法的定义拆分到两个或多个源文件中。每个源文件包含类型或方法定义的一部分，编译应用程序时将把所有部分组合起来。
- 处理大型项目时，使一个类分布于多个独立文件中可以让多位程序员同时对该类进行处理
- 使用自动生成的源时，无需重新创建源文件便可将代码添加到类中。`Visual Studio` 在创建 `Windows` 窗体、`Web`服务包装代码等时都使用此方法。无需修改 `Visual Studio `创建的文件，就可创建使用这些类的代码。
- 若要拆分类定义，请使用 `partial `关键字修饰符，如下所示：

```csharp
public partial class Employee
{
    public void DoWork()
    {
    }
}

public partial class Employee
{
    public void GoToLunch()
    {
    }
}
```

- 分部方法简单一句话，就是分部类中定义的方法。

## 4.0特性

### 1.可空类型

- `int c = null;`是错误的，但是4.0可以写成`int ?c = null;`
- 值类型后边加`？`表示可空类型，是一种特殊的值类型，这种类型就可以赋值为`null`；
- 对于可空类型的一些操作

```csharp
// HasValue
if (c.HasValue)//只有可空类型才能进行判断
{
    Console.WriteLine(d.Value);//通过value属性获取值
}

// GetValueOrdefault(value)获取可空类型的值，如果没有值返回一个默认值 括号中的值就是自己设置的默认值
Console.WriteLine(x.GetValueOrDefault(100));

//GetHanCode()有值就返回值 没有值就返回一个默认值，不能指定默认值
Console.WriteLine(x.GetHashCode());

//空合并操作符?? 
//判断??左边的值，如果有值就返回值，如果没有就返回??右边的值
int z = y ?? 100;

//空合并操作符左边不能是普通值类型，可以是引用类型，也可以是可空类型 同类型只能跟同类型比(可空类型是特殊的值类型)
string sr=null;
string str = sr ?? "sg";
```

### 2.dynamic动态类型

```csharp
static void Main(string[] args)
{
    dynamic t = new ExpandoObject();
    //Abc是没有声明的变量，但使用dynamic可以直接调用并赋值，会动态判断
    t.Abc = "abc";
    t.Value = 10000;
    Console.WriteLine("t's abc = {0},t's value = {1}", t.Abc, t.Value);
}
```

- 使用`dynamic`的好处在于，可以不去关心对象是来源于`COM, IronPython, HTML DOM`或者反射，只要知道有什么方法可以调用就可以了，剩下的工作可以留给`runtime`。下面是调用`IronPython`类的例子：

```csharp
ScriptRuntime py = Python.CreateRuntime();
dynamic helloworld = py.UseFile("helloworld.py");
Console.WriteLine("helloworld.py loaded!");
```

### 3.可选参数

```csharp
static void DoSomething(int notOptionalArg,string arg1 = "default Arg1", string arg2 = "default arg2") 
{
    Console.WriteLine("arg1 = {0},arg2 = {1}",arg1,arg2);
}
```

- 上述例子里方法有三个参数，第一个是必选参数，第二个和第三个是可选参数，他们都有一个默认值。这种形式对固定参数的几个方法重载很有用。

### 4.命名参数

- 命名参数让我们可以在调用方法时指定参数名字来给参数赋值，这种情况下可以忽略参数的顺序。

```csharp
static void DoSomething(int height, int width, string openerName, string scroll) 
{
    Console.WriteLine("height = {0},width = {1},openerName = {2}, scroll = {3}",height,width,openerName,scroll);
}

//调用，可以指定参数名字赋值
DoSomething( scroll : "no",height : 10, width : 5, openerName : "windowname");
```

## 5.0特性

### 1.支持null类型的运算

```csharp
int? x = null;
int? y = x + 40;   		//y结果为40
Myobject obj = null;
Myotherobj obj2 = obj.MyProperty ??? new Myotherobj();
```

### 3.异步编程async和await

- 详细查百度->_->

## 6.0特性

### 1.nameof

- 用于获取变量、类型或成员的简单（非限定）字符串名称。可以在错误消息中使用类型或成员的非限定字符串名称，而无需对字符串进行硬编码，这样也方便重构。

```csharp
//这里用来验证字符串的参数是否为空
private void Func(string msg)
{
    if (string.IsNullOrEmpty(msg))
    {
        throw new ArgumentException(nameof(msg));
    }
}
```

- `typeof`获得变量类型名，`nameof`获得变量类型名，结合一起可以获得完全名

### 2.内插字符串，字符: $

- 用` $ `来构造字符串。 内插字符串表达式类似于包含表达式的模板字符串。内插字符串表达式通过将包含的表达式替换为表达式结果的 `ToString `表现形式来创建字符串

```csharp
var name = "Fanguzai";
Console.WriteLine($"Hello, {name}");

//结果 Hello，Fanguzai
```

- 想要在内插字符串中包含大括号`（“{” 或 “}”）`，请使用两个大括号，即` “{{” 或 “}}”`

### 3.NULL条件运算符: ?

- 用于在执行成员访问 (`?.`) 或索引 (`?[`) 操作之前，测试是否存在 `NULL `值。 这些运算符可让你编写更少的代码来检查 `null `值。

```csharp
string name = null; 
Console.WriteLine($"1：{name?.Length}");

name = "Fanguzai";
Console.WriteLine($"2：{name?.Length}");
Console.WriteLine($"3: {name?[0]}");
```

- 来看看另一种用途，可以使用非常少的代码以线程安全的方式调用委托、

```csharp
//普通的委托调用
Func<int> func = () => 0;
if (func!=null)
{
    func();
}

//简化调用
func?.Invoke();
```

### 4.catch 和 finally 块中使用 await

- 现在可以在 `catch` 和 `finally` 块中使用 `await 了`

```csharp
async Task Test()
{
    var wc = new WebClient();
    try
    {
        await wc.DownloadDataTaskAsync("");
    }
    catch (Exception)
    {
        await wc.DownloadDataTaskAsync("");
    }
    finally
    {
        await wc.DownloadDataTaskAsync("");
    }
}
```

### 5.自动实现的属性

- 现在可以通过与初始化字段相似的方式来初始化自动属性。当属性访问器中不需要任何其他逻辑时，自动实现的属性会使属性声明更加简洁

```csharp
 class MyClass
 {
     public string Name { get; set; } = "Fanguzai";
 }
```

### 6.只有getter 的自动属性

```csharp
class Person
{
    //新语法
    private string Name { get; } = "Fanguzai";  
    //不用带上 private set;

    //旧语法
    public int Age { get; private set; } ;
}
```

### 7.具有表达式主体的函数成员

- 可以采用与用于 `lambda `表达式相同的轻量语法，声明具有代码表达式主体的成员。具有立即仅返回表达式结果，或单个语句作为方法主题的方法定义很常见。 以下是使用 `=>` 定义此类方法的语法快捷方式

```csharp
class MyClass
{
    public int this[int id] => id;  //索引

    public double Add(int x, int y) => x + y;   //带返回值方法

    public void Output() => Console.WriteLine("Hi,Fanguzai!"); //无返回值方法
}
```

### 8.索引初始值设定项

- 现在可以初始化支持索引编制的集合的特定元素（如初始化字典）。如果集合支持索引，可以指定索引元素

```csharp
var nums = new Dictionary<int, string>
{
	[7] = "seven",
	[9] = "nine",
	[13] = "thirteen"
};

//这是旧的方式
var otherNums = new Dictionary<int, string>()
{
    {1, "one"},
    {2, "two"},
    {3, "three"}
};
```

### 9.using static 类型

- 可以导入静态类型的可访问静态成员，以便可以在不使用类型名限定访问的情况下引用成员

```csharp
using System;
using static System.Console;

namespace usingStatic类型
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hi,Fanguzai!");
            WriteLine("Hi,Fanguzai!");  
            // 使用了 using static System.Console;
        }
    }
}
```

- `　　using static` 仅导入可访问的静态成员和指定类型中声明的嵌套类型，不会导入继承的成员

## 7.0特性

### 1.多返回值

```csharp
static void Main(string[] args)
{
    int int1 = 25;
    int int2 = 28;
    var result = Add_Multiply(int1, int2);
    Console.WriteLine($"Add: {result.add}, Multiply: {result.multiply}");
}
public (int add, int multiply) Add_Multiply(int int1, int int2) 
    => (int1 + int2, int1 * int2);
```

- 基于`Tuple `做了语法简化的语法糖罢了，只是给人一种多个返回值的错觉

### 2.本地方法

- 在方法体内部定义一个方法

```csharp
public void Bar(int[] arr)
{
    var length = arr.Length;
    string Length() {
        return $"length is {length}";
    }
    //或:
    //string Length() => $"length is {length}";
    Length();
}
```

- 本地方法与` Lambda `的比较
- 性能：当创建 `Lambda `的时候，将会创建一个委托，这需要内存分配，因为委托是一个对象。而本地方法则不需要，它是一个真正的方法。另外，本地方法可以更为有效地使用本地变量，`Lambda `将变量放到类中，而本地方法可以使用结构，而不使用内存分配。这意味着调用本地方法更为节约且可能内联
- `Lambda `也可以实现递归，但是代码丑陋，您需要先赋予` lambda `为 `null`。本地方法可以更为自然地递归
- `Lambda `不能使用泛型。这是因为需要赋予一个实例类型的变量
- `Lambda` 不能使用 `yield return `(以及 `yield break`)关键字，以实现 `IEnumerable<T> `返回函数。本地方法可以

- 本地方法实现递归

```csharp
private static BigInteger GetFactorialUsingLocal(int number)
{
    if (number < 0)
        throw new ArgumentException("negative number", nameof(number));
    else if (number == 0)
        return 1;
    BigInteger result = number;
    while (number > 1)
    {
        Multiply(number - 1);
        number--;
    }

    void Multiply(int x) => result *= x;
    return result;
}

private static BigInteger GetFactorial(int number)
{
    if (number < 0)
        throw new ArgumentException("nagative number", nameof(number));
    return number == 0 ? 1 : number * GetFactorial(number - 1);
}
```

- 话说回来，由于它的**本质**是**成员方法**，所以它可以避免 [委托 / Lambda表达式] 的种种限制，可以**异步**，可用**泛型**，可用**out**, **ref**, **param**, 可以**yield**, **特性参数**， 等等

### 3.模式匹配

- 详细查看网址[https://docs.microsoft.com/zh-cn/dotnet/csharp/tutorials/pattern-matching]

### 4.引用返回

- `C#7.0 `中引入了**引用返回**(**ref return**)的概念，允许`C#`方法中返回一个值类型的引用。

```csharp
static ref int Max(ref int first, ref int second, ref int third)
{
    ref int max = first > second ? ref first : ref second;
    return max > third ? ref max : ref third;
}

int a = 1, b = 2, c = 3;
Max(ref a, ref b, ref c) = 4;
Debug.Assert(a == 1); // true
Debug.Assert(b == 2); // true
Debug.Assert(c == 4); // true
```

## 8.0特性

### 1.可空引用类型

- 从此，引用类型将会区分是否可分，可以从根源上解决 `NullReferenceException`。但是由于这个特性会打破兼容性，因此没有当作 error 来对待，而是使用 `warning `折衷，而且开发人员需要手动 `opt-in `才可以使用该特性（可以在项目层级或者文件层级进行设定）

```csharp
string s = null; // 产生警告: 对不可空引用类型赋值 null
string? s = null; // Ok

void M(string? s)
{
    Console.WriteLine(s.Length); // 产生警告：可能为 null
    if (s != null)
    {
        Console.WriteLine(s.Length); // Ok
    }
}
```

### 2.异步流

- 考虑到大部分 Api 以及函数实现都有了对应的 `async`版本，而 `IEnumerable<T>`和 `IEnumerator<T>`还不能方便的使用 `async`/`await`就显得很麻烦了。但是，现在引入了异步流，这些问题得到了解决。
  我们通过新的 `IAsyncEnumerable<T>`和 `IAsyncEnumerator<T>`来实现这一点。同时，由于之前 `foreach`是基于`IEnumerable<T>`和 `IEnumerator<T>`实现的，因此引入了新的语法`await foreach`来扩展 `foreach`的适用性。

```csharp
async Task<int> GetBigResultAsync()
{
    var result = await GetResultAsync();
    if (result > 20) return result; 
    else return -1;
}

async IAsyncEnumerable<int> GetBigResultsAsync()
{
    await foreach (var result in GetResultsAsync())
    {
        if (result > 20) yield return result; 
    }
}
```

### 3.范围和下标类型

- C# 8.0 引入了 Index 类型，可用作数组下标，并且使用 ^ 操作符表示倒数。**不过要注意的是，倒数是从 1 开始的。**

```csharp
Index i1 = 3;  // 下标为 3
Index i2 = ^4; // 倒数第 4 个元素
int[] a = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
Console.WriteLine($"{a[i1]}, {a[i2]}"); // "3, 6"
```

- 除此之外，还引入了 `..` 操作符用来表示范围（注意是左闭右开区间）。

```csharp
var slice = a[i1..i2]; // { 3, 4, 5 }
```

- 关于这个下标从 0 开始，倒数从 1 开始，范围左闭右开，笔者刚开始觉得很奇怪，但是发现 Python 等语言早已经做了这样的实践，并且效果不错。因此这次微软也采用了这种方式设计了 C# 8.0 的这个语法。

### 4.接口的默认实现方法

```csharp
interface ILogger
{
    void Log(LogLevel level, string message);
    void Log(Exception ex) => Log(LogLevel.Error, ex.ToString()); // 这是一个默认实现重载
}

class ConsoleLogger : ILogger
{
    public void Log(LogLevel level, string message) { ... }
    // Log(Exception) 会得到执行的默认实现
}
```

- 在上面的例子中，`Log(Exception)`将会得到执行的默认实现

### 6.目标类型推导

- 前我们写下面这种变量/成员声明的时候，大概最简单的写法就是

```csharp
var points = new [] { new Point(1, 4), new Point(2, 6) };
private List<int> _myList = new List<int>();
```

- 现在我们可以这么写啦

```csharp
Point[] ps = { new (1, 4), new (3,-2), new (9, 5) };
private List<int> _myList = new ();
```

### 注意

1. 以上的新特性需要 .NET Standard 2.1/.NET Core 3.0/.NET Framework 4.8 及以上来支持。
2. 但是，由于接口的默认实现方法这个特性需要 CLR 的支持，而 .NET Framework 4.8 还没有来得及做出修改，因此此特性在 .NET Framework 4.8 中**不可用**，需要等待进一步的更新。
3. 所有新增功能没有详细说明的都可以查看https://docs.microsoft.com/zh-cn/dotnet/csharp/whats-new/csharp-8

