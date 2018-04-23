# Perulangan

## For loop
Dibagian perulangan ini kita akan memulainya dari for-loop. Dalam bahasa C syntax for terdiri dari bagian init, condition, increment.
```c
for ( init; condition; increment ) {
   statement(s);
}
```

Contoh kode C nya seperti ini.
``` c
#include <stdio.h>

int main(void)
{
    int i;
    for(i = 0; i < 5; i++)
    {
        printf("Hello world\n");
    }
}
```

Kita compile dan lihat hasil assemblynya.

```
$ gcc -o loop loop.c
$ gdb -batch -ex 'file loop' -ex 'disas main' 
Dump of assembler code for function main:
   0x00000000004004e7 <+0>:     push   rbp
   0x00000000004004e8 <+1>:     mov    rbp,rsp
   0x00000000004004eb <+4>:     sub    rsp,0x10
   0x00000000004004ef <+8>:     mov    DWORD PTR [rbp-0x4],0x0
   0x00000000004004f6 <+15>:    jmp    0x400508 <main+33>
   0x00000000004004f8 <+17>:    lea    rdi,[rip+0xa5]        # 0x4005a4
   0x00000000004004ff <+24>:    call   0x4003f0 <puts@plt>
   0x0000000000400504 <+29>:    add    DWORD PTR [rbp-0x4],0x1
   0x0000000000400508 <+33>:    cmp    DWORD PTR [rbp-0x4],0x4
   0x000000000040050c <+37>:    jle    0x4004f8 <main+17>
   0x000000000040050e <+39>:    mov    eax,0x0
   0x0000000000400513 <+44>:    leave  
   0x0000000000400514 <+45>:    ret    
End of assembler dump.
```

Pada `main+8` itu merupakan bagian dari statement for loop yang biasa disebut `init`. Dalam bahasa C, bagian `init` akan dieksekusi lebih dulu, karena bagian init akan menginisialisasi variable yang digunakan sebagai pengontrol loop. Dalam kasus ini variable i akan menjadi pengontrol loop, ia akan diinisialisasi dengan 0.

Selanjutnya pada main+15 ada instruksi `jmp 0x400508`, eksekusi akan loncat ke `main+33`. Sekarang kita lihat instruksi pada `main+33`. Disana nilai yang berada di `[rbp-0x4]` akan dibandingkan dengan nilai 0x4. Di instruksi selanjutnya program akan loncat lagi ke `main+17` jika nilai yang dibandingkan sebelumnya kurang dari atau sama dengan 0x4.

Pada `main+17` sampai `main+24` itu adalah kode kita yang berada didalam blok for. Didalam blok for kita akan mencetak string "Hello world".

Pada main+29, variable i akan ditambah 1.


# While loop
Dalam bahasa C statement while loop biasanya ditulis seperti berikut.

``` c
#include <stdio.h>

int main(void)
{
    int i = 0;
    while(i < 5)
    {
        printf("Hello world\n");
        i++;
    }
}
```

Compile dan lihat hasil assemblynya.

```
$ gcc -o while while.c
$ gdb -batch -ex 'file while' -ex 'disas main'
Dump of assembler code for function main:
   0x00000000004004e7 <+0>:     push   rbp
   0x00000000004004e8 <+1>:     mov    rbp,rsp
   0x00000000004004eb <+4>:     sub    rsp,0x10
   0x00000000004004ef <+8>:     mov    DWORD PTR [rbp-0x4],0x0
   0x00000000004004f6 <+15>:    jmp    0x400508 <main+33>
   0x00000000004004f8 <+17>:    lea    rdi,[rip+0xa5]        # 0x4005a4
   0x00000000004004ff <+24>:    call   0x4003f0 <puts@plt>
   0x0000000000400504 <+29>:    add    DWORD PTR [rbp-0x4],0x1
   0x0000000000400508 <+33>:    cmp    DWORD PTR [rbp-0x4],0x4
   0x000000000040050c <+37>:    jle    0x4004f8 <main+17>
   0x000000000040050e <+39>:    mov    eax,0x0
   0x0000000000400513 <+44>:    leave  
   0x0000000000400514 <+45>:    ret    
End of assembler dump.
```

Pada `main+8` kita isi variable i dengan 0. Pada `main+15` program akan loncat ke alamat 0x400508, disana terdapat instruksi cmp yang membandingkan variable i dengan nilai 0x4. Setelah itu terdapat instruksi jle yang akan loncat ke alamat 0x4004f8 jika nilai yang dibandingkan sebelumnya kurang dari atau sama dengan 4 (sesuai instruksi cmp sebelumnya). Di alamat 0x4004f8 program akan mencetak hello world dan mengincrement variable i.