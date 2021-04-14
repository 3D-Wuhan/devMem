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
ע�⵽��û����ɫ��Ϣ�� ply �ļ�����Ⱦ�������ã�
�պ������Ҫ�� ply �ĵ�����̽��и�Ԥʹ�ú�������֧�ֶ�����ɫ�ĸı䡣
