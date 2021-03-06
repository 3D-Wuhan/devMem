# Bit Flags

<http://vcg.isti.cnr.it/~cignoni/newvcglib/html/flags.html>

************************************

For each simplex of the mesh we have a component called BitFlags that store a 
fixed size vector of 32 bits that are used for a variety of needs. Related 
classes to manage these flags:
* vcg\:\:tri\:\:UpdateFlags (defined in [flag.h](http://vcg.isti.cnr.it/~cignoni/newvcglib/html/flag_8h_source.html))
* vcg\:\:tri\:\:UpdateSelection (defined in [selection.h](http://vcg.isti.cnr.it/~cignoni/newvcglib/html/selection_8h_source.html))

## Delete Bit

```cpp
    IsD();
    ClearD();   // deprecated
    SetD();     // deprecated
```
This bit flag is the most used one; see 
[Allocating and Deleting mesh elements](Allocating_n_Deleting.md) 
page for more detail on its use.

> **Warning**
> 
> you should **never** directly invoke SetD() or ClearD(), but you should use 
> the function described in the 
> [vcg\:\:tri\:\:Allocator](http://vcg.isti.cnr.it/~cignoni/newvcglib/html/classvcg_1_1tri_1_1Allocator.html).


## Border Bit

```cpp
    IsB();
    ClearB();
    SetB();
```
To store the fact that a vertex or a face is on the boundary. These bits are not 
computed by default. They can be computed with or without the topology (obviously 
if you FF topology the computation of these bit is faster).

The rationale of these bits is that there are a lot of algorithms that need only 
boundary information for correctly working so computing it once is sufficient 
without needing to burden everything with the whole FF topology structure.

## Selection bit

```cpp
    IsS();
    ClearS();
    SetS();
```

## Visiting bit

```cpp
    IsV();
    ClearV();
    SetV();
```

Useful bit for any algorithm. You should not rely on the state of this bit or 
store anything persistent here. Any algorithm can clear it and use it for its 
own local purposes. use 
tri\:\:UpdateFlags\<YourMeshClass\>::VertexClearV(yourMesh) to clear all bits.

## User bits

If you want to allocate some private bits among the one stored with your mesh 
elements you can do it by mean of the NewBitFlag() function that returns a mask 
that can be used for setting, clearing and testing elements against that 
particular bit. The following example allocate three new bits for each face and 
clear them.

```cpp
    int e0bit   = MyFace::NewBitFlag();
    int e1bit   = MyFace::NewBitFlag();
    int e2bit   = MyFace::NewBitFlag();
    int ebit[3] = {e0bit,e1bit,e2bit};

    for( MyMesh::FaceIterator fi = m.face.begin(); fi != m.face.end(); ++fi )
        for( int i=0; i<3; ++i )
            (*fi).ClearUserBit( ebit[i] );
```
The task of clearing a given bit of all the faces (vertexes) of a mesh can be 
done with 
```cpp
    tri::UpdateFlags<YourMeshClass>::FaceClear( yourMesh, e0bit | e1bit | e2bit );
``` 
to clear all bits.
