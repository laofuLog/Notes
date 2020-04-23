# 文件夹

## Unity的特殊文件夹

- `Resources`：资源文件夹，可以通过路径直接用`API  Resources`进行加载；工程文件打包时，不在特殊文件夹内时，跟其他文件都没有依赖关系时，此资源不会打进包；但是`Resources`文件内所有资源，无论是否跟其他资源有依赖关系，都会打进包；为了减小包体大小，`Resources`不能乱放资源，打包时会被压缩加密，只读属性

- `StandardAssets`：默认文件夹，此文件内资源会被优先编译，一般`U3D`自带资源都在这个文件夹下

- `Plugins`：`Plugins`文件夹用来放`native`插件。它们会被自动包含进`build`中去。注意这个文件夹只能是`Assets`文件夹的直接子目录。跟`Standard Assets`一样，这里的脚本会更早的编译，允许它们被之外的脚本访问

  - 在安卓平台还有个Android文件夹
  - 在苹果平台还有个IOS文件夹
  - 在Windows平台下，`native `插件是`dll`文件
  - `Mac OS X`下，是`bundle`文件
  - `Linux`下，是`.so`文件

- `Editor`：以`Editor`命名的文件夹允许其中的脚本访问`Unity Editor`的`API`。如果脚本中使用了在`UnityEditor`命名空间中的类或方法，它必须被放在名为`Editor`的文件夹中。`Editor`文件夹中的脚本不会在`build`时被包含。在项目中可以有多个`Editor`文件夹，打包的时候不会被打包出去，开发`U3D`编辑器时，编辑器脚本放在该文件夹中

  > 注意：如果在普通的文件夹下，Editor文件夹可以处于目录的任何层级。

- `StreamingAssets`：`StreamingAssets`文件夹也是一个==只读==的文件夹，但是它和`Resources`有点区别，`Resources`文件夹下的资源会进行一次压缩，而且也==会加密==，不使用点特殊办法是拿不到原始资源的。但是`StreamingAssets`文件夹就不一样了，它下面的所有资源不会被加密，然后是原封不动的打包到发布包中，这样很容易就拿到里面的文件。所以`StreamingAssets`适合放一些二进制文件，而`Resources`更适合放一些`GameObject`和Object文件

- `Application.persistentDataPath`：固定数据路径

- `Application.DataPath`：返回`Assets`文件夹的路径

## 文件夹的操作



