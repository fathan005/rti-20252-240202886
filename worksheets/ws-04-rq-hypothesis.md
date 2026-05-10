# WS-04: Research Question & Hypothesis

> **Bab 4 — Research Question, Contribution & Hypothesis**

---

## Ringkasan Materi

### RQ Bukan Pertanyaan Biasa

Research Question yang baik secara implisit mengandung cetak biru eksperimen: subjek, baseline, metrik, domain, dataset.

| Kualitas | Contoh |
|----------|--------|
| **Buruk** | "Bagaimana pengaruh deep learning terhadap deteksi malware?" |
| **Baik** | "Apakah CNN menghasilkan F1-Score lebih tinggi dari RF pada CIC-MalMem-2022?" |

Perbedaan: RQ yang baik menyebutkan **metode spesifik**, **metrik terukur**, **baseline**, dan **dataset**.

### Tiga Jenis RQ

| Jenis | Pola | Kebutuhan |
|-------|------|-----------|
| **Comparison** | A vs B → mana lebih baik? | ≥ 2 metode, metrik sama |
| **Improvement** | A' vs A → modifikasi lebih baik? | Pre/post, bukti perbaikan |
| **Exploratory** | Faktor X₁...Xₙ → pengaruh terhadap Y? | Multi-variabel, korelasi/regresi |

### Contribution Statement

Tiga jenis kontribusi: **Improvement** (metode terbukti lebih baik), **Comparison** (perbandingan sistematis yang belum ada), **Novel Approach** (pendekatan baru). Kontribusi harus terhubung langsung dengan gap — kontribusi tanpa gap = klaim tanpa justifikasi.

### Hypothesis H₀ / H₁

- **H₀** (Null) = Tidak ada perbedaan signifikan — asumsi default, harus dibuktikan salah
- **H₁** (Alternative) = Ada perbedaan signifikan — diterima hanya jika H₀ ditolak
- Harus **falsifiable**, mengandung **metrik terukur**, dirumuskan **SEBELUM eksperimen**

### Rantai Operasionalisasi

```
RQ → Variable → Metric → Data → Analysis
```

Jika rantai ini tidak lengkap, RQ belum mature. Bi-directional: RQ yang tidak bisa jadi hipotesis testable harus direvisi mundur.

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan pertanyaan | Apa yang harus dibangun? | Apa yang harus dibuktikan? |
| Bentuk jawaban | Sistem yang berfungsi | Bukti empiris terukur |
| Sukses diukur oleh | User satisfaction, uptime | Signifikansi statistik, effect size |
| Jika gagal | Debug dan perbaiki | Laporkan, analisis mengapa |

### Istilah Penting

- **Research Question (RQ)** — Pertanyaan spesifik: variabel terukur + metrik + konteks
- **Contribution Statement** — Apa yang diketahui setelah riset selesai yang sebelumnya belum ada
- **H₀ / H₁** — Null vs Alternative Hypothesis
- **Falsifiability** — Kondisi hipotesis ditolak harus bisa didefinisikan sebelum eksperimen
- **Operationalization** — Proses mewujudkan konsep abstrak menjadi variabel terukur

---

## Template A.4 — RQ-Contribution-Hypothesis

```
RQ-CONTRIBUTION-HYPOTHESIS

Gap Statement  :IDS berbasis signature tidak mampu mendeteksi ancaman baru (zero-day). Metode ML klasik seperti SVM masih mengandalkan feature engineering manual yang memakan waktu dan membutuhkan keahlian domain. Belum ada sistem yang mengintegrasikan CNN sebagai feature extractor otomatis dengan SVM sebagai classifier untuk deteksi anomali jaringan secara real-time dengan notifikasi otomatis.

Research Question:
  Tipe         : [ ] Comparison  [ ] Improvement  [ ] Exploratory
  Formulasi    : Apakah model hybrid CNN-SVM yang dilatih menggunakan dataset NSL-KDD dapat mendeteksi anomali lalu lintas jaringan komputer secara real-time dengan akurasi yang lebih baik dibandingkan metode SVM tunggal?
  Variabel IV  : Jenis model deteksi — CNN-SVM hybrid vs SVM tunggal
  Variabel DV  : Akurasi deteksi anomali lalu lintas jaringan komputer secara real-time
  Metrik       : Akurasi klasifikasi, jumlah anomali terdeteksi, stabilitas performa real-time
  Dataset      : NSL-KDD (training); lalu lintas real-time via Scapy (testing)
  Baseline     : SVM tunggal dengan feature engineering manual

Quality Check RQ:
  [✓ ] Variabel spesifik - CNN-SVM vs SVM, dataset NSL-KDD
  [ ✓] Metrik jelas - akurasi, jumlah deteksi anomali
  [✓] Baseline ada- SVM tunggal dan sistem berbasis rules Snort
  [✓] Konteks disebutkan - lalu lintas jaringan komputer real-time
  [✓ ] Memerlukan eksperimen (bukan hanya survei literatur)- simulasi serangan nyata di testbed

Contribution Statement:
  Apa yang baru diketahui : 
Model hybrid CNN-SVM mampu mendeteksi pola anomali jaringan secara mandiri dan real-time tanpa bergantung pada rules statis, dengan laporan periodik otomatis via Telegram Bot
  Jenis kontribusi        : [✓ ] Improvement  [ ] Comparison  [✓ ] Novel approach
  Gap yang diisi          : Keterbatasan SVM dalam feature engineering manual → diatasi dengan CNN sebagai extractor otomatis; keterbatasan rules statis Snort → diatasi dengan model yang belajar dari pola data
Hypothesis Pair:
  H₀ : Tidak ada perbedaan signifikan antara akurasi deteksi anomali model hybrid CNN-SVM dibandingkan SVM tunggal pada lalu lintas jaringan real-time menggunakan dataset NSL-KDD
  H₁ : Model hybrid CNN-SVM menghasilkan akurasi deteksi anomali yang lebih tinggi secara signifikan dibandingkan SVM tunggal pada lalu lintas jaringan real-time menggunakan dataset NSL-KDD
  Threshold              : α = 0.05; akurasi model ≥ baseline SVM
  Justifikasi threshold  : Threshold α = 0.05 adalah standar umum dalam penelitian machine learning; penelitian rujukan (Ma et al., 2021) mencapai >99% akurasi dengan Kernel SVM sebagai patokan perbandingan
```

---

## Latihan 1 — Dari Gap ke RQ

Gunakan gap yang ditemukan di WS-03. Transformasikan menjadi Research Question.

**Gap dari WS-03:** Data + Context Gap — Model CNN-SVM belum pernah diuji menggunakan data lalu lintas yang ditangkap langsung secara real-time dari simulasi serangan multi-attacker; semua studi sebelumnya hanya mengevaluasi pada dataset statis (NSL-KDD offline).
**RQ versi pertama (tulis bebas):**
> "Bagaimana performa model CNN-SVM dalam mendeteksi anomali jaringan secara real-time?

**Evaluasi RQ:**

| Komponen | Ada? | Isi |
|----------|------|-----|
| Metode spesifik | sebagian | CNN-SVM disebutkan, tapi tidak ada perbandingan eksplisit dengan baseline|
| Metrik terukur |Tidak | Performa" terlalu umum — akurasi? F1? jumlah deteksi?|
| Baseline |Tidak |Tidak ada — dibandingkan dengan apa? |
| Dataset/konteks | Sebagian	| Real-time" ada, tapi dataset training (NSL-KDD) dan metode capture (Scapy) tidak disebut|

**Tipe RQ:** [ ] Comparison / [✓ ] Improvement / [ ] Exploratory

**RQ versi revisi (setelah evaluasi):**
> Apakah model hybrid CNN-SVM yang dilatih pada dataset NSL-KDD menghasilkan akurasi dan F1-Score lebih tinggi dibandingkan SVM standalone dalam mendeteksi anomali lalu lintas jaringan yang ditangkap secara real-time menggunakan Scapy pada skenario simulasi serangan multi-attacker?
---

## Latihan 2 — Hypothesis Pair

Rumuskan pasangan hipotesis dari RQ di Latihan 1.

