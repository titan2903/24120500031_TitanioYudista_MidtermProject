# Midterm Predictive Analytics — Bank Marketing Term Deposit Prediction

**Titanio Yudista · NIM: 24120500031 · Cakrawala University**

Mata Kuliah: *Predictive Analytics* (Ujian Tengah Semester)

---

## 📌 Ringkasan Proyek

Repositori ini memuat alur kerja analitik prediktif menyeluruh (*end-to-end predictive analytics workflow*) untuk memprediksi apakah catatan kampanye/kontak akan berakhir dengan langganan deposito berjangka (*term deposit*; target `y=yes` atau `no`). Satu baris adalah catatan terkait kampanye/kontak, bukan identitas nasabah unik.

Dataset yang digunakan bersumber dari **UCI Machine Learning Repository** varian resmi `bank-additional-full.csv` (41.188 baris × 21 kolom), yang mencakup atribut profil demografi nasabah, riwayat kontak kampanye, serta indikator makroekonomi sosial.

---

## 📂 Struktur Repositori

Struktur berkas utama proyek saat ini untuk [repository GitHub](https://github.com/titan2903/24120500031_TitanioYudista_MidtermProject):

```text
.
├── .gitignore
├── README.md
├── requirements.txt
├── midterm_predictive_analytics.ipynb
├── executive_summary.pdf
├── github_proof.pdf
├── images/
│   └── screenshot_github.png
└── data/
    ├── bank-additional-full.csv
    ├── bank-additional-names.txt
    └── source_metadata.json
```

Dataset CSV dan dokumentasi varian additional disertakan langsung. Notebook memuat output tabel dan grafik; `executive_summary.pdf` berisi ringkasan eksekutif dua halaman. `github_proof.pdf` dibuat dari `images/screenshot_github.png` sebagai bukti unggahan ke GitHub. Kedua berkas bukti GitHub tersebut masih perlu di-commit dan di-push agar muncul di repository publik.

---

## ⚙️ Cara Menjalankan Proyek (*Reproducibility*)

### A. Lokal

1. Clone repository dengan URL yang dapat digunakan tanpa konfigurasi alias SSH khusus:

   ```bash
   git clone https://github.com/titan2903/24120500031_TitanioYudista_MidtermProject.git
   cd 24120500031_TitanioYudista_MidtermProject
   ```

2. Gunakan Python 3.12 jika tersedia, lalu instal dependensi:

   ```bash
   python -m pip install -r requirements.txt
   ```

3. Buka `midterm_predictive_analytics.ipynb` dari **root repository** dan jalankan *Restart Kernel / Runtime → Run All*. Path dataset relatif terhadap direktori kerja. Seluruh hasil analisis baru akan dibuat ulang saat notebook dijalankan.

### B. Google Colab

Clone repository ke runtime Colab, pindah ke root repository dengan `%cd /content/24120500031_TitanioYudista_MidtermProject`, instal `requirements.txt`, lalu jalankan semua sel notebook. Keberhasilan Run All **sudah diverifikasi lokal**, tetapi eksekusi di Colab belum diverifikasi.

---

## 🔬 Metodologi & Alur Kerja Analitik

Notebook mengikuti 11 bagian wajib: Project Overview; Dataset Description & Source; Data Quality Assessment; Data Preparation; EDA; Feature Engineering; Baseline Model; Model Training; Model Evaluation; Feature Importance; serta Conclusion & Recommendations.

- Dua belas baris duplikat identik dihapus pada 21 kolom asli. Nilai kategori `unknown` dipertahankan. Sentinel `pdays=999` diubah menjadi indikator pernah dihubungi dan `pdays_clean`.
- `duration` dikeluarkan dari prediktor karena durasi panggilan belum tersedia sebelum kontak. Fitur `balance` milik varian klasik tidak ada pada dataset additional ini.
- Profil prediktor identik dijaga dalam grup yang sama. Fold pertama `StratifiedGroupKFold` menjadi holdout; lima fold baru pada development dipakai sama untuk semua model.
- Imputasi, scaling, dan one-hot encoding di-fit **di dalam pipeline pada data fit fold saja**. Model yang dibandingkan: DummyClassifier, Logistic Regression, Random Forest, dan XGBoost.
- Model dipilih menggunakan mean F1 kelas positif dari CV pada threshold 0,5. Holdout dipakai untuk laporan akhir. Selain F1, notebook melaporkan precision, recall, accuracy, ROC-AUC, dan **AP (*Average Precision*)**.
- Importance model terpilih berasal dari `feature_importances_` Random Forest. Ini ukuran prediktif berbasis penurunan impurity, bukan bukti sebab akibat; notebook tidak menghitung permutation importance.

---

## 📊 Hasil Utama Pemodelan

Hasil yang tersimpan dalam notebook (dibulatkan tiga desimal):

| Model | Mean CV F1 | Test F1 | Test Precision | Test Recall | Test ROC-AUC | Test AP |
| :--- | ---: | ---: | ---: | ---: | ---: | ---: |
| Dummy | 0,000 | 0,000 | 0,000 | 0,000 | 0,500 | 0,113 |
| Logistic Regression | 0,453 | 0,451 | 0,354 | 0,620 | 0,796 | 0,448 |
| **Random Forest (terpilih)** | **0,493** | **0,485** | **0,417** | **0,581** | **0,786** | **0,446** |
| XGBoost | 0,467 | 0,463 | 0,366 | 0,629 | 0,807 | 0,474 |

Random Forest dipilih karena mean CV F1 tertinggi. Pada holdout, XGBoost memberi recall dan AP lebih tinggi, sedangkan Random Forest memberi precision dan F1 lebih tinggi. Hasil holdout ini **tidak** dipakai untuk memilih ulang model. Baseline Dummy memperlihatkan mengapa akurasi mayoritas yang tinggi tidak cukup: F1 dan recall kelas positifnya nol.

---

## 📚 Sitasi Sumber Data

```bibtex
@misc{moro2014bank,
  author       = {Moro, Sérgio and Rita, Paulo and Cortez, Paulo},
  title        = {{Bank Marketing Dataset}},
  year         = {2014},
  howpublished = {UCI Machine Learning Repository},
  note         = {{DOI: 10.24432/C5K306}}
}
```
