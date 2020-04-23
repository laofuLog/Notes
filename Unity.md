[TOC]

# Unity的基本信息

## 各个面板
- `Scene`场景面板
    - Y轴正方向相对屏幕向上
    - X轴正方向相对屏幕向右
    - Z轴正方向相对屏幕向右
- `Game`游戏视窗面板
    - 分辨率
    - `stats`：场景中的各信息
- `Project`工程面板
- `Inspector`组件、属性面板
- `Hierarchy`层级面板
- `Console`控制台面板
    - `Clear on Play`：运行之前清除之前的信息
    - `Error Pause`：遇到错误信息就暂停游戏运行

## 脚本
- 脚本文件名必须要和类名一致

- `Start();`：游戏运行时该方法只会执行一次

- `Update();`：游戏运行时每一帧都会执行一次

  > 写之前清除自己写的代码是需要多次执行还是只需要执行一次

- 组件必须继承`MonoBehavior`类才能挂载

  - 继承`Monobehavior`的类可以当成游戏物体组件挂载到游戏物体上
  - 继承`MonoBehavior`的类挂载到游戏物体上，游戏物体是激活状态[^1]就会执行代码

[^1]: 游戏物体是显示状态

## 预制体

- 预制体的`Prefab`的三个按钮功能
  - `Select` 找到预制体本体
  - `Revert `对预制体修改后可以重置这个修改过的预制体
  - `Apply` 对预制体的修改应用到相对的其他预制体身上
- 取消预制体关系：菜单栏-->`GameObject`-->`Break Prefab Instance`

## 坐标系

- 世界坐标：Unity本身的世界坐标
- 本地坐标：相对父物体的坐标，若无父对象则世界坐标就是父对象的坐标

## 快捷键

- Ctrl+Shift+F：快速调整摄像机视角按当前你的游戏视窗视角
- Ctrl+D：快速复制物体在本目录
- Ctrl：按住Ctrl在场景中可以按一个单位位移

# 常用几个API

## transform状态

### **属性**

- `position`：字段赋值是`Vector3`类型的，针对的是世界坐标系的位置

- `localPosition`：字段可赋值，针对的是本地坐标系的位置

- `parent`：父物体字段，可以赋值就可以设置父物体

- `chileCount`：子物体的个数

- `losalScale`：相对本地坐标进行缩放

- `lossyScale`：相对世界坐标进行缩放，但是只能得到不能更改

- `forward`：本地坐标的z轴，且值是相对于世界坐标的方向值

  > 还有其他方向的值，可以F12查看其他方向
### **方法**
- `Translate(Vector3)`：做位置增量，传进的是向量[^2]

  [2]:有方向有长度

  - 重载函数`Translate(Vector3,transform)`：除了`Vector3`类型的，还可以传进一个物体的`transform`，然后就会以这个物体的坐标系做位置增量

- `RotateAround(Vector3,Vector3,float)`：围绕着某个点选择，传进的是一个点的坐标，一个方向和转的速度值

- `Rotate(Vecotr3,speed);`:使物体朝某个方向旋转`speed`角度

  - `Rotate(Vecotr3(0,1,0),10);`:向y轴方向旋转10度

- `Rotate(Vecotr3);`：使物体在原始角度上朝这个方向旋转多少角度，传进的是向量

  - `Rotate(Vecotr3(0,10,0));`：朝y轴旋转10度

- `Rotate(Vecotr3,Space.Self/World);`：使物体基于自己/世界的坐标系旋转多少角度

- `SetParent(Transform)`：通过方法设置父物体，传进的是父物体的`transform`[^3]信息

  [^3]:大小、位置、方向

- `GetGhild(int)`：获取子物体，类似列表的索引

- `LookAt(Vector3)`：物体的z轴始终指向传进游戏物体的坐标，传进的是这个点的坐标

- `Find(string)`：传进游戏物体的名字（字符串），找到一个子物体（只能是子物体，子物体的子物体就无法找到），即使游戏物体隐藏也可以找到该物体

## Debug控制台输出
### **方法**
  - `Debug.Log()`输出
  - `Debug.LogError()` 错误输出
  - `Debug.LogWarning()`警告输出
  - `Debug.DrawLine(Vector3,Vector3,color)`：在Unity中画线，传 一个起始点一个终点，就可以在场景中画出这条线来
## GameObject游戏物体
### **属性**
- `name`：当前游戏物体的名字，可以修改
### **方法**
  - `Instantiate<T>(T)`：克隆，可以克隆游戏物体，设置泛型参数类型为`GameObject`即可
  - `Find(Sting)`：传进游戏物体的名字，整个场景中找一个游戏物体，返回该游戏物体，但是游戏物体如果是隐藏状态就无法找到，且查找整个场景的物体消耗运行效率
  - `FindGameObjectWithTag(string)`：传进游戏物体的标签，整个场景中找一个游戏物体，且非激活状态无法找到
  - `SetActive(bool)`：传进一个布尔值可以控制一个游戏物体的激活状态，`true`为激活，`false`为非激活
  - `Destory(GameObject)`：销毁一个游戏物体
      - 重载函数`Destory(GameObject，Time)`：延迟多少时间销毁这个物体

### **递归查找子物体**

```C#
//第一种方法
GameObject SetChild(GameObject parent,string childName)
{
    if(parent.name==childName)
        return parent;
    if(parent.transform.childCount<1)
        return null;
    GameObject go=null;
    for (int i = 0; i < parent.transform.childCount; i++)
    {
        go=SetChild(parent.transform.GetChild(i).gameObject,childName);
        if(go!=null)
            break;
    }
    return go;
}
//第二种方法	
GameObject SetChild2(GameObject parent,string childName)
{
    GameObject go=new GameObject();
    if(parent.transform.childCount<1)
        return null;
    if(parent.transform.Find(childName)!=null)
        return parent.transform.Find(childName).gameObject;
    for (int i = 0; i < parent.transform.childCount; i++)
    {
        go=SetChild2(parent.transform.GetChild(i).gameObject,childName);
        if(go!=null)
            break;
    }
    return go;
}
```



## Vector3方向、坐标、向量

