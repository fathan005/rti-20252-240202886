# WS-07: Experimental Design & Validity

> **Bab 7 — Experimental Design & Validity**

---

## Ringkasan Materi

### Correlation ≠ Causality

Kausalitas membutuhkan 3 syarat:
1. **Covariance** — X dan Y bergerak bersama
2. **Temporal precedence** — X berubah sebelum Y
3. **Elimination of alternatives** — Tidak ada faktor lain yang menjelaskan Y

Controlled experiment adalah satu-satunya metode yang bisa membuktikan kausalitas.

### Empat Jenis Validitas

| Jenis | Pertanyaan | Ancaman Umum |
|-------|-----------|-------------|
| **Internal** | Apakah hubungan IV→DV nyata? | Confounding variable, selection bias |
| **External** | Apakah bisa digeneralisasi? | Dataset terlalu spesifik |
| **Construct** | Apakah mengukur konsep yang benar? | Metrik tidak sesuai |
| **Conclusion** | Apakah kesimpulan statistik valid? | Sample size kecil, uji salah |

Internal dan external validity sering berkonflik: semakin terkontrol (internal kuat) → semakin artificial (external lemah).

### Tiga Tipe Eksperimen dalam Riset TI

| Tipe | Deskripsi | Kapan Digunakan |
|------|----------|----------------|
| **Comparison Study** | Metode A vs B pada kondisi identik | Membandingkan pendekatan berbeda |
| **Ablation Study** | Full system → lepas komponen satu per satu | Mengukur kontribusi tiap komponen |
| **Parameter Study** | Variasikan satu parameter, amati dampak | Uji sensitifitas/robustness |

### Fairness dalam Perbandingan

Perbandingan yang adil = **kondisi identik** untuk semua metode: dataset sama, preprocessing sama, tuning effort sebanding, environment sama, metrik sama.

Contoh tidak adil: Transformer (30 fitur tambahan + Bayesian optimization) vs RF (default params) → hasilnya misleading.

### Threats to Validity = Diidentifikasi Sebelum Eksperimen

Ancaman validitas harus diidentifikasi **sebelum** eksperimen dan mitigasinya dirancang sebagai bagian dari desain — bukan ditulis sebagai boilerplate setelah selesai.

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan testing | Memastikan sistem memenuhi requirement | Membuktikan hubungan kausal antar variabel |
| Baseline | Versi sebelumnya (last release) | Metode tervalidasi dari literatur |
| Kegagalan | Bug → fix → release | H₀ tidak ditolak → tetap kontribusi ilmiah |
| Sukses | 100% test pass | Evidence valid — mendukung atau menolak hipotesis |

### Istilah Penting

- **Causality** — Hubungan sebab-akibat (covariance + temporal + elimination)
- **Controlled Experiment** — Ubah satu variabel, kontrol sisanya, amati efek
- **Fairness** — Semua metode diuji pada kondisi yang benar-benar identik
- **Threats to Validity** — Faktor yang bisa melemahkan kesimpulan jika tidak dimitigasi
- **Conclusion Validity** — Validitas statistik: power, sample size, uji yang tepat

---

## Template A.7 — Desain Eksperimen Lengkap

