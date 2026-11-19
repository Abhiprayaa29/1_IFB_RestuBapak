# Anggota Kelompok

Anggota Kelompok :
Abdillah Abhipraya (123240004)  •  Radja Azhara Indra Kusuma (123240012)
Ari Satriya Dinata (123240018)  

## Status

Notebook sudah dieksekusi ulang end-to-end tanpa galat pada Python 3.13.2, kernel `python3`, dengan
`nbclient` (tanpa `allow_errors`). Notebook kini memuat 100 sel yang mencakup seluruh materi
silabus Praktikum Data Science (Modul I sampai IX) secara lengkap hingga Bagian I (Kesimpulan dan
Rekomendasi Kebijakan). Seluruh keluaran, tabel, dan gambar tersimpan di dalam notebook serta berkas
audit/visualisasi terkait di folder `output/`.

Jalankan ulang dari folder ini agar path relatif `data_komoditas_pangan/` dan `output/` tetap valid.

### Aktivasi Virtual Environment (`.venv`):

Virtual environment telah dibuat di direktori `.venv/` dan didaftarkan sebagai kernel Jupyter:

- **PowerShell:**
  ```powershell
  .\.venv\Scripts\Activate.ps1
  ```
- **Command Prompt (CMD):**
  ```cmd
  .\.venv\Scripts\activate.bat
  ```
- **Di VS Code / Jupyter Notebook:**
  Pilih kernel: **`Python (RestuBapak IFB)`** atau arahkan interpreter ke `D:\Projects\1_IFB_RestuBapak\.venv\Scripts\python.exe`.

### Eksekusi Ulang Notebook (*Headless* / Otomatis):

```bash
.\.venv\Scripts\python.exe - <<'PY'
import nbformat
from nbclient import NotebookClient

path = "Projek_Akhir_Data_Science.ipynb"
nb = nbformat.read(path, as_version=4)
NotebookClient(nb, timeout=1800, kernel_name="restubapak-env").execute()
nbformat.write(nb, path)
print("Eksekusi selesai tanpa galat!")
PY
```

## Sumber data

Data diunduh dari Kaggle (`muhyusuf1112/indonesia-commodity-price-based-piphs-source`, pemilik
Muh.Yusuf; versi 2) dan disimpan sebagai `archive.zip` berukuran 1.117.287 byte (1,12 MB)
terkompresi, dengan total isi tidak terkompresi 12.240.348 byte. Arsip berisi 31 entri: 30 berkas
CSV dan satu direktori `.ipynb_checkpoints` yang diabaikan. Metadata Kaggle mencantumkan lisensi
MIT; sumber asal yang dinyatakan adalah PIHPS Nasional, Bank Indonesia. Atribusi dan tautan
dicantumkan di Bagian B.1 notebook.

Struktur:

```
data_komoditas_pangan/{pasar_modern,pasar_tradisional,produsen}/
    Cleaned_After_Imputation/final_komoditas_<kanal>_<komoditas>_2022_2026.csv
```

Setiap berkas berbentuk lebar dengan kolom `Date_Param` dan 32 sampai 34 kolom provinsi. Periode
mencakup 1 Januari 2022 sampai 12 Februari 2026, yaitu 1.504 hari kalender. Tidak ada kolom satuan
di data sumber, sehingga satuan mengikuti komoditas: minyak goreng dalam Rp/liter, sembilan komoditas
lain dalam Rp/kg.

## Angka kunci dan buktinya

Semua angka di bawah dihasilkan oleh sel notebook dan dapat diperiksa langsung pada keluarannya.

