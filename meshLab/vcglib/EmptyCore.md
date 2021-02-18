# Class Template: EmptyCore

The most interesting class template in vcglib should be EmptyCore. In fact, there 
are four such class templates, lying in vcglib/vcg/simplex/vertex/component.h, 
vcglib/vcg/simplex/face/component.h, vcglib/vcg/simplex/edge/component.h and 
vcglib/vcg/simplex/tetrahedron/component.h.

EmptyCore 之所以精彩，是因为可以通过模板类 EmptyCore 给不同类增加相同的 section 完成类似的 
aspect。EmptyCore 的模板参数是其父类，当父类即模板参数不同时就给不同的类增加了相同的 
section(截面)，而 EmptyCore 实现的其实就是截面的功能。

## 2 COORD

Templated on the coordinate class. In practice you use one of the two specialized 
class Coord3f and Coord3d.
You can access to the coordinate of a vertex by mean of the P( ), cP( ) member 
functions.

```cpp
template <class A, class T> class Coord: public T 
{
public:
    typedef A CoordType;
    typedef typename A::ScalarType      ScalarType;
    /// Return a const reference to the coordinate of the vertex
    inline const CoordType &P() const { return _coord; }
    /// Return a reference to the coordinate of the vertex
    inline       CoordType &P()       { return _coord; }
    /// Return a const reference to the coordinate of the vertex
    inline       CoordType cP() const { return _coord; }

    template < class RightValueType>
    void    ImportData(const RightValueType  & rVert ) { if(rVert.IsCoordEnabled()) P().Import(rVert.cP()); T::ImportData( rVert); }
    static bool HasCoord()   { return true; }
    static void Name(std::vector<std::string> & name){name.push_back(std::string("Coord"));T::Name(name);}

private:
  CoordType _coord;
};

/// Specialized Coord Component in floating point precision.
template <class T> class Coord3f: public Coord<vcg::Point3f, T> 
{
public:
    static void Name(std::vector<std::string> & name)
    {
        name.push_back(std::string("Coord3f"));
        T::Name(name);
    }
};
```

网上说类模板的静态成员函数不能重写，但上面 Coord3f 对 Coord 的静态成员函数 Name ( ... )
进行了重写，似乎也没有问题。将来有机会写代码验证一下到底情况如何。参见
<https://stackoverflow.com/questions/34222703/how-to-override-static-method-of-template-class-in-derived-class>

<https://blog.csdn.net/chen134225/article/details/81188476>





