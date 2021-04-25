# Ergonomics Evaluating System

For 3D reconstruction, please refer to 
[this repo](https://github.com/hjwdzh/Manifold.git).

<span id="top">top</span>

***************

�����������ǽ����㼯�е�����ѵ���Ĵ��������ϣ������������
* ���ɷ��� patches row
* [���ӻ�����Ч��](visualDebug.md#showing-results-of-patches-rows)
* �Ե����ÿ�� mesh ����ͶӰ��
* ����ͶӰ������γ�����ͶӰ�⣻
* ������ mesh ����ͶӰ����������ͶӰ��Ĳ�ֵ��
* ʹ����ɫӳ����ʾͶӰ��ֵ��
* ��̬��ʾ�ԱȽ�����ڽ�������ʾ���׼����ͷģ������һ�ȶԵĶ�̬Ч����

[jump to the bottom](#jump)

[jump to the train mesh](#train-mesh)

[jump to the batch train](#batch-train)

## 1 Ellipsoid projection calculating for each mesh

**�ر�Ҫע�⣺** ������Ʒ���������ͶӰ�㷨��ͷ�沿����ģ�͵�����ͶӰ�㷨**�ǲ�ͬ�ģ�**
* ���ߵ�ͶӰ���Ĳ�ͬ
* ����ͶӰû�б߾�����

[��ת������ͶӰ](#evaluating_projection)

The original version of ellipsoid projection is implemented in 
MainWindow\:\:docSphericalProject ( ) at the top of the file 
mainwindow_RunTime_1.cpp. A adapted version for batch training is 
MainWindow\:\:ellipsoidProject_single_file ( QString fileName ) in ln92 of the 
file mainwindow_RunTime_2.cpp.

���°汾������ȡ������ CMgrPatches����Ϊֱ���� MeshModel ������ doProjection ( ) 
����������ͶӰ���㡣���õ�λ��λ�� mainwindow_RunTime_2.cpp �� ln225:
```cpp
	pMM->doProjection ( );
```
����Ĵ���ʽ�Ѿ����ϳ���
> �ڵ�ǰ�İ汾�����ǲ��� CMgrPatches �������Ƭ��ͶӰ��
> ```cpp
> CMgrPatches *	pMgr	= new CMgrPatches( );
> ```
>
> ��ʵ�ϣ�CMgrPatches ������� [CPatchesRowO](../meshLab/mlMeshType/CPatchesRowO.md#class-cpatchesrowo)��
> ����������Ƭ [CPatchO](../meshLab/mlMeshType/CPatchO.md#class-cpatcho)��

�� CMeshO ������������
```cpp
	PatchesRowContainer patches_rows;
```

ÿ����Ƭ��ͶӰ����λ�� meshmodel_1.cpp �� ln65:
```cpp
void MeshModel::doProject (CPatchO *pPatch)
```

### 1.1 ���� patches_row

theta ���� z ��(�� y ��ת������)�ļн�, ��ΧΪ [0, PI], cos_theta ���ϵ����� 1
�𽥼�������� -1�����Ƕ� cos_theta �����½ض�, ʹ�� trunc_cosInterval.arr ����ȡֵ��



### 1.2 Ϊÿ�� CPatchesRowO ������ݳ�Ա

phi �Ƕ����� x ��(�� z ��ת������)�ļн�, ��Χ [-PI, PI], ͷ�沿ǰ�沿�ֶ�Ӧ�ķ�
ΧΪ [-PI/2, PI/2. ��ʱ sin_phi �ı仯��ΧΪ [-1, 1]

<span id="ellipsoid_projection"></span>
### 1.3 �Ż� MainWindow\:\:ellipsoidProject_single_file ( QString fileName )

������ͶӰҪ����������ļ������������⡣

���� MainWindow::findPoints ( ... ) λ�� mainwindow_RunTime.cpp �Ķ�����
�ú����ǵ�ǰ�������ܼ����½��Ĺؼ����ҵ�ǡ���ķ���ͶӰ�㷨������������ܵĹؼ���
��˽�������Ŀ���ǣ�

* ���Ʒ���ͶӰ�㷨�Ķ�����������
* ���ö��߳���ʵ���㷨

### 1.4 ������������

�������������ÿ�������ָ���¼�ڷ��������С�
������� vector< > ������Ҫע���ڴ�ռ�ã���ط������¼�
[��ά vector �ڴ�ռ���������](../cpp/vector.md)��

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

#### 1.4.1 ���ȳ��Ե���ƽ����������������㲢���൫Ч���ܲ�

�ʼ����ʹ�õ���ƽ����������Ե��ƶ�����з�����
����������ģ��μ���binarySearchTree.hpp��
ʹ�õĹ��̲μ���mainwindow_RumTime_1.cpp �е� MainWindow::docSphericalProject ( )��
�Ǹ� avlTree ����ƽ������������Ч���ܲ����ԭ��δ��ϸ̽��������ԭ�������ڶ�����̫��
���¶�����̫���ڴ�����̫����ʹ�㷨������������ԭ��δ֪��ֻ������ debug �汾��
û׼ release �汾û�����⣩��

���ڸ��㷨��������Ŀ�Ļ����㷨������ release ���� debug ��Ҫ���������ã�
������Ǿ�������˼�������������⣬���˴���ʱ����д������֡�
ϣ��δ��Ч���ܵõ��ϴ�������

### 1.5 ����� PATCH ���಻���˲������ȼ�����

����ÿһ������������Ƕ�ȥ����ͷ�󲿶��㣬���һ��� margin��
�������ȼ������޷������һ���ֱ�ȥ���Ķ��㡣������������һ���ֲ� PATCH �ۺ��㷨��
�㷨�Ļ���˼�������ж�������һ PATCH �㣬����ÿһ���ж������ĸ� PATCH��
ÿ�� PATCH �ж���һ�� vector ��¼�������Ķ��㡣

**������CMgrPatches���ڶ�PATCH�Ĺ���**

#### 1.5.1 CMgrPatches

���룺ͷ�沿���ƣ�ȥ�����账��Ĳ��֣���

���������ͶӰ������������Ľ�������ڱǴ��ڲ��֣��迼�Ƕ���ǡ�������ݽṹ�����������

�������������������������ȫ�������������ֻ��ȷ��ÿ�����������ĸ�������
Ȼ��Է����������򼴿ɡ�Ϊ�˸��õؼ��㷨���������潫��������� CPatchRow �ͷ���
CPatch�����ǽ��ȶԷ����н��������ٶ�ÿ���������еķ�����������

#### 1.5.2 CPatchesRow

Patch �й���������ĵ��Ʒ��ɵ�ǡ���� patch �С�

#### 1.5.3 CPatch

ÿ�� patch �к������ɵ��ƶ������ݣ����ڷ���ͶӰ��


<span id="evaluating_projection"></span>
### 1.6 ����ͶӰ MainWindow\:\:ellipsoidProject_evaluating_file ( QString fileName )

**����ע������ͶӰ�����ͶӰ�Ĳ��죡**

[��ת������ͶӰ](#ellipsoid_projection)




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
The corresponding slot function is in ln938 of mainwindow_RunTime_2.cpp.
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


### 2.3 Storage of the projection results

* Ӧ��Ŀ¼�½����洢Ŀ¼���洢�Ѿ����ѵ����Ŀ¼�嵥
    * ѡ��ѵ��Ŀ¼�󽫸�Ŀ¼��ӵ��洢�嵥 ( see ln974 of mainwindow_RunTime_2.cpp )
* ����Ŀ¼�½�����Ŀ¼�������ļ���
	* proj_results.mat �д洢���ݽ��
    * proj_list.txt �д洢�Ѿ����ɹ�������ļ��嵥

### 2.4 Synthesis

ln463 of mainwindow_Runtime_3.cpp

* ����ѵ��Ŀ¼�嵥
* ��������ѵ����¼����ͶӰ��¼
* �γ��ۺ�������¼

* �����׼����
* ��֯��׼���ݣ����������ݣ�

## 3. Evaluation

* ����ͶӰ����
* �Ա���������
* �Աȱ�׼����
* ���׼���ݻ���

## 4. displaying evaluating results

For messages displaying, please refer to [GLArea](../meshLab/GLArea/GLArea.md)

<span id="jump">Hello World</span>

[jump to the top](#top)