- `Vector3`类型可以作为方向、坐标、向量[^2]来使用

- `forward`：本地坐标的z轴，(0,0,1)

- `back`：本地坐标的z轴反方向，(0,0,-1)

  > 还有其他方向的值可以VS中查看F12

## Input输入控制

### **键盘Key**

- `GetKeyDown(KeyCode)`：按键按下瞬间执行一次，返回布尔值，传入的是按键键值
  - `Input.GetKeyDown(KeyCode.A)`：判断是否输入了A键
- `GetKey(KeyCode)`：按键按下期间一直执行，不放键盘就会一直执行
- `GetKeyUp(KeyCode)`：按键弹起瞬间执行一次

### **鼠标Mouse**

- `GetMouseButton(int)`：检测鼠标的键位，返回的布尔值，按下鼠标的时候一直执行
  - `int=0`：鼠标左键
  - `int=1`：鼠标右键
  - `int=2`：鼠标中键
- `GetMouseButtonDow(int)`：检测鼠标的键位，按下瞬间执行一次
- `GetMouseUp(int)`：检测鼠标键位，键位弹起瞬间执行一次

### **GetAxis获取轴**

- ` Input`设置热键：菜单栏`Edit`-->`Project Settings`-->`Input`
  - 可以设置对应轴的名称，和按键
- `GetAxis(string)`：传入的是`Input`设置中轴的名称，返回的是一个`float`类型的值，根据这个`float`值，可以搭配`Translate`、`Rotate`进行左右旋转前后移动
  - `“Horizontal”`：对应水平方向，对应键盘A、D键，对应游戏手柄的左右
  - `“Vertical”`：对应竖直方向，对应键盘W、S键，对应游戏手柄的上下
  - `“Mouse X”`：对应鼠标的左滑右滑

## Random

- `Random.Range`：随机数方法，和控制台中的`Random`用法类似

## Resources加载游戏资源

- 从硬盘中将资源加载到游戏中
- 需要建一个**Resources**文件夹
- `Resources.Load<T>(Stirng)`：传入一个**Resources**文件夹下游戏物体的路径，返回的就是这个实例化后的物体
  - 例如找一个**Resources**下**Prefab**文件夹的`GameObject`类型的`Bullet`预设体
  - `Resources.Load<GameObject>("Prefab/Bullet");`
  - 加载这个文件夹下的所有文件，且按命名顺序`Resources.LoadAll<GameObject>("Prefab/Bullet/");`

## Time时间类

- `time`：游戏开始运行时这个时间就从0开始计算，单位秒
- `deltaTime`：上一帧到这一帧结束的单位时间，其实就是每帧所用的时间
- `fixedDeltaTime`：固定的时间，且该时间是每帧的运行时间
- `timeScale`：对时间进行缩放，可以赋值，赋值的就是要放大缩小的倍数数值，大于1时间加快，小于1时间变慢，等于0时间暂停
- `unScaleTime`：不接受任何时间的暂停，即使游戏暂停运行它也在继续计算时间
- `frameCount`：当前游戏跑了多少帧

## Mathf数学类

- `Clamp(float,float,float)`：限制大小，第一个参数只能在第二个和第三个之间，当小于第二个参数的时候等于它，当大于第三个参数的时候就等于它，可以用来设置血量大小等等
- `Lerp(float a,float b,float c)`：插值运算，第一个参数是起始值，第二个是终点值，第三个是系数，计算公式-->`a+(b-a)*c`

## PlayerPrefs存储数据

- `SetFloat(string,float)`：类似字典，存储一个==标识--值内容==，将内容存在本地数据，随后可以在其他的脚本中也可以读取到这个数据，即使重新运行游戏还是会读取到这个数据
  - `SetInt()`：存整型
  - `SetString()`：存字符型
- `GetFloat(string)`：通过键去寻找这个值内容，获得到这个值
  - `GetInt()`
  - `GetString()`
  - 重载函数`GetFloat(string，int)`：int是一个默认值。当没有这个东西的时候，就返回的是默认值
- `DeleteAll()`：删除所有数据
- `DeleteKey(string)`：删除对应标识的数据
- 可以做一个排行榜

## 场景切换

- `SceneManager.LoadScene("场景名")`：需要引入命名空间`UnityEngine.SceneManagement`

## 关于Application的一些API

- `Application.targetFrameRate = int`：限制游戏的帧率
- `Application.Quit()`：退出游戏

# 生命周期

## 生命周期函数

**按执行顺序排序**

- `Awake()`：只有在游戏物体唤醒的时候只执行一次，即被创建的时候执行一次，该函数不一定是最开始执行的，当这个绑定的物体再被创建的时候就会又执行一次
- `OnEnable()`：游戏物体每激活[^4]一次就会执行一次
- `Start()`：游戏运行时调用一次
- `FixedUpdate`：固定步长更新，即一定时间内执行一次
  - 一般物理相关更新写在这里
  - 设置步长在菜单栏`Edit`—>`Project Settings`—>`Time`
- `Update()`：游戏运行时每一帧执行一次
- `LateUpdate()`：每一帧都执行，但是是在`Upadate`后执行
  - 一般写摄像机跟随
- `OnDesable()`：游戏物体每失活[^5]一次就会执行一次
- `OnDestroy()`：销毁之前调用的函数，销毁物体之前会调用一次`OnDesable`

[^4]:游戏物体的激活即显示状态，可在游戏物体组件上勾选隐藏或非隐藏，也可以通过方法设置，游戏物体生成的时候也叫激活状态
[^5]:游戏物体的非激活即隐藏状态，游戏物体销毁之后也是叫失活状态

# 基础组件操作

## Light灯光组件

- `Type`：灯光类型
  - `Directional`直射光(类似太阳光)
  - `Point`点光源(类似灯泡)
  - `Spot`聚光灯
  - `Area`区域光，常是烘焙使用
- `Color`：灯光颜色
- `Mode`：灯光烘焙模式
  - `Realtime`实时光
  - `Mixed`混合光
  - `Baked`烘焙光
- `Instensity`：光照强度
- `Indirect Multiplier`：光照乘数
  - 间接光从另一个游戏物体反射到另一个游戏物体的光线，小于1时每次反射会变暗
