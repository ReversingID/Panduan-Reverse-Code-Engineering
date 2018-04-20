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

Kode diatas akan memangil fungsi `f()` dengan argumen `y`, pada  fungsi `f` argumen akan dicek apakah bernilai 10 ?, jika ya `printf()` akan dieksekusi. Sekarang kita compile dan lihat output assemblynya.

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

Lihat kode assembly pada fungsi `main()` diatas. Nilai `0xa` akan diisi ke alamat `[rbp-0x4]`, `[rbp-0x4]` bisa kita simpulkan bahwa itu adalah lokasi memory yang ditempati variable `y`. Setelah itu, nilai yang terdapat di `[rbp-0x4]` akan diisi ke `eax` dan register `eax` akan dicopy ke `edi` sebagai tempat untuk argumen pertama.

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

Pertama, register edi (sebagai argumen pertama) dimasukkan ke lokasi memory `[rbp+0x4]`. Setelah itu, ada instruksi `cmp`, instruksi `cmp` digunakan untuk membandingkan 2 buah nilai, dalam hal ini nilai `0xa` dibandingkan dengan nilai yang berada di `rbp-0x4`. Dan terdapat instruksi `jne 0x400504`, instruksi `jne` singkatan dari `jump if not equal`, Jadi, program akan loncat ke alamat `0x400504` jika nilai yang dibandingkan sebelumnya tidak sama. Jika kedua nilai sama, maka eksekusi program tidak akan loncat, dan melanjutkan eksekusi instruksi pada alamat `0x4004f8`.

## IF ELSE
Kode assembly pada statement IF ELSE tidak jauh berbeda dengan yang sebelumnya. Contohnya seperti kode C berikut ini,

``` c
#include <stdio.h>

void f(int x)
{
    if(x > 5) {
        printf("x lebih dari 5\n");
    } else {
        printf("x tidal lebih dari 5\n");
    }
}

int main(void)
{
    int y = 10;
    f(y);
}
```

Kita compile dan lihat hasil assemblynya.

```
$ gcc -o ifelse ifelse.c
$ gdb -batch -ex 'file ifelse' -ex 'disas f'
Dump of assembler code for function f:
   0x00000000004004e7 <+0>:     push   rbp
   0x00000000004004e8 <+1>:     mov    rbp,rsp
   0x00000000004004eb <+4>:     sub    rsp,0x10
   0x00000000004004ef <+8>:     mov    DWORD PTR [rbp-0x4],edi
   0x00000000004004f2 <+11>:    cmp    DWORD PTR [rbp-0x4],0x5
   0x00000000004004f6 <+15>:    jle    0x400506 <f+31>
   0x00000000004004f8 <+17>:    lea    rdi,[rip+0xc5]        # 0x4005c4
   0x00000000004004ff <+24>:    call   0x4003f0 <puts@plt>
   0x0000000000400504 <+29>:    jmp    0x400512 <f+43>
   0x0000000000400506 <+31>:    lea    rdi,[rip+0xc6]        # 0x4005d3
   0x000000000040050d <+38>:    call   0x4003f0 <puts@plt>
   0x0000000000400512 <+43>:    nop
   0x0000000000400513 <+44>:    leave  
   0x0000000000400514 <+45>:    ret    
End of assembler dump.
```

Kita fokus pada fungsi `f()`. Pertama register edi sebagai argumen pertama disimpan di `[rbp-0x4]`. Setelah itu ada instruksi `cmp` yang membandingkan nilai yang berada di `[rbp-0x4]` dengan angka `5`. Instruksi `jle 0x400506`  yang membuat eksekusi akan loncat ke alamat `0x400506` jika nilai yang dibandingkan sebelumnya lebih kecil atau sama dengan (*jump if less than or equal*). Dan juga pada `main+29` terdapat instruksi `jmp 0x400512`, instruksi tersebut untuk mengakhiri blok if (jika blok if yang dieksekusi).

## Switch - Case
Conditional jump juga dapat dilakukan dengan statement Switch - case. Seperti kode C dibawah ini.

``` c
#include <stdio.h>

void f(int x)
{
    switch(x)
    {
    case 0:
        printf("Bad\n");
        break;
    case 1:
        printf("Good!\n");
        break;
    default:
        printf("Good or Bad ?\n");
        break;
    }
}

int main(void)
{
    int y = 1;
    f(y);
}
```

Sekarang kita compile dan lihat hasil assemblynya

```
$ gcc -o switch_case switch_case.c
$ gdb -batch -ex 'file switch_case' -ex 'disas f'
Dump of assembler code for function f:
   0x00000000004004e7 <+0>:     push   rbp
   0x00000000004004e8 <+1>:     mov    rbp,rsp
   0x00000000004004eb <+4>:     sub    rsp,0x10
   0x00000000004004ef <+8>:     mov    DWORD PTR [rbp-0x4],edi
   0x00000000004004f2 <+11>:    mov    eax,DWORD PTR [rbp-0x4]
   0x00000000004004f5 <+14>:    test   eax,eax
   0x00000000004004f7 <+16>:    je     0x400500 <f+25>
   0x00000000004004f9 <+18>:    cmp    eax,0x1
   0x00000000004004fc <+21>:    je     0x40050e <f+39>
   0x00000000004004fe <+23>:    jmp    0x40051c <f+53>
   0x0000000000400500 <+25>:    lea    rdi,[rip+0xcd]        # 0x4005d4
   0x0000000000400507 <+32>:    call   0x4003f0 <puts@plt>
   0x000000000040050c <+37>:    jmp    0x400529 <f+66>
   0x000000000040050e <+39>:    lea    rdi,[rip+0xc3]        # 0x4005d8
   0x0000000000400515 <+46>:    call   0x4003f0 <puts@plt>
   0x000000000040051a <+51>:    jmp    0x400529 <f+66>
   0x000000000040051c <+53>:    lea    rdi,[rip+0xbb]        # 0x4005de
   0x0000000000400523 <+60>:    call   0x4003f0 <puts@plt>
   0x0000000000400528 <+65>:    nop
   0x0000000000400529 <+66>:    nop
   0x000000000040052a <+67>:    leave  
   0x000000000040052b <+68>:    ret    
End of assembler dump.
```

Pada `main+14` perbandingan dilakukan menggunakan instruksi `test eax, eax`. Instruksi `test` pada assembly, berkerja seperti instruksi `and`, hanya saja instruksi `test` hanya berpengaruh pada flag. Dengan kata lain, instruksi `test eax, eax` sama berlakunya dengan instruksi `cmp eax, 0`.

Pada `main+16` terdapat jump ke alamat 0x40050e jika 2 nilai yang dibandingkan sebelumnya bernilai sama, yakni `eax == 0`. Setelah itu terdapat instruksi `cmp eax, 0x1; je 0x40050e`, artinya eksekusi instruksi akan loncat ke alamat 0x40050e jika eax bernilai 1. Dilanjutkan dengan instruksi `jmp 0x40051c`, instruksi ini akan dieksekusi jika pada perbandingan - perbandingan sebelumnya tidak memenuhi syarat, sehingga masuk ke branch default pada switch case.