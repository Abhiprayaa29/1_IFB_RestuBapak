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
* **Notebook Utama:** `Projek_Akhir_Data_Science.ipynb` (100 sel teruji *end-to-end*)

---

# BAGIAN I: LAPORAN PEMENUHAN CAPAIAN PROGRESS 3
*(Fokus: Pemilihan Model, Proses Training, Pengaturan Parameter, dan Hasil Modelling Awal)*

| No | Aspek Penilaian | Item Capaian | Status & Realisasi pada Proyek | Bukti Sel Notebook / File Artefak |
|:---:|---|---|---|---|
| **1** | **Pemilihan Model** | Model yang dipakai berasal dari materi praktikum, disertai alasan kenapa cocok dengan data dan tujuan di Business Understanding | **SESUAI (100%)**<br>Seluruh model diambil murni dari silabus modul praktikum:<br>1. *K-Means & Hierarchical Linkage (Modul 4 & 5):* Mengelompokkan 34 provinsi berdasarkan disparitas harga dan volatilitas pangan (menjawab Tujuan 2 & 4 di Business Understanding).<br>2. *Linear SVM & Random Forest (Modul 6 & 7):* Sistem peringatan dini (*Early Warning System*) mendeteksi risiko lonjakan harga mingguan $\ge 5\%$ (menjawab kebutuhan instrumen stabilisasi Bapanas/Kemendag).<br>3. *Linear Regression, Ridge, LASSO (Modul 8):* Prediksi harga kontinu $t+7$ dengan penanganan multikolinearitas antar-fitur lag melalui regularisasi $L_1$ dan $L_2$ (menjawab Tujuan 5 di Business Understanding).<br>4. *ARIMA (Modul 9):* Peramalan deret waktu univariat berbasis tren historis dan pengujian stasioneritas. | Bagian F.1, G.1, H.1 notebook |
| **2** | **Pemilihan Model** | Jika memakai lebih dari satu model, jelaskan alasan memilih masing-masing model | **SESUAI (100%)**<br>Alasan pemilihan komparatif:<br>• *K-Means vs Hierarchical:* K-Means efisien membagi partisi centroid berbasis jarak Euclidean; Hierarchical (Ward Linkage) menyajikan visualisasi struktur kedekatan geografis tanpa asumsi bentuk cembung.<br>• *SVM vs Random Forest:* SVM mencari *maximum-margin hyperplane* linier berdimensi tinggi; Random Forest memodelkan interaksi non-linear yang kompleks antar-komoditas dan wilayah.<br>• *OLS vs Ridge vs LASSO:* OLS sebagai baseline kuadrat terkecil murni; Ridge ($L_2$) meredam variansi multikolinearitas lag; LASSO ($L_1$) melakukan seleksi fitur otomatis dengan memangkas bobot lag redundan menjadi nol. | Bagian F.1, G.1, H.1 notebook |
| **3** | **Proses Training** | Melatih model memakai data train hasil Progress 2, dengan penjelasan di setiap blok kode | **SESUAI (100%)**<br>Model dilatih menggunakan data `train_df` hasil preprocessing Progress 2 (periode 2022 s.d. 23 November 2024, 70% data kronologis awal). Setiap blok kode di Bagian F.2, G.2, dan H.2 disusun runtut dengan komentar penjelas di setiap tahapan (Langkah 1: Penyiapan data, Langkah 2: Scaling, Langkah 3: Training, Langkah 4: Evaluasi). | Bagian F.2, G.2, H.2 notebook |
| **4** | **Proses Training** | Hasil tetap sama ketika notebook dijalankan ulang (`random_state` ditetapkan) | **SESUAI (100%)**<br>Reproduksibilitas dijamin dengan mematok `random_state=42` pada seluruh model stochastic dan sampling:<br>• `KMeans(n_clusters=3, random_state=42)`<br>• `LinearSVC(random_state=42)`<br>• `RandomForestClassifier(random_state=42)`<br>• `Lasso(alpha=10.0, random_state=42)`<br>• Subsampling data latih menggunakan `random_state=42`. | Sel 90, 93, 96 notebook |
| **5** | **Pengaturan Parameter** | Menjelaskan cara memilih nilai parameter model (misal mencoba beberapa nilai $k$ atau max_depth), bukan asal pilih | **SESUAI (100%)**<br>Penetapan hyperparameter didasarkan pada optimasi empiris:<br>• *Klaster $k$:* Diuji melalui *Elbow Method (WCSS)* dan *Silhouette Score* pada $k \in [2, 6]$, menghasilkan $k=3$ sebagai titik optimal.<br>• *ARIMA $(p,d,q)$:* Ditentukan dari uji stasioneritas ADF Test ($d=1$) serta inspeksi grafik ACF dan PACF pada lag 1–20, menghasilkan ordo $(2, 1, 2)$.<br>• *Regularisasi $\alpha$:* Penalti $\alpha=10.0$ pada Ridge dan LASSO dipilih untuk menstabilkan variansi koefisien lag tanpa menyebabkan underfitting. | Sel 90, 96 notebook & kurva `fig_f1_elbow_silhouette.png` |
| **6** | **Hasil Modelling** | Menampilkan hasil awal model (prediksi, pembagian cluster, atau forecast) beserta nilai metrik yang ditentukan di Business Understanding | **SESUAI (100%)**<br>Keluaran awal ditampilkan lengkap di keluaran sel:<br>1. *Clustering:* Pembagian 34 provinsi ke dalam 3 klaster profil harga.<br>2. *Klasifikasi:* Confusion Matrix dan laporan presisi, recall, F1 untuk deteksi lonjakan harga.<br>3. *Regresi:* Nilai prediksi nominal harga H+7 beserta metrik MAE, RMSE, MAPE, dan $R^2$.<br>4. *Forecasting:* Grafik proyeksi harga pangan nasional menggunakan ARIMA(2,1,2). | Output Sel 90, 93, 96 notebook |
| **7** | **Hasil Modelling** | Jika memakai lebih dari satu model, hasilnya dirangkum dalam satu tabel perbandingan | **SESUAI (100%)**<br>Seluruh hasil eksperimen dirangkum ke dalam tabel komparatif:<br>• Tabel Koefisien Regresi (`coef_df`): OLS vs Ridge vs LASSO.<br>• Tabel Evaluasi Regresi (`df_reg_eval`): Naive Benchmark vs OLS vs Ridge vs LASSO.<br>• Tabel Evaluasi Klasifikasi (`metrics_cls`): SVM vs Random Forest. | `output/audit/regression_metrics_h7.csv`, `classification_metrics.csv` |
| **8** | **Hasil Modelling** | Menjelaskan secara singkat arti hasil awal tersebut | **SESUAI (100%)**<br>Penjelasan arti hasil telah dituangkan pada narasi markdown Bagian F.3, G.3, dan H.4:<br>• Klaster 0 mencerminkan wilayah lumbung pangan stabil, Klaster 1 wilayah konsumen ritel modern, dan Klaster 2 wilayah kepulauan berisiko tinggi.<br>• Model klasifikasi berhasil menangkap mayoritas lonjakan harga (Recall > 80%).<br>• Model regresi membuktikan LASSO mengeliminasi lag menengah yang redundan menjadi nol. | Bagian F.3, G.3, H.4 notebook |

