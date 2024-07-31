---
layout: post
tags: [Panduan Dasar Excel, Excel]
categories: [TIK]
#date: 2019-06-25 13:14:15
#excerpt: ''
#image: 'BASEURL/assets/blog/img/.png'
#description:
#permalink:
title: 'Panduan Dasar Excel'
---


Excel adalah perangkat lunak yang digunakan untuk mengelola data dalam bentuk tabel. Ini sangat berguna untuk berbagai keperluan, seperti analisis data, pembuatan laporan, dan manajemen data. Berikut adalah beberapa fitur dasar Excel yang perlu Anda ketahui sebagai pemula:

1. **Worksheet dan Workbook**:
   - **Workbook** adalah file Excel yang dapat berisi beberapa **worksheet** atau lembar kerja.
   - Setiap worksheet terdiri dari sel-sel yang disusun dalam baris dan kolom.

2. **Sel**:
   - **Sel** adalah unit terkecil di worksheet yang dapat berisi data, seperti teks, angka, atau rumus.
   - Sel diidentifikasi berdasarkan kombinasi huruf kolom dan nomor baris, misalnya A1, B2.

3. **Formula dan Fungsi**:
   - Excel memungkinkan Anda untuk melakukan perhitungan otomatis menggunakan formula. Formula dimulai dengan tanda "=".
   - **Fungsi** adalah formula yang telah ditentukan sebelumnya untuk melakukan tugas tertentu, seperti SUM (menjumlahkan angka), AVERAGE (menghitung rata-rata), dan lainnya.

4. **Format Sel**:
   - Anda dapat mengubah tampilan data di sel, seperti font, warna, dan format angka (misalnya, mata uang, persentase).

5. **Filter dan Sortir**:
   - Excel memungkinkan Anda untuk menyaring data berdasarkan kriteria tertentu dan mengurutkan data secara ascending atau descending.

6. **Grafik**:
   - Anda dapat membuat grafik untuk memvisualisasikan data, seperti grafik batang, grafik garis, dan grafik pie.

7. **Pivot Table**:
   - Fitur ini memungkinkan Anda untuk meringkas data secara dinamis, sangat berguna untuk analisis data yang lebih mendalam.
   
   Berikut adalah contoh data sederhana yang bisa Anda gunakan untuk latihan di Excel:

### Tabel Penjualan Bulanan

| No | Nama Produk | Kategori  | Bulan   | Jumlah Terjual | Harga Satuan | Total Penjualan |
|----|-------------|-----------|---------|----------------|--------------|-----------------|
| 1  | Pensil      | Alat Tulis| Januari | 100            | 2.000        | =E2*F2          |
| 2  | Buku        | Alat Tulis| Januari | 50             | 5.000        | =E3*F3          |
| 3  | Penghapus   | Alat Tulis| Februari| 75             | 1.500        | =E4*F4          |
| 4  | Buku        | Alat Tulis| Februari| 120            | 5.000        | =E5*F5          |
| 5  | Pensil      | Alat Tulis| Maret   | 150            | 2.000        | =E6*F6          |

**Penjelasan:**
- **Kolom "No"**: Nomor urut setiap produk.
- **Kolom "Nama Produk"**: Nama produk yang dijual.
- **Kolom "Kategori"**: Kategori dari produk tersebut.
- **Kolom "Bulan"**: Bulan penjualan.
- **Kolom "Jumlah Terjual"**: Jumlah unit produk yang terjual.
- **Kolom "Harga Satuan"**: Harga per unit produk.
- **Kolom "Total Penjualan"**: Menggunakan rumus Excel untuk menghitung total penjualan (Jumlah Terjual dikalikan dengan Harga Satuan).

Berikut adalah pengembangan lebih lanjut dari contoh data penjualan yang telah disebutkan sebelumnya:

### Penambahan Fungsi dan Rumus Lanjutan

1. **Penambahan Kolom Diskon**: Misalnya, memberikan diskon jika jumlah penjualan mencapai jumlah tertentu.

   | No | Nama Produk | Kategori  | Bulan   | Jumlah Terjual | Harga Satuan | Total Penjualan | Diskon | Total Setelah Diskon |
   |----|-------------|-----------|---------|----------------|--------------|-----------------|--------|----------------------|
   | 1  | Pensil      | Alat Tulis| Januari | 100            | 2.000        | 200.000         | 10%    | =G2*(1-H2)           |
   | 2  | Buku        | Alat Tulis| Januari | 50             | 5.000        | 250.000         | 0%     | =G3*(1-H3)           |
   | 3  | Penghapus   | Alat Tulis| Februari| 75             | 1.500        | 112.500         | 5%     | =G4*(1-H4)           |
   | 4  | Buku        | Alat Tulis| Februari| 120            | 5.000        | 600.000         | 15%    | =G5*(1-H5)           |
   | 5  | Pensil      | Alat Tulis| Maret   | 150            | 2.000        | 300.000         | 10%    | =G6*(1-H6)           |

   **Keterangan**:
   - **Kolom "Diskon"**: Menampilkan persentase diskon yang diberikan.
   - **Kolom "Total Setelah Diskon"**: Menghitung total penjualan setelah diskon.

2. **Penggunaan Fungsi `IF` untuk Diskon**:
   - Anda bisa menambahkan logika untuk diskon berdasarkan jumlah penjualan menggunakan fungsi `IF`. Misalnya, memberikan diskon 10% jika jumlah terjual lebih dari 100 unit.

   ```excel
   =IF(E2 > 100, 0.1, 0)
   ```

   Rumus di atas dapat ditempatkan di kolom "Diskon" untuk menentukan apakah produk tersebut mendapatkan diskon.

3. **Pivot Table untuk Analisis Lebih Lanjut**:
   - Dengan menggunakan pivot table, Anda dapat menganalisis total penjualan berdasarkan kategori, bulan, atau produk. Ini sangat berguna untuk melihat tren penjualan atau menentukan produk yang paling laris.

4. **Grafik untuk Visualisasi**:
   - Anda dapat membuat grafik batang atau garis untuk memvisualisasikan data penjualan per bulan atau per kategori produk, yang membantu dalam memahami tren dan membuat keputusan bisnis.




