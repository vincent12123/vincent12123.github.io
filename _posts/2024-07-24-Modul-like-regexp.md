---
title: "Modul Like Regexp SQL pada Tabel Siswa"
date: 2024-07-24
categories: [SQL, Database]
tags: [SQL, Query, Siswa]
---

### SQL untuk Membuat Database dan Tabel
```sql
CREATE DATABASE IF NOT EXISTS sekolah;
USE sekolah;

CREATE TABLE IF NOT EXISTS siswa (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(100),
    alamat VARCHAR(100),
    usia INT
);
```

### SQL untuk Menambahkan 50 Data Random
```sql
INSERT INTO siswa (nama, alamat, usia) VALUES
('Aaron Smith', 'Jakarta', 16),
('Bella Anderson', 'Bandung', 18),
('Charlie Johnson', 'Surabaya', 17),
('David Wilson', 'Jakarta', 19),
('Eva Thomas', 'Bandung', 17),
('Frank Martin', 'Surabaya', 18),
('Grace Lee', 'Jakarta', 20),
('Hannah White', 'Bandung', 16),
('Ian Harris', 'Surabaya', 18),
('Jane Clark', 'Jakarta', 19),
('Kevin Lewis', 'Bandung', 17),
('Laura Walker', 'Surabaya', 20),
('Mike Hall', 'Jakarta', 21),
('Nina Allen', 'Bandung', 19),
('Oscar Young', 'Surabaya', 22),
('Paul King', 'Jakarta', 18),
('Quinn Wright', 'Bandung', 17),
('Rachel Scott', 'Surabaya', 19),
('Sam Green', 'Jakarta', 20),
('Tina Adams', 'Bandung', 18),
('Uma Baker', 'Surabaya', 21),
('Victor Nelson', 'Jakarta', 22),
('Wendy Carter', 'Bandung', 16),
('Xander Mitchell', 'Surabaya', 17),
('Yara Perez', 'Jakarta', 18),
('Zane Roberts', 'Bandung', 19),
('Amy Moore', 'Surabaya', 20),
('Brian Reed', 'Jakarta', 21),
('Chloe Cook', 'Bandung', 22),
('Dylan Bailey', 'Surabaya', 16),
('Ella Bell', 'Jakarta', 17),
('Fiona Parker', 'Bandung', 18),
('George Hughes', 'Surabaya', 19),
('Hazel Price', 'Jakarta', 20),
('Jack Turner', 'Bandung', 21),
('Kara Diaz', 'Surabaya', 22),
('Liam Edwards', 'Jakarta', 16),
('Mia Collins', 'Bandung', 17),
('Noah Stewart', 'Surabaya', 18),
('Olivia Sanchez', 'Jakarta', 19),
('Peter Morris', 'Bandung', 20),
('Queen Rogers', 'Surabaya', 21),
('Riley Cook', 'Jakarta', 22),
('Sophie Ross', 'Bandung', 16),
('Theo Foster', 'Surabaya', 17),
('Uma Powell', 'Jakarta', 18),
('Victor Hughes', 'Bandung', 19),
('Willa Kelly', 'Surabaya', 20),
('Xander Wood', 'Jakarta', 21),
('Yara Brooks', 'Bandung', 22);
```

### 1. Memilih Semua Siswa yang Nama Depannya Dimulai dengan Huruf 'A'
```sql
SELECT * FROM siswa WHERE nama LIKE 'A%';
```
**Jawaban:**
```plaintext
id | nama          | alamat | usia
-----------------------------------
 1 | Aaron Smith   | Jakarta| 16
26 | Amy Moore     | Surabaya | 20
```

### 2. Memilih Semua Siswa yang Nama Belakangnya Berakhiran dengan 'son'
```sql
SELECT * FROM siswa WHERE nama REGEXP 'son$';
```
**Jawaban:**
```plaintext
id | nama            | alamat   | usia
---------------------------------------
 3 | Charlie Johnson | Surabaya | 17
```

### 3. Memilih Semua Siswa yang Tinggal di 'Jakarta' dan Berusia Lebih dari 17 Tahun
```sql
SELECT * FROM siswa WHERE alamat = 'Jakarta' AND usia > 17;
```
**Jawaban:**
```plaintext
id | nama        | alamat  | usia
---------------------------------
 4 | David Wilson| Jakarta | 19
 7 | Grace Lee   | Jakarta | 20
10 | Jane Clark  | Jakarta | 19
13 | Mike Hall   | Jakarta | 21
16 | Paul King   | Jakarta | 18
19 | Sam Green   | Jakarta | 20
22 | Victor Nelson | Jakarta | 22
31 | Hazel Price | Jakarta | 20
```

### 4. Memilih Semua Siswa yang Nama Depannya Dimulai dengan 'B' dan Tinggal di 'Bandung'
```sql
SELECT * FROM siswa WHERE nama LIKE 'B%' AND alamat = 'Bandung';
```
**Jawaban:**
```plaintext
id | nama           | alamat  | usia
-------------------------------------
 2 | Bella Anderson | Bandung | 18
27 | Brian Reed     | Bandung | 21
```

### 5. Memilih Semua Siswa yang Namanya Mengandung Angka dan Tinggal di 'Surabaya'
```sql
SELECT * FROM siswa WHERE nama REGEXP '[0-9]' AND alamat = 'Surabaya';
```
**Jawaban:**
Tidak ada data yang memenuhi kondisi ini dalam contoh data yang diberikan.

