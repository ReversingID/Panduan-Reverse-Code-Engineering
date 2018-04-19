# Calling Convention

Calling convention adalah implementasi pada level assembly tentang cara/aturan mengirim parameter ke fungsi dan bagaimana sebuah fungsi menerima parameter. Ada banyak macam - macam calling convention, tapi disini kita hanya membicarakan salah satu calling convention yang paling sering ditemui.

## Function Call
Pada level assembly, pemanggilan fungsi dapat dilakukan dengan instruksi `call`. Lihat contoh kode C dibawah ini.

``` c
#include <stdio.h>

int f()
{
    return 123;
}

int main(void)
{
    f();
}
```

Sekarang kita compile, dan kita lihat output assemblynya

```
$ gcc -o call call.c
$ gdb -batch -ex 'file call' -ex 'disas main'
Dump of assembler code for function main:
   0x00000000004004a2 <+0>:     push   rbp
   0x00000000004004a3 <+1>:     mov    rbp,rsp
   0x00000000004004a6 <+4>:     mov    eax,0x0
   0x00000000004004ab <+9>:     call   0x400497 <f>
   0x00000000004004b0 <+14>:    mov    eax,0x0
   0x00000000004004b5 <+19>:    pop    rbp
   0x00000000004004b6 <+20>:    ret
End of assembler dump.
$ gdb -batch -ex 'file call' -ex 'disas f'
Dump of assembler code for function f:
   0x0000000000400497 <+0>:     push   rbp
   0x0000000000400498 <+1>:     mov    rbp,rsp
   0x000000000040049b <+4>:     mov    eax,0x7b
   0x00000000004004a0 <+9>:     pop    rbp
   0x00000000004004a1 <+10>:    ret
End of assembler dump.
```

Pada main+9 bisa kita lihat, program memanggil fungsi `f()` dengan instruksi `call`

## Mengirim Argumen ke Fungsi
Bagaimana caranya mengirim parameter ke sebuah fungsi ?. Langsung saja kita lihat contoh kode dibawah ini. Kita modifikasi sedikit script call.c sebelumnya.

``` c
#include <stdio.h>

int f(int x, int y)
{
    return 0;
}

int main(void)
{
    f(1, 2);
}
```

Kita compile dan lihat hasil assemblynya.

```
$ gcc -o call call.c
$ gdb -batch -ex 'file call' -ex 'disas main' 
Dump of assembler code for function main:                                       
   0x00000000004004a8 <+0>:     push   rbp                                      
   0x00000000004004a9 <+1>:     mov    rbp,rsp                                  
   0x00000000004004ac <+4>:     mov    esi,0x2
   0x00000000004004b1 <+9>:     mov    edi,0x1
   0x00000000004004b6 <+14>:    call   0x400497 <f>
   0x00000000004004bb <+19>:    mov    eax,0x0
   0x00000000004004c0 <+24>:    pop    rbp
   0x00000000004004c1 <+25>:    ret
End of assembler dump.
```

Lihat pada main+4 dan main+9, register esi akan diisi dengan nilai 0x2, dan register edi akan diisi dengan 0x1. Sesuai dengan kode C telah dibuat, bisa kita simpulkan argumen pertama akan ditaruh di register rdi dan argumen kedua akan diisi di register rsi.

Bagaimana dengan argumen ke-3, ke-4 dan seterusnya ?. Kita akan mencoba contoh lain, kita membuat pemanggilan fungsi dengan 8 argumen.

``` c
#include <stdio.h>

int f(int x, int y, int z, int a, int b, int c, int d, int e)
{
    return 0;
}

int main(void)
{
    f(1, 2, 3, 4, 5, 6, 7, 8);
}
```

Kita compile dan lihat output assemblynya
```
$ gcc -o call call.c
$ gdb -batch -ex 'file call' -ex 'disas main'
Dump of assembler code for function main:
   0x00000000004004b6 <+0>:     push   rbp
   0x00000000004004b7 <+1>:     mov    rbp,rsp
   0x00000000004004ba <+4>:     push   0x8
   0x00000000004004bc <+6>:     push   0x7
   0x00000000004004be <+8>:     mov    r9d,0x6
   0x00000000004004c4 <+14>:    mov    r8d,0x5
   0x00000000004004ca <+20>:    mov    ecx,0x4
   0x00000000004004cf <+25>:    mov    edx,0x3
   0x00000000004004d4 <+30>:    mov    esi,0x2
   0x00000000004004d9 <+35>:    mov    edi,0x1
   0x00000000004004de <+40>:    call   0x400497 <f>
   0x00000000004004e3 <+45>:    add    rsp,0x10
   0x00000000004004e7 <+49>:    mov    eax,0x0
   0x00000000004004ec <+54>:    leave  
   0x00000000004004ed <+55>:    ret    
End of assembler dump.
```

Dari hasil kode assembly diatas dapat kita simpulkan bahwa : Argumen pertama sampai keenam akan disimpan secara terurut dari register rdi, rsi, rdx, rcx, r8 dan r9. Argumen selanjutnya akan disimpan distack. Kita lihat juga diatas, setelah instruksi `call` terdapat instruksi `add rsp, 0x10`, instruksi tersebut berfungsi untuk men-clearkan stack karena sebelumnya kita mem-push 2 nilai ke stack.