- `ShadowType`：阴影类型
  - `Soft Shadows`平滑阴影
  - `HardShadow`硬阴影
  - `NoShadows`没有阴影
- `BakeShadowAngle`：人为增加阴影平滑度，让阴影更光滑
- `Strength`：阴影强度
- `Resolution`：阴影质量，质量越高，消耗越大
- `Bias`：阴影推移距离
- `Normal Bias`：光阴表面推移距离
- `NearPlane`：（在点光源和聚光灯下起作用）影子的近裁剪面
- `Cookie`：投射阴影的指定纹理
  - `cookiesize`纹理大小
- `Draw Halo`：光晕开启
- `Flare`：光耀（只有电光源和聚光灯下起作用）赋值光耀文件
- `Culling Mask`：裁剪层（二进制标识）

## 组件获取与添加

### **组件的获取**

- 组件控制的步骤

  1.找到游戏物体

  2.获取到组件控制权

  3.控制组件

**例如获得灯光的light组件，并控制它**

- 找到游戏物体
  - `go = GameObject.Find(“Directional Light”)`
- 获取组件控制权
  - 方法`GetComponent<>()`
  - `object = go.GetComponent<Light>()`
- 控制组件
  - 此时就可以用`object`去点出该组件上的各个变量
  - `object.type = LightType.Point`将灯光状态改成点光源

### **组件添加**

- `AddComponent()`：添加一个组件

### **GameObject和Transform的关系**

- `GameObject`必须有`transform`组件，所以`GameObject`内置了属性包含了自己的`transform`
  - 所以所有游戏物体都可以点出`transform`属性
- `transform`不能独立于游戏物体而存在，且任何组件都不能独立于游戏物体存在
  - 所以所有组件都可以点出`gameObject`

## 二进制标识

- 以`Light`组件的`Cullling Mark`为例，每个选项都代表一个二进制数值，当在代码选择这些数值的时候就会选择对应的选项，游戏中的应用常有buff的叠加等
  - 例如红BUFF的二进制标识是`00000001`，蓝BUFF的二进制标识是`00000010`，当同时获得这两个BUFF的时候，就会获得数值`00000011`，以此来判断角色身上有多少个状态，这样就不需要用枚举来定义状态，只需要`int`类型的数值就可以来存储这个状态

## Audio Source声音组件

### **属性**

- `Audio.Clip`：音效片段
- `PlayOnAwake`：一开始就播放
- `Loop`：循环与否
- `Volume`：音量大小

- `mute：true`开启静音,false关闭静音

### **方法**

- Play()：开启声音播放

### **Audio Clip**

- 声音片段，赋值给声音组件就可以播放对应片段

# OnGUI老版UI系统

- Unity最原始的一版UI系统
- 优点是比较轻量级，缺点是操作不够方便
- 可以显示文字、图片、按钮等元素
- 全部是写在方法`OnGUI`中

### GUI类

- `Label()`：显示一个文字框，有以下常用重载函数
  - `Label(Rect,string)`：第一个传进的是一个`Rect`参数，`Rect`代表一个矩形框，可以设置位置，大小，第二个传的是显示的文字
    - `Rect(Vector2,Vector2)`：第一个设置的是在场景上的坐标，第二个是大小
    - `Rect(float,float,float,float)`：设置位置x,y，大小l,w
  - `Label(Rect,string,GUIStyle)`：`GUIStyle`可以设置字的字体颜色大小等风格
- `Button()`：显示一个按钮，有以下常用重载函数
  - `Button(Rect,string)`：传进一个矩形框的位置大小，传进按钮上的文字
  - `Button(Rect,string,GUIStyle)`：可以设置风格
  - `Button(Rect,Texture)`：可以设置按钮的图片
  - `Button(Rect,GUIContent)`：`GUIContent`可以设置图片，文字加图片，文字加提醒信息，文字加图片加提醒信息
- `HorizontalScrollbar（Rect position，float value，float size，float leftValue，float rightValue）`：左右的滚动条，需要的参数滚动条的位置、最小和最大之间的位置、滑块的尺寸、滚动条左端的值、滚动条右端的值，还可以传一个`Style`，增加滚动条背景，这个滚动条类似浏览器下方的那种滑动条
- ` HorizontalSlider`：这个滚动条传的参数与上述一样，少了个尺寸，但这个样式就类似调节音量的
- `GrawTexture()`：画出这个图片

# 碰撞

## Rigibody刚体和Collider碰撞体

### **参数**

- `Mass`:质量
- `Drag`：摩擦力
- `Angular Drag`:转弯阻力
- `Use Gravity`:是否使用重力
- `Is Kinematic`:运动学刚体，当两个刚体发生碰撞，它不会发生位移
- `Collision Detection`：检测精度
  - `Discrete`：离散性监测
  - `Continuous`：连续性监测
  - `Continuous Dynamic`：连续动态监测
- `Constraints`：
  - `Freeze Position`：锁轴位移
  - `Freeze Rotation`：锁轴旋转                   
- `Interpolate`:如果刚体抖动，none：没有插值运算
- `Interpolate`：根据上一帧位置做插值运算
- `Extrapolate`：根据上一帧运动预测下一帧的插值

### **发生碰撞的必要条件**

- 发生碰撞的两个游戏物体都必须要有碰撞体`Collider`，有一方游戏物体有刚体`Rigidbody`就可以发生碰撞了（一般把刚体绑定绑在运动的游戏物体上）
- 两个游戏物体的碰撞器`Is Trigger`是关闭的

### **检测碰撞OnCollision**

- `ONCollisionEnter(Collision)`：两个游戏物体==碰撞瞬间==调用这个函数，要传进的是对方的碰撞体
- `ONCollisionStay(Collision)`：两个游戏物体==碰撞期间==调用这个函数，要传进的是对方的碰撞体
- `ONCollisionExit(Collision)`：两个游戏物体碰撞==弹开瞬间==调用这个函数，要传进的是对方的碰撞体
- 即使当前脚本组件失活，也会执行这三个方法

### **发生触发的必要条件**

