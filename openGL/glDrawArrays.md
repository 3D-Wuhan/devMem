# glDrawArrays vs glDrawElements

[glDrawArrays��glDrawElements����](https://blog.csdn.net/cubesky/article/details/38753439)

---------------------------------------

���续һ����2��3������ɵ������Σ����Ͻ�������l,t,���½�������r,b

ʹ�� glDrawArrays ����ʱ����2�������Σ���Ҫ��������

(l,t),(r,t),(l,b)

(r,t),(r,b),(l,b)

����glDrawElements���Ļ���������
```cpp
float coord[4][2]={{l,t},{r,t},{r,b},{l,b}};
```

����ʱ��

0,1,3

1,2,3


Ҳ����˵ glDrawArrays �����ָ�������������յ���ʵ����,�ڻ���ʱ���ܸ��á�
�� glDrawElements ָ��������ʵ���ݵĵ�������,���ڴ�/�Դ�ռ���ϸ���ʡ



