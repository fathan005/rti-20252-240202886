# WS-02: Problem Statement

> **Bab 2 — Problem Formulation & System Context**

Sumber: Hadiyani & Handaga (2025) — Deteksi Anomali Jaringan CNN–SVM, JIUP Vol.10 No.2

## Ringkasan Materi
Domain
Keamanan Jaringan Komputer / Network Intrusion Detection System (NIDS)
Konteks
Sistem deteksi intrusi berbasis anomali yang memproses lalu lintas jaringan secara real-time menggunakan pendekatan hybrid deep learning dan machine learning (CNN–SVM)

### Problem Formation Model

Reality
Volume dan kompleksitas lalu lintas jaringan terus meningkat seiring perkembangan teknologi digital dan internet

Observed Issue (Symptom)
IDS berbasis signature gagal mendeteksi serangan zero-day dan ancaman baru yang belum terdokumentasi; rekayasa fitur manual pada ML klasik (SVM tunggal) memakan waktu dan butuh keahlian domain mendalam

Diagnosed Problem (Root Cause)
Ketergantungan pada rules statis dan feature engineering manual membuat sistem tidak adaptif terhadap pola serangan yang terus berkembang; tidak ada mekanisme pembelajaran fitur otomatis dari data mentah jaringan

Researchable Problem
Belum ada sistem yang menggabungkan kemampuan ekstraksi fitur otomatis CNN dengan kekuatan klasifikasi SVM untuk deteksi anomali jaringan secara real-time yang mandiri dan tidak bergantung pada rules statis

Measurable Variable
Akurasi klasifikasi model CNN–SVM; jumlah paket anomali terdeteksi per interval 5 menit; perbandingan flag TCP (S0, REJ); jumlah deteksi per layanan target (FTP, HTTP, SSH)
```
Reality → Observed Issue (Symptom) → Diagnosed Problem (Root Cause)
→ Researchable Problem (Scoped) → Measurable Variable (Operationalized)
```

### Topic ≠ Problem ≠ Research Problem

| Level | Contoh | Status |
|-------|--------|--------|
| **Topik** | Keamanan IoT | Terlalu luas, tidak bisa diuji |
| **Problem** | MQTT tidak terenkripsi | Spesifik tapi belum riset |
| **Research Problem** | Belum ada studi membandingkan overhead TLS 1.3 vs DTLS pada MQTT di IoT RAM < 64KB | Bisa dirancang eksperimennya |

### Symptom vs Root Cause

Apa yang diamati (gejala) ≠ mengapa terjadi (akar masalah). Gunakan **5 Whys** atau **Fishbone Diagram** untuk menggali.

Contoh: "User meninggalkan checkout" (symptom) → "Waktu loading > 8 detik karena API call sequential" (root cause).

### System Thinking

Setiap masalah riset TI harus terikat pada komponen sistem: **Input → Process → Output → Outcome → Constraints → Stakeholders**.

### Problem Quality Check

Masalah riset yang layak harus memenuhi 5 kriteria:
- **Clarity** — Satu orang membaca akan paham
- **Measurability** — Ada metrik kuantitatif
- **Relevance** — Penting untuk domain
- **Testability** — Bisa gagal (falsifiable)
- **Impact** — Ada kontribusi jika terjawab

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan | Menyelesaikan masalah (*solve*) | Memahami dan membuktikan (*understand & prove*) |
| Masalah | Bug, error, fitur belum ada | Gap dalam pengetahuan |
| Scope | Selesaikan semua yang perlu | Batasi agar bisa dibuktikan |
| Output | Working system | Evidence, paper, replicable findings |

### Istilah Penting

- **Problem Statement** — Formulasi tertulis: konteks sistem + gap + dampak + justifikasi
- **System Context** — Deskripsi lengkap: input, proses, output, outcome, constraints, stakeholders
- **Problem Drift** — Masalah "bermutasi" dari pendahuluan ke metodologi karena statement awal tidak presisi
- **Solution-First Thinking** — Memulai dari solusi tanpa masalah yang jelas — berbahaya dalam riset
- **Operational Definition** — Definisi variabel yang cukup jelas agar peneliti lain bisa mengukur hal yang sama

