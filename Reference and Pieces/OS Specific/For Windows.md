Quick links untuk windows

1. [[For Windows#Instalasi Chocolatey|Chocolatey]]
2. [[For Windows#Instalasi CMake|CMake]]
3. [[For Windows#Instalasi VSCode|VSCode]]
	1. [[For Windows#Extension untuk CMake|Extension CMake]]
	2. [[For Windows#Extension untuk C/C++|Extension C/C++]]

# Instalasi Chocolatey

Guide instalasi [[Chocolatey]] ini menggunakan apa yang tertulis di https://chocolatey.org/install

Silahkan buka Powershell sebagai Admin
![[Pasted image 20240218160128.png]]

Akan ada prompt untuk memberikan akses sebagai administrator, silahkan pilih `Yes`. Pastikan shell menampilkan posisi di `system32` dan window name memiliki prefix `Administrator`. Jangan lihat backgroundnya berwarna hitam dan berasumsi ini CMD, ini hanya theme. Secara default, powershell memiliki theme dengan background berwarna biru.

![[Pasted image 20240218160428.png]]

Setelah anda sudah berada di posisi ini, silahkan copy dan paste kode ini.

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

Silahkan menunggu beberapa saat sampai proses selesai. Setelah selesai silahkan mengecek instalasi berhasil atau tidak dengan mengetik `choco`. Jika berhasil, akan mengeluarkan output versi dari chocolatey seperti ini:

![[Pasted image 20240218161013.png]]

# Instalasi CMake

Untuk instalasi [[CMake]] ini akan menggunakan [[Chocolatey]] agar lebih mudah. Silahkan run command ini sebagai **admin** untuk menginstall [[CMake]] 

```powershell
choco install cmake --installargs '"ADD_CMAKE_TO_PATH=System"'
```

Setelah menjalankan script di atas, akan ada prompt untuk mengeksekusi script tambahan, silahkan pilih `yes` dengan cara mengetik `Y`.

Jika anda terlanjur menginstall [[CMake]] menggunakan [[Chocolatey|choco]] tanpa memberikan installargs, silahkan ulangi command tersebut dengan ditambahkan `--force` diakhir command.

Untuk mengecek apakah sudah terinstall, silahkan **membuka window powershell baru** (tidak perlu sebagai admin) lalu mengetik `cmake` seperti berikut:

![[Pasted image 20240218163105.png]]

Jika tampil general usage dari [[CMake]], artinya instalasi telah berhasil, Selamat!
# Instalasi C/C++

Sangat disarankan untuk menggunakan [[MinGW]] versi terbaru, meskipun anda bisa menggunakan versi lama. Silahkan mengupgrade versi [[MinGW]] jika ada masalah pada saat melakukan compile test di akhir guide ini.

Sama seperti [[CMake]], kita juga akan menggunakan [[Chocolatey]]. Silahkan run command ini (seperti biasa, run menggunakan admin powershell dan jika ada prompt silahkan jawab `Yes`):

```powershell
choco install mingw
```
# Instalasi Git

Jika [[Git]] sudah terinstall silahkan lewati step ini. Seperti biasa, run menggunakan admin powershell dan jika ada prompt silahkan jawab `Yes`

```powershell
choco install git
```

# Instalasi VSCode

Saya menganggap kalian sudah menginstall VSCode

## Setup Extensions

Ada beberapa extension yang digunakan untuk mempermudah alur pengembangan aplikasi OpenGL menggunakan VSCode. Yang wajib adalah extension untuk [[CMake]] yang digunakan untuk melakukan configurasi [[MakeFile]] serta memilih compiler secara otomatis. Extensi untuk C/C++ dapat membantu alur pengembangan dengan memberikan suggestion serta melakukan type checking secara realtime.
### Extension untuk CMake
![[Pasted image 20240218202120.png]]
Name: CMake Tools
Id: ms-vscode.cmake-tools
Description: Extended CMake support in Visual Studio Code
Version: 1.17.15
Publisher: Microsoft
VS Marketplace Link: https://marketplace.visualstudio.com/items?itemName=ms-vscode.cmake-tools
### Extension untuk C/C++
![[Pasted image 20240218202033.png]]
Name: C/C++
Id: ms-vscode.cpptools
Description: C/C++ IntelliSense, debugging, and code browsing.
Version: 1.18.5
Publisher: Microsoft
VS Marketplace Link: https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools

![[Testing Development]]