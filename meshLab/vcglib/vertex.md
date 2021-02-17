# Derivation chain to define class Vertex

建议阅读 [TriMesh 的关键数据成员 vert](vertTriMesh.md)

*********

我们都知道三维数据最重要的是定义好顶点 vertex 和网格 mesh，为了避免使用虚基类及产生歧义，
vcglib 在定义 vertex 等重要数据结构时，几乎都采用了非常抽象且非常长的派生链（derivation 
chain）技术来进行。其所采用的方法非常值得初学者学习，最好参照学习 
[virtual base class](../../cpp/virtualClass.md)。

## 1 VertexArityMax

This is the base class form which we start to add our components.
It has the empty definition for all the standard members (coords, color flags).

**Note:**
In order to avoid both virtual classes and ambiguous definitions all 
the subsequent overrides must be done in a sequence of derivation.

In other words we cannot derive and add in a single derivation step 
(with multiple ancestor), both the real (non-empty) normal and color, but 
we have to build the type a step a time (deriving from a single ancestor at a time).
也就是说不能用多重继承，这即是所谓 **单参数派生链**。 

The Real Big Vertex class;

The class __VertexArityMax__ is the one that is the Last to be derived,
and therefore is the only one to know the real members 
(after the many overrides) so all the functions with common behaviour 
using the members defined in the various Empty/nonEmpty component classes 
MUST be defined here. 

I.e. IsD() that uses the overridden Flags() member must be defined here.

```cpp
template <class UserTypes,
          template <typename> class A, template <typename> class B,
          template <typename> class C, template <typename> class D,
          template <typename> class E, template <typename> class F,
          template <typename> class G, template <typename> class H,
          template <typename> class I, template <typename> class J,
          template <typename> class K, template <typename> class L>
class VertexArityMax: public Arity12<vertex::EmptyCore<UserTypes>, A, B, C, D, E, F, G, H, I, J, K, L> 
{
// ----- Flags stuff -----
public:
 	enum { 
		DELETED    = 0x0001,		// This bit indicate that the vertex is deleted from the mesh
		NOTREAD    = 0x0002,		// This bit indicate that the vertex of the mesh is not readable
		NOTWRITE   = 0x0004,		// This bit indicate that the vertex is not modifiable
		MODIFIED   = 0x0008,		// This bit indicate that the vertex is modified
		VISITED    = 0x0010,		// This bit can be used to mark the visited vertex
		SELECTED   = 0x0020,		// This bit can be used to select 
		BORDER     = 0x0100,    // Border Flag
		USER0      = 0x0200			// First user bit
    };
 	
    bool IsD() const {return (this->cFlags() & DELETED) != 0;} ///  checks if the vertex is deleted
    bool IsR() const {return (this->cFlags() & NOTREAD) == 0;} ///  checks if the vertex is readable
    bool IsW() const {return (this->cFlags() & NOTWRITE)== 0;}///  checks if the vertex is modifiable
    bool IsRW() const {return (this->cFlags() & (NOTREAD | NOTWRITE)) == 0;}/// This funcion checks whether the vertex is both readable and modifiable
    bool IsS() const {return (this->cFlags() & SELECTED) != 0;}///  checks if the vertex is Selected
    bool IsB() const {return (this->cFlags() & BORDER) != 0;}///  checks if the vertex is a border one
    bool IsV() const {return (this->cFlags() & VISITED) != 0;}///  checks if the vertex Has been visited

...

}
```

上述 Flags() 来自父类 vertex::EmptyCore，返回的是该函数中的静态变量 dummyflags(0) 的引用。
**由于类的函数中的静态变量是全局的，因此该 Flags() 返回的结果是所有对象所共享的!**


## 2 Vertex

These are the three main classes that are used by the library user to define its 
own vertices.
The user MUST specify the names of all the type involved in a generic complex.
so for example when defining a vertex of a trimesh you must know the name of the 
type of the edge and of the face.
Typical usage example:

A vertex with coords, flags and normal for use in a standard trimesh:
```cpp
class VertexNf   : public VertexSimp2< VertexNf, EdgeProto, FaceProto, vert::Coord3d, vert::Flag, vert::Normal3f  > {};
```

A vertex with coords, and normal for use in a tetrahedral mesh AND in a standard trimesh:
```cpp
class TetraVertex   : public VertexSimp3< TetraVertex, EdgeProto, FaceProto, TetraProto, vert::Coord3d, vert::Normal3f  > {};
```

A summary of the available vertex attributes (see component.h for more details):
```cpp          
Coord3f,  Coord3d, 
Normal3s,  Normal3f,  Normal3d
Mark                              // a int component (incremental mark)
BitFlags
TexCoord2s,  TexCoord2f,  TexCoord2d
Color4b
Qualitys, Qualityf, Qualityd
VFAdj                             // topology (vertex->face adjacency)
```

### 2.1 Definition and TypesPool

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

adjacency [ə'dʒeɪsənsɪ]


