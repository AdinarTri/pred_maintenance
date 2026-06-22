# Interpretasi Hasil — Predictive Maintenance (BAB 4)

## 1. Ringkasan Dataset
- Jumlah observasi (event downtime): **1716**
- Jumlah fitur model: **12**
- Failure (kegagalan berdampak besar): **430** (25.1%)
- Non-failure: **1286** (74.9%)
- Definisi target: **Severity P75** — event dengan `Total DT Hours >= 21.8 jam` dianggap kegagalan berdampak besar.

> Catatan struktur data: kolom `Date` dan `DT Cost` berupa *merged cells* sehingga sel kosong diisi mengikuti nilai di atasnya (*forward-fill*). Dataset asli juga tidak memiliki label kegagalan eksplisit (semua baris berstatus *Unplanned Down*); target dibentuk dari severity downtime. Pendekatan alternatif (rasio downtime & agregasi komponen) telah diuji; agregasi-komponen ditolak karena *data leakage*.

## 2. Hasil Logistic Regression (Model Utama)
| Metrik | Nilai |
|--------|-------|
| Accuracy | 0.601 |
| Precision | 0.328 |
| Recall | 0.570 |
| F1 Score | 0.416 |
| ROC-AUC | 0.652 |

## 3. Perbandingan Model
| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|-------|----------|-----------|--------|----|---------|
| Logistic Regression (utama) | 0.601 | 0.328 | 0.570 | 0.416 | 0.652 |
| Random Forest (pembanding) | 0.674 | 0.388 | 0.533 | 0.449 | 0.671 |

- Model terbaik berdasarkan ROC-AUC: **Random Forest (Pembanding)**
- Model terbaik berdasarkan Recall: **Logistic Regression (Utama)**
- Model paling interpretable: **Logistic Regression**

## 4. Interpretasi Failure Risk Score
FRS = probabilitas kegagalan × 100 (skala 0–100). Klasifikasi risiko: **Kritis** (≥70), **Tinggi** (50–70), **Sedang** (30–50), **Rendah** (<30). Peralatan dengan FRS tertinggi diprioritaskan untuk inspeksi/penggantian.

## 5. Faktor Penyebab & Rekomendasi Maintenance
- **Meningkatkan risiko**: periode operasi lanjut (`month_idx`), komponen **Tire/Wheel** & **Electrical/Lamp**.
- **Menurunkan risiko**: komponen **Engine/Fuel** & **Driveline/Transmission** (umumnya downtime singkat).
- **Rekomendasi**: jadwalkan preventive maintenance berbasis-risiko, fokus pada ban/roda & kelistrikan, dan tingkatkan inspeksi menjelang akhir periode operasi.

## 6. Catatan Tesis (Kesesuaian Metodologi)
Logistic Regression berhasil ditetapkan sebagai **model utama** penghasil Failure Risk Score sesuai metodologi penelitian. Random Forest sebagai pembanding memberikan hasil yang **searah**, memperkuat validitas temuan. Metrik bersifat moderat karena dataset merupakan *log downtime* dengan fitur prediktif terbatas; penambahan data telemetri sensor (jam operasi mesin, beban, suhu, getaran) direkomendasikan untuk meningkatkan akurasi pada penelitian lanjutan.
