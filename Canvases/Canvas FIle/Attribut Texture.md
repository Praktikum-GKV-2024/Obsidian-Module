```cpp
glEnableVertexAttribArray(2);
glVertexAttribPointer(
	2, // Texture merupakan index 2
	2, // 2 komponen (Tex x, Tex y)
	GL_FLOAT, // bertipe data float
	GL_FALSE, // Tidak perlu dilakukan normalisasi
	5 * sizeof(float), // stride (besar satuan vertex)
	(void *) (5 * sizeof(float)) // Offset Texture Data
);
```