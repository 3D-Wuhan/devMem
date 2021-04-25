# Ergonomics Evaluating System

For 3D reconstruction, please refer to 
[this repo](https://github.com/hjwdzh/Manifold.git).

<span id="top">top</span>

***************

接续下来我们将焦点集中到数据训练的处理流程上，完成如下任务：
* 生成分区 patches row
* [可视化分区效果](visualDebug.md#showing-results-of-patches-rows)
* 对导入的每个 mesh 计算投影；
* 保存投影结果，形成评估投影库；
* 对评估 mesh 计算投影并计算其与投影库的差值；
* 使用颜色映射显示投影差值；
* 动态显示对比结果，在界面上显示与标准数字头模进行逐一比对的动态效果。

[jump to the bottom](#jump)

[jump to the train mesh](#train-mesh)

[jump to the batch train](#batch-train)

## 1 Ellipsoid projection calculating for each mesh

**特别要注意：** 面罩设计方案的评估投影算法与头面部点云模型的特征投影算法**是不同的！**
* 二者的投影中心不同
* 评估投影没有边距设置

[跳转到评估投影](#evaluating_projection)

The original version of ellipsoid projection is implemented in 
MainWindow\:\:docSphericalProject ( ) at the top of the file 
mainwindow_RunTime_1.cpp. A adapted version for batch training is 
MainWindow\:\:ellipsoidProject_single_file ( QString fileName ) in ln92 of the 
file mainwindow_RunTime_2.cpp.

在新版本中我们取消了类 CMgrPatches，改为直接在 MeshModel 中增加 doProjection ( ) 
函数来进行投影计算。调用的位置位于 mainwindow_RunTime_2.cpp 的 ln225:
```cpp
	pMM->doProjection ( );
```
下面的处理方式已经被废除：
> 在当前的版本中我们采用 CMgrPatches 来管理分片的投影。
> ```cpp
> CMgrPatches *	pMgr	= new CMgrPatches( );
> ```
>
> 事实上，CMgrPatches 管理的是 [CPatchesRowO](../meshLab/mlMeshType/CPatchesRowO.md#class-cpatchesrowo)，
> 后者则管理分片 [CPatchO](../meshLab/mlMeshType/CPatchO.md#class-cpatcho)。

在 CMeshO 中增加了容器
```cpp
	PatchesRowContainer patches_rows;
```

每个分片的投影计算位于 meshmodel_1.cpp 的 ln65:
```cpp
void MeshModel::doProject (CPatchO *pPatch)
```

### 1.1 生成 patches_row

theta 是与 z 轴(由 y 轴转换而来)的夹角, 范围为 [0, PI], cos_theta 从上到下由 1
逐渐减少最后变成 -1。我们对 cos_theta 做上下截断, 使用 trunc_cosInterval.arr 采样取值。



### 1.2 为每个 CPatchesRowO 添加数据成员

phi 是顶点与 x 轴(由 z 轴转换而来)的夹角, 范围 [-PI, PI], 头面部前面部分对应的范
围为 [-PI/2, PI/2. 此时 sin_phi 的变化范围为 [-1, 1]

<span id="ellipsoid_projection"></span>
### 1.3 优化 MainWindow\:\:ellipsoidProject_single_file ( QString fileName )

椭球面投影要解决定义球心及量化两个问题。

函数 MainWindow::findPoints ( ... ) 位于 mainwindow_RunTime.cpp 的顶部，
该函数是当前导致性能急剧下降的关键。找到恰当的分区投影算法是提高整体性能的关键。
因此接下来的目标是：

* 改善分区投影算法的顶点搜索问题
* 采用多线程来实现算法

### 1.4 顶点搜索问题

定义分区链表，将每个顶点的指针记录在分区链表中。
如果采用 vector< > 容器则要注意内存占用，相关分析文章见
[多维 vector 内存占用问题分析](../cpp/vector.md)。

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

#### 1.4.1 最先尝试的是平衡二叉树来搜索顶点并分类但效果很差

最开始我们使用的是平衡二叉树来对点云顶点进行分区，
二叉树的类模板参见：binarySearchTree.hpp。
使用的过程参见：mainwindow_RumTime_1.cpp 中的 MainWindow::docSphericalProject ( )，
那个 avlTree 即是平衡二叉树。结果效果很差，具体原因未详细探究，怀疑原因是由于顶点数太大，
导致二叉树太大，内存消耗太多致使算法缓慢。（具体原因未知，只试了试 debug 版本，
没准 release 版本没有问题）。

由于该算法是整个项目的基础算法，无论 release 还是 debug 都要经常被调用，
因此我们决定重新思考顶点搜索问题，花了大量时间重写这个部分。
希望未来效果能得到较大提升。

### 1.5 顶点的 PATCH 分类不适宜采用优先级队列

由于每一层点云数据我们都去掉了头后部顶点，而且还有 margin，
采用优先级队列无法处理好一部分被去掉的顶点。因此我们设计了一个分层 PATCH 聚合算法。
算法的基本思想是先判断属于哪一 PATCH 层，再在每一层判断属于哪个 PATCH。
每个 PATCH 中都有一个 vector 记录属于它的顶点。

**增加类CMgrPatches用于对PATCH的管理。**

#### 1.5.1 CMgrPatches

输入：头面部点云（去掉无需处理的部分）。

输出：椭球投影结果（由于最后的结果不含口鼻窗口部分，需考虑定义恰当的数据结构保存输出）。

由于我们无需象二叉树那样对全部顶点进行排序，只需确定每个顶点属于哪个分区，
然后对分区进行排序即可。为了更好地简化算法，我们下面将定义分区行 CPatchRow 和分区
CPatch。我们将先对分区行进行排序，再对每个分区行中的分区进行排序。

#### 1.5.2 CPatchesRow

Patch 行管理，将输入的点云分派到恰当的 patch 行。

#### 1.5.3 CPatch

每个 patch 中含有若干点云顶点数据，用于分区投影。


<span id="evaluating_projection"></span>
### 1.6 评估投影 MainWindow\:\:ellipsoidProject_evaluating_file ( QString fileName )

**务请注意评估投影与点云投影的差异！**

[跳转到点云投影](#ellipsoid_projection)




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
The corresponding slot function is in ln938 of mainwindow_RunTime_2.cpp.
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


### 2.3 Storage of the projection results

* 应用目录下建立存储目录，存储已经完成训练的目录清单
    * 选定训练目录后将该目录添加到存储清单 ( see ln974 of mainwindow_RunTime_2.cpp )
* 数据目录下建立本目录的两个文件：
	* proj_results.mat 中存储数据结果
    * proj_list.txt 中存储已经被成功计算的文件清单

### 2.4 Synthesis

ln463 of mainwindow_Runtime_3.cpp

* 读入训练目录清单
* 逐条根据训练记录读入投影记录
* 形成综合评估记录

* 读入标准数据
* 组织标准数据（含点云数据）

## 3. Evaluation

* 计算投影数据
* 对比评估数据
* 对比标准数据
* 与标准数据互动

## 4. displaying evaluating results

For messages displaying, please refer to [GLArea](../meshLab/GLArea/GLArea.md)

<span id="jump">Hello World</span>

[jump to the top](#top)
