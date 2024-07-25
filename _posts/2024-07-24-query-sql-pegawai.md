---
title: "Query SQL pada Tabel Pegawai"
date: 2024-07-24
categories: [SQL, Database]
tags: [SQL, Query, Siswa]
toc: true
pin: true
---

### Lembar Kerja Peserta Didik (LKPD)

**Judul LKPD**: Penggunaan Operator LIKE dan REGEXP dalam MySQL

**Mata Pelajaran**: Rekayasa Perangkat Lunak 

**Semester**: III

**Tempat**: Lab Komputer 

---

#### Petunjuk Belajar:
1. Bacalah setiap bagian dengan seksama.
2. Ikuti langkah-langkah yang diberikan dalam tugas.
3. Diskusikan dengan teman sekelas jika ada hal yang kurang dipahami.
4. Gunakan perangkat lunak MySQL untuk praktek.

#### Kompetensi yang Akan Dicapai:
Peserta didik mampu memahami dan menggunakan operator LIKE dan REGEXP dalam MySQL untuk pencarian data.

#### Indikator yang Akan Dicapai Peserta Didik:
1. Peserta didik dapat menjelaskan fungsi operator LIKE dan REGEXP.
2. Peserta didik dapat menggunakan operator LIKE untuk pencarian data dengan pola tertentu.
3. Peserta didik dapat menggunakan operator REGEXP untuk pencarian data dengan ekspresi reguler.

---

### Informasi Pendukung:

#### Operator LIKE
Operator LIKE digunakan dalam klausa WHERE untuk mencari pola yang ditentukan dalam kolom teks. Simbol `%` digunakan untuk mewakili nol atau lebih karakter, sementara `_` digunakan untuk mewakili satu karakter tunggal.

Contoh:
```sql
SELECT * FROM pegawai WHERE nama LIKE 'A%';
```
Query di atas akan mencari semua baris di tabel `pegawai` di mana kolom `nama` dimulai dengan huruf 'A'.

#### Operator REGEXP
Operator REGEXP digunakan untuk pencarian data berdasarkan ekspresi reguler. Ekspresi reguler adalah sekumpulan karakter yang menggambarkan pola pencarian.

Contoh:
```sql
SELECT * FROM pegawai WHERE nama REGEXP '^A';
```
Query di atas akan mencari semua baris di tabel `pegawai` di mana kolom `nama` dimulai dengan huruf 'A'.
### Perbedaan `LIKE` dan `REGEXP`
Perbedaan utama antara `LIKE` dan `REGEXP` dalam MySQL terletak pada fleksibilitas dan kompleksitas pola pencarian yang mereka dukung:

### `LIKE`
- **Penggunaan**: Digunakan untuk pencocokan pola sederhana.
- **Sintaks**: 
  - `%` mewakili nol atau lebih karakter apapun.
  - `_` mewakili tepat satu karakter apapun.
- **Contoh**:
  - `nama LIKE 'A%'` akan mencari semua nama yang dimulai dengan huruf 'A'.
  - `nama LIKE '_e%'` akan mencari semua nama yang memiliki 'e' sebagai karakter kedua.

### `REGEXP`
- **Penggunaan**: Digunakan untuk pencocokan pola yang lebih kompleks menggunakan ekspresi reguler.
- **Sintaks**: Mendukung berbagai pola ekspresi reguler seperti:
  - `.` mewakili tepat satu karakter apapun.
  - `*` mewakili nol atau lebih dari karakter sebelumnya.
  - `^` mewakili awal dari string.
  - `$` mewakili akhir dari string.
  - `[abc]` mewakili salah satu dari karakter 'a', 'b', atau 'c'.
  - `[a-z]` mewakili rentang karakter dari 'a' sampai 'z'.
- **Contoh**:
  - `nama REGEXP '^A'` akan mencari semua nama yang dimulai dengan huruf 'A'.
  - `nama REGEXP 'son$'` akan mencari semua nama yang berakhir dengan 'son'.
  - `nama REGEXP '^..a..$'` akan mencari semua nama yang terdiri dari lima karakter dan huruf ketiga adalah 'a'.

### Tabel Perbandingan

| Kriteria         | LIKE                           | REGEXP                          |
|------------------|-------------------------------|---------------------------------|
| Penggunaan       | Pencocokan pola sederhana      | Pencocokan pola kompleks        |
| Sintaks          | `%`, `_`                       | Ekspresi reguler lengkap        |
| Contoh Penggunaan| `LIKE 'A%'`                    | `REGEXP '^A'`                   |
| Dukungan Pola    | Terbatas                       | Sangat fleksibel dan beragam    |

### Contoh Query dengan `LIKE` dan `REGEXP`

1. **LIKE**: Mencari semua nama yang dimulai dengan huruf 'A'
   ```sql
   SELECT * FROM pegawai WHERE nama LIKE 'A%';
   ```

2. **REGEXP**: Mencari semua nama yang dimulai dengan huruf 'A'
   ```sql
   SELECT * FROM pegawai WHERE nama REGEXP '^A';
   ```

