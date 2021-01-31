# 光源与材质

<https://www.cnblogs.com/yyyyyyzh666/p/9916793.html>

------------------------------

glLightModelfv 对应的是材质:
* glMaterialfv(GL_FRONT,GL_AMBIENT,mat_ambient); // 材质属性中的环境光
* glMaterialfv(GL_FRONT,GL_DIFFUSE,mat_diffuse); // 材质属性中的散射光
* glMaterialfv(GL_FRONT, GL_SPECULAR, mat_specular); // 材质属性中的镜面反射光
* glMaterialfv(GL_FRONT, GL_SHININESS, mat_shininess); // 材质属性的镜面反射指数
* glMaterialfv(GL_FRONT,GL_EMISSION,mat_emission); // 材质属性中的发射光

glLightfv 对应的是光源:
* glLightfv(GL_LIGHT0, GL_POSITION, light_position); // 指定光源位置
* glLightfv(GL_LIGHT0,GL_AMBIENT,light_ambient); // 光源中的环境光强度
* glLightfv(GL_LIGHT0,GL_DIFFUSE,white_light); // 光源中的散射光强度
* glLightfv(GL_LIGHT0,GL_SPECULAR,white_light); // 光源中的镜面反射光强度



