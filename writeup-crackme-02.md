# Writeup Crackme #2: Simple Math Logic Validation

## 1. Informasi Target
* **Platform:** crackmes.one
* **Arsitektur:** x86/x64 PE
* **Tingkat Kesulitan:** Mudah

## 2. Proses Analisis (Analisis Dinamis)
Tantangan kedua ini tidak lagi menyimpan kunci dalam bentuk teks polos (*hardcoded string*). Program meminta input berupa angka PIN dan melakukan operasi matematika dasar di dalam register CPU sebelum melakukan validasi.

Saya menggunakan debugger **x64dbg** untuk mengamati alur jalannya registrasi secara real-time:
1. Memasang *breakpoint* tepat sebelum fungsi pembacaan input user dipanggil.
2. Melakukan teknik *Step-Over* (F8) untuk menelusuri instruksi demi instruksi assembly.
3. Menemukan bagian di mana nilai input saya yang berada di register `EAX` dikalikan dengan angka konstanta menggunakan operasi `IMUL`.

## 3. Logika Validasi
Berdasarkan pembacaan instruksi assembly, aturan validasi matematika yang diterapkan oleh program adalah:
* Input user disimpan ke register `EAX`.
* Nilai di `EAX` dikalikan dengan `2` (`IMUL EAX, 2`).
* Hasil perkalian kemudian dibandingkan dengan nilai heksadesimal `0x7D0` (atau `2000` dalam desimal) menggunakan instruksi `CMP EAX, 0x7D0`.

$$Input \times 2 = 2000$$
$$Input = \frac{2000}{2} = 1000$$

* **Kesimpulan:** PIN rahasia yang harus dimasukkan agar program sukses melakukan bypass adalah **`1000`**.

