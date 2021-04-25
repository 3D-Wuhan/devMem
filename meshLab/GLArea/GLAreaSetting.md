# GLArea Setting

һЩ��ʾ���������綥����ʾ�Ĵ�С�ڴ����á�

����˵������ߡ� -> ��options" ������������ý���:

![GLArea Setting](pix/glarea_setting.PNG)

�� GLAreaSetting �Ķ���λ�� MeshLab/src/meshlab/glarea_setting.h �С�
�� MeshLab/src/meshlab/glarea.h ln147 �� GLArea ������������� GLAreaSetting glas��
�������ǿ���ͨ�� GLA ( ) �����ʴ�����������á�

�������˵������ߡ� -> ��options" ����Ӧ����λ��
MeshLab/src/meshlab/mainwindow_init.cpp �� ln496��
```cpp
	setCustomizeAct = new QAction(tr("&Options..."), this);
	connect(setCustomizeAct, SIGNAL(triggered()), this, SLOT(setCustomize()));
```
�ۺ����Ķ���λ�� 
MeshLab/src/meshlab/mainwindow_RunTime.cpp �� ln5771��
```cpp
void MainWindow::setCustomize()
{
    CustomDialog dialog(currentGlobalParams,defaultGlobalParams, this);
    connect(&dialog,SIGNAL(applyCustomSetting()),this,SLOT(updateCustomSettings()));
    dialog.exec();
}
```
�ڶԻ��� CustomDialog dialog �����ź� applyCustomSetting() ��
�ۺ��� updateCustomSettings ( ) ������Ӧ��
```cpp
void MainWindow::updateCustomSettings()
{
    mwsettings.updateGlobalParameterSet(currentGlobalParams);
    emit dispatchCustomSettings(currentGlobalParams);
}
```
���������һЩȫ�ֲ��������ã����� glarea ������������ͨ���ź� dispatchCustomSettings
�������ݸ� GLArea �������á�

���ź� dispatchCustomSettings ��۵�����λ�� MeshLab/src/meshlab/glarea.cpp ln121��
```cpp
        connect(mainwindow,SIGNAL(dispatchCustomSettings(RichParameterSet&)),this,SLOT(updateCustomSettingValues(RichParameterSet&)));
```

�� MeshLab/src/meshlab/glarea.cpp ln1778 �жԴ������ź� dispatchCustomSettings 
������Ӧ��
```cpp
void GLArea::updateCustomSettingValues( RichParameterSet& rps )
{
	makeCurrent();
    glas.updateGlobalParameterSet(rps);

    this->update();
}
```

