# How meshlab calling into functions in vcglib

��������ͨ���������� stl �ĵ�����ش�������̽�����������̡�

meshlab ͨ�� MainWindow::loadMesh (...) ��������Ӧ·���ļ��е� meshmodel���ú���λ��
mainwindow_RunTime.cpp �� ln2794������д��һ�����뵥���ļ�����Ӧ�汾 loadMesh_single
��λ�� mainwindow_RunTime_3.cpp �� ln271��

�ú�������ò�� pCurrentIOPlugin->open ( extension, fileName, *mm, mask, *prePar, QCallBack, this /*gla*/ ) 
��������Ӧ���ļ�, ���� QCallBack �ǻص��������ԶԽ��������в�����

���� stl �ļ���˵, pCurrentIOPlugin ����λ�� MeshLab\src\meshlabplugins\io_base �е�
baseio.cpp ֮λ�� ln85 �ĺ��� BaseMeshIOPlugin\:\:open ( ... )�����ú�������� vcglib �е�
tri \:\: io \:\: ImporterSTL\<CMeshO\> \:\: Open(m.cm, filename.c_str(), mask, cb) ��
��� stl �ļ��Ķ�ȡ����������������õ���λ�� vcglib\wrap\io_trimesh\import_stl.h ��
ln189 �ĺ��� static int Open( OpenMeshType &m, const char * filename, int &loadMask, 
CallBackPos *cb=0 )����������ߵ���ͬһ�ļ��� ln204 �ĺ��� static int OpenBinary( ... )��
���ߵ��� ln254 �ĺ��� static int OpenAscii( ... )�����׵��õ�����һ��ȡ������Ӧ�� stl �ļ�
�Ƕ����ƻ��� ascii ��ʽ�������ṩ�����ǵ� stl �� ascii ��ʽ��������в�����ɫ��Ϣ��






