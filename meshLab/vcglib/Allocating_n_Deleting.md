# Allocating and Deleting mesh elements

<http://vcg.isti.cnr.it/~cignoni/newvcglib/html/allocation.html>

******************************

## 1 Creating elements
To create a simple single triangle mesh or to add elements to an existing mesh 
you should use the AddVertices and AddFaces functions, elements are added at the 
end of the mesh. These functions returns a pointer to the first allocated element.

Adding element to a vector can cause reallocation, and therefore invalidation of 
any pointer that points to the mesh elements. These fucntion manage safely 
re-allocation and updating of pointers for all the pointers stored internally in 
the mesh (e.g. if you add some vertices and that causes a reallocation of the 
vertex vector, the pointers from faces to vertices will be automatically updated 
by the Allocator functions. **You should never directly reallocate or resize the 
vertex or face vectors**.
```cpp
class MyMesh : public vcg::tri::TriMesh< std::vector<MyVertex>, std::vector<MyFace> > {};
int main()
{
    MyMesh  m;
    MyMesh::VertexIterator  vi = vcg::tri::Allocator<MyMesh>::AddVertices(m,3);
    MyMesh::FaceIterator    fi = vcg::tri::Allocator<MyMesh>::AddFaces(m,1);

    MyMesh::VertexPointer   ivp[4];
    ivp[0]  = &*vi; vi->P() = MyMesh::CoordType ( 0.0, 0.0, 0.0); ++vi;
    ivp[1]  = &*vi; vi->P() = MyMesh::CoordType ( 1.0, 0.0, 0.0); ++vi;
    ivp[2]  = &*vi; vi->P() = MyMesh::CoordType ( 0.0, 1.0, 0.0); ++vi;
    fi->V(0)    = ivp[0];
    fi->V(1)    = ivp[1];
    fi->V(2)    = ivp[2];
```
look to [platonic.h](http://vcg.isti.cnr.it/~cignoni/newvcglib/html/platonic_8h_source.html) 
for more examples.

Alternatively you can add single vertex and faces in a more compact way as follows:
```cpp
    // Alternative, more compact, method for adding a single vertex
    ivp[3]  = &*vcg::tri::Allocator<MyMesh>::AddVertex(m,MyMesh::CoordType ( 1.0, 1.0, 0.0));
    // Alternative, more compact, method for adding a single face (once you have the vertex pointers)
    vcg::tri::Allocator<MyMesh>::AddFace(m, ivp[1],ivp[0],ivp[3]);
```
If you keep internally some pointers to the mesh elements adding elements can 
invalidate them. In that case you should pass to the vcg\:\:tri\:\:Allocator 
functions a PointerUpdater to be used to update your private pointers.
```cpp
    // a potentially dangerous pointer to a mesh element
    MyMesh::FacePointer fp = &m.face[0];
    vcg::tri::Allocator<MyMesh>::PointerUpdater<MyMesh::FacePointer> pu;

    // now the fp pointer could be no more valid due to eventual re-allocation of the m.face vector.
    vcg::tri::Allocator<MyMesh>::AddVertices(m,3);
    vcg::tri::Allocator<MyMesh>::AddFaces(m,1,pu);

    // check if an update of the pointer is needed and do it.
    if( pu.NeedUpdate() )   pu.Update(fp);
```

## 2 Destroying Elements
The library adopts a Lazy Deletion Strategy i.e. the elements in the vector that 
are deleted are only flagged as deleted, but they are still there.

Note that the basic deletion functions are very low level ones. They simply mark 
as deleted the corresponding entries without affecting the rest of the structures. 
So for example if you delete a vertex with these structures without checking that 
all the faces incident on it have been removed you could create a non consistent 
situation. Similarly, but less dangerously, when you delete a face its vertices 
are left around so at the end you can have unreferenced floating vertices. Again 
you should never, ever try to delete vertexes by yourself by directly flagging 
deleted, but you must call the Allocator utility function. The following snippet 
of code shows how to delete a few faces from an icosahedron.
```cpp
    // Now fill the mesh with an Icosahedron and then delete some faces
    vcg::tri::Icosahedron(m);
    vcg::tri::Allocator<MyMesh>::DeleteFace( m, m.face[1] );
    vcg::tri::Allocator<MyMesh>::DeleteFace( m, m.face[3] );
```
After such a deletion the vector of the faces still contains 20 elements (number 
of faces of an icosahedron) but the m.FN() function correctly reports 18 faces. 
Therefore if your algorithm performs deletions it happens that the size of a 
container could be different from the number of valid element of your meshes:
```cpp
m.vert.size() != m.VN()
m.face.size() != m.FN()
```
Therefore when you scan the containers of vertices and faces you could encounter 
deleted elements so you should take care with a simple !IsD() check:
```cpp
    // If you loop in a mesh with deleted elements you have to skip them!
    MyMesh::CoordType b(0,0,0);
    for(fi = m.face.begin(); fi!=m.face.end(); ++fi )
    {
        if( !fi->IsD() ) //    <---- Check added
        {
            b   += vcg::Barycenter(*fi);
        }
    }
```
In some situations, particularly when you have to loop many many times over the 
element of the mesh without deleting/creating anything, it can be practical and 
convenient to get rid of deleted elements by explicitly calling the two garbage 
collecting functions:
```cpp
    vcg::tri::Allocator<MyMesh>::CompactFaceVector(m);
    vcg::tri::Allocator<MyMesh>::CompactVertexVector(m);
```
After calling these function it is safe to not check the IsD() state of every 
element and always holds that:
```cpp
m.vert.size() == m.VN()
m.face.size() == m.FN()
```
**Note**

> If there are no deleted elements in your mesh, the compactor functions returns 
> immediately (it just test that the container size match the number of the 
> elements) so it is safe to call it before lenghty operations.
> 
> If you are messing around with deleted elements it is rather dangerous to loop 
> over mesh elements using FN() or VN() as end guards in the for; the following 
> code snipped is WRONG:

```cpp
    // WRONG WAY of iterating: FN() != m.face.size() if there are deleted elements
    for(int i=0;i<m.FN();++i)
    {
        if( !fi->IsD() )
        {
            b += vcg::Barycenter(*fi);
        }
    }
```

## 3 How to copy a mesh

Given the intricate nature of the mesh itself it is severely forbidden any 
attempt of copying meshes as simple object. To copy a mesh you have to use the 
Append utility class:
```cpp
    MyMesh  m2;
    vcg::tri::Append<MyMesh,MyMesh>::MeshCopy(m2,m);
```
In the vcg\:\:tri\:\:Append utility class there are also functions for copying 
only a subset of the second mesh (the selected one) or to append the second mesh 
to the first one.