| Metrik | Nilai | Lokasi bukti |
|---|---|---|
| Observasi pada panel | 1.354.641 | Bagian B.4 |
| Sebaran per kanal | 503.141 / 481.236 / 370.264 | Bagian B.4 |
| Deret (kanal x komoditas x provinsi) | 925 | Bagian E.3 |
| Sel bersisa desimal | 23.389 (1,7266%) | Bagian B.6 |
| Outlier IQR per kanal dan komoditas | 28.032 (2,0693%) | Bagian B.6 |
| Sel dikoreksi (orde besaran) | 14 sel pada 2 deret | Bagian D.3 |
| Sel dikarantina (ramp interpolasi) | 40 sel pada 1 deret | Bagian D.4 |
| Baris akhir pekan | 362.891 | Bagian B.8 |
| Akhir pekan identik hari sebelumnya | 352.668 (97,18%) | Bagian B.8 |
| Observasi Sabtu dengan Jumat tersedia | 183.533 | Bagian B.8 |
| Sabtu identik dengan Jumat | 178.540 (97,28%) | Bagian B.8 |
| Baris siap dilatih | 1.320.376 | Bagian E.3 |
| Himpunan latih / validasi / uji | 920.911 / 199.610 / 199.855 | Bagian E.5 |

## Aturan pembersihan

Nilai tidak dihapus dan tidak diimputasi ulang. Tiga perlakuan dipisahkan menurut tingkat keyakinan
terhadap kerusakan datanya.

**R1. Koreksi kesalahan orde besaran.** Sebuah sel dikoreksi hanya bila rasionya terhadap pangkat
sepuluh terdekat berada dalam 0,5%, dan nilai terkoreksinya berada dalam 5% median bergerak 15 hari
dari deret yang sama. Hasilnya 14 sel pada dua deret:

- `pasar_modern`, `daging_ayam`, `Kalimantan Utara`: 7 sel, 3 Januari sampai 9 Januari 2024, 3.500
  menjadi 35.000.
- `pasar_modern`, `daging_sapi`, `Gorontalo`: 7 sel, 5 Oktober sampai 11 Oktober 2022, 16.900 menjadi
  169.000.

**R2. Karantina ramp interpolasi.** Sebuah run ditandai bila selisihnya konstan dan bukan nol
selama minimal lima titik berurutan, dan rentangnya melebihi satu kali median deret. Hanya ada satu
run yang lolos: `pasar_modern`, `gula_pasir`, `Kalimantan Selatan`, 17 April sampai 28 Mei 2024, 40
sel, kenaikan 14.700 per hari dari 17.500 ke 605.500, lalu kembali ke 17.500. Rasio puncak terhadap
median deret 37,84. Nilai dikarantina, bukan diganti.

**R3. Penandaan tanpa mengubah nilai.** Sisa desimal, outlier IQR, dan akhir pekan yang menyalin hari
sebelumnya hanya diberi penanda (`is_desimal`, `is_outlier_iqr`, `is_ffill_akhir_pekan`). Nilai asli
tetap dipakai, karena ketiganya lebih mencerminkan cara data diproduksi daripada kesalahan
pengukuran.

Setelah R1 dan R2, tidak ada baris yang dihapus. Jumlah observasi tetap 1.354.641, dengan 40 nilai
kosong baru berasal dari karantina.

## Praproses

Target pemodelan adalah harga tujuh hari ke depan (H+7), bukan H+1. Alasannya terdokumentasi di
Bagian E.1: pada H+1, sekitar 27% target jatuh di akhir pekan dan bernilai salinan hari sebelumnya,
sehingga model bisa terlihat akurat tanpa belajar apa pun.

Lima belas fitur dibentuk per deret: `lag_7`, `lag_14`, `lag_30`, `ma_7`, `ma_14`, `ma_30`,
`std_7`, `std_30`, `month_sin`, `month_cos`, `dow_sin`, `dow_cos`, `masa_panen`, `bulan`, `minggu`.
Seluruh fitur harga digeser minimal tujuh hari.

Pemeriksaan kebocoran di Bagian E.4 memakai tiga uji:

1. Kesetaraan dengan baris kalender tepat t-7 dan t-14 bersifat deskriptif: 94,45% dan 90,44%.
   Angka ini tidak mencapai 100% karena grid tanggal pada data sumber tidak lengkap, sehingga
   pergeseran tujuh posisi kadang melompati hari yang tidak tersedia.
