Pertemuan ini berisikan pengenalan cara menggambar dengan [[OpenGL]] dengan bantuan wrapper [[GLUT]].

# Setup

![[Setup - FreeGLUT]]

Untuk mengikuti praktikum ini, silahkan clone [repo ini](https://github.com/Praktikum-GKV-2024/Template-FreeGLUT) disana sudah terdapat kode yang dituliskan dibawah. Alur pengembangannya sama seperti test di setup. Cukup clone, configure, ubah kode lalu build and run.
# Sejarah OpenGL

OpenGL pertama kali dikembangkan di tahun 1992 sebagai alternatif open source dari graphics API [[IrisGL]] yang bersifat proprietary. 

-tbd #TODO

# Kenapa lab kita tidak menggunakan glut?

1. Di cite langsung dari [Khronos](https://www.khronos.org/opengl/wiki/Legacy_OpenGL#Reasons_to_avoid_legacy_OpenGL)(yang menulis spesifikasi OpenGL).
2. Ilmu yang kamu pelajari ga bisa diapply di teknik industri game.
3. Resource belajar yang sedikit.
4. OpenGL Legacy died in 2004! And we still use it!??? WHY!???
5. Dengan modern OpenGL kita bisa menggunakan [[Emscripten]] untuk translasi kode c/c++ ke js sehingga bisa run di browser.
6. Pengetahuan kalian tentang modern OpenGL (lebih tepatnya ilmu shader) bisa dimanfaatkan di bidang game development dan pemahaman tentang bagaimana paralelisasi di GPU bisa dimanfaatkan di Sistem Cerdas mengenai training model.
7. Just my hate from my praktikum (tehee~)

---
# Legacy OpenGL
Tapi kita akan mengenalkan kalian dulu bagaimana OpengGL 1.x terlebih dahulu kalian sebagai pengantar ke OpenGL Modern.

-tbd #TODO
## Membuat Window 

Hal yang paling dasar dalam membuat aplikasi adalah membuka window. Disini GLUT bekerja hanya untuk membantu kita untuk membuatkan window. Terlihat pekerjaan yang mudah bukan? Tidak semudah itu ferguso. Dibalik pengerjaan pembuatan window ini sangat spesifik untuk setiap operating system. Di Windows kita perlu memanggil win32 api bernama `gdi`, di Linux kita dapat memilih untuk menggunakan `wayland` dan `X11`, di MacOS kita bisa menggunakan `X11` dan `XQuartz`. Huft... menulisnya saja sudah terdengar melelahkan, untung ada [[GLUT]]!.

Berikut merupakan kode paling simpel dari GLUT

```cpp
void update(void) {
    glClear(GL_COLOR_BUFFER_BIT);
    glFlush();
}

int main(int argc, char* argv[])  {
    glutInit(&argc, argv);
    glutInitWindowSize(640, 480);
    glutInitDisplayMode(GLUT_SINGLE | GLUT_RGBA);
    glutCreateWindow("FreeGLUT");
    glutDisplayFunc(update);
    glClearColor(0.0f, 0.0f, 1.0f, 1.0f);
    glutMainLoop();
    
    return 0;
}
```

Untuk membuat window, kita pertama memerlukan inisiasi [[GLUT]], variable argc dan argv ini diambil dari terminal saat memanggil program, baca lebih lanjut opsi yang bisa diberikan [disini](https://www.opengl.org/resources/libraries/glut/spec3/node10.html)

Setelah menginisiasi, kita bisa memberikan windows size, `640` untuk width, dan `480` untuk `height`

Selanjutnya kita akan menginisiasi opsi dari canvas yang digunakan. Yang biasa kita gunakan adalah jenis canvas yang akan digunakan, silahkan baca lebih lanjut disini [lebih lanjut](https://www.opengl.org/resources/libraries/glut/spec3/node12.html). Untuk pembahasan lebih lanjut tentang buffer silahkan cek pembahasan tentang [[Video Buffer|video buffer]]

`glutCreateWindow` digunakan untuk mengeset nama window dari program kita

`glutDisplayFunc` digunakan untuk mengeset fungsi yang akan dipanggil setiap loop selesai. Pastikan nama fungsi saja, tidak dipanggil menggunakan `()`, nama fungsi yang dipanggil dengan diberikan referencenya ini disebut dengan `callback function`.

`glClearColor` berfungsi untuk mengeset default warna kanvas dari program

`glutMainLoop` ini merupakan loop dari program kita, kita bisa anggap fungsi ini akan memanggil 
```c
while (true) update()
```

## Menggambar Primitives
Semua primitive di OpenGL menggunakan paradigma begin dan end (kecuali rectangle dan wrapper [[GLUT]]) Kita pertama perlu menyebutkan jenis primitif yang digambarkan menggunakan `glBegin` dengan parameter contant primitive OpenGL legacy (silahkan cek [disini](https://www.khronos.org/opengl/wiki/Primitive) untuk lebih lengkap)
### Point
```cpp
void point(void) {
    glClear(GL_COLOR_BUFFER_BIT);
    glPointSize(10.0f);
    
    glColor3f(1.0f, 0.0f, 0.0f);
    glBegin(GL_POINTS);
        glVertex3f(0, 0, 0.0);    // titik 1

        glVertex3f(0, 0.1, 0.0);  // titik 2

        glVertex3f(0, 0.2, 0.0);  // titik 3

        glVertex3f(0, 0.3, 0.0);  // titik 4

        glVertex3f(0, 0.4, 0.0);  // titik 5
        
        glVertex3f(0, 0.5, 0.0);  // titik 6
    glEnd();

    glFlush();
}
```
Contoh di atas akan menggambar 6 titik dengan vertex yang diberikan di antara begin dan end. Untuk menggambar titik kita perlu memberikan parameter constant `GL_POINTS` ke `glBegin`. 

Untuk mengatur besarnya titik kita dapat menggunakan `glPointSize` dengan parameter `double`yang menentukan besarnya titik. Untuk memberikan warna kita bisa menggunakan `glColor`dengan parameter `R`, `G`, dan `B` untuk warnanya. Untuk apa arti dari akhirannya dalam contoh ini `f` anda bisa membaca lebih lanjut di [[C Data Types]] .

Hasil:
![[Pasted image 20240224225209.png]]

### Garis
```cpp
void garis(void) {
    glClear(GL_COLOR_BUFFER_BIT);
    glLineWidth(2.0f);

    glBegin(GL_LINES);
        // garis pertama warna merah
        glColor3f(1.0f, 0.0f, 0.0f);
        glVertex3f(0.5,  0.20, 0.0);
        glVertex3f(0.5, -0.20, 0.0);
        
        // garis kedua warna hijau
        glColor3f(0.0f, 1.0f, 0.0f);
        glVertex3f(-0.5,  0.20, 0.0);
        glVertex3f(-0.5, -0.20, 0.0);
    glEnd();

    glFlush();
}
```

Serupa dengan titik, kita hanya mengganti begin menjadi `GL_LINES`. Untuk garis ini untuk valid di gambar kita memerlukan setidaknya 2 vertex (jumlahnya genap). Jika kurang, maka vertex terakhir akan diabaikan. Kita juga bisa mengganti warna di tengah menggambar dan akan tetap bekerja.

Hasil:
![[Pasted image 20240224225226.png]]
### Segitiga
```cpp
void segitiga(void) {
    glClear(GL_COLOR_BUFFER_BIT);
    
    glBegin(GL_TRIANGLES);
        // vertex 1
        glColor3f(1.0f, 0.0f, 0.0f);
        glVertex3f(-0.10, -0.10, 0.00);
        
        // vertex 2
        glColor3f(0.0f, 1.0f, 0.0f);
        glVertex3f(0.10, -0.10, 0.00);

        // vertex
        glColor3f(0.0f, 0.0f, 1.0f);
        glVertex3f(0.00, 0.10, 0.00);
    glEnd();
    
    glFlush();
}
```
Untuk menggambar segitiga kita memberikan `GL_TRIANGLES` untuk begin. Pada contoh ini kita mengganti warna pada setiap vertex. Hasil yang didapatkan adalah segitiga yang memiliki warna yang bercampur. Ini dapat terjadi karena GPU akan melakukan interpolasi dari tiga titik diskrit yang kita berikan. Interpolasi ini dilakukan dari 3 titik sekaligus dengan teknik yang bernama [barycentric interpolation](https://en.wikipedia.org/wiki/Barycentric_coordinate_system). Simplenya ini merupakan [[Linear Interpolation|linear interpolation]] untuk 3 titik sekaligus
![[Pasted image 20240224225259.png]]
### Persegi
```cpp
void persegi_panjang(void) {
    glClear(GL_COLOR_BUFFER_BIT);

    glColor3f(1.0f, 0.0f, 0.0f);
    glRectf(-0.18, -0.18, 0.18, 0.18);

    glFlush();
}

```
Tidak seperti primitive lain yang membutuhkan begin dan end, persegi dapat menggunakan `glRect` dengan menyuplai 4 titik koordinat dari persegi tersebut. 2 argumen pertama merupakan titik pertama `(x1, y1)` dan argumen kedua merupakan titik kedua `(x2, y2)`

![[Pasted image 20240224225309.png]]


# Afterthoughts
- [ ] Mungkin bisa ditambah tugas opsional
- [ ] Memindahkan sejarah ke file lain
- [ ] Contoh lain yang lebih menarik

# Closing
Jika ada yang dirasa salah dari modul ini, silahkan kontak discord: `shariyl` lewat PC ataupun group Discord.