| Komponen | Isi |
|----------|-----|
| H₀ | Tidak ada perbedaan signifikan antara akurasi dan F1-Score model hybrid CNN-SVM dengan SVM standalone dalam mendeteksi anomali lalu lintas jaringan yang ditangkap secara real-time menggunakan Scapy pada dataset NSL-KDD |
| H₁ | Model hybrid CNN-SVM menghasilkan akurasi dan F1-Score yang lebih tinggi secara signifikan dibandingkan SVM standalone dalam mendeteksi anomali lalu lintas jaringan real-time yang ditangkap menggunakan Scapy pada dataset NSL-KDD|
| Metrik | Akurasi (%), F1-Score, jumlah anomali terdeteksi per periode 5 menit|
| Threshold | α = 0.05; peningkatan akurasi CNN-SVM ≥ 2% di atas SVM standalone|
| Justifikasi threshold | α = 0.05 adalah standar umum penelitian ML; ambang 2% dipilih karena baseline SVM (Ma et al., 2021) sudah mencapai >99% sehingga perbedaan kecil tetap bermakna secara praktis|

**Apakah hipotesis ini falsifiable?** [✓ ] Ya / [ ] Tidak
> Bagaimana cara membuktikannya salah? Jika pada pengujian real-time menggunakan Scapy, akurasi dan F1-Score CNN-SVM tidak lebih tinggi dari SVM standalone (atau bahkan lebih rendah), maka H₁ ditolak dan H₀ gagal dibuktikan salah — eksperimen tetap dilaporkan sebagai null result yang valid
---

## Latihan 3 — Rantai Operasionalisasi

Lengkapi rantai dari RQ hingga metode analisis.

| Tahap | Isi |
|-------|-----|
| RQ | Apakah CNN-SVM hybrid menghasilkan akurasi dan F1-Score lebih tinggi dari SVM standalone dalam mendeteksi anomali jaringan real-time via Scapy pada NSL-KDD? |
| Variable (IV) |Jenis algoritma deteksi: CNN-SVM hybrid vs SVM standalone |
| Variable (DV) | Akurasi dan F1-Score klasifikasi lalu lintas jaringan (normal vs anomali) pada kondisi real-time|
| Metric |Akurasi (%), F1-Score (%), jumlah true positive dan false positive per periode 5 menit |
| Data source |Training: NSL-KDD (benchmark statis); Testing: traffic real-time dari simulasi serangan (SYN Flood SSH/FTP/HTTP) via Scapy pada testbed 3 PC (2 Kali Linux + 1 Ubuntu) |
| Analysis method | Perbandingan performa CNN-SVM vs SVM standalone; analisis log anomali periodik; visualisasi top-5 IP sumber, layanan target, flag TCP (S0, REJ); uji statistik jika sampel mencukupi|

**Apakah rantai lengkap?** [ ✓] Ya / [ ] Tidak
> Jika tidak, tahap mana yang perlu direvisi? semua tahap terhubung dari RQ hingga metode analisis

---

## Refleksi

> Ambil satu judul skripsi/paper yang pernah dibaca. Coba ekstrak RQ-nya. Apakah RQ tersebut memenuhi semua komponen (metode, metrik, baseline, konteks)? Jika tidak, apa yang hilang?

**Judul:**Hadiyani & Handaga (2025) — Implementasi Sistem Deteksi Anomali Berbasis Jaringan Menggunakan CNN dan SVM untuk Klasifikasikan Data Secara Real-time
**RQ yang diekstrak:**Apakah sistem hybrid CNN-SVM yang dilatih pada NSL-KDD dan diuji pada traffic real-time via Scapy mampu mendeteksi anomali jaringan dengan akurasi tinggi dan stabil?
**Komponen yang hilang:**Dua komponen kritis tidak terpenuhi: (1) baseline perbandingan tidak hadir — paper hanya melaporkan hasil CNN-SVM tanpa membandingkan dengan SVM standalone secara kuantitatif, sehingga klaim "lebih baik" tidak dapat diverifikasi; (2) metrik tidak didefinisikan dengan threshold yang jelas sebelum eksperimen — "akurasi baik" bersifat subjektif. Ini membuat paper lebih bersifat engineering report daripada penelitian dengan hipotesis testable yang falsifiable.
