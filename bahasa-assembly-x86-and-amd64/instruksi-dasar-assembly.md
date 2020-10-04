# Intruksi Dasar Assembly

### MOV (Move From Memory)

Perintah MOV digunakan untuk mengcopy nilai atau angka menuju suatu
register,variabel atau memory. Adapun syntax untuk perintah MOV ini adalah :


** MOV Tujuan,Asal**


##### Sebagai contohnya :
```    
MOV AL,9 ; masukkan nilai 9 pada AL.
MOV AH,AL ; nilai AL=9 dan AH=9
MOV AX,9 ; AX=AH+AL hingga AH=0 dan AL:=9
```
Pada baris pertama(MOV AL,9), kita memberikan nilai 9 pada register AL.
Kemudian pada baris kedua(MOV AH,AL) kita mengcopykan nilai register AL untuk
AH. Jadi setelah operasi ini register AL akan tetap bernilai 9, dan register
AH akan sama nilainya dengan AL atau 9.
Pada baris ketiga(MOV AX,9), kita memberikan register AX nilai 9. Karena AX terdiri atas AH dan AL, maka
register AH akan bernilai 0, sedangkan AL akan bernilai 9.

Perintah MOV akan mengcopykan nilai pada sumber untuk dimasukan ke
Tujuan, nilai sumber tidaklah berubah. Inilah sebabnya MOV(E) akan kita
terjemahkan disini dengan mengcopy, dan bukannya memindahkan. 


### JMP (Jump to sum of Accumulator and Data Pointer)


Perintah JMP(JUMP) ini digunakan untuk melompat menuju tempat yang
ditunjukkan oleh perintah JUMP. Adapun syntaxnya adalah:

**JUMP Tujuan .**

Dimana tujuannya dapat berupa label seperti yang digunakan pada bagan
diatas. Mengenai perintah JUMP ini akan kita bahas lebih lanjut nantinya.
Perintah JUMP yang digunakan pada bagan diatas dimaksudkan agar melewati
tempat data program, karena jika tidak ada perintah JUMP ini maka data program
akan ikut dieksekusi sehingga kemungkinan besar akan menyebabkan program anda
menjadi Hang.

### INT 21h (Mencetak Huruf)

Bila dihasilkan interupsi 21h apa yang akan dikerjakan oleh komputer ?.
Jawabnya, ada banyak sekali kemungkinan. Pada saat terjadi interupsi 21h maka
pertama-tama yang dilakukan komputer adalah melihat isi atau nilai apa yang
terdapat pada register AH. Misalkan bila nilai AH adalah 2 maka komputer akan
mencetak sebuah karakter, berdasarkan kode ASCII yang terdapat pada register
DL. Bila nilai pada register AH bukanlah 2, pada saat dilakukan interupsi 21h
maka yang dikerjakaan oleh komputer akan lain lagi.
Dengan demikian kita bisa mencetak sebuah karakter yang diinginkan
dengan meletakkan angka 2 pada register AH dan meletakkan kode ASCII dari
karakter yang ingin dicetak pada register DL sebelum menghasilkan interupsi
21h.
```
;~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~;
; PROGRAM : A0.ASM ;
; FUNGSI : MENCETAK KARATER ;
; 'A' DENGAN INT 21 ;
;==========================S’to=;
 .MODEL SMALL
 .CODE
 ORG 100h
Proses :
 MOV AH,02h ; Nilai servis ntuk mencetak karakter
 MOV DL,'A' ; DL = Karakter ASCII yang akan dicetak
 INT 21h ; Cetak karakter !!
 INT 20h ; Selesai ! kembali ke DOS
END Proses
 Program 7.1. Mencetak sebuah karakter
```
Setelah program 7.1. anda ketik compile-lah dengan menggunakan TASM.EXE
dengan perintah :
```
 C:\>tasm a0
 Turbo Assembler Version 2.0 Copyright (c) 1988, 1990
 Borland International
 Assembling file: a0.ASM
 Error messages: None
 Warning messages: None
 Passes: 1
 Remaining memory: 306k
 C:\>dir a0.*
 Volume in drive C is S’to
 Directory of C:\
 A0 ASM 506 08-14-94 3:56p
 A0 OBJ 179 08-14-94 11:24p
 2 file(s) 685 bytes
 1,267,200 bytes free
```
Sampai disini sudah dihasilkan suatu file object dari KAL.ASM yang siap
dijadikan file COM(karena kita membuat file dengan format program COM).

Untuk itu lakukanlah langkah kedua, dengan perintah :
```
 C:\>tlink/t a0
 Turbo Link Version 3.0 Copyright (c) 1987, 1990
 Borland International
 C:\>tlink/t a0
 Turbo Link Version 3.0 Copyright (c) 1987, 1990
 Borland International
 C:\>dir a0.*
 Volume in drive C is S’to
 Directory of C:\
 A0 ASM 506 08-14-94 3:56p
 A0 OBJ 179 08-14-94 11:26p
 A0 MAP 229 08-14-94 11:26p
 A0 COM 8 08-14-94 11:26p
 4 file(s) 922 bytes
 1,266,176 bytes free
```
Setelah kedua proses itu selesai maka dihasilkanlah suatu program COM
yang sudah siap untuk dijalankan. File-file yang tidak digunakan bisa anda
hapus. Bila program 7.1. dijalankan maka pada layar akan ditampilkan
```
C:\>A0
 A
```

### LOOP(PENGULANGAN)

Perintah LOOP digunakan untuk melakukan suatu proses yang berulangulang. Adapun syntax dari perintah ini adalah :

**LOOP Tujuan**

Tujuan dapat berupa suatu label yang telah didefinisikan, contoh:
```
MOV CX,3 ; Banyaknya pengulangan yang dilakukan
Ulang : INT 10h ; Tempat terjadinya pengulangan
LOOP Ulang ; Lompat ke label 'Ulang'
```
Pada proses pengulangan dengan perintah LOOP, register CX memegang suatu
peranan yang khusus dimana register ini dijadikan sebagai counter/penghitung
terhadap banyaknya looping yang boleh terjadi. Setiap ditemui perintah LOOP,
maka register CX akan dikurangi dengan 1 terlebih dahulu, kemudian akan
dilihat apakah CX sudah mencapai 0. Proses looping akan selesai bila nilai
pada register CX mencapai nol. Seperti pada contoh diatas, maka interupsi 10h
akan dihasilkan sebanyak 3 kali(sesuai dengan nilai CX).
Perlu diperhatikan bahwa jangan sampai anda menaruh CX kedalam proses
LOOP karena hal ini akan menyebabkan nilai CX diSET terus sehingga proses
looping tidak bisa berhenti.

**TRICK:**
Bila anda menetapkan nilai CX menjadi nol pada saat pertama kali sebelum
dilakukan loop, maka anda akan mendapatkan proses looping yang terbanyak. Hal
ini dikarenakan proses pengurangan 0 dengan 1 akan menghasilkan nilai FFFFh(-1), 
Contoh :
```
MOV CX,00
Ulang: LOOP Ulang 
```

### ADD (Add Immediate Data)
Untuk menambah dalam bahasa assembler digunakan perintah ADD dan ADC
serta INC. Perintah ADD digunakan dengan syntax :
**ADD Tujuan,Asal**
Perintah ADD ini akan menambahkan nilai pada Tujuan dan Asal. Hasil yang
didapat akan ditaruh pada Tujuan, dalam bahasa pascal sama dengan instruksi
Tujuan:=Tujuan + Asal. Sebagai contohnya :
```
 MOV AH,15h ; AH:=15h
 MOV AL,4 ; AL:=4
 ADD AH,AL ; AH:=AH+AL, jadi AH=19h
```
Perlu anda perhatikan bahwa pada perintah ADD ini antara Tujuan dan Asal
harus mempunyai daya tampung yang sama, misalnya register AH(8 bit) dan AL(8
bit), AX(16 bit) dan BX(16 bit).
Mungkin ada yang bertanya-tanya, apa yang akan terjadi bila Tujuan
tempat hasil penjumlahan disimpan tidak mencukupi seperti pertambahan 1234h
dengan F221h.
```
 1234 h Biner --> 0001 0010 0011 0100
 F221 h Biner --> 1111 0010 0010 0001
 ---------- + --------------------- +
 10455 h 1 0000 0100 0101 0101
```
Pada pertambahan diatas dapat dilihat bahwa pertambahan bilangan 1234
dengan F221 akan menghasilkan nilai 10455. Supaya lebih jelas dapat anda lihat
pada pertambahan binernya dihasilkan bit ke 17, padahal register terdiri atas
16 bit saja. Operasi pertambahan yang demikian akan menjadikan carry flag
menjadi satu, Contoh :
```
 MOV AX,1234h ; NIlai AX:=1234h dan carry=0
 MOV BX,0F221h ; Nilai BX:=F221h dan carry=0
 ADD AX,BX ; Nilai AX menjadi 0455h dan carry=1
```