### 6. Memilih Semua Siswa yang Nama Depannya Memiliki Huruf 'e' sebagai Huruf Kedua
```sql
SELECT * FROM siswa WHERE nama LIKE '_e%';
```
**Jawaban:**
```plaintext
id | nama          | alamat  | usia
-----------------------------------
 5 | Eva Thomas    | Bandung | 17
11 | Kevin Lewis   | Bandung | 17
15 | Oscar Young   | Surabaya | 22
18 | Rachel Scott  | Surabaya | 19
20 | Tina Adams    | Bandung | 18
33 | Jack Turner   | Bandung | 21
36 | Liam Edwards  | Jakarta | 16
39 | Noah Stewart  | Surabaya | 18
```

### 7. Memilih Semua Siswa yang Nama Depannya Terdiri dari 5 Huruf dan Huruf Ketiga adalah 'a'
```sql
SELECT * FROM siswa WHERE nama REGEXP '^..a..$';
```
**Jawaban:**
```plaintext
id | nama       | alamat | usia
-------------------------------
12 | Laura Walker | Surabaya | 20
```

### 8. Memilih Semua Siswa yang Nama Depannya Dimulai dengan Huruf 'A' atau 'a' (Case Insensitive)
```sql
SELECT * FROM siswa WHERE nama LIKE 'A%' OR nama LIKE 'a%';
```
**Jawaban:**
```plaintext
id | nama          | alamat  | usia
-----------------------------------
 1 | Aaron Smith   | Jakarta | 16
26 | Amy Moore     | Surabaya | 20
```

### 9. Memilih Semua Siswa yang Berusia Antara 18 dan 20 Tahun
```sql
SELECT * FROM siswa WHERE usia BETWEEN 18 AND 20;
```
**Jawaban:**
```plaintext
id | nama        | alamat  | usia
---------------------------------
 2 | Bella Anderson | Bandung | 18
 5 | Eva Thomas  | Bandung | 17
 6 | Frank Martin | Surabaya | 18
10 | Jane Clark  | Jakarta | 19
16 | Paul King   | Jakarta | 18
18 | Rachel Scott | Surabaya | 19
20 | Tina Adams  | Bandung | 18
27 | Amy Moore   | Surabaya | 20
29 | Fiona Parker | Bandung | 18
32 | George Hughes | Surabaya | 19
```

### 10. Menghitung Jumlah Siswa yang Tinggal di 'Bandung'
```sql
SELECT COUNT(*) AS jumlah_siswa FROM siswa WHERE alamat = 'Bandung';
```
**Jawaban:**
```plaintext
jumlah_siswa
------------
12
```

### 11. Memilih Semua Siswa yang Nama Depannya Dimulai dengan Huruf 'S' atau 'T' dan Tinggal di 'Jakarta'
```sql
SELECT * FROM siswa WHERE (nama LIKE 'S%' OR nama LIKE 'T%') AND alamat = 'Jakarta';
```
**Jawaban:**
```plaintext
id | nama        | alamat | usia
-------------------------------
19 | Sam Green   | Jakarta | 20
```

### 12. Memilih Semua Siswa yang Namanya Terdiri dari Lebih dari 10 Karakter
```sql
SELECT * FROM siswa WHERE LENGTH(nama) > 10;
```
**Jawaban:**
```plaintext
id | nama            | alamat    | usia
---------------------------------------
 3 | Charlie Johnson | Surabaya  | 17
 5 | Eva Thomas      | Bandung   | 17
 6 | Frank Martin    | Surabaya  | 18
10 | Jane Clark      | Jakarta   | 19
12 | Laura Walker    | Surabaya  | 20
18 | Rachel Scott    | Surabaya  | 19
```

### 13. Memilih Semua Siswa yang Nama Depannya Tidak Dimulai dengan Huruf 'A'
```sql
SELECT * FROM siswa WHERE nama NOT LIKE 'A%';
```
**Jawaban:**
```plaintext
id | nama           | alamat   | usia
--------------------------------------
 2 | Bella Anderson | Bandung  | 18
 3 | Charlie Johnson| Surabaya | 17
 4 | David Wilson   | Jakarta  | 19
 5 | Eva Thomas     | Bandung  | 17
```

### 14. Memilih Semua Siswa yang Nama Depannya Dimulai dengan Huruf Vokal (A, E, I, O, U)
```sql
SELECT * FROM siswa WHERE nama REGEXP '^[AEIOUaeiou]';
```
**Jawaban:**
```plaintext
id | nama         | alamat   | usia
------------------------------------
 1 | Aaron Smith  | Jakarta  | 16
 5 | Eva Thomas   | Bandung  | 17
```

### 15. Memilih Semua Siswa yang Usianya 18, 19, atau 20 Tahun
```sql
SELECT * FROM siswa WHERE usia IN (18, 19, 20);
```
**Jawaban:**
```plaintext
id | nama         | alamat   | usia
------------------------------------
 2 | Bella Anderson | Bandung  | 18
 5 | Eva Thomas    | Bandung  | 17
 6 | Frank Martin  | Surabaya | 18
10 | Jane Clark    | Jakarta  | 19
16 | Paul King     | Jakarta  | 18
18 | Rachel Scott  | Surabaya | 19
27 | Amy Moore     | Surabaya | 20
29 | Fiona Parker  | Bandung  | 18
32 | George Hughes | Surabaya | 19
```