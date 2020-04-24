# 矩阵

## 矩阵的定义

### **向量**

- 点乘：计算向量夹角
- 叉乘：计算两个向量构成平面的法向量

### **矩阵**

$$
M_{32}=\left[\begin{matrix} 2 &3 \\-8  &12 \\0 &7 \end{matrix} \right]
$$

- 矩阵有$3$行，$2$列，所以表示为$M_{32}$
- 获取固定元素$M_{32}$，表示获取第二行第二列元素

### **行矩阵**

$$
M_{13}=\left[\begin{matrix} 3 &9 &88 \end{matrix}\right]
$$

### **列矩阵**

$$
M_{31}=\left[\begin{matrix}3 \\9 \\88 \end{matrix}\right]
$$

- 矩阵实际上是一个数组存储（$2$维数组），向量也是一个数组（$1$维数组）矩阵是有行（`Row`）和列（`Column`）之分

## 矩阵的转置

- 矩阵的转置就是行变列列变行

$$
M_{32}=\left[\begin{matrix} 2 &3 \\-8  &12 \\0 &7 \end{matrix} \right]
\\矩阵的转置:\\
M_{23}^T=\left[\begin{matrix} 2 &-8 &0  \\3 &12 &7 \end{matrix} \right]
$$

- 矩阵的特效：$M^{TT}=M$

## 矩阵的乘法

### **矩阵乘标量**

- 矩阵和标量的乘法：矩阵的每个分量，乘以标量

$$
M_{33} \times X
=
\left[\begin{matrix} m_{11}\times X  &m_{12}\times X &m_{13}\times X  \\m_{21}\times X &m_{22}\times X  &m_{23}\times X \\m_{31}\times X  &m_{32}\times X &m_{33}\times X\end{matrix}\right]
$$

### **矩阵乘矩阵**

- 矩阵乘法其实就是一个矩阵变换另一个矩阵
- **限制条件**：乘号左边的矩阵列数$=$乘号右边的矩阵行数
  - 左列等右行
- 矩阵相乘得出的矩阵：左边矩阵的行数$\times$右边矩阵的列数
  - 左行右列
- 矩阵相乘不满足交换律，满足结合律
  - 交换律：$3\times4=4\times3$
    - 即矩阵不满足$M1\times M2=M2 \times M1$
  - 结合律：$3\times 4\times 5=(3\times4)\times5=3\times(4\times5)$
    - 即矩阵满足$M1\times M2 \times M3=M1 \times (M2 \times M3)$
- 矩阵相乘运算公式

$$
\left[\begin{matrix}a_{11} &a_{12} \\a_{21} &a_{22} \\a_{31} &a_{32} \\a_{41} &a_{42}\end{matrix}\right]  
\times 
\left[\begin{matrix}b_{11} &b_{12} &b_{13} &b_{14} \\b_{21} &b_{22} &b_{23} &b_{24} \end{matrix}\right]
=
\left[\begin{matrix}c_{11} &c_{12} &c_{13} &c_{14} \\c_{21} &c_{22} &c_{23} &c_{24}\\c_{31} &c_{32} &c_{33} &c_{34} \\c_{41} &c_{42} &c_{43} &c_{44} \end{matrix}\right]
$$

- 矩阵相乘技巧
  - 新矩阵的每个元素编号列出
  - 找到左边矩阵对应的行和右边矩阵对应的列，相乘再相加
  - 即$ c_{23}=a_{21}\times b_{13}+a_{22}\times b_{23}$

### **Unity向量乘以矩阵**

- 当向量经过矩阵乘法后，我们可以理解为矩阵对向量进行了变换操作。
- 行矩阵与矩阵相乘，因为左边列数要等于右边行数，所以要左乘

$$
\left[\begin{matrix}x &y &z\end{matrix}\right]
\times
\left[\begin{matrix}c_{11} &c_{12} &c_{13}  \\c_{21} &c_{22} &c_{23} \\c_{31} &c_{32} &c_{33}   \end{matrix}\right]
$$

- 列矩阵与矩阵相乘
  - `Unity`矩阵与向量运算，普遍采用列矩阵右乘

$$
\left[\begin{matrix}c_{11} &c_{12} &c_{13}  \\c_{21} &c_{22} &c_{23} \\c_{31} &c_{32} &c_{33}   \end{matrix}\right]
\times
\left[\begin{matrix}x \\y \\z\end{matrix}\right]
$$

## 特殊矩阵

- 方阵：行数与列数相同的矩阵，现阶段考虑$2\times2$，$3\times3$，$4\times4$
- 对角
  - 对角线元素：方阵中，行数和列数相同的元素，就是对角线元素
    - 即$ m_{11}$、$m_{22}$、$m_{33}$行列相同的元素为对角线元素
- 非对角线元素：方阵中，除了对角线元素以外的所有其他元素



### **对角矩阵**

- 非对角线元素的值为0，对角线元素是任意值的方阵

$$
M=\left[\begin{matrix}1 &0 &0  \\0 &-3 &0 \\0 &0 &2  \end{matrix}\right]
$$

### **数量矩阵**

- 对角线元素相等的对角矩阵

$$
M=\left[\begin{matrix}3 &0 &0  \\0 &3 &0 \\0 &0 &3  \end{matrix}\right]
$$

### **单位矩阵**

- 对角线元素都为$1$的对角矩阵
  - 特性：单位矩阵乘以另一个矩阵，还是原来的矩阵

$$
M=\left[\begin{matrix}1 &0 &0  \\0 &1 &0 \\0 &0 &1  \end{matrix}\right]
$$

## 逆矩阵

- 逆矩阵本身就是一个变换过程

- 逆矩阵可以还原一个矩阵的变换过程

### **矩阵的行列式**

- 由于计算逆矩阵时，行列式会作为除数。因为除法的除数不能为$0$，所以可以通过计算矩阵的行列式，来判定矩阵是否存在逆矩阵
  - 行列式表示方式：假设有$M$矩阵，$|M|$表示$M$矩阵的行列式
  - **注意：**行列式是一个标量

- $2\times 2$矩阵行列式：对角元素相乘$-$非对角元素

$$
M=
\left[\begin{matrix}m_{11} & &m_{12}  \\ &\ddots   \\m_{21} & &m_{22}   \end{matrix}\right]
\\公式：
\\
|M|=m_{11} \times m_{22}-m_{21} \times m_{12}
$$

- $3\times 3$矩阵行列式：先算中间一条河，再在河两边相互对望
  - 注：相乘的数一定是和中间河乘数是一样的

$$
M=
\left[\begin{matrix}m_{11}  &m_{12} &m_{13} \\m_{21}  &m_{22} &m_{23}   \\m_{31}  &m_{32} &m_{33} \end{matrix}\right]
\\公式
\\
|M|=
m_{11} \times m_{22} \times m_{33}+m_{21} \times m_{32} \times m_{13} + m_{31} \times m_{12} \times m_{23}
-\\
m_{31} \times m_{22} \times m_{13} - m_{32} \times m_{23} \times m_{11} - m_{21} \times m_{12} \times m_{33}
$$

### **代数余子式**

