# TriMesh 的关键数据成员 vert

建议阅读 [Derivation chain to define class Vertex](vertex.md)

*************************

## 1 TriMesh\:\:vert 是 std\:\:vector<...>

类模板 TriMesh 是 vcglib 最关键的数据结构之一，
在这里我们将分析 TriMesh 的关键数据成员 vert 的定义过程。

首先 vert 的类型是 TriMesh::VertContainer，这个类型在结构体 BaseMeshTypeHolder 
中被定义。结构体 BaseMeshTypeHolder 的定义非常有趣，它没有任何数据，
只含有类型的定义，也就是说仅仅就是一个 TypeHolder。
联想到 Bruce McGauphy 曾经告诉我可以用类中 static 函数来定义全局函数的 FunctionHolder，
二者同工异曲之妙。日常编程实践中很少采用这些 Holder，今后要多多使用这种奇妙的方式，
增强代码的抽象性。C++ 的精彩就在于有无穷无尽的这种殊能异技来表达高层的抽象思维，
加强用 C++ 表达抽象思维能力是技术进阶的关键。

BaseMeshTypeHolder 的定义位于 vcglib\vcg\complex\base.h 的 ln58。
```cpp
template <class TYPESPOOL>
struct BaseMeshTypeHolder{
	typedef std::vector< typename TYPESPOOL::VertexType >	CONTV;

	typedef CONTV						VertContainer;
```
**整个解决方案中BaseMeshTypeHolder只在定义TriMesh时作为模板参数被使用。**

由此可知 VertContainer 其实就是 std\:\:vector\< typename TYPESPOOL::VertexType \>。

## 2 类模板 TriMesh 与 TriMesh\:\:vert 相关的参数

通过上面的分析我们知道要弄清楚 vert 就必须搞清楚与之相关的模板参数。先来看定义：
```cpp
template < 
	class Container0 = DummyContainer, 
	class Container1 = DummyContainer, 
	class Container2 = DummyContainer, 
	class Container3 = DummyContainer, 
	class Container4 = DummyContainer >
class TriMesh
        : public  MArity5<
			BaseMeshTypeHolder<typename Container0::value_type::TypesPool>, 
			Container0, Der, 
			Container1, Der, 
			Container2, Der, 
			Container3, Der, 
			Container4, Der >
{
public:

};
```
从中我们可以看到 BaseMeshTypeHolder\<typename Container0\:\:value_type\:\:TypesPool\> 
使用的模板参数为 TriMesh 的第一个模板参数 class Container0 = DummyContainer。
而 MArity5 的第一个模板参数其实就是整个双模板参数派生链（chain with 2 template 
arguments on the derivers）的基类第一个模板参数 A\< Base, TA \>，这个可以从 
vcglib\vcg\container\derivation_chain.h 的 ln138 开始跟踪得出结论。
仔细观察这个双参数派生链，第 3 个模板参数是含两个参数的类模板，它是这个派生链的基类。
而第 1 个模板参数是基类（第 3 个模板参数）的第一个模板参数。
**整个双参数派生链的每一个基类其实都是含两个参数的类模板**，
总以最后一个参数为基类（基类是双参数类模板），然后前面的其它参数递归形成基类的双参数。

在定义 CMeshO 时在作为类模板的 TriMesh 中使用的模板参数 Container0 为 
vcg\:\:vertex\:\:vector_ocf\<CVertexO\>，此时
```cpp
BaseMeshTypeHolder<typename Container0::value_type::TypesPool>
```
为
```cpp
BaseMeshTypeHolder<vector_ocf<CVertexO>::value_type::TypesPool>
```
亦即
```cpp
BaseMeshTypeHolder<CVertexO::TypesPool>
```

