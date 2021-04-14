# TypesPool

## 1 TypesPool of vcg\:\:Vertex
vcglib �� vcglib\vcg\simplex\vertex\base.h �� ln191 
�� vcg\:\:Vertex ��һ��ģ���������Ϊ TypesPool:
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
vcglib �� vcglib\vcg\simplex\edge\base.h �� ln216 
�� vcg\:\:Edge �ĵ�һ��ģ���������Ϊ TypesPool:
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
vcglib �� vcglib\vcg\simplex\face\base.h �� ln291 
�� vcg\:\:Face �ĵ�һ��ģ���������Ϊ TypesPool:
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


