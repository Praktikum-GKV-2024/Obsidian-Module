```cpp
glEnableVertexAttribArray(1);
glVertexAttribPointer(
	1, // UV merupakan index 1
	2, // 2 komponen (UV x, UV y)
	GL_FLOAT, // bertipe data float
	GL_FALSE, // Tidak perlu dilakukan normalisasi
	5 * sizeof(float), // stride (besar satuan vertex)
	(void *) (3 * sizeof(float)) // Offset UV Data
);
```