- 发生碰撞出发的两个游戏物体都必须要有碰撞体（`Collider`），有一方游戏物体有刚体（`Rigidbody`）就可以发生碰撞触发了（一般是把刚体绑定在运动的物体上）
- 两个游戏物体有一个游戏物体的碰撞器和`Is Trigger`是开启的

## 触发器OnTrigger

- `ONTriggerEnter(Collider)`：两个游戏物体==触发瞬间==调用这个函数
- `ONTriggerStay(Collider)`：两个游戏物体==触发期间==调用这个函数
- `ONTriggerExit(Collider)`：两个游戏物体==触发后==调用这个函数

### **触发器和碰撞器的区别和联系**

**相同点：**

- 两个发生碰撞的游戏物体必须都有`Collider`，至少有一个游戏物体带刚体组件
- 代码组件失活了同样还会检测
- 检测碰撞代码无论写在两个发生碰撞的游戏物体的哪一方都会检测到

**不同点**

- 碰撞器不能开启`collider`的`Is Trigger`选项，触发器必须要有一方开启`Is Trigger`选项
- 触发函数的参数不同

# 缓存池

## 单个缓存池

1. 先在池子里看有没有游戏对象
2. 如果有就从池子拿出来用，没有就`Instantiate`出游戏对象
3. 游戏对象用完之后不进行销毁，将游戏对象回收到池子里，重复利用

> 使用发射子弹作为例子

```C#
//在玩家类中
{
    public GameObject bulletPool;//子弹缓存池
    
    void Update()
    {
        if (Input.GetMouseButtonDown(0))
        {
            GameObject go = null;
            //1.需要使用子弹的时候现在缓存池查询是否有子弹，如果有直接用
            //将物体设置成池子的子物体，然后进行重复使用就行
            if (bulletPool.transform.childCount > 0)
            {
                //池中有备用子弹
                go = bulletPool.transform.GetChild(0).gameObject;
                //将子物体从父物体拿出来，防止再次被调用
                go.transform.parent = null;
                //物体被放入缓存池时是失活的，所以拿出来的时候要激活
                go.SetActive(true);
            }
            else //2.若是池子里无子弹，就实例化出子弹
            {
                go = Instantiate(bullet);
            }
            //将子弹回收，放入缓存池，这一步需要在子弹类中操作


            //要给物体（子弹）设置位置和方向
            go.transform.position = startPos.transform.position;
            go.transform.rotation = startPos.transform.rotation;
        }
    }
}
//子弹类中
{
    private float timer_BulletPool;//设置一个计时器，规划子弹放入池子的时间
    private GameObject bulletPool;//设置一个标签用来找这个池子
    
    void Update()
    {
        //设置一个计时器
        timer_BulletPool += Time.deltaTime;
        //当时间大于五秒时，将子弹放入缓存池中，重复利用
        if (timer_BulletPool > 5)
        {
            if (bulletPool == null)//如果没有找到缓存池就找一下缓存池
            {
                bulletPool = GameObject.FindGameObjectWithTag("BulletPool");
            }
            //将子弹物体作为缓存池的子物体
            transform.parent = bulletPool.transform;
            //将放入池中的物体失活
            gameObject.SetActive(false);
            //计时器设置为0
            timer_BulletPool = 0;
        }
        
		//让子弹飞
        transform.Translate(Vector3.forward * speed * Time.deltaTime, Space.Self);
        
		//就不需要在去销毁子弹物体，少让系统调用垃圾回收GC，可以提高效率
        //Destroy(this.gameObject, 5);
    }
}
```

- 如何在缓存池中放入不同的游戏物体
  - 设置一个缓存池管理类，在设置一个字典，将每个游戏物体放入对应的标签中，需要什么的时候就取这个标签字典下的游戏物体

## 缓存池存多个物体

```C#
/// <summary>
/// 缓存池 单例模式
/// </summary>
public class ObjectPool
{
    private Dictionary<string, List<GameObject>> pool = new Dictionary<string, List<GameObject>>();
    private static ObjectPool instance;

    public static ObjectPool GetInstance()
    {
        if (instance == null)
            instance = new ObjectPool();
        return instance;
    }

    /// <summary>
    /// 指定缓存池名 得到其中的一个对象
    /// </summary>
    /// <param name="name">缓存池名</param>
    /// <returns></returns>
    public GameObject Pop(string name)
    {

        GameObject obj = null;
        //找下字典当中与没有这个线性表
        if (pool.ContainsKey(name))
        {
            //如果有这个线性表就查询表中有没有游戏物体
            if (pool[name].Count > 0)
            {
                //如果有游戏物体就将其取出来
                obj = pool[name][0];
                obj.SetActive(true);
                //取出来之后要把表中这个数据删除
                pool[name].RemoveAt(0);
                return obj;
            }
        }
        else
        {
            //如果没有这个线性表就构建这个线性表
            pool.Add(name, new List<GameObject>());
        }
        //如果没有这个游戏物体就将其实例化出来
        obj = GameObject.Instantiate<GameObject>(Resources.Load<GameObject>(name));
        return obj;
    }

    /// <summary>
    /// 将使用完的对象 压入对象中
    /// </summary>
    /// <param name="name">缓存池名</param>
    /// <param name="obj">游戏物体</param>
    public void Push(string name, GameObject obj)
    {
        //在对应的线性表中添加这个游戏物体
        pool[name].Add(obj);
        obj.SetActive(false);
    }

    /// <summary>
    /// 清空缓存池
    /// </summary>
    public void Clear()
    {
        pool.Clear();
    }
}
```

- ==过场景时要清空缓存池==

---

# 3D数学

## 三角函数

### **基础公式**

- 勾股定理：$$ a^2+b^2=c^2$$

- 弧度与角度的换算：弧度`rad`，边长`l`，边长`r`

  - $$
    弧度=\frac{边长}{半径}:rad=\frac{l}{r}
    $$



- $1弧 度=\frac{180}{\pi}度:1rad =\frac {180}  {\pi}deg$
- $1\cdot\pi=180deg$
- $1deg=\frac{1}{360}rad$

### **在Unity中的单位换算**