2. Uji keselamatan yang menentukan: tanggal sumber setiap fitur histori selalu minimal tujuh hari
   sebelum tanggal prediksi, untuk seluruh 1.348.126 baris pada `lag_7` dan 1.341.651 baris pada
   `lag_14`. Jarak minimum terukur tepat 7 dan 14 hari.
3. Tanggal target selalu minimal tujuh hari setelah tanggal prediksi. Median horizon 7 hari,
   persentil ke-95 9 hari, maksimum 51 hari, dan 89,61% baris berhorizon tepat 7 hari.

Konsekuensi yang diakui: karena pergeseran bersifat posisional, horizon efektif pada sebagian kecil
baris lebih panjang dari tujuh hari kalender. Untuk pemakaian yang menuntut horizon kalender ketat,
panel perlu dijahit ke kalender lengkap sebelum fitur dibentuk.

Pembagian data bersifat kronologis berdasarkan tanggal unik, dengan proporsi 70/15/15. Batas
latih-validasi jatuh pada 23 November 2024 dan batas validasi-uji pada 2 Juli 2025. Tidak ada tanggal
yang beririsan antarhimpunan, dan seluruh 1.456 tanggal terpakai.

## Perbedaan terhadap laporan asli

Perbedaan dicatat terbuka, bukan disembunyikan.

1. **Korelasi beras dan cabai merah.** Laporan menyebut sekitar -0,01. Perhitungan ulang memberi
   +0,0533 pada konstruksi yang sama (rata-rata nasional harian, pasar tradisional). Tiga konstruksi
   alternatif menghasilkan +0,7753 (rata-rata provinsi), +0,3070 (gabungan provinsi-hari), dan
   +0,0861 (selisih logaritmik harian). Tidak ada konstruksi yang menghasilkan nilai negatif
   mendekati nol. Keempat angka dihitung di sel Bagian C.6.1.
2. **Jumlah sel ramp interpolasi.** Versi awal analisis menyebut 173 sel. Dengan kriteria yang
   dinyatakan eksplisit, hanya satu run yang memenuhi syarat, yaitu 40 sel.
3. **Label pengisian akhir pekan.** Angka 183.533 adalah jumlah observasi hari Sabtu yang memiliki
   pasangan hari Jumat, bukan jumlah observasi Sabtu dan Minggu. Jumlah baris akhir pekan seluruhnya
   362.891, dan 352.668 di antaranya (97,18%) identik dengan hari sebelumnya.
4. **Target.** Laporan memakai H+1. Notebook ini memakai H+7 dengan alasan yang diuji, bukan
   diasumsikan.

## Berkas yang dihasilkan

```
output/figures/
  fig_b1_kelengkapan_provinsi.png
  fig_b2_pengisian_akhir_pekan.png
  fig_c1_harga_antarkanal.png
  fig_c2_pola_temporal.png
  fig_c3_volatilitas.png
  fig_c4_disparitas_wilayah.png
  fig_c5_korelasi.png
  fig_d1_koreksi_orde_besaran.png
  fig_d2_anomali_ramp.png
  fig_d3_sebelum_sesudah_cleaning.png
  fig_e1_fitur_vs_target_h7.png
  fig_f1_elbow_silhouette.png
  fig_f2_dendrogram_provinsi.png
  fig_h1_prediksi_vs_aktual_regresi.png

output/audit/
  data_inventory.csv
  data_quality_report.csv
  column_quality_report.csv
  deskriptif_komoditas_kanal.csv
  disparitas_wilayah.csv
  korelasi_komoditas.csv
  cleaning_log.csv
  anomali_orde_besaran.csv
  korelasi_fitur_target_h7.csv
  feature_selection_h7.csv
  kamus_data_final.csv
  clustering_provinsi.csv
  classification_metrics.csv
  regression_metrics_h7.csv
  perbandingan_komoditas_regresi.csv

output/processed/
  _panel_raw.parquet          # panel sebelum pembersihan, 1.354.641 baris
  pihps_siap_model.parquet    # 1.320.376 baris x 28 kolom, siap dilatih
  preprocessor_h7.joblib    # one-hot kategori + StandardScaler, fit pada train saja
```

