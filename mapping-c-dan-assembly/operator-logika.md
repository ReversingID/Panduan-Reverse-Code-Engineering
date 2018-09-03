# Operator Logika

Sekarang kita akan belajar mengenai salah satu jenis operator yaitu operator logika, operator logika merupakan operator yang digunakan untuk melakukan operasi dimana nilai yang dihasilkan dari operasi tersebut hanya berupa nilai benar (`true`) dan nilai salah (`false`), nilai itu biasa disebut dengan nilai `boolean`.

<h2>Operator <code>&&</code> (AND)</h2>

``` c
#include <stdio.h>

int main(void) {
   int x = 1;
   int y = 0;
   int z = x && y;

 //  printf("hasil : %d\n", z);
   return 0;
}
```

Compile program diatas menggunakan `gcc`, seperti ini :

`user:~$ gcc -o and and.c`

Kemudian disassemble program menggunakan `gdb` :

```
user:~$ gdb -q and
Reading symbols from and...(no debugging symbols found)...done.
gdb> disas main
Dump of assembler code for function main:
   0x080483db <+0>:	push   ebp
   0x080483dc <+1>:	mov    ebp,esp
   0x080483de <+3>:	sub    esp,0x10
   0x080483e1 <+6>:	mov    DWORD PTR [ebp-0xc],0x1    => x = 1
   0x080483e8 <+13>:	mov    DWORD PTR [ebp-0x8],0x0    => y = 0
   0x080483ef <+20>:	cmp    DWORD PTR [ebp-0xc],0x0    => bandingkan nilai variabel x dengan variabel y
   0x080483f3 <+24>:	je     0x8048402 <main+39>        => lompat ke main+39 yang berada di alamat 0x8048402, jika nilai x = y
   0x080483f5 <+26>:	cmp    DWORD PTR [ebp-0x8],0x0    => bandingkan nilai variabel y dengan variabel x 
   0x080483f9 <+30>:	je     0x8048402 <main+39>        => lompat ke main+39 yang berada dialamat 0x8048402, jika nilai y = x
   0x080483fb <+32>:	mov    eax,0x1                    
   0x08048400 <+37>:	jmp    0x8048407 <main+44>        => lompat ke main+44 yang berada dialamat 0x8048407, z = x && y
   0x08048402 <+39>:	mov    eax,0x0                     
   0x08048407 <+44>:	mov    DWORD PTR [ebp-0x4],eax    => simpan nilai hasil perbandingan nilai, nilai variabel z = 0 
   0x0804840a <+47>:	mov    eax,0x0                    
   0x0804840f <+52>:	leave  
   0x08048410 <+53>:	ret    
End of assembler dump.
gdb>
```

Dari hasil `disassembly` fungsi `main` pada program diatas, terdapat instruksi pada `main+6` dan `main+13` dimana instruksi tersebut merupakan variabel `x` dan variabel `y`, variabel `x` mempunyai nilai `0x1` nilai tersebut disimpan pada `[ebp-0xc]` dan variabel `y` mempunyai nilai `0x0` nilai tersebut disimpan pada `[ebp-0x8]`. Kemudian kedua nilai tersebut dibandingkan karena ada instruksi lompat bersyarat (instruksi `cmp` dan `je`), operasi `&&` akan menghasilkan nilai yang benar (`true`) jika kedua nilai tersebut sama, sedangkan jika salah satu dari kedua nilai tersebut ada yang berbeda maka akan menghasilkan nilai yang salah (`false`).

<h2>Operator <code>||</code> (OR)</h2>

``` c
#include <stdio.h>

int main(void) {
   int x = 1;
   int y = 0;
   int z = x || y;

 //  printf("hasil : %d\n", z);
   return 0;
}
```

Compile program diatas menggunakan `gcc`, seperti ini :

`user:~$ gcc -o or or.c`

Kemudian disassemble program menggunakan `gdb` :

