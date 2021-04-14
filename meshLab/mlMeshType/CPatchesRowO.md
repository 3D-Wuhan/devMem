# Class CPatchesRowO

引入类 CPatchesRowO 是为了对分区进行快速排序。在判断某个顶点属于哪个分区时，
若分区已经被排序则能大大加快搜索的性能。我们增加 CPatch 及 CPatchesRow 
的初衷就是加快顶点的搜索速度。

定义 CPatchesRowO 需要考虑如下要素：
* 一个 array 来保存 patches。事实上这些 patches 已经有排序；
* patchesrow 之间也有排序；

需要修改的代码：
* 在 CMeshO 中增加保存 patches_row 的容器 jicheng\:\:patches_row\:\:array_ocf
* 在 CPatchesRowO 中定义保存 paches 的容器 jicheng\:\:patch\:\:array_ocf
* 修改 CUsedTypesO 增加对类型 CPatchesRowO 的管理

步骤：
* 为 patches_row 增加 EmptyCore 及颜色等其它模块，jicheng/patches_row/component.h
* 增加 PatchesRowTypeHolder，ln37 of jicheng/patches_row/based.h
* 定义 PatchesRowBase，ln45 of jicheng/patches_row/based.h
* 定义类模板 PatchesRowArityMax 作为最终派生类，ln61 of jicheng/patches_row/based.h
* 由 PatchesRowArityMax 派生类模板 PatchesRow，ln239 of jicheng/patches_row/based.h
* 由类模板 PatchesRow 派生模板类 CPatchesRowO

目前存在的问题：
* 目前 jicheng/patches_row/component.h 中定义的模块太少，仅含 Flag 和 Color；
* CPatchesRowO 中需要增加 vector_ocf\<CPatchO\>

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

