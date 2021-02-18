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
* 为 patch 增加 EmptyCore
* 




