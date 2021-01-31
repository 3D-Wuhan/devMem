# Introduction to vcglib

[�����������ѧ��֮һѧϰ---VCGlib��1��](https://www.cnblogs.com/icmzn/p/6640752.html)

----------------

VCG lib is a General Purpose Library for Simplicial Complexes(��ʵ���������򵥵Ĵ���
����ѧ�ĵ��������۵�һ����������Ϥ������ѧ��ͬѧ����Ҫ���ʧ����).
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

 VCG Libary ������� Visulization and Computer Graphics Libary ����д��
��һ����Դ��C++ģ��⣬�����������������������Ŀ��ơ������OpenGL��ʾ�����а����˳���
100 000�еĴ��롣���ڸÿ� Visual Computing Lab�����˼��������Ĺ��ߣ��� metro �� MeshLab��

VCG Libary��ר��Ϊ���������������Ƶģ���ܴ����ṩ��������Ƚ��Ĵ�������Ĺ��ܣ��磺
* ���ڱ�̮��(edge-collapse)�������ĸ����������(simplfication)��
* ��Ч�Ŀռ�������ݽṹ(uniform grids, hashed grids, kdtree, ...)��
* �Ƚ�������ƽ���͹�˳�㷨��
* ���ʼ��㣻
* ���������Ż���
* Hausdorff������㣻
* ���·����
* �����޸�����
* ��ֱ���ȡ��ǰ�ص����񻮷��㷨��
* ����Բ�̲��������������������㷨��
* ϸ�����档

## 2 How to use

�� vcglib �ļ��зŵ��������ġ�include��Ŀ¼��, ʹ��ʱ����������Ҫ��ͷ�ļ����ɡ�

vcglib �ļ�������Ҫ������ 5 �����ļ��У�
* vcg������������ĺ��ģ����ж��������е��㷨�����ݽṹ���ò������е� C++ ���붼�� STL ֧��
����ͨ���ݽṹ���㷨���������κ�������׼��֮��Ŀ⣬���Ҹò���ֻ��ͷ�ļ�(.h)��
* wrap���������һЩ����ض�����/������/��� VCG ����ķ�װ���������ڶ��ָ�ʽ��
�������ݵĵ���͵������� OpenGL ��Ⱦ����������Ĵ��룻��ͨ GUI ����������򣬵ȵȣ�
* apps������ļ��а���һЩ�� VCG Lib �����������г���Ӧ�á�
�ܶ����Ӷ�����MeshLab���ҵ���apps/simple �ļ��а�������Щ�����һ���������Ӽ�����һ��
��ѧ�ߺܺõ���ڵ㣻
* docs����Ҫ�� doxygen �ĵ�, ������һ������ vcg �� ppt��
* eigenLib�����Դ����� eigen ��������ȶ��汾��һ���������൱�ھ��ǽ��õ��������ˣ���
VCGLib �еĸ߼�����������ǻ��������ġ�



