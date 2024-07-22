---
title: "Skrip SQL untuk Tabel Perpustakaan"
date: 2024-07-21 10:00:00 +0800
categories: [SQL, Database]
tags: [SQL, database, tutorial]
---

Berikut adalah skrip lengkap untuk membuat tabel `buku`, `user_peminjam`, dan `peminjaman`, serta mengisi tabel `buku` dan `user_peminjam` dengan data awal, dan tabel `peminjaman` dengan data acak yang mencerminkan aktivitas peminjaman di sebuah perpustakaan.

```sql
-- Membuat tabel buku
CREATE TABLE buku (
    id_buku INT PRIMARY KEY AUTO_INCREMENT,
    judul_buku VARCHAR(100),
    pengarang VARCHAR(50),
    tahun_terbit INT
);

-- Menambahkan data awal ke dalam tabel buku
INSERT INTO buku (judul_buku, pengarang, tahun_terbit) VALUES
('Pemrograman Java', 'Budi', 2015),
('Pemrograman Python', 'Sinta', 2018),
('Database MySQL', 'Asep', 2017),
('Algoritma dan Struktur Data', 'Cici', 2016),
('Jaringan Komputer', 'Dedi', 2019),
('Pemrograman C++', 'Eko', 2014),
('Pemrograman Ruby', 'Farid', 2020),
('Pemrograman PHP', 'Gita', 2016),
('Pemrograman Swift', 'Hani', 2021),
('Pemrograman Kotlin', 'Ida', 2019),
('Pemrograman Go', 'Joko', 2018),
('Pemrograman R', 'Kiki', 2017),
('Pemrograman MATLAB', 'Lina', 2015),
('Pemrograman Scala', 'Maya', 2020),
('Pemrograman Rust', 'Nina', 2021),
('Pemrograman Haskell', 'Oni', 2018),
('Pemrograman Perl', 'Putu', 2017),
('Pemrograman Lua', 'Qori', 2016),
('Pemrograman Elixir', 'Rina', 2019),
('Pemrograman Dart', 'Sari', 2020),
('Pemrograman TypeScript', 'Toni', 2021),
('Pemrograman Objective-C', 'Umi', 2018),
('Pemrograman Shell', 'Vera', 2017),
('Pemrograman AWK', 'Wati', 2016),
('Pemrograman SQL', 'Xena', 2015);

-- Membuat tabel user_peminjam
CREATE TABLE user_peminjam (
    id_user INT PRIMARY KEY AUTO_INCREMENT,
    nama_user VARCHAR(100),
    alamat_user VARCHAR(150)
);

-- Menambahkan data awal ke dalam tabel user_peminjam
INSERT INTO user_peminjam (nama_user, alamat_user) VALUES
('Ali', 'Jl. Merpati No. 1'),
('Budi', 'Jl. Kenari No. 2'),
('Cici', 'Jl. Kakatua No. 3'),
('Dedi', 'Jl. Elang No. 4'),
('Eka', 'Jl. Rajawali No. 5'),
('Fani', 'Jl. Nuri No. 6'),
('Gita', 'Jl. Cendrawasih No. 7'),
('Hana', 'Jl. Kenari No. 8'),
('Iwan', 'Jl. Jalak No. 9'),
('Joko', 'Jl. Kutilang No. 10');

-- Membuat tabel peminjaman
CREATE TABLE peminjaman (
    id_peminjaman INT PRIMARY KEY AUTO_INCREMENT,
    id_user INT,
    id_buku INT,
    tanggal_pinjam DATE,
    tanggal_kembali DATE,
    FOREIGN KEY (id_user) REFERENCES user_peminjam(id_user),
    FOREIGN KEY (id_buku) REFERENCES buku(id_buku)
);

-- Menghapus semua data di tabel peminjaman (jika ada)
DELETE FROM peminjaman;

-- Mengatur ulang nilai AUTO_INCREMENT
ALTER TABLE peminjaman AUTO_INCREMENT = 1;

-- Menambahkan 20 data acak baru ke dalam tabel peminjaman
INSERT INTO peminjaman (id_user, id_buku, tanggal_pinjam, tanggal_kembali) VALUES
(1, 1, '2023-07-01', '2023-07-10'),
(2, 2, '2023-07-01', NULL),
(3, 3, '2023-07-03', '2023-07-12'),
(4, 4, '2023-07-04', NULL),
(5, 5, '2023-07-05', '2023-07-14'),
(6, 6, '2023-07-06', '2023-07-15'),
(7, 7, '2023-07-07', NULL),
(8, 8, '2023-07-08', '2023-07-18'),
(9, 9, '2023-07-09', NULL),
(10, 10, '2023-07-10', '2023-07-20'),
(1, 11, '2023-07-11', '2023-07-21'),
(2, 12, '2023-07-12', NULL),
(3, 13', '2023-07-13', '2023-07-23'),
(4, 14, '2023-07-14', NULL),
(5, 15, '2023-07-15', '2023-07-25'),
(6, 16, '2023-07-15', '2023-07-26'),
(7, 17, '2023-07-16', NULL),
(8, 18, '2023-07-17', '2023-07-28'),
(9, 19, '2023-07-17', NULL),
(10, 20, '2023-07-18', '2023-07-30'),
(1, 21, '2023-07-19', '2023-08-01'),
(2, 22, '2023-07-20', NULL),
(3, 23, '2023-07-20', '2023-08-03'),
(4, 24, '2023-07-21', '2023-08-04'),
(5, 25, '2023-07-21', NULL),
(6, 6, '2023-07-22', '2023-08-05'),
(7, 7, '2023-07-23', '2023-08-06'),
(8, 8, '2023-07-24', NULL),
(9, 9, '2023-07-25', '2023-08-08'),
(10, 10, '2023-07-25', NULL),
(1, 11, '2023-07-26', '2023-08-10'),
(2, 12, '2023-07-27', NULL),
(3, 13, '2023-07-27', '2023-08-12'),
(4, 14, '2023-07-28', NULL),
(5, 15, '2023-07-29', '2023-08-14'),
(6, 16, '2023-07-30', NULL),
(7, 17, '2023-07-31', '2023-08-16'),
(8, 18, '2023-07-31', NULL),
(9, 19, '2023-08-01', '2023-08-18'),
(10, 20, '2023-08-01', NULL),
(1, 21, '2023-08-02', '2023-08-20'),
(2, 22, '2023-08-03', NULL),
(3, 23, '2023-08-03', '2023-08-22'),
(4, 24, '2023-08-04', NULL),
(5, 25, '2023-08-05', '2023-08-24');
```
