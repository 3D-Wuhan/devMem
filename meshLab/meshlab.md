# Codes Review of Meshlab

[Meshlab源码初探](https://blog.csdn.net/qq_38906523/article/details/75210330)

[在meshLab的3D场景中绘制2D透明信息面板](https://www.cnblogs.com/kekec/archive/2011/06/28/2092623.html)

[meshlab](https://blog.csdn.net/sx341125/category_6566097.html)

[Qt实现读取显示obj文件——动态绑定纹理与消除纹理](https://blog.csdn.net/sx341125/article/details/68940860)

[Qt实现读取显示obj文件——多线程加载纹理](https://blog.csdn.net/sx341125/article/details/68926600)

[OpenGL多线程创建纹理，附加我的测试结果](https://www.cnblogs.com/mazhenyu/archive/2010/04/29/1724190.html)

[]()


*************************************************


## 0 Architecture of Meshlab

Meshlab 使用了 vcglib，二者都是意大利国立研究院的杰作（大中华类似的东东罕见，汗颜！加油啊！）。
网上说由于 vcglib 发展快，meshlab 已经无法兼容其新版本了，值得考证，但个人觉得好象不是的。
那个 external.sln 似乎就是用于把 vcglib 相应的模块加入 meshlab 中的。

### 0.1 Chinese version

see <https://blog.csdn.net/juebai123/article/details/78740186>

---


## 1 Does Meshlab Support Multi-thread?

由于 openGL 本质上不是线程安全的，MeshLab 进行界面显示的关键类 GLArea 又继承自 QGLWidget 
这个 openGL 支持窗口，因此 MeshLab 本质上不支持多线程。但由于采取了 processEvent，某些
界面元素类似多线程支持，例如进度条的支持。

当然 QtThreadSafeGLMeshAttributesMultiViewerBOManager 做了简单的多线程支持封装，
例如对 draw ( .. ) 加了 QReadLocker locker(&_lock)，但真要在你的项目中使用多线程架构并
使用 MeshLab 的三维处理代码，还得多多小心可能出现的坑。

## 2 [class GLArea](GLArea/GLArea.md)
这个是 MeshLab 最关键的界面绘制封装类

## 3 [vcglib Library](vcglib/vcglib.md)

In this section we analyze how meshlab call into functions in vcglib.

MeshLab 的许多关键数据结构使用的是 vcglib 中所定义的数据结构，例如 TriMesh 就是在 
vcglib 中被定义的，因此要熟悉 MeshLab 的关键数据结构首先要熟悉 vcglib 的数据结构。

## 4 [meshLab Mesh Types](mlMeshType/meshTypes.md)
