Untuk melakukan [[Commit]], ada beberapa data yang wajib anda masukkan. Beberapa data tersebut:
- Email
- Nama
- Pesan commit

Permasalahan yang umum dilakukan pengguna baru git adalah lupa mengeset informasi email dan nama ini dan mendapatkan pesan warning mirip seperti ini:

![[Pasted image 20240301002415.png]]

Tenang saja tidak usah down, semua pengguna git pasti setidaknya pernah mendapatkan pesan ini.

Solusinya sesuai dengan yang dikatakan,
```powershell
git config --global user.email "email github kamu"
git config --global user.name "nickname untuk ditinggal di commit"
```

Dengan mensubstitusi field yang ada di dalam petik dua