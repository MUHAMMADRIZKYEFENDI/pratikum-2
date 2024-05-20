# pratikum-3
![Screenshot (57)](https://github.com/MUHAMMADRIZKYEFENDI/pratikum-2/assets/168548623/3136d11c-1e55-4891-81e9-df093de2eada)
1.Buat DDL Script berdasarkan skema ERD tersebut diatas.

2.Jalankan script DDL tersebut pada DBMS MYSQL.

3.Implementasikan penggunaan CONSTRAINT FOREIGN KEY pada semua tabel yang berelasi.

4.yang perlu diperhatikan:

•tipe data pada field yang berelasi harus sama termasuk juga ukuran datanya.

• misal: pada tabel dosen, kd_ds VARCHAR(10) maka pada tabel yang merujuk yaitu tabel mahasiswa, kd_ds juga harus bertipe VARCHAR(10
# TABLE MAHASISWA
USE kampus;
Database changed
MariaDB [kampus]>
MariaDB [kampus]> -- Table Mahasiswa
MariaDB [kampus]> CREATE TABLE Mahasiswa (
    ->     nim VARCHAR(10) PRIMARY KEY,
    ->     nama VARCHAR(50),
    ->     jenis_kelamin CHAR(1),
    ->     tgl_lahir DATE,
    ->     jalan VARCHAR(100),
    ->     kota VARCHAR(50),
    ->     kodepos VARCHAR(10),
    ->     no_hp VARCHAR(15),
    ->     kd_ds VARCHAR(10)
    -> );
Query OK, 0 rows affected (0.007 sec)
![1](https://github.com/MUHAMMADRIZKYEFENDI/pratikum-2/assets/168548623/be1a9b7e-03a8-438d-8631-3b92b637d184)

# TABLE DOSEN
MariaDB [kampus]>
MariaDB [kampus]> -- Table Dosen
MariaDB [kampus]> CREATE TABLE Dosen (
    ->     kd_ds VARCHAR(10) PRIMARY KEY,
    ->     nama VARCHAR(50)
    -> );
Query OK, 0 rows affected (0.006 sec)

![2](https://github.com/MUHAMMADRIZKYEFENDI/pratikum-2/assets/168548623/dbdbd37a-d574-4a8e-9c60-8f26eaac7b76)

# MATAKULIAH
MariaDB [kampus]>
MariaDB [kampus]> -- Table Matakuliah
MariaDB [kampus]> CREATE TABLE Matakuliah (
    ->     kd_mk VARCHAR(10) PRIMARY KEY,
    ->     nama VARCHAR(50),
    ->     sks INT
    -> );
Query OK, 0 rows affected (0.006 sec)

![3](https://github.com/MUHAMMADRIZKYEFENDI/pratikum-2/assets/168548623/d69bd0cb-900b-4516-9d19-8506222188c2)

# JADWAL MENGAJAR
MariaDB [kampus]>
MariaDB [kampus]> -- Table JadwalMengajar
MariaDB [kampus]> CREATE TABLE JadwalMengajar (
    ->     kd_ds VARCHAR(10),
    ->     kd_mk VARCHAR(10),
    ->     hari VARCHAR(10),
    ->     jam TIME,
    ->     ruang VARCHAR(10),
    ->     PRIMARY KEY (kd_ds, kd_mk),
    ->     FOREIGN KEY (kd_ds) REFERENCES Dosen(kd_ds),
    ->     FOREIGN KEY (kd_mk) REFERENCES Matakuliah(kd_mk)
    -> );
Query OK, 0 rows affected (0.015 sec)

![4](https://github.com/MUHAMMADRIZKYEFENDI/pratikum-2/assets/168548623/b9e7a7fd-ae97-4f0f-b2c1-88459d0c9ec8)

# KRS MAHASISWA

MariaDB [kampus]>
MariaDB [kampus]> -- Table KRSMahasiswa
MariaDB [kampus]> CREATE TABLE KRSMahasiswa (
    ->     nim VARCHAR(10),
    ->     kd_mk VARCHAR(10),
    ->     kd_ds VARCHAR(10),
    ->     semester INT,
    ->     nilai CHAR(1),
    ->     PRIMARY KEY (nim, kd_mk, kd_ds),
    ->     FOREIGN KEY (nim) REFERENCES Mahasiswa(nim),
    ->     FOREIGN KEY (kd_mk) REFERENCES Matakuliah(kd_mk),
    ->     FOREIGN KEY (kd_ds) REFERENCES Dosen(kd_ds)
    -> );
Query OK, 0 rows affected (0.015 sec)

#
MariaDB [kampus]>
MariaDB [kampus]> -- Adding Foreign Key to Mahasiswa referencing Dosen
MariaDB [kampus]> ALTER TABLE Mahasiswa
    -> ADD CONSTRAINT fk_mahasiswa_dosen
    -> FOREIGN KEY (kd_ds) REFERENCES Dosen(kd_ds);
Query OK, 0 rows affected (0.026 sec)
Records: 0  Duplicates: 0  Warnings: 0
![Screenshot (56)](https://github.com/MUHAMMADRIZKYEFENDI/pratikum-2/assets/168548623/fdc30ee1-2158-452d-bb53-d236d5ad8276)

# 1. Lakukan penambahan data pada table mahasiswa dengan mengisi kd_ds yang belum ada pada data dosen.
MariaDB [Universitas]> -- Tambahkan data ke tabel Dosen
MariaDB [Universitas]> INSERT INTO Dosen (kd_ds, nama)
    -> VALUES
    ->     ('DS001', 'Dr. Ahmad Yani'),
    ->     ('DS002', 'Prof. Budi Santoso');
Query OK, 2 rows affected (0.007 sec)
Records: 2  Duplicates: 0  Warnings: 0

MariaDB [Universitas]>
MariaDB [Universitas]> -- Tambahkan data ke tabel Mahasiswa dengan kd_ds yang valid
MariaDB [Universitas]> INSERT INTO Mahasiswa (nim, nama, jenis_kelamin, tgl_lahir, jalan, kota, kodepos, no_hp, kd_ds)
    -> VALUES
    ->     ('2023456789', 'John Doe', 'M', '2000-01-01', 'Jl. Kebon Jeruk No. 1', 'Jakarta', '12345', '08123456789', 'DS001'),
    ->     ('2023456790', 'Jane Smith', 'F', '1999-12-12', 'Jl. Mangga Dua No. 5', 'Bandung', '67890', '08234567890', 'DS002');
Query OK, 2 rows affected (0.001 sec)
Records: 2  Duplicates: 0  Warnings: 0

![Screenshot (62)](https://github.com/MUHAMMADRIZKYEFENDI/pratikum-2/assets/168548623/bfd6b224-9008-45d6-8151-8543a1e610e6)

# 2. Hapus satu record data pada table dosen yang telah dirujuk pada tabel mahasiswa.
-- Delete a record from the Dosen table that is not referenced by any record in the Mahasiswa table
DELETE FROM Dosen
WHERE kd_ds = 'DS001'
AND NOT EXISTS (
    SELECT 1 FROM Mahasiswa WHERE kd_ds = 'DS001'
);
![Screenshot (64)](https://github.com/MUHAMMADRIZKYEFENDI/pratikum-2/assets/168548623/30d44a17-994a-41bf-a6ac-3bd0c7f1212e)

# 3. Ubah mode menjadi ON UPDATE CASCADE ON DELETE RESTRICT
![Screenshot (65)](https://github.com/MUHAMMADRIZKYEFENDI/pratikum-2/assets/168548623/7f77078b-8871-47d5-ac8d-d681aaf7873b)
# 4. Lakukan perubahan data pada table dosen (kd_ds)
![1](https://github.com/MUHAMMADRIZKYEFENDI/pratikum-2/assets/168548623/ab52c99c-5da1-44f5-838b-35a3f40067f6)

# 5. Lakukan penghapusan data pada table dosen

![2](https://github.com/MUHAMMADRIZKYEFENDI/pratikum-2/assets/168548623/98221b06-6232-45eb-8eae-fd95b79bfbb0)

# 6. Ubah mode menjadi ON UPDATE CASCADE ON DELETE SET NULL

![Screenshot (68)](https://github.com/MUHAMMADRIZKYEFENDI/pratikum-2/assets/168548623/24e5fe28-c8fd-4887-b21a-6bc5bb3743f3)

# 7. Lakukan penghapusan data pada table dosen
![Screenshot (69)](https://github.com/MUHAMMADRIZKYEFENDI/pratikum-2/assets/168548623/c8703061-f238-4668-93ec-a69cd2196136)

# Tulis semua perintah-perintah SQL percobaan di atas beserta outputnya!

# Apa bedanya penggunaan RESTRICT dan penggunaan CASCADE
Bedanya Penggunaan RESTRICT dan CASCADE:
RESTRICT: Ketika Anda menggunakan ON DELETE RESTRICT dalam definisi kunci asing, itu berarti bahwa tidak akan memungkinkan untuk menghapus baris induk dari tabel yang berhubungan jika ada baris anak yang bergantung padanya. Dalam kasus ini, DELETE akan gagal dan melemparkan pesan kesalahan.

CASCADE: Ketika Anda menggunakan ON DELETE CASCADE, itu berarti ketika baris induk dihapus, semua baris anak yang bergantung padanya juga akan dihapus secara otomatis. Dalam hal ini, DELETE akan merambat ke baris anak dan menghapusnya juga.

Jadi, perbedaan utama antara RESTRICT dan CASCADE adalah bagaimana perilaku penghapusan diproses ketika ada baris anak yang bergantung pada baris induk yang akan dihapus.

# Berikan kesimpulan anda!
### Kesimpulan:

1. **Pembuatan Tabel**: Dalam praktikum ini, kami telah membuat beberapa tabel dalam database, termasuk tabel untuk Dosen, Mahasiswa, Matakuliah, Jadwal Mengajar, dan KRS Mahasiswa. Setiap tabel memiliki struktur yang ditentukan, termasuk kunci utama dan kunci asing yang didefinisikan untuk menjaga integritas data.

2. **Penambahan Data**: Kami telah menambahkan data ke dalam tabel Dosen dan Mahasiswa menggunakan perintah INSERT. Ini memungkinkan kami untuk memasukkan informasi tentang dosen dan mahasiswa ke dalam database.

3. **Penghapusan Data**: Kami juga telah melakukan percobaan dengan menghapus data dari tabel Dosen. Namun, kita menemui masalah dengan kunci asing yang menyebabkan kesalahan. Ini menunjukkan pentingnya memahami konstrain kunci asing dan bagaimana cara mengelolanya dengan benar.

4. **Perubahan Kunci Asing**: Kami telah mencoba mengubah mode kunci asing di tabel Mahasiswa untuk menggunakan `ON UPDATE CASCADE` dan `ON DELETE SET NULL`. Namun, kami mengalami kesalahan dalam percobaan ini, yang menunjukkan kompleksitas dalam mengelola kunci asing.

5. **Perbedaan Antara RESTRICT dan CASCADE**: Kami juga mempelajari perbedaan antara `RESTRICT` dan `CASCADE` dalam konteks konstrain kunci asing. Ini adalah konsep penting dalam desain database yang mempengaruhi bagaimana data dihapus atau diubah.

Secara keseluruhan, praktikum ini memberikan pemahaman yang lebih baik tentang bagaimana mendesain, membuat, dan mengelola struktur database, serta pentingnya memahami konsep konstrain kunci asing. Hal ini dapat membantu dalam pengembangan aplikasi yang membutuhkan manipulasi data yang aman dan konsisten.

# Buat laporan praktikum yang berisi, langkah-langkah praktikum beserta screenshot yang sudah dilakukan dalam bentuk dokumen.
