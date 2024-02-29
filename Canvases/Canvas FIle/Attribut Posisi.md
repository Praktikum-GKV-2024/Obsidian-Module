```cpp
glEnableVertexAttribArray(0);
glVertexAttribPointer(
	0, // posisi merupakan index 0
	3, // 3 komponen (x, y, z)
	GL_FLOAT, // bertipe data float
	GL_FALSE, // Tidak perlu dilakukan normalisasi
	5 * sizeof(float), // stride (besar satuan vertex)
	0 // Tidak ada offset karena di awal
);
```