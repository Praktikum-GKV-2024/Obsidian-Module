Modern GL masuk ke era modern pada versi 2.0 yang dikenalkan bahasa pemograman yang dapat berjalan di GPU. Bahasa ini disebut dengan GLSL atau GL shading language. Program yang berjalan di GPU ini disebut dengan [[Shader]]
# Modern Pipeline

Jika dilihat secara umum, Modern GL lebih terlihat kompleks jika dibandingkan dengan Legacy GL. Namun, dengan adanya tambahan kompleksitas ini kita mendapat kontrol lebih dalam proses penggambaran. Simpelnya di modern GL developer diberikan akses lebih untuk GPU.

Yang membedakan juga Legacy dan Modern GL adalah adanya shader

# Setup Tambahan
\* hanya untuk linux dan mac

# Membuat Window (Lagi)
Kali ini kita akan membuat window lagi, tapi menggunakan GLFW.

Kode untuk praktikum ini dapat diakses di sini https://github.com/Praktikum-GKV-2024/Template-GLFW
```cpp
#define GLM_FORCE_PURE
#include <glm/glm.hpp>

#include <GL/glew.h>
#include <GLFW/glfw3.h>
#include <iostream>
#include <string>
#include "MainScene.cpp"

int main(void) {
    GLFWwindow* window;

    /* Initialize the library */
    if (!glfwInit())
        return -1;

    /* Options untuk GLFW*/
    glfwWindowHint(GLFW_SAMPLES, 4);
	glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
	glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 0);
	// menggunakan OpenGL 3.0 (masih dapat menggunakan legacy)

    /* Create a windowed mode window and its OpenGL context */
    std::string window_title = "Template - GLFW";
    window = glfwCreateWindow(640, 480, window_title.c_str(), NULL, NULL);
    if (!window) {
        glfwTerminate();
        return -1;
    }

    /* Make the window's context current */
    glfwMakeContextCurrent(window);

    GLenum err = glewInit();
    glewExperimental = true;
    if (GLEW_OK != err) {
        /* Problem: glewInit failed, something is seriously wrong. */
        std::cerr << "Error: " << glewGetErrorString(err);
    }

    std::cout << "GL Version: " << glGetString(GL_VERSION) << std::endl;

    // ========================================================

	// Instantiate MainScene
    MainScene* scene = new MainScene(window);

    /* Loop until the user closes the window */
    while (!glfwWindowShouldClose(window)) {
		glClear(GL_COLOR_BUFFER_BIT);

        /* update frame of the scene */
        scene->update();

        /* Swap front and back buffers */
        glfwSwapBuffers(window);

        /* Poll for and process events */
        glfwPollEvents();
    }

    glfwTerminate();
    return 0;
}
```
# Menggambar Segitiga (Spoiler: It's a bit complicated)
If anything doesn't make sense you can consult https://docs.gl/ , search the function and select gl3

## Vertex Array Object (VAO)
Perkenalkan, ini adalah VAO. VAO merupakan **sebuah array yang menyimpan lokasi data [[Vertex]] di [[GPU]]**. Data ini tidak mesti hanya posisi, kita bisa memberikan data lain di sebuah [[Vertex]] misalkan [[UV]], [[Normal Map]], [[Texture Map]], dan masih banyak lagi.

Untuk mendapat bagaimana gambaran VAO, silahkan melihat canvas [[ModernGL Pipeline.canvas|ModernGL Pipeline]]

