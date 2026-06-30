# Writeup Crackme #3: XOR Encryption Bypass Challenge

## 1. Informasi Target
* **Platform:** crackmes.one
* **Arsitektur:** x64 PE Executable
* **Tingkat Kesulitan:** Menengah

## 2. Deskripsi Masalah
Pada tantangan ketiga ini, serial key dienkripsi di dalam memori menggunakan operasi logika XOR bitwise sederhana. Jika kita mengekstrak string secara statis, kita hanya akan mendapatkan karakter acak yang tidak valid.

## 3. Solusi & Langkah Bypass
Untuk melewati proteksi ini, saya menggunakan kombinasi analisis statis (Ghidra) untuk memahami algoritma enkripsinya, dan analisis dinamis (x64dbg) untuk melihat hasil dekripsinya di memori.

1. Di Ghidra, saya menemukan sebuah perulangan (*loop*) yang melakukan operasi XOR antara setiap byte *buffer* rahasia dengan kunci tetap yaitu `0x5A`.
2. Alih-alih melakukan dekripsi manual dengan menghitung matematika bitwise, saya membuka file tersebut di **x64dbg**.
3. Saya memasang *breakpoint* tepat setelah fungsi *looping* XOR selesai dieksekusi.
4. Saya memeriksa jendela **Dump Memori** pada alamat pointer yang ditunjuk oleh register `EDX`.
5. Di dalam dump memori tersebut, string acak tadi sudah berubah bentuk (terdekripsi secara otomatis oleh aplikasi) menjadi teks polos.

* **Hasil Akhir:** Kunci rahasia hasil dekripsi runtime yang berhasil didapatkan adalah **`XOR_Bypass_Success`**.
