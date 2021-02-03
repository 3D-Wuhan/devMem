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
������Ǿ��������Ϊ trainMesh ( int )��

