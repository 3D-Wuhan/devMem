# Class CFaceO

The definition of this class is at ln210 of MeshLab/src/common/ml_mesh_type.h:
```cpp
// Each face needs 32 byte, on 32bit arch. and 48 byte on 64bit arch.
class CFaceO : public vcg::Face< CUsedTypesO,
    vcg::face::InfoOcf,              /* 4b */
    vcg::face::VertexRef,            /*12b */
    vcg::face::BitFlags,             /* 4b */
    vcg::face::Normal3m,             /*12b */
    vcg::face::QualityfOcf,          /* 0b */
    vcg::face::MarkOcf,              /* 0b */
    vcg::face::Color4bOcf,           /* 0b */
    vcg::face::FFAdjOcf,             /* 0b */
    vcg::face::VFAdjOcf,             /* 0b */
    vcg::face::CurvatureDirmOcf,     /* 0b */
    vcg::face::WedgeTexCoordfOcf     /* 0b */
> {};
```
As you see this class is derived from class template vcg\:\:Face.


## 1 Class Template vcg\:\:Face

These are the three main classes that are used by the vcg library user to define 
its own Facees. The user MUST specify the names of all the type involved in a 
generic complex. So for example when defining a Face of a trimesh you must know 
the name of the type of the edge and of the face.

### 1.1 Definition of vcg\:\:Face

The definition of this class is at ln283 of vcglib/vcg/simplex/face/base.h:
```cpp
template <class UserTypes,
          template <typename> class A = DefaultDeriver, template <typename> class B = DefaultDeriver,
          template <typename> class C = DefaultDeriver, template <typename> class D = DefaultDeriver,
          template <typename> class E = DefaultDeriver, template <typename> class F = DefaultDeriver,
          template <typename> class G = DefaultDeriver, template <typename> class H = DefaultDeriver,
          template <typename> class I = DefaultDeriver, template <typename> class J = DefaultDeriver,
          template <typename> class K = DefaultDeriver, template <typename> class L = DefaultDeriver >
                            class Face: public FaceArityMax<UserTypes, A, B, C, D, E, F, G, H, I, J, K, L>  {
                            public: typedef AllTypes::AFaceType IAm; typedef UserTypes TypesPool;};
```

### 1.2 Typical usage example:

A Face with coords, flags and normal for use in a standard trimesh:
```cpp
class MyFaceNf : public FaceSimp2< VertProto, EdgeProto, MyFaceNf, face::Flag, face::Normal3f > {};
```

A Face with coords, and normal for use in a tetrahedral mesh AND in a standard 
trimesh:
```cpp
class TetraFace : public FaceSimp3< VertProto, EdgeProto, TetraFace, TetraProto, face::Coord3d, face::Normal3f > {};
```

A summary of the components that can be added to a face (see components.h for 
details):

VertexRef
NormalFromVert, WedgeNormal
Normal3s, Normal3f, Normal3d
WedgeTexCoord2s, WedgeTexCoord2f, WedgeTexCoord2d
BitFlags
WedgeColor, Color4b
Qualitys, Qualityf, Qualityd
Mark                                            //Incremental mark (int)
VFAdj                                           //Topology vertex face adjacency
                                                 (pointers to next face in the ring of the vertex
FFAdj                                           //topology: face face adj
                                                  pointers to adjacent faces


## 2 Class template FaceArityMax

The definition of this class is from ln84 to ln246 of vcglib/vcg/simplex/face/base.h:
```cpp
template < class UserTypes,
          template <typename> class A, template <typename> class B,
          template <typename> class C, template <typename> class D,
          template <typename> class E, template <typename> class F,
          template <typename> class G, template <typename> class H,
          template <typename> class I, template <typename> class J,
          template <typename> class K, template <typename> class L >
                    class FaceArityMax: public L<Arity11<FaceBase<UserTypes>, A, B, C, D, E, F, G, H, I, J, K> >
{
……
};
```
This class template is the **Last** to be derived, and therefore is the only one 
to know the real members (after the many overrides) so all the functions with 
common behaviour using the members defined in the various Empty/nonEmpty 
component classes MUST be defined here.

I.e. IsD() that uses the overridden Flags() member must be defined here.

### 2.1 Class template FaceBase
```cpp
template <class UserTypes>
class FaceBase: public
            face::EmptyCore< FaceTypeHolder <UserTypes> > { };
```
This is the base class form which we start to add our components. It has the 
empty definition for all the standard members (coords, color flags).

**Note:**

In order to avoid both virtual classes and ambiguous definitions all
the subsequent overrides must be done in a sequence of derivation.

In other words we cannot derive and add in a single derivation step
(with multiple ancestor), both the real (non-empty) normal and color but
we have to build the type a step a time (deriving from a single ancestor 
at a time).

### 2.2 Class template FaceTypeHolder

```cpp
template <class UserTypes>
                class FaceTypeHolder: public UserTypes
{
public:
    template <class LeftF>
    void ImportData(const LeftF & ){}
    static void Name(std::vector<std::string> & /* name */){}


// prot
    inline int VN()  const { return 3;}
    inline int Prev(const int & i) const { return (i+(3-1))%3;}
    inline int Next(const int & i) const { return (i+1)%3;}
    inline void Alloc(const int & ){}
    inline void Dealloc(){}
};
```
The base class of all the recusive definition chain. It is just a container of 
the typenames of the various simplexes.
These typenames must be known form all the derived classes.

## Class template vcg\:\:face\:\:EmptyCore

这个类模板主要以函数的静态局部变量的方式来定义一些全局属性，
例如：
```cpp
    typedef int FlagType;
    int &Flags() { static int dummyflags(0);  assert(0); return dummyflags; }
    int cFlags() const { return 0; }
    static bool HasFlags()   { return false; }

    typedef vcg::Color4b ColorType;
    ColorType &C()       { static ColorType dumcolor(vcg::Color4b::White);  assert(0); return dumcolor; }
    ColorType cC() const { static ColorType dumcolor(vcg::Color4b::White);  assert(0); return dumcolor; }
```
上述静态变量 dummyflags 与 dumcolor 分别用于保存标志位与颜色信息。

**EmptyCore这种截面技术的技巧在vcglib中被广泛使用，是模板meta-programming的常规技术。**
将来有空闲时再在metaProgramming中对宏及模板元编程技术进行详细总结。