![代数余子式](https://s1.ax1x.com/2020/04/24/J0O5se.png)

- 余子式有正负，所以$-1^{i+j}$就是用来控制正负，数值是行列式来控制
- 算出每个分量的代数余子式组成一个新矩阵，用于转置再计算逆矩阵

### **标准伴随矩阵**

- 代数余子式构成的矩阵的转置$C^T$

### **逆矩阵**

- 逆矩阵计算公式：逆矩阵 = 标准伴随矩阵 / 行列式
- 逆矩阵的表示方法：假设有矩阵$M$，逆矩阵就是$M^{-1}$
- 逆矩阵的计算有几步
  - 先算矩阵行列式，若为$0$则不存在逆矩阵就不需要再计算
  - 计算每个位置的代数余子式
  - 将代数余子式矩阵转置，得到标准伴随矩阵
  - 标准伴随矩阵除与行列式就得到了逆矩阵
- 特点
  - 逆矩阵的逆矩阵就是原始矩阵$M=(M^{-1})^{-1}$
  - 单位矩阵的逆矩阵就是单位矩阵本身
  - 转置矩阵的逆矩阵就是逆矩阵的转置$(M^T)^{-1}=(M^{-1})^T$
  - 两个矩阵相乘的逆矩阵=后矩阵的逆矩阵 $\times$ 前矩阵的逆矩阵$(A\times B)^{-1}=B^{-1} \times A^{-1}$
- 逆矩阵重要的几何含义
  - 一个矩阵可以表示一个变换，而逆矩阵可以还原这个变换过程

### **算出一个逆矩阵**

$$
一个原始矩阵
\\M=\left[\begin{matrix}1 &2 &0 \\2 &-2 &1  \\1 &0 &-2 \end{matrix}\right]
\\1.求得行列式，行列式必须不为0.........................
\\|M|=14
\\2.求得代数余子式构成的矩阵...................
\\C=\left[\begin{matrix}4 &5 &2 \\4 &-2 &2  \\2 &-1 &-6 \end{matrix}\right]
\\3.标准伴随矩阵=C^T，余子式矩阵的转置........
\\C^T=\left[\begin{matrix}4 &4 &2 \\5 &-2 &-1  \\2 &2 &-6 \end{matrix}\right]
\\4.逆矩阵M^{-1}=C^T\div |M|...................
\\M^{-1}=\left[\begin{matrix}0.29 &0.29 &0.14 \\0.36 &-0.14 &-0.07  \\0.14 &0.14 &-0.43 \end{matrix}\right]
$$

## 矩阵变换

- 变换（`transform`）：指的是我们把一些数据，如点、方向向量甚至是颜色，通过某种方式（矩阵运算），进行转换的过程。
  - 矩阵的乘法就是一种变化过程

### **变换类型**

#### 线性变换
- 保留**矢量加**和**标量乘**的计算
  - $f_{(x)}+f_{(y)}=f_{(x+y)}$
  - $Kf_{(x)}=f_{(K x)}$
  - 线性变换包含：旋转、缩放、镜像、投影，可以使用$3 \times 3$矩阵，表示变换过程
#### 非线性变换
- 包含平移等，可以使用$4 \times 4$矩阵，表示变换过程
#### 仿射变换
- 仿射变换就是合并线性变换与平移变换的变换类型，仿射变换可以使用一个$4 \times 4$的矩阵表示。所以需要将矢量扩展到四维空间下，这就是**齐次坐标空间**，变换矩阵称为**齐次矩阵**。
  - 齐次坐标：$\left[\begin{matrix}x \\y  \\z \\W \end{matrix}\right]$
  - 点的列矩阵$W$分量：补$1$，因为点会受到平移变化的影响
  - 方向矢量列矩阵的$W$分量：补$0$，因为平移不会影响到方向向量的方向

#### 齐次矩阵构成

$$
\left[\begin{matrix}M_{3\times3} &T_{3\times1}  \\0_{1\times3} &1  \end{matrix}\right]
$$

- $M_{3\times3}$：表示线性变换矩阵
- $T_{3\times1}$：表示平移变化矩阵
- $0_{1\times3}$：用于补位，使变换不受其他影响，因为如果要进行仿射变换带上平移的话$ 3\times 3$矩阵是不够的，所以要补位
- 通过齐次矩阵就可以推出以下几个变换矩阵，这些矩阵全都应用于列矩阵的相乘，即右乘一个列矩阵，如果是行矩阵就无法使用，因为行矩阵是左乘

#### 平移矩阵

$$
\left[\begin{matrix}1 &0 &0 &X平移量 \\0 &1 &0 &Y平移量\\0 &0 &1 &Z平移量 \\0 &0 &0 &1 \end{matrix}\right]
$$

- $M_{3\times3}$：这里是一个单位矩阵，因为平移变化是非线性，所以线性处不能影响
- $T_{3\times1}$：平移矩阵

#### 缩放矩阵

$$
\left[\begin{matrix}X_s &0 &0 &0 \\0 &Y_s &0 &0\\0 &0 &Z_s &0 \\0 &0 &0 &1 \end{matrix}\right]
$$
- $M_{3\times3}$：缩放是线性变化，所以这里是一个线性变化矩阵
- $T_{3\times1}$：因为缩放是线性变换，所以平移变换这里为$0$，不能对缩放产生影响

#### 旋转矩阵

- 绕$X$轴，旋转$\theta$角度

$$
\left[\begin{matrix}1 &0 &0 &0 \\0 &cos\theta &-sin\theta &0\\0 &sin\theta &cos\theta &0 \\0 &0 &0 &1 \end{matrix}\right]
$$

- 绕$Y$轴，旋转$\theta$角度

$$
\left[\begin{matrix}
cos\theta &0 &sin\theta &0 
\\0 &1 &0 &0
\\-sin\theta &0 &cos\theta &0 
\\0 &0 &0 &1 
\end{matrix}\right]
$$



- 绕$Z$轴，旋转$\theta$角度

$$
\left[\begin{matrix}
cos\theta  &-sin\theta  &0  &0 
\\sin\theta &cos\theta &0 &0
\\0 &0 &1 &0 
\\0 &0 &0 &1 
\end{matrix}\right]
$$



### **复合变换**

- 例如：一个点$P$$（1,1,1）$，需要做绕$Z$轴旋转$30$度，平移$（5,4,2）$，缩放$（3,2,1）$
  - $M_s \times M_m \times M_r \times P$
  - 即 $M_{缩放}  \times M_{平移} \times M_{旋转} \times P$
- 复合变换，是存在顺序的，因为矩阵乘运算不满足乘法交换律，复合变换的顺序，决定了变换矩阵相乘的顺序
  - 所以$M_r \times M_m \times M_s \times P$是不可行的，结果不一样
- 因为矩阵满足结合律，所以可以得出变换矩阵为
  - $（M_s \times M_m \times M_r ）$
- 最后的结果是
  - $（M_s \times M_m \times M_r ）\times P$

## 坐标空间

### **模型空间**

- 模型内部点的位置都存储在模型文件内，所以点都是相对于模型空间的

### **世界空间**

- 模型在游戏运行时，需要加载到场景中，所以点存储在世界空间中

### **观察（摄像机）空间**

- 物体是否被投射到屏幕中，是由相机控制的，相机相对于物体的位置，决定了显示效果

### **裁剪空间**

- 需要判定点，是否存在于摄相机裁剪视椎体下，如果存在于视椎体内，则点可以进行显示

### **屏幕空间**

- 最终显示的设备为显示器，所以需要将点投影到显示器上，算出屏幕坐标点，由显示器显示

## Unity着色器中常见矩阵

- `UNITY_MATRIX_MVP`：将点从模型空间下，转换到裁剪空间下，是由以下三个矩阵构成
- `UNITY_MATRIX_M`：将点从模型空间下，转换到世界空间下
- `UNITY_MATRIX_V`：将点从世界空间下，转换到观察空间下
- `UNITY_MATRIX_P`：将点从观察空间下，转换到裁剪空间下
- 以下两个互为逆矩阵
- `_Object2World`：将点从模型到世界空间转换
- `_World2Object`：将点从世界空间到模型转换

# Shader着色器

## CPU编程和GPU编程的区别

- `（CPU）Center Processing Unit`：逻辑编程
- `（GPU）Graphics Processing Unit`：图形处理（矩阵运算，数据公式运算，光栅化）

- `shader`实际上就是`GPU`编程

## GPU渲染流程

- 渲染管线

![GPU渲染过程](https://s1.ax1x.com/2020/04/24/J0OLJP.md.jpg)

- 渲染管道也称为渲染流水线，就是CPU准备一些数据，然后告诉GPU渲染出一个二维的图像过程。
- 渲染管道的三个阶段：应用程序阶段，几何阶段，光栅化阶段
- 应用阶段`（CPU）`
  - 主要就是`CPU`将硬盘中的数据加载后，和内存进行通讯，将需要显示的数据信息（点数据，纹理坐标，法线信息），传递给`GPU`处理
- 几何阶段`（GPU）`
  - 几何阶段最重要的工作就是“变换三维顶点坐标”，变换的工作由矩阵运算完成。
  - 物体最终显示：需要GPU进行一系列的矩阵运算，将模型空间下的网格顶点，转换到屏间下（转换点的过程是并行）
  - 非模型网格顶点信息中记录的点，GPU会以线性插值的方式获得并显示
- 光栅化阶段`（GPU）`
  - 使用上一个阶段（几何阶段）递过来的数据，对像素点进行染色，最终显示出来，染色过程为并行执行。

## 着色器Shader

- 给GPU编程的语言，代码会在GPU上并行执行（几何运算，光栅化）

### **Unity的Shader类型**

- 固定管线`Shader`：很古老显卡的编程着色器，现在很少使用，画质不好
- 顶点/片元`Shader`：通用的着色器类型（重点学习），可编程
- 表面体`Shader`：``Unity`自己创建的着色器类型，基于顶点/片元着色器二次封装实现，降低了着色器的编写难度
- 材质球和`Shader`的关系
  -  `Unity`中`Material`用来关联游戏物体和`Shader`着色器，所以编写`Shader`前，需要先建立材质球

## 创建一个Shader

- 创建一个材质球
- 创建一个`Shader`文件，有四种类型
  - `Standard Surface Shader`：表面体`Shader`，Unity封装的，功能强大，性能较弱
  - `Unlit Shader`：顶点/片元`Shader`，通用，在大部分引擎都可以用
  - `Image Effect Shader`：图片效果`Shader`
  - `Compute Shader`：`GPU`运算`Shader`，不仅可以渲染图象还可以计算数据

- `Shader`的代码基础结构

```C#
//着色器名称，可以在材质球上选择
Shader "Unlit/NewUnlitShader"
{
	//添加属性变量，才材质球中会以参数配置的方式进行显示
	Properties
	{
		
	}

	//可以存在多个SubShader,根据显卡性能的不同配置不同的SubShader
	//游戏引擎会从上向下执行SubShader，只会成功执行一个SubShader
	//开发人员会根据不同显卡编写不同的SubShader代码，一个SubShader执行失败就会执行下一个SubShader
	//可以把最新的显卡的SubShader写在上面，较老的显卡的SubShader写在下面，实现多种设备的适配
	SubShader
	{
		//Pass通道就是用来渲染图象的一个通道
		//Pass通道也可以写多个
		//一个Pass通道会产生一次绘制调用DrawCall
		//Pass通道从上往下执行且每一个通道都会执行
		Pass
		{
			
		}
	}
	//所有着色器都执行失败，最终使用Diffuse着色器进行显示
	Fallback "Diffuse"
}
```

### **固定管线Shader**

- 一个基础的改变颜色的代码

```csharp
Shader "Unlit/NewUnlitShader"
{
	
	Properties
	{
		//显示在材质球属性上
		//"_Color"是材质球变量的名称
		//"颜色"这是显示在面板上的变量文字
		//"Color"变量类型，材质球上显示为吸管
		//"(1,1,1,1)"颜色默认值
		_Color("颜色",Color) = (1,1,1,1)
	}

	SubShader
	{
		
		Pass
		{
			//使用固定管线着色器对物体进行染色
			//Color[]固定管线的命令
			//_Color材质球传递过来的颜色命令
			Color[_Color]
		}
	}

	Fallback "Diffuse"
}

```

- `Shader`变量的声明为`变量名(显示在面板上的文字，变量类型) = 默认值`

- 以下是几个常用类型

```csharp
Shader "Unlit/fixedShader"
{
	Properties
	{	
		//2D纹理,white为默认值，没有给图就默认为白图
		_MainTex("2D纹理",2D) = "white"{}
		
		//rect纹理
		_RectTex("Rect纹理",Rect) = ""{}

		//Cube纹理，应用在天空盒上
		_CubeTex("Cube纹理",Cube) = ""{}

		//用来控制一些数值，作为阈值
		_Float("浮点数",Float) = 12

		//可以限定一个范围，比如做一些阿尔法值得改变等等
		_Range("范围",Range(0,1)) = 1

		//齐次坐标
		_Vector("齐次坐标",Vector) = (1,1,1,1)
	}
	SubShader
	{
		Pass
		{
			//设置主纹理到物体上
			SetTexture[_MainTex]
			{
				//将纹理合并到物体上
				Combine texture
			}
			
		}
	}
}
```

### **基础CPU与GPU数据传递**

- 更改材质球上的参数其实就是一个简单的`CPU`传递数据到`GPU`
- 上述写好`Shader`后，将材质球赋给物体，可以通过代码修改上面的参数

```csharp
void Start () 
{

    //获取MeshRender上的材质球，材质球就是CPU和GPU之间的桥梁
    Material m = gameObject.GetComponent<MeshRenderer>().material;
    //这一步可以理解为将数据从CPU传递到GPU
    //参数1：在Shader中定义的变量名称
    //参数2：需要设定的纹理
    m.SetTexture("_MainTex", Resources.Load<Texture>(""));
}
```

## 着色器常用变量类型

```csharp
//2D纹理,white为默认值，没有给图就默认为白图
_MainTex("2D纹理",2D) = "white"{}
		
//rect纹理
_RectTex("Rect纹理",Rect) = ""{}

//Cube纹理，应用在天空盒上
_CubeTex("Cube纹理",Cube) = ""{}

//用来控制一些数值，作为阈值
_Float("浮点数",Float) = 12

//可以限定一个范围，比如做一些阿尔法值得改变等等
_Range("范围",Range(0,1)) = 1

//齐次坐标
_Vector("齐次坐标",Vector) = (1,1,1,1)
    
//类似布尔型    
[Toggle(UNITY_UI_ALPHACLIP)] _BoolGray("To Gray", Float) = 0
```



##  顶点/片元着色器

- 顶点/片元着色器是可编程的着色器，对GPU编程

### **Cg语言**

- Cg语言是可编程管线语言
- 可编程管线语言大概有3种
  - `GLSL`：支持`OpenGL`接口，使用这个开发后只能在`Linux`上运行
  - `HLSL`：支持`DirectX`接口，`Windows`推出的，只能在微软上运行
  - `Cg`：`C for Graphic`，支持两种图形化接口，由`Nvidia`开发，`Cg`允许对顶点变换和像素着色进行编程

#### Cg支持的7种基本数据类型

- `float`：32 位浮点数据,一个符号位;

- `half`：16 位浮点数据;
- `fixed`：12 位浮点数;

- `int`：32 位整形数据,有些 profile 会将 int 类型作为 float 类型使用;

- `bool`：布尔数据；
-  `string`：字符类型；

- `sampler*`：纹理对象的句柄,分为 6 类: `sampler`, `sampler1D`, `sampler2D`, `sampler3D`, `samplerCUBE`和 `samplerRECT`；

- `Cg`是针对`GPU`编程，而`GPU`常做的是矩阵预算，即浮点数字的计算，所以常用的数据类型就是几个浮点型，其他的逻辑运算比较少，逻辑运算一般丢给`CPU`来做

#### Cg向量数据类型

- `Cg`提供了内置的向量数据类型`(built-in vector data types)`，内置的向量数据类型基于基础数据类型
  - 例如：`float4`，表示`float`类型的4元向量。 `bool4`，表示`bool `类型 4 元向量。

- 注意:向量最长不能超过 4 元,即在 `Cg `程序中可以声明 `float1`、`float2`、`float3`、 `float4 `类型的数组变量,但是不能声明超过 4 元的向量
- 向量初始化方式一般为：`float4 array = float4(1.0, 2.0, 3.0, 4.0); `

#### Cg矩阵数据类型

- `Cg` 提供矩阵数据类型,不过最大的维数不能超过$ 4  \times 4$ 阶。

- `float1x1 matrix1`：等价于 `float matirx1`，`x`是字符，并不是乘号

- `float2x3 matrix2`：表示 $2 \times 3$ 阶矩阵，包含 $6 $个 `float` 类型数据 
- `float4x2 matrix3`：表示 $4 \times 2$ 阶矩阵，包含 $8$个 `float` 类型数据 
- `float4x4 matrix4`：表示 $4 \times 4$ 阶矩阵，这是最大的维数

- 矩阵的初始化方式为：`float2x3 matrix5 = {1.0, 2.0, 3.0, 4.0, 5.0, 6.0};`

#### Cg数组

- 在着色程序中，数组通常的使用目的是：作为从外部应用程序传入大量参数输出到`Cg`的顶点程序中的形参接口。
  - 简而言之，数组数据类型在`Cg`程序中的作用是：作为函数的形参，用于大量数据的转递。
- `Cg`中声明数组变量的方式和`C`语言类似
  - `float a[10]`：声明了一个数组,包含 10 个 `float` 类型数据
  - `float4 b[10]`：声明了一个数组,包含 10 个` float4 `类型向量数据
  - 对数组进行初始化的方式为：`float a[4] = {1.0, 2.0, 3.0, 4.0}; `
- 获取数组长度：`float a[2] = {1.0, 2.0}; int nLen = a.length;`
- 声明多维数组以及初始化的方式如下
  - `float b[2][3] = {{0.0, 0.0, 0.0},{1.0, 1.0, 1.0}};`
- 对多维数组取长度
  - `int length1 = b.length; // length1 值为 2`
  - `int length2 = b[0].length; // length2 值为3`

- 数组和矩阵类型的区别
  -  $4 \times 4$阶数组的的声明方式为: `float M[4][4];`
  - $4 $阶矩阵的声明方式为：`float4x4 M;`
  - 前者是一个数据结构，包含16个`float`类型数据，后者是一个4阶矩阵数据。
  - `float4x4 M[4]`，表示一个数组,包 含$4$个$4$阶矩阵数据。
  - **注意**：*进行数组变量声明时,一定要指定数组长度,除非是作为函数参数而声明的
    形参数组。*

#### Cg结构体

- `Cg`语言支持结构体`structure`，实际上`Cg`中的结构体的声明、使用和`C++ `非常类似（只是类似,不是相同）
- 声明一个结构体类型

```c#
struct MyAdd
{
	float val;
	float Add(float x)
	{
		return val + x;
	}
};//结构体后要跟分号
MyAdd s;//声明一个结构体变量
```

- 注意：在当前的所有的`profile`版本下，如果结构体的一个成员函数使用了成员变量，则该成员变量要声明在前。此外，成员函数是否可以重载依赖于使用的`profile`版本。

#### Cg中的数据类型转换

- `Cg`中的类型转换和`C`语言中的类型转换很类似。
- `C` 语言中类型转换可以是强制类型转换，也可以是隐式转换，如果是隐式转换，则数据类型从低精度向高精度转换。在`Cg `语言中也是如此

```csharp
float a = 1.0;
half b = 2.0;
float c = a+b; //等价于 float c = a + (float)b
```

- `Cg` 语言中对于常量数据可以加上类型后缀，表示该数据的类型
- 常量的类型后缀`(type suffix)`有 3 种
  - `f`：表示 `float`
  - `h`：表示 `half`
  - `x`：表示 `fixed`

```csharp
float a = 1.0;
float  b = a + 2.0h; //2.0h 为 half 类型常量数据,运算是需要做类型转换
```

#### Cg中的关系符

- 关系操作符，用于比较同类型数据之间的大小关系或者等价关系（不同类型的基础数据需要进行类型转换，不同长度的向量，不能进行比较）
- 支持的关系符和`C`语言中差不多
  - `<`：小于
  - `<=`：小于或等于
  - `!=`：不等于
  - `==`：等于
  - `>=`：大于等于
  - `>`：大于

#### Cg向量bool逻辑运算

- `Cg`语言表达式允许对向量使用所有的`boolean operator`，如果是二元操作符，则被操作的两个向量的长度必须一致。表达式中向量的每个分量都进行一对一的运算，最后返回的结果是一个`bool`类型的向量，长度和操作数向量一致

```c
float3 a = float3(0.5, 0.0, 1.0);
float3 b = float3(0.6, -0.1, 0.9);
bool3 c = a < b;
//运算后向量c的结果为bool3(true, false, false);
```

- `Cg`语言中有3种逻辑操作符（也被称为`boolean Operators`），逻辑操作符运算后的返回类型均为`bool`类型
  - 逻辑与：`&&`
  - 逻辑或：`||`
  - 逻辑非：`!`
- 逻辑操作符也可以对向量使用，返回的变量类型是同样长度的内置`bool`向量
- `Cg`中的`&&`和`||`不存在`C`中的短路现象（`short-circuiting`），即只用计算一个操作数的`bool`值即可)，而是参与运算的操作数据都进行`bool`分析。
  - 短路现象就是当一个条件满足的时候后续条件就不会再去执行

#### Cg数学操作符

- `Cg`语言对向量的数学操作提供了内置的支持，`Cg`中的数学操作符有
  - `*`、`/`、`-`、`+`、`%`、`++`、`--`、`*=`、`/=`、`+=`、`-=`
  - 后面四种运算符有时被归纳入赋值操作符，不过它们实际上进行数学计算,然后进行赋值，所以这里也放入数学操作符中进行说明
  - 需要注意的是：求余操作符`％`。只能在`int`类型数据间进行，否则编译器会提示错误信息：`error C1021: operands to “%” must be integral.`

#### Cg位移操作符

- `Cg`语言中的移位操作符，功能和`C`语言中的一样，也可以作用在向量上，但向量类型必须是`int`类型。

```csharp
int2 a = int2(0.0,0.0);
int2 b = a>>1;
```

#### Cg的Swizzle操作符

- 可以使用`Cg`语言中的`swizzle`操作符（`.`）将一个向量的成员取出组成一个新的向量。`swizzle`操作符被`GPU`硬件高效支持。
- `swizzle`操作符后接`x、y、z、w`，分别表示原始向量的第一个、第二个、第三个、第四个元素
- `swizzle`操作符后接` r、g、b、a`的含义与前者等同。不过为了程序的易读性，建议对于表示颜色值的向量，使用`swizzle`操作符后接`r、g、b、a`的方式。取得值是一样的
- `Swizzle`操作符只能对结构体和向量使用，不能对数组使用。

```csharp
float4(a, b, c, d).xyz;// 等价于 float3(a, b, c),即取出来的是一个多元向量
float4(a, b, c, d).xyy;// 等价于 float3(a, b, b)
float4(a, b, c, d).wzyx;// 等价于 float4(d, c, b, a)
float4(a, b, c, d).w;// 等价于 float d
```

#### Cg条件运算符

```csharp
expr1 ? expr2 : expr3;

if(a < 0)
{
    b = a;
}
else
{
    c = a;
}
```

#### Cg控制流语句

- `Cg`中的控制流语句和循环语句与`C`语言类似
- 条件语句有：`if、if-else、if-else 、if-else；`
- 循环语句有：`while、for；`
- `break、continue `语句可以在`for`和`while`语句中使用。

#### Cg关键字

- `Cg `中的关键 字很多都是照搬` C\C++`中的关键字，不过` Cg `中也创造了一系列独特的关键字, 这些关键字不但用于指定输入图元的数据含义(*是位置信息,还是法向量信息*)，本质也则对应着这些图元数据存放的硬件资源(*寄存器或者纹理*)，称之为语义词(`Semantics`)，通常也根据其用法称之为绑定语义词(`binding semantics`)
- 除语义词外，`Cg` 中还提供了三个关键字：`in`、`out`、`inout`。用于表示函数的输入参数的传递方式，称为**输入/输出**关键字，这组关键字可以和语义词合用表达硬件上不同的存储位置，即同一个语义词,使用 `in` 关键字修辞和` out` 关键词修辞，表示的图形硬件上不同的寄存器
- `Cg` 语言还提供两个修辞符：`uniform `用于指定变量的数据初始化方式；`const `关键字的含义与` C\C++`中相同，表示被修辞变量为常量变量

#### Cg输入数据流Uniform

- Cg 语言将输入数据流分为两类
  - `Varying inputs`，即数据流输入图元信息的各种组成要素
  - `Uniform inputs`，表示一些与三维渲染有关的离散信息数据，这些数据通常由应用程序传入，并通常不会随着图元信息的变化而变化,如材质对光的反射信息、运动矩阵等。
- 需要注意的一点是：`uniform`修辞的变量的值是从外部传入的，所以在` Cg` 程序(*顶点程序和片段程序*)中通常使用 `uniform `参数修辞函数形参，不容许声明一个用 `uniform `修辞的局部变量

#### Cg输入输出修饰符

- 参数传递是指:函数调用实参值初始化函数形参的过程
- `in`：修辞一个形参只是用于输入,进入函数体时被初始化,且该形参值的改变不会影响实参值,这是典型的值传递方式
- `out`：修辞一个形参只是用于输出的,进入函数体时并没有被初始化，这种类型的形参一般是一个函数的运行结果
- `inout`：修辞一个形参既用于输入也用于输出,这是典型的引用传递

#### Cg输入语义和输出语义的区别

- 语义：是两个处理阶段(*顶点程序、片段程序*)之间的输入／输出数据和寄存器之间的桥梁，同时语义通常也表示数据的含义，如`POSITION `一般表示参数种存放的数据是顶点位置。
  - 只对两个处理阶段的输入／输出数据有意义，也就是说语义只有在入口函数中才有效，在内部函数(*一个阶段的内部处理函数，和下一个阶段没有数据传递关系*)的无效，被忽略。

- 分为输入语义和输出语义，输入语义和输出语义是有区别的

#### 常用输入语义

- `POSITION`：
- `NORMAL`：
- `BINORMAL`：
- `BLENDINDICES`：
- `BLENDWEIGHT`：
- `TANGENT`：
- `PSIZE`：
- `TEXCOORD0—TEXCOORD7`：
- 参数使用语义使用举例：`in float4 modelPos: POSITION`
  - 表示该参数中的数据是的顶点位置坐标(*通常位于模型空间*)，属于输入参数，语义词 `POSITION `是输入语义，如果在` OpenGL `中则对应为接受应用程序传递的顶点数据的寄存器(*图形硬件上*)

#### 输出语义

- 顶点程序的输出数据被传入到片断程序中，所以顶点着色程序的输出语义词，通常也是片段程序的输入语义词，不过语义词`POSITION`除外。
- 这些语义词适用于所有的`Cg vertex profiles`（*几何阶段运行的回调函数*）作为输出语义和`Cg fragment profiles`（*光栅化阶段运行的回调函数*）的输入语义：`POSITION`、`PSIZE`、`FOG`、`COLOR0-COLOR1`、`TEXCOORD0-TEXCOORD7`

#### 顶点着色程序输出语义

- 顶点着色程序必须声明一个输出变量，并绑定`POSITION`语义词，该变量中的数据将被用于，且只被用于光栅化！如果没有声明一个绑定`POSITION`语义词的输出变量就会报错

- 示例代码

```csharp
void main_v(float4 position: POSITION,
            out float4 oposition:POSITION,//输出语义
			uniform float4x4 modelViewProj)
{
	//oposition = mul(modelViewProj,position); 
}
```

- 为了保持顶点程序输出语义和片段程序输入语义的一致性，通常使用相同的 `struct`类型数据作为两者之间的传递,这是一种非常方便的写法，推荐使用

```csharp
struct VertexIn 
{
	float4 position : POSITION; 
	float4 normal : NORMAL;
};
struct VertexScreen
{
	float4 oPosition : POSITION;
	float4 objectPos : TEXCOORD0; 
	float4 objectNormal : TEXCOORD1;
};
```

- 注意:当使用`struct`结构中的成员变量绑定语义时，需要注意到顶点着色程序中使用的`POSITION`语义词，是不会被片段程序所使用的。 
- 如果需要从顶点着色程序向片段程序传递数据，例如顶点投影坐标、光照信息等，则可以声明另外的参数,绑定到`TEXCOORD`系列的语义词进行数据传递,，实际上`TEXCOORD`系列的语义词通常都被用于从顶点程序向片段程序之间传递数据
- 当然，也可以选择不使用`struct`结构，而直接在函数形参中进行语义绑定。无论使用何种方式，都要记住`vertex program`中的绑定语义（*`POSITION`除外*）的输出形参中的数据会传递到`fragment program`中绑定相同语义的输入形参中。 

#### 片段着色程序输出语义

- 片段着色程序的输出语义词较少，通常是`COLOR`。这是因为片段着色程序运行完毕后，就基本到了`GPU`流水线的末端了。片段程序必须声明一个`out`向量(*三元或四元*)，绑定语义词`COLOR`，这个值将被用作该片断的最终颜色值

```csharp
void main_f(out float4 color : COLOR) 
{ 
	color.xyz = float3(1.0,1.0,1.0); 
	color.w = 1.0; 
} 
```

#### 语义绑定方法

- 入口函数输入/输出数据的绑定语义有四种方法
- 绑定语义放在函数的参数列表的参数声明后面

```csharp
[const][in |out |inout] <type><identifier> [:<binding-semantic>][=<initializer>]

例如：
out float4 color:COLOR  
```

- 绑定语义可以放在结构体的成员变量后面

```csharp
struct <struct-tag> 
{ 
	<type><identifier> [:<binding-semantic >]; 
}; 

例如：
struct VertexIn 
{
	float4 position : POSITION; 
	float4 normal : NORMAL;
};
```

- 绑定语义词可以放在函数声明的后面

```csharp
<type> <identifier> (<parameter-list>) [:<binding-semantic]
{
	<body>
}

例如：
float4 vert(float4 v):SV_POSITION
{
	//body
}
```

- 最后一种语义绑定的方法是,将绑定语义词放在全局非静态变量的声明后面

```csharp
<type> <identifier> [:<binding-semantic>][=<initializer>]; 

例如：
float v :POSITION
```

#### Cg函数

- `Cg` 语言中的函数声明形式与 `C\C++`中相同，由返回类型(`return type`)、函数名、形参列表(`parameter list`，位于括号中，并用逗号分隔的参数表)和函数体组成，函数体包含在花括号中。 如果没有返回值，则函数的返回类型是 `void`
- `Cg` 语言支持函数重载`(Functon Overlaoding`)，其方式和 `C++`基本一致，通过形参列表的个数和类型来进行函数区分
- 所谓入口函数，即一个程序执行的入口，例如` C\C++`程序中的 `main()`函数。通常高级语言程序中只有一个入口函数，不过由于着色程序分为顶点程序和片断程序，两者对应着图形流水线上的不同阶段，所以这两个程序都各有一个入口函数。

