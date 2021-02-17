# 常规数组、std\:\:array 和 std\:\:vector 拷贝操作时性能对比

[C++ Core Guidelines: std\:\:array and std\:\:vector are your Friends](https://www.modernescpp.com/index.php/c-core-guidelines-std-array-and-std-vector-are-your-friends)

[关于拷贝常规数组、std::array和std::vector速度的一些测试](https://blog.csdn.net/qq_35976351/article/details/82940121)

[c++数组和 vector访问执行性能比较](https://blog.csdn.net/h799710/article/details/107544792)

**************************

本文测试这几种数据的复制速度，编译器支持C++11/14。

```cpp
#include <iostream>
#include <chrono>
#include <cstring>
#include <algorithm>
#include <array>
#include <vector>

using std::chrono::high_resolution_clock;
using std::chrono::milliseconds;

int main() {
    int Board[10][10] = {0}, tmp[10][10] = {0};
    std::array<std::array<int, 10>, 10>ABoard, Atmp;
    unsigned long long N = 10000000;
    // 复制数组
    high_resolution_clock::time_point beginTime = high_resolution_clock::now();
    for(unsigned long long i = 0; i < N; ++i) {
        std::copy(ABoard.begin(), ABoard.end(), Atmp.begin());
    }
    high_resolution_clock::time_point endTime = high_resolution_clock::now();
    milliseconds timeInterval = std::chrono::duration_cast<milliseconds>(endTime - beginTime);
    std::cout << "array=" << timeInterval.count() << "ms\n";
	
	// 复制vector
    std::vector<std::vector<int>>vec(10, std::vector<int>(10)), vec1(10, std::vector<int>(10));
    beginTime = high_resolution_clock::now();
    for(unsigned long long i = 0; i < N; ++i) {
        // vec.assign(vec1.begin(),vec1.end());  // 时间特别长，大约是array的20倍
        vec1 = vec;   // 与assign类似
        // vec.swap(vec1);  // 与Array类似

    }
    endTime = high_resolution_clock::now();
    timeInterval = std::chrono::duration_cast<milliseconds>(endTime - beginTime);
    std::cout << "vector=" << timeInterval.count() << "ms\n";

	// C语言方式的快速写入
    beginTime = high_resolution_clock::now();
    for(unsigned long long i = 0; i < N; ++i) {
        memcpy(Board, tmp, 10 * 10 * sizeof(int));
    }
    endTime = high_resolution_clock::now();
    timeInterval = std::chrono::duration_cast<milliseconds>(endTime - beginTime);
    std::cout << "memcpy=" << timeInterval.count() << "ms\n";
}
/*
未开优化：
array=466ms
vector=7923ms
memcpy=198ms
*/
/*
-O3优化，最高速度：
array=0ms
vector=453ms
memcpy=0ms
*/
```

因此，如果想要追求最快速度，那么尽量使用memcpy这个方式。即使增加到一亿个10*10的整型数组，
速度1948ms也是很快的。

实际的计算中，一亿个局面的情况，耗费时间的可能不是复制操作了，应该是背后其它的一些计算，
复制的时间开销可以不计。因此一般不必考虑复制浪费的时间。编译优化一旦开启，速度会更快。