- `Mathf.Rad2Deg`：弧度转角度
- `Mathf.Deg2Rad`：角度转弧度

### **Unity中的三角函数方法**

- `Mathf.Sin(float)`：正弦函数，传入一个弧度`rad`就可以计算正弦值，若是想传角度就可以使用`Mathf`中的单位换算进行换算
  - ` float sinValue = Mathf.Sin(30 * Mathf.Deg2Rad);`：计算`Sin30°`的值
- `Mathf.Cos(float)`：余弦函数，传入一个弧度`rad`就可以计算余弦值，若是想传角度就可以使用`Mathf`中的单位换算进行换算
  - ` float cosValue = Mathf.Cos(30 * Mathf.Deg2Rad);`：计算`Cos30°`的值
- `Mathf.Acos(float)`：反余弦三角函数，传入一个弧度值就可以得出这个夹角，返回的是弧度值，可以换算为角度值
  - ` float includedAngle = Mathf.Acos(dotValue) * Mathf.Rad2Deg;`：计算这个夹角并转成度数

## 向量与矢量

- 向量没有位置
- 向量只有大小有方向的
- 单位向量就是向量除与模长：$\frac{\vec{a}}{| \vec{a} |}$

### **向量的加减**

- 相对应坐标的加减
- 平行四边形法则、三角形法则

### **向量的点乘**

- 模长：就是向量的长度
- 当有一个向量$\vec{a}$，和一个向量$\vec{b}$相点乘，即是$\vec{a}$的模长乘与$\vec{b}$在$\vec{a}$上面的投影，$\vec{b}$在$\vec{a}$上面的投影即是$\vec{b}$的模长乘两个向量的夹角余弦值

$$
\vec{a}\cdot\vec{b}=|\vec{a}|\times|\vec{b}|\times COS<a,b>
$$

- 一般$|\vec{a}|$和$|\vec{b}|$都使用单位向量表示，即最后得到的其实就是两个向量夹角余弦值
  $$
  \vec{a}\cdot\vec{b} = COS<a,b>
  $$

### **Unity中的点乘公式**

- `Vector3.Dot(Vector3,Vector3)`：传入两个向量，就可以得出这两个向量的点乘值，即这两个向量的余弦值，一般要传入两个标准化的向量，即单位向量
- `Vector3.normalized`：向量标准化，即转化为一个单位向量
- ==通过点乘可以判断物体是不是在自己的角度范围内==

```C#
public GameObject cube1;//自己这个物体
public GameObject cube2;//敌方物体
{
     //计算物体离自己的距离
    float distance = Vector3.Distance(cube1.transform.position, cube2.transform.position);
    //计算物体与自己之间的向量与自己正方向向量的点乘值，计算点乘值的时候传入的必须是标准化的向量
    float dotValue = Vector3.Dot((cube2.transform.position - cube1.transform.position).normalized, cube1.transform.forward);
    //通过点乘值使用反余弦函数就可以计算物体与自己的夹角
    float includedAngle = Mathf.Acos(dotValue) * Mathf.Rad2Deg;
    //当夹角与自己小于30°且距离小于2的时候就在攻击范围内
    if (includedAngle < 30 && distance < 2)
    {
        Debug.Log("在我的攻击范围内");
    }
}
```



### **向量的叉乘**

- 两个向量叉乘，得到一个新的向量，新向量跟原始两个向量都垂直，也就是得到这两个向量所确定平面的法向量[^5]

  [^5]: 法向量就是垂直于两个向量相交的点的向量

### **Unity中的叉乘公式**

- `Vector3.Cross(Vector3,Vector3)`：传进两个向量，返回的也是一个向量，计算的就是法向量[^5]
- 通过叉乘可以判断物体在自己的左边还是右边
- 若是传入的第二个参数向量在第一个参数向量的左边，得到的法向量的`Y`值小于0，右边则大于0

```C#
{  
	//计算物体与自己的叉乘，返回的是一个向量
    Vector3 cross = Vector3.Cross(transform.forward * 10, cube2.transform.position - transform.position);
    //判断向量的Y值
    if (cross.y > 0)
    {
        Debug.Log("目标在右边");
    }
    else if (cross.y < 0)
    {
        Debug.Log("目标在左边");
    }
}
```

## Vector3数学运算

- `Vector3.Lerp(Vector3,Vector3,float)`：==线性插值==，和数学类中的`Lerp`算法是一样的，做的行为就是从第一个点直线运动到第二个点，一般用于摄像机巡游，这样运动起来更自然，一般第一个参数传自己当前的位置，第二个参数传要去的位置
  - `Vector3.Lerp(transform.position, new Vector3(100, 0, 0), 0.01f);`：从自己当前位置做线性运动到(100,0,0)点
- `Vector3.Slerp(Vector3,Vector3,float)`：==球型插值==，返回的也是一个向量，做的行为就是从第一个点球形运动到第二个点
  - `Vector3.Slerp(transform.position, new Vector3(100, 0, 0), 0.01f);`：从自己当前位置做球形运动到(100,0,0)点
- `Vector3.SmoothDamp(Vector3,Vector3,ref Vector3,float,float)`：平滑阻尼运动，参数传进起点、终点、初始速度，时间（阻尼时间），最大速度（可不传），做的行为就是从初始速度开始加速到最大速度后匀速运动然后再近些减速运动

## 四元素

- 四元素是一个超复数，由一个实部三个虚部组成，三个虚部代表`X`、`Y`、`Z`轴，实部是角度
- 四元素旋转只能一个轴一个轴的旋转
- 了解下`Quaternion`构造函数的使用
  - `Quaternion(float x, float y, float z, float w)`：前三个参数传的就是要旋转的轴的值，且角度要除与2，即要旋转60°，则就是要计算30°的值，如下：

```C#
//绕z轴旋转了60°
Quaternion quaternion = new Quaternion(0, 0, Mathf.Sin(30 * Mathf.Deg2Rad), Mathf.Cos(30 * Mathf.Deg2Rad));
cube1.transform.rotation = quaternion;
```

### **四元素常用API**

