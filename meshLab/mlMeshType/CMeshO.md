# Class CMeshO

## 1 CMeshO 是由 vcglib 中类模板 TriMesh 定义的模板类的子类

类 CMeshO 是 meshLab 中最重要数据的管理抽象，是模板类
vcg\:\:tri\:\:TriMesh\< vcg\:\:vertex\:\:vector_ocf\<CVertexO\>, vcg\:\:face\:\:vector_ocf\<CFaceO\> \>
的子类。我们知道 TriMesh 里的数据成员 vert 中存放的是点云数据，
是整个系统的核心数据。这里我们详细分析这些数据是如何通过双模板参数派生链被逐步定义的。

**关于TriMesh的详细分析参见**[**TriMesh的关键数据成员vert**](../vcglib/vertTriMesh.md)

TriMesh 可以赋5个模板参数（缺省都是 DummyContainer），而类 CMeshO 只使用了2个参数。
先分析第一个模板参数 vcg\:\:vertex\:\:vector_ocf\<CVertexO\>。

TriMesh 的父类模板为 MArity5，使用的参数 
BaseMeshTypeHolder\<typename Container0\:\:value_type\:\:TypesPool\> 
在现在的情况下为：
```cpp
BaseMeshTypeHolder< vcg::vertex::vector_ocf<CVertexO>::value_type::TypesPool >
```
如是需要追踪 vcg\:\:vertex\:\:vector_ocf\<VALUE_TYPE\> 的定义，
该类模板的定义位于 vcglib/vcg/simplex/vertex/component_ocf.h 中的 ln43。
