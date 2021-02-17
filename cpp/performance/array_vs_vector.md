# �������顢std\:\:array �� std\:\:vector ��������ʱ���ܶԱ�

[C++ Core Guidelines: std\:\:array and std\:\:vector are your Friends](https://www.modernescpp.com/index.php/c-core-guidelines-std-array-and-std-vector-are-your-friends)

[���ڿ����������顢std::array��std::vector�ٶȵ�һЩ����](https://blog.csdn.net/qq_35976351/article/details/82940121)

[c++����� vector����ִ�����ܱȽ�](https://blog.csdn.net/h799710/article/details/107544792)

**************************

���Ĳ����⼸�����ݵĸ����ٶȣ�������֧��C++11/14��

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
    // ��������
    high_resolution_clock::time_point beginTime = high_resolution_clock::now();
    for(unsigned long long i = 0; i < N; ++i) {
        std::copy(ABoard.begin(), ABoard.end(), Atmp.begin());
    }
    high_resolution_clock::time_point endTime = high_resolution_clock::now();
    milliseconds timeInterval = std::chrono::duration_cast<milliseconds>(endTime - beginTime);
    std::cout << "array=" << timeInterval.count() << "ms\n";
	
	// ����vector
    std::vector<std::vector<int>>vec(10, std::vector<int>(10)), vec1(10, std::vector<int>(10));
    beginTime = high_resolution_clock::now();
    for(unsigned long long i = 0; i < N; ++i) {
        // vec.assign(vec1.begin(),vec1.end());  // ʱ���ر𳤣���Լ��array��20��
        vec1 = vec;   // ��assign����
        // vec.swap(vec1);  // ��Array����

    }
    endTime = high_resolution_clock::now();
    timeInterval = std::chrono::duration_cast<milliseconds>(endTime - beginTime);
    std::cout << "vector=" << timeInterval.count() << "ms\n";

	// C���Է�ʽ�Ŀ���д��
    beginTime = high_resolution_clock::now();
    for(unsigned long long i = 0; i < N; ++i) {
        memcpy(Board, tmp, 10 * 10 * sizeof(int));
    }
    endTime = high_resolution_clock::now();
    timeInterval = std::chrono::duration_cast<milliseconds>(endTime - beginTime);
    std::cout << "memcpy=" << timeInterval.count() << "ms\n";
}
/*
δ���Ż���
array=466ms
vector=7923ms
memcpy=198ms
*/
/*
-O3�Ż�������ٶȣ�
array=0ms
vector=453ms
memcpy=0ms
*/
```

��ˣ������Ҫ׷������ٶȣ���ô����ʹ��memcpy�����ʽ����ʹ���ӵ�һ�ڸ�10*10���������飬
�ٶ�1948msҲ�Ǻܿ�ġ�

ʵ�ʵļ����У�һ�ڸ������������ķ�ʱ��Ŀ��ܲ��Ǹ��Ʋ����ˣ�Ӧ���Ǳ���������һЩ���㣬
���Ƶ�ʱ�俪�����Բ��ơ����һ�㲻�ؿ��Ǹ����˷ѵ�ʱ�䡣�����Ż�һ���������ٶȻ���졣
