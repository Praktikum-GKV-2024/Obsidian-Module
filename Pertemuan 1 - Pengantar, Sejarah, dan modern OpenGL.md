Pertemuan ini berisikan pengenalan cara menggambar dengan [[OpenGL]] dengan bantuan wrapper [[GLUT]].

# Setup

![[Setup]]

# Sejarah OpenGL

OpenGL pertama kali dikembangkan di tahun 1992 sebagai alternatif open source dari graphics API [[IrisGL]] yang bersifat proprietary. 

= to be discussed = #TODO 
# Membuat Window

Hal yang paling dasar dalam membuat aplikasi adalah membuka window. 

```C
int main(int argc, char* argv[])  {
    glutInit(&argc, argv);
    glutInitWindowSize(640, 480);
    glutInitDisplayMode(GLUT_SINGLE | GLUT_RGBA);
    glutCreateWindow("biru");
    glutDisplayFunc(Jendela);
    glClearColor(0.0f, 0.0f, 1.0f, 1.0f);
    glutMainLoop();
    
    return 0;
}
```


