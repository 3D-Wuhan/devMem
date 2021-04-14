# Qt Coding Tips

<https://www.jianshu.com/p/304c9e6de4d2>

********************************

����ʹ�� Qt������ meshlab ʹ���� Qt��ֻ�û���ʱ����Ϥһ�¡�

## 0. Qt Ӧ�ó���Ĳ�������

Qt Ӧ�ó���� Release �汾������ú�������𻷾���δ��װ��Ӧ�Ķ�̬���ӿ⣬
������к��������´���

![error running without qt environment](pix/releaseErr.PNG)

����İ취�ǵ�ǰĿ¼������ D:\Qt\Qt5.14.2\5.14.2\msvc2017_64\bin �е� windeployqt.exe:
```shell
D:\Qt\Qt5.14.2\5.14.2\msvc2017_64\bin\windeployqt.exe 3dAI.exe
```
������ɺ�Ŀ��.exeͬ��Ŀ¼�оͻ�������������ˣ�
��ص�plugins��platform�ļ����붯̬�ⶼ�´���ڸ�Ŀ¼�¡�

## 1. QPainter

entrance level: <https://blog.csdn.net/myuml/article/details/4271540>

senior level: [Qt����������Qt��ͼϵͳ������QPainter���](https://blog.csdn.net/qq21497936/article/details/105506028)

---

#include <qpainter.h>

QPainter���е������� MFC �е� CDC �ࡣ

��Ҫ���ƵĲ�����Ҫ�������⺯�� paintEvent

```cpp
protected:
    virtual void paintEvent( QPaintEvent * pPaintEvent );
```

�� paintEvent �н��л��ƣ�
```cpp
{
    QPainter p;

    p.begin(this);

    ......

    p.end();
}
```

QPainter ���õĳ�Ա������ MFC �г��õĳ�Ա�����ǳ�����, ����:

drawRect/fillRect/drawLine/drawText/drawPoint/drawPoints/moveTo/lineTo

drawLineSegments/drawPolyline/drawPolygon/drawEllipse/drawArc/drawPixmap...

�������� MFC��QPainter �������û��ʡ���ˢ�����磺

```cpp
QBrush brush( QColor("white") );

p.setBrush(brush);  // ��begin֮��set

QPen pen( QColor("black"), 2, Qt::SolidLine );

p.setPen(pen);
```
 

����һ����仭ˢ:

```cpp
QBrush brush;

QPixmap pixmap(...);

brush.setPixma( pixmap );
```

## 2 paintEvent

[QT�ؼ�������֮paintEvent���](https://blog.csdn.net/u012151242/article/details/78947024)

--------------

## 3 [QSplitter](QSplitter.md)

## 4 Change Solution's Qt Version

Right click the name of the solution and ...

![change Qt version](pix/changeQtVersion.PNG)

## 5 Is there an event loop for Qt applications?

<https://doc.qt.io/qt-5/qcoreapplication.html#details>

-------------------------------------------------------

The QCoreApplication class provides an event loop for Qt applications without UI. 
This class is used by non-GUI applications to provide their event loop. For non-GUI 
application that uses Qt, there should be exactly one QCoreApplication object. For 
GUI applications, see QGuiApplication. For applications that use the Qt Widgets 
module, see QApplication.

QCoreApplication contains the main event loop, where all events from the operating 
system (e.g., timer and network events) and other sources are processed and 
dispatched. It also handles the application's initialization and finalization, as 
well as system-wide and application-wide settings.

### 5.1 The Event Loop and Event Handling

The event loop is started with a call to exec(). Long-running operations can call 
processEvents() to keep the application responsive.

In general, we recommend that you create a QCoreApplication, QGuiApplication or a 
QApplication object in your main() function as early as possible. **exec( ) will not 
return until the event loop exits**; e.g., when quit() is called.

Several static convenience functions are also provided. The QCoreApplication object 
is available from instance(). Events can be sent with sendEvent() or posted to an 
event queue with postEvent(). Pending events can be removed with removePostedEvents() 
or dispatched with sendPostedEvents().

## 6 [Qt Resource Editor](resourceEditor.md)



