# Operasi Aritmatika

Sekarang kita akan mempelajari bagaimana operasi artimatika dalam bahasa C jika ditranslate ke dalam bahasa assembly. Saya mempunyai contoh program seperti berikut,

``` c
#include <stdio.h>

int main(void)
{
    int x = 10;
    int y = 10;
    int z = x + y;
    return 0;
}
```

Sekarang kita compile dan lihat hasil assemblynya.

```
$ gcc -o arith arith.c 
$ gdb -batch -ex 'file arith' -ex 'disas main'
Dump of assembler code for function main:
   0x0000000000400497 <+0>:     push   rbp
   0x0000000000400498 <+1>:     mov    rbp,rsp
   0x000000000040049b <+4>:     mov    DWORD PTR [rbp-0x4],0xa
   0x00000000004004a2 <+11>:    mov    DWORD PTR [rbp-0x8],0xa
   0x00000000004004a9 <+18>:    mov    edx,DWORD PTR [rbp-0x4]
   0x00000000004004ac <+21>:    mov    eax,DWORD PTR [rbp-0x8]
   0x00000000004004af <+24>:    add    eax,edx
   0x00000000004004b1 <+26>:    mov    DWORD PTR [rbp-0xc],eax
   0x00000000004004b4 <+29>:    mov    eax,0x0
   0x00000000004004b9 <+34>:    pop    rbp
   0x00000000004004ba <+35>:    ret    
End of assembler dump.
```

Perhatikan kode assembly diatas, diinstruksi main+4 dan main+11 menunjukkan bahwa variable x disimpan di [rbp+0x4] dan variable y disimpan di [rbp+0x8]. setelah itu, variable x akan disimpan di register edx, dan variable y akan disimpan di variable eax, dilanjutkan dengan instruksi `add eax, edx` untuk melakukan operasi pertambahan. Hasil dari pertambahan akan tersimpan di register eax, dan register eax akan disimpan di lokasi [rbp-0xc] untuk mengisi variable z.

Sekarang kita ganti operasi pertambahan dengan perkalian, jadi kodenya seperti ini
``` c
#include <stdio.h>

int main(void)
{
    int x = 10;
    int y = 10;
    int z = x * y;
    return 0;
}
```

Compile.

```
$ gcc -o arith arith.c
$ gdb -batch -ex 'file arith' -ex 'disas main'
Dump of assembler code for function main:
   0x0000000000400497 <+0>:     push   rbp
   0x0000000000400498 <+1>:     mov    rbp,rsp
   0x000000000040049b <+4>:     mov    DWORD PTR [rbp-0x4],0xa
   0x00000000004004a2 <+11>:    mov    DWORD PTR [rbp-0x8],0xa
   0x00000000004004a9 <+18>:    mov    eax,DWORD PTR [rbp-0x4]
   0x00000000004004ac <+21>:    imul   eax,DWORD PTR [rbp-0x8]
   0x00000000004004b0 <+25>:    mov    DWORD PTR [rbp-0xc],eax
   0x00000000004004b3 <+28>:    mov    eax,0x0
   0x00000000004004b8 <+33>:    pop    rbp
   0x00000000004004b9 <+34>:    ret    
End of assembler dump.
```

Perbedaan dari output assembly ini dari yang sebelumnya hanya berada apa instruksi imul, imul adalah instruksi assembly untuk melakukan operasi perkalian.

Kalian bisa bereksperimen dengan operator-operator lain, seperti pengurangan, xor atau yang lain.