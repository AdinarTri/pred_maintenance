# Interpretasi Hasil — Predictive Maintenance (BAB 4)

## 1. Ringkasan Dataset
- Unit analisis: **equipment-level monthly** (komponen sebagai proxy unit) — **78** observasi (komponen × bulan), 8 bulan (Jan–Agu 2025).
- Prediktor (konteks operasional): **12** | Failure (MTBF<median): **39** (50.0%), Non-failure: **39**.

## 2. Maintenance KPI (BAB 3.3.3)
Dihitung per komponen-bulan: **MTBF** (jam), **Availability** (rasio), **Downtime Rate** (rasio). Lihat `outputs/kpi_panel.csv`.

## 3. Logistic Regression (Model Utama)
| Metrik | Nilai |
|--------|-------|
| Accuracy | 0.750 |
| Precision | 0.692 |
| Recall | 0.900 |
| F1 | 0.783 |
| ROC-AUC | 0.890 |
| CV-5 ROC-AUC | 0.857 (±0.139) |

## 4. Proposisi Analitis 1 — Korelasi Pearson FRS vs KPI
| KPI | r | p-value | Signifikan | Arah |
|-----|---|---------|-----------|------|
| MTBF | -0.628 | 7.5e-10 | Ya | negatif |
| Availability | -0.783 | 2.4e-17 | Ya | negatif |
| Downtime Rate | +0.716 | 1.7e-13 | Ya | positif |

**Kesimpulan:** seluruh korelasi signifikan (p<0,05) dengan arah konsisten teoretis → **Proposisi 1 didukung**. FRS valid sebagai indikator prediktif keandalan.

## 5. Perbandingan Model
| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|-------|----------|-----------|--------|----|---------|
| Logistic Regression (utama) | 0.750 | 0.692 | 0.900 | 0.783 | 0.890 |
| Random Forest (pembanding) | 0.800 | 0.750 | 0.900 | 0.818 | 0.865 |

## 6. Kesesuaian Metodologi (Thesis Notes)
Pemodelan mengikuti BAB 3: KPI keandalan dihitung di level equipment-bulanan, Logistic Regression menghasilkan Failure Risk Score, divalidasi train-test split + metrik confusion matrix, dan Proposisi Analitis 1 diuji dengan korelasi Pearson. **Logistic Regression tetap model utama**; Random Forest hanya pembanding. Keterbatasan utama: tidak ada ID unit & telemetri sensor, panel kecil, serta indikator nilai operasional (Proposisi 2) belum tersedia — direkomendasikan untuk penelitian lanjutan.
