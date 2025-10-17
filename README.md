# 🛒 Supermarket Customer Data Cleaning & Preparation
**Capstone Project 2 – JCDS BSD Bachtiar Mardiansyah**

Proyek ini merupakan bagian dari Capstone Project yang berfokus pada proses *data cleaning* dan *data preparation* terhadap dataset penjualan dan pelanggan perusahaan **Supermarket Customer**.

Tujuannya adalah memastikan dataset siap digunakan untuk **analisis eksploratif** dan **visualisasi di Looker Studio**.

---

## 📊 **1. Deskripsi Proyek**

Supermarket Customer adalah perusahaan ritel (fiktif) yang bertujuan memahami perilaku pembelian dan demografi pelanggannya. Data yang digunakan merupakan gabungan antara informasi demografi pelanggan dan catatan pengeluaran mereka selama dua tahun terakhir di berbagai kategori produk supermarket (seperti anggur, daging, buah, dll.).

Analisis ini bertujuan untuk memahami:
* **Bagaimana hubungan** antara **pendapatan ($\text{Income}$) pelanggan** dengan **total pengeluaran ($\text{TotalSpent}$) mereka**.
* **Distribusi pengeluaran** pelanggan di berbagai kategori produk.
* **Efektivitas** berbagai **kampanye promosi** yang ditawarkan supermarket.
* Persiapan data agar dapat digunakan di **Looker Studio Dashboard** (untuk visualisasi preferensi dan segmentasi pelanggan).

---

## 🧾 **2. Dataset**

* **Nama file:** `Supermarket-Customer-Raw.csv` (raw)
* **Hasil cleaning:** `Supermarket-Customer-Clean.csv`
* **Sumber data:** Kaggle – *Customer Personality Analysis* / *Marketing Campaign Data*
* **Periode waktu:** Data Transaksi dan Aktivitas selama 2 tahun terakhir.
* **Ukuran data:** $\pm 2.240$ baris (simulasi pelanggan).

### Kolom utama yang relevan dengan analisis:
| Kolom | Deskripsi |
|---|---|
| `Income` | Pendapatan tahunan rumah tangga (Numerik) |
| `Dt_Customer` | Tanggal pendaftaran sebagai pelanggan |
| `MntWines`, `MntMeatProducts`, dll. | Jumlah pengeluaran untuk kategori produk tertentu |
| `NumWebPurchases` | Jumlah pembelian yang dilakukan melalui *website* |
| `AcceptedCmp1` s.d. `Response` | Status penerimaan ($\text{0}$ atau $\text{1}$) dari $\text{6}$ kampanye promosi terakhir |
| `Complain` | Status keluhan pelanggan ($\text{0}$ atau $\text{1}$) |
| **`TotalSpent`** | **Kolom baru** gabungan dari semua pengeluaran produk (dibuat saat *preparation*) |

---

## 🧹 **3. Proses Data Cleaning & Preparation (Jupyter Notebook)**

Notebook: `supermarket-customer-analysis.ipynb`

Langkah-langkah utama yang dilakukan:

### 🧩 a. Import & Initial Exploration
* Membaca dataset mentah menggunakan `pandas`.
* Mengecek struktur data, tipe kolom, dan jumlah *missing values*.
* Melihat ringkasan statistik awal dengan `.describe()`.

### 🔧 b. Data Type & Formatting
* Mengonversi kolom tanggal (`Dt_Customer`) menjadi `datetime`.
* Mengubah kolom numerik seperti pengeluaran (`MntWines`, dll.) dan pendapatan (`Income`) menjadi tipe $\text{float}$.
* Membuat kolom baru **`TotalSpent`** dengan menjumlahkan semua kolom pengeluaran produk.

### 🧭 c. Missing Values & Duplicates
* Menghapus baris duplikat (jika ada) berdasarkan `ID` pelanggan.
* Menghapus baris dengan *missing values* pada kolom **`Income`** (karena jumlahnya sedikit $\approx 1\%$ dan variansi data besar, sehingga *imputation* dengan *mean/median* berisiko tidak akurat).

### 📈 d. Outlier Detection & Correction
* Mengidentifikasi anomali dengan *boxplot* & *IQR* pada kolom kunci seperti **`Income`** dan **`TotalSpent`**.
* Menghapus atau menangani *outlier* ekstrem (misalnya, pendapatan sangat tinggi) yang mungkin mempengaruhi analisis korelasi dan segmentasi.
* Membuat kolom **kategori pendapatan** (`Income_Level`) menggunakan *binning* berdasarkan *cutoff* visual (Low: $\leq \text{40k}$, Medium: $\text{40k-80k}$, High: $\geq \text{80k}$).
