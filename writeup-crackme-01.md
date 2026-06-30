# Writeup Crackme #1: Simple Hardcoded Key Verification

## 1. Informasi Target
* **Platform:** crackmes.one
* **Arsitektur:** x64 PE (Windows Binary)
* **Tingkat Kesulitan:** Sangat Mudah / Pemula

## 2. Proses Analisis (Analisis Statis)
Pada tantangan pertama ini, saya melakukan analisis statis menggunakan bantuan dekompiler **Ghidra**. Langkah pertama adalah mencari string interaktif yang muncul pada program saat dijalankan.

1. Saya membuka jendela *Defined Strings* di Ghidra untuk melihat semua teks konstan yang tertanam di dalam `.rdata section`.
2. Ditemukan string instruksi `"Ketikkan Serial Number Anda: "` dan string verifikasi sukses `"Selamat! Serial Number Benar."`.
3. Saya melakukan *double-click* pada string tersebut untuk melihat referensi silang (*cross-reference / XREF*) menuju fungsi utama (`main`) yang mengeksekusinya.

## 3. Rekonstruksi Logika & Penemuan Key
Di dalam fungsi `main`, terdapat fungsi bawaan `strcmp` (String Compare) yang membandingkan input dari user dengan sebuah string statis yang sudah disimpan oleh developer di memori. 

Potongan kode semu (*pseudo-code*) tampak seperti berikut:
```c
if (strcmp(input_user, "AmikomRE2026") == 0) {
    printf("Selamat! Serial Number Benar.");
} else {
    printf("Serial Number Salah, Coba Lagi!");
}

