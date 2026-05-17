# WS-06: System-Experiment Mapping

> **Bab 6 — System Design sebagai Experimental Artifact**

---

## Ringkasan Materi

### Sistem = Instrumen Pengujian, Bukan Produk

Seorang engineer bertanya "apakah sistem bekerja?" — seorang peneliti bertanya "apa yang bisa dibuktikan sistem ini?" Sistem dalam riset adalah **artifact** — objek yang sengaja dibuat untuk menguji klaim spesifik.

### System as Experiment Model

```
RQ → Variable → System Component → Experimental Setup → Output
```

Setiap komponen sistem harus bisa ditelusuri ke variabel riset (top-down), dan setiap pengukuran harus menjawab RQ (bottom-up).

### Mapping Variabel ke Komponen

| Tipe Variabel | Peran di Sistem | Contoh |
|---------------|----------------|--------|
| **IV** (Independent) | Modul yang bisa di-toggle/swap | Algoritma A vs B |
| **DV** (Dependent) | Modul pengukuran | Logger, metrics collector |
| **CV** (Control) | Config yang dikunci | Dataset, parameter tetap |

Jika variabel tidak bisa di-map ke komponen apapun → arsitektur perlu didesain ulang.

### 4 Prinsip Desain Eksperimental

| Prinsip | Pertanyaan Kunci |
|---------|-----------------|
| **Traceability** | Komponen ini melayani variabel yang mana? |
| **Modularity** | Bisakah IV diubah tanpa memengaruhi yang lain? |
| **Controllability** | Apakah CV dieksternalisasi ke config file? |
| **Measurability** | Apakah sistem otomatis menghasilkan data yang dibutuhkan? |

### Variable Isolation melalui Arsitektur

- **Modular architecture** — Pisahkan berdasarkan variabel
- **Configuration-driven** — Ubah config (YAML/JSON), bukan code
- **Feature toggles** — On/off flag untuk ablation study

  Contoh config YAML dengan feature toggles:
  ```yaml
  model:
    type: cnn          # IV: ganti "rf" untuk kondisi baseline
  features:
    use_temporal: true  # toggle komponen temporal
    use_normalization: true  # toggle preprocessing
  experiment:
    seed: 42
    runs: 5
  ```
  Dengan pendekatan ini, berbeda kondisi eksperimen = berbeda satu baris config, **tanpa mengubah kode**.

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan sistem | Memenuhi kebutuhan user | Menguji hipotesis, menghasilkan bukti |
| Arsitektur | Optimasi performa & skalabilitas | Optimasi isolasi variabel & reprodusibilitas |
| Konfigurasi | Sering hardcoded | Dieksternalisasi ke config file |
| Fitur tambahan | Menambah nilai user | Menambah noise jika tidak terkait RQ |

### Istilah Penting

- **Artifact** — Objek yang sengaja dibuat untuk memecahkan masalah atau menguji proposisi
- **Traceability** — Kemampuan menelusuri hubungan RQ → variabel → komponen → output
- **Variable Isolation** — Mengubah hanya satu variabel sambil menahan yang lain konstan
- **Ablation Study** — Menguji kontribusi tiap komponen dengan melepasnya satu per satu
- **Configuration-driven Execution** — Semua parameter di config file, bukan hardcoded

---

## Template A.6 — Mapping RQ ke Arsitektur Sistem