### Kesimpulan
- Gunakan `LIKE` untuk pencarian pola sederhana yang mudah dipahami dan diimplementasikan.
- Gunakan `REGEXP` untuk pencarian pola yang lebih kompleks dan fleksibel, terutama ketika `LIKE` tidak cukup untuk menangani kebutuhan pencarian yang lebih spesifik.

### Tugas SQL untuk Membuat Database dan Tabel
```sql
CREATE DATABASE IF NOT EXISTS perusahaan;
USE perusahaan;

CREATE TABLE IF NOT EXISTS pegawai (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(100),
    departemen VARCHAR(100),
    gaji INT
);
```

### SQL untuk Menambahkan 40 Data Random
```sql
INSERT INTO pegawai (nama, departemen, gaji) VALUES
('Andi Wirawan', 'IT', 8000000),
('Budi Santoso', 'HR', 7000000),
('Citra Dewi', 'Finance', 9000000),
('Dedi Kusuma', 'IT', 8500000),
('Eka Saputra', 'HR', 7500000),
('Feri Yanto', 'Marketing', 9500000),
('Gina Hartono', 'Finance', 9000000),
('Hadi Pranoto', 'IT', 8200000),
('Indah Sari', 'HR', 7800000),
('Joko Widodo', 'Marketing', 10000000),
('Kiki Amalia', 'Finance', 8700000),
('Lukman Hakim', 'IT', 8500000),
('Maya Lestari', 'HR', 7600000),
('Nina Hartono', 'Marketing', 9400000),
('Oscar Putra', 'Finance', 8800000),
('Putu Wibowo', 'IT', 8300000),
('Qori Annisa', 'HR', 7700000),
('Rina Wijaya', 'Marketing', 9500000),
('Santi Agustina', 'Finance', 9100000),
('Tomi Saputra', 'IT', 8400000),
('Ujang Mulyadi', 'HR', 7800000),
('Vina Anggraini', 'Marketing', 9600000),
('Wawan Setiawan', 'Finance', 8900000),
('Xenia Pratiwi', 'IT', 8200000),
('Yusuf Maulana', 'HR', 7900000),
('Zara Pramudita', 'Marketing', 9700000),
('Ayu Wulandari', 'Finance', 9200000),
('Bagas Nugroho', 'IT', 8500000),
('Cici Permatasari', 'HR', 8000000),
('Dian Kartika', 'Marketing', 9300000),
('Eko Purwanto', 'Finance', 9100000),
('Fauzan Hakim', 'IT', 8600000),
('Gilang Saputra', 'HR', 8200000),
('Hana Pratama', 'Marketing', 9500000),
('Irwan Susanto', 'Finance', 9000000),
('Joko Santoso', 'IT', 8400000),
('Kartika Dewi', 'HR', 8100000),
('Lina Susanti', 'Marketing', 9600000),
('Maman Suryaman', 'Finance', 9200000),
('Nino Setiawan', 'IT', 8600000);
```

### Soal-soal SQL
1. Memilih Semua Pegawai yang Nama Depannya Dimulai dengan Huruf 'S'

2. Memilih Semua Pegawai yang Nama Belakangnya Berakhiran dengan 'ri'

3. Memilih Semua Pegawai yang Bekerja di Departemen 'Finance' dan Bergaji Lebih dari 8500000

4. Memilih Semua Pegawai yang Nama Depannya Dimulai dengan 'B' dan Bekerja di Departemen 'HR'

5. Memilih Semua Pegawai yang Namanya Mengandung Angka

6. Memilih Semua Pegawai yang Nama Depannya Memiliki Huruf 'd' sebagai Huruf Ketiga

7. Memilih Semua Pegawai yang Nama Depannya Terdiri dari 6 Huruf dan Huruf Ketiga adalah 'k'

8. Memilih Semua Pegawai yang Nama Depannya Dimulai dengan Huruf 'A' atau 'a' (Case Insensitive)

9. Memilih Semua Pegawai yang Bergaji Antara 8000000 dan 9000000

10. Menghitung Jumlah Pegawai yang Bekerja di Departemen 'IT'

11. Memilih Semua Pegawai yang Nama Depannya Dimulai dengan Huruf 'D' atau 'A' dan Bekerja di Departemen 'Marketing'

12. Memilih Semua Pegawai yang Namanya Terdiri dari Lebih dari 7 Karakter

13. Memilih Semua Pegawai yang Nama Depannya Tidak Dimulai dengan Huruf 'k'

14. Memilih Semua Pegawai yang Nama Depannya Dimulai dengan Huruf Vokal (A, F, I, W, U)

15. Memilih Semua Pegawai yang Bergaji 8000000, 8500000, atau 9000000

Jawaban untuk soal-soal di atas dapat dihasilkan dengan menjalankan query-query tersebut pada database yang telah dibuat.

#### Penilaian:
- Pemahaman Fungsi Operator 
- Ketepatan Penggunaan Operator LIKE 
- Ketepatan Penggunaan Operator REGEXP 
- Kebenaran Hasil Query 

---
