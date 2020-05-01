# SoalShiftSISOP20_modul4_T09

Nama Anggota:
- Robby Irvine Surya 05311840000023
- Widyantari Febiyanti 05311840000030

# Soal 1
Jasir, pekerja baru yang jenius, akan membuat program yang mengimplementasikan 2 (dua) buah metode enkripsi yang berjalan pada directory. Pada metode enkripsi pertama "encv1 _ " dirancang dengan detail filesystem berikut ini :
- Jika terdapat sebuah directory yang dibuat dan di-rename dengan awalan kata "encv1 _ ", maka directory tersebut akan terenkripsi menggunakan metode enkripsi pertama "encv1 _ ".
- Jika terdapat sebuah directory terenkripsi yang di-rename, maka directory tersebut menjadi tidak terenkripsi.
- Setiap pembuatan directory terenkripsi baru (mkdir ataupun rename) akan tercatat ke sebuah database/log dalam file.
- Proses pengenkripsian dalam metode enkripsi pertama ini menggunakan metode enkripsi caesar cipher dengan key
      ```9(ku@AW1[Lmvgax6q`5Y2Ry?+sF!^HKQiBXCUSe&0M.b%rI'7d)o4~VfZ*{#:}ETt$3J-zpc]lnh8,GwP_ND|jO```
- Metode enkripsi pada suatu direktori juga berlaku kedalam direktori lainnya yang ada didalamnya.
Note: Metode enkripsi pertama ini mengabaikan karakter ‘/’ pada penamaan file dan ekstensi dari file.

# Penyelesaian
untuk membuat program yang akan mengenkripsi suatu nama file/folder, dibutuhkan 3 kriteria yang akan dibuat system call yaitu: 

- ```create```
: adalah sebuah kondisi proses rename di directory sesuai dengan nama yang ditentukan.
- ```mkdir```
: adalah perintah yang berfungsi untuk membuat direktori tujuan.
- ```write``` 
: adalah sebuah kondisi proses akan menyimpan jika mendeteksi adanya perubahan pada database/log file. 

Setiap system call akan menggunakan fungsi ```changePath()``` ```getDirAndFile()``` ```decrypt()``` yang berfungsi untuk dekripsi dan melakukan pengambilan juga pengecekan untuk setiap pathfile extension dan directory. 
