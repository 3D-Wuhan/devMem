# class GLArea

����� MeshLab ��ؼ��Ľ��洦���࣬�̳��� QGLWidget���� QGLWidget �Ѿ���ʱ�ˣ��� Qt5.4 
��ʼ��Qt �Ƽ�ʹ�� QOpenGLWidget �� QOpenGL �ࡣ�� MainWindow �п���ͨ�� 
GLArea *GLA() const �����ʵ�ǰ���û����档�����������Ҫ�ĺ������� painEvent ( ... )

## 1 GLArea::paintEvent(QPaintEvent* /\*event\*/)

�����Ҫ�������ط����ñ�׼�� OpenGL API ���������磬��С�����Ĺ��캯�����Լ���paint�����У���
��������ȵ��� makeCurrent()����˸ú�����ʵ�������ȵ����˸ú�����

�����Ÿú������� glClearColor( 1.0, 1.0, 1.0, 0.0 ) �� glClear( GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT )
���ػ汳�������������������ܵ��¼����µ� mesh ʱ����ǰ�Ļ���һ�������

### 1.1 GLArea::displayInfo(QPainter *painter)

����ʱ������Ҫ������������� gitMesh ����Ӧ���ӵĺ���Ϊ
 GLArea::displayInfo_chinese(QPainter *painter)��
�ú���λ�� glarea_1.cpp �С������ж��ο�����Ҫ�ڽ�������ʾһЩ��Ϣ�����������������С�

## 2 Mesh drawing procedure

��������׷������Ļ������̡�׷�ٵ������ glarea_1.cpp �� ln267���������£�
```cpp
datacont->draw( mp->id( ), context( ) );
```
������������ǰ��Ĵ��룺
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
���� mvc( ) ���ص��Ƕ���������MultiViewer_Container������ Splitter �������� Splitter 
���� [QSpliter](../../Qt/QSplitter.md) �����࣬��ɵ��ǵ�ǰ QGLWidget �Ķ��ӹ���
datacont �� GL �������������ġ�

**�������ǵ������ǣ�datacont �Ǳ����������ڴ�ģ�**

### 2.1 datacont ��ʵ�� MLSceneGLSharedDataContext * scenecontext

```cpp
 MLSceneGLSharedDataContext* MultiViewer_Container::sharedDataContext() {return scenecontext;}
```
Ҳ����˵���շ��ص��ǳ�Ա���� MLSceneGLSharedDataContext* scenecontext��
�������ڵ�����ͱ���˶��������������Ա�����Ǹ�ʲô�ģ�
```cpp
class MLSceneGLSharedDataContext : public QGLWidget
```
ԭ����������� QGLWidget �յ��ģ����������������ˣ�datacont->draw( mp->id( ), context( ) )
��ʵ���ǵ��� QGLWidget ���� mp->id ( )��

**GLArea �����һ�������������� paintEvent ���յ��õ��Ƕ��������� scenecontext ���������ơ�**
�� scenecontext �� QGLWidget �յ����Ӵ��ڡ�

���� datacont->draw( mp->id( ), context( ) ) ���յ��õ��� 
```cpp
NotThreadSafeGLMeshAttributesMultiViewerBOManager :: draw(UNIQUE_VIEW_ID_TYPE viewid, const std::vector<GLuint>& textid = std::vector<GLuint>()) const 
```
�μ� [wrap vcglib with openGL](../vcglib/wrapGL/wrapGL.md)��


### 2.2 ��� mesh ����ʱ��������

���öϵ㣬ͨ�����ö�ջ׷�ٻ������̡�

������ GLArea::paintEvent( QPaintEvent\* /\*event\*/ ) �����öϵ㣬
��������ڶ�ȡ�ļ�ʱ�������Ļص����� MainWindow\:\:QCallBack(const int pos, const char \* str) 
������ qApp->processEvents( ) �Ӷ����� GLArea\:\:paintEvent �����ö��ػ���棨������е� mesh����
���θ����ٵ������У����ֽ����е� mesh ���ٱ��������ǰ����� mesh ����������ʾ��

**���ۣ�** ������ GLArea::paintEvent( QPaintEvent\* /\*event\*/ ) �л��ػ汳����
�����ѻ��Ƶ� Mesh ��������������뱣����ǰ���Ƶ� Mesh����Ҫ�޸����������
���߱�֤��������������á�










