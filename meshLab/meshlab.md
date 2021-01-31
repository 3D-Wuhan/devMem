# Codes Review of Meshlab


-------------------------------------------------------

## 0 Architecture of Meshlab

Meshlab ʹ���� vcglib�����߶�������������о�Ժ�Ľ��������л����ƵĶ������������գ����Ͱ�������
����˵���� vcglib ��չ�죬meshlab �Ѿ��޷��������°汾�ˣ�ֵ�ÿ�֤�������˾��ú����ǵġ�
�Ǹ� external.sln �ƺ��������ڰ� vcglib ��Ӧ��ģ����� meshlab �еġ�

### 0.1 Chinese version

see <https://blog.csdn.net/juebai123/article/details/78740186>

---



## 1 Does Meshlab Support Multi-thread?

���� openGL �����ϲ����̰߳�ȫ�ģ�MeshLab ���н�����ʾ�Ĺؼ��� GLArea �ּ̳��� QGLWidget 
��� openGL ֧�ִ��ڣ���� MeshLab �����ϲ�֧�ֶ��̡߳������ڲ�ȡ�� processEvent��ĳЩ
����Ԫ�����ƶ��߳�֧�֣������������֧�֡�


## 2 GLArea

����� MeshLab ��ؼ��Ľ��洦���࣬�̳��� QGLWidget���� QGLWidget �Ѿ���ʱ�ˣ��� Qt5.4 
��ʼ��Qt �Ƽ�ʹ�� QOpenGLWidget �� QOpenGL �ࡣ�� MainWindow �п���ͨ�� 
GLArea *GLA() const �����ʵ�ǰ���û����档�����������Ҫ�ĺ������� painEvent ( ... )

### 2.1 GLArea::paintEvent(QPaintEvent* /\*event\*/)

�����Ҫ�������ط����ñ�׼�� OpenGL API ���������磬��С�����Ĺ��캯�����Լ���paint�����У���
��������ȵ��� makeCurrent()����˸ú�����ʵ�������ȵ����˸ú�����

�����Ÿú������� glClearColor( 1.0, 1.0, 1.0, 0.0 ) �� glClear( GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT )
���ػ汳�������������������ܵ��¼����µ� mesh ʱ����ǰ�Ļ���һ�������

#### 2.1.1 GLArea::displayInfo(QPainter *painter)

����ʱ������Ҫ������������� gitMesh ����Ӧ���ӵĺ���Ϊ
 GLArea::displayInfo_chinese(QPainter *painter)��
�ú���λ�� glarea_1.cpp �С������ж��ο�����Ҫ�ڽ�������ʾһЩ��Ϣ�����������������С�

## 3 [vcglib](vcglib/vcglib.md)

In this section we analyze how meshlab call into functions in vcglib.