### ADC (Add Carry Plus Immediate Data to Accumulator)

Perintah ADC digunakan dengan cara yang sama pada perintah ADD, yaitu :
**ADC Tujuan,Asal**
Perbedaannya pada perintah ADC ini Tujuan tempat menampung hasil
pertambahan Tujuan dan Asal ditambah lagi dengan carry flag
(Tujuan:=Tujuan+Asal+Carry). Pertambahan yang demikian bisa memecahkan masalah
seperti yang pernah kita kemukakan, seperti pertambahan pada bilangan
12345678h+9ABCDEF0h.


Seperti yang telah kita ketahui bahwa satu register hanya mampu
menampung 16 bit, maka untuk pertambahan seperti yang diatas bisa anda gunakan
perintah ADC untuk memecahkannya, Contoh:
```
 MOV AX,1234h ; AX = 1234h CF = 0
 MOV BX,9ABCh ; BX = 9ABCh CF = 0
 MOV CX,5678h ; BX = 5678h CF = 0
 MOV DX,0DEF0h ; DX = DEF0h CF = 0
 ADD CX,DX ; CX = 3568h CF = 1
 ADC AX,BX ; AX = AX+BX+CF = ACF1
```
Hasil penjumlahan akan ditampung pada register AX:CX yaitu ACF13568h.
Adapun flag-flag yang terpengaruh oleh perintah ADD dan ADC ini adalah
CF,PF,AF,ZF,SF dan OF. 

### DSB.

## OPERASI STACK

### POP (Pop Stack to Memory)
Stack dapat kita bayangkan sebagai sebuah tabung yang panjang. Sedangkan
nilai pada register dapat dibayangkan berbentuk koin yang dapat dimasukkan
dalam tabung tersebut.
Untuk memasukkan nilai suatu register pada stack, digunakan perintah
push dengan syntax:
`PUSH Reg16Bit`
Sebagai contohnya pada perintah:
```
MOV AX,12
MOV BX,33
MOV CX,99
PUSH AX ; Simpan nilai AX pada stack
PUSH BX ; Simpan nilai BX pada stack
PUSH CX ; Simpan nilai CX pada stack
```
Maka pada stack akan tampak seperti:

![Penyimpanan nilai stack](https://miro.medium.com/max/600/1*vhJk3D_nRDwPJGdljOGViw.png)

 Gambar 14.2. Penyimpanan Nilai Pada Stack

Dari gambar 14.2. dapat anda lihat bahwa nilai yang terakhir
dimasukkan(99) akan terletak pada puncak tabung stack.
Untuk mengambil keluar koin nilai pada tabung stack, digunakan perintah
pop dengan syntax:
`POP Reg16Bit`
Perintah POP akan mengambil koin nilai pada stack yang paling atas dan
dimasukkan pada Reg16Bit. Dari sini dapat anda lihat bahwa data yang terakhir
dimasukkan akan merupakan yang pertama dikeluarkan. Inilah sebabnya operasi
stack dinamankan LIFO(Last In First Out).
Sebagai contohnya, untuk mengambil nilai dari register AX, BX dan CX
yang disimpan pada stack harus dilakukan pada register CX dahulu barulah BX
dan AX, seperti:
```
POP CX ; Ambil nilai pada puncak stack, masukkan ke CX
POP BX ; Ambil nilai pada puncak stack, masukkan ke BX
POP AX ; Ambil nilai pada puncak stack, masukkan ke AX
```

**Perhatikan:**
Bila anda terbalik dalam mengambil nilai pada stack dengan POP AX
kemudian POP BX dan POP CX, maka nilai yang akan anda dapatkan pada register
AX, BX dan CX akan terbalik. Sehingga register AX akan bernilai 99 dan CX akan
bernilai 12.
**TRIK:**
Seperti yang telah kita ketahui, data tidak bisa dicopykan antar segment
atau memory. Untuk mengcopykan data antar segment atau memory anda harus
menggunakan register general purpose sebagai perantaranya, seperti:
```
MOV AX,ES ; Untuk menyamakan register
MOV DS,AX ; ES dan DS
```
Dengan adanya stack, anda bisa menggunakannya sebagai perantara,
sehingga akan tampak seperti:
```
PUSH ES ; Untuk menyamakan register
POP DS ; ES dan DS
```

## Di atas adalah intruksi dasar dalam bahasa Assembly selebihnya bisa dipelajari lebih lanjut oleh kalian.

Reversing.ID \| Komunitas Reverse Engineering Indonesia

