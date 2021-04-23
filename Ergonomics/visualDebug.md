# Visual Debugging


****************************

## 1. Showing Results of Patches Rows


## 2. Rendering Vertices with Different Colors

界面右侧的分层交互界面初始化位于：ml_render_gui.cpp 。其初始化函数为 ln605
```cpp
void MLRenderingPointsParametersFrame::initGui( )
```
而自 ln623 开始的一段代码则是生成相应的 _colortool 这个 toolbar
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
我们需要跟踪的是最后一行用户自定义颜色是如何影响当前 mesh 对顶点进行颜色渲染的。
经过跟踪我们发现该界面交互改变的是 PerViewData，
似乎这只是对每个视进行颜色改变，与我们的期待不一致。
下一步是对 ply 的导入过程进行追踪，看顶点颜色是如何被导入的。

注意到对没有颜色信息的 ply 文件该渲染不起作用，
日后可能需要对 ply 的导入过程进行干预使得后续处理支持顶点颜色的改变。

## 3. Loading procedure of ply file

MainWindow\:\:importSingleMesh ( QString fileName ) in ln121 of the 
file mainwindow_RunTime_3.cpp.

最终调用的是 MeshLab\\src\\meshlabplugins\\io_base\\baseio.cpp ln85 的
```cpp
bool BaseMeshIOPlugin::open(const QString &formatName, const QString &fileName, MeshModel &m, int& mask, const RichParameterSet &parlst, CallBackPos *cb, QWidget * /*parent*/)
```
该函数分别在 ln105 和 ln126 处理 ply 和 stl 格式的输入。
我们在 ln112 和 ln135 处加上了下面的代码来扩充顶点的定义增加对颜色的支持
```cpp
	/// !!! added by jicheng
	m.Enable (tri::io::Mask::IOM_VERTCOLOR);
```
其最终调用
```cpp
    updateDataMask(MM_VERTCOLOR);
```
完成顶点对颜色的支持。



