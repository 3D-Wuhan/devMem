# TriMesh �Ĺؼ����ݳ�Ա vert


## 1 TriMesh\:\:vert �� std\:\:vector<...>

��ģ�� TriMesh �� vcglib ��ؼ������ݽṹ֮һ��
���������ǽ����� TriMesh �Ĺؼ����ݳ�Ա vert �Ķ�����̡�

���� vert �������� TriMesh::VertContainer����������ڽṹ�� BaseMeshTypeHolder 
�б����塣�ṹ�� BaseMeshTypeHolder �Ķ���ǳ���Ȥ����û���κ����ݣ�
ֻ�������͵Ķ��壬Ҳ����˵��������һ�� TypeHolder��
���뵽 Bruce McGauphy ���������ҿ��������� static ����������ȫ�ֺ����� FunctionHolder��
����ͬ������֮��ճ����ʵ���к��ٲ�����Щ Holder�����Ҫ���ʹ����������ķ�ʽ��
��ǿ����ĳ����ԡ�C++ �ľ��ʾ������������޾������������켼�����߲�ĳ���˼ά��
��ǿ�� C++ ������˼ά�����Ǽ������׵Ĺؼ���

BaseMeshTypeHolder �Ķ���λ�� vcglib\vcg\complex\base.h �� ln58��
```cpp
template <class TYPESPOOL>
struct BaseMeshTypeHolder{
	typedef std::vector< typename TYPESPOOL::VertexType >	CONTV;

	typedef CONTV						VertContainer;
```

�ɴ˿�֪ VertContainer ��ʵ���� std\:\:vector\< typename TYPESPOOL::VertexType \>��

## 2 ��ģ�� TriMesh �� TriMesh\:\:vert ��صĲ���

ͨ������ķ�������֪��ҪŪ��� vert �ͱ���������֮��ص�ģ����������������壺
```cpp
template < 
	class Container0 = DummyContainer, 
	class Container1 = DummyContainer, 
	class Container2 = DummyContainer, 
	class Container3 = DummyContainer, 
	class Container4 = DummyContainer >
class TriMesh
        : public  MArity5<
			BaseMeshTypeHolder<typename Container0::value_type::TypesPool>, 
			Container0, Der, 
			Container1, Der, 
			Container2, Der, 
			Container3, Der, 
			Container4, Der >
{
public:

};
```