```
EXPERIMENT DESIGN

Research Question : Apakah model hybrid CNN-SVM yang dilatih pada dataset NSL-KDD menghasilkan akurasi dan F1-Score lebih tinggi dibandingkan SVM standalone dalam mendeteksi anomali lalu lintas jaringan yang ditangkap secara real-time menggunakan Scapy pada skenario simulasi serangan multi-attacker?
Hypothesis        :H₀ Tidak ada perbedaan signifikan antara akurasi dan F1-Score CNN-SVM dengan SVM standalone (α = 0,05).
H₁ CNN-SVM menghasilkan akurasi dan F1-Score lebih tinggi secara signifikan dibanding SVM standalone (α = 0,05; peningkatan ≥ 2%).
Tipe Eksperimen   : [ya ] Comparison  [ya ] Ablation  

Kondisi Eksperimen:
| Kondisi | Deskripsi | IV Value | CV Settings |
|---------|-----------|----------|-------------|
| Control | SVM standalone dengan fitur diekstrak manual (serror_rate, rerror_rate, protocol one-hot) dari parameter jaringan| SVM standalone|NSL-KDD versi hash MD5 terkunci, preprocessing MinMax + one-hot via preprocessor.py, testbed 3 PC identik, hping3 SYN Flood port 22/21/80, sesi 30 menit × 3 replikasi|
| Treatment | Pipeline CNN-SVM hybrid — CNN Conv1D sebagai feature extractor otomatis, output-nya diteruskan ke SVM sebagai classifier|CNN-SVM hybrid| Semua CV identik dengan kondisi control — hanya model_loader.py yang berbeda via satu config flag|

Fairness Checklist:
  [ya ] Dataset identik untuk semua kondisi
  [ya ] Preprocessing setara
  [ ] Tuning effort setara
  [ ya] Environment identik
  [ ya] Metrik evaluasi sama

Threat Analysis:
| Threat Type | Ancaman Spesifik | Mitigasi |
|-------------|-----------------|----------|
| Internal    |  Variable leakage: CV tidak terisolasi sempurna jika modifikasi model_loader.py mempengaruhi preprocessing atau cara data diproses | Arsitektur modular — satu config flag mengubah IV tanpa menyentuh komponen lain; ablation study memverifikasi isolasi antar komponen |
| External    |  Testbed closed network — hasil mungkin tidak berlaku untuk jaringan enterprise dengan trafik heterogen dan beban tinggi | Scope dibatasi eksplisit dalam paper; generalisasi ke UNSW-NB15 dan CIC-IDS2018 direncanakan di tahap lanjutan  |
| Construct   |F1-Score mungkin tidak cukup menangkap kinerja pada kelas yang sangat tidak seimbang jika salah satu serangan mendominasi ekstrem  | F1-Score dipilih pre-eksperimen dan dicatat sebelum data dikumpulkan; precision dan recall terpisah dilaporkan sebagai secondary untuk mendeteksi ceiling effect |
| Conclusion  | Sample size (6 sesi × 30 menit) mungkin tidak cukup untuk uji t berpasangan yang valid secara statistik |Uji t berpasangan dilakukan hanya jika distribusi data mencukupi; jika tidak, dilaporkan perbandingan deskriptif dengan ukuran efek  |

Statistical Plan:
  Uji statistik   : Paired t-test
  Justifikasi      : Dua kondisi pada testbed yang sama, hasil berpasangan per sesi
  Alpha            : 0,05
  Effect size min  : ≥ 2%
```

---

## Latihan 1 — Desain Eksperimen

Susun desain eksperimen berdasarkan RQ, variabel, dan sistem dari WS-04 sampai WS-06.

**RQ:**Apakah model hybrid CNN-SVM yang dilatih pada dataset NSL-KDD menghasilkan akurasi dan F1-Score lebih tinggi dibandingkan SVM standalone dalam mendeteksi anomali lalu lintas jaringan yang ditangkap secara real-time menggunakan Scapy pada skenario simulasi serangan multi-attacker?
**Tipe eksperimen:** [ ] Comparison / [ ] Ablation / [ ] Parameter

| Kondisi | Deskripsi | IV Value | CV Settings |
|---------|-----------|----------|-------------|
| Baseline | SVM standalone dengan fitur diekstrak manual dari parameter jaringan (serror_rate, rerror_rate, protocol one-hot) — pendekatan umum dalam literatur sebelumnya (Ma et al., 2021) | SVM standalone
fitur manual| Dataset NSL-KDD versi terkunci (hash MD5), preprocessing MinMax + one-hot via preprocessor.py, testbed 3 PC (2 Kali Linux attacker, 1 Ubuntu target), script serangan hping3 SYN Flood port 22/21/80 parameter tetap, sesi 30 menit, replikasi 3×, seed random tetap|
| Intervensi |Pipeline CNN-SVM hybrid — CNN Conv1D sebagai feature extractor otomatis hierarkis dari data paket 1D, output fitur diteruskan ke SVM sebagai classifier akhir |fitur otomatis |Semua CV identik dengan kondisi control. Satu-satunya perbedaan: model_loader.py di-switch via config flag dari SVM standalone ke CNN-SVM hybrid — tidak ada perubahan pada preprocessor, dataset, testbed, maupun script serangan |

---

## Latihan 2 — Fairness Checklist

Evaluasi apakah desain eksperimen di Latihan 1 sudah fair.

| Kriteria | Status | Detail |
|----------|--------|--------|
| Dataset identik | ✅ |Kedua model menggunakan NSL-KDD versi yang sama, dikunci dengan hash MD5 sehingga tidak ada perbedaan data latih maupun distribusi kelas |
| Preprocessing setara | ✅ | preprocessor.py yang sama digunakan untuk kedua kondisi: MinMax normalization + one-hot encoding identik. Tidak ada preprocessing tambahan eksklusif untuk salah satu model|
| Tuning effort setara | | Proposal belum menyebutkan apakah hyperparameter CNN (jumlah filter Conv1D, learning rate, epoch) dan SVM (kernel, C, gamma) di-tune dengan effort yang sebanding. Jika CNN di-tune intensif (Bayesian optimization, grid search luas) sementara SVM menggunakan default params, perbandingan menjadi tidak adil dan hasil bias ke CNN-SVM|
| Environment identik | ✅ |Testbed 3 PC yang sama digunakan untuk semua kondisi (hardware tidak berganti), NTP disinkronisasi antar mesin, script serangan attack_script.sh dengan parameter hping3 tetap dijalankan identik setiap sesi |
| Metrik evaluasi sama | ✅ |F1-Score, Accuracy, Precision, Recall dihitung via metrics.py yang sama untuk kedua kondisi. Metrik primer (F1-Score) ditetapkan pre-eksperimen sebelum data dikumpulkan — menghindari p-hacking |