- `Quaternion Euler(Vector3 euler)`：传要旋转的轴的角度值，返回的是一个`rotation`值
  - 重载函数：`Quaternion Euler(float x, float y, float z)`
  - `Quaternion Euler(0,100,0)`：绕`Y`轴转100°
- `Quaternion AngleAxis(float angle, Vector3 axis)`：绕一个轴旋转多少度，传进一个要旋转的度数和一个方向
- `Quaternion LookRotation(Vector3 forward)`：传进一个方向向量，就会使自己的`Z`轴朝着这个方向旋转

```C#
//算物体与自己的方向向量，即算出一个方向
Vector3 dir = cube2.transform.position - transform.position;
//让自己朝着这个方向旋转
transform.rotation = Quaternion.LookRotation(dir);
```

- `Quaternion Lerp(Quaternion a, Quaternion b, float t)`：与`Vector3`的`Lerp`类似，就是让自己的旋转更平滑，`a`是当前的方向，`b`是要旋转的方向
- 两个四元素相乘就是让两个旋转角度叠加

### **让摄像机随着鼠标移动**

```C#
float x = Input.GetAxis("Mouse X");//检测鼠标的左右移动
Vector3 dir = transform.position - cube.transform.position;//得到目标物体与自己的向量
Quaternion q = Quaternion.AngleAxis(x, Vector3.up);//当鼠标操作时，让自己朝y轴旋转角度
Vector3 pos = cube.transform.position + q * dir;//四元素和向量相乘就是新的向量位置，加上物体的位置就得到自己旋转后的新位置
transform.position = pos;//让自己移动到新位置
```

- 摄像机全面巡游

```C#
x = Input.GetAxis("Mouse X");//检测鼠标的左右移动
Vector3 dir = transform.position - cube.transform.position;//得到目标物体与自己的向量
Quaternion q = Quaternion.AngleAxis(x, Vector3.up);//当鼠标操作时，让自己朝y轴旋转角度
Vector3 pos = cube.transform.position + q * dir;//四元素和向量相乘就是新的向量位置，加上物体的位置就得到自己旋转后的新位置
transform.position = pos;//让自己移动到新位置

y = Input.GetAxis("Mouse Y");
dir = transform.position - cube.transform.position;
Quaternion u = Quaternion.AngleAxis(y, Vector3.right);
pos = cube.transform.position + u * dir;
transform.position = pos;//让自己移动到新位置

transform.rotation = Quaternion.LookRotation(cube.transform.position - transform.position);//让自己面朝向一直是这个方向向量，即看向物体

float scroll = Input.GetAxis("Mouse ScrollWheel");//检测鼠标滚轮
transform.Translate(Vector3.forward * scroll);//滚轮动的时候自己可以前后移动
```

### **发射圆形子弹**

```C#
for (int i = 0; i < 12; i++)
{
    GameObject go = Instantiate(bullet);
    go.transform.position = transform.position;
    Vector3 dir = Quaternion.AngleAxis(30 * i, Vector3.up) * transform.forward;//得到一个新向量
    go.transform.rotation = Quaternion.LookRotation(dir);
}
```

---

# 协程

- 因为Unity不支持线程，所以就有了协程这个概念，它是一种==“假”线程==，同步主线程执行，在主线程执行的时候开辟分支，执行完之后回到主线程
- 当前脚本失活了，协程也会照样执行，但是游戏物体失活了就不会执行

## 延迟函数

- `Destory(GameObject,float)`：延迟几秒销毁
- `Invoke(string,float)`：传入函数名，延迟时间执行这个函数
- `CancelInvoke(string)`：传入函数名，取消这个`Invoke`执行方法，若是不传参数，就是取消所有`Invoke`
- `InvokeRepeating(string,float,float)`：重复一定时间执行一个函数，传入一个函数名、延迟执行函数的时间、重复要执行这个函数的时间
- 当前脚本失活了，延迟函数也会照样执行，但是游戏物体失活了就不会执行

## 协程

```C#
IEnumerator Enumerator()//声明一个协程
{
    yield return new WaitForSeconds(3);//必须要用yield return返回
    //waitForSeconds()等待几秒
}
void Start()
{
    StartCoroutine("Enumerator");//启动协程
}
```

- 协程在执行的时候，主线程也在执行，相当于多线程

```C#
IEnumerator Enumerator()//声明一个协程
{
    Debug.Log(1)
    yield return new WaitForSeconds(3);//必须要用yield return返回
    //waitForSeconds()等待几秒
    Debug.Log(2);
}
void Start()
{
    Debug.Log(0)
    StartCoroutine("Enumerator");//启动协程
    Debug.Log(3);
}
```

- 上述代码输出结果是`0,1,3,2`，当输出0的时候就进入协程中，执行打印1，然后要等待三秒延迟返回，当协程等待的时候主线程同时在执行，执行打印3，然后三秒结束执行打印2
- 协程可以传参数，当声明协程的时候声明了参数，开启的时候就可以在名字后面加一个参数`StartCoroutine("Enumerator",参数);`

### **开启关闭协程**

```C#
//开启关闭方式1
StartCoroutine("Enumerator");//启动协程
StopCoroutine("Enumerator");//关闭协程

//开启关闭方式2
StartCoroutine(Enumerator());//开启协程
StopCoroutine(StartCoroutine(Enumerator()));//关闭协程

//如果通过StopCoroutine(Enumerator());此方法是关闭不了的协程
//可以通过StopAllCoroutines来关闭、
StopAllCoroutines();
//但是StopAllCoroutines是关闭所有的协程，最好不要使用
```

### **协程的返回内容**

```C#
yield return new WaitForSeconds(3);//等多少秒
yield return new WaitForFixedUpdate();//等固定步长0.02的秒数
yield return null;//等一帧
yield return new WaitForEndOfFrame();
yield break;//直接跳出协程
yield 1;//下一帧执行，无论填什么数字都是下一帧执行
```

## 异步加载
### **异步加载场景**

```C#
AsyncOperation async = null;

IEnumerator Loading()
{
    async = SceneManager.LoadSceneAsync("TankBattle");//异步加载这个场景
    yield return async;//一直等待这个场景加载出来就退出这个协程
}

StartCoroutine("Loading");
```

