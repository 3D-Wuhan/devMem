# glEnableClientState / glDisableClientState

[OpenGL顶点数组的理解](http://blog.sina.com.cn/s/blog_68bcb17f0101nsoa.html)

------------------------------------------

有了 glEnable/glDisable 函数，为什么还要引入 glEnableClientState / glDisableClientState
函数呢？这跟OpenGL的工作机制有关。OpenGL在设计时，认为可以将整个OpenGL系统分为两部分，
一部分是客户端，它负责发送OpenGL命令。一部分是服务端，它负责接收OpenGL命令并执行相应的操作。
对于个人计算机来说，可以将CPU、内存等硬件，以及用户编写的OpenGL程序看做客户端，而将 OpenGL 
驱动程序、显示设备等看做服务端。

通常，所有的状态都是保存在服务端的，便于OpenGL使用。例如，是否启用了纹理，
服务端在绘制时经常需要知道这个状态，而我们编写的客户端 OpenGL 
程序只在很少的时候需要知道这个状态。所以将这个状态放在服务端是比较有利的。

但顶点数组的状态则不同。我们指定顶点，实际上就是把顶点数据从客户端发送到服务端。
是否启用顶点数组，只是控制发送顶点数据的方式而已。服务端只管接收顶点数据，
而不必管顶点数据到底是用哪种方式指定的（可以直接使用glBegin/glEnd/glVertex*，
也可以使用顶点数组）。所以，服务端不需要知道顶点数组是否开启。因此，
顶点数组的状态放在客户端是比较合理的。

为了表示服务端状态和客户端状态的区别，服务端的状态用 glEnable/glDisable，客户端的状态则用 
glEnableClientState/glDisableClientState。