```
SYSTEM-EXPERIMENT MAPPING

Research Question:Apakah model hybrid CNN–SVM yang dilatih pada dataset NSL-KDD menghasilkan akurasi dan F1-Score lebih tinggi dibandingkan SVM standalone dalam mendeteksi anomali lalu lintas jaringan yang ditangkap secara real-time menggunakan Scapy pada skenario simulasi serangan multi-attacker?

Variable → Component Mapping:
| Variabel | Tipe | Komponen Sistem | Cara Manipulasi/Pengukuran |
|----------|------|-----------------|---------------------------|
|Jenis model deteksi | IV   |Modul classifier — model_loader.py | Ganti config model_type = "cnn_svm" | "svm_only". CNN–SVM: feature extraction via Conv1D → flatten → SVM. SVM standalone: fitur manual langsung ke SVM.|

|Akurasi & F1-Score klasifikasi| DV   |Modul evaluasi — metrics.py | Dihitung otomatis dari confusion matrix setiap akhir interval 5 menit menggunakan sklearn.metrics. Output disimpan ke log CSV dan dikirim via Telegram Bot.|

|Dataset pelatihan (NSL-KDD)| CV   |Modul preprocessing — preprocessor.py| Dataset di-load sekali saat inisiasi. Versi file dikunci (hash MD5 dicatat). Kedua model (CNN–SVM dan SVM standalone) menggunakan preprocessor.transform() yang identik.|

4 Prinsip Desain:
  [ ya] Traceability— Setiap komponen sistem berlabel variabel:
model_loader
= IV,
metrics.py
= DV,
preprocessor
= CV. Log output menyertakan nama model aktif di setiap baris.

  [ ya] Variable Isolation— IV (jenis model) dapat diganti hanya dengan mengubah satu config flag tanpa menyentuh modul preprocessing, capture, atau pelaporan.

  [ya ] Measurement Integration— Pengukuran DV built-in di dalam pipeline: setiap batch 5 menit otomatis menghasilkan confusion matrix dan dikirim ke Telegram.
  [ya ] Reproducibility— Setup dapat direkonstruksi: script serangan tetap, dataset dikunci via hash, testbed fisik terdokumentasi, kode tersedia di GitHub.

Experimental Setup:
  Input data     : Training: NSL-KDD (CSV, versi tetap); Testing: paket TCP real-time dari scapy capture pada interface jaringan DHCP testbed
  Parameter      : Interval pelaporan: 5 menit; batch size capture: semua paket dalam window; model: CNN–SVM hybrid vs SVM standalone; fitur: timestamp, IP src, IP dst, protocol, flag TCP
  Output format  : Log CSV per interval (waktu, IP sumber, IP tujuan, layanan, flag, label); bar chart top-5 anomali; notifikasi Telegram Bot; file laporan periodik
```

---

## Latihan 1 — Variable-to-Component Mapping

Gunakan RQ dan variabel dari WS-05. Petakan ke komponen sistem.

**RQ:** Apakah model hybrid CNN–SVM yang dilatih pada dataset NSL-KDD menghasilkan akurasi dan F1-Score lebih tinggi dibandingkan SVM standalone dalam mendeteksi anomali lalu lintas jaringan yang ditangkap secara real-time menggunakan Scapy pada skenario simulasi serangan multi-attacker?

| Variabel | Tipe | Komponen Sistem | Cara Manipulasi / Pengukuran |
|----------|------|-----------------|---------------------------|
|Jenis model deteksi anomali | *IV* | Modul classifier — swap CNN–SVM ↔ SVM standalone via config flag | Ubah satu baris: model_type = "cnn_svm" atau "svm_only". Semua modul lain tidak berubah.|

|Akurasi & F1-Score klasifikasi anomali | DV |Modul evaluasi (metrics.py) — otomatis dijalankan di akhir setiap batch |Dihitung dari confusion matrix: TP, TN, FP, FN per 5 menit. Disimpan ke CSV dan dikirim Telegram.|

|Dataset pelatihan NSL-KDD | CV |Modul preprocessing (preprocessor.py) — dijalankan sekali saat init | File dikunci (hash MD5). Normalisasi MinMax + one-hot encoding identik untuk kedua model. Tidak diubah antar sesi.|

**Apakah semua variabel bisa di-map?** Ya 
> Jika tidak, komponen apa yang perlu ditambahkan? Semua variabel berhasil di-map ke komponen sistem. Tidak ada variabel yang "menggantung" tanpa representasi teknis. Komponen yang ada sudah mencukupi untuk menjalankan eksperimen perbandingan IV tanpa modifikasi CV.
---

## Latihan 2 — 4 Prinsip Desain

Evaluasi desain sistem terhadap 4 prinsip.

| Prinsip | Status | Bukti / Penjelasan |
|---------|--------|-------------------|
| Traceability |Terpenuhi |Setiap modul dalam pipeline berlabel variabel: model_loader.py → IV, metrics.py → DV akurasi, reporter.py → DV jumlah anomali, preprocessor.py → CV dataset, attack_script.sh → CV serangan. Log output menyertakan nama model aktif di setiap baris sehingga hasil dapat ditelusuri balik ke kondisi eksperimen. |

| Modularity |Terpenuhi |Arsitektur terbagi ke modul terpisah: capture (scapy) → preprocessing → feature extraction (CNN atau manual) → classification (SVM) → evaluation → reporting. IV dapat diganti (swap model) hanya dengan mengubah satu config flag, tanpa menyentuh modul capture, preprocessing, atau Telegram Bot — ini bukti modularity yang baik. |

| Controllability |Sebagian |CV yang bersifat software (dataset, script serangan) mudah dikontrol. Namun CV hardware (spesifikasi PC, kondisi jaringan DHCP, beban sistem OS saat simulasi) sulit dikontrol sepenuhnya — fluktuasi CPU usage atau buffer OS saat SYN Flood dapat mempengaruhi jumlah paket yang berhasil di-capture. Ini adalah titik lemah utama controllability. |

