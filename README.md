# Simple LMS - Database Optimization

Repository ini merupakan proyek Simple LMS untuk praktikum Database Optimization Module 5.

## 1. Menjalankan Project

Pastikan Docker dan Docker Compose sudah terpasang.

Clone repository:

```bash
git clone https://github.com/Feedd/simple-lms-database-optimization.git
cd simple-lms-database-optimization
```

Jalankan container:

```bash
docker compose up -d
```

Periksa status container:

```bash
docker compose ps
```

Jalankan migrasi database:

```bash
docker compose exec app python manage.py migrate
```

Akses aplikasi melalui:

```text
http://localhost:8000
```

Untuk membuka Django Shell:

```bash
docker compose exec app python manage.py shell
```

## 2. Database Query Optimization

### A. select_related()

Digunakan untuk mengambil data Course beserta Lecturer melalui JOIN sehingga mengurangi query berulang (N+1 query problem).

Contoh:

```python
Course.objects.select_related("lecturer")
```

### B. prefetch_related()

Digunakan untuk mengambil data Course beserta Lesson dengan query terpisah yang efisien, lalu menghubungkan hasilnya di Django.

Contoh:

```python
Course.objects.prefetch_related("lessons")
```

### C. Database Index

Index ditambahkan pada field `is_active` dan `title` di model Course untuk membantu pencarian berdasarkan kedua field tersebut.

## 3. Benchmark Hasil Pengujian

| Skenario | Jumlah Data | Query Sebelum | Query Sesudah |
|---|---:|---:|---:|
| Course + Lecturer | 100 | 101 | 1 |
| Course + Lesson | 50 | 51 | 2 |

### Waktu Eksekusi

| Skenario | Sebelum | Sesudah |
|---|---:|---:|
| Course + Lecturer | 0,0983 detik | 0,0080 detik |
| Course + Lesson | 0,0743 detik | 0,0118 detik |

Waktu eksekusi dapat berubah bergantung pada kondisi database dan lingkungan pengujian.

## 4. Kesimpulan

Penggunaan `select_related()` dan `prefetch_related()` dapat mengurangi jumlah query database. Penambahan index juga membantu proses pencarian data menjadi lebih efisien.

Optimasi ini membantu meningkatkan performa aplikasi Simple LMS.
