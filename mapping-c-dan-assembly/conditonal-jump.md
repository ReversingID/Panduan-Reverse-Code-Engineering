# Conditional jump

Sudah kita ketahui conditional jump merupakan lompatan ke suatu bagian program sesuai dengan kondisi tertentu. Biasanya conditional jump dalam bahasa pemrograman dapat dilakukan dengan statement if. Contoh berikut adalah conditional jump dalam bahasa C.

``` c
#include <stdio.h>

int f(int x)
{
    if(x == 10)
    {
        printf("X adalah 10\n");
    }
}

int main(void)
{
    int y = 10;
    f(y);
}
```

Kode diatas, akan memangil fungsi `f()` dengan argumen `y`, pada  fungsi `f` argumen akan dicek apakah bernilai 10 ?, jika ya `printf()` akan dieksekusi. Sekarang kita compile dan lihat output assemblynya.

```
$ gcc -o jump jump.c
$ gdb -batch -ex 'file jump' -ex 'disas main'
Dump of assembler code for function main:                                       
   0x0000000000400507 <+0>:     push   rbp                                      
   0x0000000000400508 <+1>:     mov    rbp,rsp                                  
   0x000000000040050b <+4>:     sub    rsp,0x10                                 
   0x000000000040050f <+8>:     mov    DWORD PTR [rbp-0x4],0xa                  
   0x0000000000400516 <+15>:    mov    eax,DWORD PTR [rbp-0x4]                  
   0x0000000000400519 <+18>:    mov    edi,eax                                  
   0x000000000040051b <+20>:    call   0x4004e7 <f>                             
   0x0000000000400520 <+25>:    mov    eax,0x0                                  
   0x0000000000400525 <+30>:    leave                                           
   0x0000000000400526 <+31>:    ret
End of assembler dump.
```

Lihat kode assembly pada fungsi `main()` diatas. Nilai `0xa` akan diisi ke alamat `[rbp-0x4]`, `[rbp-0x4]` bisa kita simpulkan bahwa itu adalah lokasi memory yang ditempati variable `y`. Setelah itu nilai yang terdapat di `[rbp-0x4]` akan diisi ke `eax` dan register `eax` akan dicopy ke `edi` sebagai tempat untuk argumen pertama.

Sekarang kita lihat kode assembly pada fungsi `f()`.
```
$ gdb -batch -ex 'file jump' -ex 'disas f'
$ Dump of assembler code for function f:
   0x00000000004004e7 <+0>:     push   rbp
   0x00000000004004e8 <+1>:     mov    rbp,rsp
   0x00000000004004eb <+4>:     sub    rsp,0x10
   0x00000000004004ef <+8>:     mov    DWORD PTR [rbp-0x4],edi
   0x00000000004004f2 <+11>:    cmp    DWORD PTR [rbp-0x4],0xa
   0x00000000004004f6 <+15>:    jne    0x400504 <f+29>
   0x00000000004004f8 <+17>:    lea    rdi,[rip+0xb5]        # 0x4005b4
   0x00000000004004ff <+24>:    call   0x4003f0 <puts@plt>
   0x0000000000400504 <+29>:    nop
   0x0000000000400505 <+30>:    leave
   0x0000000000400506 <+31>:    ret
End of assembler dump.
```

Pertama register edi (sebagai argumen pertama) dimasukkan ke lokasi memory `[rbp+0x4]`. Setelah itu ada instruksi `cmp`, instruksi `cmp` digunakan untuk membandingkan 2 buah nilai, dalam hal ini nilai `0xa` dibandingkan dengan nilai yang berada di `rbp-0x4`. Dan terdapat instruksi `jne 0x400504`, instruksi `jne` singkatan dari `jump if not equal`, Jadi, program akan loncat ke alamat `0x400504` jika nilai yang dibandingkan sebelumnya tidak sama. Jika kedua nilai sama, maka program tidak loncat, dan melanjutkan eksekusi instruksi pada lamat `0x4004f8`.