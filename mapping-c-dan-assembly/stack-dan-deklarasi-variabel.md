# Stack dan deklarasi variable

Dalam bahasa C kita telah mengetahui bahwa sebelum kita menggunakan variable kita harus lebih dahulu mendeklarasikan variable tersebut. Mendeklarasikan artinya menentukan tipe data dari variable sebelum digunakan. Kenapa harus menentukan tipe datanya ?, karena hal tersebut dibutuhkan oleh compiler untuk mengalokasikan memory untuk variable lokal.

Pada low level, ketika sebuah fungsi dipanggil, maka stack akan berkembang (bertambah ukurannya) sesuai dengan ukuran total dari variable lokal yang diperlukan. Untuk itulah compiler membutuhkan informasi tentang tipe data dari variable, agar ketika program mengalokasikan memory baru untuk variable lokal ukurannya pas, tidak lebih, tidak kurang.

Pada level assembly, variable lokal akan disimpan di area memory yang bernama stack. Setiap fungsi mempunyai area stacknya sendiri - sendiri. Stack akan dialokasikan ketika sebuah fungsi dipanggil dan akan didealokasikan ketika fungsi tersebut berakhir (return). 

    Saya anggap kalian telah mengerti tentang stack, stack pointer, base pointer sebelumnya. Jika belum, kalian bisa mempelajari konsep stack di internet atau buku. Saya merekomendasikan jika kalian mempelajari konsep stack di bahasa assembly.

Sekarang kita praktik dengan program C sederhana, kita akan melihat bagaimana deklarasi variable dalam level assembly dan seperti apa peran stack dalam alokasi/dealokasi memory.

