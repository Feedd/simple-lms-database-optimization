# simple-lms-database-optimization
Simple LMS - Database Optimization Module 5

## Database Query Optimization

### 1. select_related()

Digunakan untuk mengambil data Course beserta Lecturer melalui JOIN
sehingga mengurangi query berulang (N+1 query problem).

Contoh:

```python
Course.objects.select_related("lecturer")

2. prefetch_related()
Digunakan untuk mengambil data Course beserta Lesson dengan query
terpisah yang efisien, lalu menghubungkan hasilnya di Django.

Course.objects.prefetch_related("lessons")

3. Database Index
Index ditambahkan pada field is_active dan title di model Course
untuk membantu pencarian berdasarkan kedua field tersebut.

4. Benchmark
- Course/Lecturer: 101 query menjadi 1 query.
- Course/Lesson: 51 query menjadi 2 query.
Waktu eksekusi dapat berubah bergantung pada kondisi database dan lingkungan pengujian.