- `DontDestoryOnLoad(GameObject)`:不销毁这个物体，切换场景时是会清空场景物体的，所以有些管理类挂载的物体是不应该销毁的，可以用这个函数

### **异步加载场景实例**

```C#
//声明一个单例类，同一管理切换场景
using UnityEngine;
using System.Collections;
using UnityEngine.SceneManagement;

public delegate void LoadCallBack(params object[] args);//一个委托变量
public class LoadScene : MonoBehaviour
{
	//单例模式
    public static LoadScene Instance;
    private void Awake()
    {
        Instance = this;
        DontDestroyOnLoad(this.gameObject);
    }
    

    // 统一切换场景接口，用来外部调用，参数分别为场景的名字，和加载场景完之后要做的方法
    public void LoadScene_1(string levelName,LoadCallBack callBack)
    {
        StartCoroutine(IELoadScene(levelName, callBack));
    }

    private AsyncOperation async = null;//一个异步值，用来异步加载
    private IEnumerator IELoadScene(string levelName,LoadCallBack callBack)
    {
    	//参数分别为场景的名字，和加载场景完之后要做的方法，使用委托来传
        async = SceneManager.LoadSceneAsync(levelName);
        yield return async;
        
        //加载完场景需要做的事情-1.打开一个面板
        //假如加载的场景是主场景，要做的是打开玩家信息面板

        //总结：加载完场景需要干的事情不一样，加载什么场景,做什么事情我不管
        if (callBack != null)
        {
            callBack();//如果委托变量不为空即是有这个方法，那就执行这个方法
        }
    }
}

--------------------------------------------------------------------------------------------------
//如何外部调用
private void OnGUI()
{
    if(GUI.Button(new Rect(0, 0, 0, 0), "加载场景")){
        LoadScene.Instance.LoadScene_1("场景名", OpenPanel);
    }
}
private void OpenPanel(params object[] args)
{
    //打开一个面板或者自己声明方法来决定加载场景之后做什么
}
```

---

# 高级组件操作

## Camera摄像机

### **摄像机静态属性**

- `allCameras`：场景中所有摄像机
- `allCamerasCount`：场景中相机数量
- `main`：第一个被标记为MainCamera的相机

### **Camera类常用方法**

- 一般可以通过静态属性`main`点出这些方法

- `WorldToScreenPoint()`：将世界坐标转换成屏幕的坐标，返回一个`Vector3`的值
- `ScreenToWorldPoint(float,float,float)`：将屏幕坐标转世界坐标，第三个参数是指与摄像机的距离
- `Camera.main.ScreenPointToRay（Vector3）`：从屏幕点发射一条射线
- `Camera.main.ViewportPointToRay(vector3)`：从视口点发射一条射线
- `Camera.main.ScreenToViewportPoint(vector3)`：屏幕坐标转视口坐标
- 屏幕坐标系原点在左下角
- GUI坐标系的原点在左上角

### **摄像机组件属性**

- `Clear Flags`：如何清除背景
  - `skybox`天空盒
  - `Solid Color`颜色填充
  - `Depth only` 只画该层，背景透明
  - `Don't Clea`r 不移除，覆盖渲染
- `Culling Mask`：选择性渲染部分场景，可以指定只渲染对应层级的对象
- `Projection`：摄像机模式
  - `Perspective `透视模式
    - `Clipping Planes`裁剪平面距离
  - `orthographic `正交摄像机，一般用于2D游戏制作
    - `Size`在正交模式下相机一半的尺寸
- `Viewport `Rect：视口范围
  - 主要用于双摄像机游戏
  - 0~1 相当于宽高百分比
- `Depth`：渲染顺序上的深度
- `Redering path`：渲染路径
- `Target Texture`：渲染纹理
  - 可以把摄像机画面渲染到一张图上，主要用于制作小地图
  - 在`Project`右键创建 `Render Texture`
- `Occlusion Culling`：是否启用剔除遮挡
- `Target Display`：用于哪个显示器，主要用来开发有多个屏幕的平台游戏

## 组件Line Renderer

- 划线组件，可以在`Game`视窗观察到划的线

### **LineRenderer类常用方法**

- 这个类需要`new`出对象使用
- `SetPosition(int index, Vector3 position)`：设置一个点在这条线上，传入点的索引值和位置
- `positionCount`：设置线段的数量
- `startWidth`：开始位置
- `endWidth`：结束位置

### **组件的重要属性**

- `Materials`：线的材质球
- `Positions`：线段的点
- `Use World Space`：是否使用世界坐标系
- `Width`：线段宽
- `Loop`：是否重点起始自动相连



## Rigidbody刚体组件
### **力Force**

- 力是矢量，有大小有方向
- 牛顿第二定律$F=m\cdot a$，$a$是加速度，$m$质量，被力的作用质量越大惯性越大
- 动量定理$Ft=m\cdot v$，质量乘于速度就是动量/冲量的值，冲量$Impulse$
- 动量守恒定理$m1\cdot v1=m2\cdot v2$
- 以下这些是作用在刚体`Rigidbody`的组件上的

### **Rigidbody类**

- `velocity`：速度
- `AddForce(Vector3 force)`：给物体添加一个力的作用，传进的是这个力的方向，以世界坐标为参考系
  - `AddForce(Vector3 force, ForceMode mode)`：第二个参数是==力的模式==`ForceMode`，有以下四个模式
    - `Force`：只关注力的作用，与质量相关，因为$F=m\cdot a$
    - `Impulse`：关注的是冲量，质量越大速度越小，$Ft=m\cdot v$
    - `VelocityChange`：只关注速度的变化，与质量就无关
    - `Acceleration`：只关注加速度$a$，与质量无关
- `AddRelativeForce(Vector3 force)`：相对于自身坐标系的力，功能与`AddForce`一样
- `AddTorque(Vector3 torque)`：给物体一个扭力，传一个方向物体就会绕这个轴旋转，就相当于旁边有一个力在推动这个物体
- `AddRelativeTorque()`：与`AddRelativeForce`类似
- `AddForceAtPosition(Vector3 force, Vector3 position)`：给一个点的表面添加一个力的作用，使周围的物体被这个力作用，类似爆炸的时候

