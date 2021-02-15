# Optional Component

<http://vcg.isti.cnr.it/~cignoni/newvcglib/html/optional_component.html>

**************************************

There are many cases where some vertex or face attributes (like for example the 
Face-Face adjacency) are not necessary all the time. VCG Lib gives you a way to 
specify optional components, i.e. attributes that are not statically stored 
within the simplex but can be dynamically allocated only when you need them. 
Their use is totally transparent with respect to the standard, statically 
allocated components.

To 'define' optional component you need to do two things:

* to use a special type of container (derived from std::vector) in the mesh 
definition
* to specify the right type of component in the simplex definition

An optional component can be used only when it is 'enabled', e.g. when the 
memory for it has been allocated. An optional component can be enable by calling 
the function Enable. VCG Lib handles optional components with two alternative 
mechanisms: the first is called 'Ocf' (for Optional Component Fast) which uses 
one pointer for each simplex but it makes accessing optional attribute almost 
as fast as non-optional ones; the second is called 'Occ' (for Optional Component 
Compact) which only requires a little extra space for each mesh (unrelated to its 
size in terms of number of simplicies) but which can result in a slower access. 
In the following, only their use is discussed. The implementation detail can be 
found here.

## Optional Component Fast
The following definition of MyMesh specifies that a few components of the vertex 
and face types are optional. Note that the special attribute 
vcg\:\:vertex\:\:InfoOcf and vcg\:\:face\:\:InfoOcf are the first attribute 
of vertex and face types and that we use the vcg\:\:vertex\:\:vector_ocf and 
vcg\:\:face\:\:vector_ocf as containers;
```cpp
class MyVertexOcf;
class MyFaceOcf;

struct MyUsedTypesOcf : public vcg::UsedTypes<
    vcg::Use<MyVertexOcf>::AsVertexType,
    vcg::Use<MyFaceOcf>::AsFaceType> {};

class MyVertexOcf : public vcg::Vertex< MyUsedTypesOcf,
    vcg::vertex::InfoOcf,   //   <--- Note the use of the 'special' InfoOcf component
    vcg::vertex::Coord3f,   vcg::vertex::QualityfOcf,
    vcg::vertex::Color4b,   vcg::vertex::BitFlags,
    vcg::vertex::Normal3f,  vcg::vertex::VFAdjOcf > {};

class MyFaceOcf : public vcg::Face< MyUsedTypesOcf,
    vcg::face::InfoOcf,     //   <--- Note the use of the 'special' InfoOcf component
    vcg::face::FFAdjOcf,    vcg::face::VFAdjOcf,
    vcg::face::Color4bOcf,  vcg::face::VertexRef,
    vcg::face::BitFlags,    vcg::face::Normal3fOcf > {};

// the mesh class must make use of the 'vector_ocf' containers instead of the classical std::vector
class MyMeshOcf : public vcg::tri::TriMesh< vcg::vertex::vector_ocf<MyVertexOcf>, vcg::face::vector_ocf<MyFaceOcf> > {};
```

Before accessing the data contained in the optional component you must enable 
that attribute by calling EnableXXX() corresponding function; afterwards the 
space for the component will be allocated and accessible until you call again 
DisableXXXX().
```cpp
    MyMeshOcf cmof;

    assert(tri::HasFFAdjacency(cmof) == false);
    cmof.face.EnableFFAdjacency();
    assert(tri::HasFFAdjacency(cmof) == true);
```

Trying to access the value of a unallocated component before enabling or after 
disabling the corresponding component will throw an assertion.
```cpp
    cmof.face.EnableNormal();  // remove this line and you will throw an exception for a missing 'normal' component
    tri::UpdateNormal<MyMeshOcf>::PerVertexPerFace(cmof);
```

### See Also
[trimesh_optional.cpp](http://vcg.isti.cnr.it/~cignoni/newvcglib/html/trimesh__optional_8cpp.html)