| Measurability |Terpenuhi |Pengukuran DV built-in di pipeline: confusion matrix dihitung otomatis tiap 5 menit, disimpan ke CSV, divisualisasikan, dan dikirim via Telegram Bot. Tidak ada langkah pengukuran manual yang bisa menimbulkan human error. Format output terstruktur (log tabel + bar chart) memudahkan analisis pasca-eksperimen. |

**Prinsip mana yang paling sulit dipenuhi?**Variabel CV hardware tidak dapat dikontrol penuh — beban CPU, kondisi buffer jaringan, dan fluktuasi OS saat SYN Flood berlangsung dapat memvariasikan jumlah paket yang berhasil di-capture antar sesi. Strategi: (1) jalankan setiap kondisi eksperimen 3 kali dan ambil rata-rata; (2) monitor CPU & memory usage secara paralel selama eksperimen menggunakan htop atau psutil; (3) catat jumlah paket yang di-capture vs yang dihasilkan attacker sebagai indikator completeness.

**Strategi untuk mengatasinya:**
> replikasi 3x per kondisi + monitoring resource paralel.
---

## Latihan 3 — Ablation Study Planning

Jika sistem memiliki 3 komponen utama, rencanakan ablation study.

> **Panduan jumlah kondisi:** Untuk 3 komponen (A, B, C), kondisi minimal yang direkomendasikan:
> Full + (-A) + (-B) + (-C) = **4 kondisi dasar**. Jika waktu memungkinkan, tambahkan kombinasi ganda: (-A,-B), (-A,-C), (-B,-C) = **7 kondisi**. Sesuaikan dengan *computational cost* dan tenggat waktu penelitian.

| Kondisi | Komponen A | Komponen B | Komponen C | Hasil yang Diharapkan |
|---------|-----------|-----------|-----------|----------------------|
| Full | ✅ CNN Conv1D | ✅ MinMax + one-hot | ✅ serror_rate, rerror_rate, srv_error_rate|Performa tertinggi — CNN mengekstrak fitur spasial, preprocessing menstabilkan skala, sliding-window menangkap pola temporal serangan |

| – A | ❌ Ganti dengan fitur manual (tanpa CNN)| ✅ | ✅ |Penurunan F1-Score karena fitur yang masuk SVM lebih sederhana dan tidak hierarkis — ini mengukur kontribusi CNN sebagai feature extractor |

| – B | ✅ | ❌ Tanpa normalisasi (raw features)| ✅ | Penurunan akurasi signifikan karena CNN kesulitan belajar dari fitur berskala sangat berbeda (durasi 0–58329 vs flag biner 0/1). Diperkirakan menjadi kondisi terburuk.|

| – C | ✅ | ✅ | ❌ Tanpa sliding-window (hanya fitur per-paket statis) |Penurunan kemampuan deteksi pola serangan berkelanjutan (SYN Flood) — model gagal mengenali bahwa banyak koneksi berurutan dari IP yang sama adalah anomali, bukan insiden tunggal |

**Komponen mana yang diprediksi paling berkontribusi?**Komponen A (CNN feature extractor)
**Mengapa?**
>  Ini adalah inti dari novelty penelitian — CNN memungkinkan SVM menerima representasi fitur hierarkis yang lebih kaya tanpa feature engineering manual. Jika kondisi –A menunjukkan penurunan F1-Score yang besar, ini membuktikan klaim kontribusi utama paper. Kondisi –A, –C secara khusus berguna karena hasilnya dapat langsung dibandingkan dengan SVM standalone sebagai baseline dari WS-04.
---

## Refleksi

> Apa risiko jika sistem dibangun seperti produk (monolitik, fitur lengkap) lalu baru dilakukan eksperimen? Mengapa arsitektur modular penting untuk riset?

**Jawaban:**
> Jika sistem dibangun seperti produk — fitur lengkap, semua komponen terintegrasi erat — maka ketika hasil eksperimen tidak sesuai harapan, peneliti tidak bisa mendiagnosis komponen mana yang bermasalah. Dalam konteks penelitian ini: jika sistem CNN–SVM monolitik menghasilkan F1 rendah, apakah masalahnya di arsitektur CNN, di normalisasi data, atau di sliding-window features? Tidak bisa diketahui. Ini memaksa peneliti untuk menebak perbaikan, bukan menganalisisnya secara sistematis.
> Bahaya kedua: sistem monolitik sulit direproduksi. Jika preprocessing, feature extraction, dan classification bercampur dalam satu fungsi, peneliti lain tidak bisa mengganti hanya satu komponen untuk memverifikasi klaim. Ini melanggar prinsip falsifiability dari WS-04 — klaim "CNN-SVM lebih baik dari SVM" tidak dapat diuji secara terisolasi.
