# Sistem Bilangan

Unit terkecil dalam penyimpanan komputer disebut bit. sebuah bit bisa bernilai 0 atau 1. beberapa bit membentuk byte. 1 byte = 8 bit. Jarak angka yang dapat disimpan dimasing masing ukuran data ini adalah 0 sampai 2^n-1, dimana n adalah jumlah bitnya. Prosesor mendukung ukuran data seperti berikut :
```
                 +------------------+---------------------------+
                 |      SIZE        |          Jarak            |
+----------------+------------------+---------------------------+
| Byte           | 1byte atau 8bit  |  0 - 255                  |
| Word           | 2byte atau 16bit |  0 - 65535                |
| Doubleword     | 4byte atau 32bit |  0 - 4294967295           |
| Quadword       | 8byte atau 64bit |  0 - 18446744073709551615 |
+----------------+------------------+---------------------------+
```        

## Bilangan biner

Bilangan biner merupakan sistem bilangan base 2, yang artinya setiap posisi mempunyai nilai 2 pangkat n, dimana, n adalah index dari posisi bilangan itu dari kiri

```
             +---------------------------------------------------------------+
     2^n     |  2^7  |  2^6  |  2^5  |  2^4  |  2^3  |  2^2  |  2^1  |  2^0  |
             +---------------------------------------------------------------+
             |  128  |   64  |   32  |   16  |   8   |   4   |   2   |   1   |
             +---------------------------------------------------------------+
 Bit Number  |   7   |   6   |   5   |   4   |   3   |   2   |   1   |   0   |
             +---------------------------------------------------------------+

             Example : [1] 11111111 = 1+2+4+8+16+32+64+128 = 255
                       [2] 11010011 = 1+2+16+64+128        = 211
                       [3] 00011101 = 1+4+8+16             = 29
```

Jika kita ingin mengubah bilangan desimal ke biner, maka yang dilakukan adalah mengambil sisa pembagian dari pembagian bil. desimal dengan 2, hasil dari pembagian itu dibagi lagi dengan 2 dan diambli sisa pembagiannya, sampai hasil pembagian itu sama dengan 0 
```
Example : [1] 29
-  29 / 2 = 14  --> Sisa 1
-  14 / 2 = 7   --> Sisa 0  
-  7  / 2 = 3   --> Sisa 1
-  3  / 2 = 1   --> Sisa 1
-  1  / 2 = 0   --> Sisa 1
            '----------> 0

-> Hasilnya adalah : 011101
[2] 36
-  36 / 2 = 18  --> Sisa 0
-  18 / 2 =  9  --> Sisa 0
-   9 / 2 =  4  --> Sisa 1
-   4 / 2 =  2  --> Sisa 0
-   2 / 2 =  1  --> Sisa 0
             '---------> 1
-> Hasilnya adalah : 100100  
```
## Bilangan Hexa

Bilangan hexa merupakan sistem bilangan base 16, sebenarnya bilangan hexa dibuat untuk menyederhanakan bilangan biner. Bilangan hexa berjumlah 16, dimulai dari 0-9 dan A-F, dimana A-F mewakili 10-15.

```
          +-------------------------+
          | Hexa |   Biner   | Dec  |
          +-------------------------+           Example : [1] 0000 1111     =  0 F
          |  0   |   0000    |  0   |                         '--' '--'        Y Y
          |  1   |   0001    |  1   |                           `.   `---------|-'
          |  2   |   0010    |  2   |                             `------------'
          |  3   |   0011    |  3   |                     [2] 1101 0100     =  D 4
          |  4   |   0100    |  4   |                         '--' '--'        Y Y
          |  5   |   0101    |  5   |                           `.   `---------|-'
          |  6   |   0110    |  6   |                             `------------'
          |  7   |   0111    |  7   |                     [3] C 5           =  11000100
          |  8   |   1000    |  8   |                         Y Y              '--''--'
          |  9   |   1001    |  9   |                         '-|---------------^   |
          |  A   |   1010    |  10  |                           '-------------------'
          |  B   |   1011    |  11  |                   
          |  C   |   1100    |  12  |                  
          |  D   |   1101    |  13  |                 
          |  E   |   1110    |  14  |                
          |  F   |   1111    |  15  |                        
          +-------------------------+
```
Untuk mengkonversikan Hexa ke desimal, kita harus mengalikan masing-masing angka bilangan hexa dengan 16^n dimana n adalah posisi bilangan dihitung dari kiri. Untuk memudahkan saya membuat table base 16.

```
                +-----------------------------------+
       2^n      |  16^3  |  16^2  |  16^1  |  16^0  |
                +-----------------------------------+
                |  4096  |  256   |   16   |    1   |
                +--------+--------+--------+--------+
  Bit Number    |    3   |   2    |   1    |    0   |
                +-----------------------------------+
       
                Example : [1] FD         =  15*16 + 13*1  
                                         =  253
                          [2] C45        =  12*256 + 4*16 + 5*1 
                                         =  3141
```

## Penjumlahan bilangan biner
         
Bilangan biner bisa melakukan operasi aritmatika seperti pengurangan dan penjumlahan. Seperti penjumlahan dan pengurangan biasa tetapi yg perlu kalian ketahui bilangan biner hanya mempunyai 2 angka saja yaitu 1 dan 0. Contoh sederhana : 
```
00000101 (5)  
00000010 (2)
----------+
00000111 (7)
```         
Didalam pertambahan ini juga terdapat carry, carry terjadi karena hasil penjumlahan yang dilakukan melebihi 1, seperti 1+1 = 10 (2 dalam desimal), 1+1+1 = 11 (3 dalam desimal). Contoh

