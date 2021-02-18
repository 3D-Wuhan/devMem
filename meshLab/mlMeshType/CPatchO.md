# Class CPatchO

引入类 CPatchO 是为了进行分区投影，一个分区（patch）最终会被投影成为一个浮点值。
定义 CPatchO 需要考虑如下要素：
* 一个 vector_ocf 来保存 VertexRef。我们不直接保存 Vertex 形成的 vector_ocf，
因为 patch 中的顶点其实在 CMeshO 中已经被定义好，不需要重复定义；
* 颜色属性；
* 投影值

需要修改的代码：
* 在 CMeshO 中增加保存分区的容器 jicheng\:\:patch\:\:vector_ocf
* 在 CPatchO 中增加保存 VertexRef 的容器
* 修改 CUsedTypesO 增加对类型 CPatchO 的管理

步骤：
* 为 patch 增加 EmptyCore 及颜色等其它模块，jicheng/patch/component.h
* 增加 PatchTypeHolder，ln37 of jicheng/patch/based.h
* 定义 PatchBase，ln45 of jicheng/patch/based.h
* 定义类模板 PatchArityMax 作为最终派生类，ln61 of jicheng/patch/based.h
* 由 PatchArityMax 派生类模板 Patch，ln239 of jicheng/patch/based.h
* 由类模板 Patch 派生模板类 CPatchO

目前存在的问题：
* 目前 jicheng/patch/component.h 中定义的模块太少，仅含 Flag 和 Color；
* 需要处理 CPatchesRow


