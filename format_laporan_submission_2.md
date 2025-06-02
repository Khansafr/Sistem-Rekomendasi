# Laporan Proyek Machine Learning - Khansa Fakhirah Rifli

## Project Overview

Dalam era digital yang sangat kompetitif saat ini, industri smartphone berkembang pesat. Setiap tahun, berbagai merek meluncurkan puluhan model baru dengan fitur dan harga yang beragam, sehingga konsumen sering kali kesulitan menentukan pilihan smartphone yang paling sesuai dengan kebutuhan dan preferensinya.

Beberapa penelitian telah menunjukkan bahwa Collaborative Filtering merupakan pendekatan efektif dalam sistem rekomendasi, khususnya di domain hiburan seperti film dan anime (Herlocker et al., 2004; Resnick et al., 1994). Selain itu, Neural Collaborative Filtering terbukti unggul dalam menangkap interaksi non-linear antara pengguna dan item (Zhang et al., 2019).

Dengan memanfaatkan teknik tersebut, sistem rekomendasi dapat memberikan rekomendasi smartphone yang dipersonalisasi berdasarkan riwayat rating pengguna lain yang memiliki preferensi serupa.

---

## Business Understanding

### Problem Statements

* Bagaimana membuat sistem rekomendasi smartphone yang dapat memberikan rekomendasi personalisasi berdasarkan pola interaksi pengguna?
* Bagaimana memprediksi produk yang berpotensi diminati oleh pengguna berdasarkan data interaksi sebelumnya?

### Goals

* Membangun sistem rekomendasi yang mampu merekomendasikan Top-N smartphone untuk tiap pengguna secara personalisasi.
* Menghasilkan prediksi produk yang relevan berdasarkan pola interaksi pengguna.

### Solution Approach

* **Model Development dengan Content-Based Filtering:**
  Menerapkan metode yang merekomendasikan produk berdasarkan kemiripan fitur produk yang pernah diinteraksikan pengguna.

* **Model Development dengan Collaborative Filtering:**
  Menggunakan pola interaksi antar pengguna dan produk untuk memprediksi preferensi pengguna dan memberikan rekomendasi yang sesuai.

---

## Data Understanding

