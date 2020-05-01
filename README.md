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

- Fungsi ```changPath()```
```void changePath(char *fpath, const char *path, int isWriteOper, int isFileAsked) {
  char *ptr = strstr(path, "/encv1_");
  int state = 0;
  if (ptr != NULL) {
    if (strstr(ptr+1, "/") != NULL) state = 1;
  }
  char fixPath[1000];
  memset(fixPath, 0, sizeof(fixPath));
  if (ptr != NULL && state) {
    ptr = strstr(ptr+1, "/");
    char pathEncvDirBuff[1000];
    char pathEncryptedBuff[1000];
    strcpy(pathEncryptedBuff, ptr);
    strncpy(pathEncvDirBuff, path, ptr-path);
    if (isWriteOper) {
      char pathFileBuff[1000];
      char pathDirBuff[1000];
      getDirAndFile(pathDirBuff, pathFileBuff, pathEncryptedBuff);
      decrypt(pathDirBuff, 0);
      sprintf(fixPath, "%s%s/%s", pathEncvDirBuff, pathDirBuff, pathFileBuff);
    } else if (isFileAsked) {
      char pathFileBuff[1000];
      char pathDirBuff[1000];
      char pathExtBuff[1000];
      getDirAndFile(pathDirBuff, pathFileBuff, pathEncryptedBuff);
      char *whereIsExtension = strrchr(pathFileBuff, '.');
      if (whereIsExtension-pathFileBuff <= 1) {
        decrypt(pathDirBuff, 0);
        decrypt(pathFileBuff, 0);
        sprintf(fixPath, "%s%s/%s", pathEncvDirBuff, pathDirBuff, pathFileBuff);
      } else {
        char pathJustFileBuff[1000];
        memset(pathJustFileBuff, 0, sizeof(pathJustFileBuff));
        strcpy(pathExtBuff, whereIsExtension);
        snprintf(pathJustFileBuff, whereIsExtension-pathFileBuff+1, "%s", pathFileBuff);
        decrypt(pathDirBuff, 0);
        decrypt(pathJustFileBuff, 0);
        sprintf(fixPath, "%s%s/%s%s", pathEncvDirBuff, pathDirBuff, pathJustFileBuff, pathExtBuff);
      }
    } else {
      decrypt(pathEncryptedBuff, 0);
      sprintf(fixPath, "%s%s", pathEncvDirBuff, pathEncryptedBuff);
    }
  } else {
    strcpy(fixPath, path);
  }
  if (strcmp(path, "/") == 0) {
    sprintf(fpath, "%s", dirpath);
  } else {
    sprintf(fpath, "%s%s", dirpath, fixPath);
  }
}
```
- Terdapat dua kondisi state, dengan defalut ```0```, state ini berfungsi untuk path yang hanya memiliki satu directory setelah encv_1 CONTOH: encv_sisop. dan ```1``` untuk path yang masih berlanjut setelah encv_1 CONTOH: encv1_/document/sisop
- Fungsi ```void changePath(char *fpath, const char *path, int isWriteOper, int isFileAsked) {``` akan melakukan pencarian terhadap file name dengan nama "encv1 _ "
- Fungsi ```if (ptr != NULL) {``` apabila tidak ada tulisan "/encv1_" ```if (strstr(ptr+1, "/") != NULL) state = 1;``` dan jika setelah "/encv1_" ada path lagi, statenya = 1 sesuai pada ketentuan kondisi state diatas. 
- Fungsi ```char fixPath[1000]; memset(fixPath, 0, sizeof(fixPath));```
untuk inisiasi buffer yang berfungsi sebagai tempat menyimpan proccessed path dari tiap kondisi yang berbeda. ```char fixPath[1000];``` memiliki arti yaitu mengatur buffer dengan set size memori dengan panjang array 1000. 
```if (ptr != NULL && state) {
ptr = strstr(ptr+1, "/");
char pathEncvDirBuff[1000];
char pathEncryptedBuff[1000];
strcpy(pathEncryptedBuff, ptr);
strncpy(pathEncvDirBuff, path, ptr-path);
```
- memiliki fungsi yaitu disini akan berjalan untuk kondisi (1.0 & 0.1) yang akan mendefinisikan dan mengisi buffer untuk directory dan encv1_ dan mengisi buffer yang telah disediakan. 

