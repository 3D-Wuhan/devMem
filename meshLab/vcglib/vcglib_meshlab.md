# How meshlab calling into functions in vcglib

这里我们通过分析导入 stl 文档的相关代码来窥探整个调用流程。

meshlab 通过 MainWindow::loadMesh (...) 来读入相应路径文件中的 meshmodel。该函数位于
mainwindow_RunTime.cpp 的 ln2794，我们写了一个读入单个文件的相应版本 loadMesh_single
则位于 mainwindow_RunTime_3.cpp 的 ln271。

该函数则调用插件 pCurrentIOPlugin->open ( extension, fileName, *mm, mask, *prePar, QCallBack, this /*gla*/ ) 
来读入相应的文件, 其中 QCallBack 是回调函数可以对进度条进行操作。

而对 stl 文件来说, pCurrentIOPlugin 就是位于 MeshLab\src\meshlabplugins\io_base 中的
baseio.cpp 之位于 ln85 的函数 BaseMeshIOPlugin\:\:open ( ... )，而该函数则调用 vcglib 中的
tri \:\: io \:\: ImporterSTL\<CMeshO\> \:\: Open(m.cm, filename.c_str(), mask, cb) 来
完成 stl 文件的读取。至此最后真正调用的是位于 vcglib\wrap\io_trimesh\import_stl.h 中
ln189 的函数 static int Open( OpenMeshType &m, const char * filename, int &loadMask, 
CallBackPos *cb=0 )，后者则或者调用同一文件中 ln204 的函数 static int OpenBinary( ... )，
或者调用 ln254 的函数 static int OpenAscii( ... )。到底调用的是哪一个取决于相应的 stl 文件
是二进制还是 ascii 格式，王工提供给我们的 stl 是 ascii 格式，因此其中不含颜色信息。






