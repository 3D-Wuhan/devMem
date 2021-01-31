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


## 2 GLArea

这个是 MeshLab 最关键的界面处理类，继承自 QGLWidget。类 QGLWidget 已经过时了，从 Qt5.4 
开始，Qt 推荐使用 QOpenGLWidget 和 QOpenGL 类。在 MainWindow 中可以通过 
GLArea *GLA() const 来访问当前的用户界面。界面绘制最重要的函数当属 painEvent ( ... )

### 2.1 GLArea::paintEvent(QPaintEvent* /\*event\*/)

如果需要从其他地方调用标准的 OpenGL API 函数（例如，在小部件的构造函数或自己的paint函数中），
则必须首先调用 makeCurrent()，因此该函数的实现中首先调用了该函数。

紧接着该函数采用 glClearColor( 1.0, 1.0, 1.0, 0.0 ) 及 glClear( GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT )
来重绘背景（清除背景），这可能导致加载新的 mesh 时将先前的绘制一并清除。

#### 2.1.1 GLArea::displayInfo(QPainter *painter)

汉化时我们需要改这个函数。在 gitMesh 中相应增加的函数为
 GLArea::displayInfo_chinese(QPainter *painter)，
该函数位于 glarea_1.cpp 中。若进行二次开发需要在界面上显示一些信息，可以在这里编码进行。

## 3 [vcglib](vcglib/vcglib.md)

In this section we analyze how meshlab call into functions in vcglib.

