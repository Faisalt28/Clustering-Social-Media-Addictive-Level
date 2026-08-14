# 📱 Social Media Addiction Level Clustering

Sistem klasterisasi (clustering) tingkat kecanduan media sosial pada mahasiswa/siswa menggunakan algoritma **K-Means**, berdasarkan pola penggunaan harian dan durasi tidur. Live demo dapat dicoba di Hugging Face Spaces:

🔗 **[social_media_addictive_level](https://huggingface.co/spaces/Axion28/social_media_addictive_level)**

## 📋 Deskripsi

Project ini mengelompokkan pengguna ke dalam **3 tingkat kecanduan media sosial** — Tinggi, Sedang, dan Rendah — menggunakan pendekatan *unsupervised learning* (K-Means Clustering), berdasarkan dua fitur utama:
- Rata-rata penggunaan media sosial harian (jam)
- Durasi tidur per malam (jam)

Asumsi yang digunakan: semakin tinggi penggunaan media sosial dan semakin rendah durasi tidur, semakin tinggi indikasi tingkat kecanduannya.

## 📊 Dataset

Dataset yang digunakan: [Social Media Addiction vs Relationships](https://www.kaggle.com/datasets/adilshamim8/social-media-addiction-vs-relationships) dari Kaggle, diunduh otomatis melalui `kagglehub` (file: `Students Social Media Addiction.csv`).

Fitur yang dipakai untuk clustering:
| Kolom | Deskripsi |
|---|---|
| `Avg_Daily_Usage_Hours` | Rata-rata penggunaan media sosial per hari (jam) |
| `Sleep_Hours_Per_Night` | Durasi tidur per malam (jam) |

## 📁 Struktur File

```
.
├── UAS_Addictive.ipynb        # Notebook eksplorasi data, clustering, dan pembuatan model
├── uas_ml_addictive.pkl       # Model hasil training (scaler + model KMeans)
└── app.py                     # Aplikasi Hugging Face Space
```

## ⚙️ Alur Kerja

### 1. Eksplorasi & Persiapan Data
- Dataset diunduh langsung dari Kaggle menggunakan `kagglehub`
- Melihat persebaran data awal antara jam penggunaan harian dan jam tidur
- Normalisasi fitur menggunakan **StandardScaler** agar skala data seragam sebelum clustering

### 2. Menentukan Jumlah Cluster Optimal (Elbow Method)
- Mencoba nilai K dari 1 sampai 10
- Menghitung *inertia* (within-cluster sum of squares) tiap nilai K
- Memilih **K = 3** sebagai jumlah cluster optimal berdasarkan titik elbow

### 3. Training Model K-Means & Pelabelan
- Melatih model **K-Means (K=3)** dengan `init='k-means++'` pada data yang sudah dinormalisasi
- Menghitung *addiction score* dari tiap centroid cluster (penggunaan tinggi − tidur rendah = skor makin tinggi)
- Mengurutkan cluster berdasarkan skor tersebut dan memberi label:
  - **Kecanduan Tinggi**
  - **Kecanduan Sedang**
  - **Kecanduan Rendah**
- Visualisasi hasil clustering dalam scatter plot

### 4. Menyimpan Model untuk Deployment
Model disimpan dalam satu file `uas_ml_addictive.pkl` berisi:
- `scaler` — `StandardScaler` yang sudah di-*fit*
- `model` — model `KMeans` yang sudah dilatih

File ini kemudian digunakan oleh aplikasi Hugging Face Space untuk memprediksi tingkat kecanduan dari input pengguna baru.

## 🚀 Cara Menjalankan Secara Lokal

```bash
pip install kagglehub pandas numpy matplotlib seaborn scikit-learn
jupyter notebook UAS_Addictive.ipynb
```

Dataset akan otomatis terunduh ke cache lokal melalui `kagglehub` saat notebook dijalankan.

## 🌐 Demo

Aplikasi ini telah di-deploy dan dapat dicoba langsung di Hugging Face Spaces:

👉 **https://huggingface.co/spaces/Axion28/social_media_addictive_level**

Pengguna dapat memasukkan rata-rata jam penggunaan media sosial harian dan jam tidur per malam untuk mengetahui prediksi tingkat kecanduannya.

## 📝 Catatan

- Model ini bersifat **unsupervised** (tidak ada label ground-truth kecanduan pada dataset asli); label "Tinggi/Sedang/Rendah" ditentukan berdasarkan interpretasi posisi centroid, bukan dari anotasi manusia.
- Karena sifatnya clustering, hasil label bisa berubah jika model dilatih ulang dengan random_state atau data yang berbeda.