```
user:~$ gdb -q or
Reading symbols from or...(no debugging symbols found)...done.
gdb> disas main
Dump of assembler code for function main:
   0x080483db <+0>:	push   ebp
   0x080483dc <+1>:	mov    ebp,esp
   0x080483de <+3>:	sub    esp,0x10
   0x080483e1 <+6>:	mov    DWORD PTR [ebp-0xc],0x1    => x = 1
   0x080483e8 <+13>:	mov    DWORD PTR [ebp-0x8],0x0    => y = 0
   0x080483ef <+20>:	cmp    DWORD PTR [ebp-0xc],0x0    => bandingkan nilai variabel x dengan variabel y
   0x080483f3 <+24>:	jne    0x80483fb <main+32>        => lompat ke main+32 yang berada di alamat 0x80483fb, jika nilai x != y
   0x080483f5 <+26>:	cmp    DWORD PTR [ebp-0x8],0x0    => bandingkan nilai variabel y dengan variabel x
   0x080483f9 <+30>:	je     0x8048402 <main+39>        => lompat ke main+39 yang berada di alamat 0x8048402, jika nilai y = x
   0x080483fb <+32>:	mov    eax,0x1
   0x08048400 <+37>:	jmp    0x8048407 <main+44>        => lompat ke main+44 yang berada dialamat 0x8048407, z = x || y
   0x08048402 <+39>:	mov    eax,0x0
   0x08048407 <+44>:	mov    DWORD PTR [ebp-0x4],eax    => simpan nilai hasil perbandingan nilai, nilai variabel z = 1
   0x0804840a <+47>:	mov    eax,0x0
   0x0804840f <+52>:	leave  
   0x08048410 <+53>:	ret    
End of assembler dump.
gdb>
```

Dari hasil `disassembly` fungsi `main` pada program diatas, terdapat instruksi pada `main+6` dan `main+13` dimana instruksi tersebut merupakan variabel `x` dan variabel `y`, variabel `x` mempunyai nilai `0x1` nilai tersebut disimpan pada `[ebp-0xc]` dan variabel `y` mempunyai nilai `0x0` nilai tersebut disimpan pada `[ebp-0x8]`. Kemudian kedua nilai tersebut dibandingkan karena ada instruksi lompat bersyarat (instruksi `cmp` dan `jne`) lalu dilanjutkan dengan instruksi `je`, operasi `||` akan menghasilkan nilai yang salah (`false`) jika kedua nilai tersebut bernilai `0x0`, sedangkan jika salah satu dari kedua nilai tersebut bernilai `0x1` maka akan menghasilkan nilai yang benar (`true`).

<h2>Operator <code>!</code> (NOT)</h2>

``` c
#include <stdio.h>

int main(void) {
   int x = 1;
   int y = !x;

  // printf("hasil : %d\n", y);
   return 0;
}
```

Compile program diatas menggunakan `gcc`, seperti ini :

`user:~$ gcc -o not not.c`

Kemudian disassemble program menggunakan `gdb` :

```
user:~$ gdb -q not
gdb> disas main
Dump of assembler code for function main:
   0x080483db <+0>:	push   ebp
   0x080483dc <+1>:	mov    ebp,esp
   0x080483de <+3>:	sub    esp,0x10
   0x080483e1 <+6>:	mov    DWORD PTR [ebp-0x8],0x1    => x = 1
   0x080483e8 <+13>:	cmp    DWORD PTR [ebp-0x8],0x0    => bandingkan nilai variabel x
   0x080483ec <+17>:	sete   al                         => set byte pada variabel x jika variabel x = 1 pada register al 
   0x080483ef <+20>:	movzx  eax,al                     => pindahkan nilai variable x pada register al secara zero-extend ke register eax, x != 1
   0x080483f2 <+23>:	mov    DWORD PTR [ebp-0x4],eax    => simpan nilai hasil perbandingan nilai, nilai y = 0
   0x080483f5 <+26>:	mov    eax,0x0
   0x080483fa <+31>:	leave  
   0x080483fb <+32>:	ret    
End of assembler dump.
gdb>
```

Dari hasil `disassembly` fungsi `main` pada program diatas, terdapat instruksi pada `main+6` dimana instruksi tersebut merupakan variabel `x`, variabel `x` mempunyai nilai `0x1` nilai tersebut disimpan pada `[ebp-0x8]`. Bandingkan nilai variabel `x` lalu instruksi `sete al` melakukan set `byte` pada register `al` jika nilai variabel `x` bernilai `0x1`, berarti register `al` memiliki nilai `0x1` kemudian instruksi `movzx eax,al` memindahkan nilai variabel `x` secara `zero-extend` pada register `al` ke register `eax`, berarti register `eax` memiliki nilai `x != 1` setelah itu nilai register `eax` disimpan pada `[ebp-0x4]` untuk mengisi nilai variabel `y`. 