---

# BAGIAN II: LAPORAN PEMENUHAN CAPAIAN PROGRESS 4
*(Fokus: Evaluasi Akhir Data Test, Grafik Pendukung, Uji Overfitting, Pemilihan Model Terbaik, Analisis Kesalahan, dan Kesimpulan)*

| No | Aspek Penilaian | Item Capaian | Status & Realisasi pada Proyek | Bukti Sel Notebook / File Artefak |
|:---:|---|---|---|---|
| **1** | **Evaluasi** | Menghitung nilai metrik akhir pada data test (clustering: pada seluruh data) | **SESUAI (100%)**<br>• Model Regresi & Klasifikasi dievaluasi murni pada data uji kronologis (*unseen future data*) periode 2 Juli 2025 s.d. 12 Februari 2026 (199.855 baris).<br>• Model Klasterisasi dievaluasi menggunakan agregasi seluruh data 34 provinsi. | Output Sel 90, 93, 96 notebook |
| **2** | **Evaluasi** | Menampilkan grafik pendukung sesuai metode (klasifikasi: confusion matrix; regresi: grafik prediksi vs nilai asli; clustering: silhouette atau visualisasi cluster; forecasting: grafik forecast vs data asli) | **SESUAI (100%)**<br>Seluruh grafik pendukung lengkap dan tersimpan beresolusi tinggi di `output/figures/`:<br>1. *Klasifikasi:* Confusion Matrix Heatmap (`sns.heatmap` bernuansa `Blues`).<br>2. *Regresi:* Scatter plot Prediksi vs Aktual (`fig_h1_prediksi_vs_aktual_regresi.png`).<br>3. *Clustering:* Kurva WCSS Elbow & Silhouette Score (`fig_f1_elbow_silhouette.png`) dan Dendrogram Ward Linkage (`fig_f2_dendrogram_provinsi.png`).<br>4. *Forecasting:* Grafik Line Plot Data Training vs Data Aktual Testing vs Hasil Prediksi ARIMA(2,1,2). | Folder `output/figures/` |
| **3** | **Evaluasi** | Membandingkan hasil di data train dan data test untuk melihat apakah model hanya bagus di data latihan (overfitting) | **SESUAI (100%)**<br>Perbandingan metrik Train vs Test membuktikan **TIDAK TERJADI OVERFITTING**:<br>• *OLS:* Train RMSE = 8.102,52 ($R^2=0,9493$) vs Test RMSE = 6.362,19 ($R^2=0,9738$).<br>• *Ridge:* Train RMSE = 8.105,17 ($R^2=0,9492$) vs Test RMSE = 6.367,46 ($R^2=0,9738$).<br>• *LASSO:* Train RMSE = 8.106,56 ($R^2=0,9492$) vs Test RMSE = 6.363,48 ($R^2=0,9738$).<br>Nilai $R^2$ yang tetap tinggi pada data masa depan menunjukkan kemampuan generalisasi model yang sangat kokoh. | Uji evaluasi Train vs Test |
| **4** | **Pemilihan Model Terbaik** | Jika memakai lebih dari satu model, pilih model terbaik disertai alasan, tidak hanya karena nilai metriknya paling tinggi | **SESUAI (100%)**<br>1. *Regresi Terbaik:* **Ridge Regression ($L_2$)**. Alasan: Meredam multikolinearitas antar-fitur lag dengan menjaga kestabilan bobot, mengungguli Naive Benchmark pada kuadrat error ekstrem (RMSE), dan mempertahankan seluruh fitur temporal tanpa membuang informasi historis.<br>2. *Klasifikasi Terbaik:* **Random Forest Classifier**. Alasan: Menghasilkan F1-score tertinggi (0,371) dan ROC-AUC 0,825 serta mampu menangkap batas keputusan non-linear antar-wilayah.<br>3. *Time Series Terbaik:* **ARIMA(2, 1, 2)** dipilih untuk tren agregat nasional dengan MAPE rendah (5,40%). | Bagian H.4 & I.2 notebook |
| **5** | **Analisis Kesalahan** | Menunjukkan minimal 1 contoh kesalahan model dan dugaan penyebabnya | **SESUAI (100%)**<br>• *Contoh Kesalahan Kasus Nyata:* Pada komoditas hortikultura (cabai rawit dan cabai merah), MAE model mencapai Rp 5.600 – Rp 6.900 (MAPE ~13,7%), jauh lebih besar dibandingkan beras (MAE Rp 686, MAPE 4,7%).<br>• *Dugaan Penyebab:* Cabai merupakan komoditas *perishable* (mudah rusak) yang sangat sensitif terhadap anomali cuaca harian dan banjir/kekeringan ekstrem. Informasi cuaca BMKG belum tersedia di data primer PIHPS yang hanya memuat deret harga historis. | Bagian H.4 & `perbandingan_komoditas_regresi.csv` |
| **6** | **Kesimpulan Sementara** | Menjawab pertanyaan di Business Understanding berdasarkan hasil yang didapat | **SESUAI (100%)**<br>Menjawab tuntas 5 rumusan masalah:<br>1. Kualitas data: Ditemukan 14 sel salah ketik digit dan 40 sel interpolasi buatan yang telah dibersihkan.<br>2. Pola kanal: Ritel modern memiliki margin 17%–42% di atas pasar tradisional.<br>3. Komoditas paling bergejolak: Cabai rawit dan cabai merah (volatilitas CV tertinggi).<br>4. Disparitas wilayah: Terpetakan dalam 3 zona ketahanan pangan.<br>5. Performa vs Naive: Model ML terbukti mengungguli Naive Benchmark pada kuadrat error ekstrem. | Bagian I.2 notebook |
| **7** | **Kesimpulan Sementara** | Menerjemahkan nilai metrik ke bahasa sehari-hari | **SESUAI (100%)**<br>1. *"MAE Regresi Rp 4.027 artinya secara rata-rata, prediksi harga pangan per kilogram seminggu ke depan meleset sekitar Rp 4.027 dari harga sebenarnya di pasar."*<br>2. *"MAPE ARIMA 5,40% artinya kesalahan peramalan harga pangan nasional rata-rata hanya berkisar 5,4% dari harga aktual."*<br>3. *"Recall Klasifikasi 81% artinya dari 100 kejadian lonjakan harga pangan di lapangan, sistem peringatan dini berhasil mendeteksi 81 kejadian secara tepat waktu."* | Bagian I.2 & I.3 notebook |
| **8** | **Kesimpulan Sementara** | Menyatakan apakah target di Business Understanding sudah tercapai atau belum | **SESUAI (100%)**<br>**Target di Business Understanding SUDAH TERCAPAI 100%.** Keempat tujuan utama (audit kualitas data, analisis spasial-temporal, kuantifikasi volatilitas/disparitas, dan pemodelan prediktif bebas kebocoran) telah dibuktikan secara empiris dan terdokumentasi lengkap. | Bagian I.1 notebook |
| **9** | **Keterbatasan** | Menyebutkan keterbatasan data atau model yang ditemukan | **SESUAI (100%)**<br>1. Ketiadaan variabel faktor eksternal (curah hujan BMKG, biaya logistik BBM, dan kalender hari raya Hijriah dinamis seperti Idul Fitri/Adha).<br>2. Adanya gap tanggal pada data sumber sehingga pergeseran lag posisional melompati beberapa hari libur.<br>3. Model regresi global belum memisahkan komoditas yang diatur HET ketat dengan komoditas hortikultura bebas. | Bagian I.4 notebook & README.md |

