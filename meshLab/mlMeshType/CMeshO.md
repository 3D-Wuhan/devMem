# Class CMeshO

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
