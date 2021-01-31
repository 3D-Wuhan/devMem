# Coding Tips

很少使用 Qt，由于 meshlab 使用了 Qt，只好花点时间熟悉一下。


## 1. QPainter

entrance level: <https://blog.csdn.net/myuml/article/details/4271540>

senior level: [Qt开发技术：Qt绘图系统（二）QPainter详解](https://blog.csdn.net/qq21497936/article/details/105506028)

---

#include <qpainter.h>

QPainter类有点类似于 MFC 中的 CDC 类。

对要绘制的部件需要重载虚拟函数 paintEvent

```cpp
protected:
    virtual void paintEvent( QPaintEvent * pPaintEvent );
```

在 paintEvent 中进行绘制：
```cpp
{
    QPainter p;

    p.begin(this);

    ......

    p.end();
}
```

QPainter 常用的成员函数和 MFC 中常用的成员函数非常类似, 例如:

drawRect/fillRect/drawLine/drawText/drawPoint/drawPoints/moveTo/lineTo

drawLineSegments/drawPolyline/drawPolygon/drawEllipse/drawArc/drawPixmap...

而且类似 MFC，QPainter 可以设置画笔、画刷。例如：

```cpp
QBrush brush( QColor("white") );

p.setBrush(brush);  // 在begin之后set

QPen pen( QColor("black"), 2, Qt::SolidLine );

p.setPen(pen);
```
 

创建一个填充画刷:

```cpp
QBrush brush;

QPixmap pixmap(...);

brush.setPixma( pixmap );
```

## 2. paintEvent

[QT关键问题解决之paintEvent理解](https://blog.csdn.net/u012151242/article/details/78947024)

--------------





