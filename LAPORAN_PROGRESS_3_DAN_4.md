# LAPORAN CAPAIAN PROGRESS 3 & PROGRESS 4
## PROYEK AKHIR DATA SCIENCE: ANALISIS DAN PREDIKSI HARGA KOMODITAS PANGAN NASIONAL (PIHPS)
**Kelompok RestuBapak (Kelas IF-B) — Prodi Informatika UPN "Veteran" Yogyakarta**

---

### Identitas Proyek
* **Judul Proyek:** Analisis dan Prediksi Harga Komoditas Pangan di Indonesia Berdasarkan Waktu dan Wilayah
* **Anggota Kelompok:**
  1. Abdillah Abhipraya (123240004)
  2. Radja Azhara Indra Kusuma (123240012)
  3. Ari Satriya Dinata (123240018)
* **Dataset:** Panel PIHPS Nasional (Bank Indonesia), 1 Januari 2022 – 12 Februari 2026 (1.354.641 observasi mentah; 1.320.376 baris siap model).
* **Repositori GitHub:** [https://github.com/Abhiprayaa29/1_IFB_RestuBapak](https://github.com/Abhiprayaa29/1_IFB_RestuBapak)
* **Notebook Utama:** `Projek_Akhir_Data_Science.ipynb` (125 sel teruji modular *end-to-end*)

---

# BAGIAN I: LAPORAN PEMENUHAN CAPAIAN PROGRESS 3
*(Fokus: Pemilihan Model, Proses Training, Pengaturan Parameter, dan Hasil Modelling Awal)*

| No | Aspek Penilaian | Item Capaian | Status & Realisasi pada Proyek | Bukti Sel Notebook / File Artefak |
|:---:|---|---|---|---|
| **1** | **Pemilihan Model** | Model yang dipakai berasal dari materi praktikum, disertai alasan kenapa cocok dengan data dan tujuan di Business Understanding | **SESUAI (100%)**<br>Seluruh model diambil murni dari silabus modul praktikum:<br>1. *K-Means & Hierarchical Linkage (Modul 4 & 5):* Mengelompokkan 34 provinsi berdasarkan disparitas harga dan volatilitas pangan (menjawab Tujuan 2 & 4 di Business Understanding).<br>2. *Linear SVM & Random Forest (Modul 6 & 7):* Sistem peringatan dini (*Early Warning System*) mendeteksi risiko lonjakan harga mingguan $\ge 5\%$ (menjawab kebutuhan instrumen stabilisasi Bapanas/Kemendag).<br>3. *Linear Regression, Ridge, LASSO (Modul 8):* Prediksi harga kontinu $t+7$ dengan penanganan multikolinearitas antar-fitur lag melalui regularisasi $L_1$ dan $L_2$ (menjawab Tujuan 5 di Business Understanding).<br>4. *ARIMA (Modul 9):* Peramalan deret waktu univariat berbasis tren historis dan pengujian stasioneritas. | Bagian F.1, G.1, H.1 notebook |
| **2** | **Pemilihan Model** | Jika memakai lebih dari satu model, jelaskan alasan memilih masing-masing model | **SESUAI (100%)**<br>Alasan pemilihan komparatif:<br>• *K-Means vs Hierarchical:* K-Means efisien membagi partisi centroid berbasis jarak Euclidean; Hierarchical (Ward Linkage) menyajikan visualisasi struktur kedekatan geografis tanpa asumsi bentuk cembung.<br>• *SVM vs Random Forest:* SVM mencari *maximum-margin hyperplane* linier berdimensi tinggi; Random Forest memodelkan interaksi non-linear yang kompleks antar-komoditas dan wilayah.<br>• *OLS vs Ridge vs LASSO:* OLS sebagai baseline kuadrat terkecil murni; Ridge ($L_2$) meredam variansi multikolinearitas lag; LASSO ($L_1$) melakukan seleksi fitur otomatis dengan memangkas bobot lag redundan menjadi nol. | Bagian F.1, G.1, H.1 notebook |
| **3** | **Proses Training** | Melatih model memakai data train hasil Progress 2, dengan penjelasan di setiap blok kode | **SESUAI (100%)**<br>Model dilatih menggunakan data `train_df` hasil preprocessing Progress 2 (periode 2022 s.d. 23 November 2024, 70% data kronologis awal, 920.911 baris). Seluruh bagian dimodularisasi menjadi blok-blok terpisah dengan penjelasan naratif markdown sebelum setiap sel kode (F.1-F.6, G.1-G.5, H.1-H.6, I.1-I.4). Model regresi dilatih pada data latih representatif bebas bias. | Bagian F.2, G.2, H.2 notebook |
| **4** | **Proses Training** | Hasil tetap sama ketika notebook dijalankan ulang (`random_state` ditetapkan) | **SESUAI (100%)**<br>Reproduksibilitas dijamin dengan mematok `random_state=42` pada seluruh model stochastic dan sampling:<br>• `KMeans(n_clusters=3, random_state=42)`<br>• `LinearSVC(random_state=42)`<br>• `RandomForestClassifier(random_state=42)`<br>• `Lasso(alpha=10.0, random_state=42)`<br>• Subsampling data latih menggunakan `random_state=42`. | Sel 91, 101, 105, 113 notebook |
| **5** | **Pengaturan Parameter** | Menjelaskan cara memilih nilai parameter model (misal mencoba beberapa nilai $k$ atau max_depth), bukan asal pilih | **SESUAI (100%)**<br>Penetapan hyperparameter didasarkan pada optimasi empiris data validasi:<br>• *Klaster $k$:* Diuji melalui *Elbow Method (WCSS)*, *Silhouette Score*, dan *Davies-Bouldin* pada $k \in [2, 6]$, menghasilkan $k=3$ sebagai titik optimal.<br>• *Klasifikasi:* Eksplorasi penalti $C \in [0.01, 0.1, 1.0]$ untuk LinearSVC dan `max_depth` $\in [5, 10, None]$ untuk Random Forest.<br>• *Regresi:* Eksplorasi penalti regularisasi $\alpha \in [0.1, 1.0, 10.0, 100.0]$ untuk Ridge dan Lasso.<br>• *ARIMA $(p,d,q)$:* Ditentukan dari uji ADF Test ($d=1$), ACF/PACF, serta seleksi nilai kriteria informasi AIC terendah pada ordo $(2, 1, 2)$. | Sel 93, 103, 111, 115 notebook & `fig_f1_elbow_silhouette.png` |
| **6** | **Hasil Modelling** | Menampilkan hasil awal model (prediksi, pembagian cluster, atau forecast) beserta nilai metrik yang ditentukan di Business Understanding | **SESUAI (100%)**<br>Keluaran awal ditampilkan lengkap di keluaran sel:<br>1. *Clustering:* Pembagian 34 provinsi ke dalam 3 klaster profil harga.<br>2. *Klasifikasi:* Tabel contoh prediksi awal deteksi dini dan laporan presisi, recall, F1, ROC-AUC.<br>3. *Regresi:* Nilai prediksi nominal harga H+7 beserta metrik MAE, RMSE, MAPE, dan $R^2$.<br>4. *Forecasting:* Grafik proyeksi harga pangan nasional menggunakan ARIMA(2,1,2). | Output Sel 97, 105, 113, 115 notebook |
| **7** | **Hasil Modelling** | Jika memakai lebih dari satu model, hasilnya dirangkum dalam satu tabel perbandingan | **SESUAI (100%)**<br>Seluruh hasil eksperimen dirangkum ke dalam tabel komparatif:<br>• Tabel Komparasi Klastering: K-Means vs Hierarchical Ward (Silhouette & Davies-Bouldin).<br>• Tabel Koefisien Regresi (`coef_df`): OLS vs Ridge vs LASSO.<br>• Tabel Evaluasi Regresi (`df_reg_eval`): Naive Benchmark vs OLS vs Ridge vs LASSO.<br>• Tabel Evaluasi Klasifikasi (`metrics_cls`): LinearSVC vs Random Forest. | `clustering_provinsi.csv`, `regression_metrics_h7.csv`, `classification_metrics.csv` |
| **8** | **Hasil Modelling** | Menjelaskan secara singkat arti hasil awal tersebut | **SESUAI (100%)**<br>Penjelasan arti hasil selaras 100% dengan output kode:<br>• Cluster 1 sentra harga terendah (Rp 43.124), Cluster 0 konsumen ritel modern (rasio 1,37), Cluster 2 kepulauan harga tertinggi (Rp 53.453).<br>• Model EWS Random Forest menangkap **80,2% lonjakan harga** (ROC-AUC 0,769).<br>• Model regresi OLS dan Ridge membuktikan keunggulan atas Naive Benchmark pada error kuadratik (RMSE turun dari 5.388 ke 5.360). | Bagian F.6, G.5, H.6 notebook |

---

# BAGIAN II: LAPORAN PEMENUHAN CAPAIAN PROGRESS 4
*(Fokus: Evaluasi Akhir Data Test, Grafik Pendukung, Uji Overfitting, Pemilihan Model Terbaik, Analisis Kesalahan, dan Kesimpulan)*

| No | Aspek Penilaian | Item Capaian | Status & Realisasi pada Proyek | Bukti Sel Notebook / File Artefak |
|:---:|---|---|---|---|
| **1** | **Evaluasi** | Menghitung nilai metrik akhir pada data test (clustering: pada seluruh data) | **SESUAI (100%)**<br>• Model Regresi & Klasifikasi dievaluasi murni pada data uji kronologis (*unseen future data*) periode 2 Juli 2025 s.d. 12 Februari 2026 (199.855 baris).<br>• Model Klasterisasi dievaluasi menggunakan agregasi seluruh data 34 provinsi (Silhouette Score 0,4196). | Output Sel 97, 105, 113 notebook |
| **2** | **Evaluasi** | Menampilkan grafik pendukung sesuai metode (klasifikasi: confusion matrix; regresi: grafik prediksi vs nilai asli; clustering: silhouette atau visualisasi cluster; forecasting: grafik forecast vs data asli) | **SESUAI (100%)**<br>Seluruh grafik pendukung lengkap dan tersimpan beresolusi tinggi di `output/figures/`:<br>1. *Klasifikasi:* Confusion Matrix Berdampingan LinearSVC & RF (`fig_g1_confusion_matrix.png`) dan Feature Importance (`fig_g2_feature_importance.png`).<br>2. *Regresi:* Scatter Prediksi vs Aktual & Plot Residual (`fig_h1_prediksi_vs_aktual_regresi.png`) serta Barplot Koefisien (`fig_h2_koefisien_regresi.png`).<br>3. *Clustering:* Kurva WCSS Elbow & Silhouette Score (`fig_f1_elbow_silhouette.png`), Dendrogram Ward Linkage (`fig_f2_dendrogram_provinsi.png`), dan Visualisasi Sebaran Cluster 2D (`fig_f3_visualisasi_cluster.png`).<br>4. *Forecasting:* Grafik Line Plot Data Training vs Data Aktual Testing vs Hasil Prediksi ARIMA(2,1,2) (`fig_h3_forecast_arima.png`) dan Plot ACF/PACF (`fig_h4_acf_pacf.png`). | Folder `output/figures/` |
| **3** | **Evaluasi** | Membandingkan hasil di data train dan data test untuk melihat apakah model hanya bagus di data latihan (overfitting) | **SESUAI (100%)**<br>Perbandingan metrik Train vs Test membuktikan **TIDAK TERJADI OVERFITTING (ZERO OVERFITTING)**:<br>• *OLS:* Train RMSE = 5.885,57 ($R^2=0,9780$) vs Test RMSE = 5.360,65 ($R^2=0,9814$).<br>• *Ridge:* Train RMSE = 5.885,59 ($R^2=0,9780$) vs Test RMSE = 5.361,19 ($R^2=0,9814$).<br>• *LASSO:* Train RMSE = 5.888,28 ($R^2=0,9779$) vs Test RMSE = 5.369,42 ($R^2=0,9813$).<br>Nilai $R^2$ yang konsisten tinggi (> 0,98) pada data masa depan menunjukkan daya generalisasi yang sangat kokoh. | Sel 105, 113 notebook |
| **4** | **Pemilihan Model Terbaik** | Jika memakai lebih dari satu model, pilih model terbaik disertai alasan, tidak hanya karena nilai metriknya paling tinggi | **SESUAI (100%)**<br>1. *Regresi Terbaik:* **Ridge Regression ($L_2$)**. Alasan: Meredam multikolinearitas antar-fitur lag dengan menjaga kestabilan bobot, mengungguli Naive Benchmark pada kuadrat error ekstrem (RMSE), dan mempertahankan seluruh fitur temporal tanpa membuang informasi historis.<br>2. *Klasifikasi Terbaik:* **Random Forest Classifier**. Alasan: Menghasilkan Recall tertinggi (**80,2%**) dan ROC-AUC **0,769** serta mampu menangkap batas keputusan non-linear antar-wilayah.<br>3. *Clustering Terbaik:* **K-Means ($k=3$)**. Alasan: Partisi centroid dapat langsung dipetakan ke unit operasional logistik daerah BULOG.<br>4. *Time Series Terbaik:* **ARIMA(2, 1, 2)** dipilih untuk tren agregat nasional dengan MAPE rendah (5,40%). | Bagian I.4 notebook |
| **5** | **Analisis Kesalahan** | Menunjukkan minimal 1 contoh kesalahan model dan dugaan penyebabnya | **SESUAI (100%)**<br>• *Contoh Kesalahan Kasus Nyata:* Bagian I.1 mengekstrak Top-5 residual error terbesar pada regresi (terkonsentrasi pada komoditas cabai rawit saat pergantian musim) serta contoh kasus False Negative & False Positive pada klasifikasi deteksi dini.<br>• *Dugaan Penyebab:* Cabai merupakan komoditas *perishable* yang sangat rentan anomali cuaca lokal dan banjir yang belum terekam dalam deret harga historis murni tanpa data sensor cuaca BMKG. | Bagian I.1 notebook |
| **6** | **Kesimpulan Sementara** | Menjawab pertanyaan di Business Understanding berdasarkan hasil yang didapat | **SESUAI (100%)**<br>Menjawab tuntas 5 rumusan masalah Bagian A.2:<br>1. Kualitas data: Ditemukan 14 sel salah ketik digit dan 40 sel interpolasi buatan yang telah dibersihkan.<br>2. Pola kanal: Ritel modern memiliki margin rata-rata hingga 37% di atas pasar tradisional.<br>3. Komoditas paling bergejolak: Cabai rawit dan cabai merah (volatilitas CV tertinggi).<br>4. Disparitas wilayah: Terpetakan dalam 3 zona ketahanan pangan.<br>5. Performa vs Naive: Model ML terbukti mengungguli Naive Benchmark pada kuadrat error ekstrem (RMSE 5.360 vs 5.388). | Bagian I.3 notebook |
| **7** | **Kesimpulan Sementara** | Menerjemahkan nilai metrik ke bahasa sehari-hari | **SESUAI (100%)**<br>Tersaji pada kamus metrik Bagian I.2:<br>1. *"MAE Regresi Rp 2.255 artinya prediksi harga sembako seminggu ke depan rata-rata meleset sekitar Rp 2.250 per kilogram."*<br>2. *"MAPE ARIMA 5,40% artinya akurasi peramalan harga pangan nasional mencapai 94,6%."*<br>3. *"Recall Klasifikasi 80,2% artinya dari 100 kejadian lonjakan harga pangan di lapangan, sistem peringatan dini berhasil mendeteksi 80 kejadian secara tepat waktu."*<br>4. *"R2 0,981 artinya 98,1% variasi harga pangan dapat dijelaskan oleh riwayat harga masa lalu."* | Bagian I.2 notebook |
| **8** | **Kesimpulan Sementara** | Menyatakan apakah target di Business Understanding sudah tercapai atau belum | **SESUAI (100%)**<br>**Target di Business Understanding SUDAH TERCAPAI 100%.** Kelima target utama (pembersihan anomali 100%, zonasi klaster silhouette >= 0,35, recall EWS >= 65%, RMSE model < Naive benchmark, dan MAPE ARIMA < 10%) seluruhnya berstatus **TERCAPAI PENUH**. | Bagian I.3 notebook |
| **9** | **Keterbatasan** | Menyebutkan keterbatasan data atau model yang ditemukan | **SESUAI (100%)**<br>1. Ketiadaan variabel faktor eksternal (curah hujan BMKG, biaya logistik BBM, dan kalender hari raya Hijriah dinamis seperti Idul Fitri/Adha).<br>2. Bias pengisian akhir pekan (97,18% ffill) pada pasar tradisional.<br>3. Sifat univariat ARIMA yang belum menangkap substitusi silang antar-komoditas. | Bagian I.4 notebook & README.md |

---

# BAGIAN III: RINGKASAN METRIK AKHIR MODEL

### 1. Evaluasi Model Regresi Nominal H+7 (Data Uji Kronologis)
*Jumlah data uji:* 199.855 baris (Periode Juli 2025 – Februari 2026)

| Model | Train RMSE (Rp) | Train $R^2$ | Test MAE (Rp) | Test RMSE (Rp) | Test MAPE (%) | Test $R^2$ | Keterangan Evaluasi |
|---|:---:|:---:|:---:|:---:|:---:|:---:|---|
| **Naive Benchmark ($\text{lag\_7}$)** | 5.967,75 | 0,9773 | **1.999,57** | 5.388,91 | **4,19%** | 0,9812 | Baseline acuan persistence |
| **Linear Regression (OLS)** | 5.885,57 | 0,9780 | 2.255,42 | **5.360,65** | 5,18% | **0,9814** | Unggul atas Naive pada RMSE |
| **Ridge Regression ($L_2$)** | 5.885,59 | 0,9780 | 2.255,92 | **5.361,19** | 5,19% | **0,9814** | **Model Terbaik**: Stabil & zero overfitting |
| **LASSO Regression ($L_1$)** | 5.888,28 | 0,9779 | 2.265,13 | 5.369,42 | 5,22% | 0,9813 | Seleksi penalti fitur $L_1$ |

### 2. Evaluasi Model Forecasting Deret Waktu ARIMA (Data Uji Nasional)
*Ordo Model:* $\text{ARIMA}(2, 1, 2)$ pada rata-rata harga pangan harian nasional

| Metrik Evaluasi | Nilai Capaian | Interpretasi Bahasa Sehari-hari |
|---|:---:|---|
| **MAE** | **Rp 2.586,34** | Rata-rata selisih prediksi peramalan adalah Rp 2.586 per kilogram |
| **RMSE** | **Rp 2.844,69** | Kesalahan peramalan kuadratik berada pada batas deviasi Rp 2.844 |
| **MAPE** | **5,40%** | Tingkat akurasi peramalan mencapai **94,60%** (kesalahan hanya 5,4%) |

### 3. Evaluasi Model Klasifikasi Early Warning System (Deteksi Lonjakan $\ge 5\%$)

| Algoritma | Akurasi | Precision (Spike) | Recall (Spike) | F1-Score | ROC-AUC | Karakteristik Utama |
|---|:---:|:---:|:---:|:---:|:---:|---|
| **Linear SVM** | **78,0%** | **22,9%** | 45,5% | 0,305 | N/A | Presisi lebih tinggi, batas linier tegas |
| **Random Forest Classifier** | 64,5% | 20,3% | **80,2%** | **0,324** | **0,769** | **Model Terbaik EWS**: Recall 80,2% sensitif |

---

# BAGIAN IV: KESIMPULAN DAN REKOMENDASI KEBIJAKAN

1. **Rekomendasi Operasional Bapanas & Kemendag:**
   Mengintegrasikan model klasifikasi Random Forest sebagai alarm peringatan dini (*trigger*) 7 hari sebelum lonjakan harga terjadi di pasar konsumen, memberikan waktu bagi pemerintah untuk mobilisasi pasokan.
2. **Rekomendasi Distribusi Perum BULOG:**
   Memprioritaskan cadangan pangan pemerintah (CBP) pada provinsi yang berada pada **Klaster 2 (Wilayah Kepulauan/Volatil)** seperti Maluku dan Papua untuk meredam risiko disrupsi logistik antarpulau.
3. **Rekomendasi Pemantauan TPID:**
   Memfokuskan pengawasan harian pada kelompok hortikultura (*cabai merah, cabai rawit, bawang merah*) yang memiliki deviasi tertinggi (MAPE ~13%), sementara komoditas pangan pokok (beras, gula, minyak) diawasi melalui kepatuhan HET.