Dataset yang digunakan berasal dari Kaggle dengan nama *Cellphones Recommendations* ([https://www.kaggle.com/datasets/meirnizri/cellphones-recommendations](https://www.kaggle.com/datasets/meirnizri/cellphones-recommendations)). Dataset ini terdiri dari tiga file utama, yaitu data handphone, data rating pengguna, dan data profil pengguna.

### Deskripsi Dataset

Dataset yang digunakan dalam proyek ini terdiri dari tiga file utama yang saling berhubungan, yaitu:

* **cellphones data.csv**
  Dataset ini berisi informasi detail tentang berbagai produk ponsel, seperti nama model, merek, spesifikasi teknis, dan fitur lain yang menggambarkan karakteristik produk. Terdapat 33 data produk dengan 14 fitur yang terdiri dari 8 kolom numerik bertipe integer, 2 kolom numerik bertipe float, dan 4 kolom kategorikal. Tidak terdapat data yang hilang pada file ini.

* **cellphones ratings.csv**
  Dataset ini memuat data penilaian atau rating yang diberikan oleh pengguna terhadap produk ponsel. Dataset berisi 990 entri dengan tiga kolom numerik, yaitu ID pengguna, ID ponsel yang diberi rating, dan skor rating itu sendiri. Skala rating berkisar antara 1 hingga 9, mencerminkan tingkat kepuasan pengguna terhadap produk.

* **cellphones users.csv**
  Berisi informasi profil pengguna yang memberikan rating, seperti ID pengguna, usia, jenis kelamin, dan pekerjaan. Dataset ini terdiri dari 99 entri dengan 4 kolom, dua kolom numerik dan dua kolom kategorikal. Terdapat satu nilai yang hilang pada kolom pekerjaan (occupation).

Ketiga dataset ini diintegrasikan untuk analisis preferensi pengguna dan pengembangan sistem rekomendasi ponsel berdasarkan karakteristik produk dan perilaku pengguna. Dataset ini diperoleh dari sumber internal dan dapat diakses melalui direktori `/content/cellphones-recommendations/`.

### Deskripsi Variabel

Berikut adalah ringkasan variabel yang terdapat pada masing-masing dataset:

#### Dataset Handphone (data)

| Variabel         | Tipe Data | Deskripsi                            |
| ---------------- | --------- | ------------------------------------ |
| cellphone\_id    | int       | ID unik untuk tiap produk ponsel     |
| brand            | object    | Merek ponsel (misal: Apple, Samsung) |
| model            | object    | Nama model ponsel                    |
| operating system | object    | Sistem operasi (misal: iOS, Android) |
| internal memory  | int       | Kapasitas penyimpanan internal (GB)  |
| RAM              | int       | Kapasitas RAM (GB)                   |
| performance      | float     | Skor performa berdasarkan benchmark  |
| main camera      | int       | Resolusi kamera utama (MP)           |
| selfie camera    | int       | Resolusi kamera depan (MP)           |
| battery size     | int       | Kapasitas baterai (mAh)              |
| screen size      | float     | Ukuran layar (inci)                  |
| weight           | int       | Berat ponsel (gram)                  |
| price            | int       | Harga produk (mata uang lokal)       |
| release date     | object    | Tanggal rilis produk                 |

#### Dataset Rating Pengguna (ratings)

| Variabel      | Tipe Data | Deskripsi                        |
| ------------- | --------- | -------------------------------  |
| user\_id      | int       | ID unik pengguna                 |
| cellphone\_id | int       | ID ponsel yang diberikan rating  |
| rating        | int       | Skor rating dari pengguna (1-18) |

#### Dataset Profil Pengguna (users)

| Variabel   | Tipe Data | Deskripsi                     |
| ---------- | --------- | ----------------------------- |
| user\_id   | int       | ID unik pengguna              |
| age        | int       | Usia pengguna                 |
| gender     | object    | Jenis kelamin                 |
| occupation | object    | Pekerjaan pengguna (opsional) |

### Exploratory Data Analysis (EDA)

#### Eksplorasi Dataset Handphone (data)
![Distribusi Brand Handphone](https://github.com/user-attachments/assets/e46fda20-6689-4503-9500-fb58d0d879bc)

**Insight:**
* Samsung memiliki jumlah produk terbanyak, yaitu 8 unit, menunjukkan bahwa Samsung cukup dominan dalam koleksi ponsel yang ada di dataset.
* Apple berada di posisi kedua dengan 6 produk, menandakan merek ini juga cukup banyak representasinya.
* Merek seperti Motorola, OnePlus, dan Xiaomi masing-masing memiliki 4 produk, yang menunjukkan variasi produk yang cukup baik dari beberapa brand populer lainnya.
* Merek seperti Google (3 produk) dan beberapa brand lain seperti Asus, Oppo, Vivo, dan Sony hanya memiliki 1 produk masing-masing, menunjukkan kehadiran yang lebih kecil dalam dataset.

![Distribusi Sistem Operasi](https://github.com/user-attachments/assets/05293211-4124-423d-a5c4-176d16fd0ccc)
**Insight:**
* Sebagian besar ponsel dalam dataset menggunakan Android, yaitu sebanyak 27 unit.
* Sedangkan ponsel dengan iOS hanya berjumlah 6 unit

![Distribusi Harga Handphone](https://github.com/user-attachments/assets/80768f36-ba29-42f8-af60-7942d51017db)
* Harga ponsel paling sering muncul adalah 699 (sebanyak 3 unit), diikuti oleh 899 (2 unit).
* Sisanya, harga-harga lain muncul masing-masing 1 kali, menunjukkan variasi harga yang cukup luas dan tidak terlalu banyak produk dengan harga sama.
* Harga termurah adalah 129, dan harga tertinggi mencapai 1998.
* Harga-harga yang sering muncul berada di kisaran menengah hingga atas, yaitu antara 600 sampai 900.
* Ada harga-harga yang cukup unik, misalnya 236, 154, 528, dan 780, yang kemungkinan menunjukkan variasi model dan segmen pasar yang berbeda.

**Eksplorasi Dataset Rating (ratings)**

![Distribusi Rating oleh Pengguna](https://github.com/user-attachments/assets/d7336394-63e7-49ef-b056-9a688d8605d7)
**Insight:**
* Rating paling banyak diberikan pada nilai 8 (195 kali), diikuti 7 (169 kali) dan 10 (130 kali).
* Rating rendah seperti 1 juga cukup banyak (74 kali), menunjukkan variasi preferensi pengguna.
* Ada rating tidak valid yaitu 18 yang hanya muncul sekali dan sudah dihapus dalam preprocessing.
* Secara umum, rating cenderung condong ke nilai tengah atas (7-10), mengindikasikan sebagian besar pengguna cukup puas dengan produk.

![Jumlah Rating per Produk](https://github.com/user-attachments/assets/fc2c85e9-0c96-4862-afed-24e37fb0621f)
**Insight:**
* Jumlah rating tiap produk bervariasi antara 20 sampai 41.
* Produk dengan cellphone_id 30 paling banyak mendapat rating (41), artinya paling populer.
* Produk dengan rating paling sedikit (20) adalah cellphone_id 0 dan 21, mungkin kurang populer.
* Produk dengan sedikit rating mungkin kurang data untuk model, jadi bisa dipertimbangkan filter minimal rating.

**Eksplorasi Dataset Rating (users)**

![Jumlah Kemunculan Usia](https://github.com/user-attachments/assets/5bd83cd7-fbd7-43bb-8edd-4cb6fc4fe18a)
**Insight:**
* Sebagian besar user berumur antara 25 hingga 50 tahun.
* Umur 25 dan 32 tahun muncul paling banyak, dengan masing-masing 12 dan 10 kemunculan.
* Umur di bawah 25 dan di atas 55 sangat sedikit, menandakan pengguna mayoritas adalah dewasa muda hingga paruh baya.
* Data ini bisa membantu segmentasi pengguna berdasarkan usia dalam analisis atau rekomendasi.

![Distribusi Gender Pengguna](https://github.com/user-attachments/assets/12cab826-cf92-484b-8d6f-f18672aaec33)
**Insight:**
* Mayoritas pengguna terdiri dari laki-laki (50) dan perempuan (46), distribusinya cukup seimbang.
* Terdapat 3 data yang belum diisi atau default dengan label "-Select Gender-".

![Distribusi Pekerjaan (Occupation) Pengguna](https://github.com/user-attachments/assets/55501cfe-19d4-4b20-ad93-95bdf4d73093)
**Insight:**
* Terdapat 45 jenis pekerjaan unik dengan variasi cukup beragam.
* Pekerjaan dengan jumlah terbanyak adalah manager (18), diikuti information technology (12) dan it (6).
* Ada beberapa entri yang tampaknya duplikat atau typo seperti healthare dan healthcare, serta it dan information technology yang sudah diperbaiki di data preparation.
* Perlu standarisasi dan pembersihan lebih lanjut untuk menyatukan pekerjaan yang sama dengan penulisan berbeda agar analisis lebih akurat.

---

## Data Preparation

Pada tahap ini dilakukan beberapa proses persiapan data agar data siap digunakan untuk pembangunan model rekomendasi. Tahapan yang dilakukan meliputi penggabungan dataset, pembersihan data, transformasi, dan encoding.

### 1. Penggabungan Dataset

Data rating, data handphone, dan data pengguna digabungkan menjadi satu dataframe dengan menggunakan `merge` berdasarkan `cellphone_id` dan `user_id`.

**Alasan:** Menggabungkan data agar semua informasi yang relevan (spesifikasi handphone, profil pengguna, dan rating) berada dalam satu tabel untuk analisis dan pemodelan.

### 2. Pengecekan dan Penghapusan Missing Values

Baris data yang mengandung nilai kosong (`null`) dihapus menggunakan `dropna()`.

**Alasan:** Missing values dapat menyebabkan error pada model dan menurunkan kualitas prediksi, sehingga data harus bersih.

### 3. Filtering Rating Tidak Valid

Data rating dengan nilai tidak valid (`rating = 18`) dihapus karena merupakan outlier atau kesalahan input.

**Alasan:** Memastikan data rating valid dan konsisten agar model belajar dari data yang representatif.

### 4. Pembersihan Kolom `occupation`

Melakukan normalisasi teks pada kolom pekerjaan pengguna dengan mengubah semua huruf menjadi huruf kecil dan memperbaiki kesalahan ketik (`healthare` menjadi `healthcare` dan `it` menjadi `information technology`).

**Alasan:** Konsistensi data kategori memudahkan analisis dan mencegah duplikasi kategori akibat perbedaan penulisan.

### 5. Penghapusan Duplikasi Data

Data handphone dideduplikasi berdasarkan `cellphone_id` untuk mendapatkan data unik tiap produk.

**Alasan:** Mencegah pengaruh ganda dari entri yang sama dalam analisis.

### 6. Ekstraksi Data Utama Handphone

Membuat dataframe baru yang berisi fitur utama handphone seperti `brand`, `model`, dan `operating_system`.

**Alasan:** Memisahkan data fitur utama untuk memudahkan pemodelan berbasis konten.

### 7. Persiapan Data untuk Collaborative Filtering

Pada data rating yang sudah dibersihkan, dilakukan proses encoding terhadap `user_id` dan `cellphone_id` menjadi indeks numerik (`user`, `cellphone`) untuk keperluan pemodelan. Nilai rating juga dinormalisasi ke rentang 0 sampai 1. Data diacak (shuffled) dan dibagi menjadi data latih dan validasi dengan perbandingan 80:20.

**Alasan:** Encoding diperlukan agar model dapat mengelola data kategori dalam format numerik. Normalisasi rating membantu model lebih stabil saat training. Pembagian data latih dan validasi digunakan untuk mengukur performa model secara objektif.

---

## Modeling

Tahap modeling dilakukan untuk membangun sistem rekomendasi handphone berdasarkan data yang telah dipersiapkan sebelumnya. Dua pendekatan digunakan untuk menyajikan rekomendasi, yaitu:

### 1. Content-Based Filtering menggunakan TF-IDF dan Cosine Similarity

Pendekatan pertama memanfaatkan informasi fitur produk, dalam hal ini `brand` handphone, untuk menghitung kemiripan antar model menggunakan teknik TF-IDF (Term Frequency-Inverse Document Frequency) dan cosine similarity.

* **Proses:**

  * Menghitung matriks TF-IDF dari kolom `brand` untuk merepresentasikan setiap model handphone.
  * Menghitung cosine similarity antar model handphone berdasarkan TF-IDF.
  * Menyajikan rekomendasi top-N model handphone yang paling mirip berdasarkan kemiripan cosine terhadap model input.

* **Kelebihan:**

  * Mudah diimplementasikan dan tidak memerlukan data rating yang lengkap.
  * Rekomendasi bersifat explainable, karena didasarkan pada fitur yang jelas (brand).
  * Cocok untuk produk baru yang belum memiliki rating (cold-start problem pada user-item rating).

* **Kekurangan:**

  * Tidak memperhitungkan preferensi pengguna secara eksplisit.
  * Terbatas pada fitur yang digunakan, sehingga jika fitur yang dipakai sedikit (misal hanya brand), rekomendasi bisa kurang variatif.

### 2. Collaborative Filtering menggunakan Neural Network (RecommenderNet)

Pendekatan kedua menggunakan teknik collaborative filtering dengan model embedding berbasis neural network. Model ini belajar dari interaksi pengguna dan handphone (rating) secara langsung.

* **Proses:**

  * Membuat embedding vektor untuk setiap user dan handphone.
  * Melakukan training untuk memprediksi rating menggunakan embedding tersebut.
  * Model menggunakan sigmoid activation untuk output prediksi rating ter-normalisasi.
  * Data dibagi menjadi data latih dan validasi untuk mencegah overfitting.

* **Kelebihan:**

  * Mampu menangkap pola preferensi pengguna secara eksplisit dari data rating.
  * Lebih fleksibel untuk menangani interaksi kompleks antara user dan produk.
  * Bisa memberikan rekomendasi personalisasi yang lebih akurat.

* **Kekurangan:**

  * Membutuhkan data rating yang cukup lengkap dan berkualitas.
  * Lebih kompleks dan butuh waktu training lebih lama.
  * Masalah cold-start untuk produk atau pengguna baru yang belum punya interaksi.

* **Proses pelatihan:**
  Model dilatih selama maksimal 100 epoch dengan batch size 8, menggunakan early stopping jika tidak ada perbaikan val\_loss selama 5 epoch berturut-turut.

---

## Evaluation

Pada proyek sistem rekomendasi ini, metrik evaluasi yang digunakan adalah **Root Mean Squared Error (RMSE)**. RMSE adalah metrik yang umum digunakan untuk mengukur kualitas model prediksi kontinu, khususnya pada permasalahan regresi dan rekomendasi dengan rating numerik.

### Penjelasan Metrik RMSE

RMSE dihitung sebagai akar kuadrat dari rata-rata kuadrat selisih antara nilai prediksi dan nilai aktual:

<img width="460" alt="Rumus RMSE" src="https://github.com/user-attachments/assets/fee2ffee-b072-4463-a088-698e0711044b" />

**RMSE mengukur seberapa besar kesalahan prediksi model secara rata-rata, dalam satuan yang sama dengan nilai asli.** Nilai RMSE yang lebih kecil menunjukkan prediksi model yang lebih akurat dan lebih dekat ke nilai sebenarnya.

### Hasil Evaluasi Model

Evaluasi dilakukan pada data training dan data validasi untuk mengukur kemampuan model dalam mempelajari pola (fit) dan kemampuannya dalam menggeneralisasi ke data baru (validasi).

```python
# Prediksi rating pada data training dan hitung RMSE
y_train_pred = model.predict(x_train).flatten()
rmse_train = np.sqrt(mean_squared_error(y_train, y_train_pred))

# Prediksi rating pada data validasi dan hitung RMSE
y_val_pred = model.predict(x_val).flatten()
rmse_val = np.sqrt(mean_squared_error(y_val, y_val_pred))

# Cetak hasil evaluasi
print(f"RMSE Train     : {rmse_train:.4f}")
print(f"RMSE Validation: {rmse_val:.4f}")
```

### RMSE Plot
![RMSE plot](https://github.com/user-attachments/assets/824a2b25-0def-4611-b4ad-3f5f3887c211)

### Interpretasi Hasil

* **RMSE Train** yang rendah menunjukkan model dapat mempelajari pola rating dengan baik pada data training.
* **RMSE Validation** yang tidak jauh berbeda dari RMSE train menunjukkan model memiliki kemampuan generalisasi yang baik, tidak overfitting.
* Jika RMSE validation jauh lebih tinggi dari RMSE train, maka model cenderung overfitting, artinya model terlalu cocok dengan data training dan kurang baik pada data baru.

---
### Referensi
Herlocker, J. L., Konstan, J. A., Terveen, L. G., & Riedl, J. T. (2004). Evaluating collaborative filtering recommender systems. ACM Transactions on Information Systems (TOIS), 22(1), 5–53.
https://doi.org/10.1145/963770.963772

Resnick, P., Iacovou, N., Suchak, M., Bergstrom, P., & Riedl, J. (1994). GroupLens: an open architecture for collaborative filtering of netnews. Proceedings of the 1994 ACM Conference on Computer Supported Cooperative Work (CSCW).
https://doi.org/10.1145/192844.192905

Zhang, S., Yao, L., Sun, A., & Tay, Y. (2019). Deep Learning based Recommender System: A Survey and New Perspectives. ACM Computing Surveys, 52(1), 1–38.
https://doi.org/10.1145/3285029

---
