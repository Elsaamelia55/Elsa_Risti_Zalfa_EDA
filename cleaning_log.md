# Cleaning Log

> **Template M2 — Exploratory Data Analysis (KMTI21133).**

**Tim: Elsa Amelia (0110223288), Risty (0110223294), Zalfa Khairunnisa (0110223269)**
**Dataset: diabetes_prediction_dataset**
**Baris mentah: 100000** → **baris bersih: 96146**
**Terakhir diperbarui: 23 Oktober 2026**


## Urutan langkah

| # | Langkah | Kolom | Baris terdampak | Keputusan | Alasan |
|---|---|---|---|---|---|
| 1 | Perbaiki tipe data | Semua kolom | 0 | Tidak melakukan perubahan data | Tipe data hasil sudah sesuai untuk analisis yang dilakukan |
| 2 | Buang spasi pada kolom teks | gender, smoking_history | 0 | Tidak mengubah nilai teks | Pemeriksaan tidak menemukan perbedaan jumlah kategori akibat spasi di tepi atau kapitalisasi |
| 3 | Placeholder menjadi kosong | smoking_history | 35.816 | Mempertahankan No info sebagai kategori tersendiri | No Info menunjukkan informasi yang tidak diketahui. Tidak ada dasar yang cukup untuk mengubahnya menjadi NaN. |
| 4 | Seragamkan kategori | gender, smoking_history | 0 | Tidak melakukan perubahan kategori | Tidak ditemukan inkonsistensi kategori berdasarkan pemeriksaan yang dilakukan. |
| 5 | Tangani duplikat | Semua kolom | 3854 | Menghapus baris duplikat persis kemunculan pertama | Penghapusan mengurangi duplikasi persis. Karena tidak ada ID unik, belum dapat dipastikan apakah baris identik berasal dari individu atau pengamatan yang sama. |
| 6 | Tangani nilai mustahil | age, bmi, HbA1c_level, blood_glucose_level, hypertension, heart_disease, diabetes | 0 | Tidak mengubah nilai | Tidak ditemukan pelanggaran aturan rentang dan kode yang digunakan dalam diagnosis. |
| 7 | Tangani nilai kosong | Semua kolom | 0 | Tidak menghapus baris berdasarkan NaN | Dataset tidak memiliki nilai kosong berdasarkan hasil pemeriksaan |
| 8 | Buat kolom turunan | Tidak ada | 0 | Tidak membuat kolom turunan | Pembersihan difokuskan pada pemeriksaan kualitas data dan penghapusan duplikat persis. |


---

## Peta keputusan manual

| Nilai asal | Diubah menjadi | Dasar keputusan |
|smoking_history = No info| Tetap No info | Informasi riwayat merokok tidak diketahui. Nilai dipertahankan sebagai kategori tersendiri karena tidak ada dasar yang cukup untuk menentukan nilai penggantinya. |
| Baris dengan seluruh nilai identik | Satu kemunculan dipertahankan, berikutnya dihapus | Sebanyak 3.854 baris duplikat persis dihapus setelah kemunculan pertama. Namun, tanpa ID unik, tidak dapat dipastikan bahwa setiap baris identik berasal dari individu atau pengamatan yang sama. |
| Nilai pada kolom teks | Tetap seperti semula | Pemeriksaan tidak menemukan perbedaan jumlah kategori akibat spasi di tepi atau kapitalisasi, sehingga normalisasi teks tidak diperlukan. |
| Nilai numerik dan kode biner | Tetap seperti semula | Tidak ditemukan pelanggaran terhadap aturan rentang dan kode yang digunakan dalam pemeriksaan kualitas data. |
| Nan | Tetap seperti semula | Tidak ditemukan NaN, sehingga tidak dilakukan proses apapun |

---

## Yang sengaja TIDAK diubah

| Masalah | Mengapa dibiarkan | Akibat bagi analisis |
| Kategori No Info pada smoking_history sebanyak 35.816 baris sebelum cleaning | Nilai ini menunjukkan informasi riwayat merokok yang tidak diketahui. Tidak ada dasar yang cukup untuk menentukan kategori penggantinya, sehingga nilai dipertahankan. | Analisis berdasarkan riwayat merokok perlu memperhitungkan kategori No Info. Kategori ini tidak boleh langsung ditafsirkan sebagai kelompok dengan kebiasaan merokok tertentu. |
| Kemungkinan baris identik berasal dari individu yang berbeda | Dataset tidak memiliki ID unik untuk memverifikasi identitas setiap pengamatan. Penghapusan duplikat persis dilakukan berdasarkan kesamaan seluruh nilai, bukan identitas individu yang telah terkonfirmasi. | Jumlah baris setelah cleaning berkurang 3.854. Jika beberapa baris identik sebenarnya merupakan pengamatan berbeda yang sah, penghapusan dapat mengurangi representasi data. |
| Nilai numerik yang belum divalidasi berdasarkan sumber atau konteks medis | Pemeriksaan hanya menggunakan aturan rentang yang ditetapkan dalam diagnosis kualitas data. Pemeriksaan ini tidak membuktikan bahwa setiap nilai benar secara medis atau sesuai dengan kondisi individu. | Nilai yang lolos pemeriksaan tetap perlu ditafsirkan secara hati-hati. Hasil analisis tidak dapat dianggap sebagai validasi medis. |
|Tidak ada NaN | Tidak ditemukan NaN sehingga tidak ada imputasi yang dilakukan. Namun, ketiadaan NaN tidak berarti semua informasi dalam dataset lengkap. | Kategori No Info tetap menunjukkan adanya informasi yang tidak diketahui meskipun tidak tercatat sebagai nilai kosong teknis. |


---

## Pertanyaan yang belum terjawab

* [Apakah `No Info` pada `smoking_history` dapat ditentukan nilai sebenarnya? Tidak, karena informasi pengganti tidak tersedia dalam dataset.]
* [Apakah setiap baris duplikat persis pasti berasal dari individu atau pengamatan yang sama? Belum dapat dipastikan karena dataset tidak memiliki ID unik.]
* [Apakah semua nilai numerik yang lolos aturan rentang sudah benar secara faktual atau medis? Belum dapat dipastikan karena validasi hanya menggunakan aturan rentang yang ditetapkan.]
* [Apakah kategori `No Info` termasuk pola data hilang yang bergantung pada karakteristik tertentu? Belum dapat dipastikan hanya dari pemeriksaan ini, diperlukan analisis tambahan untuk menguji pola tersebut.]

---

## Dampak keseluruhan

| Ukuran | Sebelum | Sesudah | Catatan |
|---|---|---|---|
| Jumlah baris | 100000 | 96146 | Berkurang 3.854 baris setelah penghapusan duplikat persis. |
| Jumlah kolom | 9 | 9 | Tidak ada kolom yang ditambah atau dihapus |
| Total sel kosong | 0 | 0 | Tidak ditemukan nilai kosong sebelum atau sesudah cleaning |
| Baris duplikat | 3854 | 0 | Duplikat diperiksa dengan duplicated() dan dihapus dengan drop_duplicates(). |
| Kategori pada kolom gender | 3 | 3 | Tidak ada perubahan |
| Kategori pada kolom smoking_history | 35816 | 32887 | Berkurang karena sebagian baris yang mengandung No Info ikut terhapus sebagai duplikat persis; nilai No Info tidak diubah secara langsung. |
| Proposi diabetes = 0 | 91.50% | 91.18% | Proporsi dihitung ulang setelah penghapusan duplikat. |
| Proposi diabetes = 1 | 8.50% | 8.82% | Proporsi meningkat 0,32 poin persentase setelah penghapusan duplikat. |

## Reproduktibilitas

- [ ] Notebook berjalan dari awal sampai akhir setelah *Restart and run all*
- [ ] Pembersihan dibungkus dalam satu fungsi, bukan tersebar di banyak sel
- [ ] `data/raw/` tidak berubah dan tidak ikut diunggah
- [ ] Hasil tersimpan di `data/processed/`
- [ ] `python cek_m2.py` berjalan tanpa status GAGAL
