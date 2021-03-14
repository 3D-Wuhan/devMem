# Codes Review of Meshlab

[MeshlabԴ���̽](https://blog.csdn.net/qq_38906523/article/details/75210330)

[��meshLab��3D�����л���2D͸����Ϣ���](https://www.cnblogs.com/kekec/archive/2011/06/28/2092623.html)

[meshlab](https://blog.csdn.net/sx341125/category_6566097.html)

[Qtʵ�ֶ�ȡ��ʾobj�ļ�������̬����������������](https://blog.csdn.net/sx341125/article/details/68940860)

[Qtʵ�ֶ�ȡ��ʾobj�ļ��������̼߳�������](https://blog.csdn.net/sx341125/article/details/68926600)

[OpenGL���̴߳������������ҵĲ��Խ��](https://www.cnblogs.com/mazhenyu/archive/2010/04/29/1724190.html)

[]()


*************************************************


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

��Ȼ QtThreadSafeGLMeshAttributesMultiViewerBOManager ���˼򵥵Ķ��߳�֧�ַ�װ��
����� draw ( .. ) ���� QReadLocker locker(&_lock)������Ҫ�������Ŀ��ʹ�ö��̼ܹ߳���
ʹ�� MeshLab ����ά������룬���ö��С�Ŀ��ܳ��ֵĿӡ�

## 2 [class GLArea](GLArea/GLArea.md)
����� MeshLab ��ؼ��Ľ�����Ʒ�װ��

## 3 [vcglib Library](vcglib/vcglib.md)

In this section we analyze how meshlab call into functions in vcglib.

MeshLab �����ؼ����ݽṹʹ�õ��� vcglib ������������ݽṹ������ TriMesh ������ 
vcglib �б�����ģ����Ҫ��Ϥ MeshLab �Ĺؼ����ݽṹ����Ҫ��Ϥ vcglib �����ݽṹ��

## 4 [meshLab Mesh Types](mlMeshType/meshTypes.md)
