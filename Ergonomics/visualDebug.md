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
注意到对没有颜色信息的 ply 文件该渲染不起作用，
日后可能需要对 ply 的导入过程进行干预使得后续处理支持顶点颜色的改变。
