# AssetBundle包

## AssetBundle资源管理

- 之前很多动态资源都放在`Resource`文件夹下，但是当Unity项目打包出去的时候`Resource`文件夹下的资源都会被打包出去

  - 存储在`Resources`下的资源，最终会存储在游戏的主体包中，发送给用

    户，手机系统上，如果需要做资源的更新，是无法使用`Resources`即时更新（不

    需要关闭手机游戏，即可实现游戏更新，即热更新）

- 这时就需要引入`AsserBundle`文件包，简称`AB`包

  - 其实可以理解为一个压缩包，这个压缩包可以理解为一个文件夹，文件夹里包含了多个文件，这些文件可以分为两类

    - serialzed file：资源被打碎放在一个对象中，最后被统一写进一个单独得文件（只有一个）
    - resource files：资源源文件

  - `AB`包是独立于游戏主包存在的资源存储文件，使用内部资源时，需要单独下

    载和加载。

### **AB包和Resource的区别**

- `AB`包可以理解为可下载的`Resource`，但其实结构是不一样的

- 存储：

  - `Resources`内部资源存储在游戏的发布包中

  - `AB`包存储在独立的文件中：`AB`包存储在非特殊目录下时，不在游戏的发                         

    布包中

- 加载：

  - `Resources`内部资源使用`Resources.Load()`
  - AB包的加载：
    - 通过`UnityWebRequest`下载这个AB包文件
    - 通过名称去加载AB包中的内部资源

### **AssetBundle的定义**

- `AssetBundle`是把一些资源文件，场景文件或二进制文件以某种紧密的方式保存在    

  一起的一些文件。

- `AssetBundle`内部不能包含C#脚本文件，`AssetBundle`可以配合Lua实现资源和游戏

  逻辑代码的更新

  - 即`AssetBundle`不能更新代码，只能更新资源，所以需要配合Lua实现

------

## AB包的创建与使用

### AB包的使用流程

- 指定资源的AB包名
- 创建AB包
- 上传AB包
- 加载AB包和包内资源

### AB包的创建 

#### 插件AssetBundle Browser

- 下载`AssetBundle`插件
  - 先在`GitHub`上搜`Unity`，找一下`AssetBundle`，下载下来
  - 导入到项目的Asset文件夹下，其实就是一个插件
  - 可以Windows任务栏中找到`AssetBundle Browser`插件

- 在资源设置面板的右下角可以`New`一个AB文件的名字，或者选择一个文件名字
  - 也可以打开插件，直接右键新建一个AB文件或者直接将资源拖入到`Configure`
- 打开插件`AssetBundle Browser`就可以在`Configure`看到新建的这个AB文件名字
- 点击`Build`在`Build Target`选择平台，`Output Path`选择路径，点击`build`就可以创建一个AB文件
- AB包配置修改后或AB内部的资源修改后，都需要重新生成AB包
- 导入到AB包的资源，包内部是不存在目录的，如果把一个目录当成一个文件放入AB包中将会报错
  - 如果AB包名称如果配置为这样的结构”ui/package”，ui会作为AB包存储的父目录，package是AB包的名称

##### Build下的参数信息

- `Build Target`：平台
- `Output Path`：输出路径
- `Clear Folders`：生成新的AB包的时候删除旧的同名AB包
- `Copy to StreamingAssets`：
- 以下是高级设置
- `Compression`：AB包的压缩方式
  - 不压缩：AB包比较大，下载较慢，加载速度快，因为CPU不用运算解压缩
  - `LZMA`算法压缩：默认压缩模式，文件尺寸最小，加载速度比LZ4长，使用的时候会将包整体解压
  - `LZ4`算法压缩：5.3版本以后可用，压缩比例居中，文件居中，加载速度居中，使用的时候不需要整体解压，可以指定加载资源而不用加载全部，最好选用这种压缩方式

#### 代码创建

- 使用到的API：`BuildPipeline.BuildAssetBundles()`
- 参数时：打包路径，压缩选项，对象平台等
- 使用代码打包时，先判断路径是否存在，若不存在则进行创建
- `BuildPipeline`在`UnityEditor`的命名空间下

```csharp
[MenuItem("Tools/BuildAssetBundles")]
private static void BuildAllAssetBundles()
{
    string url = "AssetBundles";
    if (!Directory.Exists(url))
    {
        Directory.CreateDirectory(url);
    }
    BuildPipeline.BuildAssetBundles(url, BuildAssetBundleOptions.None, BuildTarget.StandaloneWindows64);
    Debug.Log("finishd");
}
```

### AB包的使用的四种方式

- 主要有四种方式
  - `AssetBundle.LoadFromMemoryAsync`：当AB包在服务器上下载下来时有可能得到的是个byte数组，这时候可以用这个将byte数组转化为AB包文件
  - `AssetBundle.LoadFromFile`：本地加载
  - `WWW.LoadFromCacheOrDownload`：已经弃用了，不推荐使用，与`UnityWebRequest`一致
  - `UnityWebRequest`：服务器网络端下载下AB包文件

### **本地加载AB包内部数据**

- `Resource`加载文件直接是加载文件夹下的资源，填写一个`Resource`路径就可
- 加载AB包分两步，第一步加载AB包文件，第二部根据资源在AB包内的名称加载AB包内的资源
  - 加载：加载的含义就是将文件状态的AB文件加载到内存中
  - 卸载：将内存中的AB文件释放掉
  - 第一步：加载AB包文件，这个路径要直接填写电脑路径加AB包名称，返回的就是AB包文件对象
    - `AB包 = AssetBundle.LoadFromFile(AB包文件路径)`
    - `AssetBundle.LoadFromFileSync(AB包文件路径)`，异步加载
      - 异步加载得到的是一个`AssetBundleRequest`请求类型，访问请求类型下的`asset`属性就会得到资源
  - 第二步：加载AB包内部资源，参数是资源在AB包中的名称
    - `资源对象 = AB包对象.LoadAsset<资源类型>(“资源名称”)`
    - `AB包对象.LoadAssetSync<资源类型>(“资源名称”)`，异步加载
- **注意**：AB包不能重复加载，但是可以卸载后再加载
  - 但可以同一个AB包重复加载资源
  - 卸载方法`AB包对象.Unload(bool)`

```c#
{
    //加载这个AB包文件
    AssetBundle ab = AssetBundle.LoadFromFile("url");
	//加载这个包文件中的资源
    Sprite sprite = ab.LoadAsset<Sprite>("assetName");
	//使用包内的资源
    GameObject.Find("/Canvas/AB").GetComponent<Image>().sprite = sprite;
}
```

- 一定要在使用完之后卸载AB包对象，减少内存的占用

### 服务端加载AB包

- `WWW.LoadFromCacheOrDownload`：已经弃用了，不推荐使用，与`UnityWebRequest`一致

- `UnityWebRequest`：服务器网络端下载下AB包文件，一共仨步

  1. 创建一个访问web服务器的请求，可以根据访问不同类型资源使用不同的类，例如这里要访问AB包资源就使用的是`UnityWebRequestAssetBundle`
     - ``UnityWebRequest webRequest = UnityWebRequestAssetBundle.GetAssetBundle(url);`

  2. 开启下载请求
     - `webRequest.SendWebRequest()`
  3. 得到资源文件，同样不同的文件有不同的`DownloadHandler`类
     - `AssetBundle asset = DownloadHandlerAssetBundle.GetContent(webRequest)`

- 注意在开启下载请求后，最好还要做一下判空操作

  - `String.IsNullOrEmpty(webRequest.error)`，是否下载错误

```csharp
IEnumerator Start()
{
    string url = @"http://localhost/AssetBundles/zxr/pink.unity";
    //创建请求
    UnityWebRequest webRequest = UnityWebRequestAssetBundle.GetAssetBundle(url);
    //开启下载请求
    yield return webRequest.SendWebRequest();
    //判空
    if (!String.IsNullOrEmpty(webRequest.error))
    {
        UnityEngine.Debug.Log(webRequest.error);
        yield break;
    }
	//获得AB包资源
    AssetBundle asset = DownloadHandlerAssetBundle.GetContent(webRequest);
    //使用AB包内资源
    GameObject obj = Instantiate(asset.LoadAsset<GameObject>("Sprite"));
    obj.name = "WebRequest";

}
```

### **AB包的依赖关系**

- 如果一个AB（名称A）包中的资源，使用到了另一个AB（名称B）包的资源，那么两

  个AB包就产生了依赖关系。也就是A依赖于B。那么在使用A中资源的时候就也要把B包一起加载出来，否则A中资源就会出现资源不完整的情况

  - 在A中的配置文件`.manifest`中的`Dependencies`就会记录这种依赖关系

- 在这个主AB包，即和储存目录同名的AB文件的配置文件中，记录了所有这个包下所有文件的依赖关系

- 如果要加载有依赖关系AB包，除了使用`LoadFromFile`把一个一个包加载，也可以去先加载主AB包，加载数据有以下几个步骤

  - 加载主AB包
  - 根据主AB包的配置文件，获得我当前需要加载的AB所依赖的AB们
  - 将所有的依赖AB们，加载进来
  - 然后就是加载你想要的AB包

```c#
{
    
    //加载主AB包
    AssetBundle ab = AssetBundle.LoadFromFile(url);
    //加载主AB包的配置文件，文件类型就是AssetBundleManifest
    AssetBundleManifest abm = ab.LoadAsset<AssetBundleManifest>("AssetBundleManifest");
    
    //查询某个AB包的依赖情况
    //参数：被查询依赖关系的AB包名称
    //返回值：被查询依赖关系的AB包，所依赖的所有AB包名称
    string[] names = abm.GetAllDependencies(name);
    
    //加载所有的依赖
    foreach (string name in names)
    {
        //加载依赖的AB包
        AssetBundle.LoadFromFile(name);
    }
    
	//加载这个AB包
    AssetBundle realAB = AssetBundle.LoadFromFile(url);
	//加载这个包中的资源
    GameObject prefab = realAB.LoadAsset<GameObject>(name);
	//使用
    GameObject go = GameObject.Instantiate<GameObject>(prefab);
    go.transform.SetParent(GameObject.Find("/Canvas").transform);
}
```

------

## AB包的卸载内存的释放

- `Resources`加载的资源，Unity会存储在内存中，用`Destory`删除对象后是不会降内存的，资源的内存占用依然存在，所以要使用`  Resources.UnloadUnusedAssets();`定期的释放场景没有引用的资源
- 加载AB包，内存的占用根据AB包的压缩模式来决定
  - `LZMA`算法压缩：虽然压缩后尺寸最小，但是加载会占用内存，就要跟`Resources`一样，不使用时候要释放掉
    - 卸载方法`AB包对象.Unload(bool)`
  - `LZ4`算法压缩：不会占用内存，可以一直挂着这个AB包，不需要去释放内存
- 加载AB包中的资源，无论是什么压缩模式都会导致内存占用的上涨，所以就和卸载方法中的参数有关
  - `.Unload(false)`：仅仅释放这个AB包但是不释放AB包加载出来的资源，如果仅仅释放包，资源就成为游离资源，这部分内存是没有被释放的，如果继续加载同一个包，加载同一个资源，内存会再叠加上涨，**因为这个资源虽然是同名同资源，但不是从一个包出来的，所以会叠加占用内存**
    - 同一个AB包重复加载资源，不会叠加占用内存
  - ==重点==：`.Unload(true)`：不仅释放AB包，也释放AB包中的资源，并且资源占用的那部分内存也会被释放，推荐使用这种方式，但是这样在使用的资源也会被卸载，所以卸载之前要确保资源已经不需要使用了，一般可以在关卡与关卡之间调用这个API，就是转场景的时候。

## AB包使用的一些问题

1. 依赖包重复问题
   - 把需要的共享的资源打在一起
   - 分割包，这些包不是在同一时间使用的
   - 把共享的资源达成一个包，其他包只去依赖这个包
2. 图集重复问题
   - 需要给图片规划一个图集，然后把这个图集下的图片打进一个包

------

# Lua

## 语言的执行方式

- 编译型语言：代码在运行前需要使用编译器，先将程序源代码编译为可执行文件，再执行。
  - `C/C++`
  - `Java`，`C#`
  - `Go`，`Objective-C`
- 解释型语言（脚本语言）：需要提前安装编程语言解析器，运行时使用解析器执行代码，就可以在不编译就可以直接运行，即写即运行，开发速度快，但是效率比编译型语言慢点，解释性语言可以实现热更
  - `JavaScript（TypeScript）`
  - `Python`，`PHP`，`Perl`，`Ruby`
  - `SQL`
  - `Lua`

------

## Lua文本工具安装和解析器安装

- 先安装文本工具`Sublime`
- 安装解析器
- Lua回顾网站：<https://www.runoob.com/lua/lua-tutorial.html>

------

## Lua语言特点

- `Lua `是一种轻量小巧的脚本语言，用标准`C`语言编写并以源代码形式开放，其设计目的是为了嵌入应用程序中，从而为应用程序提供灵活的扩展和定制功能。
- 特点：
  - **轻量级:** 它用标准`C`语言编写并以源代码形式开放，编译后仅仅一百余K，可以很方便的嵌入别的程序里。
  - **可扩展:** `Lua`提供了非常易于使用的扩展接口和机制：由宿主语言(通常是`C`或`C++`)提供一部分功能，`Lua`可以使用它们，就像是本来就内置的功能一样。
- 使用领域：
  - 游戏行业（魔兽世界的插件，愤怒的小鸟）
  - `Web`领域（`Nginx`功能开发）
  - 嵌入式领域
- 使用方式：
  - 需要高性能的程序部分，使用（`C/C++`）实现。需要快速实现功能部分，使用`Lua`实现。
- 国内游戏行业流行的`Lua`使用方式：
  - Cocos2D-X(Cocos)：核心引擎使用`C++`实现，提供`LuaBinding`（`API`的`Lua`调用）,游戏逻辑开发部分可使用`Lua`开发（刀塔传奇）
  - Unity3D：使用如xLua插件（`API`的`Lua`调用），实现游戏核心代码使用`C#`实现（打包时`il2cpp`，编译为`C++`），游戏逻辑使用`Lua`开发（王者荣耀）
- **`Lua`支持多返回值**

------

## Lua语法

```lua
--打印方法
print("hello world");

-- 单行注释

--[[
	多行注释
]]

--Lua可以不加分号

--Lua的变量不需要声明变量类型
name="不需要声明变量类型"
print(name);

--可以打印没有复制的变量，打印出来的是nil，nil==null
print(id);

--销毁变量赋值为nil，变量就会销毁
name=nil;

--这个变量是一个局部变量
local data = "局部变量"
print(data)
```

### **Lua变量的作用域**

- 当前文件：加了`local`就只在这一个文件有效，其他地方都无用
- 当前方法(函数)
- 系统全局：不加`local`就是全局变量，整个编译器下都有效，任意地方随意访问，轻易不要使用全局变量
- 如果在一个函数中加`local`，那么这个变量只在这个函数的作用域中有作用

### **Lua函数与方法**

- 在`C#`中，函数和方法是同一个东西，都必须写在类或者接口中，外面必须包裹一层
- 但是在`Lua`中，可以定义函数也可以定义方法，方法就是像`C#`一样要有一个作用域，还可以定义一个纯函数，不属于任何表

### **Lua保留字**

|            |         |         |          |
| ---------- | ------- | ------- | -------- |
| `and`      | `break` | `do`    | `else`   |
| `else if`  | `end`   | `false` | `for`    |
| `function` | `if`    | `in`    | `local`  |
| `nil`      | `not`   | `or`    | `repeat` |
| `return`   | `then`  | `true`  | `until`  |
| `while`    |         |         |          |

### **Lua数据类型**

- `type()`函数判断变量类型，这个函数的返回值是一个`string`类型

```lua
local name = "Unity"

--判定变量的数据类型
print(type(name))  				--string

--判定变量类型是不是字符串
print(type(name)=="string")  	--true

--对没有声明的变量查询类型
print(type(id))					--nil

--判定一个变量是否已经定义
print(type(id)=="nil")  --true
--判定一个变量是否已经定义第二种方法
print(id==nil)  				--true


--lua的数字只有number型
print(type(123))  				--number
print(type(0.12))  				--number

--布尔型
print(type(true))  				--boolean
print(type(false))  			--boolean

--单引号字符串
print(type('Unity'))  			--string

--Lua一切复杂数据皆表（table）
--C#对象，结构体，数组，列表，字典
print(type({}))   				--table

--只打印一个大括号是内存地址
print({})						--table: 00A0C278

```

- `nil`：空值
- `boolean`：布尔型
- `number`：所有数字皆是`number`，无论整型字符型
- `string`：字符型，可以单引号和双引号
- `function`：函数类型
- `table`：表，复杂数据类型

### **Lua字符串的操作**

- `Lua`里的下标从1开始

```lua
--声明单行字符串
local str1 = 'abc'
local str2 = 'def'

--多行字符串声明
--每次换行都会插入一个换行符‘\n’,会占一个长度
local str3 = [[
	ghi
	jkl
]]

--字符串拼接
print(str1..str2)			--abcdef

--Lua会将字符串尝试转化为数字再相加，如果字符串不是数字就会报错，如果是数字就会等于和
print('6'+'7')				--13

--Lua获取字符串长度
print(#str1)				--3

--字符串函数使用
--string.函数名（）

--字符串大写化
print(string.upper(str1))

--字符串小写化
print(string.lower(str2))

--字符串反转
print(string.reverse(str1))

--字符串查找(KMP算法)
--参数1：要查找的字符串
--参数2：查找的内容
--返回值(多返回值)：起始位置和结束位置
print(string.find("abcdef","cd"))	-- 3  5

--字符串截取
--参数1：被截取的字符串
--参数2：起始下标
--返回值：从下标到结束的字符串
print(string.sub("abcdef",3))  		--cdef

--带有结束位置的截取字符串
print(string.sub("abcdef",3,5)) 	--cde

--截取倒数第二个字符
local str4 = 'acvdefghijk'
print(string.sub(str4,3,#str4-1))

--字符串格式化
print(string.format("I'm a Player %s,Level is %d","FYX",99))
--I'm a Player FYX,Level is 99

--字符串重复
--参数1：重复的字符串
--参数2：重复的次数
print(string.rep("abc",3))

--字符串修改
--参数1：要修改的字符串
--参数2：修改的字符串里的内容
--参数3：要替换的内容
--返回值1：替换后的内容
--返回值2：替换的次数
print(string.gsub("abcdef","cd","**"))
--ab**ef	1

--字符串转ASCII
print(string.byte("A"))
--ASCII转字符
print(string.char(65))

```

### **Lua运算符**

- 没有`++`，`--`
- 没有`+=`，`-=`，`*=`，`/=`
- 次方`^`
- 不等于`~=`

### **Lua的表[数组]**

```lua
-- 数组实际上就是表，现阶段只考虑数字索引
-- 这样就声明了一个数组，下标从1开始
local data = {}
-- 内部存储类型可以混合
data={"abc",123,[-1]=100,[0]=true,[4]=233,one="212",["two"]=11}
print(data[-2])

-- 获取table的长度
-- 计算表长是从1开始的，并且根据连续的索引计算，如果中间有断开的索引值，计数就停止
print("数组长度：".. #data)

--通过下标号操作
data[0]=3.14

--通过下标号索引
print(data[2])
print(data["one"])
print(data.two)

--多维table
local multi={{"abc",123},{true,nil}}

--多维表2
local multI={
	{"abc",123},
	{true,nil,
	{'hehe'}}
}

print(multI[2][3][1])
```

- 数组实际上就是表，表不止代表数组，可以当字典、列表等等来使用，现阶段只考虑数字索引
- `Lua`里的表下标从1开始，索引还可以是字符串的索引，若是用了字符串索引，就相当于字典的键值
- 表中存储的数据可以混合存储，一个数组里既可以放字符串也可以放数字其他类型
- 索引可以有间隔，跳着索引值进行查询，所以跟字典很像，不需要遍历表
- 还可以指定下标号进行赋值，下标号可以是0和负数
- 如果没有赋值的下标号那这个下标号上的值就是nil
- 数组可以声明后赋值也可以直接赋值
- 如果没有提高索引的数据，下标是从1开始
- 计算表长是从1开始的，并且根据连续的索引计算，如果中间有断开的索引值，计数就停止，一般计算表长不要这种方式，不准确，一般使用迭代器
- 修改值直接通过下标进行修改，可以连类型都修改
- 支持多维表，跟多维数组意思一样，可以在表中再套表

### **Lua的分支判断循环控制语句**

```lua
--if
--Lua的if判断语句没有大括号，使用then开头end结束，前面判断条件也可以不加小括号，then和end支持无限极嵌套
if true 
then

end

--if else
--同样不需要大括号，再then和end中间写个else，会自动检测并把两部分分开
if false 
then
	
else
	
end

--if elseif else
--if后一定要跟then，其实这两个结合就像大括号，然后结尾是else或者end
if false 
then
	
elseif true 
then

else
	
end

--if嵌套
if true 
then
	if true 
	then
		print("进入第二层if")
	end
end

```

- `Lua`的判断语句没有大括号，`if`加`then`代表判断语句起始，`else`或者`end`结尾
- `if`一定要跟`then`
- `Lua`没有`switch`分支方法，全都用`if`来写
- 判断语句都是`then`起始`end`结尾

```lua
--while循环，使用do开头，end结尾
while num < 4 
do
	
end

--repeat until 用法类似于do while但不等同于do while
repeat

until --直到条件满足时跳出循环

--老司机写法，这样写在这种语言中可以提高效率，可以不写else if，并且写在until中，可以写上break让直行一个条件满足的时候就不执行后面的代码，提高效率
repeat
	if false
	then
		print("执行第一个分支")
		break
	end

	if true
	then
		print("执行第二个分支")
		break
	end

	if false
	then
		print("执行第三个分支")
		break
	end

until  true


--for循环，直接指定起始值和终点值就可
for i=1,10 
do
	print(i)
end

--for循环,指定增长步长
for i=1,10,2 
do
	print(i)
end
```

- `while`、`for`循环语句由`do`起始，`end`结尾
- `repeat..until`用法和`do..while`类似，但是`do..while`是直到条件不满足的时候跳出循环，而`until`是到条件满足时跳出循环
- `for`循环直接指定起始值和终点值即可，遍历表要从下标1开始，如果不指定增长步长的话，它会加1加1的增长
- `for`循环的步长可以指定负数，就是递减
- `for`循环只能遍历规则索引的表

### **Lua的与或非**

- `and`：与
- `or	`：或
- `not`：非

### **Lua迭代器**

- `ipairs`：续数字索引迭代器
- `pairs`：所有数据迭代器

```lua
--连续数字索引迭代器
--i：下标号
--v：数据
--ipairs(表)
for i,v in ipairs(data) 
do
	print("i和v  "..i,v)
end

--所有数据迭代器，可以遍历表中所有数据
for k,v in pairs(data) do
	print("k："..k,"  v:"..v)
end
```

- 连续数字索引迭代器和for循环没什么区别，同样只能遍历连续的数字索引，中间有断开就不能再继续索引
- 所有数据迭代器可以遍历所有数据，并返回下标号和对应的值

### **Lua函数**

```lua
--写一个函数， function开头 end结尾
function function_name()
	-- body
end

--函数的调用
function_name()

--本地函数的声明，其实这也像是一个委托，将一个函数赋值给fun2
local fun2 = function()
	--body
end

--可变参传参
local func3 = function ( ... )
	--body
end

--可变参传参，若要让传入参数相加做操作需要将参数转化为表
--arg是一个内置参数
local func3 = function ( ... )
	local arg = {...}
	
	local total = 0
	--不需要第一个参数的时候可以让他定义为一个下划线
	for _,v in pairs(arg) 
	do
		total=total+v
	end

	print("输出："..total)
end


-[[
    lua中函数的传参，参数不需要定义类型，而是自己约定传参，
    就是自己觉得需要传递什么，随后在调用函数的时候按想法传参就行
    例如下方：参数我自己约定第一个为字符串，第二个为表，第三个为函数
    那么传参的时候我就要按自己约定的传递参数即可，
    为了清楚，可以将参数名称定义为自己想传类型的名称缩写，这样就更清晰
]]
function Function1(str, tabels, fun)
    --body
end

--函数可以作为数据赋值，可以作为参数传递
--传参与匿名函数
Function1(
    "字符串参数1",
    A,
    function()
        --参数传递
        print("匿名函数")
    end
)

Tmep = Function1 --赋值
MyPrint = function(arg1, arg2, arg3)    --赋值，其实类似委托
    --body
end
```

- `Lua`中函数定义时从`function`开头，`end`结尾
- `Lua`中的函数结构实际上是一个变量，所以可以实现委托的效果
- `Lua`中函数的调用只能在定义的下方调用，但编译型语言可以在上方调用下方调用都行
- 解释型语言只能在定义下方调用函数，因为他们都是自上而下进行解析，编译型语言是整体编译过后再执行的，所以无论定义上方下方调用函数都可以
- 函数调用的时候，如果传入参数比定义的函数的参数多，也会正常调用，它只会用到它需要的参数，但是参数不能少，即传参只多不少
- 参数如果是三个点`...`，就是可变参传参，类似C#的`params`，但是无固定的参数需要转换为`Table`

### **Lua多返回值与多变量赋值**

```lua
function fun4()
	--返回两个返回值
	return 99,999
end
--用两个变量来接收这两个返回值
local num1,num2 = fun4()

print(num1)
print(num2)

--多变量赋值
local a, b = 10.20
local c, d, e = 11, 22, "33"
```

### **Lua表Table的常见操作**

- 表不仅可以用来代表数组列表字典等，也可以将他理解为一个对象

```lua
local data={"abc",123,[-1]=100,[0]=true,[4]=233,one="212",["two"]=11}
--因为function是一种数据类型
--这样就相当于将函数储存在了data表中，也就是成员方法
--这个函数就可以访问这个表中的其他变量
data.fun5=function ( ... )
	-- body
end 

--self等同于this，就是把自己当作参数传递过去了
--成员方法内部可以通过self获得当前表的其他数据
data.fun6=function(self)
	--取值
	print(self.one)
end

--self需要表，则将data传过去
data.fun6(data)
--冒号表示隐式传递data到fun6中
data:fun6()

--不需要在参数里写self也可以在函数内部通过self访问表中的值
--这里的意思是，冒号帮助data的fun7函数隐式声明了一个self参数，所以可以直接在内部使用self
function data:fun7()
	--取值
	print(self.one)
end


--将表里的数据全部拼接在一起，返回的是字符串
    --多参数
    --参数1：表
    --参数2：拼接的分割符号
    --参数3和4：指定索引区域的拼接
table.concat(data)

--表的插入
table.insert(data, "插入的数据")
--指定索引插入
table.insert(data, 2, "指定索引插入")

--表的移除
--不是置空操作，置空操作那个索引会消失，但是后面的数据不会向前移动
table.remove(data, 2)

--表的排序，不同类型数据无法排序
table.sort(data)
```

- 有时候这个表定义在其他文件中且是`local`，所以就需要用到`self`
- `self`等同于`this`
- `.`需要显示的传递表，`:`隐式传递表
- Lua冒号和点的区别
  - 声明时：使用点声明方法，如果在内部需要调用成员变量，则需要使用`self`，那么方法也需要声明`self`参数，而冒号会帮开发者隐式声明
  - 调用时：点相当于成员变量的方式调用，如果声明的方法上有`self`参数，则需要显示的传递当前表，而冒号会帮开发者隐式传递

### Lua中的模块module

- 类似命名空间的意思
- 在Lua中实现模块就是用表来实现，将所有的变量，函数等等放在一个表里，就实现了一个模块

