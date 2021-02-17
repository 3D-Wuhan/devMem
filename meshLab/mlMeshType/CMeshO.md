# Class CMeshO

**关键结论：**
* CMeshO 中存储顶点的关键数据成员 vert 的容器类型是 std\:\:vector< CVertexO >
* CMeshO 中存储面的关键数据成员 face 的容器类型是 std\:\:vector< CFaceO >
* 需要在 CMeshO 中增加存储 patch 的数据成员，其容器类型 jicheng\:\:patch\:\:vector_ocf\<CPatchO\>
* 需要在 CPatchO 中增加储存 VertRef 的容器类型 

类 CMeshO 的定义位于 MeshLab/src/common/ml_mesh_type.h 最后部分。
由于需要增加对分块（patch）的支持，我们将对其进行适当的改写。

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

注意自 ln245 至 ln263，定义了 vcg\:\:vertex\:\:vector_ocf 的一些属性，
包括颜色、曲率、法向量、文字颜色等等。

### 1.1 TypesPool

我们现在来分析 BaseMeshTypeHolder\<typename Container0\:\:value_type\:\:TypesPool\> 
中的 TypesPool。按照 [vertex](../vcglib/vertex.md) 的 2.1 中的说法，
TypesPool 其实就是定义 Vertex 的第一个模板参数 UserTypes。
而对 CVertexO 来说这个模板参数就是 CUsedTypesO。
于是在我们更改 CUsedTypesO 为下面的定义后，TypesPool 现在就应该包含 CVertexO，
CEdgeO，CFaceO 和 CPatchO：
```cpp
class CUsedTypesO : public vcg::UsedTypes < UseO<CVertexO>::AsVertexType,
	UseO<CEdgeO>::AsEdgeType,
	UseO<CFaceO>::AsFaceType,
	UseO<CPatchO>::AsPatchType> {};
```

### 1.2 TriMesh\:\:VertContainer 其实就是 std\:\:vector< CUsedTypesO\:\:VertexType >

这样 CMeshO 的关键数据 vert 也就是属于 std\:\:vector< CVertexO > 类型。

在 vcglib/vcg/complex/base.h 的 ln58 中是如下定义 BaseMeshTypeHolder 的：
```cpp
template <class TYPESPOOL>
struct BaseMeshTypeHolder
{
	typedef std::vector< typename TYPESPOOL::VertexType > CONTV;
……
	typedef CONTV									VertContainer;
……
}；
```

而在 vcglib/vcg/complex/base.h 的 ln172 中
```cpp
template < class Container0 = DummyContainer, class Container1 = DummyContainer, class Container2 = DummyContainer, class Container3 = DummyContainer, class Container4 = DummyContainer >
class TriMesh
        : public  MArity5< BaseMeshTypeHolder<typename Container0::value_type::TypesPool>, Container0, Der ,Container1, Der, Container2, Der, Container3, Der, Container4, Der >{
```
又被用于定义 TriMesh。而对 CMeshO 来说 Container0 就是 
vcg\:\:vertex\:\:vector_ocf\<CVertexO\>，其 value_type 就是 CVertexO，
相应的 TypesPool 就是 CUsedTypesO。再回到上述 BaseMeshTypeHolder 的定义可知 
VertContainer 就是 std\:\:vector< CVertexO >。


### 1.3 增加 patch\:\:vector_ocf

为了能在 CMeshO 中增加对 CPatchO 的支持，首先必须定义好 patch\:\:vector_ocf。
定义的方法可以参考 vcg\:\:vertex\:\:vector_ocf 的定义。



## 2 在 CMeshO 中增加对 CPatchO 的支持

由于将来要用不同的颜色来表现不同的 patch 在评估时的效果，需要为 CPatchO 增加颜色支持。
按照 vcglib 的设计思路，最好的方式当然是在 patch namespace 中增加类模板 EmptyCore。

### 2.1 增加 jicheng\:\:patch\:\:EmptyCore



