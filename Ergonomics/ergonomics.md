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

### 1.1 优化 MainWindow\:\:ellipsoidProject_single_file ( QString fileName )

椭球面投影要解决定义球心及量化两个问题。

函数 MainWindow::findPoints ( ... ) 位于 mainwindow_RunTime.cpp 的顶部，
该函数是当前导致性能急剧下降的关键。找到恰当的分区投影算法是提高整体性能的关键。
因此接下来的目标是：

* 改善分区投影算法的顶点搜索问题
* 采用多线程来实现算法

### 1.2 顶点搜索问题

定义分区链表，将每个顶点的指针记录在分区链表中。
如果采用 vector< > 容器则要注意内存占用，相关分析文章见
[多维 vector 内存占用问题分析](../meshLab/vcglib/cpp/vector.md)。

以前采用的是 map<DINDEX, LPPROJ_POINT> mapEllipsoidProj 来保存搜索的结果，
效率低处理速度慢，下面改为 vector 内嵌数组的方式。
**本项目重要的数据结构定义在MeshLab/src/common/ml_mesh_type.h中**。
包括最重要的 CVertexO 位于 ln94，而 LPPROJ_POINT 位于 ln143。

算法大意：定义分区对象，并将分区对象排序。每个顶点只能属于某个分区对象，
判断某个顶点属于哪个分区对象采用由后向前逐一查询的方式进行。

由于顶点属于哪个区域具有局部稳定性，因此选择判断起点可以利用这种局部稳定性。

分区的参数：左上角球面坐标、右下角球面坐标

经椭球面投影后区域成长方形（已经切除了顶部和底部，因为两处不含有效数值）。
排序的规则是上部在前、左边在前。两个不同分区的顺序其实由左上角的值就可以判断，
但为了快速排序我们增加了右下角的点做检测。

由于分区都比较小，我们将其称为 PATCH，
定义的位置在 MeshLab/src/common/ml_mesh_type.h 中 ln159。

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
因此刚开始时我们决定将其改为 trainMesh ( int )。但经过 MainWindow 的数据成员
m_iCurrent 可以用于处理进度条，因此最终我们没有修改 trainMesh 的定义。