```lua
--定义一个模块Module,该文件的文件名为config
Module = {}

Module.var = "模块中的变量"

Module.func1 = function()
    print("模块中的函数")
end

--可指定为Module表内的函数，其实也可以不指定，直接func2即可，其实都是属于该模块的
function Module.func2()
    print("模块中的函数")
end

local function func3()
    print("相当于一个私有函数")
end

return Module
```

### **Lua模块(文件)的使用**

- 要是想在一个文件调用另一个模块(文件)的变量或者方法，使用`require("文件路径")`
- 路径不加文件的拓展名
- 若是在同级目录下就可以直接通过文件名查找
- 若是在下级目录就通过`/文件夹名/文件名`路径查找
- 若是要用`require`加载上级目录有两步
  - 在path里添加一个访问上级目录的路径`package.path = package.path .. ';..\\?.lua'`
  - 然后就可以访问到上级目录`require "../HeHeBaby"`
  - 因为`require`查找文件是在`package.path`中去查找，若是没有添加上级目录的路径是默认访问不到的

```lua
--上级目录文件HeHeBaby
package.path = package.path .. ';..\\?.lua
require "../HeHeBaby

--文件fileResource
require("Config")
print(config["var"])

--也可以指定变量
m = require "Config"
print(m.var)

```

- 但是要访问其他文件的变量，那个变量必须是全局，这样很不安全，所以使用`return`返回这个变量，就可以保证这个变量只会被访问但是不会被修改，有点像属性
- `require`不支持多返回值
- 多次执行一个文件的内部代码，所有被加载过的文件，路径信息都会记录在`package.loaded`中，当再次加载的时候会判断这个信息，如果这个路径信息存在，则不会再次加载，必须要清空才可以

```lua
--文件fileResource
require("Config")
require("Config")--只执行一次输出

--文件fileResource
require("Config") 
package.loaded["Config"]=nil
require("Config") --可以两次输出了
```

### Lua中C包

- 可以引入C语言的包给Lua添加一些方法，详情查看菜鸟教程

### **Lua元表**

- 直接打印`table`，显示的是内存地址
- 期望打印的是表的内容，便于程序员阅读
- 现在就需要扩展`table`的功能，实现打印便于阅读，拓展使用的就是`metatable`语法特性---元表

```lua
local t1 = {1,2,3}

--直接打印table，显示的是内存地址
--期望打印的是'{1,2,3}'，便于程序员阅读
--现在就需要扩展t1的功能，实现打印便于阅读，拓展使用的就是metatable语法特性---元表
print(t1)   --table: 0097C228

--元表是一个table，但是这个表有特定的写法
--需求：打印利于阅读的内容，实际上是将表转化为string类型
local meta = {
	__tostring=function(t)
		local format='{'

		for i,v in pairs(t1) do
			format = format .. tostring(v) .. ','
		end

		format= format .. '}'
		return format
	end
}
--将meta设置为t1的功能拓展表(元表)
--设置元表的方法
setmetatable(t1,meta)
--将t1当成字符串进行打印
--如果t1设置了元表，并且元表中实现了__tostring方法，则自动调用元表的__tostring方法
print(t1)

```

