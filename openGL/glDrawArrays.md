# glDrawArrays vs glDrawElements

[glDrawArrays、glDrawElements区别](https://blog.csdn.net/cubesky/article/details/38753439)

---------------------------------------

比如画一个由2个3角形组成的正方形，左上角坐标是l,t,右下角坐标是r,b

使用 glDrawArrays 绘制时，画2个三角形，需要这样传：

(l,t),(r,t),(l,b)

(r,t),(r,b),(l,b)

而用glDrawElements画的话可以这样
```cpp
float coord[4][2]={{l,t},{r,t},{r,b},{l,b}};
```

绘制时：

0,1,3

1,2,3


也就是说 glDrawArrays 传输或指定的数据是最终的真实数据,在绘制时性能更好。
而 glDrawElements 指定的是真实数据的调用索引,在内存/显存占用上更节省



