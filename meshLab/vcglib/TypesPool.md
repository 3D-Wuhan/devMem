# TypesPool

## 1 TypesPool of vcg\:\:Vertex
vcglib 中 vcglib\vcg\simplex\vertex\base.h 的 ln191 
将 vcg\:\:Vertex 第一个模板参数定义为 TypesPool:
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
    typedef AllTypes::AVertexType IAm;
    typedef UserTypes TypesPool;
};
```

## 2 TypesPool of vcg\:\:Edge
vcglib 中 vcglib\vcg\simplex\edge\base.h 的 ln216 
将 vcg\:\:Edge 的第一个模板参数定义为 TypesPool:
```cpp
template <class UserTypes,
          template <typename> class A = DefaultDeriver, template <typename> class B = DefaultDeriver,
          template <typename> class C = DefaultDeriver, template <typename> class D = DefaultDeriver,
          template <typename> class E = DefaultDeriver, template <typename> class F = DefaultDeriver,
          template <typename> class G = DefaultDeriver, template <typename> class H = DefaultDeriver,
					template <typename> class I = DefaultDeriver, template <typename> class J = DefaultDeriver>
							class Edge: public EdgeArityMax<UserTypes, A, B, C, D, E, F, G, H, I, J>  
{
public:
    typedef AllTypes::AEdgeType IAm;
    typedef UserTypes TypesPool;
};
```

## 3 TypesPool of vcg\:\:Face
vcglib 中 vcglib\vcg\simplex\face\base.h 的 ln291 
将 vcg\:\:Face 的第一个模板参数定义为 TypesPool:
```cpp
template <class UserTypes,
          template <typename> class A = DefaultDeriver, template <typename> class B = DefaultDeriver,
          template <typename> class C = DefaultDeriver, template <typename> class D = DefaultDeriver,
          template <typename> class E = DefaultDeriver, template <typename> class F = DefaultDeriver,
          template <typename> class G = DefaultDeriver, template <typename> class H = DefaultDeriver,
          template <typename> class I = DefaultDeriver, template <typename> class J = DefaultDeriver,
          template <typename> class K = DefaultDeriver, template <typename> class L = DefaultDeriver >
                            class Face: public FaceArityMax<UserTypes, A, B, C, D, E, F, G, H, I, J, K, L>
{
public:
    typedef AllTypes::AFaceType IAm;
    typedef UserTypes TypesPool;
};
```


