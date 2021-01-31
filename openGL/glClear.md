
# ���� glClearColor �� glClear �ı䱳����ɫ

<https://www.cnblogs.com/1024Planet/p/5643651.html>

-------------------------------------------------------------

��Ҫ���������������
```cpp
void glClearColor(GLclampf red,
����������������    GLclampf green,
��������������������GLclampf blue,
��������������������GLclampf alpha);
```
��
```cpp
void glClear(GLbitfield mask);
```

glClear ���� mask��
* GL_COLOR_BUFFER_BIT����ɫ����
* GL_DEPTH_BUFFER_BIT����Ȼ���
* GL_STENCIL_BUFFER_BIT��ģ�建��

ǰһ���������ú������ɫ����������ǰһ���������úõĵ�ǰ�����ɫ���ô�����ɫ�� ʾ�����룺

```cpp
#include <stdio.h>
#include <gl/glut.h>

/*
 ����������ʹ��OpenGL�򵥻�һ�����Ρ�
 */

//���ģʽ��0-������ģʽ����0˫����ģʽ
#define OUTPUT_MODE 1

void display(void)
{
    //glClearColor�������ú������ɫ��glClear����glClearColor�������úõĵ�ǰ�����ɫ���ô�����ɫ
    glClearColor(1.0, 1.0, 0.6, 1.0);
    glClear(GL_COLOR_BUFFER_BIT);

    glRectf(-0.5f, -0.5f, 0.5f, 0.5f);

    if (OUTPUT_MODE == 0) {
        glFlush();//������GLUT_SINGLEʱʹ��
    } else {
        glutSwapBuffers();//��Ϊʹ�õ���˫����GLUT_DOUBLE�������������Ҫ��������Ż���ʾ
    }
}

int main(int argc, char *argv[])
{
    glutInit(&argc, argv);

    glutInitDisplayMode(GLUT_RGB | (OUTPUT_MODE == 0 ? GLUT_SINGLE : GLUT_DOUBLE));
    glutInitWindowPosition(100, 100);
    glutInitWindowSize(400, 400);
    glutCreateWindow("��һ�� OpenGL ����");
    glutDisplayFunc(&display);
    glutMainLoop();
    return 0;
}
```
