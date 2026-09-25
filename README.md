# Midterm Predictive Analytics — Bank Marketing

**Titanio Yudista · 24120500031 · Cakrawala University**

Proyek ini memprediksi apakah catatan kampanye/kontak bank berakhir dengan langganan deposito (`y=yes`). Dataset yang dipakai hanya varian UCI `bank-additional-full.csv`. Semua artefak lokal dibuat dari data asli dan notebook yang disertakan.

## Menjalankan proyek

1. Gunakan Python 3.12.10 bila memungkinkan. Dari root folder proyek, buat environment sendiri dan jalankan `python -m pip install -r requirements.txt`.
2. Buka `midterm_predictive_analytics.ipynb` dari root folder proyek dengan kernel Python 3. Pilih **Restart Runtime → Run All**. Path di notebook relatif terhadap direktori kerja saat notebook dijalankan.
3. Dataset sudah ada di `data/`. Jika CSV atau dokumentasi additional tidak tersedia, sel setup mencoba mengunduh arsip resmi UCI hingga tiga kali. File asli tidak diubah oleh cleaning.
4. Untuk menjalankan tanpa antarmuka notebook: `python -m jupyter nbconvert --to notebook --execute --inplace --ExecutePreprocessor.timeout=1800 midterm_predictive_analytics.ipynb`.
5. Setelah notebook berhasil, buat ulang ringkasan: `python scripts/build_summary.py`. Script membaca hasil aktual dari `artifacts/` dan menulis `executive_summary.md` serta `executive_summary.pdf`.

Di workspace yang memakai perintah `rtk`, awali perintah shell dengan `rtk`; contoh: `rtk proxy python scripts/build_summary.py`.

Untuk Google Colab, unggah dan ekstrak **seluruh folder proyek** terlebih dahulu, pindahkan working directory ke root folder tersebut, instal `requirements.txt`, lalu buka notebook dan jalankan semua sel. Validasi Colab belum dilakukan.

## Sumber data

Moro, S., Rita, P., & Cortez, P. (2014). *Bank Marketing [Dataset]*. UCI Machine Learning Repository. DOI [10.24432/C5K306](https://doi.org/10.24432/C5K306). [Halaman dataset](https://archive.ics.uci.edu/dataset/222/bank+marketing). CSV versi additional full (41.188 baris, 21 kolom) berasal dari arsip resmi UCI. Waktu unduh dan checksum SHA-256 ada di `data/source_metadata.json`; dokumentasi varian ada di `data/bank-additional-names.txt`.

## Metode dan hasil

Setelah 12 duplikat penuh dihapus, 32.940 catatan masuk development dan 8.236 ke holdout. Profil prediktor identik dijaga dalam grup yang sama. Lima fold development membandingkan baseline mayoritas, Logistic Regression, Random Forest, dan XGBoost dengan preprocessing per fit fold. Metrik utama adalah F1 kelas positif pada threshold 0,5. Model dipilih dari CV sebelum holdout dibuka.

Random Forest terpilih dengan mean CV F1 **0,493**; pada holdout F1 **0,485**, precision **0,417**, recall **0,581**, ROC-AUC **0,786**, dan AP **0,446**. Angka lengkap ada di `artifacts/cv_results.csv` dan `artifacts/test_metrics.csv`. Ini evaluasi retrospektif pada satu bank/periode historis, bukan validasi masa depan. Tanpa ID nasabah, grup profil identik juga tidak menjamin pemisahan semua nasabah berulang.

## Struktur penting

- `midterm_predictive_analytics.ipynb`: laporan analitik 11 bagian dengan output tersimpan.
- `data/`: CSV asli, dokumentasi additional, dan metadata asal/checksum.
- `artifacts/`: manifest split, log cleaning, hasil CV, keputusan model, hasil test, prediksi, importance, metadata run, dan grafik.
- `executive_summary.md` dan `executive_summary.pdf`: ringkasan manajerial dua halaman dari hasil aktual.
- `scripts/create_notebook.py`: sumber sel notebook yang dapat diedit. Perintah biasa menolak menimpa notebook yang sudah ada. Gunakan `--output /tmp/preview.ipynb` untuk pratinjau; `--force` hanya setelah membuat backup, karena regenerasi menghapus output dan perlu diikuti Run All.
- `scripts/build_summary.py`: generator PDF dan Markdown dari artefak.
- `scripts/package_submission.py`: validasi lokal dengan `--check-local`; membuat ZIP final hanya setelah `github_proof.pdf` nyata tersedia.
- `QA.md`: bukti pemeriksaan lokal dan pekerjaan pengumpulan yang masih pending.

## Pemeriksaan paket

Jalankan `python scripts/package_submission.py --check-local` untuk memeriksa file lokal. Setelah notebook dan PDF benar-benar diunggah ke repository GitHub mahasiswa, simpan screenshot nyata beserta username/URL sebagai `github_proof.pdf` di root proyek. Lalu jalankan `python scripts/package_submission.py`; skrip memeriksa isi ZIP dan path dataset setelah ekstraksi. Unggah ZIP ke RISE secara terpisah. Skrip menolak membuat ZIP jika bukti GitHub belum ada.

## Status pengumpulan

Artefak lokal selesai. Bukti upload nyata ke GitHub (`github_proof.pdf`), verifikasi Colab, ZIP final lengkap, unggah RISE, dan wawancara mahasiswa masih pending. Jangan menyebut paket ini sudah dikumpulkan. Brief resmi menjadwalkan wawancara **Selasa, 29 September 2026, 21.12–21.20 WIB**; tenggat RISE harus dicek di pengumuman kuliah.

Untuk persiapan wawancara 7–8 menit, pastikan dapat menjelaskan: alasan `duration` dibuang; mengapa `unknown/default` dipertahankan; arti `pdays=999`; grup profil dan split; fit preprocessing per fold; F1 dan threshold 0,5; pemilihan dari CV; importance; serta keterbatasan generalisasi.
