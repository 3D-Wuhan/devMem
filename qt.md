# Coding Tips

����ʹ�� Qt������ meshlab ʹ���� Qt��ֻ�û���ʱ����Ϥһ�¡�


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

## 2. paintEvent

[QT�ؼ�������֮paintEvent���](https://blog.csdn.net/u012151242/article/details/78947024)

--------------