**Ada yang tidak fair?** Ya — pada kriteria tuning effort setara
> Jika ya, bagaimana cara memperbaikinya? Tetapkan budget tuning yang eksplisit dan setara untuk kedua model sebelum eksperimen dimulai. Pilihan: (1) keduanya menggunakan default hyperparameter tanpa tuning sama sekali, atau (2) keduanya menjalankan random search / grid search dengan jumlah iterasi yang sama (misalnya 20 iterasi). Dokumentasikan keputusan ini di bagian metodologi agar perbandingan dapat difalsifikasi

---

## Latihan 3 — Threat Analysis

Identifikasi ancaman validitas untuk desain eksperimen ini.

| Threat Type | Ancaman Spesifik | Mitigasi |
|-------------|-----------------|----------|
| Internal | Variable leakage antar kondisi: jika model_loader.py tidak benar-benar memisahkan komponen, perubahan pada CNN bisa secara tidak sengaja mempengaruhi preprocessing atau cara data masuk ke SVM — sehingga IV dan CV tidak terisolasi sempurna | Arsitektur modular dengan satu config flag yang hanya mengubah komponen model; ablation study memverifikasi isolasi tiap komponen secara terisolasi — jika hasil -A setara SVM standalone, isolasi terbukti berjalan |
| External |Testbed closed network 3 PC tidak merepresentasikan jaringan nyata — tidak ada trafik background (browsing, streaming, update OS) yang biasanya ada di jaringan enterprise atau kampus |Scope dibatasi secara eksplisit di laporan; generalisasi ke dataset UNSW-NB15 dan CIC-IDS2018 serta jaringan lebih kompleks direncanakan di tahap penelitian lanjutan |
| Construct |F1-Score agregat menyembunyikan performa per kelas serangan: model mungkin sangat baik mendeteksi SYN Flood di port 22 tetapi gagal di port 80, namun angka F1-Score keseluruhan tetap tinggi |Precision dan Recall per kelas serangan (SSH/FTP/HTTP) dilaporkan secara terpisah sebagai secondary metrics — jika ada ceiling effect atau perbedaan antar kelas, akan terdeteksi |
| Conclusion |Sample size kecil untuk uji statistik: 6 sesi (3 replikasi × 2 kondisi) mungkin tidak memenuhi asumsi normalitas paired t-test, sehingga kesimpulan statistik menjadi lemah |Uji normalitas dilakukan sebelum paired t-test; jika asumsi tidak terpenuhi, diganti dengan Wilcoxon signed-rank test (non-parametrik). Jika distribusi tidak mencukupi, dilaporkan deskriptif dengan ukuran efek (Cohen's d) |

**Ancaman mana yang paling sulit dimitigasi?**Domain shift (validitas eksternal)
**Mengapa?**
> NSL-KDD dibangun dari data DARPA 1999 — lebih dari 25 tahun yang lalu. Karakteristik trafik jaringan modern (ukuran paket, protokol, pola serangan) sudah berubah drastis. Ancaman ini tidak bisa dihilangkan tanpa mengganti dataset latih sepenuhnya. Solusi parsial yang tersedia hanya mendokumentasikan limitasi ini secara jujur dan merencanakan generalisasi ke dataset lebih baru di tahap penelitian berikutnya. Selama dataset latih tetap NSL-KDD, domain shift akan selalu menjadi celah validitas eksternal yang tidak bisa ditutup sepenuhnya dalam penelitian ini.

---

## Refleksi

> Sebuah paper melaporkan "metode kami mengalahkan semua baseline." Apa 3 pertanyaan pertama yang harus diajukan untuk mengevaluasi klaim ini?

**Jawaban:**
1. Apakah kondisi pengujian benar-benar identik untuk semua metode (fair comparison)?
2. Apakah perbedaan performa signifikan secara statistik dan bermakna secara praktis?
3. Apakah baseline yang digunakan adalah metode yang valid dan representatif dari literatur?