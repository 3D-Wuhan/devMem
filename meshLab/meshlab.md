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


## 2 [class GLArea](GLArea/GLArea.md)
����� MeshLab ��ؼ��Ľ�����Ʒ�װ��

## 3 [vcglib Library](vcglib/vcglib.md)

In this section we analyze how meshlab call into functions in vcglib.