```
00000101 (5)
00000001 (1)
----------+                                                                     
00000110 (6)    ,-> bil. 10 (2 bit pertama) didapat dari penjumlahan bilangan seblumnya yaitu 1+1, 
      `---------'   Karena 1+1 = 10, maka bit 1 ditaruh di bit selanjutnya     1 <--. 
                                                                                           ..01   | c
                                                                                           ..01   | a
                                                                                          ------+ | r
                                                                                             10   | r
                                                                                               `--' y
```
Untuk melakukan pengurangan bilangan biner, kita harus bisa mengubah bilangan positif menjadi negatif. Contoh pengurangan 10 dengan 5, sama saja dengan 10 + (-5), artinya kita harus mengubah angka 5 (positif) menjadi -5. Untuk mengubahnya mari kita pelajari tentang Signed dan Unsigned number terlebih dahulu.

##  Signed dan Unsigned number

Seperti yang kita ketahui. jumlah bilangan biner tergantung dari jumlah bit itu sendiri. contoh bilangan biner 8bit. dari 00000001, 00000010, 00000011, sampai 11111111. bilangan biner 8bit ini mempunyai jumlah 256 atau 2^8 atau 2^n, dimana n adalah jumlah bit itu sendiri. dari 0-255 merupakan bilangan positif. Bagaimana jika kita ingin membuat bilangan negatif ? 

Salah satu cara untuk membedakan bilangan negatif dan positif adalah tanda yg berada pada bit MSB (bit paling kiri/terakhir) jika bit ini diset (bernilai 1) maka bilangan tersebut adalah bilangan negatif, dan sebaliknya jika bernilai 0 maka bilangan itu positif. 

Untuk mengubah bilangan positif ke negatif adalah dengan cara mengubah bit 1 menjadi 0, dan bit 0 menjadi 1, setelah itu menambahkannya dengan 1. Contoh 
```
      [1] --> 00000101 (5). Sekarang kita ubah bit 0 menjadi 1 dan sebaliknya. menjadi
              11111010      Lalu kita tambahkan 1, menjadi
              11111011      <-- Ini adalah -5 dalam bilangan biner
```
## Pengurangan bilangan biner

Sekarang kita akan mempelajari bagaimana pengurangan dilakukan pada bilangan biner. Sebenernya ada 2 cara pengurangan bilangan biner. Cara pertama yaitu langsung mengurangi kedua bilangan biner, Cara yang kedua yaitu mengubah operand menjadi bilangan negatif dan melakukan pertambahan. Kita akan menggunakan kedua cara tersebut.
```
    Contoh: 
    [1] 00001010 (10)
        00000101  (5)
        --------- -                 
        00000101  (5) 
```
### Cara #1 : 
- Pertama, bit pertama (paling kanan) dikurangi, jadi 0-1, karena operand pertama lebih besar dari operand yg kedua, maka dilakukan peminjaman. yaitu mengambil angka yg berada dibit selanjutnya
- Sebelumnya bit pertama 0, Setelah diambil menjadi 10. dan bit kedua menjadi 0
  (yg sebelumnya menjadi 1, karena bitnya sudah diambil)
- Setelah itu dilakukan pengurangan 10 (2) - 1 (1) = 1 
- Bit kedua dikurangi, karen bit kedua sudah menjadi 0, maka jadinya 0 - 0 = 0
- Bit ketiga dilakukan dengan cara yg sama seperti bit pertama, yaitu dilakukan 
  peminjaman terhadap bit selanjutnya (bit keempat) dan dilakukan pengurangan
- Bit keempat (operand pertama) sudah menjadi 0, karena sudah diambil untuk
  bit ketiga tadi, jadinya 0 - 0 = 0
- Dilanjutkan bit2 selanjutnya 0 - 0 = 0

### Cara #2 
- Pertama operand kedua (5) diubah menjadi bilangan negatif, seperti pada bagian sebelumnya (2.5 tentang signed dan unsigned number). 00000101 diubah, bit 1 menjadi 0, dan bit 0 menjadi 1 hasilnya 11111010. Setelah itu ditambahkan 1 menjadi 11111011.
- Selanjutnya 10 ditambahkan dengan -5 (yg sebelmunya sudah diubah ke negatif)

00001010 (10)
11111011 (-5)
---------+
100000101
- Hasilnya adalah 100000101 (9 bit), kenapa 9 bit ?, karena terjadi carry pada bit terakhir. Kita buang saja bit terakhir itu menjadi 00000101 (5), dan itulah hasilnya.

# Representasi Data
      
Data disimpan dalam komputer dalam bentuk biner. Misalkan saja karakter "A" akan disimpan didalam komputer menjadi "01000001", untuk memudahkan kita ubah bilangan biner tsb ke hexa menjadi 0x41 (65 dalam desimal). Begitu juga karakter "B" diformat menjadi 0x42. Itu sudah diatur dalam format ascii.

ASCII adalah sistem pemformatan karaketer (huruf, simbol, angka) yang direpresentasikan menjadi 7bit bilangan biner. Jumlah karakter yang dapat didefinisikan yaitu sebanyak 2^7 atau 128 karakter. Silahkan mengacu pada link berikut untuk dapat melihat lebih banyak karakter ascii http://www.asciitable.com/

Misalkan kita mempunyai karakter "ABCD", maka dikkomputer akan disimpan sebagai `0x41424344`. Karakter "Saya" jika direpresentasikan oleh komputer menjadi `0x53617961`