# Analisis dan Prediksi Harga Komoditas Pangan di Indonesia Berdasarkan Waktu dan Wilayah
### Projek Akhir Praktikum Data Science — Kelompok RestuBapak (Kelas IF-B)

**Anggota Kelompok:**
* Abdillah Abhipraya (123240004)
* Radja Azhara Indra Kusuma (123240012)
* Ari Satriya Dinata (123240018)

---

## Ringkasan Proyek

Repositori ini memuat kode, dokumentasi audit, visualisasi, dan laporan capaian lengkap untuk Proyek Akhir Data Science. Analisis memanfaatkan data panel harga harian pangan nasional (PIHPS Bank Indonesia) periode 1 Januari 2022 hingga 12 Februari 2026 yang mencakup 10 komoditas strategis, 3 kanal pasar (*produsen, pasar tradisional, pasar modern*), dan 34 provinsi di Indonesia.

Seluruh materi perkuliahan dari **Modul I hingga Modul IX** diintegrasikan secara terpadu ke dalam 100 sel notebook yang dapat direproduksi secara *end-to-end*:
* **Modul I & II:** Data Collection, Audit Integritas, Cleaning & Preprocessing (Bagian B & D)
* **Modul III:** Feature Engineering & Exploratory Data Analysis (Bagian C & E)
* **Modul IV:** Clustering I — K-Means (Bagian F)
* **Modul V:** Clustering II — Hierarchical Clustering / Ward Linkage (Bagian F)
* **Modul VI:** Classification I — Support Vector Machine / SVM (Bagian G)
* **Modul VII:** Classification II — Random Forest Classifier (Bagian G)
* **Modul VIII:** Regression I — Linear Regression (OLS), Ridge ($L_2$), dan LASSO ($L_1$) (Bagian H)
* **Modul IX:** Regression II — Prediksi Deret Waktu & Evaluasi Uji Kronologis (Bagian H)

---

## Status Eksekusi dan Lingkungan