---

## Template A.2 — Problem Statement Builder

```
PROBLEM STATEMENT BUILDER

Domain & Konteks
  Domain   : Keamanan Jaringan Komputer / Network Intrusion Detection System (NIDS)
  Konteks  : Sistem deteksi intrusi berbasis anomali yang memproses lalu lintas jaringan secara real-time menggunakan pendekatan hybrid deep learning dan machine learning (CNN–SVM)

System Context
  Input       : Paket lalu lintas jaringan yang ditangkap secara real-time menggunakan Scapy (parameter: timestamp, IP src, IP dst, protocol, TCP flag), serta dataset pelatihan NSL-KDD

  Process     : Preprocessing (normalisasi, one-hot encoding, log transformation) → ekstraksi fitur otomatis dengan CNN → klasifikasi anomali/normal oleh SVM

  Output      : Label klasifikasi per paket (normal / anomali), log deteksi per 5 menit, visualisasi grafik top-5 IP sumber anomali, layanan target, flag TCP mencurigakan

  Outcome     : Administrator jaringan dapat merespons ancaman siber lebih cepat melalui notifikasi otomatis via Telegram Bot

  Constraints : Dataset pelatihan terbatas pada NSL-KDD (tidak sepenuhnya merepresentasikan trafik terkini); interval deteksi 5 menit; tergantung ketersediaan jaringan dan spesifikasi hardware; serangan zero-day belum tentu terpola dalam training data

  Stakeholders: Administrator jaringan, tim keamanan siber (blue team), peneliti IDS, organisasi dengan infrastruktur jaringan kritis

Fenomena → Problem
  Fenomena yang diamati             : Volume dan kompleksitas lalu lintas jaringan terus meningkat seiring perkembangan teknologi digital dan internet
  Gejala (symptom) yang terukur     : IDS berbasis signature gagal mendeteksi serangan zero-day dan ancaman baru yang belum terdokumentasi; rekayasa fitur manual pada ML klasik (SVM tunggal) memakan waktu dan butuh keahlian domain mendalam
  Masalah yang didiagnosis          : Ketergantungan pada rules statis dan feature engineering manual membuat sistem tidak adaptif terhadap pola serangan yang terus berkembang; tidak ada mekanisme pembelajaran fitur otomatis dari data mentah jaringan
  Masalah riset (researchable)      : Belum ada sistem yang menggabungkan kemampuan ekstraksi fitur otomatis CNN dengan kekuatan klasifikasi SVM untuk deteksi anomali jaringan secara real-time yang mandiri dan tidak bergantung pada rules statis
  Variabel yang terukur             : Akurasi klasifikasi model CNN–SVM; jumlah paket anomali terdeteksi per interval 5 menit; perbandingan flag TCP (S0, REJ); jumlah deteksi per layanan target (FTP, HTTP, SSH)

Problem Quality Check
  [ ] Clarity — Apakah satu orang membaca akan paham?
Masalah jelas: IDS konvensional tidak adaptif terhadap ancaman baru karena bergantung pada rules statis

  [ ] Measurability — Apakah ada metrik kuantitatif?
Ada metrik kuantitatif: akurasi model, jumlah paket anomali, distribusi flag TCP, deteksi per layanan

  [ ] Relevance — Apakah penting untuk domain?
Sangat relevan: ancaman zero-day dan keterbatasan IDS berbasis signature adalah isu aktif dalam keamanan siber

  [ ] Testability — Apakah bisa gagal?
Bisa diuji: ada skenario simulasi serangan (SYN Flood, brute force) dengan lingkungan terkontrol (Kali Linux vs Ubuntu)

  [ ] Impact — Apakah ada kontribusi jika terjawab?
Kontribusi nyata: model CNN–SVM yang mandiri, tidak bergantung rules statis, dengan notifikasi real-time via Telegram — memperkuat respons mitigasi ancaman siber

Problem Statement (1 paragraf):
  Meningkatnya kompleksitas lalu lintas jaringan menciptakan celah keamanan yang tidak dapat ditangani oleh sistem deteksi intrusi berbasis signature, khususnya terhadap serangan zero-day yang belum terdokumentasi. Pendekatan anomali menggunakan machine learning klasik seperti SVM menunjukkan potensi, namun masih terkendala proses rekayasa fitur manual yang tidak efisien dan tidak skalabel. Oleh karena itu, dibutuhkan sebuah sistem deteksi anomali yang mampu mengekstrak fitur secara otomatis dari lalu lintas jaringan mentah dan melakukan klasifikasi secara real-time tanpa bergantung pada rules statis — yang direalisasikan melalui model hybrid CNN–SVM yang dilatih dengan dataset NSL-KDD dan diuji pada skenario serangan nyata.
```

