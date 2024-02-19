# Testing apakah sudah siap menggunakan VSCode sebagai code editor

Silahkan clone repo ini https://github.com/freeglut/freeglut. Buka explorer dan navigasikan ke tempat yang anda ingin taruh repo git ini. Misalkan disini `I:\Works`

![[Pasted image 20240218202543.png]]

Pada navigation bar (idk the proper name, silahkan ketik `powershell` kemudian enter
![[Pasted image 20240218202642.png]]

Akan langsung terbuka powershell di folder tersebut
![[Pasted image 20240218202724.png]]

Sekarang kita akan clone reponya

```powershell
git clone https://github.com/freeglut/freeglut
```

Setelah selesai kita dapat menghemat waktu untuk membuka VSCode dengan mengetik

```powershell
code freeglut
```

![[Pasted image 20240218202819.png]]

Akan muncul VSCode di folder sebelumnya dan akan muncul pop up untuk mengconfigure project. Silahkan pilih GCC.

![[Pasted image 20240218203122.png]]

Jika anda tanpa sengaja mengeklik pop up di kiri bawah atau kegiatan apapun yang membuat menu select kit hilang atau salah memilih compiler, silahkan membukanya lagi lewat CMake extension yang berada di navigation rail atau langsung bisa skip ke step selanjutnya (hanya untuk tidak sengaja menutup pop up)

![[select_kit.png]]

Di bawah akan banyak aktivitas yang dilakukan oleh extensionnya, tunggu hingga selesai (diakhiri dengan `Build files have been written to: xxxx`) kemudian pilih build di bar di bawah seperti gambar di bawah

![[build.png]]

Setelah build di tekan, [[CMake]] akan melakukan build library freeglut serta demo demonya. Proses ini membutuhkan waktu yang cukup lama untuk pertama kali. Setelah selesai (semoga tidak ada error), silahkan pergi menuju `build/bin/` untuk mencoba demo demo dari OpenGL freeglut. Selamat!

---