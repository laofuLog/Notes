# TCP长链接

![TCP-IP模型](https://s1.ax1x.com/2020/04/24/J0XVQU.md.png)

- 网络层IP协议，找到需要链接的IP地址

- TCP/UDP协议一般处于传输层，用于数据传递

- Http协议处于应用层

- TCP和UDP区别

  ![TCP和UDP](https://s1.ax1x.com/2020/04/24/J0XCon.jpg)

## Socket套接字

- Socket套接字：[https://baike.baidu.com/item/%E5%A5%97%E6%8E%A5%E5%AD%97/9637606](https://baike.baidu.com/item/套接字/9637606)

- IP协议实现主机的网络定位，就是一个地方的地址
- 操作系统的端口实现数据的流入与流出，这个地址的门
- 套接字：是将IP地址与主机端口号合并在一起后的数据，IP地址定位主机位置，端口号知道通讯入口与出口，从而就可以实现主机的数据交换
  - 例如一个主机IP地址是$10:28:213:186$，开放端口$8080$，则套接字就是：$10:28:213:186:8080$

- Socket编程基于传输层实现，所以需要指定协议类型（TCP或UDP）

## TCP编程方法

- 字节长度之间的换算

- 计算机只能识别$1/0$，同理也只能存储$1/0$
- 一个二进制单位叫做位（ `bit `）
- `bit`就是计算机最小得到储存单位

| 单位    | 换算       |
| ------- | ---------- |
| `1Byte` | `8Bit`     |
| `1KB`   | `1024Byte` |
| `1MB`   | `1024KB`   |
| `1GB`   | `1024MB`   |
| `1TB`   | `1024GB`   |

- UTF-8编码长度是从1个字节~6个字节存储，其中中文是3个字节
- 查询API：<https://msdn.microsoft.com/zh-cn/>，查询Socket的API或者其他语言API都可以在这个手册中找一下

### **线程**

- Unity中是存在线程的，但是线程中不能对场景中的对象进行任何操作，只有主线程可以操作场景对象

```csharp
//调用线程命名空间
using System.Threading;
using UnityEngine;

/// <summary>
/// 测试线程
/// </summary>
public class TestThread : MonoBehaviour
{
    void Start()
    {
        //创建线程，会在独立的CPU内核运行
        Thread t = new Thread(InThread);
        //让线程开始执行
        t.Start();
    }

    void InThread()
    {
        int i = 0;

        while(i < 10)
        {
            Debug.Log("这是分线程");
            i++;
        }

        //Unity中是存在线程的，但是线程中不能对场景中的对象进行任何操作，只有主线程可以操作场景对象
        //new GameObject("分线程运行结束");
        TestFunc();//即使是这样回调，在外面去创建一个对象，也会报错，因为这个函数是被线程引用的

        //函数运行退出时，线程将退出
    }

    void TestFunc()
    {
        new GameObject("分线程运行结束");
    }
}
```

### **链接（三次握手）**

![三次握手](https://s1.ax1x.com/2020/04/24/J0OzLQ.md.png)

- 链接可以写在脚本的`Start`函数中

```csharp
//同步
//创建套接字
//创建了一个套接字，参数：网络类型，基于数据流方式，基于TCP协议
Socket socket = new Socket(AddressFamily.InterNetwork,SocketType.Stream,ProtocolType.Tcp);
//调用连接方法,与远程主机建立连接。主机由主机名和端口号指定。
socket.Connect("IP地址", 端口号)
-------------------------------------------------------------------
//异步连接
//创建套接字
Socket socket = new Socket(AddressFamily.InterNetwork,SocketType.Stream,ProtocolType.Tcp);
//参数：主机地址、端口、链接成功后的回调函数、其他参数
socket.BeginConnect(Host, Port, _EndConnect, null);



//_EndConnect回调函数中执行
void _EndConnect(IAsyncResult ar)
{
    socket.EndConnect(ar异步连接结果);
}
--------------------------------------------------------------------
```

- 异步链接就不会占用主线程，万一链接时间过长就不会卡掉程序

### **断开（四次挥手）**

![四次挥手](https://s1.ax1x.com/2020/04/24/J0X9ds.md.png)

- 断开可以写在脚本的`OnDestroy`周期函数中

```csharp
//同步
//下次使用，会创建全新的套接字
socket.Disconnect(false);
//关闭套接字连接，释放资源
socket.Close();
-------------------------------------------------------------------
//异步断开
socket.BeginDisconnect(false, _EndDisconnect, null);

// EndDisconnect回调函数执行
void _EndDisconnect(IAsyncResult ar)
{
    socket.EndDisconnect(ar异步断开连接结果);//有BeginConnect就需要有EndDis
	socket.Close();
}
-------------------------------------------------------------------
```

### **发送**

```csharp
//创建套接字
_Socket = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
//链接主机与端口
_Socket.Connect("hxsd.tcp.honorzhao.com", 8282);

string data = "分手";
//字符串，转字节数组
//Encoding类，根据字符串的字符集，自动将其转换为字节数组
byte[] datab = System.Text.Encoding.UTF8.GetBytes(data);

//返回值是发送到网卡的数据字节长度
//Send是阻塞的，在主线程中运行，如果数据过大，则游戏会假死
//参数：要发送的字节数组，发送的起始下标，终止下标，发送的类型
int length = _Socket.Send(datab, 0, datab.Length, SocketFlags.None);

Debug.Log("向服务器发送了长度：" + length + "的数据");
```

### **接收**

```csharp
//这里开了一个1M的缓存区
private byte[] _Buffer = new byte[1024 * 1024];//开一个字节缓存区
//创建套接字
_Socket = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
_Socket.Connect("hxsd.tcp.honorzhao.com", 8282);

//参数：
//接收缓冲区
//数据在网卡中的接收起始偏移量
//当次接收的数据长度（缓冲区是1MB，所以接收数据的长度也是1MB）
//不对接收数据的来源进行指定（接收所有的数据）
//返回值：
//返回值是从服务器接收到的数据的长度
//代码会阻塞主线程，直到服务器给客户端发送了数据，接收的阻塞才会结束
int length = _Socket.Receive(_Buffer, 0, _Buffer.Length, SocketFlags.None);
Debug.Log("从服务器接收到了长度：" + length + "的数据");

 byte[] data = new byte[length];
//遍历接收缓存区中的数据进行数组拷贝
for (int i = 0; i < length; i++)
{
    data[i] = _Buffer[i];
}
//将二进制数据编码回字符串变成人可以看懂得数据类型
string finalData = System.Text.Encoding.UTF8.GetString(data);
```

### **lock关键字**

- 锁（`lock`）：当一个线程，锁住变量后，另一个线程，必须等待锁释放，才能继续操作变量，这样就能防止并行执行代码时，出现内存数据的错误（读写会同时执行）

## 数据包处理

### **字节序**

- 字节序，针对数字，字符串是没有什么顺序问题的

![字节序](https://s1.ax1x.com/2020/04/24/J0XeL4.jpg)

- 大端字节序：数据的前位字节，放在内存低地址位，后位字节，放在高内存地址位，符合人类的思维方式
- 小端字节序：将数字的后位字节，放在内存栈的低地址位，前位字节放在高地址位，不符合人类思维方式，但是计算机处理这样的内存更快
- 主机字节序：当前计算机，数字的字节表示方式，与硬件和操作系统有关
- 网络字节序：无论主机是怎样的字节序，互联网规定，传递数据时，都应该转换为大端字节序进行传递。

### **数据包定制**

- 包头：记录有关于整个数据包的信息，需要加密
- 包体：原始数据，需要加密，防止泄露，RC4方式

![网络通讯](https://s1.ax1x.com/2020/04/24/J0XZyF.md.jpg)

- 数据打包：对原始数据，添加协议头的过程，就是数据打包的过程（前四个字节记录数据包总长度，后面拼接包体内容，成为一个数据包）
  - 发送一个“分手”字符串的时候，打包为（10分手）
- 数据解包：接收到数据包时，读取包头，并根据包头记录信息，获取到包内的原始数据的过程，就是解包
  - 例如包头得到是10，得到这个数据长度为10个字节，去掉包头4个字节，得到要接收的字符串是6个字节
- 数据粘包：发送数据前，如果有多个数据包需要一起发送，则可以将数据包，拼接在一起，再发送，这样的发送效率更高
  - 数据粘包有时是硬件将数据包拼接在一起，有时是代码将数据包拼接在一起

- 数据分包：当接到数据后，需要将每一个定制的数据包按格式分离出来，所写的代码就是分包代码

### **数据包的生命周期**

- 对数据打包
- 对多个数据粘包
- 套接字：链接、发送
- 套接字：接收
- 对数据包进行分包
- 对分包后的数据包进行解包
- 获得数据后，根据数据进行其他代码逻辑

### **心跳包**

- 因为TCP是有连接的，所以必须在两个PC间，建立连接。但是如果长时间连接，却又不发送数据，则会占用互联网络的通信信道，就有可能被网络的中间设备（路由器，防火墙）将网络连接断开，所以为了防止网络被断开，则需要两台计算机间，定期发送一些数据，这样的数据就是心跳数据（心跳）。
- 常说的网络延迟计算：服务器返回心跳时间 - 客户端发送心跳时间（ms）