---

## Latihan 1 — Dari Topik ke Masalah Riset

Pilih satu topik di bidang TI yang diminati. Transformasikan melalui 5 tahap Problem Formation Model.

**Topik awal:** Sistem Deteksi Intrusi Berbasis Machine Learning pada Jaringan Komputer

| Tahap | Hasil |
|-------|-------|
| Reality | Lalu lintas jaringan komputer terus meningkat volume dan kompleksitasnya seiring pertumbuhan penggunaan internet dan layanan digital di berbagai organisasi |

| Observed Issue (Symptom) | IDS berbasis signature gagal mendeteksi serangan zero-day; administrator jaringan tidak mendapat peringatan dini secara real-time ketika terjadi pola trafik mencurigakan yang belum terdokumentasi |

| Diagnosed Problem (Root Cause) |Sistem IDS konvensional bergantung pada rules statis dan feature engineering manual — tidak mampu belajar pola baru secara otomatis, sehingga rentan terhadap ancaman yang terus berevolusi |

| Researchable Problem |Belum ada implementasi sistem yang memadukan CNN (ekstraksi fitur otomatis) dan SVM (klasifikasi) secara hybrid untuk mendeteksi anomali jaringan secara real-time dan mandiri tanpa bergantung rules statis, yang diuji pada skenario serangan nyata |

| Measurable Variable |Akurasi klasifikasi model CNN–SVM; jumlah paket anomali terdeteksi per 5 menit; distribusi jenis TCP flag (S0, REJ); jumlah deteksi per layanan target (FTP, HTTP, SSH); IP sumber dengan anomali tertinggi |

**Apakah terjebak solution-first thinking?** [ ] Ya / [ ] Tidak
> Jika ya, kembali ke tahap mana? Ya — judul dan abstrak jurnal menyebut "CNN dan SVM" sebelum problem statement dirumuskan secara eksplisit. Jika terjebak, kembali ke tahap Diagnosed Problem untuk memastikan masalah riset dirumuskan tanpa menyebut solusi terlebih dahulu.

---

## Latihan 2 — System Context Decomposition

Gambarkan konteks sistem dari masalah riset di Latihan 1.

| Komponen | Deskripsi |
|----------|----------|
| Input | Paket jaringan real-time yang direkam oleh Scapy (timestamp, IP src, IP dst, protocol, TCP flag) + dataset pelatihan NSL-KDD (labeled benchmark) |

| Process | Preprocessing data (normalisasi Min-Max, one-hot encoding protocol, log transformation) → ekstraksi fitur spasial 1D oleh CNN → klasifikasi normal/anomali oleh SVM dengan fungsi keputusan sign(wTx + b)|

| Output |Label per paket (normal/anomali), log tabel 1958 entri per periode, grafik top-5 IP sumber anomali, grafik layanan target, grafik flag TCP mencurigakan, laporan dikirim via Telegram Bot tiap 5 menit |

| Outcome | Administrator jaringan dapat mengidentifikasi dan merespons ancaman siber secara proaktif dan lebih cepat tanpa harus memantau sistem secara manual terus-menerus|

