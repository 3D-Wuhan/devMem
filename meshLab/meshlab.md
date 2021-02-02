# Codes Review of Meshlab


-------------------------------------------------------

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


## 2 [class GLArea](GLArea/GLArea.md)
这个是 MeshLab 最关键的界面绘制封装类

## 3 [vcglib Library](vcglib/vcglib.md)

In this section we analyze how meshlab call into functions in vcglib.

