Dari pertemuan sebelumnya kita menemukan beberapa masalah dan kesulitan:
- Hasil yang digambar ke layar menyesuaikan aspect ratio dari window yang pertama kali diinisialisasi
-  Memerlukan memasukkan data vertex secara manual yang melelahkan dan rawan salah

Praktikum kali ini akan membahas cara untuk menyelesaikan masalah tersebut. Kita akan mulai dari membenarkan tampilan.

# Glossarium
- Dalam catatan ini akan disebut Model dan Objek yang merujuk ke hal yang sama

# MVP Matrix
MVP Matrix merupakan sebuah matrix yang menyimpan transformasi komposit Model -> View -> Projection. Kita akan membahas tiap komponen matrix ini di bagian selanjutnya. MVP Matrix ini menyimpan informasi dimana **posisi vertex akan ditampil di layar**. Kita akan membahas satu persatu mulai dari model
## Model Matrix
Model Matrix menyimpan informasi dimana objek berada di dunia (virtual, akan disebut dengan [[World Space]] untuk selanjutnya). Matriks ini terdiri dari 3 komponen, translasi, rotasi, dan skala. Jika tiap posisi vertex model dikalikan dengan matriks ini, yang dihasilkan adalah posisi model di [[World Space]].

Jika dirumuskan, rumusnya akan seperti ini

$$
 \begin{align}
	 \begin{aligned}
	    M         &= T \cdot R \cdot S\\
	    v_{world} &= M \cdot v_{model} \\
	 \end{aligned}
 \end{align}
$$
dengan

- $T$ : Matrix translasi
- $R$ : Matrix rotasi
- $S$ : Matrix scale
- $M$ : Model Matrix
- $v_{world}$ : Vertex posisi model yang berada di [[World Space]]

## View Matrix

Setelah model sudah berada di [[World Space]], kita selanjutnya akan menyesuaikan posisi Model terhadap kamera sehingga kamera ada di origin dari dunia. Sama seperti objek biasa, kamera juga merupakan objek yang memiliki [[Model Matrix]]. Bagaimana cara membuat kamera menjadi origin? Dengan melakukan inverse [[Model Matrix]] milik kamera. 

Berikut merupakan rumus untuk [[View Matrix]]:

$$
 \begin{align}
	 \begin{aligned}
	    V          &= C^{-1}\\
	    v_{camera} &= V \cdot v_{world} \\
	 \end{aligned}
 \end{align}
$$
dengan:
- $C$ : Model Matrix milik Camera
- $C^{-1}$ : Hasil inverse model matrix camera
- $V$ : Matrix View
- $v_{camera}$ : Vertex posisi model yang berada di [[Camera Space]]

## Projection Matrix
Matrix yang terakhir adalah Projection Matrix. Matrix ini digunakan untuk meprojeksikan dari [[Camera Space]] ke [[Clip Space]]. Apa itu [[Clip Space]]? Clip space merupakan space terakhir dalam rendering process yang siap ditampilkan ke layar. Mungkin penjelasan tersebut masih terlalu *vague*, contoh dari projection matrix adalah [[Perspective]] dan [[Orthographic]]. 

Berikut merupakan contoh projeksi perspektif. Projeksi ini seperti bagaimana kita melihat dunia. Kita dapat melihat depth dari objek yang diprojeksikan.

![[Pasted image 20240229154637.png]]
![[Pasted image 20240229155415.png]]
Berbeda dengan [[Orthographic]], projeksi ini membuat layar menerima silhouette dari   

![[Pasted image 20240229154610.png]]
![[Pasted image 20240229155355.png]]

Projection lain yang tidak sepopuler dua di atas ada [[Gnomonic]] dan [[Stereographic]] untuk menggambar non-euclidean space. 

Rumus dari matrix projection ini adalah:

$$
 \begin{align}
	 \begin{aligned}
	    v_{world}     &= M \cdot v_{model} \\
	    v_{camera}    &= V \cdot M \cdot v_{model} \\
	    v_{clip}      &= P \cdot V \cdot M \cdot v_{model}
	 \end{aligned}
 \end{align}
$$
dengan:
- $v_{clip}$ : Vertex posisi akhir yang berada di [[Clip Space]]

## Normalize Device Coordinates (optional)


# Advanced Shader 🔥
Dari teori yang kita bahas serta implementasi MVP matrix dari sisi GPU nya, kita akan membahas lebih lanjut untuk menggunakannya di Shader

## More on Vertex Shader

### Lighting on Vertex Shader

## More on Fragment Shader

### Lighting on Fragment Shader


![[Pasted image 20240307024455.png]]