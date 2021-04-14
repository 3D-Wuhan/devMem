# Types of Meshlab

在 vcglib 中每一个单纯形的定义中都定义了 [TyesPool](../vcglib/TypesPool.md).
而在最关键的 TriMesh 中则使用了第一个参数的 TypesPool (通常就是 CVertexO 的TypesPool) 
通过 BaseMeshTypeHolder 来管理 Types。

BaseMeshTypeHolder 是个抽象结构体，其中只有类型的定义，完成对类型的管理。
详细内容参见 vcglib\vcg\simplex\complex\base.h 的 ln58。

对于其中没有的定义，使用者必须在自己的 TriMesh 子类中加以定义。
