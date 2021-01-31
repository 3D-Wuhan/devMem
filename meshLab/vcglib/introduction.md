# Introduction to vcglib

[几个经典的数学库之一学习---VCGlib（1）](https://www.cnblogs.com/icmzn/p/6640752.html)

----------------

VCG lib is a General Purpose Library for Simplicial Complexes(其实仅仅包含简单的代数
拓扑学的单纯形理论的一丁点儿概念，熟悉基础数学的同学恐怕要大大失望了).
Nevertheless, standing at the height of pure Mathematics, everything is so EASY.

## 0 Simplices

VCG simplices include Vertex, Edge, Face and Tetrahedron. A simplex can have 
attributes other then its geometrical position: normal, color, quality, connectivity 
information, etc. Note that normal, color and quality are not topological attributes.

One may want:
* To have a vertex with any combination of these attributes
* To choose a set of attributes dynamically
* To create user-defined attributes


## 1 Introduction

 VCG Libary 是意大利 Visulization and Computer Graphics Libary 的缩写，
是一个开源的C++模板库，用于三角网格和四面体网格的控制、处理和OpenGL显示。其中包含了超过
100 000行的代码。基于该库 Visual Computing Lab开发了几个著名的工具，如 metro 和 MeshLab。

VCG Libary是专门为处理三角网格而设计的，库很大，且提供了许多最先进的处理网格的功能，如：
* 基于边坍塌(edge-collapse)二次误差的高质量网格简化(simplfication)；
* 高效的空间检索数据结构(uniform grids, hashed grids, kdtree, ...)；
* 先进的网格平滑和光顺算法；
* 曲率计算；
* 纹理坐标优化；
* Hausdorff距离计算；
* 测地路径；
* 网格修复能力
* 等直面抽取和前沿的网格划分算法；
* 泊松圆盘采样和其他的网格点采样算法；
* 细分曲面。

## 2 How to use

将 vcglib 文件夹放到编译器的“include”目录后, 使用时包含其中需要的头文件即可。

vcglib 文件夹中主要含以下 5 个子文件夹：
* vcg：这是整个库的核心，其中定义了所有的算法和数据结构。该部分所有的 C++ 代码都是 STL 支持
的普通数据结构和算法，不包含任何其它标准库之外的库，而且该部分只含头文件(.h)；
* wrap：这里包含一些针对特定需求/上下文/库的 VCG 概念的封装。例如用于多种格式的
网格数据的导入和导出；用 OpenGL 渲染三角形网格的代码；普通 GUI 工具如跟踪球，等等；
* apps：这个文件夹包含一些用 VCG Lib 开发的命令行程序应用。
很多例子都能在MeshLab中找到，apps/simple 文件夹包含了这些程序的一个基础的子集，是一个
初学者很好的入口点；
* docs：主要是 doxygen 文档, 还包含一个介绍 vcg 的 ppt；
* eigenLib：线性代数的 eigen 库最近的稳定版本的一个副本（相当于就是借用第三方库了），
VCGLib 中的高级矩阵操作都是基于这个库的。