| Constraints |Dataset NSL-KDD tidak mencerminkan trafik modern sepenuhnya; interval 5 menit membatasi deteksi serangan sub-menit; performa bergantung pada hardware; model tidak diuji lintas lingkungan jaringan berbeda |

| Stakeholders | Administrator jaringan (pengguna utama notifikasi), tim keamanan siber (blue team), peneliti IDS, manajemen organisasi yang memiliki infrastruktur jaringan kritis|

**Komponen mana yang paling relevan dengan masalah riset?**  Process — karena inti kontribusi penelitian ada pada pipeline CNN (feature extraction) → SVM (classification) yang menggantikan feature engineering manual dan rules statis.

---

## Latihan 3 — Problem Quality Check

Evaluasi problem statement yang sudah dibuat menggunakan 5 kriteria.

| Kriteria | Skor (1-5) | Justifikasi |
|----------|-----------|-------------|
| Clarity | 4 / 5| Masalah terbaca jelas dari abstrak dan pendahuluan, namun tidak dirumuskan dalam satu kalimat problem statement yang eksplisit — pembaca perlu menyimpulkan sendiri dari teks|

| Measurability |4 / 5 |Ada metrik kuantitatif (jumlah paket, distribusi flag, deteksi per layanan), namun tidak ada tabel perbandingan akurasi model secara eksplisit seperti confusion matrix atau F1-score |

| Relevance |5 / 5 |Sangat relevan — ancaman zero-day dan keterbatasan IDS berbasis signature adalah isu aktif dan kritis dalam keamanan siber, didukung banyak referensi terkini |

| Testability |4 / 5 |Ada skenario pengujian terkontrol (Kali Linux vs Ubuntu, serangan SYN Flood, brute force Hydra), namun pengujian hanya di satu lingkungan jaringan lokal — belum direplikasi di jaringan berbeda |

| Impact |5 / 5 |Kontribusi jelas: model CNN–SVM mandiri tanpa rules statis + notifikasi Telegram real-time — dapat langsung diadopsi oleh praktisi keamanan jaringan |

**Skor total:** 22 / 25

**Problem statement versi final (1 paragraf):**
> Sistem deteksi intrusi berbasis signature terbukti tidak mampu mendeteksi serangan zero-day karena ketergantungan pada rules statis, sementara pendekatan machine learning klasik seperti SVM masih memerlukan feature engineering manual yang tidak efisien. Penelitian ini menjawab gap tersebut dengan merancang model hybrid CNN–SVM yang mampu mengekstrak fitur secara otomatis dan melakukan klasifikasi anomali jaringan secara real-time setiap 5 menit, dilatih menggunakan dataset NSL-KDD dan diuji pada simulasi serangan SYN Flood terhadap layanan SSH, FTP, dan HTTP — menghasilkan sistem deteksi yang adaptif, mandiri, dan dilengkapi notifikasi otomatis via Telegram Bot.

---

## Refleksi

> Bandingkan "masalah" yang biasa ditemui saat coding (bug, error) dengan masalah riset. Apa perbedaan fundamental dalam cara mendefinisikan dan mendekati keduanya?

**Jawaban:**
> Masalah Engineering (Bug/Error)

Tujuan: memperbaiki sesuatu yang tidak bekerja sesuai ekspektasi
Definisi: jelas dan spesifik — error message, baris kode, output salah
Pendekatan: debug → fix → test → selesai
Ukuran selesai: program berjalan tanpa error
Konteks: berlaku untuk sistem tertentu saja

> Masalah Riset

Tujuan: memahami, membuktikan, dan menghasilkan pengetahuan baru
Definisi: harus dirumuskan — berasal dari gap pengetahuan, bukan error yang tampak
Pendekatan: observasi → diagnosis → hipotesis → eksperimen → generalisasi
Ukuran selesai: ada bukti yang dapat direplikasi dan dikontribusikan
Konteks: berlaku umum, bisa diterapkan di konteks lain