![[ModernGL Pipeline.png]]
Berikut merupakan contoh paling sederhana untuk membuat sebuah [[VAO]] yang nantinya akan di isikan data posisi untuk menggambar persegi.
```cpp
// vertecies yang di pass ke GPU
GLuint vao; // pointer di GPU yang akan menyimpan lokasi VAO 
float positions[] = {
	 0.5f,  0.5f, // 0
	 0.5f, -0.5f, // 1
	-0.5f, -0.5f, // 2
	-0.5f,  0.5f  // 3
};

// Initialize Vertex Array Buffer
glGenVertexArrays(1, &vao); // buat sebuah VAO di GPU dan simpan alamat di vao
glBindVertexArray(vao);     // aktifkan VAO

// Membuat vertex buffer
GLuint buffer;
glGenBuffers(1, &buffer);              // buat sebuah buffer
glBindBuffer(GL_ARRAY_BUFFER, buffer); // aktifkan buffer
glBufferData(GL_ARRAY_BUFFER, 4 * 2 * sizeof(float), positions, GL_STATIC_DRAW);
/*           ^^^^^^^^^^^^^^^  ^^^^^^^^^^^^^^^^^^^^^  ^^^^^^^^^  ^^^^^^^^^^^^^^
^^^^^^^^^^^^  Untuk VAO        Besar data yang akan   sumbernya  flag tambahan
Fungsi untuk                   dimasukkan ke GPU                 untuk bagaimana
memindahakan                   4 vertex dengan 2                 memori disimpan
data dari cpu                  komponen untuk masing
ke gpu                         masing vertex      
*/
```
Kode diatas akan membuat sebuah VAO dan menyimpannya ke dalam variabel `vao`. Kemudian kita mengaktifkannya. Kita juga membuat vertex buffer yang diisikan `positions` dari [[CPU]] ke [[GPU]].

Kita sudah berhasil membuat sebuah VAO dan memasukkan data vertex ke gpu, tapi mereka belum saling terhubung. Kita perlu memberikan instruksi ke [[GPU]] cara bagaimana membaca data tersebut. Kita dapat menggunakan [[Vertex Attribute Pointer]] untuk melakukannya 
### Vertex Attribute Pointer
Vertex Attribute Pointer berfungsi **memberitahu [[GPU]] cara membaca data** sehingga data dapat digunakan dengan mudah saat menggunakan shader. Berikut contoh penggunaannya untuk memberi tahu [[GPU]] cara membaca atribut posisi dari contoh sebelumnya.
```cpp
glEnableVertexAttribArray(0);
glVertexAttribPointer(
	0, // posisi merupakan index 0
	2, // 2 komponen
	GL_FLOAT, // bertipe data float
	GL_FALSE, // Tidak perlu dilakukan normalisasi
	5 * sizeof(float), // stride (besar satuan vertex)
	0 // Tidak ada offset karena di awal
);
```
## Index Buffer Object
Satu lagi yang penting adalah Index Buffer Object atau IBO. IBO berguna untuk **memberi tahu GPU bagaimana cara menggambar objek primitif**. Berikut merupakan contoh penggunaan IBO untuk menggambar persegi untuk contoh ini.

Kita memiliki 4 titik, index dimulai dari 0 agar konsisten dengan kode nantinya.

0. `0.5,  0.5`
1.  `0.5, -0.5`
2. `-0.5, -0.5`
3. `0.5,  0.5`

Untuk menggambar persegi, kita perlu menggambar 2 segitiga. Salah satu caranya adalah sebagai berikut:

![[Pasted image 20240227083136.png]]

Kita menggambar segitiga pertama menggunakan titik 0, 1, dan 2 (biru). Kemudian segitiga kedua menggunakan titik 2, 3, dan 0 (magenta)
```cpp
unsigned int indices[] = {
	0, 1, 2,
	2, 3, 0
};

glGenBuffers(1, &ibo);
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, ibo);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, 3 * 2 * sizeof(unsigned int), indices, GL_STATIC_DRAW);
```

Mirip dengan vertex buffer, kita mengirim data ke [[GPU]] menggunakan bind buffer, lalu mengisinya dangan buffer data. Yang berbeda adalah jenisnya bukan `GL_ARRAY_BUFFER` tetapi `GL_ELEMENT_ARRAY_BUFFER`. yang berbeda lainnya adalah tipe datanya **harus `unsigned int` dan tidak bisa diganti dengan yang lain**.
## Shader
Huft membahas VAO, Vertex attribute pointer, dan IBO sudah cukup melelahkan. Tinggal satu step lagi! Yakni shader! Untuk membaca shader lebih lanjut silahkan buka [[Shader|Catatan ini]]

Kita pertama akan membaca file shader menggunakan fungsi `LoadShaders` yang sediakan `<common/shader.hpp>`. Silahkan untuk mengecek filenya untuk mengetahui cara load shadernya (opsional).
```cpp
GLuint programId = LoadShaders("res/shader/super_basic.vs", "res/shader/super_basic.fs");
```
Hasil balikan dari `LoadShaders` ini adalah id dari program yang berasal dari kode shader yang sudah di compile. Program shader ini akan digunakan sebelum menggambar ke layar.
### Vertex Shader
Silahkan buka file `res/shader/super_basic.vs`.  Ini merupakan kode shader paling simpel untuk menggambar sebuah segitiga. Masih ingat dengan vertex attribute pointer? kita memilih index 0 untuk posisi. Begitu pula cara membacanya di shader. Disini kita memberikan keyword `layout(location = 0) in` untuk menandakan ini merupakan input dari VAO.

Kode ini sebenarnya tidak melakukan proses apapun terhadap vertex, kita hanya melanjutkan data position ke proses selanjutnya. (kita akan melakukan manipulasi vertex di praktikum selanjutnya). 
```c
#version 330 core

// Input vertex data, different for all executions of this shader.
layout(location = 0) in vec4 position;

void main(){
	gl_Position = position;
}
```

### Fragment Shader
Silahkan buka file `res/shader/super_basic.fs`.  Ini merupakan kode fragment shader paling simpel untuk menggambar sebuah segitiga. Kita hanya memberikan warna statik, yakni merah untuk setiap pixel yang di tempati oleh persegi (atau 2 segitiga) yang kita gambar
```c
#version 330 core

// Output data sebuah warna
layout(location = 0) out vec4 color;

void main(){
	// Output color, warna yang akan digambar ke layar
	color = vec4(1., 0., 0., 1.);
}
```

## Saatnya menggambar
Silahkan check `IBO.cpp` bagian method `update`
```cpp
/* do every frame here*/
glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
        
glUseProgram(programId);

glBindVertexArray(vao);
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, ibo);

glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, nullptr);
```
Disini kita menggunakan program yang tadi sudah diload dari shader dan buffer yang sudah kita buat dan kirim ke GPU. Kita juga menggunakan IBO yang sudah dibuat sebelumnya. Untuk mengirim perintah ke GPU untuk menggambar perseginya kita akan memanggil `glDrawElements` dengan parameter `GL_TRIANGLES` untuk menggunakan segitiga dan 6 untuk menggambar sebanyak 6 vertex.


Voila! Persegi merah berhasil digambar!
![[Pasted image 20240227100042.png]]
# Tugas Pertemuan 2
Silahkan modifikasi kode IBO yang berada di template yang disediakan. Kriteria modifikasi yang dilakukan minimal adalah:
- Buat minimal 4 segitiga 
- Ganti warna segitiga (bisa menggunakan shader, pass data dari VAO, atau keduanya)

Clue / tasklist:
1. Ganti dan tambahkan vertex data
2. Sesuaikan IBO untuk bentuk yang kamu ingin gambar
3. Ganti size untuk VAO `glBufferData(GL_ARRAY_BUFFER, size, positions, GL_STATIC_DRAW);`
4. Ganti size untuk IBO `glBufferData(GL_ELEMENT_ARRAY_BUFFER, size, indices, GL_STATIC_DRAW);`
5. Ganti jumlah vertex yang digambar `glDrawElements(GL_TRIANGLES, n_vertex, GL_UNSIGNED_INT, nullptr);`

> Tantangan untuk pertemuan 2. Silahkan modifikasi kode sehingga bisa menampilkan bentuk seperti berikut. 

![[Pasted image 20240227095359.png]]
Untuk posisi titik dan meshnya silahkan cek disini https://www.geogebra.org/calculator/cwx52vqb.

Clue:
- Modifikasi buffer position agar juga menerima warna
- Modifikasi Vertex Attribute Pointer untuk index 1, dan sesuaikan dengan layout buffer
- Susun IBO agar bisa menggambar segitiganya
- Jangan lupa untuk mengganti angka untuk besar vertex buffer dan IBO
- Modifikasi Vertex shader agar bisa menerima layout index 1, kemudian teruskan ke Fragment Shader
- Gunakan warna yang diberikan Vertex shader untuk output warna