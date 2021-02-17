# Class CVertexO

类 CVertexO 使用 vcglib 中的类模板 Vertex 来定义。

## 1 CVertexO\:\:TypesPool 其实就是 CUsedTypesO

先来回顾一下 vcglib 对类模板 Vertex 的定义（vcglib/vcg/simplex/vertex/base.h 的 ln183）：
```cpp
template <class UserTypes,
          template <typename> class A = DefaultDeriver, template <typename> class B = DefaultDeriver,
          template <typename> class C = DefaultDeriver, template <typename> class D = DefaultDeriver,
          template <typename> class E = DefaultDeriver, template <typename> class F = DefaultDeriver,
          template <typename> class G = DefaultDeriver, template <typename> class H = DefaultDeriver,
					template <typename> class I = DefaultDeriver, template <typename> class J = DefaultDeriver,
					template <typename> class K = DefaultDeriver, template <typename> class L = DefaultDeriver>
							class Vertex: public VertexArityMax<UserTypes, A, B, C, D, E, F, G, H, I, J, K, L>  
{
public:
typedef AllTypes::AVertexType   IAm;
typedef UserTypes               TypesPool;
};
```
可以看到第一个模板参数 UserTypes 被重新定义为 TypesPool，也就是说作为类模板 Vertex 
的模板类的子类的 CVertexO，其 TypesPool 其实就是第一个模板参数，
对于 CVertexO 来说这个 TypesPool 就是 CUsedTypesO。

再来看 CVertexO 的定义（MeshLab/src/common/ml_mesh_type.h 中 ln94）：
```cpp
class CVertexO  : public vcg::Vertex< CUsedTypesO,
    vcg::vertex::InfoOcf,           /*  4b */
    vcg::vertex::Coord3m,           /* 12b */
    vcg::vertex::BitFlags,          /*  4b */
    vcg::vertex::Normal3m,          /* 12b */
    vcg::vertex::Qualityf,          /*  4b */
    vcg::vertex::Color4b,           /*  4b */
    vcg::vertex::VFAdjOcf,          /*  0b */
    vcg::vertex::MarkOcf,           /*  0b */
    vcg::vertex::TexCoordfOcf,      /*  0b */
    vcg::vertex::CurvaturefOcf,     /*  0b */
    vcg::vertex::CurvatureDirmOcf,  /*  0b */
    vcg::vertex::RadiusmOcf         /*  0b */>
{
……
}
```
第一个模板参数为 CUsedTypesO，下面详细对其进行分析。


## 2 CUsedTypesO

我们将采用 vcglib 中的类模板 vcg\:\:UsedTypes 来定义 meshLab 的类 CUsedTypesO。
下面将先分析这个类模板的定义过程，然后定义类 CUsedTypesO。

### 2.1 vcg\:\:UsedTypes

vcg\:\:UsedTypes 的定义如下：
```cpp
template <class A>
struct Use
{
    template <class T> struct AsVertexType : public T
    {
        typedef A   VertexType;
        typedef VertexType *    VertexPointer;
    };

    template <class T> struct AsEdgeType : public T
    {
        typedef A   EdgeType;
        typedef EdgeType *  EdgePointer;
    };

    template <class T> struct AsFaceType : public T
    {
        typedef A   FaceType;
        typedef FaceType *  FacePointer;
    };

    template <class T> struct AsTetraType : public T
    {
        typedef A   TetraType;
        typedef TetraType * TetraPointer;
    };

    template <class T> struct AsHEdgeType : public T
    {
        typedef A   HEdgeType;
        typedef HEdgeType * HEdgePointer;
    };
};

template <template <typename> class A = DefaultDeriver, template <typename> class B = DefaultDeriver,
                    template <typename> class C = DefaultDeriver, template <typename> class D = DefaultDeriver,
                    template <typename> class E = DefaultDeriver, template <typename> class F = DefaultDeriver,
                    template <typename> class G = DefaultDeriver, template <typename> class H = DefaultDeriver >
class UsedTypes : public Arity13<DummyTypes,
    Use< Vertex<UsedTypes< A, B, C, D , E, F, G, H > > > :: template AsVertexType,
    Use< Edge<UsedTypes< A, B, C, D , E, F, G, H > > > :: template AsEdgeType,
    Use< Face<UsedTypes< A, B, C, D , E, F, G, H > > > :: template AsFaceType,
    Use< TetraSimp<UsedTypes< A, B, C, D , E, F, G, H > > > :: template AsTetraType,
    Use< HEdge<UsedTypes< A, B, C, D , E, F, G, H > > > :: template AsHEdgeType,
    A, B, C, D, E, F, G, H>
{ };
```

Note that AsVertexType is a nested structure template, and that the structure 
template vcg\:\:use is the enclosing template. In fact AsVertexType is an empty 
structure template, which can be considered as a "typeholder" since it does 
only define VertexType to be the type of template argument of the structure 
template vcg\:\:use.

### 2.2 Expand the definition of CUsedTypesO

注意到 vcg::Use 中不含 AsPatchType，我们不好直接修改 vcglib 的原始文件，
那样会影响将来对 vcglib 版本升级后的支持。因此采用定义其模板子结构 UseO 来增加相应的 
AsPatchType。具体的定义方式参见 ml_mesh_type.h 中 ln86 to ln 97：
```cpp
template <class A>
struct UseO : public vcg::Use<A>
{
	template <class T> struct AsPatchType : public T
	{
		typedef A	PatchType;
		typedef PatchType * PatchPointer;
	};
};
```

原始的 CUsedTypesO 的定义如下：
```cpp
class CUsedTypesO: public vcg::UsedTypes < vcg::Use<CVertexO>::AsVertexType,
    vcg::Use<CEdgeO>::AsEdgeType,
    vcg::Use<CFaceO>::AsFaceType >{};
```

修改后的 CUsedTypesO 定义位于 MeshLab/src/common/ml_mesh_type.h 中 ln103：
```cpp
class CUsedTypesO : public vcg::UsedTypes < UseO<CVertexO>::AsVertexType,
	UseO<CEdgeO>::AsEdgeType,
	UseO<CFaceO>::AsFaceType,
	UseO<CPatchO>::AsPatchType> {};
```

经过上述步骤我们将 CPatchO 的类型加入到 CUsedTypesO 中了，方便后续使用。


