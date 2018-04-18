# Hello world

Mari kita mulai dengan program yang paling sederhana, kode dibawah ini hanya melakukan return sebuah nilai integer, kita akan lihat seperti apa kode assembly yang dihasilkan compiler untuk kode ini.

``` c
#include <stdio.h>

int main(void)
{
    return 1337;
}
```

Sekarang kita compile, disini saya menggunakan gcc sebagai compiler. Untuk melihat hasil assemblynya saya menggunakan single line command dengan GDB.
```
$ gcc -o return return.c 
$ gdb -batch -ex 'file return' -ex 'disas main'
Dump of assembler code for function main:
   0x0000000000400497 <+0>:     push   rbp
   0x0000000000400498 <+1>:     mov    rbp,rsp
   0x000000000040049b <+4>:     mov    eax,0x539
   0x00000000004004a0 <+9>:     pop    rbp
   0x00000000004004a1 <+10>:    ret    
End of assembler dump. 
```

    Saya mengatur agar GDB menampilkan output assembly dengan syntax intel, untuk mengatur agar GDB menampilkan syntax intel tambahkan baris baru dengan isi `set disassembly-flavor intel` di file ~/.gdbinit

Lihat output assembly diatas. Dengan melihat intruksi `mov eax,0x539` bisa kita simpulkan bahwa return value akan disimpan di register eax. Lalu apa gunanya intruksi-lainnya seperti intruksi `push rbp`, `mov rbp, rsp` ?, jawabannya akan kita lihat dibagian lain setelah ini.

Sekarang kita atur agar gcc agar mengoptimize program C, apa yang akan terjadi ?. Kita mengaktifkan optimization dengan opsi -O.

```
$ gcc -O -o return return.c
$ gdb -batch -ex 'file return' -ex 'disas main' 
Dump of assembler code for function main:
   0x0000000000400497 <+0>:     mov    eax,0x539
   0x000000000040049c <+5>:     ret    
End of assembler dump.
```

    Optimize artinya compiler akan berusaha mengurangi instruksi - instruksi yang sekiranya tidak perlu atau mengganti sebuah instruksi menjadi instruksi lain yang lebih cepat. Tujuan optimization adalah menghasilkan kode lebih cepat, dan ukuran kode yang lebih kecil.

Berbeda dengan output assembly yang pertama. Kali ini output assembly lebih sedikit, gcc akan mengurangi/menghilangkan instruksi-instruksi yang tidak diperlukan.

## Print hello world
Kita akan melihat seperti apa program hello world ini jika dalam bentuk assembly.

``` C
#include <stdio.h>

int main(void)
{
    printf("Hello world\n");
    return 0;
}
```

Output assembly yang dihasilkan adalah seperti ini.

```
$ gcc -o hello hello.c
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

Kita lihat output assembly diatas, terdapat pemenggilan ke fungsi puts.

    Kenapa bukan printf yang dipanggil ?, Itu adalah builtin optimization dari GCC yang mengganti setiap printf ke puts kecuali string yang akan ditampilkan berisi format string.


Sebelum instruksi call terdapat instruksi `lea rdi,[rip+0x92]`. Lihat nilai yang berada setelah `#`, GDB telah memberikan kita hint bahwa [rip+0x92] itu akan menunjuk ke alamat `0x400584`. Sekarang kita lihat ada nilai apa yang berada di alamat `0x400584`

```
$ gdb hello -q
Reading symbols from hello...(no debugging symbols found)...done.
(gdb) x/10bc 0x400584
0x400584:       72 'H'  101 'e' 108 'l' 108 'l' 111 'o' 32 ' '  119 'w' 111 'o'
0x40058c:       114 'r' 108 'l'
(gdb) x/s 0x400584
0x400584:       "Hello world"
```

Yap, di alamat `0x400584` berisi string "Hello world". Sampai saat ini bisa kita simpulkan alamat string "Hello world" akan disimpan di register rdi sebagai argumen pertama.