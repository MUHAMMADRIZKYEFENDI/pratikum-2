# pratikum-2
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