- 元表的意思其实就是给某个表添加了一些方法，例如当执行打印操作时，就会先检查两个表之一中是否有元表，之后就会检查有没有一个`__tostring`的键，如果有就会调用对应键的值，而且对应的值往往是一个表或者一个函数，即“元方法”

  ![img](https://upload-images.jianshu.io/upload_images/1804600-14dc7f05beac96d6.png?imageMogr2/auto-orient/strip|imageView2/2/w/1186/format/webp)

- 自动调用的`tostring`关键字是两个下划线`__tostring`

- 设置元表的方法`setmetatable(表,元表)`，但是如果元表中有`__metatable`键值的话会关联失败

  - 也可已如此设置：`mytable=setmetatable({},{})`
  - `setmetatable`方法有返回值，返回的是普通表的数据

- `getmetatable(table)`：返回对象的元表，如果元表中有`__metatable`键，就会返回该键的值

- 使用`__metatable`可以保护元表，当`__metatable="lock"`时，可以禁止用户访问元表中的成员或者修改元表，此时`getmetatable`的时候得到的就是字符串

- 当两个表都有元表的时候，lua是怎么去调用表中的键值呢

  - 对于二元操作符，如果第一个操作数有元表，并且元表中有所需要的字段定义，比如我们这里的`__add`元方法定义，那么Lua就以这个字段为元方法，而与第二个值无关
  - 对于二元操作符，如果第一个操作数有元表，但是元表中没有所需要的字段定义，比如我们这里的`__add`元方法定义，那么Lua就去查找第二个操作数的元表
  - 如果两个操作数都没有元表，或者都没有对应的元方法定义，Lua就引发一个错误

### Lua元表中可设置元方法

```lua
__add(a, b) --加法
__sub(a, b) --减法
__mul(a, b) --乘法
__div(a, b) --除法
__mod(a, b) --取模
__pow(a, b) --乘幂
__unm(a) --相反数
__concat(a, b) --连接
__len(a) --长度
__eq(a, b) --相等
__lt(a, b) --小于
__le(a, b) --小于等于
__index(a, b) --访问表中不存的字段 ，访问时调用，rawget(tab, i)
__newindex(a, b, c) --索引更新,更新：向表中不存在索引赋值 ，赋值时调用,rawswt(t, key, value)
__call(a, ...) --执行方法调用，把表当作函数使用时调用，例如mytable(666)
__tostring(a) --字符串输出
__metatable --保护元表
```

- `__index`元方法

  - 当访问一个table的字段时，如果table有这个字段，则直接返回对应的值

  - 当table没有这个字段，则会促使解释器去查找一个叫`__index`的元方法，接下来就就会调用对应的元方法，返回元方法返回的值

  - 如果没有这个元方法，那么就返回`nil`结果

    ![img](https://upload-images.jianshu.io/upload_images/1804600-1a3a8561ade817ec.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

- `__newindex`元方法，当对一个table中不存在的索引赋值时，在Lua中是按照以下步骤进行的

  - Lua解释器先判断这个table是否有元表
  - 如果有了元表，就查找元表中是否有`__newindex`元方法；如果没有元表，就直接添加这个索引，然后对应的赋值
  - 如果有这个`__newindex`元方法，Lua解释器就执行它，而不是执行赋值
  - 如果这个`__newindex`对应的不是一个函数，而是一个table时，Lua解释器就在这个table中执行赋值，而不是对原来的table

- 有的时候，我们就不想从`__index`对应的元方法中查询值，我们也不想更新table时，也不想执行`__newindex`对应的方法，或者`__newindex`对应的table。那怎么办？在Lua中，当我们查询table中的值，或者更新table中的值时，不想理那该死的元表，我们可以使用`rawget`函数，调用`rawget(tb, i)`就是对table tb进行了一次“原始的（raw）”访问，也就是一次不考虑元表的简单访问；你可能会想，一次原始的访问，没有访问`__index`对应的元方法，可能有性能的提升，其实一次原始访问并不会加速代码执行的速度。对于`__newindex`元方法，可以调用`rawset(tabel, key, value)`函数，它可以不涉及任何元方法而直接设置table中与key相关联的value。

- 应用

  ```lua
  Set = {}
  mt = {} --元表
   
  function Set.new(l)
      local set = {}
      setmetatable(set, mt)
      for _, v in ipairs(l) do
          set[v] = true  --把表的数值设置为键
      end
      return set
  end
  --在此之后，所有由Set.new创建的集合都具有一个相同的元表，例如
  --[[
      local set1 = Set.new({10, 20, 30})
      local set2 = Set.new({1, 2})
      print(getmetatable(set1))
      print(getmetatable(set2))
      assert(getmetatable(set1) == getmetatable(set2))
  ]]
   
  --================================================
  function Set.tostring(set)
      local l = {}
      for e in pairs(set) do	--单个得到的就是键，而不是值
          tabel.insert(l,e)   --将键作为值插入l中
      end
      return "{" .. table.concat(l, ",") .. "}"
  end
   
   
  function Set.print(s)
      print(Set.tostring(s))
  end
   
   
  --1 加(__add), 并集===============================
  function Set.union(a, b)
      --如果a的元表不是mt后者b的元表不是mt则打印错误
    	if getmetatable(a) ~= mt or getmetatable(b) ~= mt then
          error("attemp to 'add' a set with a non-set value", 2)   
          --error第二个参数的含义P116
      end
      
      local res = Set.new{}
      
      for k in pairs(a) do 
          res[k] = true 
      end
      
      for k in pairs(b) do 
          res[k] = true 
      end
      return res
  end
   
  s1 = Set.new{10, 20, 30, 50}
  s2 = Set.new{30, 1}
   
  mt.__add = Set.union
   
  s3 = s1 + s2
  Set.print(s3)
   
  --元表混用
  s = Set.new{1, 2, 3}
  s = s + 8
  Set.print(s + 8)
  
   
  --2 乘(__mul), 交集==============================
  function Set.intersection(a, b)
      local res = Set.new{}
      for k in pairs(a) do
          res[k] = b[k]
      end
      return res
  end
   
  mt.__mul = Set.intersection
  --Set.print(s2 * s1)
   
   
   
  --3 关系类===================================NaN的概念====
  mt.__le = function(a, b)
      for k in pairs(a) do
          if not b[k] then 
              return false 
          end
      end
  	return true
  end
   
  mt.__lt = function(a, b)
      return a <= b and not (b <= a)
  end
   
  mt.__eq = function(a, b)           --竟然能这么用！？----
      return a <= b and b <= a
  end
   
  g1 = Set.new{2, 4, 3}
  g2 = Set.new{4, 10, 2}
  print(g1 <= g2)
  print(g1 < g2)
  print(g1 >= g2)
  print(g1 > g2)
  print(g1 == g1 * g2)
   
   
  --============================================
  --4 table访问的元方法=========================
  --__index有关继承的典型示例
  Window = {}
  Window.prototype = {x = 0, y = 0, width = 100, height}
  Window.mt = {}
   
  function Window.new(o)
      setmetatable(o, Window.mt)
      return o
  end
   
  Window.mt.__index = function (table, key)
      return Window.prototype[key]
  end
   
  w = Window.new{x = 10, y = 20}
  print(w.width)
   
  --__index修改table默认值
  function setDefault (t, d)
      local mt = {__index = function () return d end}
      setmetatable(t, mt)
  end
   
  tab = {x = 10, y = 20}
  print(tab.x, tab.z)
  setDefault(tab, 0)
  print(tab.x, tab.z)
   
  --13.4.5 只读的table
  function readOnly(t)
      local proxy = {}
      local mt = {
      __index = t,
      __newindex = function(t, k, v)
          error("attempt to update a read-only table", 2)
      end
      }
      setmetatable(proxy, mt)
      return proxy
  end
   
  days = readOnly{"Sunday", "Monday", "Tuesday", "W", "T", "F", "S"}
  print(days[1])
  days[2] = "Noday"
  
  ```

### Lua协程

**线程和协程的区别**

- 线程与协同程序的主要区别在于，一个具有多个线程的程序可以同时运行几个线程，而协同程序却需要彼此协作的运行。
- 在任一指定时刻只有一个协同程序在运行，并且这个正在运行的协同程序只有在明确的被要求挂起的时候才会被挂起。
- 协同程序有点类似同步的多线程，在等待同一个线程锁的几个线程有点类似协同。

**基本语法**

| 方法                  | 描述                                                         |
| :-------------------- | :----------------------------------------------------------- |
| `coroutine.create()`  | 创建 `coroutine`，返回 `coroutine`， 参数是一个函数，当和 `resume `配合使用的时候就唤醒函数调用 |
| `coroutine.resume()`  | 重启 `coroutine`，和 `create `配合使用                       |
| `coroutine.yield()`   | 挂起 `coroutine`，将 `coroutine `设置为挂起状态，这个和 `resume `配合使用能有很多有用的效果 |
| `coroutine.status()`  | 查看 `coroutine `的状态 注：`coroutine` 的状态有三种：`dead`，`suspended`，`running`，具体什么时候有这样的状态请参考下面的程序 |
| `coroutine.wrap（）`  | 创建 `coroutine`，返回一个函数，一旦你调用这个函数，就进入 `coroutine`，和 `create `功能重复 |
| `coroutine.running()` | 返回正在跑的 `coroutine`，一个 `coroutine `就是一个线程，当使用`running`的时候，就是返回一个 `corouting `的线程号 |

**应用**

```lua
--Lua中的协程

--定义一个协程
--参数1：传入函数，可以直接定义一个匿名函数
Co1 =
    coroutine.create(
    function(numA, numB)
        print(numA + numB)
        print(coroutine.status(Co1))--out:running(正在运行)
        --暂停协程，跳出继续向下执行代码，yiled中也可以传入参数，当作在暂停时协程的返回值
        coroutine.yield("暂停时的返回值") 
        print(numA - numB)
        return "返回值"
    end
)

print(coroutine.status(Co1))--out:suspended(暂停的未启动的)

--运行协程，启动协程
--参数：
    --1：协程函数
    --2：参数，传入协程函数需要的参数
--返回值：1.Boolean执行是否成功，2.函数的返回值
Res1, Res2 = coroutine.resume(Co1, 20, 30)
print(Res1, Res2)	--out:true,"暂停时的返回值"

print(coroutine.status(Co1))--out:suspended(暂停的未启动的)
print("协程暂停后输出")

Res3, Res4 = coroutine.resume(Co1) --继续运行被暂停的协程，可以只传函数不传参
print(Res3, Res4) 	--out:true,"返回值"

print(coroutine.status(Co1)) --out:dead(死亡)

--另一个定义协程函数的方法
--该方法启动不需要通过resume来启动，可以直接调用协程即可
Co2 =
    coroutine.wrap(
    function(numA, numB)
        print(numA * numB)
    end
)

--使用wrap创建的协程启动方法
Co2(10, 20)
```

### Lua中的文件操作

- Lua `I/O` 库用于读取和处理文件。分为简单模式（和C一样）、完全模式

  - 简单模式（`simple model`）拥有一个当前输入文件和一个当前输出文件，并且提供针对这些文件相关的操作
  - 完全模式（`complete model`） 使用外部的文件句柄来实现。它以一种面对对象的形式，将所有的文件操作定义为文件句柄的方法

- 简单模式在做一些简单的文件操作时较为合适。但是在进行一些高级的文件操作的时候，简单模式就显得力不从心。例如同时读取多个文件这样的操作，使用完全模式则较为合适

- 打开文件操作语句：`file = io.open (filename [, mode])`

  - `filename`就是文件名，也可以指定路径，如果只写文件名的话就会在lua文件的同级目录下查找文件

- `mode`的值有

  | 模式 | 描述                                                         |
  | :--- | :----------------------------------------------------------- |
  | `r`  | 以只读方式打开文件，该文件必须存在。                         |
  | `w`  | 打开只写文件，若文件存在则文件长度清为0，即该文件内容会消失。若文件不存在则建立该文件。 |
  | `a`  | 以附加的方式打开只写文件。若文件不存在，则会建立该文件，如果文件存在，写入的数据会被加到文件尾，即文件原先的内容会被保留。（EOF符保留） |
  | `r+` | 以可读写方式打开文件，该文件必须存在。                       |
  | `w+` | 打开可读写文件，若文件存在则文件长度清为零，即该文件内容会消失。若文件不存在则建立该文件。 |
  | `a+` | 与a类似，但此文件可读可写                                    |
  | `b`  | 二进制模式，如果文件是二进制文件，可以加上b                  |
  | `+`  | 号表示对文件既可以读也可以写                                 |

**应用**

- 简单模式

```lua
-- 以只读方式打开文件
file = io.open("test.lua", "r")

-- 设置默认输入文件为 test.lua
io.input(file)

-- 输出文件第一行
print(io.read())

-- 关闭打开的文件
io.close(file)

-- 以附加的方式打开只写文件
file = io.open("test.lua", "a")

-- 设置默认输出文件为 test.lua
io.output(file)

-- 在文件最后一行添加 Lua 注释
io.write("--  test.lua 文件末尾注释")

-- 关闭打开的文件
io.close(file)
```

- `read`也可以传递参数

  | 模式           | 描述                                                         |
  | :------------- | :----------------------------------------------------------- |
  | `"*n"`         | 读取一个数字并返回它。例：`file.read("*n")`                  |
  | `"*a"`         | 从当前位置读取整个文件。例：`file.read("*a")`                |
  | `"*l"（默认）` | 读取下一行，在文件尾 (EOF) 处返回 `nil`。例：`file.read("*l")` |
  | `number`       | 返回一个指定字符个数的字符串，或在 EOF 时返回 `nil`。例：`file.read(5)` |

- io的其他方法

  ```lua
  --用字符串 mode 指定的模式打开一个文件，返回一个文件类型
  io.open(filename: string ,[ mode: string("r")]) -> FILE*
  --设置 file 为默认输入文件
  io.input([file: string/FILE*])-> [FILE*]
  --设置 file 为默认输出文件。
  io.output([file: string/FILE*]) -> [FILE*]
  --读文件 file， 指定的格式决定了要读什么，等价于 io.input():read(···)
  io.read(mode: ...) -> string/number
  --将参数的值逐个写入默认输出文件，等价于 io.output():write(···)
  io.write(...) -> FILE* [2. errmsg: string]
  --用一个分离进程开启程序 prog 
  io.popen(prog: string ,[ mode: string("r")]) -> file: FILE*
  --将写入的数据保存到默认输出文件中，等价于 io.output():flush()
  io.flush()
  --如果成功，返回一个临时文件的句柄
  io.tmpfile() -> FILE*
  --检查 obj 是否是合法的文件句柄。 
  --如果 obj 它是一个打开的文件句柄，返回字符串 "file"
  --如果 obj 是一个关闭的文件句柄，返回字符串 "closed file"
  --如果 obj 不是文件句柄，返回 nil
  io.type(obj: FILE*) -> type: string
  --返回一个迭代函数，迭代函数的功能根据mode传的参数决定
  io.lines([filename: string, mode: ...])
  --关闭 file 或默认输出文件
  io.close([file: FILE*])
  ```

- 完全模式

  ```lua
  -- 以只读方式打开文件
  file = io.open("data.txt", "r")
  
  -- 输出文件第一行
  print(file:read())
  
  -- 关闭打开的文件
  file:close()
  
  -- 以附加的方式打开只写文件
  file = io.open("data.txt", "a")
  
  -- 在文件最后一行添加 Lua 注释
  file:write("--test")
  
  -- 关闭打开的文件
  file:close()
  ```

### Lua的错误处理/调试/垃圾回收

- [菜鸟教程](https://www.runoob.com/lua/lua-garbage-collection.html)

### Lua面向对象

```lua
--[[
    对象由属性和方法组成。LUA中最基本的结构是table，所以需要用table来描述对象的属性
    table是引用类型

    封装：lua 中的 function 可以用来表示方法。那么LUA中的类可以通过 table + function 模拟出来。
    继承：至于继承，可以通过 metetable 模拟出来（不推荐用，只模拟最基本的对象大部分时间够用了）

    Lua 中的表不仅在某种意义上是一种对象。像对象一样，表也有状态（成员变量）；
    也有与对象的值独立的本性，特别是拥有两个不同值的对象（table）代表两个不同的对象；
    一个对象在不同的时候也可以有不同的值，但他始终是一个对象；
    与对象类似，表的生命周期与其由什么创建、在哪创建没有关系。对象有他们的成员函数，表也有
]]
--声明一个类（表）
Shape = {}

--给类（表）添加成员变量
Shape.area = 0
Shape.name = "shape"

--创建一个构造函数
function Shape:new(otable, side)
    --如果传过来一个新的表则赋值给atable，如果没有传入数据，则默认构建一个空表
    local atable = otable or {}
    --设置元表为自己，这样通过new 方法创建出来的对象（表）的元表就是Shape
    setmetatable(atable, self)
    --！！！！这是重点：给元表的__index添加值为自己
    --当创建的对象访问某个成员的时候，就会先在自身查找，如果自身查找不到，就会去元表index指向的表中去查找
    self.__index = self
    --构造函数中初始化成员
    side = side or 0
    self.area = side * side
    --返回创建的对象（新表）
    return atable
end

--设置一个析构方法
Shape.__gc = function(atable)
    --对象销毁时调用
    print(atable.name, "--destroy")
end

--给类（表）添加成员函数
function Shape:printArea()
    print("面积为", self.area)
end

----------------------以上就是Lua中一个类的创建-----------------------

--创建对象（通过源表创建新表）
local myshape = Shape:new(nil, 10)

--使用成员方法
myshape:printArea()

-----------------------以下就是Lua中的继承-----------------------------

--创建一个新类，通过源类创建，其实就是构建一个新表，和上面的创建对象差不多
Square = Shape:new()

--创建派生类的构造函数
function Square:new(otable, side)
    --这个就是若有传递表内容过来，则新表的数据就等于该表，若没有则通过父类的构造方法创建
    --类似：base.new()的意思
    local atable = otable or Shape:new(otable, side)
    --设置元表，设置index属性
    setmetatable(atable, self)
    --这里就引出了继承的效果，该类创建的对象首先会在自己的元表里查找成员属性
    --若自己没有这个属性，那么因为square又是通过shape创建的，就会去shape里查找属性
    self.__index = self
    self.name = "square"
    return atable
end

--重写方法
function Square:printArea()
    print("正方形的面积为 ", self.area)
end

--创建对象
local mysquare = Square:new(nil, 10)
mysquare:printArea()

--创建一个派生类
Rectangle = Shape:new()
--派生类的构造函数，这里可以添加新的成员
function Rectangle:new(otable, length, breadth)
    local atable = otable or Shape:new(otable)
    setmetatable(atable, self)
    self.__index = self
    self.area = length * breadth
    self.name = "rectangle"
    return atable
end

--派生类重写方法
function Rectangle:printArea()
    print("矩形面积 ", self.area)
end

--创建对象
local myrectangle = Rectangle:new(nil, 10, 20)
myrectangle:printArea()
myrectangle = nil

-----------------------------------------
out:
    面积为	100
    正方形的面积为 	100
    矩形面积 	200
    rectangle	--destroy
    square	--destroy
    shape	--destroy
```

- [参考资料](https://www.jianshu.com/p/b538e5d9a871)

### **Lua作业**

```lua
--计算不规则表的长度
local tabel1 = {1,2,3,[0]=1,[6]=6,[9]=10,one=12,[4]={3,4,{1,2,3}}}
--递归
function count(a,con)

	if type(a)=="table" then
		--print("table")
		for _,v in pairs(a) do
			con = count(v,con)
		end
	else
		con = con + 1
	end

	return con
end

local con = 0
print("表的长度："..count(tabel1,con))

--两个表的合并
local tabel2 = {2,6,5,7,[0]=10,two=11}
local table3 = {1,2,3,4,5}

local meta = {
	
	__add=function (t1,t2)
		local temp = {}

		for _,v in pairs(t1) do
			table.insert(temp,v)
		end

		for _,v in pairs(t2) do
			table.insert(temp,v)
		end

		return temp
	end,

	__tostring = function(t)

		local format='{'
		for i,v in pairs(t) do
			format = format .. tostring(v) .. ','
		end
		format= format .. '}'

		return format
	end
}

local table5 = {}
setmetatable(tabel2,meta)
table5 = tabel2 + table3
setmetatable(table5,meta)
print(table5)

--冒泡排序
local maopao = function( ... )
	local data = {...}
	for i=1,#data,1 do
		for j=i+1,#data do
			if data[i]<data[j] then
				local tmp = data[i]
				data[i]=data[j]
				data[j]=tmp
			end
		end
	end
	return data
end
```

------

## 热更新

- 不关闭Unity应用的前提，实现游戏资源和代码逻辑的更新。
  - 例如王者经常上线时候加载的一些小文件
- 冷更新：直接停服更新，完了需要重新下载这个游戏，一般就是大版本更新
- 因为`Lua`可以实时更新代码，所以用`Lua`实现热更

### **热更新**

- 广义：无需关闭应用，不停机状态下修复漏洞，更新资源等，重点是更新逻辑代码。
- 狭义定义（ `iOS`热更新）：无需将代码重新打包提交至`AppStore`，即可更新客户端的执行代码，即不用下载`app`而自动更新程序。
- 现状：苹果禁止了`C#`的部分反射操作，禁止`JIT`（即时编译，程序运行时创建并运行新代码），不允许逻辑热更新，只允许使用`AssetBundle`进行资源热更新。
  - 注意：2017年，苹果更新了热更新政策说明，上线后的项目，一旦发现使用热更新，一样会以下架处理
- 为何要热更新：缩短用户获取新版应用的客户端的流程，改善用户体验，具体到`iOS`平台的应用上，有以下几个原因
  - `AppStore`的审核周期难控制
  - 手机应用更新频繁
  - 对于大型应用，更新成本太大
- 终极目标：不重新下载、不停机状态下完全变换一个应用的内容
- 每个平台如何做热更新
  - `Android`，`PC（C#）`端
    - 将执行代码预编译为`Assembly.DLL`，这个文件在`Library`下的`ScriptAssemblies`文件夹的`Assembly-CSharp.dll`
    - 将代码作为`TextAsset`打包进`AssetBundle`
    - 运行时调用`Assembly.DLL`代码
    - 更新相应的`AssetBundle`即可实现热更新
  - `iOS（Lua）`
    - 苹果官方禁止`iOS`下的程序热更新；`JIT`在`iOS`下无效
    - 热更新方案：`Unity `+ `Lua`插件

### **常见的Unity热更新插件**

- `sLua`：最快的`Lua`插件
- `toLua`：由`uLua`发展而来的，第三代`Lua`热更新方案
- `xLua`：特性最先进的`Lua`插件，鹅厂的
- `ILRuntime`：纯C#实现的热更新插件

------

# xLua的使用

- 将`xLua`的文件夹下`Assets`文件夹下的资源拷贝到项目中
- 导入命名空间`XLua;`
- C#执行`Lua`代码

```csharp
{
    //Lua是解释型语言，所以运行Lua代码，需要在Unity中创建Lua解析器

    //创建Lua解析器
    LuaEnv luaEnv = new LuaEnv();

    //将字符串作为Lua代码运行
    luaEnv.DoString("print('hello world')");//结果LUA: hello world

    //没有Lua代码再运行时，记得释放Lua解释器
    luaEnv.Dispose();

}

```

- C#调用`Lua`文件，必须放在`Resource`文件夹下，且必须是`txt`拓展名，因为`Unity`不支持`.lua`拓展名，但是这种系统自带的加载器很局限，必须在特点文件夹下，且还不是`Lua`文件

```csharp
{
    //Lua是解释型语言，所以运行Lua代码，需要在Unity中创建Lua解析器

    //创建Lua解析器
    LuaEnv luaEnv = new LuaEnv();

    //使用require调用文件
    luaEnv.DoString("require 'task' ");

    //没有Lua代码再运行时，记得释放Lua解释器
    luaEnv.Dispose();

}
```

## **自定义加载器**

- 自定义加载器`LuaEnv.AddLoader()`
- 因为`require`是通过路径去查找的文件，所以要找到文件，就需要去自定义加载器添加路径
- 自定义加载器优先级高于内置加载器

```csharp
{
     //创建Lua解析器
    LuaEnv luaEnv = new LuaEnv();

    //将自定义加载器的回调函数添加到xLua中
    //当Lua编写require代码的时候，自定义加载器的回调函数就会执行
    luaEnv.AddLoader(MyLoader);
	
    //test文件不在Resource文件夹下，而是在同级的一个Lua文件夹下
    luaEnv.DoString("require 'test'");

    //没有Lua代码再运行时，记得释放Lua解释器
    luaEnv.Dispose();
}
byte[] MyLoader(ref string path)
{
    //path是回调函数街道的是require加载的那个文件的名称，没有拓张吗
	
    //添加一个路径
    //返回值是希望将Lua的代码文件的内容以byte[]方式返回
    string realPath = "D:/UnityProject/xLuaProject/Assets/Lua/" + path + ".lua";

    //将文件读取为byte[]
    byte[] code = File.ReadAllBytes(realPath);

    return code;
}
```

- 但是这样添加路径太固定了，使用`Application.dataPath`访问到自己本身这个项目的`Asset`文件夹，所有项目内容都在`Asset`文件夹下，再通过字符串拼接去添加路径

```csharp
string realPath = "D:/UnityProject/xLuaProject/Assets/Lua/" + path + ".lua";
//-------替换为----------
string realPath = Application.dataPath + "/Lua/" + path + ".lua";
```

- 但是还需要让加载器去判空，所以要再改造一下，如果没有找到文件就会调用自带加载器

```csharp
 byte[] MyLoader(ref string path)
 {
     //path是回调函数街道的是require加载的那个文件的名称，没有拓张吗

     //返回值是希望将Lua的代码文件的内容以byte[]方式返回
     string realPath = "D:/UnityProject/xLuaProject/Assets/Lua/" + path + ".lua";

     realPath = Application.dataPath + "/Lua/" + path + ".lua";

     //通过文件路径，判断文件是否存在
     if (File.Exists(realPath))
     {
         return File.ReadAllBytes(realPath);
     }
     else
     {
         //null表示当前加载器没有加载到需要的Lua文件(xLua会继续用其他的加载器加载Lua文件)
         return null;
     }

 }
```

## **使用AB包加载Lua脚本**

- 因为AB包也不支持`.lua`拓展名，所以打进AB包的文件都需要是`.txt`文件
- 因为AB包文件夹一般是在`Asset`文件夹的同级目录，所以需要通过字符串操作获得AB包的目录

```csharp
{
    luaEnv.AddLoader(MyLoader);
    luaEnv.DoString("require 'subfile'");
}


byte[] MyLoader(ref string path)
{
    //加载AB包
    //得到Asset的路径后减去Asset这个字符串再拼接上AB包文件夹的名称就是AB包的路径
    string abPath = Application.dataPath.Substring(0, Application.dataPath.Length - 7) + "/AB";
    string abFile = abPath + "/lua";

    //加载Lua以TextAsset方式读取出来
    AssetBundle assetBundle = AssetBundle.LoadFromFile(abFile);
    TextAsset textAsset = assetBundle.LoadAsset<TextAsset>(path + ".lua");

    //通过文件路径，判断文件是否存在
    if (textAsset == null)
    {
        return null;
    }
    else
    {
        return textAsset.bytes;
    }

}
```

## **单例模式的解析器LuaEnv**

- 因为`LuaEnv`会被频繁使用，所以整个全局只需要一个解析器就够了，所以要编写一个单例模式的解析器

### **xLua拓展学习**

[沈军老师博客][https://shenjun-coder.github.io/LuaBook/%E7%AC%AC%E4%B8%89%E7%AB%A0%20XLua%E6%95%99%E7%A8%8B/]

------

## xLua与C#代码的相互调用

- `Unity`是基于`C#`语言开发的，所有生命周期函数都是基于`C#`实现，`xLua`本身是不存在`Unity`的相关生命周期函数的。如果希望`xLua`能够拥有生命周期函数，那么我们可以实现`C#`作为`Unity`原始调用，再使用`C#`调用`Lua`对应的方法。

```csharp
//以下Lua调用使用的参考类
namespace Hxsd
{
    //静态类，其中包括静态变量，静态属性和静态方法
    public class TestStatic
    {
        public static int ID = 100;
        //这样写就默认写了一个属性，很方便
        public static string Name
        {
            get;
            set;
        }
        public static string OutPut()
        {

            return "static";
        }
        public static void  Default(string str="abc")
        {
            Debug.Log("C#:" + str);
        }
    }
    //非静态类，其中包括构造函数、变量、属性和方法
    public class TestClassObject
    {
        public string name;
        public int ID
        {
            get;
            set;
        }

        public TestClassObject()
        {

        }
        public TestClassObject(string name)
        {
            this.name = name;
        }
        public int Output()
        {
            return ID;
        }
    }
}
```

### **Lua调用C#静态结构**

- `Lua`调用静态类内部结构，在`lua`中写调用`C#`的静态方法
  - `CS.命名空间.类名.需要调用的结构`
  - 若是没有命名空间可以直接`CS.类名`

```lua
--调用静态类和静态结构就是通过 CS.命名空间.类名.需要调用的结构 访问
CS.Hxsd.TestStatic.ID=99
CS.Hxsd.TestStatic.Name="Unity"
print(CS.Hxsd.TestStatic.ID)
print(CS.Hxsd.TestStatic.Name)
print(CS.Hxsd.TestStatic.OutPut())
```

### **Lua调用C#非静态结构**

- `Lua`通过调用`C#`的构造方法，实现`C#`类的实例化
- 实例化之后就可以通过`实例化对象.变量/属性`访问
- 访问方法需要通过冒号去访问，因为方法内部会使用到`this`等关键字访问自己的成员变量，所以`lua`调用的时候就要用冒号将自己的实例化对象传入到方法中

```lua
-- Lua实例化对象
-- Lua通过调用C#的构造方法，实现C#类的实例化
local obj = CS.Hxsd.TestClassObject()
--通过对象访问属性和变量
obj.name = "名字"
obj.ID = 98
-- 在成员方法中为了保证其内部可通过this访问到成员变量，所以必须通过传递当前表的方式
print(obj:Output())
--还可以调用有参构造函数
local newObj = CS.Hxsd.TestClassObject("霸王枪")
print(newObj.name)
```

### **Lua调用C#继承的方法**

```csharp
//调用继承
public class TestFather
{
    public string name = "father";
    public void Talk()
    {
        Debug.Log("这是来自父对象的方法");

    }
    public virtual void Cover()
    {
        Debug.Log("这是父对象的虚方法");
    }

}

public class TestChild : TestFather
{
    public override void Cover()
    {
        Debug.Log("这是来自子对象的重写方法");
    }
}
```

- `Lua`可以调用C#中继承多态的特性
- 也可以调用重载的方法，传入参数就可以了，但是`lua`中数字不分浮点或整型，所以参数如果分整型或浮点型的化只会统一认为是数字，会调用失败
  - 所以避免使用`Lua`中可合并类型进行参数传递的重载函数

```lua
local father = CS.TestFather()
print(father.name)
father:Talk()
father:Cover()

local child = CS.TestChild()
child.name="child"
print(child.name)
child:Talk()
child:Cover()
```

### **Lua调用C#拓展的方法**

```csharp
//调用类拓展
public class TestExtension
{
    public void Output()
    {
        Debug.Log("类本身定义的方法");
    }
}
[LuaCallCSharp]
public static class MyExtension
{
    public static void Show(this TestExtension test)
    {
        Debug.Log("扩展类实现的方法");
    }
}
```

- 默认`Lua`是不能调用拓展方法的，所以需要在拓展类的上打一个标签`[LuaCallCSharp]`

```lua
local obj = CS.TestExtension()
--调用对象本身的方法
obj:Output()
--调用拓展方法
obj:Show()
```

### **Lua调用C#结构体的方法**

```csharp
//结构体
public struct TestStruct
{
    public static int ID;
    public static string name;
    public string Gender
    {
        get;
        set;
    }

    public static string Output()
    {
        return name;
    }

    public string Talk()
    {
        return Gender;
    }

}
```

- 和类的调用时非常类似的

```lua
--静态结构的使用
CS.TestStruct.ID=99
CS.TestStruct.name="Unity"
print(CS.TestStruct.Output())

--实例化结构体进行调用方法，和类的调用是一样的
local obj = CS.TestStruct()
obj.Gender = "Famale"
print(obj:Talk())
```

### **Lua使用C#枚举**

```csharp
//枚举
public enum TestEnum
{
    Male,
    Fimale
}
```

- 使用`CS.枚举名.枚举值`访问到枚举，但是这列代码的数据类型是`userdata`
  - `CS.枚举名.__CastFrom(枚举值)`，该枚举值可以是枚举本身的值或者是字符串
- 因为`lua`中没有与`enum`对应的数据类型，所以将其放入`userdata`这种数据类型中，表示其是一个来自于母体语言的数据类型

```lua
--lua中没有与enum对应的数据类型，所以将其放入userdata这种数据类型中
--表示其是一个来自于母体语言的数据类型
print(CS.TestEnum.Male)

--其他获取枚举的方式
print(CS.TestEnum.__CastFrom(1))
print(CS.TestEnum.__CastFrom("Male"))
```

### **Lua使用C#泛型**

```csharp
//泛型
public class TestGenericType
{
    public void Output<T>(T data)
    {
        Debug.Log("泛型方法：" + data);
    }
    public void Output(string data)
    {
        Output<string>(data);
    }
}
```

- `lua`是不支持泛型的，但是我们添加组件的时候那些方法几乎都是泛型，为了解决这个问题，`Unity`中是有一些重载函数的，所以我们可以使用`AddComponent`的重载函数在`lua`添加组件
- lua中`typeof`函数是`xLua`帮我们定义的和`C#`中`typeof()`功能一样的函数

```lua
local obj = CS.TestGenericType()
obj:Output("Unity")

--创建一个物体在场景上
local go = CS.UnityEngine.GameObject("物体名称")
--typeof函数是xLua帮我们定义的和C#中typeof()功能一样的函数
--AddComponent因为有重载，所以我们可以使用AddComponent(Type t)方式添加组件
go:AddComponent(typeof(CS.UnityEngine.BoxCollider));
```

### **Lua的多返回值**

```csharp
//多返回值
public class TestOutRef
{
    public static string Func1()
    {
        return "Func1";
    }
    public static string Func2(out string r2)
    {
        r2 = "Func2 Out";
        return "Func2";
    }
}
```

- `C#`中是没有多返回值的，但是`Lua`有，所以如何解决这个问题，就使用`out`关键字即可

```lua
print(CS.TestOutRef.Func1())
--return是第一个返回值，out是第二及n个返回值
--使用out在lua这边是不需要给函数传参的
local s1,s2 = CS.TestOutRef.Func2()
print(s1,s2)
```

- `ref`和`out`，在`xLua`中是一样的

### **Lua使用C#委托**

```csharp
//C#中的委托
public delegate void TestDelegate();

public class TestDelegateClass
{
    public static TestDelegate Static;
    public TestDelegate Dynamic;

    public static void StaticFunc()
    {
        Debug.Log("静态成员方法");
    }

    public void DynamicFunc()
    {
        Debug.Log("对象成员方法");
    }
}
```

- `Lua`去使用委托，注册方法不仅可以注册`C#`代码中的方法，还可以注册`Lua`脚本中的函数

```lua
--静态方法的注册，跟C#中差不多
CS.TestDelegateClass.Static=CS.TestDelegateClass.StaticFunc
--使用委托也是和C#一样
CS.TestDelegateClass.Static()
--但是静态方法是和程序同生共死的，但是Lua解析器会释放，如果在解析器释放前不把委托置空，就会报错，解析器释放失败
--应该在解析器释放前，把静态的委托变量中的委托进行置空
CS.TestDelegateClass.Static=nil

local callback = function (  )
	-- body
	print("lua中的回调函数")
end
--添加Lua自己的函数
CS.TestDelegateClass.Static=callback
CS.TestDelegateClass.Static()
CS.TestDelegateClass.Static=nil
--注册多个函数
CS.TestDelegateClass.Static=CS.TestDelegateClass.Static+callback
```

- 如何判断使用加号还是等号，在注册前进行判空处理
- 静态委托和非静态委托的注册是一样的，但是非静态委托是不需要进行置空处理的
- 因为静态委托变量需要手动释放，非静态的会有C#自动帮你释放

------

## C#与Lua代码的互相调用

### **C#使用Lua的变量**

- `lua`中局部变量只能在`lua`中使用，所以C#调用的时候需要使用全局变量
- `xLua`的解析器`LuaEnv`中封装了一个变量`LuaTable _G`，存放了`lua`中的所有全局变量
  - `LuaTable `是一个封装的用来与`lua`对照变量的一个数据类型

```lua
id=1
name="Unity"
isMale=true
pi=3.14
```

- 封装一个取`Global`的方法

```csharp
public LuaTable Global
{
	get
	{
		//lua的解析器
		return luaEnv.Global;
	}
}
```

- 通过`Global`去取得`lua`中的全局变量

```csharp
//加载出文件
LuaEnvSingleCase.Instance.DoString("require 'LuaCallCsharp/LuaCallClassExtend'");
//通过Global访问全局变量
int id = LuaEnvSingleCase.Instance.Global.Get<int>("id");
string name = LuaEnvSingleCase.Instance.Global.Get<string>("name");
bool isMale = LuaEnvSingleCase.Instance.Global.Get<bool>("isMale");
float pi = LuaEnvSingleCase.Instance.Global.Get<float>("pi");
```

### **C#使用Lua的函数**

- 要在C#中使用`lua`函数，就要在C#代码中有个类型与之对应，才可以产生映射关系

```lua
--无返回值不带参
func1=function()
	print("这是Lua中的函数1")
end

--无返回值带参
func2=function(name)
	print("这是Lua中的函数2,参数是："..name)
end

--单返回值
func3=function()
	return "这是Lua中的函数3"
end

--多返回值
func4=function()
	return "这是Lua中的函数4",99
end
```

- Lua函数的映射应该使用委托

```csharp
//加载出文件
LuaEnvSingleCase.Instance.DoString("require 'LuaCallCsharp/LuaCallClassExtend'");
//函数都是定义在全局变量中，所以可以使用Global获得lua的全局变量(变量内部是函数结构)
Func1 func1 = LuaEnvSingleCase.Instance.Global.Get<Func1>("func1");
Func2 func2 = LuaEnvSingleCase.Instance.Global.Get<Func2>("func2");
Func3 func3 = LuaEnvSingleCase.Instance.Global.Get<Func3>("func3");
Func4 func4 = LuaEnvSingleCase.Instance.Global.Get<Func4>("func4");
//使用
func1();
func2("Unity");
Debug.Log(func3());

string name;
int id;
func4(out name, out id);
Debug.Log(name);
Debug.Log(id);
//如果释放放在这里，func1~func4的局部变量还没有被释放，释放Lua运行环境就会报错
LuaEnvSingleCase.Instance.Dispose();
//如果就想在start里释放解析器，就要手动的将func1~func4变量置空
func1 = null;
```

- 如果要释放解析器，局部变量委托就要先释放才可以释放解析器，否则就会报错
  - 第一种方法将委托变量置空
  - 第二张方法将释放方法放在另一个函数里，当这些执行完了，垃圾回收启动后再去调用这个释放方法

### **C#使用Lua的表**

- `lua`中最中要的就是表类型

```lua
Core={
	ID=99,
	Name="Unity",

	Func1=function()
		print("这是Lua中的Core表的func1方法")
	end,

	Func2=function(name)
		print("这是Lua中的Core表的func2方法，参数是："..name)
	end,


	Func3=function(self)
		print("这是Lua中的Core表的func3方法，成员变量Name是："..self.Name)
	end,
}

function Core:Func4()
	print("这是Lua中的Core表的func4方法，成员变量Name是："..self.Name)
end
```

- C#中使用`Lua`的表有三种方法，一种是导出`LuaTable`类型，一种是映射到接口类型，最后一种是映射到类/结构体类型
- 导出`LuaTable`类型

```csharp
//Lua中有个隐式传递self，所以委托中必须要有能传递表类型的参数，LuaTable
public delegate void Func3Self(LuaTable self);

 public void ToLuaTable()
 {
     //Lua中的Table
     LuaTable core = LuaEnvSingleCase.Instance.Global.Get<LuaTable>("Core");
     Debug.Log(core.Get<int>("ID"));
     core.Set<string, string>("Name", "Unreal");
     Debug.Log(core.Get<string>("Name"));

     Func1 func1 = core.Get<Func1>("Func1");
     Func2 func2 = core.Get<Func2>("func2");
     func2("Unity");

     //因为有隐式self参数，所以必须将LuaTable再传会Lua中
     Func3Self func3 = core.Get<Func3Self>("Func3");
     Func3Self func4 = core.Get<Func3Self>("Func4");
     func3(core);
     func4(core);
 }
```

- 但是这种方法性能较弱，因为`LuaTable`是通用数据类型，所以使用的时候需要进行一些转换，性能降低
- 类/结构体要和`Lua`表进行一对一的映射，即属性必须是`public`，且名称类型要对应
- 导出结构体可以避免再`xLua`中产生`GC`，从而影响性能

```csharp
//添加这个特性，可以让Lua中的table，导出结构体的时候不产生垃圾回收GC
[GCOptimize]
public struct TableCore
{
    [CSharpCallLua]
    public delegate void Func3Self(TableCore self);

    public int ID;
    public string Name;
    public bool IsMale;

    public Func1 func1;
    public Func2 func2;
    public Func3Self func3;
    public Func3Self func4;
}


//转换的方法
//导出结构体可以避免再xLua中产生GC，从而影响性能
public void ToStruct()
{
    TableCore core = LuaEnvSingleCase.Instance.Global.Get<TableCore>("Core");
    Debug.Log(core.ID);
    Debug.Log(core.Name);

    core.func1();
    core.func2("Unity");
    //因为有隐式self传参，所以必须将结构体结构再传会Lua中
    core.func3(core);
    core.func4(core);
}
```

- 最后一种就是导出一个接口类型
- 定义一个接口，需与`lua`表中的**元素名称，类型一样** (数量可以不一致)
- 这个接口必须是**public**的，且需要在上面加上**[CSharpCallLua]** ,并且**要在Unity中重新生成xLua代码** 

```csharp
[CSharoCallLua]
public interface TableCore
{
    [CSharpCallLua]
    public delegate void Func3Self(TableCore self);

    int ID {get;set};
    string Name {get;set};
    bool IsMale {get;set};

    Func1 func1 {get;set};
   	Func2 func2 {get;set};
    Func3Self func3 {get;set};
    Func3Self func4 {get;set};
}

public void ToStruct()
{
    TableCore core = LuaEnvSingleCase.Instance.Global.Get<TableCore>("Core");
}
```

- 这种方法是引用传递，在C#中修改值，lua中也会被修改