### **根据鼠标的位置决定子弹方向**

```c#
{
    //0.实例化预制体，给一个初始位置
    GameObject go = Instantiate(bullet);
    go.transform.position = bulletPos.transform.position;

    //1.获取物体的刚体
    Rigidbody rigidbody = go.GetComponent<Rigidbody>();

    //2.获取鼠标在屏幕上的坐标点并将屏幕坐标转化为世界坐标
    Vector3 pos = Camera.main.ScreenToWorldPoint
        (new Vector3(Input.mousePosition.x, Input.mousePosition.y, 20));

    //3.根据鼠标点的目标点和子弹的起始点的到一个向量作为物体的方向
    Vector3 dir = pos - bulletPos.transform.position;

    //4.将子弹的方向赋值给子弹刚体作为速度方向
    rigidbody.velocity = dir.normalized * 20;
}
```

- `Input.mousePosition.x/y/z`：获取鼠标的坐标

## 射线检测Physics

### **Ray射线类**

- 需要实例化`new`一个`Ray`
- `Ray`有一个构造函数，初始化的时候需要填入初始值，` Ray(Vector3 origin, Vector3 direction)`，两个参数一个原点一个方向就构成这个射线
- `Ray ray = Camera.main.ScreenPointToRay()`：将屏幕上的点形成一条射线

### **Physics射线检测类**

- 这个类中多为静态方法，可以直接通过类名点方法使用
- `Raycast(Ray ray)`：传入一条射线，当射线触碰到==碰撞体==就会有一个返回值，返回的是布尔值
  - `Raycast(Ray ray, out RaycastHit hitInfo)`：传入一条射线和`RaycastHit`变量，当触碰到这个碰撞体的时候，就会返回一个布尔值和这个碰撞点的所有信息，信息包括距离、位置、刚体、碰撞体、`transform`信息、法向量等等
  - `Raycast(Ray ray, float maxDistance, int layerMask)`：三个参数，一条射线，一个最远距离和一个`layer`值
- **`layer`值：这个值就是控件上`Layer`对应层的`ID`的2次方，就是使用十进制数控制层级，例如这个层是第0层，就是$int\space layer=2^0=1$，第8层就是$int\space layer=2^8=256$，若是要同时控制两个层，将这两个值相加就同时访问到了这两个层**
  - `<<` 左移符号 `>>`右移符号：将1左移8位的值`1<<8 == 256`
- 如何多层检测：`( 1 << LayerMask.NameToLayer（层名）) + ( 1 << LayerMask.NameToLayer（层名）) + ...`
  - 本质：2进制位运算
- `Physics.RaycastAll(Ray ray, float maxDistance, int layerMask)`：返回的是一个`RaycastHit`的数组，可以一条射线检测多个碰撞体
- `RaycastHit `返回信息
  - collider 碰撞到的对象碰撞信息
  - transform 碰撞到的对象的transform信息
  - point 碰撞产生的位置点
  - normal 碰撞到的面的法向量
  - rigidbody  碰撞到的对象的刚体信息
  - distance  碰撞的对象离自己的距离

### **Physics球形搜索**

- `OverlapSphere(Vector3 position, float radius, int layerMask)`：范围型搜索，传入一个点、半径，层级值，就可以检测这个点的碰撞体周围范围内的所有碰撞体，返回的是一个碰撞体的数组

**球形搜索实例**

```C#
Collider[] colliders = Physics.OverlapSphere(transform.position, 100, 8);
for (int i = 0; i < colliders.Length; i++)
{
    Debug.Log(colliders[i].transform.name);
}
```

---

# 图片

## Texture属性

- `TextureType`：图片格式，重要的就是`Default`、`Sprite`
  - `Default`：默认图片类型
  - `Normal map`：法向贴图
  - `Editor GUI`：编辑器使用图片
  - `Sprite`：精灵图片格式
  - `Cursor`：鼠标贴图
  - `Cookie`：灯光`Cookie`贴图
  - `Lightmap`：灯光贴图
  - `Single Channel`：单通道贴图
- 其他的查询API

## SpriteRenderer 2D图片渲染组件

- `Sprite`图元资源
- `Color`颜色
- `Flip`翻转
- `Material`材质球 一般不去更改 除非有特殊需求
- `DrawMode `绘制模式
  - `Sliced9`宫图模式
  - `Tiled`平铺模式
  - `Simple`拉伸模式
- `Sorting Laye`r排序层
- `Order in Layer`属于哪一层
- `Mask  Interaction`遮罩模式

## 效应器

- 一系列` Effector 2D`的组件，都是模仿磁铁、风力、平台、浮力等效果，查询API

---

# 修改代码模板

- VS中：D:\VSIDE\Common7\IDE\Extensions\Microsoft\Visual Studio Tools for Unity\ItemTemplates\CSharp\1033
- Unity中：D:\Program Files\Unity2017\Editor\Data\Resources\ScriptTemplates

# 遇到问题解答

## 游戏的单例模式

- 在`Awake`中直接`instance=this`，但这个是在要在挂载的脚本上是这样写的，因为会执行`Awake`函数，如果不挂载就按正常单例模式写

```C#
public static GeneratingEnemies Instance;//全局唯一实例
private void Awake()
{
    Instance=this;
}
```

> 这样可以直接使用类名点出这个实例再调用这个类的方法，一般放在管理类中，即使这个脚本不挂载在组件上也可以进行调用

## 为什么子弹会跟着特效乱跑

- 其实是子弹加了刚体，受到了重力的作用，一般只要其中一方有刚体就能发生触碰，不需要两方都加刚体

## 为什么播放了音效特效就出不来这个函数了

- 因为游戏特效也是一个游戏物体，特效只是它身上的脚本文件，要找这个物体再去获得它身上的特效脚本，然后就可以播放特效 

```C#
private GameObject boom;
{
    boom = Instantiate<GameObject>(Resources.Load<GameObject>("FX/Explosion_Smoke_FX"))；
	boom.GetComponent<ParticleSystem>().Play();
}
```

