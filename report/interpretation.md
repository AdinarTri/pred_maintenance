# Interpretasi Hasil — Predictive Maintenance (BAB 4)

## 1. Ringkasan Dataset
- Unit analisis: equipment-level monthly (komponen sebagai proxy unit) — **78** observasi, 8 bulan (Jan–Agu 2025).
- Failure (MTBF<median): **39** (50.0%), Non-failure: **39**. Prediktor konteks: **12**.

## 2. Maintenance KPI (BAB 3.3.3)
Dihitung per komponen-bulan: MTBF (jam), Availability, Downtime Rate. Lihat `outputs/kpi_panel.csv`.

## 3. Logistic Regression (Model Utama)
| Metrik | Nilai |
|--------|-------|
| Accuracy | 0.800 |
| Precision | 0.800 |
| Recall | 0.800 |
| F1 | 0.800 |
| ROC-AUC | 0.850 |
| CV-5 ROC-AUC | 0.887 (±0.105) |

## 4. Proposisi Analitis 1 — Korelasi Pearson FRS vs Maintenance KPI (level panel komponen-bulan, n=78)
| Maintenance KPI | r (vs FRS) | p-value | Signifikan |
|-----------------|-----------|---------|-----------|
| MTBF | −0.646 | 0.000 | Ya |
| Availability | −0.773 | 0.000 | Ya |
| Downtime Rate | +0.705 | 0.000 | Ya |

**Kesimpulan:** Proposisi 1 **didukung** — FRS berasosiasi negatif & signifikan dengan MTBF dan Availability (risiko naik saat keandalan turun) serta positif dengan Downtime Rate. *Catatan metodologis:* label *failure* diturunkan dari ambang MTBF, sehingga hubungan FRS–MTBF **sebagian bersifat by-construction**; r=−0.646 (bukan ≈−1) karena FRS dihasilkan LogReg multi-fitur (komponen, bulan, cost), bukan MTBF murni. Hasil dibaca sebagai **konfirmasi konsistensi internal** FRS terhadap KPI keandalan, bukan klaim prediktif independen. Lihat `outputs/proposisi1_correlation.csv` & `outputs/proposisi1_scatter.png`.

## 5. Proposisi Analitis 2 — Korelasi Pearson KPI vs Nilai Operasional (level blok harian, n=241)
| Indikator Nilai | r (vs Downtime Rate) | p-value | Signifikan |
|-----------------|----------------------|---------|-----------|
| Production Loss | +0.738 | 1.1e-42 | Ya |
| Downtime Cost | -0.179 | 0.0053 | Ya |
| Budget Variance | +0.094 | 0.15 | Tidak |

**Kesimpulan:** Proposisi 2 **didukung sebagian** — keandalan rendah berhubungan **kuat & signifikan dengan kenaikan Production Loss**; hubungan dengan **Downtime Cost lemah/tidak meyakinkan** (kolom biaya sangat noisy) dan dengan **Budget Variance tidak signifikan** (variansnya terkonsentrasi di Jul–Agu).

## 6. Perbandingan Model
| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|-------|----------|-----------|--------|----|---------|
| Logistic Regression (utama) | 0.800 | 0.800 | 0.800 | 0.800 | 0.850 |
| Random Forest (pembanding) | 0.850 | 0.818 | 0.900 | 0.857 | 0.885 |

## 7. Kesesuaian Metodologi (Thesis Notes)
Pemodelan mengikuti BAB 3: KPI keandalan di level equipment-bulanan, Logistic Regression menghasilkan Failure Risk Score, divalidasi train-test split + metrik confusion matrix, **Proposisi Analitis 1** diuji dengan korelasi Pearson FRS ↔ KPI keandalan (level panel), dan **Proposisi Analitis 2** diuji dengan korelasi Pearson KPI ↔ indikator nilai operasional (level blok harian). Logistic Regression tetap model utama; Random Forest pembanding. Keterbatasan: tanpa ID unit & label asli & telemetri; Proposisi 2 di level blok harian; Budget Variance terkonsentrasi Jul–Agu.
