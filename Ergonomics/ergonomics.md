# Ergonomics Evaluating System

接续下来我们将焦点集中到数据训练的处理流程上，完成如下任务：
* 对导入的每个 mesh 计算投影；
* 保存投影结果，形成评估投影库；
* 对评估 mesh 进行计算投影并与投影库对比；
* 动态显示对比结果，在界面上显示与标准数字头模进行逐一比对的动态效果。

## 1 Ellipsoid projection calculating for each mesh

The original version of ellipsoid projection is implemented in 
MainWindow\:\:docSphericalProject ( ) at the top of the file 
mainwindow_RunTime_1.cpp. A adapted version for batch training is 
MainWindow\:\:ellipsoidProject_single_file ( QString fileName ) in ln92 of the 
file mainwindow_RunTime_2.cpp.

## 2 The training procedure

The key training procedure is as follows:
* 激发批训练按钮的是 QAction * MainWindow\:\:trainAct，其连接的槽函数是 batchTrain ( )；
* 对每个输入文件进行训练则由槽函数 MainWindow\:\:trainMesh ( QString& fileName ) 完成，
其响应的信号是 SIGNAL( processingMesh(QString&) )。

今后进行多线程改造时可以如下进行：
* 一个线程负责读取 mesh
* 一个线程做渲染
* 多个线程进行椭圆投影计算及训练

实现的过程可以先考虑把进行椭圆投影计算及训练的线程分离出了，如果计算速度快一个线程就行。
但考虑到将来会有分区磨光变换以便对标记点就行磨光变换，涉及的计算与重构代码很可能计算繁重，
架构上还是使用多个椭圆投影计算及训练线程为好。

### 2.1 batch train

ln215 in mainwindow_Init.cpp
```cpp
	trainAct = new QAction ( QIcon ( ":/images/training.png" ), tr ( "&Training with models..." ), this );
	trainAct->setShortcutContext ( Qt::ApplicationShortcut );
	trainAct->setShortcut ( Qt::CTRL + Qt::Key_T );
	connect ( trainAct, SIGNAL ( triggered ( ) ), this, SLOT ( batchTrain ( ) ) );
```
The corresponding slot function is in ln875 of mainwindow_RunTime_2.cpp.
该函数主要获取路径链表 m_listFiles，然后发出 
```cpp
emit	processingMesh ( m_listFiles[0] );
```
进而由信号 processingMesh 的响应函数 trainMesh ( QString& ) 来处理对 
m_listFiles[0] 的训练。

### 2.2 train mesh

经过测试我们发现 trainMesh ( QString& ) 无法很好地在界面进度条上显示总体进度，
因此我们决定将其改为 trainMesh ( int )。

