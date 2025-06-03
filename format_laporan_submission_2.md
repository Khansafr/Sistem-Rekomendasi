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

**Insight:**
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

Persiapan data merupakan tahap penting agar dataset siap digunakan untuk membangun model rekomendasi yang akurat dan andal. Berikut adalah tahapan yang dilakukan secara berurutan:

### 1. Penggabungan Dataset

Data rating digabungkan dengan data handphone berdasarkan `cellphone_id`, kemudian hasilnya digabung kembali dengan data pengguna berdasarkan `user_id`.

**Alasan:** Mengintegrasikan data rating, spesifikasi handphone, dan profil pengguna dalam satu tabel agar analisis dan pemodelan menjadi lebih komprehensif.

### 2. Pengecekan Missing Values

Dilakukan pengecekan terhadap data yang memiliki nilai kosong (missing values) dan baris tersebut dihapus menggunakan `dropna()`.

**Alasan:** Memastikan data yang digunakan bersih dan bebas dari nilai kosong yang dapat menyebabkan error saat pemodelan.

### 3. Filter Rating Tidak Valid

Menghapus baris data dengan rating tidak valid, misalnya rating bernilai 18 yang merupakan outlier atau kesalahan input.

**Alasan:** Menjaga kualitas data dan validitas rating agar model belajar dari data yang benar.

### 4. Normalisasi Kolom Pekerjaan (`occupation`)

Mengubah seluruh teks di kolom `occupation` menjadi huruf kecil dan memperbaiki kesalahan penulisan (misal `'healthare'` menjadi `'healthcare'`).

**Alasan:** Menjaga konsistensi data kategori agar tidak terjadi duplikasi kategori akibat perbedaan penulisan.

### 5. Penghapusan Data Duplikat

Menghapus baris yang duplikat berdasarkan `cellphone_id` untuk mendapatkan data handphone yang unik.

**Alasan:** Menghindari bias akibat data yang berulang.

### 6. Ekstraksi Data Utama Handphone

Membuat DataFrame baru `phone_new` yang berisi fitur penting seperti `cellphone_id`, `brand`, `model`, dan `operating_system`.

**Alasan:** Menyiapkan data handphone dengan fitur inti untuk memudahkan analisis dan pembuatan model berbasis konten.

### 7. Encoding User dan Handphone

Mengonversi `user_id` dan `cellphone_id` menjadi angka berurutan menggunakan mapping dictionary.

**Alasan:** Membuat data numerik yang dapat diproses oleh model collaborative filtering berbasis neural network.

### 8. Konversi dan Normalisasi Rating

Mengubah tipe data rating menjadi float dan melakukan normalisasi ke rentang 0 hingga 1 agar nilai rating seragam untuk pelatihan model.

**Alasan:** Mempermudah proses pembelajaran model dengan rentang nilai yang konsisten.

### 9. Pengacakan Data (Shuffle)

Data diacak secara acak menggunakan `sample(frac=1, random_state=42)` untuk memastikan distribusi data acak dan reproducible.

**Alasan:** Menghindari bias model akibat pola urutan data yang terstruktur.

### 10. Pembagian Data Train dan Validation

Membagi data menjadi 80% untuk pelatihan (training) dan 20% untuk validasi (validation).

**Alasan:** Mengukur performa model dengan data yang belum pernah dilihat saat pelatihan.

### 11. Penerapan TF-IDF pada Kolom Brand

Menggunakan `TfidfVectorizer` untuk mengubah teks brand handphone menjadi representasi numerik berbasis TF-IDF.

**Alasan:** Menangkap pentingnya setiap kata (brand) dalam data untuk fitur model Content-Based Filtering.

### 12. Membuat DataFrame Matriks TF-IDF

Mengubah hasil matriks TF-IDF yang berbentuk sparse matrix menjadi DataFrame dengan kolom fitur kata unik dan indeks model handphone.

**Alasan:** Memudahkan analisis dan pemrosesan data numerik dari teks untuk model rekomendasi.

---

## Modeling

### 1. Content-Based Filtering

