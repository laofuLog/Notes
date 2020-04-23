# UGUI

## UI基础

### **常见的UI类型**

- 环抱式：将主要角色放在中间，UI按钮环绕在主人物周围，例如崩坏的主界面，荣耀的游戏界面
- 弹框式：弹出任务框这些，但并未完全遮盖原界面类型的，原界面会有点暗淡
- 全屏覆盖：完全覆盖原有界面的UI类型
- 超全屏：一般塔防地图这些，可以一直拖到地图，好像没有边界一样
- 3D：炉石的UI基本都是3D类型

### **常见分辨率**

- 13:6全面屏：2340*1080
- 18:9全面屏：2010*1080
- 4:3Pad屏：1024*768
- 16:9常规：1920*1080

---

## Canvas

- 所有的UI都必须放在Canvas下，Canvas相当于一个画布，让所有UI在上面绘制
- UI控件在`Canvas`的显示顺序
  - 下方物体在上方物体的前面显示
  - 子物体在父物体的前面显示

### **Canvas组件**

- `Render Mode`：渲染模式
  - `Overlay`：覆盖模式，相当于将UI直接通过画布绘制在摄像机的镜头上，所以这个模式的UI在所有场景物体前面
    - `Pixel Perpect`：高像素清晰度渲染，一般像素风游戏可以让像素颗粒感更强
    - `Sort Order`：多`Canvas`时，控制`Canvas`显示顺序，值越大越在前方显示
    - `Target Display`：显示的哪个摄像机
  - `Camera`：相机显示，直接通过相机照到画布`Canvas`上
    - `Render Camera`：要赋值一个摄像机物体
    - `Plane Distance`：`Canvas`到相机的距离，多`Canvas`的时候，距离近的会先渲染覆盖距离远的
    - `Sorting Layer`：排序层，粒子特效的渲染形式一般与UI不同，UI总会覆盖在粒子上，所以就要修改粒子渲染先后顺序的，在Unity右上角的Layers可以设置`Sorting Layer`
      - 越靠下的层越在相机面前显示
      - 相机模式下可以选择UI的渲染排序层
      - 粒子特效选择层在最下面的一个`Render`里有选择
    - `Order in Layer`：值越大渲染靠前
  - `World Space`：世界模式，`Canvas`以3D物体显示在场景中，可以改变它的方向，大小，但是前两个不行

### **Canvas Scaler组件**

- 控制UI对屏幕分辨率的缩放，当分辨率改变的时候能自适应缩放
- `UI Scale Mode`：拉伸类型
  - `Constant Pixel Size`：静态模式
  - `Scale With Screen Size`：屏幕像素模式，一般都用这个模式
- `Reference Resolutio`：参考分辨率，这个UI画布画的UI根据一开始UI设计师设计的整体图的尺寸，然后就会自适应进行缩放
- `Screen Match Mode`：适配模式
  - 宽高自适应
  - 完全的自适应
  - 第三个不常用
- `Reference Piels `：与像素的比例，多少像素等于1米

---

## UI基础组件

### **Rect Transform位置组件**

- `Pivot`：轴点，设置这个控件中间的蓝色点
- `Stretch`：锚点，这个控件相对于`Canvas`的中心点
- 其他的都与`Transform`组件差不多，快捷键`Shift`、`Alt`快捷设置锚点和轴点

### **Text文本组件**

- `Line Spacing`：行间距

- `Rich Text`：是否开启富文本

  ```html
  加粗：<b>文字</b>
  斜体：<i>文字</i>
  大小：<size=字号>文字</size>
  修改颜色：<color=颜色名>文字</color>
  		  <color=#颜色数（十六进制）>文字</color>
  ```

- `Horizontal`：水平溢出
- `Vertical`：垂直溢出
- `Best Fit`：字号自适应，当`Canvas`放大缩小的时候字号自适应

**还可以添加两个效果组件**

- `Shadow`：阴影
- `Outline`：外发光

### **Image图片组件**

- `Image Type`：图片类型
  - `Simple`：普通模式，一般图标使用
    - `Preserve Aspect`：保持图片的宽高比，高度或宽度自适应
    - `Set Native Size`：可以快速恢复美术提供的图片原始像素尺寸
  - `Sliced`：裁剪模式
    - `Fill Center`：是否使用`Sprit`图的裁剪效果
  - `Tiled`：瓦片模式，放大图片的时候，会把当个图平铺，一般做无缝连接的图使用
  - `Filled`：填充模式，一般可以做进度条，血条等
    - `Fill Method`：填充效果
    - `Fill Origin`：填充起始点
    - `Fill Amount`：填充百分比，用作血条进度条的关键
    - `Clockwise`：顺时针还是逆时针填充
  - `Preserve Aspect`：等比缩放

