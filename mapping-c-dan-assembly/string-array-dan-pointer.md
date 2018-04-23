# String, Array dan Pointer

## Array
Dalam pemrograman array merupakan variable yang dapat memiliki banyak nilai dengan tipe data yang sama. Contohnya seperti kode C dibawah ini.

``` c
#include <stdio.h>

int main(void)
{
    int arr[] = {10, 20, 30, 40};
    return 0;
}
```

Kita coba lihat assemblynya

```
$ gcc -o array array.c
$ gdb -batch -ex 'file array' -ex 'disas main'
Dump of assembler code for function main:
   0x0000000000400497 <+0>:     push   rbp
   0x0000000000400498 <+1>:     mov    rbp,rsp
   0x000000000040049b <+4>:     mov    DWORD PTR [rbp-0x10],0xa
   0x00000000004004a2 <+11>:    mov    DWORD PTR [rbp-0xc],0x14
   0x00000000004004a9 <+18>:    mov    DWORD PTR [rbp-0x8],0x1e
   0x00000000004004b0 <+25>:    mov    DWORD PTR [rbp-0x4],0x28
   0x00000000004004b7 <+32>:    mov    eax,0x0
   0x00000000004004bc <+37>:    pop    rbp
   0x00000000004004bd <+38>:    ret    
End of assembler dump.
```

Jika kita lihat assemblynya, program seperti memiliki 4 variable yang berlokasi `rbp-0x10`, `rbp-0xc`, `rbp-0x8`, `rbp-0x4` yang sebenarnya itu adalah 1 variable bertipe array yang mempunyai 4 elemen.

Contoh dibawah ini adalah kode C untuk mengakses elemen pada array.
``` c
#include <stdio.h>

int main(void)
{
    int arr[] = {10, 20, 30, 40};
    int y = arr[2];
    return 0;
}
```

Kode assemblya akan seperti ini.

```
$ gcc -o array array.c
$ gdb -batch -ex 'file array' -ex 'disas main'
Dump of assembler code for function main:
   0x0000000000400497 <+0>:     push   rbp
   0x0000000000400498 <+1>:     mov    rbp,rsp
   0x000000000040049b <+4>:     mov    DWORD PTR [rbp-0x20],0xa
   0x00000000004004a2 <+11>:    mov    DWORD PTR [rbp-0x1c],0x14
   0x00000000004004a9 <+18>:    mov    DWORD PTR [rbp-0x18],0x1e
   0x00000000004004b0 <+25>:    mov    DWORD PTR [rbp-0x14],0x28
   0x00000000004004b7 <+32>:    mov    eax,DWORD PTR [rbp-0x18]
   0x00000000004004ba <+35>:    mov    DWORD PTR [rbp-0x4],eax
   0x00000000004004bd <+38>:    mov    eax,0x0
   0x00000000004004c2 <+43>:    pop    rbp
   0x00000000004004c3 <+44>:    ret    
End of assembler dump.
```

Kita lihat diatas, variable arr pada elemen kedua akan diisi dengan nilai 30, variable arr elemen kedua memiliki lokasi memory di `[rbp-0x18]`. Pada `main+32` variable arr pada elemen kedua akan disimpan di eax, dan nilai eax akan disimpan di `[rbp-0x4]`, `[rbp-0x4]` merupakan lokasi variable y.

Sekarang kita membuat contoh lain. Contoh dibawah ini merupakan contoh pengaksesan seluruh elemen array dengan perulangan for. Kodenya seperti ini.

``` c
#include <stdio.h>

int main(void)
{
    int arr[] = {10, 20, 30, 40};
    int i;
    for(i = 0; i < 4; i++)
    {
        printf("%d\n", arr[i]);
    }
    return 0;
}
```
```
$ gcc -o array array.c
$ gdb -batch -ex 'file array' -ex 'disas main'
Dump of assembler code for function main:
   0x00000000004004e7 <+0>:     push   rbp
   0x00000000004004e8 <+1>:     mov    rbp,rsp
   0x00000000004004eb <+4>:     sub    rsp,0x20
   0x00000000004004ef <+8>:     mov    DWORD PTR [rbp-0x20],0xa
   0x00000000004004f6 <+15>:    mov    DWORD PTR [rbp-0x1c],0x14
   0x00000000004004fd <+22>:    mov    DWORD PTR [rbp-0x18],0x1e
   0x0000000000400504 <+29>:    mov    DWORD PTR [rbp-0x14],0x28
   0x000000000040050b <+36>:    mov    DWORD PTR [rbp-0x4],0x0
   0x0000000000400512 <+43>:    jmp    0x400534 <main+77>
   0x0000000000400514 <+45>:    mov    eax,DWORD PTR [rbp-0x4]
   0x0000000000400517 <+48>:    cdqe
   0x0000000000400519 <+50>:    mov    eax,DWORD PTR [rbp+rax*4-0x20]
   0x000000000040051d <+54>:    mov    esi,eax
   0x000000000040051f <+56>:    lea    rdi,[rip+0xae]        # 0x4005d4
   0x0000000000400526 <+63>:    mov    eax,0x0
   0x000000000040052b <+68>:    call   0x4003f0 <printf@plt>
   0x0000000000400530 <+73>:    add    DWORD PTR [rbp-0x4],0x1
   0x0000000000400534 <+77>:    cmp    DWORD PTR [rbp-0x4],0x3
   0x0000000000400538 <+81>:    jle    0x400514 <main+45>
   0x000000000040053a <+83>:    mov    eax,0x0
   0x000000000040053f <+88>:    leave
   0x0000000000400540 <+89>:    ret
End of assembler dump.
```

Kode assembly yang dihasilkan cukup panjang, tapi saya akan membahasnya satu-satu. Pertama, pada `main+8` sampai `main+29` itu adalah bagian dimana variable array `arr` diinisialisasi. Pada `main+36` variable i diinisialisai dengan 0. Pada `main+43` sampai `main+81` itu merupakan bagian kode pada blok for.

Kita langsung fokus pada inti kode yang berada didalam for, kode yang for berada pada `main+45` sampai `main+68`. Variable counter yakni variable i berlokasi pada `[rbp-0x4]`. Pada `main+45` variable i disimpan di register eax, setelah itu ada instruksi `cdqe`, instruksi `cdqe` akan mengkonversi nilai dword pada eax menjadi qword.

Pada `main+50` terdapat instruksi `mov eax,DWORD PTR [rbp+rax*4-0x20]`, instruksi ini akan mengakses elemen ke `i` pada variable `arr`. Angka `-0x20` digunakan untuk menunjuk ke variable array, karena variable i berlokasi di `rbp-0x20`. Sementara `rax*4` akan menghasilkan offset untuk menunjuk ke elemen pada variable array. Jika kita lihat di instruksi sebelumnya, register rax telah diisi oleh variable i. Jadi nilai `i*4` digunakan untuk menunjuk nilai elemen ke i pada array. Sementara, angka 4 merupakan besar dari tipe data integer, karena tipe integer mempunyai besar 4 byte.