Pada tahap modeling, sistem rekomendasi handphone dibangun dengan memanfaatkan hasil ekstraksi fitur dari tahap sebelumnya, yaitu matriks TF-IDF yang merepresentasikan brand dari masing-masing model handphone. Pendekatan yang digunakan adalah **Content-Based Filtering** dengan perhitungan kemiripan antar produk menggunakan **Cosine Similarity**.

* **Proses:**

  * Memeriksa keberadaan model input pada matriks similarity, untuk menghindari error jika model tidak ditemukan.
  * Mengambil nilai kemiripan (cosine similarity) antara model input dengan semua model lain, kemudian menghapus nilai kemiripan model terhadap dirinya sendiri.
  * Mengurutkan nilai kemiripan secara menurun dan memilih sejumlah `k` model teratas sebagai rekomendasi.
  * Menggabungkan hasil rekomendasi dengan data tambahan seperti brand dan sistem operasi agar output rekomendasi lebih informatif dan mudah dipahami oleh pengguna.

* **Kelebihan:**

  * **Explainable**: Rekomendasi didasarkan pada kemiripan brand yang dapat dijelaskan secara intuitif kepada pengguna.
  * **Efisien**: Tidak membutuhkan data rating pengguna, sehingga dapat digunakan walaupun data pengguna minim.
  * **Cold-start problem**: Bisa merekomendasikan model baru yang belum memiliki data interaksi pengguna.

* **Kekurangan:**

  * Terbatas pada fitur brand dan operating system saja, sehingga variasi rekomendasi bisa kurang beragam.
  * Tidak mempertimbangkan preferensi eksplisit pengguna.

#### Contoh Hasil Similarity Model Handphone

Menghitung tingkat kemiripan antar model handphone menggunakan metode *cosine similarity*. Matriks similarity ini digunakan untuk merekomendasikan model handphone yang mirip berdasarkan kemiripan brand dan sistem operasi.

Berikut adalah contoh hasil similarity untuk beberapa model iPhone:

| Model             | Brand | Operating System | Similarity |
| ----------------- | ----- | ---------------- | ---------- |
| iPhone XR         | Apple | iOS              | 1.0        |
| iPhone 13 Pro Max | Apple | iOS              | 1.0        |
| iPhone SE (2022)  | Apple | iOS              | 1.0        |
| iPhone 13 Mini    | Apple | iOS              | 1.0        |
| iPhone 13         | Apple | iOS              | 1.0        |

Nilai similarity 1.0 menunjukkan bahwa model-model tersebut memiliki kemiripan yang sangat tinggi berdasarkan fitur brand dan sistem operasi yang identik. Nilai ini mencerminkan bahwa model-model tersebut berasal dari merek dan platform yang sama sehingga sangat relevan untuk direkomendasikan sebagai alternatif.

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

Berikut contoh penulisan laporan rekomendasi yang sudah disesuaikan dengan data User ID: 114 yang kamu berikan:


#### Contoh Output Rekomendasi untuk User ID: 114

**Handphone dengan rating tertinggi oleh user:**

| Brand   | Model            |
| ------- | ---------------- |
| Samsung | Galaxy S22 Ultra |
| Oppo    | Find X5 Pro      |
| Xiaomi  | 12 Pro           |
| Xiaomi  | 11T Pro          |
| Apple   | iPhone 13 Pro    |

**Top 10 Rekomendasi Handphone untuk user ini:**

| Brand    | Model               |
| -------- | ------------------- |
| Samsung  | Galaxy A53          |
| Google   | Pixel 6             |
| Google   | Pixel 6 Pro         |
| Apple    | iPhone 13           |
| Google   | Pixel 6a            |
| Apple    | iPhone XR           |
| Asus     | Zenfone 8           |
| Motorola | Moto G Power (2022) |
| Samsung  | Galaxy Z Fold 3     |
| Samsung  | Galaxy Z Flip 3     |

Output ini menunjukkan rekomendasi personalisasi yang dihasilkan oleh model neural network berdasarkan pola rating dan preferensi user tersebut.

---

## Evaluation

### Evaluasi Content-Based Filtering

#### Metrik Evaluasi untuk Content-Based Filtering

Untuk mengukur kualitas rekomendasi yang dihasilkan oleh sistem Content-Based Filtering, digunakan metrik berbasis **ranking** seperti:

##### 1. Precision\@K

* **Definisi:** Proporsi rekomendasi yang relevan di antara K rekomendasi teratas.
* **Interpretasi:** Seberapa akurat rekomendasi dalam menampilkan item yang benar-benar disukai pengguna.
* **Rumus:**

$$
Precision@K = \frac{\text{Jumlah item relevan dalam top K rekomendasi}}{K}
$$

##### 2. Recall\@K

* **Definisi:** Proporsi item relevan yang berhasil direkomendasikan di antara semua item relevan pengguna.
* **Interpretasi:** Seberapa banyak preferensi pengguna yang berhasil "tertangkap" oleh sistem dalam rekomendasi top K.
* **Rumus:**

$$
Recall@K = \frac{\text{Jumlah item relevan dalam top } K \text{ rekomendasi}}{\text{Jumlah total item relevan pengguna}}
$$

##### 3. F1-score\@K (opsional)

* Kombinasi harmonis antara Precision\@K dan Recall\@K, digunakan bila ingin keseimbangan antara keduanya.

### Contoh Hasil Evaluasi untuk User 114

* **Relevant Models (user rating ≥ threshold):** 10 model
* **Recommended Models (top 10 rekomendasi berdasarkan 'iPhone 13 Pro'):** 10 model

| Metrik       | Nilai | Interpretasi                                                               |
| ------------ | ----- | -------------------------------------------------------------------------- |
| Precision\@5 | 0.20  | Dari 5 rekomendasi teratas, 1 model adalah relevan (20%).                  |
| Recall\@5    | 0.10  | Dari 10 model relevan, hanya 1 model masuk ke 5 rekomendasi teratas (10%). |

Kesimpulan:

Sistem rekomendasi cukup akurat dalam memberikan beberapa rekomendasi tepat sasaran, namun belum berhasil menangkap banyak preferensi pengguna secara keseluruhan.

### Evaluasi Collaborative Filtering

Untuk model Collaborative Filtering yang memprediksi rating numerik, digunakan metrik **Root Mean Squared Error (RMSE)**.

#### Penjelasan Metrik RMSE

RMSE dihitung sebagai akar kuadrat dari rata-rata kuadrat selisih antara nilai prediksi dan nilai aktual:

$$
RMSE = \sqrt{\frac{1}{N} \sum_{i=1}^N (y_i - \hat{y}_i)^2}
$$

Dimana:

* $y_i$ adalah rating asli
* $\hat{y}_i$ adalah rating prediksi
* $N$ adalah jumlah data

RMSE mengukur seberapa besar kesalahan prediksi secara rata-rata. Nilai RMSE yang lebih kecil menunjukkan prediksi yang lebih akurat.

#### Hasil Evaluasi Model

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

Output:

```
RMSE Train     : 0.0643
RMSE Validation: 0.2746
```

#### Interpretasi Hasil RMSE

* RMSE pada data training yang rendah (0.0643) menunjukkan model dapat mempelajari pola rating dengan baik.
* RMSE pada data validasi (0.2746) relatif lebih tinggi namun masih cukup baik, menandakan model masih memiliki kemampuan generalisasi, meskipun ada potensi overfitting ringan.
* Perbedaan RMSE yang tidak terlalu besar antara training dan validasi menunjukkan model cukup stabil.

---

### Referensi
Herlocker, J. L., Konstan, J. A., Terveen, L. G., & Riedl, J. T. (2004). Evaluating collaborative filtering recommender systems. ACM Transactions on Information Systems (TOIS), 22(1), 5–53.
https://doi.org/10.1145/963770.963772

Resnick, P., Iacovou, N., Suchak, M., Bergstrom, P., & Riedl, J. (1994). GroupLens: an open architecture for collaborative filtering of netnews. Proceedings of the 1994 ACM Conference on Computer Supported Cooperative Work (CSCW).
https://doi.org/10.1145/192844.192905

Zhang, S., Yao, L., Sun, A., & Tay, Y. (2019). Deep Learning based Recommender System: A Survey and New Perspectives. ACM Computing Surveys, 52(1), 1–38.
https://doi.org/10.1145/3285029

---
