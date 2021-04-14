# Class CPatchesRowO

������ CPatchesRowO ��Ϊ�˶Է������п����������ж�ĳ�����������ĸ�����ʱ��
�������Ѿ����������ܴ��ӿ����������ܡ��������� CPatch �� CPatchesRow 
�ĳ��Ծ��Ǽӿ춥��������ٶȡ�

���� CPatchesRowO ��Ҫ��������Ҫ�أ�
* һ�� array ������ patches����ʵ����Щ patches �Ѿ�������
* patchesrow ֮��Ҳ������

��Ҫ�޸ĵĴ��룺
* �� CMeshO �����ӱ��� patches_row ������ jicheng\:\:patches_row\:\:array_ocf
* �� CPatchesRowO �ж��屣�� paches ������ jicheng\:\:patch\:\:array_ocf
* �޸� CUsedTypesO ���Ӷ����� CPatchesRowO �Ĺ���

���裺
* Ϊ patches_row ���� EmptyCore ����ɫ������ģ�飬jicheng/patches_row/component.h
* ���� PatchesRowTypeHolder��ln37 of jicheng/patches_row/based.h
* ���� PatchesRowBase��ln45 of jicheng/patches_row/based.h
* ������ģ�� PatchesRowArityMax ��Ϊ���������࣬ln61 of jicheng/patches_row/based.h
* �� PatchesRowArityMax ������ģ�� PatchesRow��ln239 of jicheng/patches_row/based.h
* ����ģ�� PatchesRow ����ģ���� CPatchesRowO

Ŀǰ���ڵ����⣺
* Ŀǰ jicheng/patches_row/component.h �ж����ģ��̫�٣����� Flag �� Color��
* CPatchesRowO ����Ҫ���� vector_ocf\<CPatchO\>

## Definition of Class Template PatchesRow

The class template PatchesRow is derived from PatchesRowArityMax as follows:
```cpp
template <class UserTypes,
	template <typename> class A = DefaultDeriver, template <typename> class B = DefaultDeriver,
	template <typename> class C = DefaultDeriver, template <typename> class D = DefaultDeriver,
	template <typename> class E = DefaultDeriver, template <typename> class F = DefaultDeriver,
	template <typename> class G = DefaultDeriver, template <typename> class H = DefaultDeriver,
	template <typename> class I = DefaultDeriver, template <typename> class J = DefaultDeriver,
	template <typename> class K = DefaultDeriver, template <typename> class L = DefaultDeriver >
class PatchesRow : public PatchesRowArityMax<UserTypes, A, B, C, D, E, F, G, H, I, J, K, L>
{
public:
	typedef AllTypes::APatchesRowType IAm;
	typedef UserTypes TypesPool;
};
```
See ln239 of jicheng/patches_row/based.h

## Definition of template class CPatchesRowO

```cpp
class CPatchesRowO : public jicheng::PatchesRow < CUsedTypesO,
	jicheng::patches_row::BitFlags,
	jicheng::patches_row::Color4b>
{
public:
	typedef TypesPool::PatchType	PatchType;
	typedef std::vector<TypesPool::PatchType>	PatchContainer;

	PatchContainer	patches;
};
```
See ln241 of MeshLab/src/common/ml_mesh_type.h