### **Raw Image组件**

- 原始图片组件：可以显示精灵或纹理，功能相对于Image少，所以性能更好，可以控制UV的偏移，来显示精灵的一部分

### **图集制作**

- 在项目面板新建一个图集`Sprite Atlas`
- 把需要制作图集的图片拖到`Objects for Packeng`下
- `Allow Rotation`和`Tight Packing`不要打开
- 一般做一个页面就把一个页面的东西打成一个图集，会减少`Batchs`(绘制调用数)的数值，提高效率

### **Mask遮罩组件**

- 可以遮盖图片显示，阿尔法值为0不显示，为1的地方显示，一般可做头像的显示

---

## UI复合组件与交互

- 交互的三个必然条件
  - 场景中是否有`EventSystem`，且这个物体上存在有`Event System`的脚本
  - Canvas上挂有`Graphic Raycaster`射线发射器
  - 控件上有勾选`Raycast Target`射线接收器
- 事件监测一般都是子物体会反馈都父物体，如果父物体没有打开射线接收，但是子物体有的话，点击子物体就会反馈父物体，UI的射线检测会依次检测**对象树**下的所有元素，有任何射线检测成功，则会反馈
  - 一般控件或者图片都是方形的，如果要做异形按钮，就要用一个个的方形图片打开射线检测，然后设置成异形图片的子物体，然后去拼成异形

### **Button按钮组件**

- `Interactable`：按钮是否可以交互，如果关闭之后点击按钮就没有任何反应
- `Transition`：按钮的交互效果
  - `Color Tint`：交互使用颜色
  - `Sprite Swap`：交互的时候使用图片，就是点击之后会改变图片
  - `Animation`：交互使用动画，即点击后会有动画效果
- `Normal`：正常状态下的样子
- `Highilighted`：高亮时候的样子，就是鼠标放在上面反馈的样子
- `Pressed`：点击时候反馈的样子
- `Disabled`：没有开启控件交互时候显示的样子
- `Color Multiplier`：颜色倍数（RGBA分量乘以倍数）
- `Fade Duration`：切换动画持续时间
- `Navigation`：是否开启键盘导航（`Visualize`开启后，能够看到导航线）
  - 横向移动
  - 纵向移动
  - 自动匹配移动
  - 指定上下左右移动
  - 手机游戏可以关闭导航
- `Button`添加事件`On Click`
  - `On Click`实际上是一个委托，当我们写方法添加到这的时候，就相当于这个委托注册了我们添加的函数，然后就执行了这个函数
  - 写一个脚本，里面写一个方法，可以挂载在Button的父对象上，用加号添加事件，将挂载脚本的对象拖到这里，然后再去对应的按钮添加事件选择对应的函数，当这个按钮被按下的时候，就会执行这个函数

### **异形按钮代码设置**

```c#
public Image img;

{
    img.alphaHitTestMinimumThreshold = 0.1f;
}
```

- 使用这句代码，就可以解决异形按钮，只有有颜色的地方会被触发

### **Button添加事件代码向**

- 代码添加事件有几个步骤，一般代码都挂载在`Panel`上，然后其他的控件都是它的子物体
  - 找到物体，使用`transform.Find()`查找子物体，如果想查找子物体的子物体可以传路径进去可以
  - 获取`Button`组件
  - 属性`onClick`可以点出方法`AddListener()`添加事件，传进的就是方法
    - `button.onClick.AddListener()`

- `RemoveListener()`：删除事件
- `Invoke()`：触发调用的所有回调函数

### **Toggle单选控件**

- 其余参数都与`Button`组件的参数是一样的，以下是不同的以下参数

- `Is On`：用于判断当前`Toggle`是否为开启状态
- `Toggle Transition`：勾切换的动画效果
- `Graphic`：钩钩的图
- `Group`：如果`Toggle`构成组，则需要将`ToggleGroup`拖给`Toggle`
  - 添加组就可以做一个单选的效果，给一个空物体添加一个`Toggle Group`组价，就可以把多个`Toggle`给这个物体做子物体就形成了一个组
