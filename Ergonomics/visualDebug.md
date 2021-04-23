# Visual Debugging


****************************

## 1. Showing Results of Patches Rows


## 2. Rendering Vertices with Different Colors

�����Ҳ�ķֲ㽻�������ʼ��λ�ڣ�ml_render_gui.cpp �����ʼ������Ϊ ln605
```cpp
void MLRenderingPointsParametersFrame::initGui( )
```
���� ln623 ��ʼ��һ�δ�������������Ӧ�� _colortool ��� toolbar
```cpp
    _colorlab = new QLabel("Color",this);
    _colorlab->setFont(boldfont);
    layout->addWidget(_colorlab,1,0,Qt::AlignLeft);
    _colortool = new MLRenderingToolbar(_meshid,this);
    _colortool->setToolButtonStyle(Qt::ToolButtonTextOnly);
    _colortool->addRenderingAction(new MLRenderingPerVertexColorAction(MLRenderingData::PR_POINTS,_meshid, _colortool));
    _colortool->addRenderingAction(new MLRenderingPerMeshColorAction(MLRenderingData::PR_POINTS,_meshid, _colortool));
    _colortool->addRenderingAction(new MLRenderingUserDefinedColorAction(MLRenderingData::PR_POINTS, _meshid, _colortool));
```
������Ҫ���ٵ������һ���û��Զ�����ɫ�����Ӱ�쵱ǰ mesh �Զ��������ɫ��Ⱦ�ġ�
�����������Ƿ��ָý��潻���ı���� PerViewData��
�ƺ���ֻ�Ƕ�ÿ���ӽ�����ɫ�ı䣬�����ǵ��ڴ���һ�¡�
��һ���Ƕ� ply �ĵ�����̽���׷�٣���������ɫ����α�����ġ�

ע�⵽��û����ɫ��Ϣ�� ply �ļ�����Ⱦ�������ã�
�պ������Ҫ�� ply �ĵ�����̽��и�Ԥʹ�ú�������֧�ֶ�����ɫ�ĸı䡣

## 3. Loading procedure of ply file

MainWindow\:\:importSingleMesh ( QString fileName ) in ln121 of the 
file mainwindow_RunTime_3.cpp.

���յ��õ��� MeshLab\\src\\meshlabplugins\\io_base\\baseio.cpp ln85 ��
```cpp
bool BaseMeshIOPlugin::open(const QString &formatName, const QString &fileName, MeshModel &m, int& mask, const RichParameterSet &parlst, CallBackPos *cb, QWidget * /*parent*/)
```
�ú����ֱ��� ln105 �� ln126 ���� ply �� stl ��ʽ�����롣
������ ln112 �� ln135 ������������Ĵ��������䶥��Ķ������Ӷ���ɫ��֧��
```cpp
	/// !!! added by jicheng
	m.Enable (tri::io::Mask::IOM_VERTCOLOR);
```
�����յ���
```cpp
    updateDataMask(MM_VERTCOLOR);
```
��ɶ������ɫ��֧�֡�



