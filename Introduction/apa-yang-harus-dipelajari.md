# Apa Yang Harus Dipelajari

Dalam Software Reverse Engineering, sederhananya, kita membongkar perangkat lunak dan melihat ada apa yang terjadi didalamnya. Tapi, itu semua bukanlah proses yang mudah, diperlukan berbagai macam teknik dan analisis yang dilakukan untuk memahami program tersebut, dan juga ada yang perlu dipelajari sebelum kalian melakukannya. Ini adalah hal - hal yang saya rangkum mengenai apa yang harus dipelajari untuk melakukan Software Reverse Engineering

### 1. Mengenali Pemrograman
Kita akan membongkar sebuah program, tentu saja minimal kita harus tau bagaimana program itu dibuat. Minimal kita sudah pernah membuat program sendiri. Dengan ini, kita minimal bisa mengetahui apa yang dilakukan didalam program itu dengan memikirkan "Jika kita akan membuat program seperti itu, apa yang kita lakukan, dan seperti apa alur programnya ?". Itu sederhana, tapi bisa sedikit membantu kita, jika pertama kali dihadapkan oleh sebuah program yang kita reverse, kita bisa membayangkan kelakuan si program tersebut.

### 2. Mengenali Bahasa Tingkat Rendah
Semua program yang kita buat dengan bahasa tingkat tinggi, akan dibuah menjadi bahasa tingkat rendah pada saat compiling ataupun pada saat eksekusinya. Komputer tidak mengerti bahasa tingkat tinggi, seperti C, C++, Python, PHP, dll. Dia hanya mengerti bahasa mesin yang tidak mungkin dipelajari oleh manusia. Terlalu sulit bagi kita untuk mempelajari bahasa mesin, karena itu dibuat bahasa tingkat rendah yang cukup manusiawi untuk dipelajari, itu dinamakan Bahasa Assembly. Satu intruksi Bahasa Assembly mewakili Satu intruksi Bahasa Mesin. 

Proses konversi dari bahasa tingkat tinggi ke bahasa mesin itu dilakukan oleh compiler pada saat compiling. Proses compile ini akan menghasilkan file binary executable yang didalamnya tidak ada source code kita (yang kita tulis sebelumnya) sama sekali. Isi dari file binary executable tersebut adalah kode program kita yang sudah dikonversi menjadi bahasa mesin (oleh compiler). Kita bisa mengubah bahasa mesin tersebut menjadi bahasa assembly, untuk memudahkan pembacaan kode untuk memahami programnya. Oleh karena itu kita harus memahami basic bahasa tingkat rendah terutama bahasa assembly.  Jika kita mereverse sebuah program binary executable, kita akan dihadapkan kode - kode assembly yang harus kita pahami

### 3. 