`pihps_siap_model.parquet` berukuran sekitar 45 MB, sehingga tidak disertakan di repositori.
Berkas tersebut dihasilkan ulang setiap kali notebook dijalankan.

## Batasan

1. Satuan tidak tersedia di data sumber, sehingga perbandingan lintas komoditas mengandalkan
   asumsi satuan per komoditas dan tidak dilakukan konversi.
2. Koreksi R1 dan karantina R2 memakai ambang heuristik yang dinyatakan terbuka. Ambang tersebut
   tidak divalidasi terhadap catatan lapangan, karena data pembanding tidak tersedia.
3. Horizon efektif target bervariasi akibat grid tanggal yang tidak lengkap.
4. Model time series saat ini berfokus pada fitur autoregresif lag dan kalender; pengembangan
   lanjutan dapat memasukkan variabel hari raya keagamaan (Idul Fitri/Adha) dan anomali cuaca BMKG.

## Progress Presentasi

Lembar **Progress 1 - Presentasi Proyek Akhir Data Science** tersedia di `output/Progress_1_Presentasi_Proyek_Akhir_Data_Science.xlsx`. Seluruh **18 capaian** telah diselesaikan 100% dan ditautkan ke bukti notebook/output.

## Ringkasan Pemenuhan 18 Capaian

1. **Sumber dan lisensi:** Kaggle versi 2, pengunggah Muh.Yusuf; lisensi metadata MIT, sumber asal PIHPS/BI (Bagian B.1).
2. **Data dictionary:** tipe, makna, dan satuan tercatat (Bagian B.5; `output/audit/kamus_data_final.csv`).
3. **Kualitas data:** sumber mentah tidak memiliki nilai kosong dan duplikat kunci; audit outlier tersedia.
4–7. **EDA, representativitas, statistik, dan visualisasi:** Bagian B.7–B.8/C, termasuk grafik target H+7.
8. **Korelasi fitur-target:** dihitung di dalam tiap deret pada train; median `lag_7` 0,856, `ma_7` 0,832, `ma_14` 0,802; `lag_7` positif di 100% deret.
9. **Audit kolom:** nol duplikat kunci; 28.032 penanda outlier IQR; 40 missing baru setelah karantina (0,003% dari panel).
10–11. **Cleaning dan before-after:** 14 koreksi orde besaran, 40 nilai dikarantina, visual perbandingan tersedia.
12. **Preprocessing:** delapan fitur numerik, one-hot kategori, StandardScaler di-fit hanya pada train; imbalance tidak berlaku pada regresi kontinu, namun ditangani dengan `class_weight='balanced'` pada klasifikasi lonjakan harga.
13. **Split:** kronologis 70/15/15 pada tanggal unik, tanpa tumpang tindih.
14. **Feature selection:** delapan fitur dipilih menggunakan korelasi dalam-deret pada train; tabel bukti disimpan.
15. **Modeling:** Baseline Naive Persistence (`lag_7`), K-Means (Modul IV), Hierarchical Clustering (Modul V), SVM (Modul VI), Random Forest (Modul VII), serta Linear, Ridge, dan LASSO Regression (Modul VIII & IX).
16. **Evaluasi:** Evaluasi komparatif lengkap pada data uji kronologis (MAE, RMSE, MAPE, R2 untuk regresi; Accuracy, Precision, Recall, F1, ROC-AUC untuk klasifikasi EWS); evaluasi sliced per komoditas.
17. **Interpretasi:** Segmentasi 3 klaster provinsi (sentra vs konsumen vs kepulauan), analisis trade-off recall pada early warning system pangan, dan analisis penyebab performa model vs naive baseline pada komoditas stabil vs volatil.
18. **Presentasi:** Dokumentasi end-to-end lengkap, gambar beresolusi tinggi, lembar capaian proyek di Excel, dan siap dipresentasikan.
