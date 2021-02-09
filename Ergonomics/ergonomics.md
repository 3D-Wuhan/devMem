# Ergonomics Evaluating System

�����������ǽ����㼯�е�����ѵ���Ĵ��������ϣ������������
* �Ե����ÿ�� mesh ����ͶӰ��
* ����ͶӰ������γ�����ͶӰ�⣻
* ������ mesh ���м���ͶӰ����ͶӰ��Աȣ�
* ��̬��ʾ�ԱȽ�����ڽ�������ʾ���׼����ͷģ������һ�ȶԵĶ�̬Ч����

## 1 Ellipsoid projection calculating for each mesh

The original version of ellipsoid projection is implemented in 
MainWindow\:\:docSphericalProject ( ) at the top of the file 
mainwindow_RunTime_1.cpp. A adapted version for batch training is 
MainWindow\:\:ellipsoidProject_single_file ( QString fileName ) in ln92 of the 
file mainwindow_RunTime_2.cpp.

### 1.1 �Ż� MainWindow\:\:ellipsoidProject_single_file ( QString fileName )

������ͶӰҪ����������ļ������������⡣

���� MainWindow::findPoints ( ... ) λ�� mainwindow_RunTime.cpp �Ķ�����
�ú����ǵ�ǰ�������ܼ����½��Ĺؼ����ҵ�ǡ���ķ���ͶӰ�㷨������������ܵĹؼ���
��˽�������Ŀ���ǣ�

* ���Ʒ���ͶӰ�㷨�Ķ�����������
* ���ö��߳���ʵ���㷨

### 1.2 ������������

�������������ÿ�������ָ���¼�ڷ��������С�
������� vector< > ������Ҫע���ڴ�ռ�ã���ط������¼�
[��ά vector �ڴ�ռ���������](../meshLab/vcglib/cpp/vector.md)��

��ǰ���õ��� map<DINDEX, LPPROJ_POINT> mapEllipsoidProj �����������Ľ����
Ч�ʵʹ����ٶ����������Ϊ vector ��Ƕ����ķ�ʽ��
**����Ŀ��Ҫ�����ݽṹ������MeshLab/src/common/ml_mesh_type.h��**��
��������Ҫ�� CVertexO λ�� ln94���� LPPROJ_POINT λ�� ln143��

�㷨���⣺����������󣬲���������������ÿ������ֻ������ĳ����������
�ж�ĳ�����������ĸ�������������ɺ���ǰ��һ��ѯ�ķ�ʽ���С�

���ڶ��������ĸ�������оֲ��ȶ��ԣ����ѡ���ж��������������־ֲ��ȶ��ԡ�

�����Ĳ��������Ͻ��������ꡢ���½���������

��������ͶӰ������ɳ����Σ��Ѿ��г��˶����͵ײ�����Ϊ����������Ч��ֵ����
����Ĺ������ϲ���ǰ�������ǰ��������ͬ������˳����ʵ�����Ͻǵ�ֵ�Ϳ����жϣ�
��Ϊ�˿��������������������½ǵĵ�����⡣

���ڷ������Ƚ�С�����ǽ����Ϊ PATCH��
�����λ���� MeshLab/src/common/ml_mesh_type.h �� ln159��

## 2 The training procedure

The key training procedure is as follows:
* ������ѵ����ť���� QAction * MainWindow\:\:trainAct�������ӵĲۺ����� batchTrain ( )��
* ��ÿ�������ļ�����ѵ�����ɲۺ��� MainWindow\:\:trainMesh ( QString& fileName ) ��ɣ�
����Ӧ���ź��� SIGNAL( processingMesh(QString&) )��

�����ж��̸߳���ʱ�������½��У�
* һ���̸߳����ȡ mesh
* һ���߳�����Ⱦ
* ����߳̽�����ԲͶӰ���㼰ѵ��

ʵ�ֵĹ��̿����ȿ��ǰѽ�����ԲͶӰ���㼰ѵ�����̷߳�����ˣ���������ٶȿ�һ���߳̾��С�
�����ǵ��������з���ĥ��任�Ա�Ա�ǵ����ĥ��任���漰�ļ������ع�����ܿ��ܼ��㷱�أ�
�ܹ��ϻ���ʹ�ö����ԲͶӰ���㼰ѵ���߳�Ϊ�á�

### 2.1 batch train

ln215 in mainwindow_Init.cpp
```cpp
	trainAct = new QAction ( QIcon ( ":/images/training.png" ), tr ( "&Training with models..." ), this );
	trainAct->setShortcutContext ( Qt::ApplicationShortcut );
	trainAct->setShortcut ( Qt::CTRL + Qt::Key_T );
	connect ( trainAct, SIGNAL ( triggered ( ) ), this, SLOT ( batchTrain ( ) ) );
```
The corresponding slot function is in ln875 of mainwindow_RunTime_2.cpp.
�ú�����Ҫ��ȡ·������ m_listFiles��Ȼ�󷢳� 
```cpp
emit	processingMesh ( m_listFiles[0] );
```
�������ź� processingMesh ����Ӧ���� trainMesh ( QString& ) ������� 
m_listFiles[0] ��ѵ����

### 2.2 train mesh

�����������Ƿ��� trainMesh ( QString& ) �޷��ܺõ��ڽ������������ʾ������ȣ�
��˸տ�ʼʱ���Ǿ��������Ϊ trainMesh ( int )�������� MainWindow �����ݳ�Ա
m_iCurrent �������ڴ���������������������û���޸� trainMesh �Ķ��塣