#### Cg标准函数库

- 和 `C `的标准函数库类似，`Cg` 提供了一系列内建的标准函数。这些函数用于执行数学上的通用计算或通用算法（*纹理映射等*）
- `Cg` 标准函数库主要分为五个部分
  - 数学函数`(Mathematical Functions)`
    - ![数学函数](https://s1.ax1x.com/2020/04/24/J0OvQS.th.jpg)
  - 几何函数`(Geometric Functions) `
    - ![几何函数](https://s1.ax1x.com/2020/04/24/J0OTZd.th.jpg)
  - 纹理映射函数`(TextureMapFunctions)`
    - ![纹理函数](https://s1.ax1x.com/2020/04/24/J0OORf.md.jpg)
  - 偏导数函数`(Derivative Functions) `
  - 调试函数`(Debugging Function) `



### **Cg程序**

- 注：变量需要导入到`Cg`代码段中

```csharp
Shader "Unlit/fixedShader"
{
	Properties
	{	
		_Color("颜色",Color)={1,1,1,1}
	}
	SubShader
	{
		Pass
		{
			//告诉Shader，我需要编写Cg代码
			//Cg开始标记
			CGPROGRAM
			//告诉GPU，vert是几何阶段运行的回调函数，几何阶段的代码写在内部
			#pragma vertex vert

			//告诉GPU，frag是光栅化阶段运行的回调函数，光栅化阶段的代码写在内部
			#pragma fragment frag
			
            //将材质球中的属性导入到Cg代码段中
			fixed4 _Color;
                
			//几何阶段需要将存储在模型空间内的点转换到裁剪空间
			float4 vert(float4 v)
			{
				
			}
			
			//光栅化阶段对像素点进行染色
			fixed4 frag()
			{

			}
			//Cg结束标记
			ENDCG
			
		}
	}
}
```

### **基础顶点着色器**

```csharp
Pass
{
	CGPROGRAM
    
    #pragma vertex vert
	#pragma fragment frag

	//顶点着色器
	//几何阶段需要将存储在模型空间内的点转换到裁剪空间
	//参数：来自于CPU的当前需要渲染点的模型空间下的位置，POSITION用来描述参数v，表示当前点的位置来自于CPU传递，模型空间
    //返回值：裁剪空间下的点的位置（屏幕空间的转换时GPU完成），SV_POSITION用来描述顶点着色器返回值，告诉GPU当前点已经转换到裁剪空间下
    float4 vert(float4 v：POSITION):SV_POSITION
    {
    	//返回值将转后后的顶点传递给GPU，光栅化阶段就会使用转换后的顶点
    	return mul(UNITY_MATRIX_MVP,v);
    }
		
	EBDCG
}
```

### **基础片元着色器**

```csharp
Pass
{
	CGPROGRAM
    #pragma vertex vert
	#pragma fragment frag
	
	//片元、片段着色器（一切模型上的像素点都会执行）
    //光栅化阶段对像素点进行染色
    //参数：来自于GPU顶点着色器，SV_POSITION修饰参数，来自于顶点着色器
    //返回值：当前需要渲染点的颜色，SV_TARGET修饰返回值，告诉GPU返回值是当前需要染得颜色
    fixed4 frag(float4 v:SV_POSITION):SV_TARGET
    {
    	return _Color;
    }
    
    EBDCG
}
```

- 首先需要导入变量到`Cg`代码中才能使用
- 其次要编写顶点着色器，传递位置将位置信息返给片元着色器
- 片元着色器进行染色

## 表面体着色器

- 表面体着色器是Unity自己封装的一个着色器，自带一些效果例如光照，只需要添加函数或者修改就可以实现效果

```csharp
Shader "Custom/NewSurfaceShader" {
	Properties {
		_Color ("Color", Color) = (1,1,1,1)
	}
	SubShader {		
        
		CGPROGRAM
		// 设定表面体着色器为surf,Standard：使用标准光照
		#pragma surface surf Standard fullforwardshadows
		// 支持DirectX最低到9.0
		#pragma target 3.0
            
		struct Input 
		{
			float2 uv_MainTex;
		};

		//导入颜色变量
		fixed4 _Color;
		//参数o因为是inout类型，所以颜色对o赋值，会传递给GPU进行染色，模型空间到裁剪空间的转换，便面提着色器已经实现了
		void surf (Input IN, inout SurfaceOutputStandard o) 
		{
			//Albedp存有材质的颜色
			o.Albedo = _Color.rgb;
			//Alpha存有颜色的透明度
			o.Alpha = _Color.a;
		}
		ENDCG
	}
	FallBack "Diffuse"
}
```

## 去色效果实现

- 参考文章：<https://blog.csdn.net/lyh916/article/details/45726273>
- 在Unity官网上下载对应版本的Shader包
- 精灵图片都有默认的Shader，在包里找到这个默认Shader，拖到项目中，这样就可以修改默认Shader了
- 在片元着色器中添加一行代码然后修改返回值

```csharp
//灰色算法处理
fixed gray = (color.r + color.g + color.b) / 3.0;
return fixed4(gray, gray, gray, color.a);
```

# Shader光照

## 标准光照的构成结构

- 自发光：材质本身发出的光，模拟环境使用的光
- 漫反射光：光照到粗糙材质后，光的反射方向随机，还有一些光发生了折射，造成材质表面没有明显的光斑

![漫反射](https://s1.ax1x.com/2020/04/24/J0Oqit.png)

- 高光反射光：光照到材质表面后，无（低）损失直接反射给观察者眼睛，材质表面能观察到光斑

![高光反射](https://s1.ax1x.com/2020/04/24/J0O4MD.png)

- 环境光：模拟场景光照（简单理解为太阳光）
- 裴祥凤提出的光照理论：标准光照 = 自发光 + 漫反射光 + 高光反射光 + 环境光
- 这个理论是模拟光照效果，并不是真实效果
- 以他的名字命名：Phong光照模型

## 逐顶点光照和逐像素光照

- 顶点着色器：会在模型渲染点上运行，其他的点会线性插值
- 片元着色器：会在模型的所有像素点上运行
- 逐顶点光照：会在顶点着色器上进行光照运算（高洛德着色）
- 逐像素光照：会在片元着色器上运行光照运算（Phong着色）
- 逐像素比逐顶点效果好，逐顶点比逐像素性能好
- 逐像素光照一般用在距离相机较劲的模型上，逐顶点一般用在距离相机有一定距离的模型上

## 漫反射光照模型

- 漫反射光照（兰伯特定律）

$$
漫反射光照 = 光源的颜色 \times 材质的漫反射颜色 \times MAX（0, 标准化后物体表面法线向量 \cdot 标准化后光源方向向量）
$$

- 几个操作数的取得
  - 光源颜色：场景中光`GameObject`取得
    - `_LightColor0.rgb`
  - 材质的漫反射颜色：材质球配置
    - 通过材质球变量去配置
  - `Max`是数学函数，标准化也有函数
    - 标准化函数：`normalize`
- 表面法线向量：`CPU`加载模型后，传递到`GPU`中的
    - 就是点的法线向量的意思
    - 通过模型空间下被渲染的点的法向量转化到世界空间下得到
    - `mul((float3x3)unity_ObjectToWorld, 模型空间下被渲染的点的法向量)`
    - 模型空间下被渲染的点的法向量由`CPU`传过来
  - 光源方向向量：场景中光`GameObject`取得
    - 就是光的方向
    - `_WorldSpaceLightPos0.xyz`
    - 世界空间下标准化后光源方向向量：` normalize(_WorldSpaceLightPos0.xyz)`
  - 取得所有数据后拿公式计算
  
- 漫反射逐顶点光照，所有运算都丢给顶点着色器

```csharp
Pass
{
	//开启前向光照渲染模型，用来计算光照
    Tags{"LightMode"="ForwardBase"}

	CGPROGRAM
    #pragma vertex vert
    #pragma fragment frag

	//需要依赖于Unity的光照Cg文件，获得光照相关的参数：光源颜色，光照方向
    #include "Lighting.cginc"
    //导入数据变量
    fixed4 _Color;

	//因为要传入和传出多个参数，所以使用结构体会比较方便
    //传入参数结构体，CPU传递给顶点着色器的数据
    struct c2v
    {
    	float4 vertex : POSITION;//来自于模型空间下被渲染的点的位置
        float3 normal : NORMAL;//来自于模型空间下被渲染的点的法向量
    };

	//传出参数结构体，顶点着色器传递给片元着色器的数据
    //因为GPU需要知道传递的数据是什么含义，所以需要使用语义修饰被传递的数据
    struct v2f
    {
    	float4 pos : SV_POSITION;//经过矩阵运算后，裁剪空间下点的位置
    	fixed3 diffuseColor : COLOR;//经过兰伯特定律运算后，当前点的颜色
	};

	//参数要取得位置和法线
    v2f vert(c2v data)
    {
    	//必须要做的：将当前需要渲染的点从模型空间转换到裁剪空间下
        float4 pos = UnityObjectToClipPos(data.vertex);
        //漫反射光照 = 光源的颜色 * 材质的漫反射颜色 * MAX（0, 标准化后物体表面法线向量 · 标准化后光源方向向量）

		//光的颜色
    	fixed3 lightColor = _LightColor0.rgb;
        //材质的漫反射颜色
        fixed3 diffuseColor = _Color.rgb;
        //世界空间下标准化后物体点的表面法线向量
        //CPU拿到的法线来自于模型空间，但是光照发生在世界空间下，所以需要将法线从模型空间转换到世界空间下
        //unity_ObjectToWorld事4*4齐次矩阵，法线是3*1列矩阵，所以需要unity_ObjectToWorld强转为3*3矩阵
        fixed3 worldNormal = normalize(mul((float3x3)unity_ObjectToWorld, data.normal));
        //世界空间下标准化后光源方向向量
        fixed3 worldLightDir = normalize(_WorldSpaceLightPos0.xyz);
        //套入兰伯特公式
        fixed3 lanbote = lightColor * diffuseColor*max(0, dot(worldNormal, worldLightDir ) );
        //使用环境光提亮下计算出的颜色
        fixed3 color = UNITY_LIGHTMODEL_AMBIENT.rgb + lanbote;

		v2f result;
        result.pos = pos;
        result.diffuseColor = color;

		//此处由于需要传递两个参数到片元着色器中，所以使用结构体传参
        return result;
	}

	fixed4 frag(v2f data):SV_TARGET
    {
    	return fixed4(data.diffuseColor,1.0);
    }

	ENDCG
}
```

- 逐像素光照，把几何运算丢给顶点着色器，而光照运算放在片元着色器

```csharp
Pass
{
	
    Tags{"LightMode"="ForwardBase"}

	CGPROGRAM
    #pragma vertex vert
    #pragma fragment frag

	
    #include "Lighting.cginc"
    fixed4 _Color;


    struct c2v
    {
    	float4 vertex : POSITION;
        float3 normal : NORMAL;
    };

	
    struct v2f
    {
    	float4 pos : SV_POSITION;
    	//返回标准化后物体表面法线向量
    	fixed3 worldNormal : NORMAL;//返回值不同，所以要修改返回值的结构体
	};

	
    v2f vert(c2v data)
    {
    	v2f result;
    	
    	//因为渲染流水线分为两个阶段，vert()执行的是几何阶段的工作
        //模型空间的裁剪空间转换属于集合运算，所以还得卸载顶点着色器中
       	result.pos = UnityObjectToClipPos(data.vertex);
        //法线变化属于集合运算，也需要放在顶点着色器中
        result.worldNormal = normalize(mul((float3x3)unity_ObjectToWorld, data.normal));
        
        return result;
	}
	//因为是逐像素光照，所以光照运算应该写在片元着色器中
	fixed4 frag(v2f data):SV_TARGET
    {
    	
     	fixed3 worldLightDir = normalize(_WorldSpaceLightPos0.xyz);
    	fixed3 lanbote = _LightColor0.rgb * _Color.rgb * max(0, dot(data.worldNormal, worldLightDir ) );
    	fixed3 color = UNITY_LIGHTMODEL_AMBIENT.rgb + lanbote;
    	return fixed4(color,1.0);
    }
	ENDCG
}
```

- 半兰伯特定律：将整体颜色降低一半，再加一半，就是亮部不会受到影响，但是暗部会提亮
  - 这个`Shader`只需要把公式替换一下即可

$$
漫反射光照=光源的颜色\times材质的漫反射颜色\times （（标准化后物体表面法线向量 \cdot 标准化后光源方向向量）\times 0.5 + 0.5）
$$

- 使用半兰伯特是解决兰伯特暗部过暗的问题

## 高光反射光照模型

- 高光反射光照公式

$$
高光光照=光源的颜色\times材质的高光反射颜色\times MAX（0，标准化后的观察方向向量 \cdot 标准化后光的反射方向）^{光照系数}
$$

- 几个操作数的取得

  - 光源颜色：场景中光`GameObject`取得

    - `_LightColor0.rgb`

  - 材质的高光反射颜色：材质球配置

  - 观察方向向量：相机观察点到当前渲染的点坐标所构成的方向

    - 当前需要渲染的点的位置$-$摄像机的位置
    - `worldPoint.xyz - _WorldSpaceCameraPos.xyz`
    - 然后要标准化：`normalize(worldPoint.xyz - _WorldSpaceCameraPos.xyz)`

  - 光反射方向：用函数`reflect（标准化后光源方向向量I，标准化后物体表面法线向量N）`计算

    - 即`reflect（标准化入射光的方向I，标准化当前点的法线方向N）`
    - `I`和`N`需要归一化，即标准化

    - 标准化入射光的方向：`normalize(_WorldSpaceLightPos0.xyz)`
    - 标准化当前点的法线方向：`normalize(mul((float3x3)unity_ObjectToWorld, 模型空间下被渲染的点的法向量))`
    - 标准化后光的反射方向：`normalize(reflect(I,N))`

  - 光晕系数：材质球配置

    - 光晕大小会和这个系数有关
    - 指数函数`pow(底数，指数)`

- 高光反射逐顶点运算

```csharp
Pass
{
	
    //开启前向光照渲染模型，用来计算光照
    Tags{"LightMode"="ForwardBase"}

	CGPROGRAM
    #pragma vertex vert
    #pragma fragment frag

    #include "Lighting.cginc"
    
    //导入数据变量
    fixed4 _Color;
    float _Gloss;

    struct c2v
    {
    	float4 vertex : POSITION;//用于计算观察方向，模型空间
        float3 normal : NORMAL;//用于计算光的反射方向，模型空间
    };

    struct v2f
    {
    	float4 pos : SV_POSITION;//裁剪空间下的点
        fixed3 specularColor : COLOR;//高光反射算出来的颜色
	};

	
    v2f vert(c2v data)
    {
    	v2f result;
        //高光光照 = 光源的颜色  x 材质的高光反射颜色 x MAX（0，标准化后的观察方向向量 · 标准化后光的反射方向）^光照系数
        
        //1.光的颜色
        fixed3 lightColor = _LightColor0.rgb;
        //2.材质的高光材质颜色
        fixed3 specularColor = _Color.rgb;
        
        //GPU需要知道裁剪空间下的点的位置（GPU会给他做投影）
        float4 clipPoint = UnityObjectToClipPos(data.vertex);
        //为什么要取世界空间下的点，因为需要这个点计算摄像机的观察方向
        //如何将这个点转换到世界空间下，用unity_ObjectToWorld矩阵来转
        //为什么不使用float3x3转换，因为被转换的点带有齐次坐标，所以需要4x4矩阵
        float4 worldPoint = mul(unity_ObjectToWorld,data.vertex);
        //3.标准化后的观察方向向量的计算（当前需要渲染的点的位置-摄像机的位置）
        fixed3 viewDir = normalize(worldPoint.xyz - _WorldSpaceCameraPos.xyz);
        
        //世界空间下标准化后光源方向向量
        fixed3 worldLightDir = normalize(_WorldSpaceLightPos0.xyz);
        //为什么需要世界空间下的法线，因为需要在世界空间下计算光的反射方向
        //为什么需要normalize，因为Cg的函数reflect函数需要的法线必须是标准化后的
        fixed3 worldNormal = normalize(mul((float3x3)unity_ObjectToWorld, data.normal));
        //4.标准化后光的反射方向  reflect（入射光的方向I，当前点的法线方向N）
        fixed3 lightRefDir = normalize(reflect(worldLightDir,worldNormal));
        
        //套入高光反射公式
        fixed3 specular = lightColor * specularColor * pow(max(0, dot(viewDir, lightRefDir)) ,_Gloss);
        
        //使用环境光提亮下计算出的颜色
        fixed3 color = UNITY_LIGHTMODEL_AMBIENT.rgb + specular;
        
        result.pos = clipPoint;
        result.specularColor = color;
        
        return result;
	}
	//因为是逐像素光照，所以光照运算应该写在片元着色器中
	fixed4 frag(v2f data):SV_TARGET
    {
     	return fixed4(data.specularColor,1.0);
    }
	ENDCG
}
```

- 高光反射逐像素运算

```csharp
Pass
{
			
    Tags{"LightMode"="ForwardBase"}

	CGPROGRAM
	#pragma vertex vert
	#pragma fragment frag
    #include "Lighting.cginc"
    fixed4 _Color;
    float _Gloss;

    struct c2v
    {
    	float4 vertex : POSITION;//用于计算观察方向，模型空间
        float3 normal : NORMAL;//用于计算光的反射方向，模型空间
    };
    struct v2f
    {
    	float4 pos : SV_POSITION;//裁剪空间下的点
        float4 worldPosition:TEXCOORD0;//借用纹理的float4语义将计算好的世界空间下的顶点，传递到片元着色器中
    	fixed3 specularNormal : NORMAL;//标准化后物体表面法线向量
	 };
     //参数要取得位置和法线
     v2f vert(c2v data)
     {
        v2f result;

        float4 clipPoint = UnityObjectToClipPos(data.vertex);
        float4 worldPoint = mul(unity_ObjectToWorld,data.vertex);
        fixed3 worldNormal = normalize(mul((float3x3)unity_ObjectToWorld, data.normal));

        result.pos = clipPoint;
        result.worldPosition = worldPoint;
        result.specularNormal = worldNormal;

        return result;
	 }

	 fixed4 frag(v2f data):SV_TARGET
     {
        //高光光照 = 光源的颜色  x 材质的高光反射颜色 x MAX（0，标准化后的观察方向向量 · 标准化后光的反射方向）^光照系数

        //1.光的颜色
        fixed3 lightColor = _LightColor0.rgb;
        //2.材质的高光材质颜色
        fixed3 specularColor = _Color.rgb;
        //3.标准化后的观察方向向量（当前需要渲染的点的位置-摄像机的位置）
        fixed3 viewDir = normalize(data.worldPosition.xyz - _WorldSpaceCameraPos.xyz);

        //4. 标准化后光的反射方向(reflect(光的方向，点的法线方向))
        fixed3 worldLightDir = normalize(_WorldSpaceLightPos0.xyz);
        fixed3 lightRefDir = normalize(reflect(worldLightDir,data.specularNormal));

        fixed3 specular = lightColor * specularColor * pow(max(0, dot(viewDir, lightRefDir)) ,_Gloss);

       	fixed3 color = UNITY_LIGHTMODEL_AMBIENT.rgb + specular;
    	return fixed4(color,1.0);
     }

	ENDCG
}
```

## Phong着色

- 裴祥凤提出的光照理论：标准光照 = 自发光 + 漫反射光 + 高光反射光 + 环境光
- 所以就是将两个光照模型结合在一起

## 纹理采样

- 导入一个纹理到项目中
- 原始纹理（边长是$2^n$），纹理不一定是方图，如果原始图的边长不是$2^n$，游戏引擎在运行时，会自动将纹理的变长补偿为$2^n$，所以补偿是有性能损耗的。
- 贴图与渲染点对应原理：`UV`贴图
- 贴纹理示例

```csharp
Pass
{			
	CGPROGRAM
    #pragma vertex vert
    #pragma fragment frag

    //将Shader材质球上的主纹理，导入到Cg语言中
    sampler2D _MainTex;
    //存储了在材质球上配置的offset和tiling值
    float4 _MainTex_ST;
    fixed4 _Color;

    struct c2v
    {
    	float4 pos : POSITION;
        float4 tex : TEXCOORD0;//来自模型的UV信息  
    };
    struct v2f
    {
    	float4 pos : SV_POSITION;
        float2 uv : TEXCOORD1;
    };

	v2f vert(c2v data)
    {
    	v2f result;
        result.pos = UnityObjectToClipPos(data.pos);
                
		//_MainTex_ST内的xy存储了tiling的缩放值，zw存放了offset的平移值
		//因为是2D纹理，所以只取纹理信息的xy
		result.uv = data.tex.xy * _MainTex_ST.xy + _MainTex_ST.zw;
		return result;
	}
	fixed4 frag(v2f data) : SV_TARGET
	{
		//通过函数，将纹理的UV解出颜色
		return tex2D(_MainTex,data.uv) * _Color;      
	}
	ENDCG
}
```

## 法线贴图

- 法线贴图，对主纹理凹凸显示
- 建模原理，法线贴图：切线空间，存储切线，映射法线，法线信息存储在切线空间中
- 模型是否凹凸，是由模型顶点决定的，现在实现的法线贴图，控制凹凸，实际上是配合光照实现的，凹进去的部分，颜色偏暗，突出来来的部分，颜色偏亮。
- 导入法线贴图，贴图类型转换为`Normal Map`，法线纹理类型
- 重点是一个转换矩阵的生成
- 还有就是通过点乘来模拟矩阵右乘
- 具体代码看文件夹中示例

## 裁剪命令

- 在引擎中，模型一般不会渲染背面，因为往往用户看不到，渲染背面，还会消耗`GPU`性能，但是可以再`Shader`中控制裁剪类型

- 裁剪命令`Cull`值，编写在`Pass`通道内
  - `Back`：裁剪模型背面，不显示模型的背面（默认值）
  - `Front`：裁剪模型正面，不显示模型的正面
  - `Off`：不裁剪，全部都显示

```csharp
Pass
{
	Cull Front
    Cull Back
    Cull Off
    //<body>
}
```

## 渲染和渲染顺序

- 默认的渲染顺序原则：距离相机越近，则渲染越靠前，非透明情况下，靠前的物体会盖住靠后的物体
- 深度：被渲染的物体，到摄像机的距离
- 深度测试：比较当前渲染的物体的深度值
- 深度写入：深度测试确定需要替换颜色时，将需要显示的颜色对应的深度信息写入深度缓冲区
- 深度缓冲区：`Unity`会为屏幕的每个点，设定一个存储有当前渲染的颜色的深度值（相机到颜色点物体的距离），物体如果在摄像机视角上发生重叠后，需要计算每个重叠点像素的深度值，如果被渲染物体的深度值小于已经存储在深度缓冲区内的值，则将被渲染物体的像素点颜色替换掉原有的颜色，默认的深度值，是正无穷
- 颜色缓冲区：与深度缓冲区中像素点一一对应，存储有最终显示在屏幕上的颜色信息，就一个像素点而言，应该存储有一个颜色信息。

### **深度测试**

- `Unity`是做的从前往后进行测试

- `ZTest`表示深度测试通过的条件，用来修改测试通过的结果
  - `Off`：深度测试永远通过，同`Always`
  - `Less`：比深度缓冲区中深度值小时，测试通过
  - `LEqual`（默认值）：比深度缓冲区中深度值小或相等时，测试通过
  - `Equal`：比深度缓冲区中深度值相等时，测试通过
  - `NotEqual`：比深度缓冲区中深度值不相等时，测试通过
  - `Greater`：比深度缓冲区中深度值大时，测试通过
  - `GEqual`：比深度缓冲区中深度值大于等于时，测试通过
  - `Always`：深度测试永远通过

```csharp
Pass
{
	ZTest Always
    ZTest LEqual
    ZTest Off
    //<body>
}
```

- 测试成功，颜色会写入颜色缓冲区中，但是屏幕最终显示的颜色，不一定是当前物体上的颜色，前方还可能有物体

### **渲染类型**

- 不透明物体，使用`Opaque`
- 透明物体，使用`Transparent`
- 写在`Pass`通道上方

```csharp
SubShader
{
	Tags{"RenderType" = "Opague"}
	Pass
	{
		
	}
}
```

### **深度写入**

- `ZWrite`控制深度测试成功后，是否进行深度写入
  - `On`：开启写入
  - `Off`：关闭写入
- 当深度写入关闭时，即使深度测试完全通过，颜色缓冲区不会被写入，必须将深度的物体，放置在`Transparent`队列下，颜色缓冲区的更新才会正常`Unity`内置的透明物体 `Shader`，会关闭深度写入
- 当透明物体在前，非透明物体在后，深度测试都使用默认状态，深度写入保持开启，结果会出现透明物体正常显示，非透明物体被透明物体遮盖的部分，会显示为透明物体颜色

```csharp
Pass
{
	ZWrite On
    ZWrite Off
    //<body>
}
```

### **丢弃片元**

#### 实现透明图

- 当`Alpha`值降低的时候，需要丢弃一些片元
- 丢弃片元：`Discard`

```csharp
 fixed4 frag(v2f data) : SV_TARGET
 {
 	//通过函数，将纹理的UV解出颜色
 	fixed4 color = tex2D(_MainTex,data.uv) * _Color; 

	//丢弃片元：discard
	//当纹理采样后，取到的颜色如果Alpha透明度低于0.5 则不显示这个片元
	if(color.a<=0)
		discard;
	return color;
}
```

#### 用噪声图实现消融效果

```csharp
fixed4 frag(v2f data) : SV_TARGET
{
	//通过函数，将纹理的UV解出颜色
	fixed4 color = tex2D(_MainTex,data.uv) * _Color; 
	//添加一个新图，噪声图
	fixed4 noise = tex2D(_NoiseTex,data.uv);

	//当纹理采样后，取到的颜色如果Alpha透明度低于阈值 则不显示这个片元
	//这样丢弃噪声图上的颜色就实现消融效果
	if(noise.r < _Flag)
		discard;
	return color;
}
```

### **渲染队列**

- 将要渲染的物体分成一组一组的
- 渲染顺序：是从小向大依次渲染
  - `Background（1000）`：最早被渲染的队列值，（天空盒，背景等） 
  - `Geometry（2000）`：**默认队列**，不透明物体的队列值
  - `AlphaTest（2450）`：透明度测试队列
  - `Transparent（3000）`：渲染透明物体存放的队列
  - `Overlay（4000）`：需要覆盖显示的队列（镜头光晕）
- 添加方法

```csharp
SubShader
{
	Tags{"Queue" = "Transparent"}
	//Tags可以叠加使用
	//Tags{"Queue" = "Transparent" "RenderType" = "Opague"}
	Pass
	{
		
	}
}
```

- 队列可以在`Shader`文件的参数面板栏可以看到

### **颜色混合**

#### 光的混合方式

- 加倍：$Color(r, g, b, a) \times 倍数 = Color(r \times倍数, g \times倍数, b \times倍数, a \times倍数)$

- $Color1(r,g, b, a) + Color2(r, g, b, a) = Color3(r1 + r2, g1 + g2, b1 + b2, a1 + a2)$

- $Color1(r,g, b, a) \times Color2(r, g, b, a) = Color3(r1 \times r2, g1 \times g2, b1 \times b2, a1 \times a2)$

- 命令：`Blend SrcFactor DstFactor`，写在`Pass`通道内

#### 颜色混合

- $最终的颜色 = 新颜色 \times SrcFactor + 旧颜色 \times DstFactor$
  - `Blend 系数 系数`

- 旧颜色：上一个`Pass`通道，颜色缓冲区
-  新颜色：当前`Pass`通道的颜色

- 常用系数

![混合颜色系数](https://s1.ax1x.com/2020/04/24/J0OHII.md.png)

- 做一个颜色叠加的效果（固定管线模式的），可以自己使用`Cg`，命令一样

```csharp
Pass
{
	Color[_Color]
}
Pass
{
	Blend One One
	Color[_Color]
}
```

- 常用混合模式命令

![常见混合类型](https://s1.ax1x.com/2020/04/24/J0OIqH.th.png)