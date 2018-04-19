# Stack dan deklarasi variable

Dalam bahasa C kita telah mengetahui bahwa sebelum kita menggunakan variable kita harus lebih dahulu mendeklarasikan variable tersebut. Mendeklarasikan artinya menentukan tipe data dari variable sebelum digunakan. Kenapa harus menentukan tipe datanya ?, karena hal tersebut dibutuhkan oleh compiler untuk mengalokasikan memory untuk variable lokal.

Pada low level, ketika sebuah fungsi dipanggil, maka stack akan berkembang (bertambah ukurannya) sesuai dengan ukuran total dari variable lokal yang diperlukan. Untuk itulah compiler membutuhkan informasi tentang tipe data dari variable, agar ketika program mengalokasikan memory baru untuk variable lokal ukurannya pas, tidak lebih, tidak kurang.

Pada level assembly, variable lokal akan disimpan di area memory yang bernama stack. Setiap fungsi mempunyai area stacknya sendiri - sendiri. Stack akan dialokasikan ketika sebuah fungsi dipanggil dan akan didealokasikan ketika fungsi tersebut berakhir (return). 

Pada bagian sebelumnya tentang function prologue, kita telah mengetahui bahwa program menggunakan instruksi pada function prologue untuk membuat ruang stack baru, dan menghilangkan ruang stack tersebut (ketika fungsi berakhir) dengan function epilogue.

Sekarang kita akan mempelajari bagaimana variable lokal itu disimpan. Kita menggunakan script hello.c di bagian sebelumnya (function prolog) sebagai c
ontoh.

``` c
#include <stdio.h>

int main(void)
{
    int x = 10;
    printf("Hello world\n");
    return 0;
}
```

Kita compile dan lihat assemblynya.

```
$ gcc -o hello hello.c
$ gdb -batch -ex 'file hello' -ex 'disas main'
Dump of assembler code for function main:
   0x00000000004004e7 <+0>:     push   rbp
   0x00000000004004e8 <+1>:     mov    rbp,rsp
   0x00000000004004eb <+4>:     sub    rsp,0x10
   0x00000000004004ef <+8>:     mov    DWORD PTR [rbp-0x4],0xa
   0x00000000004004f6 <+15>:    lea    rdi,[rip+0x97]        # 0x400594
   0x00000000004004fd <+22>:    call   0x4003f0 <puts@plt>
   0x0000000000400502 <+27>:    mov    eax,0x0
   0x0000000000400507 <+32>:    leave  
   0x0000000000400508 <+33>:    ret    
End of assembler dump.
```

Didalam kode C, kita meng-assign variable x dengan integer 10. Lalu, kita lihat instruksi pada lokasi main+8, disana terdapat instruksi `mov DWORD PTR [rbp-0x4],0xa`, kita tahu 0xa adalah bilangan hex dari 10. integer 10 akan disimpan ke alamat [rbp-0x4]. Bisa kita simpulkan disini bahwa variable x berada di alamat rbp-0x4.

Kita modifikasi sedikit kode C diatas menjadi,

``` c
#include <stdio.h>

int main(void)
{
    int x = 10;
    int y = 20;
    printf("Hello world\n");
    return 0;
}
```

Output assembly adalah

```
$ gcc -o hello hello.c
$ gdb -batch -ex 'file hello' -ex 'disas main'
Dump of assembler code for function main:
   0x00000000004004e7 <+0>:     push   rbp
   0x00000000004004e8 <+1>:     mov    rbp,rsp
   0x00000000004004eb <+4>:     sub    rsp,0x10
   0x00000000004004ef <+8>:     mov    DWORD PTR [rbp-0x4],0xa
   0x00000000004004f6 <+15>:    mov    DWORD PTR [rbp-0x8],0x14
   0x00000000004004fd <+22>:    lea    rdi,[rip+0x90]        # 0x400594
   0x0000000000400504 <+29>:    call   0x4003f0 <puts@plt>
   0x0000000000400509 <+34>:    mov    eax,0x0
   0x000000000040050e <+39>:    leave  
   0x000000000040050f <+40>:    ret    
End of assembler dump.
```

Dari output assembly diatas ada sedikit perubahan dengan output assembly sebelumnya, dengan perbedaan pada instruksi `mov DWORD PTR [rbp-0x8],0x14`. 0x14 adalah bilangan hex dari 20, bisa kita simpulkan bahwa [rbp-0x8] adalah lokasi memory yang ditempati oleh variable y.