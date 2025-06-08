# PRAKTIKUM 4-6

![CodeIgniter Badge](https://img.shields.io/badge/CodeIgniter-%23EF4223.svg?style=for-the-badge&logo=codeIgniter&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-%23777BB4.svg?style=for-the-badge&logo=php&logoColor=white)

## Profil
| Nama | Kelas | NIM | Mata Kuliah | Dosen |
|------|-------|-----|-------------|-------|
| LUTPIAH AINUS SHIDDIK    |  TI.23.A.5     | 312310474    |    Pemograman web 2         |  Agung Nugroho, S.Kom., M.Kom.     |

# Praktikum 4: Framework Lanjutan (Modul Login)

# Langkah-langkah Praktikum
Persiapan.
Untuk memulai membuat modul Login, yang perlu disiapkan adalah database server menggunakan MySQL. Pastikan MySQL Server sudah dapat dijalankan melalui XAMPP.

# 1. Persiapkan Database
Buat tabel user pada database dengan SQL berikut:

```
CREATE TABLE user (
  id INT(11) auto_increment,
  username VARCHAR(200) NOT NULL,
  useremail VARCHAR(200),
  userpassword VARCHAR(200),
  PRIMARY KEY(id)
);
```