Notebook [Projek_Akhir_Data_Science.ipynb](file:///D:/Projects/1_IFB_RestuBapak/Projek_Akhir_Data_Science.ipynb) telah dieksekusi ulang *end-to-end* tanpa galat pada Python 3.13.2 64-bit menggunakan `nbclient`. Seluruh tabel, visualisasi kurva, dan matriks evaluasi tersimpan di dalam notebook serta berkas artefak terkait di direktori `output/`.

### 1. Aktivasi Virtual Environment (`.venv`)

Lingkungan terisolasi telah disiapkan di `.venv/` dan didaftarkan sebagai kernel Jupyter:

- **PowerShell:**
  ```powershell
  cd D:\Projects\1_IFB_RestuBapak
  .\.venv\Scripts\Activate.ps1
  ```
- **Command Prompt (CMD):**
  ```cmd
  cd D:\Projects\1_IFB_RestuBapak
  .\.venv\Scripts\activate.bat
  ```
- **Jupyter Notebook / VS Code:**
  Pilih kernel: **`Python (RestuBapak IFB)`** atau arahkan interpreter ke `D:\Projects\1_IFB_RestuBapak\.venv\Scripts\python.exe`.

### 2. Eksekusi Ulang Notebook (*Headless* / Otomatis)

Jalankan perintah berikut dari direktori utama proyek:

```powershell
.\.venv\Scripts\python.exe -c "import nbformat; from nbclient import NotebookClient; nb = nbformat.read('Projek_Akhir_Data_Science.ipynb', as_version=4); NotebookClient(nb, timeout=1800, kernel_name='restubapak-env').execute(); nbformat.write(nb, 'Projek_Akhir_Data_Science.ipynb'); print('Eksekusi notebook selesai tanpa galat!')"
```

---

## Sumber Data dan Lisensi

* **Dataset Kaggle:** `muhyusuf1112/indonesia-commodity-price-based-piphs-source` (Muh. Yusuf; Versi 2)
* **Sumber Primer:** Pusat Informasi Harga Pangan Strategis (PIHPS) Nasional, Bank Indonesia
* **Lisensi Metadata:** MIT License
* **Arsip Mentah:** `archive.zip` (1,12 MB terkompresi; 30 berkas CSV; total uncompressed 12,24 MB)
* **Struktur File Sumber:**
  ```
  data_komoditas_pangan/{pasar_modern,pasar_tradisional,produsen}/
      Cleaned_After_Imputation/final_komoditas_<kanal>_<komoditas>_2022_2026.csv
  ```
* **Format:** Tabel lebar (*wide format*) berisi kolom `Date_Param` dan 32–34 kolom provinsi.
* **Satuan:** Minyak goreng dalam Rp/liter; sembilan komoditas lainnya dalam Rp/kg.

---

## Angka Kunci dan Bukti Audit

Seluruh angka di bawah ini dihitung langsung oleh sel notebook dan dapat diverifikasi pada berkas audit di `output/audit/`:

| Indikator | Nilai / Statistik | Lokasi Bukti |
|---|---|---|
| **Total observasi panel mentah** | 1.354.641 baris | Bagian B.4 |
| **Distribusi kanal pasar** | Tradisional: 481.236 • Modern: 503.141 • Produsen: 370.264 | Bagian B.4 |
| **Jumlah deret waktu mandiri** | 925 deret (Kanal $\times$ Komoditas $\times$ Provinsi) | Bagian E.3 |
| **Koreksi kesalahan orde besaran (R1)** | 14 sel pada 2 deret (daging ayam Kaltara & daging sapi Gorontalo) | Bagian D.3 |
| **Karantina ramp interpolasi buatan (R2)** | 40 sel pada 1 deret (gula pasir Kalsel melonjak ke Rp 605.500) | Bagian D.4 |
| **Observasi akhir pekan identik (ffill)** | 352.668 dari 362.891 baris (97,18% Sabtu/Minggu menyalin hari Jumat) | Bagian B.8 |
| **Baris dataset siap model (H+7)** | 1.320.376 baris $\times$ 28 kolom | Bagian E.3 |
| **Pembagian data kronologis** | Latih: 920.911 (70%) • Validasi: 199.610 (15%) • Uji: 199.855 (15%) | Bagian E.5 |
| **Klaster provinsi optimal (K-Means)** | $k = 3$ (Silhouette Score puncak & penurunan inersia elbow) | Bagian F.2 |
| **Recall deteksi dini lonjakan ($\ge 5\%$)** | SVM: 84,2% • Random Forest: 81,0% (ROC-AUC: 0,825) | Bagian G.2 |
| **Performa RMSE Regresi Deret Waktu** | OLS: 5.288,52 • Ridge: 5.290,61 • LASSO: 5.296,15 (vs Naive: 5.388,91) | Bagian H.2 |

---

## Metodologi dan Alasan Pemilihan Teknik

### 1. Filosofi Pembersihan Data (Data Cleaning)
* **Koreksi Typo (R1):** Nilai hanya diubah jika rasio terhadap pangkat 10 terdekat $\le 0,5\%$ dan berada dalam rentang $5\%$ dari median bergerak lokal. Mengoreksi 14 sel salah input digit tanpa mengubah observasi sah.
* **Karantina Ramp (R2):** Menandai run kenaikan linear konstan buatan selama $\ge 5$ titik dengan rentang $> 1 \times$ median deret. Nilai dikarantina menjadi `NaN` agar tidak merusak estimasi model.
* **Penandaan Outlier (R3):** Outlier IQR sebanyak 28.032 sel dipertahankan karena merefleksikan volatilitas riil komoditas pangan bergejolak (*volatile foods*).

### 2. Pencegahan Kebocoran Informasi (*Leakage-Free Preprocessing*)
* **Pergeseran Target ke H+7:** Karena $97,18\%$ data akhir pekan adalah hasil salinan hari sebelumnya, pemodelan H+1 akan menghasilkan ilusi akurasi semu. Horizon H+7 menjamin model benar-benar memprediksi dinamika harga masa depan.
* **Korelasi Dalam-Deret (*Within-Series Correlation*):** Korelasi fitur terhadap target dihitung per deret pada data latih saja untuk menghindari *Simpson's Paradox* akibat disparitas nominal antar-komoditas.
* **StandardScaler & OneHotEncoder:** Di-fit secara ketat hanya pada himpunan data latih (*train*) dan disimpan ke `preprocessor_h7.joblib`.

### 3. Klasterisasi Wilayah Spasial (Modul IV & V)
* **Alasan Penggunaan:** Menghindari kebijakan intervensi pasar yang seragam secara nasional dengan mengelompokkan 34 provinsi berdasarkan profil harga, margin ritel modern terhadap pasar tradisional, dan koefisien variasi volatilitas.
* **Kombinasi Algoritma:** K-Means mempartisi data secara terukur dengan evaluasi Elbow/Silhouette, sementara Hierarchical Clustering (Ward Linkage) memberikan visualisasi hierarki kedekatan kepulauan melalui Dendrogram.
* **Hasil:** 3 Klaster Wilayah:
  - *Klaster 0 (Sentra Produksi / Stabil):* Jawa dan Lampung (harga rendah, disparitas tipis).
  - *Klaster 1 (Konsumen Perkotaan):* DKI Jakarta, Banten, Bali (margin modern tinggi, konsumsi padat).
  - *Klaster 2 (Kepulauan / Volatil):* Maluku, Papua, Papua Barat (harga tinggi, volatilitas logistik besar).

### 4. Klasifikasi Sistem Peringatan Dini / Early Warning System (Modul VI & VII)
* **Alasan Penggunaan:** Bapanas dan TPID membutuhkan indikator dini apakah suatu wilayah akan mengalami kenaikan harga pangan $\ge +5\%$ dalam seminggu ke depan (*imbalanced class* ~12,7% spike).
* **Pemilihan Model:**
  - *SVM (SGD Hinge with Balanced Weight):* Menemukan batas pemisah berdimensi tinggi dengan *maximum margin*; menghasilkan **Recall 84,2%** pada deteksi lonjakan harga.
  - *Random Forest Classifier:* Menangkap interaksi non-linear dan menghasilkan diskriminasi terbaik dengan **ROC-AUC 0,825**.
  - *Prinsip Evaluasi:* Mengutamakan **Recall** karena biaya *False Negative* (gagal mengantisipasi kelangkaan pangan) jauh lebih fatal daripada *False Positive*.

### 5. Regresi & Prediksi Deret Waktu (Modul VIII & IX)
* **Alasan Pembanding Naif (*Naive Benchmark*):** Model deret waktu wajib mengalahkan standar acuan $\hat{y}_{t+7} = \text{lag\_7}$.
* **Linear, Ridge, dan LASSO:** Fitur lag dan moving average (`lag_7`, `ma_7`, `lag_14`, `ma_14`) memiliki multikolinearitas tinggi ($r > 0,85$). Ridge ($L_2$) meredam variansi koefisien, sedangkan LASSO ($L_1$) menyusutkan bobot fitur menengah yang redundan.
* **Temuan Evaluasi:** Seluruh model regresi parametrik berhasil menurunkan error kuadratik ekstrem dibandingkan Naive Benchmark (penurunan RMSE sebesar ~100 Rupiah secara nasional).

---

## Struktur Direktori dan Berkas yang Dihasilkan

```
1_IFB_RestuBapak/
│
├── Projek_Akhir_Data_Science.ipynb   # Notebook utama 100 sel teruji end-to-end
├── README.md                         # Dokumentasi teknis proyek
├── requirements.txt                  # Daftar dependensi library Python
├── .gitignore                        # Konfigurasi pengabaian berkas biner/venv
├── archive.zip                       # Data mentah asli dari Kaggle
│
├── data_komoditas_pangan/            # Direktori ekstraksi data CSV per kanal
│   ├── pasar_modern/
│   ├── pasar_tradisional/
│   └── produsen/
│
└── output/
    ├── Progress_1_Presentasi_Proyek_Akhir_Data_Science.xlsx  # Lembar pemenuhan 18 capaian
    │
    ├── figures/                      # Gambar visualisasi resolusi tinggi (300 DPI)
    │   ├── fig_b1_kelengkapan_provinsi.png
    │   ├── fig_b2_pengisian_akhir_pekan.png
    │   ├── fig_c1_harga_antarkanal.png
    │   ├── fig_c2_pola_temporal.png
    │   ├── fig_c3_volatilitas.png
    │   ├── fig_c4_disparitas_wilayah.png
    │   ├── fig_c5_korelasi.png
    │   ├── fig_d1_koreksi_orde_besaran.png
    │   ├── fig_d2_anomali_ramp.png
    │   ├── fig_d3_sebelum_sesudah_cleaning.png
    │   ├── fig_e1_fitur_vs_target_h7.png
    │   ├── fig_f1_elbow_silhouette.png
    │   ├── fig_f2_dendrogram_provinsi.png
    │   └── fig_h1_prediksi_vs_aktual_regresi.png
    │
    ├── audit/                        # Berkas tabel audit statistik CSV
    │   ├── data_inventory.csv
    │   ├── data_quality_report.csv
    │   ├── column_quality_report.csv
    │   ├── deskriptif_komoditas_kanal.csv
    │   ├── disparitas_wilayah.csv
    │   ├── korelasi_komoditas.csv
    │   ├── cleaning_log.csv
    │   ├── anomali_orde_besaran.csv
    │   ├── korelasi_fitur_target_h7.csv
    │   ├── feature_selection_h7.csv
    │   ├── kamus_data_final.csv
    │   ├── clustering_provinsi.csv
    │   ├── classification_metrics.csv
    │   ├── regression_metrics_h7.csv
    │   └── perbandingan_komoditas_regresi.csv
    │
    └── processed/                    # Artefak model & data siap latih
        ├── _panel_raw.parquet        # Panel gabungan mentah (1.354.641 baris)
        ├── pihps_siap_model.parquet  # Dataset final siap model (1.320.376 baris)
        └── preprocessor_h7.joblib    # Objek scaler dan encoder (fit pada train)
```

---

## Rekomendasi Kebijakan (Stakeholders)

1. **Badan Pangan Nasional (Bapanas) & Kemendag:** Memanfaatkan model Random Forest Classifier sebagai instrumen *Early Warning System* (EWS) untuk mendeteksi potensi lonjakan harga $\ge 5\%$ dengan waktu respons 7 hari sebelum kenaikan terjadi di pasar ritel.
2. **Perum BULOG:** Mengalokasikan cadangan beras dan minyak goreng dengan prioritas logistik ke **Klaster 2 (Wilayah Kepulauan / Rentan Volatilitas)** untuk meredam lonjakan harga akibat kendala transportasi laut.
3. **Tim Pengendali Inflasi Daerah (TPID):** Memprioritaskan pemantauan harian pada komoditas hortikultura (cabai merah, cabai rawit, bawang merah) yang memiliki volatilitas tertinggi (MAPE ~13%), sementara komoditas pangan pokok (beras, gula, minyak) dikelola melalui instrumen kepatuhan HET.

---

## Ringkasan Pemenuhan 18 Capaian Rubrik

| No | Aspek | Item Capaian | Status | Bukti / Lokasi Notebook |
|:---:|---|---|:---:|---|
| **1** | Data Understanding | Sumber data dan lisensi/izin | **Selesai** | Bagian B.1; Kaggle PIHPS v2 (MIT License). |
| **2** | Data Understanding | Data dictionary (variabel, tipe, unit) | **Selesai** | Bagian B.5 & `output/audit/kamus_data_final.csv`. |
| **3** | Data Understanding | Kualitas data (missing, outlier, duplikasi) | **Selesai** | Bagian B.3, B.4.1, D.2; 0 duplikat kunci. |
| **4** | Data Understanding | EDA (distribusi & korelasi) | **Selesai** | Bagian C.1–C.6; visualisasi antarkanal & korelasi. |
| **5** | Data Understanding | Representativitas dan bias sampel | **Selesai** | Bagian B.7–B.8; bias pengisian akhir pekan dibedah. |
| **6** | EDA | Statistik deskriptif ditampilkan & disimpulkan | **Selesai** | Bagian C.1 & `output/audit/deskriptif_komoditas_kanal.csv`. |
| **7** | EDA | Visualisasi fitur dan target H+7 | **Selesai** | Bagian C & E.4; grafik `fig_e1_fitur_vs_target_h7.png`. |
| **8** | EDA | Analisis korelasi fitur-target (min. 3 insight) | **Selesai** | Bagian E.6; korelasi within-series train dilaporkan. |
| **9** | Audit Kualitas Data | Hitung missing, outlier, duplikat tiap kolom | **Selesai** | `output/audit/column_quality_report.csv`. |
| **10** | Data Cleaning | Strategi penanganan isu kualitas data tepat | **Selesai** | Aturan R1 (14 sel), R2 (40 sel), R3 (penandaan). |
| **11** | Data Cleaning | Kondisi sebelum dan sesudah dibersihkan | **Selesai** | Bagian D.6.1 & `fig_d3_sebelum_sesudah_cleaning.png`. |
| **12** | Preprocessing | Encoding, scaling, dan penanganan imbalance | **Selesai** | `preprocessor_h7.joblib`; `class_weight='balanced'`. |
| **13** | Preprocessing | Splitting dataset train/val/test bebas kebocoran | **Selesai** | Bagian E.5; split kronologis tanggal unik 70/15/15. |
| **14** | Feature Selection | Memilih fitur disertai alasan dan bukti | **Selesai** | Bagian E.7 & `output/audit/feature_selection_h7.csv`. |
| **15** | Modeling | Membangun model dan baseline sesuai tujuan | **Selesai** | Bagian F, G, H; K-Means, Hierarchical, SVM, RF, OLS, Ridge, LASSO. |
| **16** | Evaluasi | Evaluasi model dengan metrik komparatif | **Selesai** | `regression_metrics_h7.csv` & `classification_metrics.csv`. |
| **17** | Interpretasi | Penjelasan performa, kesalahan, dan insight bisnis | **Selesai** | Bagian F.3, G.3, H.4, I.2–I.3; profil klaster & analisis residual. |
| **18** | Presentasi | Menyampaikan alur, temuan, dan batasan runtut | **Selesai** | Dokumentasi lengkap di `README.md` & lembar Excel. |
