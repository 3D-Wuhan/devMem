# Class CVertexO

�� CVertexO ʹ�� vcglib �е���ģ�� Vertex �����塣

## 1 CVertexO::TypesPool

�����ع�һ�� vcglib ����ģ�� Vertex �Ķ��壨vcglib/vcg/simplex/vertex/base.h �� ln183����
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
���Կ�����һ��ģ����� UserTypes �����¶���Ϊ TypesPool��Ҳ����˵��Ϊ��ģ�� Vertex 
��ģ���������� CVertexO���� TypesPool ��ʵ���ǵ�һ��ģ�������

������ CVertexO �Ķ��壨MeshLab/src/common/ml_mesh_type.h �� ln94����
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
����
}
```
��һ��ģ�����Ϊ CUsedTypesO��������ϸ������з�����


## 2 CUsedTypesO

���ǽ����� vcglib �е���ģ�� vcg\:\:UsedTypes ������ meshLab ���� CUsedTypesO��
���潫�ȷ��������ģ��Ķ�����̣�Ȼ������ CUsedTypesO��

### 2.1 vcg\:\:UsedTypes

vcg\:\:UsedTypes �Ķ������£�
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


CUsedTypesO �Ķ���λ�� MeshLab/src/common/ml_mesh_type.h �� ln85��
```cpp
class CUsedTypesO: public vcg::UsedTypes < vcg::Use<CVertexO>::AsVertexType,
    vcg::Use<CEdgeO>::AsEdgeType,
    vcg::Use<CFaceO>::AsFaceType >{};
```
