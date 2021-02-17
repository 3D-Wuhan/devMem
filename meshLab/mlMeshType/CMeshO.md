# Class CMeshO

**�ؼ����ۣ�**
* CMeshO �д洢����Ĺؼ����ݳ�Ա vert ������������ std\:\:vector< CVertexO >
* CMeshO �д洢��Ĺؼ����ݳ�Ա face ������������ std\:\:vector< CFaceO >
* ��Ҫ�� CMeshO �����Ӵ洢 patch �����ݳ�Ա������������ jicheng\:\:patch\:\:vector_ocf\<CPatchO\>
* ��Ҫ�� CPatchO �����Ӵ��� VertRef ���������� 

�� CMeshO �Ķ���λ�� MeshLab/src/common/ml_mesh_type.h ��󲿷֡�
������Ҫ���ӶԷֿ飨patch����֧�֣����ǽ���������ʵ��ĸ�д��

## 1 CMeshO ���� vcglib ����ģ�� TriMesh �����ģ���������

�� CMeshO �� meshLab ������Ҫ���ݵĹ��������ģ����
vcg\:\:tri\:\:TriMesh\< vcg\:\:vertex\:\:vector_ocf\<CVertexO\>, vcg\:\:face\:\:vector_ocf\<CFaceO\> \>
�����ࡣ����֪�� TriMesh ������ݳ�Ա vert �д�ŵ��ǵ������ݣ�
������ϵͳ�ĺ������ݡ�����������ϸ������Щ���������ͨ��˫ģ��������������𲽶���ġ�

**����TriMesh����ϸ�����μ�**[**TriMesh�Ĺؼ����ݳ�Աvert**](../vcglib/vertTriMesh.md)

TriMesh ���Ը�5��ģ�������ȱʡ���� DummyContainer�������� CMeshO ֻʹ����2��������
�ȷ�����һ��ģ����� vcg\:\:vertex\:\:vector_ocf\<CVertexO\>��

TriMesh �ĸ���ģ��Ϊ MArity5��ʹ�õĲ��� 
BaseMeshTypeHolder\<typename Container0\:\:value_type\:\:TypesPool\> 
�����ڵ������Ϊ��
```cpp
BaseMeshTypeHolder< vcg::vertex::vector_ocf<CVertexO>::value_type::TypesPool >
```
������Ҫ׷�� vcg\:\:vertex\:\:vector_ocf\<VALUE_TYPE\> �Ķ��壬
����ģ��Ķ���λ�� vcglib/vcg/simplex/vertex/component_ocf.h �е� ln43��

ע���� ln245 �� ln263�������� vcg\:\:vertex\:\:vector_ocf ��һЩ���ԣ�
������ɫ�����ʡ���������������ɫ�ȵȡ�

### 1.1 TypesPool

�������������� BaseMeshTypeHolder\<typename Container0\:\:value_type\:\:TypesPool\> 
�е� TypesPool������ [vertex](../vcglib/vertex.md) �� 2.1 �е�˵����
TypesPool ��ʵ���Ƕ��� Vertex �ĵ�һ��ģ����� UserTypes��
���� CVertexO ��˵���ģ��������� CUsedTypesO��
���������Ǹ��� CUsedTypesO Ϊ����Ķ����TypesPool ���ھ�Ӧ�ð��� CVertexO��
CEdgeO��CFaceO �� CPatchO��
```cpp
class CUsedTypesO : public vcg::UsedTypes < UseO<CVertexO>::AsVertexType,
	UseO<CEdgeO>::AsEdgeType,
	UseO<CFaceO>::AsFaceType,
	UseO<CPatchO>::AsPatchType> {};
```

### 1.2 TriMesh\:\:VertContainer ��ʵ���� std\:\:vector< CUsedTypesO\:\:VertexType >

���� CMeshO �Ĺؼ����� vert Ҳ�������� std\:\:vector< CVertexO > ���͡�

�� vcglib/vcg/complex/base.h �� ln58 �������¶��� BaseMeshTypeHolder �ģ�
```cpp
template <class TYPESPOOL>
struct BaseMeshTypeHolder
{
	typedef std::vector< typename TYPESPOOL::VertexType > CONTV;
����
	typedef CONTV									VertContainer;
����
}��
```

���� vcglib/vcg/complex/base.h �� ln172 ��
```cpp
template < class Container0 = DummyContainer, class Container1 = DummyContainer, class Container2 = DummyContainer, class Container3 = DummyContainer, class Container4 = DummyContainer >
class TriMesh
        : public  MArity5< BaseMeshTypeHolder<typename Container0::value_type::TypesPool>, Container0, Der ,Container1, Der, Container2, Der, Container3, Der, Container4, Der >{
```
�ֱ����ڶ��� TriMesh������ CMeshO ��˵ Container0 ���� 
vcg\:\:vertex\:\:vector_ocf\<CVertexO\>���� value_type ���� CVertexO��
��Ӧ�� TypesPool ���� CUsedTypesO���ٻص����� BaseMeshTypeHolder �Ķ����֪ 
VertContainer ���� std\:\:vector< CVertexO >��


### 1.3 ���� patch\:\:vector_ocf

Ϊ������ CMeshO �����Ӷ� CPatchO ��֧�֣����ȱ��붨��� patch\:\:vector_ocf��
����ķ������Բο� vcg\:\:vertex\:\:vector_ocf �Ķ��塣



## 2 �� CMeshO �����Ӷ� CPatchO ��֧��

���ڽ���Ҫ�ò�ͬ����ɫ�����ֲ�ͬ�� patch ������ʱ��Ч������ҪΪ CPatchO ������ɫ֧�֡�
���� vcglib �����˼·����õķ�ʽ��Ȼ���� patch namespace ��������ģ�� EmptyCore��

### 2.1 ���� jicheng\:\:patch\:\:EmptyCore