```if (isWriteOper) {
  char pathFileBuff[1000];
  char pathDirBuff[1000];
  getDirAndFile(pathDirBuff, pathFileBuff, pathEncryptedBuff);
  decrypt(pathDirBuff, 0);
  sprintf(fixPath, "%s%s/%s", pathEncvDirBuff, pathDirBuff, pathFileBuff);
} 
```
- saat kondisi ```(1.0)``` atau hanya ```isWriteOper``` yang memiliki nilai, if() disini akan mendefinisikan buffer untuk file dan directory dan menjalankan fungsi ```getDirAndFile()``` yang akan me-return directory dan file berdasarkan dari path yang ada di ```pathEncryptedBuff``` dan akan men ```decrypt()```nya yang selanjutnya akan di store di buffer ```fixpath[]``` pendeskripsian dan menyimpannya di dalam buffer ```fixPathsprintf(fixPath, "%s%s/%s", pathEncvDirBuff, pathDirBuff, pathFileBuff);``` dengan format (encv1_,pathdir,pathfile)

```
else if (isFileAsked) {
  char pathFileBuff[1000];
  char pathDirBuff[1000];
  char pathExtBuff[1000];
  getDirAndFile(pathDirBuff, pathFileBuff, pathEncryptedBuff);
  char *whereIsExtension = strrchr(pathFileBuff, '.');
  if (whereIsExtension-pathFileBuff <= 1) {
    decrypt(pathDirBuff, 0);
    decrypt(pathFileBuff, 0);
    sprintf(fixPath, "%s%s/%s", pathEncvDirBuff, pathDirBuff, pathFileBuff);
  } else {
    char pathJustFileBuff[1000];
    memset(pathJustFileBuff, 0, sizeof(pathJustFileBuff));
    strcpy(pathExtBuff, whereIsExtension);
    snprintf(pathJustFileBuff, whereIsExtension-pathFileBuff+1, "%s", pathFileBuff);
    decrypt(pathDirBuff, 0);
    decrypt(pathJustFileBuff, 0);
    sprintf(fixPath, "%s%s/%s%s", pathEncvDirBuff, pathDirBuff, pathJustFileBuff, pathExtBuff);
  }
 ```
Untuk kondisi lainnya yakni apabila hanya kondisi ```0.1``` ```else if (isFileAsked) {``` yang memiliki nilai, maka ```char pathFileBuff[1000];``` akan mendefinisikan file path, ```char pathDirBuff[1000];``` akan mendefinisikan directory path serta ```char pathExtBuff[1000];``` akan mendefinisikan ekstensi dari file tersebut. Kemudian, file path dan directory path akan disimpan terlebih dahulu ke dalam encrypted buffer menggunakan fungsi ```getDirAndFile(pathDirBuff, pathFileBuff, pathEncryptedBuff);```. Setelah melakukan penyimpanan path ke dalam buffer, kita akan melakukan ```char *whereIsExtension = strrchr(pathFileBuff, '.');``` pencarian ekstensi dari file melalui path yang telah ditentukan dengan melakukan pengecekan karakter ekstensi. Apabila ```if (whereIsExtension-pathFileBuff <= 1)``` {tidak ditemukan ekstensi sesuai dengan path yang ditentukan atau ditemukan ekstensi sesuai dengan path yang ditentukan sejumlah 1 karakter, maka akan dilakukan proses pendeskripsian ```decrypt(pathDirBuff, 0);``` pada directory path dan ```decrypt(pathFileBuff, 0);``` pada file path. Kemudian, hasil deskripsi tersebut disimpan ke dalam ```fixPath``` 
```sprintf(fixPath, "%s%s/%s", pathEncvDirBuff, pathDirBuff, pathFileBuff);```. dengan format "encv1 _ , directory path, file path". Begitupun apabila ```else {``` ditemukan ekstensi sesuai dengan path yang ditentukan sejumlah lebih dari 1 karakter, maka akan dilakukan penyimpanan ekstensi dari file tersebut ke dalam file path dan dilanjutkan dengan proses pendeskripsian ```decrypt(pathDirBuff, 0);``` pada directory path dan ```decrypt(pathFileBuff, 0);``` pada file path. Dan hasil deskripsi tersebut disimpan ke dalam ```fixPath``` 
```sprintf(fixPath, "%s%s/%s%s", pathEncvDirBuff, pathDirBuff, pathJustFileBuff, pathExtBuff);``` dengan format "__encv1 _ , directory path, file path, ekstensi".

