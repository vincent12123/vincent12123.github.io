---
title: "Modul Variabel Python"
date: 2024-07-24
categories: [Variabel, Python]
tags: [variabel, Python]
pin: true
toc: true
---

### Modul Penggunaan Variabel dan Input dalam Program Pemasukan dan Pengeluaran dengan Python

#### Tujuan Pembelajaran:
Peserta didik mampu memahami dan menggunakan variabel dan input dalam Python untuk membuat program sederhana yang mengelola pemasukan dan pengeluaran.

#### Kompetensi yang Akan Dicapai:
1. Peserta didik dapat menjelaskan apa itu variabel dalam Python.
2. Peserta didik dapat mendeklarasikan dan menginisialisasi variabel dalam Python.
3. Peserta didik dapat menggunakan fungsi input untuk menerima data dari pengguna.
4. Peserta didik dapat membuat program sederhana yang menggunakan variabel untuk menyimpan dan memproses data pemasukan dan pengeluaran.

---

### Pengertian Variabel
Variabel adalah tempat penyimpanan sementara untuk menyimpan nilai atau data yang dapat berubah-ubah selama program berjalan. Dalam Python, variabel dapat menyimpan berbagai tipe data seperti angka, teks, dan lainnya.

### Mendeklarasikan dan Menginisialisasi Variabel
Untuk mendeklarasikan dan menginisialisasi variabel dalam Python, cukup menggunakan nama variabel diikuti dengan tanda sama dengan (`=`) dan nilai yang ingin disimpan.

Contoh:
```python
pemasukan = 1000000
pengeluaran = 500000
saldo = pemasukan - pengeluaran
```

### Menggunakan Fungsi Input
Fungsi `input()` digunakan untuk menerima masukan dari pengguna. Nilai yang dimasukkan oleh pengguna akan disimpan dalam variabel.

Contoh:
```python
nama = input("Masukkan nama Anda: ")
umur = int(input("Masukkan umur Anda: "))
print("Nama Anda adalah", nama, "dan umur Anda", umur)
```

---

### Contoh Program Pemasukan dan Pengeluaran

<video width="640" height="480" controls>
  <source src="https://tracerstudy.stbapontianak.ac.id/x.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>


Berikut adalah contoh program lengkap untuk mencatat pengeluaran dan pemasukan, serta menyimpan data dalam bentuk daftar untuk melacak setiap transaksi:

```python
# Program untuk mencatat pengeluaran dan pemasukan

# Inisialisasi variabel
pemasukan = 0
pengeluaran = 0
daftar_pemasukan = []
daftar_pengeluaran = []

def tampilkan_menu():
    print("\nMenu:")
    print("1. Tambahkan Pemasukan")
    print("2. Tambahkan Pengeluaran")
    print("3. Tampilkan Total Pemasukan dan Pengeluaran")
    print("4. Tampilkan Detail Pemasukan dan Pengeluaran")
    print("5. Keluar")

def tambah_pemasukan():
    global pemasukan
    try:
        jumlah = float(input("Masukkan jumlah pemasukan: Rp "))
        keterangan = input("Masukkan keterangan pemasukan: ")
        pemasukan += jumlah
        daftar_pemasukan.append({"jumlah": jumlah, "keterangan": keterangan})
        print(f"Pemasukan sebesar Rp {jumlah} berhasil ditambahkan.")
    except ValueError:
        print("Masukkan jumlah dalam angka!")

def tambah_pengeluaran():
    global pengeluaran
    try:
        jumlah = float(input("Masukkan jumlah pengeluaran: Rp "))
        keterangan = input("Masukkan keterangan pengeluaran: ")
        pengeluaran += jumlah
        daftar_pengeluaran.append({"jumlah": jumlah, "keterangan": keterangan})
        print(f"Pengeluaran sebesar Rp {jumlah} berhasil ditambahkan.")
    except ValueError:
        print("Masukkan jumlah dalam angka!")

def tampilkan_total():
    print(f"\nTotal Pemasukan: Rp {pemasukan}")
    print(f"Total Pengeluaran: Rp {pengeluaran}")
    print(f"Saldo: Rp {pemasukan - pengeluaran}")

def tampilkan_detail():
    print("\nDetail Pemasukan:")
    if daftar_pemasukan:
        for idx, item in enumerate(daftar_pemasukan, start=1):
            print(f"{idx}. Jumlah: Rp {item['jumlah']}, Keterangan: {item['keterangan']}")
    else:
        print("Tidak ada pemasukan yang tercatat.")
    
    print("\nDetail Pengeluaran:")
    if daftar_pengeluaran:
        for idx, item in enumerate(daftar_pengeluaran, start=1):
            print(f"{idx}. Jumlah: Rp {item['jumlah']}, Keterangan: {item['keterangan']}")
    else:
        print("Tidak ada pengeluaran yang tercatat.")

def main():
    while True:
        tampilkan_menu()
        pilihan = input("Pilih menu (1-5): ")
        
        if pilihan == "1":
            tambah_pemasukan()
        elif pilihan == "2":
            tambah_pengeluaran()
        elif pilihan == "3":
            tampilkan_total()
        elif pilihan == "4":
            tampilkan_detail()
        elif pilihan == "5":
            print("Terima kasih telah menggunakan program ini.")
            break
        else:
            print("Pilihan tidak valid, silakan coba lagi.")

if __name__ == "__main__":
    main()
```

#### Penjelasan Program
1. **Inisialisasi Variabel**:
   - `pemasukan` dan `pengeluaran` diset ke 0.
   - `daftar_pemasukan` dan `daftar_pengeluaran` adalah daftar untuk menyimpan transaksi pemasukan dan pengeluaran.

2. **Fungsi tampilkan_menu**:
   - Menampilkan menu utama program.

3. **Fungsi tambah_pemasukan**:
   - Menerima input dari pengguna untuk jumlah dan keterangan pemasukan.
   - Menambahkan pemasukan ke variabel `pemasukan` dan menyimpannya di `daftar_pemasukan`.

4. **Fungsi tambah_pengeluaran**:
   - Menerima input dari pengguna untuk jumlah dan keterangan pengeluaran.
   - Menambahkan pengeluaran ke variabel `pengeluaran` dan menyimpannya di `daftar_pengeluaran`.

5. **Fungsi tampilkan_total**:
   - Menampilkan total pemasukan, pengeluaran, dan saldo saat ini.

6. **Fungsi tampilkan_detail**:
   - Menampilkan detail transaksi pemasukan dan pengeluaran yang tercatat.

7. **Fungsi main**:
   - Menjalankan program utama dan menampilkan menu sampai pengguna memilih untuk keluar.
