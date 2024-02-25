Banyak orang sering mendengar shader di dalam game. Mungkin yang pertama kali yang dipikirkan adalah tentang bayangan karena _shade_ atau _shadow_, tapi tidak. Shader merupakan sebuah program yang dieksekusi di [[GPU]], tidak lebih tidak kurang. Shader dapat melakukan banyak hal selain dari memberikan _shadow_ pada dunia.

Kenapa [[GPU]]? Karena [[GPU]] memiliki kemampuan untuk menjalankan program secara paralel secara masif. Kita berbicara ribuan prosesor di [[GPU]] vs hanya belasan di [[CPU]] disini.

Shader sangat cocok untuk grafik di mana tiap unit proses yang dilakukan selalu sama sehingga paralelisasi sangat membantu performa. Misalkan kita memproses warna apa yang perlu ditampilkan di layar. Di belakang proses ini akan ada loop untuk memproses setiap pixel yang ada. Proses ini sangat amat repetitif yang membuat [[CPU]] kewalahan dan lahirlah [[GPU]] untuk membantu pekerjaan [[CPU]] ini.
