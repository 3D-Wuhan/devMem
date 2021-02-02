# class GLArea

这个是 MeshLab 最关键的界面处理类，继承自 QGLWidget。类 QGLWidget 已经过时了，从 Qt5.4 
开始，Qt 推荐使用 QOpenGLWidget 和 QOpenGL 类。在 MainWindow 中可以通过 
GLArea *GLA() const 来访问当前的用户界面。界面绘制最重要的函数当属 painEvent ( ... )

## 1 GLArea::paintEvent(QPaintEvent* /\*event\*/)

如果需要从其他地方调用标准的 OpenGL API 函数（例如，在小部件的构造函数或自己的paint函数中），
则必须首先调用 makeCurrent()，因此该函数的实现中首先调用了该函数。

紧接着该函数采用 glClearColor( 1.0, 1.0, 1.0, 0.0 ) 及 glClear( GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT )
来重绘背景（清除背景），这可能导致加载新的 mesh 时将先前的绘制一并清除。

### 1.1 GLArea::displayInfo(QPainter *painter)

汉化时我们需要改这个函数。在 gitMesh 中相应增加的函数为
 GLArea::displayInfo_chinese(QPainter *painter)，
该函数位于 glarea_1.cpp 中。若进行二次开发需要在界面上显示一些信息，可以在这里编码进行。

## 2 Mesh drawing procedure

这里我们追踪网格的绘制流程。追踪的起点是 glarea_1.cpp 的 ln267，内容如下：
```cpp
datacont->draw( mp->id( ), context( ) );
```
首先来看看其前面的代码：
```cpp
	MLSceneGLSharedDataContext* datacont = mvc( )->sharedDataContext( );
	if ( datacont == NULL )
		return;

	foreach( MeshModel * mp, this->md( )->meshList )
	{
		if ( meshVisibilityMap[mp->id( )] )
		{
			MLRenderingData curr;
			datacont->getRenderInfoPerMeshView( mp->id( ), context( ), curr );
			MLPerViewGLOptions opts;
			if ( curr.get( opts ) == false )
				throw MLException( QString( "GLArea: invalid MLPerViewGLOptions" ) );
			setLightingColors( opts );


			if ( opts._back_face_cull )
				glEnable( GL_CULL_FACE );
			else
				glDisable( GL_CULL_FACE );

			datacont->setMeshTransformationMatrix( mp->id( ), mp->cm.Tr );
			datacont->draw( mp->id( ), context( ) );
		}
	}
```
上面 mvc( ) 返回的是多视容器（MultiViewer_Container），由 Splitter 派生，而 Splitter 
又是 [QSpliter](../../Qt/QSplitter.md) 的子类，完成的是当前 QGLWidget 的多视管理。
datacont 是 GL 共享数据上下文。

**现在我们的问题是：datacont 是被怎样导入内存的？**

### 2.1 datacont 其实是 MLSceneGLSharedDataContext * scenecontext

```cpp
 MLSceneGLSharedDataContext* MultiViewer_Container::sharedDataContext() {return scenecontext;}
```
也就是说最终返回的是成员变量 MLSceneGLSharedDataContext* scenecontext。
如是现在的问题就变成了多视容器的这个成员变量是干什么的？
```cpp
class MLSceneGLSharedDataContext : public QGLWidget
```
原来这个类是由 QGLWidget 诱导的，事情至此真相大白了，datacont->draw( mp->id( ), context( ) )
其实就是调用 QGLWidget 绘制 mp->id ( )。

**GLArea 管理的一个多视容器，其 paintEvent 最终调用的是多视容器的 scenecontext 完成网格绘制。**
而 scenecontext 是 QGLWidget 诱导的子窗口。

上述 datacont->draw( mp->id( ), context( ) ) 最终调用的是 
```cpp
NotThreadSafeGLMeshAttributesMultiViewerBOManager :: draw(UNIQUE_VIEW_ID_TYPE viewid, const std::vector<GLuint>& textid = std::vector<GLuint>()) const 
```
参见 [wrap vcglib with openGL](../vcglib/wrapGL/wrapGL.md)。


### 2.2 多个 mesh 导入时绘制流程

设置断点，通过调用堆栈追踪绘制流程。

我们在 GLArea::paintEvent( QPaintEvent\* /\*event\*/ ) 中设置断点，
结果发现在读取文件时进度条的回调函数 MainWindow\:\:QCallBack(const int pos, const char \* str) 
调用了 qApp->processEvents( ) 从而导致 GLArea\:\:paintEvent 被调用而重绘界面（清除已有的 mesh）。
屏蔽改行再调试运行，发现界面中的 mesh 不再被清除，先前导入的 mesh 可以正常显示。

**结论：** 由于在 GLArea::paintEvent( QPaintEvent\* /\*event\*/ ) 中会重绘背景，
导致已绘制的 Mesh 被清除，因此如果想保持先前绘制的 Mesh，需要修改这个函数，
或者保证这个函数不被调用。










