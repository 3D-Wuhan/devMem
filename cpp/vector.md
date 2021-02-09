# 多维vector内存占用问题分析

[C++ 多维 vector 内存占用问题分析](https://blog.csdn.net/tom13a/article/details/109746097)

---------------------

当我们需要长度不定的容器时，vector 是一个不错的选择，它可以存储单维度的不定个数的数据，
也可以叠加维度，实现不定长的二维甚至多维数组的功能。
比如使用 vector\<vector\<double\>\> 就可以定义二维的 vector 容器。

vector 容器的特性是在被定义的时候，它就会自动预留16个存储空间。在二维的情况下，
如果让他存储 double 数据，它最少的内存空间需求就是 8 * 16 * 16=2592 Byte
（一个double是8个Byte）。
如果是 3 维的 vector，vector\< vector\<vector\<double\>\> \>
其最少的占用内存数据可以达到 8 * 16 * 16 * 16 个 Byte。
在大数据量的模式下，多维内存占用问题成为一个必须面对的问题。

笔者写的程序中，需要存储一系列 4*4 的数组。一开始使用的是三维 vector 模式，内存占用较高，
程序的可拓展性受到限制。后来经过请教高手，改为用 vector 里面存储数组的模式（内嵌数组模式），
即定义如下数据类型：
```cpp
using elm_type  = array< array<double, 4>, 4 >;
vector<elm_type>    arr;
```
用来代替三维 vector 中里面嵌套的两维数据，改进后效果比较明显，内存占用量不再高企。

为了测试改进前后的对比效果，我使用了如下代码：
三维vector方式：

```cpp
#include <vector>

vector<vector<vector<double>>> arr1;
int main()
{
	while (arr1.size()<20000)
	{
		vector<vector<double>> temp{ {0},{0},{0},{0},
		{0},{0},{0},{0}, 
		{0},{0},{0},{0}, 
		{0},{0},{0},{0}, };

		arr1.push_back(temp);
		cout << arr1.size()<<"  ";
	}
	Sleep(1000000);
	return 0;
}
```
内嵌数组模式：
```cpp
#include <vector>
#include <array>

using elm_type = array<array<double, 4>, 4>;
elm_type uu;
vector<elm_type> arr;
int main()
{
	while (arr.size()<20000)
	{
		for (int i = 0; i < 4; i++)
		{
			for (int j = 0; j < 4; j++)
				uu[i][j] = 5000;
	    }
	
		arr.push_back(uu);

		cout << arr.size()<<"  ";
	}
	Sleep(1000000);
	return 0;
}
```
上面两种方式，是通过让容器填充 20000 个元素，然后跳出循环。
为了简便观察内存占用情况，需要打开 window 的运行程序管理观察窗口——任务管理器。
根据观察，前者的内存占用量在运行中逐步累加，最终到达43M，而后者最后停留在4.2M, 
内存使用量只有前者的10%，而且运行速度也快了不少。

关于如何获得某个数据的精确内存占用量，网上有操作IDE来实现的教程，操作很复杂，我没有采用。