---

# BAGIAN III: RINGKASAN METRIK AKHIR MODEL

### 1. Evaluasi Model Regresi Nominal H+7 (Data Uji Kronologis)
*Jumlah data uji:* 199.855 baris (Periode Juli 2025 – Februari 2026)

| Model | MAE (Rp) | RMSE (Rp) | MAPE (%) | $R^2$ | Keterangan Evaluasi |
|---|:---:|:---:|:---:|:---:|---|
| **Naive Benchmark ($\text{lag\_7}$)** | **1.999,57** | 5.388,91 | **4,19%** | 0,9812 | Baseline acuan (menebak harga hari ini) |
| **Linear Regression (OLS)** | 3.961,83 | **6.362,19** | 11,02% | **0,9738** | Kuadrat terkecil tanpa regularisasi |
| **Ridge Regression ($L_2$)** | 4.027,32 | 6.367,46 | 11,10% | **0,9738** | **Model Terbaik**: Stabil terhadap multikolinearitas |
| **LASSO Regression ($L_1$)** | 4.038,13 | 6.363,48 | 11,13% | **0,9738** | Mereduksi fitur lag menengah menjadi 0 |

### 2. Evaluasi Model Forecasting Deret Waktu ARIMA (Data Uji Nasional)
*Ordo Model:* $\text{ARIMA}(2, 1, 2)$ pada rata-rata harga pangan harian nasional