- `Toggle`添加事件`On Click`
  - 观察到这里的`On Click`是一个带参数的委托，参数为一个布尔值
  - 这里选择函数支持动态传参和静态传参
- 这里的代码添加事件跟`Button`差不多

### **InputField输入框组件**

- `Text Component`：输入文本的组件
- `Text`：输入的文字
- `Character Limit`：限制输入的字符个数
- `Content Type`：限制输入字符的类型，例如限制整型，或者密码变成星号
- `Line Type`：这个文本框可以输入几行，可以单行双行或者回车结束编辑
- `Placeholder`：提示的文字的组件
- `Caret Blink Rate`：光标闪动的频率
- `Caret Width`：光标的宽度
- `Custom Caret Color`：改变光标的颜色
- `Selection Color`：文字被选中的时候的颜色
- `Hide Mobile Input`：打开后禁止弹出键盘，手机设备
- `Read Only`：让这个文字变成只读文字，不可修改不可编辑
- `InputField`添加事件`On Click`
  - 这里有两个委托，一个`Change`委托是在你输入的过程中每输入一个字符就会执行一次
  - 还有一个委托就是当你输入完之后，按回车或者点输入框外面就执行一次

### **Slider滑块组件**

- `Fill Rect`：滑块填充区域的图形
- `Handle Rect`：滑块的图形
- `Direction`：滑块滑动方向，左到右还是右到左
- `Min Value & Max Value`：最大值和最小值
- `Whole Numbers`：是否限制滑块滑动的值为整数
- `Value`：滑块滑动的值
- 添加事件`On Click`，这里委托添加的参数为`Single`单精度

### **Scrollbar滚动组件**

- `Handle Rect`：拖拽条对象
- `Direction`：运行方向，可以选择上下，下上，左右，右左
- `Value`：拖拽条对应的值（0起始位置，1结束位置）
- `Size`：拖拽条占滚动条的比例，可视区域/实际区域，自动计算，一般不去调
- `Number of Steps`：按步拖拽，固定步数分步显示所有实际区域
- 这里的事件委托是一个单精度浮点型的参数，检测的是滑动块左右滑动时候的值

### **Scroll View矩形滚动组件**

- `Content`：实际显示区域的UI对象，这个是重点，可视范围就是这里控制的
- `Horizontal`：是否开启横向滚动
- `Vertical`：是否开启纵向滚动
- `Movement Type`：滚动类型
  - `Unrestricted`：无边界自由滚动
  - `Elastic`：有边界带回弹效果
    - `Elastic`：回弹系数
  - `Clamped`：有边界无回弹效果
- `Inertia`：有无拖拽惯性
- `Deceleration Rate`：设置惯量时，减速率决定内容停止移动的速度。速率为0将立即停止运动。值为1表示运动永不减速。
- `Scroll Sensitivity`：滚轮或触摸板移动系数
- `Viewport`：`ScrollView`的可视区域
- `Horizontal Scrollbar`：横向滚动条
- `Visibility`：可见性（可见区域与实际显示区域对比）
  - 一直显示
  - 自动隐藏
  - 自动隐藏，并支持自动扩展区域
- `Spacing`：空间，横纵滚动条交叉区域预留空间，当删除滚动条不想要的时候，设置这个它的留边值，就可以无留边滚动，做一个超全屏UI效果
- `VerticalScrollbar`：与上面的一样
- 事件添加`On Click`，这里的委托参数是一个`Vector2`的类型，这里检测的就是滑动条左右上下的值，当滑动到最底部的时候`Y`会等0，用此来判断是否滑动到了底部，当然这个值是是根据滑动条是从上往下滑还是从下往上滑来决定的

#### Content Size Fitter

- 宽高自适应，挂载`Content`下面，有水平自适应和纵向自适应
- ` Unconstrained`：无约束，不要根据布局元素驱动宽度。
- `  Min Size`：根据布局元素的最小宽度改变宽度
- ` Preferred Size`：根据布局元素的首选宽度改变宽度。

### 排列组件

#### Grid Layout Group表格布局组件

- 搭配`Scroll View`可以用作背包等等类型的UI

- `Padding`：外框的内边距
- `Cell Size`：内部元素的大小
- `Spacing`：子元素的间距
- `Start Corner`：第一个子元素位于的角
  - 左上，右上，左下，右下
- `Start Axis`：开始排列的轴方向
  - 横向，纵向
- `Child Alignment`：子元素对齐方式
  - 外框的九个点位，左上对齐左下对齐左中对齐等等
