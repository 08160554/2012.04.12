# 2012.04.12
WEEK08

01
----------
```c
#include <opencv/highgui.h> ///使用 OpenCV 2.1 比較簡單, 只要用  High GUI 即可
#include <opencv/cv.h>
#include <GL/glut.h>
void init()
{
    IplImage * img = cvLoadImage("01.jpg");   ///OpenCV讀圖
    cvCvtColor(img,img, CV_BGR2RGB);   ///OpenCV轉色彩 (需要cv.h)
    glEnable(GL_TEXTURE_2D); ///開啟貼圖功能
    GLuint id;    ///準備一個 unsigned int 整數, 叫 貼圖ID
    glGenTextures(1, &id);    /// 產生Generate 貼圖ID
    glBindTexture(GL_TEXTURE_2D, id); ///綁定bind 貼圖ID
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT);    /// 貼圖參數, 超過包裝的範圖T, 就重覆貼圖
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT);    /// 貼圖參數, 超過包裝的範圖S, 就重覆貼圖
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_NEAREST);   /// 貼圖參數, 放大時的內插, 用最近點
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_NEAREST);   /// 貼圖參數, 縮小時的內插, 用最近點
    glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, img->width, img->height, 0, GL_RGB, GL_UNSIGNED_BYTE, img->imageData);
}       ///貼圖影像的資料都設定好

void display()
{
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
    glBegin(GL_POLYGON);
        glTexCoord2f(0,1); glVertex3f(-1,-1,0);
        glTexCoord2f(1,1); glVertex3f(+1,-1,0);
        glTexCoord2f(1,0); glVertex3f(+1,+1,0);
        glTexCoord2f(0,0); glVertex3f(-1,+1,0);
    glEnd();
    glutSolidTeapot(0.3);
    glutSwapBuffers();
}
int main(int argc,char** argv)
{
    glutInit(&argc,argv);
    glutInitDisplayMode(GLUT_DOUBLE|GLUT_DEPTH);
    glutCreateWindow("08160554");
    init();
    glutDisplayFunc(display);
    glutMainLoop();
}
```

02
-------
```c
#include <opencv/highgui.h> 
#include <opencv/cv.h>
#include <GL/glut.h>
GLUquadric * quad;///TODO: Quad
void init()
{
    IplImage * img = cvLoadImage("02.jpg"); 
    cvCvtColor(img,img, CV_BGR2RGB); 
    glEnable(GL_TEXTURE_2D);
    GLuint id; 
    glGenTextures(1, &id);
    glBindTexture(GL_TEXTURE_2D, id); 
    glTexParameteri(GL_TEXTURE_2D,GL_TEXTURE_WRAP_S, GL_REPEAT); 
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT);
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_NEAREST); 
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_NEAREST);
    glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, img->width, img->height, 0, GL_RGB, GL_UNSIGNED_BYTE, img->imageData);
    quad = gluNewQuadric();///TODO: Quad
} 

float angle=0;
void display()
{
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
    glPushMatrix();///自動轉
        glRotatef(90, 1,0,0);
        glRotatef(angle, 0,0,1);///自動轉
        gluQuadricTexture(quad, 1);///TODO:
        gluSphere(quad,0.5,30, 30);///glutSolidTeapot(0.3);
    glPopMatrix();///自動轉
    ///glutSolidTeapot(0.3);
    glutSwapBuffers();
    angle++;}
int main(int argc, char** argv)
{
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_DOUBLE | GLUT_DEPTH);
    glutCreateWindow("week07 texture");
    glutDisplayFunc(display);
    glutIdleFunc(display);
    glEnable(GL_DEPTH_TEST);  ///有3D的深度測試(前面會蓋掉後面)
    init();///上面把OpenGL都設好後, 才設定 OpenCV 的貼圖到 OpenGL上面
    glutMainLoop();
}

```