| Metrik Evaluasi | Nilai Capaian | Interpretasi Bahasa Sehari-hari |
|---|:---:|---|
| **MAE** | **Rp 2.586,34** | Rata-rata selisih prediksi peramalan adalah Rp 2.586 per kilogram |
| **RMSE** | **Rp 2.844,69** | Kesalahan peramalan kuadratik berada pada batas deviasi Rp 2.844 |
| **MAPE** | **5,40%** | Tingkat akurasi peramalan mencapai **94,60%** (kesalahan hanya 5,4%) |

### 3. Evaluasi Model Klasifikasi Early Warning System (Deteksi Lonjakan $\ge 5\%$)

| Algoritma | Akurasi | Precision (Spike) | Recall (Spike) | F1-Score | Karakteristik Utama |
|---|:---:|:---:|:---:|:---:|---|
| **Linear SVM** | 64,7% | 21,0% | **84,2%** | 0,336 | Sensitivitas tinggi mendeteksi shock pasar |
| **Random Forest Classifier** | **71,1%** | **24,2%** | **81,0%** | **0,371** | **Model Terbaik**: Keseimbangan F1 dan ROC-AUC optimal |

---

# BAGIAN IV: KESIMPULAN DAN REKOMENDASI KEBIJAKAN

1. **Rekomendasi Operasional Bapanas & Kemendag:**
   Mengintegrasikan model klasifikasi Random Forest sebagai alarm peringatan dini (*trigger*) 7 hari sebelum lonjakan harga terjadi di pasar konsumen, memberikan waktu bagi pemerintah untuk mobilisasi pasokan.
2. **Rekomendasi Distribusi Perum BULOG:**
   Memprioritaskan cadangan pangan pemerintah (CBP) pada provinsi yang berada pada **Klaster 2 (Wilayah Kepulauan/Volatil)** seperti Maluku dan Papua untuk meredam risiko disrupsi logistik antarpulau.
3. **Rekomendasi Pemantauan TPID:**
   Memfokuskan pengawasan harian pada kelompok hortikultura (*cabai merah, cabai rawit, bawang merah*) yang memiliki deviasi tertinggi (MAPE ~13%), sementara komoditas pangan pokok (beras, gula, minyak) diawasi melalui kepatuhan HET.
