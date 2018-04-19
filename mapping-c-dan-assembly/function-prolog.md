# Function Prolog dan Epilogue

Dibagian sebelumnya kita telah melihat seperti script sederhana bahasa C yang hanya berisi `return` dan program hello world dalam assembly. Dibagian sebelumnya juga banyak kita temukan instruksi berupa `push rbp`, `mov rbp, rsp`. Seperti di kode assembly hello world sebelumnya
```
$ gdb -batch -ex 'file hello' -ex 'disas main'
Dump of assembler code for function main:
   0x00000000004004e7 <+0>:     push   rbp
   0x00000000004004e8 <+1>:     mov    rbp,rsp
   0x00000000004004eb <+4>:     lea    rdi,[rip+0x92]        # 0x400584
   0x00000000004004f2 <+11>:    call   0x4003f0 <puts@plt>
   0x00000000004004f7 <+16>:    mov    eax,0x0
   0x00000000004004fc <+21>:    pop    rbp
   0x00000000004004fd <+22>:    ret    
End of assembler dump.
```

Sebenarnya untuk apa fungsi dari instruksi itu ?. Beberapa instruksi yang berada diawal fungsi disebut function prolog, sedangkan instruksi instruksi yang berada di akhir fungsi disebut function epilog.

# Function Prolog
Function prolog berfungsi untuk membuat ruang stack baru, stack baru ini digunakan sebagai tempat penyimpanan variable lokal. Instruksi `push rbp` digunakan untuk menyimpan nilai register rbp ke dalam stack, selanjutnya instruksi `mov rbp, rsp` artinya nilai rsp dicopy ke register rbp. Sekarang kita modifikasi script hello.c sebelumnya dengan tambahan variable lokal.

``` c
#include <stdio.h>

int main(void)
{
    int x = 10;
    printf("Hello world\n");
    return 0;
}
```

Kita compile dan lihat hasil assemblynya.

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

Kita menambahkan sebuah variable x, dan kita isi dengan nilai 10. Lihat output assembly diatas, setelah instruksi `mov rbp, rsp` terdapat instruksi `sub rsp, 0x10`, instruksi ini bertujuan agar ruang stack baru yang telah dibuat menjadi lebih besar.

Kita sudah mengetahui akan tumbuh ke alamat yang lebih kecil, dengan instruksi sub ini yang artinya mengurangi nilai dari rsp dan juga menumbuhkan stack itu sendiri.

`sub rsp, N` artinya mengalokasikan stack sebanyak N byte. format instruksi untuk function prolog biasanya seperti berikut :

```
push rbp
mov rbp, rsp
sub rsp, N
```

# Functon Epilogue
Function epilogue adalah kebalikan dari function prologue. Function epilogue mengembalikan kondisi register rsp dan rbp seperti sebelum fungsi tersebut dipanggil atau bisa dikatakan mengembalikan kondisi yang sudah dilakukan oleh function prolog.

Lihat output assembly diatas. Pada akhir fungsi terdapat instruksi `leave; ret`. Dalam assembly, instruksi `leave` sama dengan instruksi `mov rsp, rbp; pop ebp`. Jadi function epilogue pada umumnya mempunyai instruksi seperti berikut:

```
mov rsp, rbp
pop rbp
ret
```