- `Constraint`：固定行列数，让其中的元素只能排几列
  - 自适应，设置固定列[列数]，设置固定行[行数]

#### Vertical Layout Group纵向排序组件

- 与表格排列类似
- `Child Aligment`：子元素对齐方式
- `Child Controls Size`：是否锁定子元素的宽高，锁定后就不能改变子元素的宽高
- `Child Force Expand`：子元素强制自适应，锁定之后就会根据父元素宽高，子元素就等分自适应

#### Horizontal Layout Group横向排序组件

- 参数与纵向一样

### Dropdown下拉列表

- 就是一个下拉菜单选择选项
- `Value`：选项的值，选项是一个列表，所以索引值从0开始
- `Options`：添加下拉选项

- 添加事件的委托参数是一个整型的值，用来判断选择的是哪个选项

## DoTween插件

### **类拓展**

- 扩展方法可能够实现向现有类型“添加”方法，而无需创建新的派生类型（继承）
- 扩展方法必须是静态方法，可以像实例方法一样调用
- 如果原始类中有同名方法，原始方法的优先级高于扩展方法
- 其实类拓展就是将新写的方法添加到原有的类中，这样我们可以往`Transform`里添加方法等等，但是`MonoBehaviour`类不可拓展
- 以下就是往`Transform`中添加一个说话的方法，参数列表中一定要指定这个类，且方法必须是静态的

```C#
public static class Twenn
{
    public static void AddSpeek(this Transform speek)
    {
        Debug.Log("");
    }
}
```

### **DoTween**

- `DoTween`插件其实就是类拓展写的一个插件
- `DOTween`是一个免费的`Unity3D`动画插件，少量编码即可以实现常见的动画效果
- 引入新的命名空间`using DG.Tweening;`
- 安装插件
  - Window->AssetStore下载
  - 导入Package
  - 菜单栏->Tools->DOTween Utility Panel->Setup按钮
- 更新插件
  - 删除Resources/DOTweenSettings文件
  - 删除老的DOTween安装目录Demigiant
  - 重新导入Package，再走安装流程
- 在线手册
  - [国外手册][http://dotween.demigiant.com/documentation.php]
  - [沈军老师翻译手册][https://shenjun4unity.github.io/unityhtml/%E7%AC%AC9%E7%AB%A0%20DOTween/9.2%20%E6%96%87%E6%A1%A3.html]
- Do Tween Animation —— 做补间动画
- Do Tween Patch —— 做补间路径

### DoTween Animation图形参数

- Do Tween Component - - - - DoTween组件的添加
- Do Tween Animation - - - - 做补间动画
- 所有可添加的动画效果
  - Move - - 移动（ - 世界坐标）
  - LocalMove - - 移动（自身坐标 - 相对于父物体）
  - Rotate - - 旋转（ - 世界轴）
  - LocalRotate - - 旋转（ - 自身轴）
  - Scale - - 缩放比例
  - Color - - 颜色
  - Fade - - 淡入/淡出( - 透明度)
  - Text - - 文本
  - Punch - - 重击（打击感）
  - Position —— 位置（ - 打击感 位移 偏移量）
  - Rotation —— 旋转（ - 打击感 角度 偏移量）
  - Scale —— 缩放比例（ - 打击感 缩放 偏移量）
  - Shake - - 摇动（震动）
    - Position —— 位置（ - 摇动 位移 偏移量）
    - Rotation —— 旋转（ - 摇动 角度 偏移量）
    - Scale —— 缩放比例（ - 摇动 缩放 偏移量）
  - Camera - - 相机
    - Aspect —— 方向（ - 改变Camera的视角范围 ）
    - Background —— 背景（ - 改变Camera的背景底色 ）
    - Field of View —— 视野 （ - 改变Camera的视野距离 ）
    - Orthosize —— 正交视野大小
    - PixelRect —— 像素矩阵
    - Rect —— 矩阵
- AutoPlay —— 自动播放（动画）
- AutoKill —— 自动删除（动画） 
- Duration —— 持续的时间
- Ignorer TimeScale —— 忽略时间表
- Ease —— 减缓（动画的过程曲线：这是一个枚举类型） 
- loops —— 循环次数
- ID —— 动画的 ID 标示（通过ID，直接用代码控制）
- TO —— 到达目标位置（可通过点击，设置Form - 从哪里来）
- Snapping —— 强烈 / 突然折断
- Relative —— 相对的
- Events —— 事件
  - 监听的事件：
    - OnStart —— 初始化（只有在第一次运行）
    - OnPlay —— 运行开始（每次运行开始）
    - OnUpdate —— 运行时每一帧
    - OnStep —— 每一步（运行中的每个步骤）
    - OnComplete —— 动画结束后 

### **常用方法**

- 直接`transform`点出即可
- `DoFade()`淡入或淡出
- `DoLocalMove()`本地坐标系，移动动画
- `DoScale()`缩放动画
- `DoRotate()`旋转
- `DoColor()`颜色变化
- `DoText()`文本逐渐展开

## UI控件的自定义交互

- 基础的交互只有一些点击等功能，若要实现长按等等没法实现，所以就需要自定义交互

### **Button的自定义交互**

- 引入命名空间`UnityEngine.EventSystems`
- 接入接口 
  - `IPointerEnterHandler`：当鼠标移入到这个控件的时候执行这个接口中的方法
  - `IPointerExitHandler`：当鼠标移出这个控件的时候执行这个接口方法
  - `IPointerDownHandler`：当鼠标在控件内部按下的时候执行这个接口方法
  - `IPointerUpHandler`：必须按下接口方法执行的时候，这个抬起接口才会执行，如果按下没有执行那么抬起也不会执行
  - `IPointerClickHandler`：必须在控件内部按下抬起的时候，才会执行`Click`，这就是有效的触发效果

- 添加添加事件的类，就是`button`的添加事件`OnClick`，引入两个命名空间`using System;`和`using UnityEngine.Events;`，这样就跟`button`一样，有了一个`OnClick`添加事件的窗口，然后就跟`button`一样可以添加事件了

```c#
//事件成员变量
public ButtonClickedEvent onClick;
//因为当前类需要作为成员变量显示在编辑器上，所以需要加标签[Serializable]
[Serializable]//继承自System
public class ButtonClickedEvent : UnityEvent//继承自UnityEngine.Events
{
	public ButtonClickedEvent()
    {

	}
}
```

### **Button长按效果的实现**

- 思路：当用户按下鼠标的时候，需要启动一个计时器，当计时器达到某个阈值的时候触发这个事件回调，当鼠标按键抬起的时候，需要有长按结束的事件执行

```C#
{
     //点击事件成员变量
    public ButtonClickedEvent onClick;
    //开始长按事件成员变量
    public UnityEvent onLongPressedStart;
    //结束长按事件成员变量
    public UnityEvent onLongPressedEnd;


    //判断是否在按钮图片内
    private bool _InEnter = false;
    //是否按下按钮
    private bool _InDown = false;
    //是否在长按中
    private bool _IsInLongPressed = false;

    //长按时候的计时器
    private float _PressedTime = 0f;
    //长按的阈值
    public float LongPressedTime = 3f;
	
    void Update()
    {
        //如果没有按下鼠标，则不计时
        if(!_InDown)
            return;
        //处于长按中，那么就不再计时
        if(_IsInLongPressed)
            return;

        //如果累积的时间，大于触发的阈值，则执行长按开始事件
        if(_PressedTime >= LongPressedTime)
        {
            onLongPressedStart.Invoke();
            //触发长按后，需要将按钮改为长按状态
            _IsInLongPressed = true;
        }
		//要在判断当前秒数之后才再累加，否则会多加一帧
        _PressedTime += Time.deltaTime;
    }

    public void OnPointerEnter(PointerEventData eventData)
    {
        _InEnter = true;
    }

    public void OnPointerExit(PointerEventData eventData)
    {
        _InEnter = false;
    }

    public void OnPointerDown(PointerEventData eventData)
    {
        _InDown = true;
    }

    public void OnPointerUp(PointerEventData eventData)
    {
        _InDown = false;

        //在按钮区域内
        if (_InEnter)
        {
            //在长按状态的时候就不能执行点击时候的事件
            if(!_IsInLongPressed)
                onClick.Invoke();
        }

        //长按结束后时间归零
        _PressedTime = 0f;

        //如果处于长按状态下，执行长按结束回调函数，并将长按状态关闭
        if(_IsInLongPressed)
        {
            //长按结束的时候执行结束事件
            onLongPressedEnd.Invoke();
            //长按状态关闭
            _IsInLongPressed = false;
        }
    }
}
```

### **拖拽效果的实现**

- 用来实现摇杆

- 接入三个接口
  - `IBeginDragHandler`：开始拖拽的时候执行接口方法
  - `IEndDragHandler`：结束拖拽的时候执行接口方法
  - `IDragHandler`：拖拽期间执行的接口方法

- 做一个让图片跟着鼠标的拖拽进行移动

```C#
{
    Vector2 localPos;
    //把一个屏幕坐标转换成一个本地坐标，为什么要转本地坐标呢，因为一般图片都是在Canvas下的，所以不是在世界坐标，一定是相对于Canvas的本地坐标
    RectTransformUtility.ScreenPointToLocalPointInRectangle(
        GameObject.Find("/Canvas").transform as RectTransform,//参考坐标系的父物体
        eventData.position,//鼠标点击的位置
        eventData.pressEventCamera,//当前触发事件的相机
        out localPos//当前的本地坐标
    );
    //把这个物体的坐标设置成鼠标点的坐标，这样就可以拖拽物体进行移动
    transform.localPosition = localPos;
}
```

- 摇杆的实现

```c#
public class DragBar : MonoBehaviour,IPointerDownHandler,IPointerUpHandler,IDragHandler
{

    //摇杆的底座
    public GameObject dragBar;
    //摇杆的那个杆
    public Transform bar;
    //底座的半径
    public float r;
    

    public void OnPointerDown(PointerEventData eventData)
    {
        //按下的时候摇杆出现
        dragBar.SetActive(true);
        Vector2 localPos;
        //把一个屏幕坐标转换成一个本地坐标，为什么要转本地坐标呢，因为一般图片都是在Canvas下的，所以不是在世界坐标，一定是相对于Canvas的本地坐标
        RectTransformUtility.ScreenPointToLocalPointInRectangle(
            transform as RectTransform,//参考坐标系的父物体
            eventData.position,//鼠标点击的位置
            eventData.pressEventCamera,//当前触发事件的相机
            out localPos//当前的本地坐标
        );
        //把这个物体的坐标设置成鼠标点的坐标，这样就可以拖拽物体进行移动
        dragBar.transform.localPosition = localPos;
    }

    public void OnPointerUp(PointerEventData eventData)
    {
        //底座在抬起的时候就消失了
        dragBar.SetActive(false);
        //抬起的时候中间的杆就回归原点
        bar.localPosition = Vector3.zero;
    }

    public void OnDrag(PointerEventData eventData)
    {
        Vector2 localPos;
        //把一个屏幕坐标转换成一个本地坐标，为什么要转本地坐标呢，因为一般图片都是在Canvas下的，所以不是在世界坐标，一定是相对于Canvas的本地坐标
        RectTransformUtility.ScreenPointToLocalPointInRectangle(
            dragBar.transform as RectTransform,//参考坐标系的父物体
            eventData.position,//鼠标点击的位置
            eventData.pressEventCamera,//当前触发事件的相机
            out localPos//当前的本地坐标
        );
        
        //用这个向量的长度去对比底座半径，如果越界了
        if (localPos.magnitude > r)
        {
            //就使用越界向量的单位向量乘与一个半径就得到这个摇杆的临界向量
            localPos = localPos.normalized * r;
        }

        //把这个物体的坐标设置成鼠标点的坐标，这样就可以拖拽物体进行移动
        bar.localPosition = localPos;
    }


    void Start ()
    {
        dragBar.SetActive(false);
    }

}
```

### **自定义交互的组件EventTrigger**

- 这个组件定义了很多不同的交互效果

- 使用方法其实跟添加组件获取参数是一样的
  - 第一步找到UI
  - 第二步给UI添加一个`EventTrigger`组件，或者获取到这个UI上的组件
  - 第三步给`Trigger`添加一个委托列表并指定交互类型
  - 第四步给`Click`添加事件

```c#
{
    //找到UI物体
    GameObject t = this.transform.Find(URL).gameObject;
    //获取或者添加一个组件
    EventTrigger trigger = t.GetComponent<EventTrigger>();
    //创建一个委托列表
    EventTrigger.Entry entry = new EventTrigger.Entry();
    //指定交互类型
    entry.eventID = EventTriggerType.PointerClick;
	//把委托列表添加给这个组件
    trigger.triggers.Add(entry);
    //给委托列表添加事件
    entry.callback = new EventTrigger.TriggerEvent();
    entry.callback.AddListener(new UnityAction<BaseEventData>(callBack));
}
```

- 也可以在指定交互类型、添加完事件之后再把委托列表添加给这个组件