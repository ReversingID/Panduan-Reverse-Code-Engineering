# Arsitektur Komputer

## Konsep

## CPU
CPU (Central Processing Unit) adalah pusat pemroses pada komputer. CPU bekerja dengan cara mengeksekusi(mengerjakan) instruksi-intruksi. Instruksi dapat berbentuk urutan byte atau array of byte yang telah ditentukan nilainya sesuai spesifikasi CPU.

## Register
Register adalah tempat penyimpanan sementara. Register merupakan salah satu bagian dari CPU. Ukuran penyimpanan data pada register sangat kecil tetapi waktu pengaksesan datanya sangat cepat. Biasanya register menyimpan alamat memory, atau jenis data apapun dengan ukuran yang kecil. Register hanya berukuran sekitar 8 sampai 64bit, sangat kecil.

## Memori
Anggap memori itu sebagai rak - rak yang bertumpuk sangat tinggi. Setiap rak mempunyai nomornya masing - masing, nomor ini kita akan sebut sebagai alamat rak/memori. Setiap rak akan mempunyai nomor/alamat rak, berurut dari yang kecil sampai yang besar. Alamat rak yang paling kecil akan berada di tumpukan paling atas, sebaliknya alamat rak paling besar di taruh di tempat yang paling bawah.

Setiap rak dapat menyimpan suatu data sebesar 4 byte (32 bit) atau 8 byte (64), itu bergantung pada arsitektur yang dipakai 32 atau 64 bit. Pada kenyataannya, alamat setiap rak akan bertambah 4 atau 8 (sesuai jumlah byte yang ada di setiap rak). Sebagai contoh alamat rak pertama (pada arsitektur 32 bit) yaitu 0, alamat rak kedua berada pada nomor 4, alamat rak ketiga berada pada nomor 8, dan seterusnya.
