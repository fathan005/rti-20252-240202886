# WS-03: Literature Mapping & Gap

> **Bab 3 — Literature Review, Research Gap & Baseline**

---

## Ringkasan Materi

### Literature Review = Positioning, Bukan Ringkasan

Literature review bukan merangkum paper satu per satu. Pendekatan yang benar adalah **concept-centric** — organisasi berdasarkan tema, metode, atau variabel. Tujuan: menemukan **pola, kontradiksi, dan gap**.

**Perbandingan pendekatan Author-centric vs Concept-centric:**

| Aspek | Author-centric (Hindari) | Concept-centric (Gunakan) |
|-------|--------------------------|---------------------------|
| Struktur | Per penulis/paper ("Rahman et al. menyatakan...") | Per konsep/metode ("Pendekatan berbasis transformer") |
| Tujuan | Ringkasan isi paper | Perbandingan metode & identifikasi gap |
| Contoh paragraph | "Rahman (2023) pakai CNN. Lee (2022) pakai LSTM. Zhang (2021) pakai RF." | "Tiga pendekatan dominan: CNN digunakan oleh 4 paper untuk representasi fitur visual; LSTM untuk data sekuensial; RF sebagai baseline klasik." |
| Hasil akhir | Daftar paper | Peta pengetahuan + gap yang teridentifikasi |

### Empat Jenis Research Gap

| Jenis Gap | Deskripsi | Contoh |
|-----------|----------|--------|
| **Performance Gap** | Performa belum memadai | Akurasi deteksi hanya 78% pada kasus tertentu |
| **Method Gap** | Pendekatan belum diterapkan | Belum ada yang pakai transformer untuk task ini |
| **Data Gap** | Dataset terbatas/tidak representatif | Semua studi pakai dataset sintetis |
| **Context Gap** | Belum diuji pada konteks berbeda | Belum ada evaluasi di negara berkembang |

Gap terkuat = kombinasi 2+ jenis.

### Systematic Search Strategy

1. **Database utama**: IEEE Xplore, ACM DL, Scopus
   - Akses IEEE/ACM melalui jaringan kampus atau VPN institusi
   - Alternatif bebas biaya: Google Scholar, ResearchGate ([researchgate.net](https://www.researchgate.net)), arXiv ([arxiv.org](https://arxiv.org))
2. **Boolean query** yang terdokumentasi eksplisit
   - Contoh: `("anomaly detection" OR "intrusion detection") AND ("deep learning" OR "neural network") NOT ("medical imaging")`
   - Gunakan tanda kutip untuk frasa eksak; AND/OR/NOT mengontrol scope
3. **Snowballing** — dua arah:
   - **Backward snowballing**: buka daftar referensi di paper kunci → telusuri paper yang dikutip
   - **Forward snowballing**: di Google Scholar, klik "Cited by" di bawah paper kunci → temukan paper yang mengutipnya
   - Ulangi 1–2 tingkat untuk membangun cakupan komprehensif
4. Klaim "belum ada penelitian" harus didukung **bukti pencarian**

### Baseline Selection — 3 Kriteria

| Kriteria | Pertanyaan |
|----------|-----------|
| **Relevan** | Apakah menyelesaikan masalah yang sama? |
| **Representatif** | Apakah mewakili common practice? |
| **State-of-the-Art** | Apakah terbaru/terbaik? |

Membandingkan deep learning 2024 dengan decision tree sederhana tanpa justifikasi = **straw man comparison** (perbandingan tidak jujur).

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan baca literatur | Mencari solusi yang sudah ada | Memahami apa yang belum terjawab |
| Cara membaca paper | Tutorial, how-to | Metode, limitasi, gap |
| Baseline | Framework terpopuler | State-of-the-art yang rigorous |
| Dokumentasi pencarian | Tidak diperlukan | Wajib (reproducible) |

### Istilah Penting

- **Concept-centric** — Organisasi literatur berdasarkan konsep/metode, bukan per penulis
- **Snowballing** — Backward (telusuri referensi) + Forward (cari yang mengutip paper kunci)
- **Research Position** — Pernyataan eksplisit posisi riset terhadap studi sebelumnya
- **Straw man comparison** — Memilih baseline lemah agar metode sendiri terlihat lebih baik

---

## Template A.3 — Literature Mapping & Gap Identification

```
LITERATURE MAPPING

Topik      :Sistem Deteksi Anomali Jaringan Berbasis Machine Learning & Deep Learning
Database   :IEEE Xplore, Computers & Security, Google Scholar
Query      : ("network anomaly detection" OR "intrusion detection system") AND ("CNN" OR "SVM" OR "deep learning") AND ("real-time")
Tahun      : 2019–2025
Hasil awal : 20 paper → Screening → 8 paper final

Literature Matrix (concept-centric):

| Study | Tahun | Method | Data | Result | Limitation |
|-------|-------|--------|------|--------|------------|
| Ma, Sun, Cui & Jin| 2021| Kernel SVM | 3 dataset jaringan (tidak spesifik) |Akurasi >99% | Tidak diuji secara real-time; statis  |
| Altunay & Albayrak |2023|CNN + LSTM (hybrid)|Industrial IoT log|Akurasi tinggi pada IoT|Tidak menggunakan capture paket langsung|
|Berhane et al.|2023|CNN + SVM (hybrid)|Credit card fraud dataset|Peningkatan akurasi signifikan|Domain berbeda (bukan network traffic)|
|Alrayes et al.|2024|CNN Channel Attention|NSL-KDD|Akurasi kompetitif|Tidak ada komponen real-time|
|Al Ghamdi|2023|CNN + SVM (hybrid)|PCAP / Snort log|Performa optimal pasca normalisasi|Tidak menggunakan notifikasi otomatis|
|Liu & Wang|2023|CNNNSL-KDD|Deteksi anomali real-time|SVM tidak diintegrasikan sebagai classifier|
|Ogah et al.|2024|Beragam ML|Heterogeneous network data|Perbandingan model ML|Tidak ada deployment real-time|
|Hadiyani & Handaga |2025|CNN–SVM hybrid + Scapy|NSL-KDD (training) + real-time capture|1958/2323 paket terdeteksi anomali|Lingkungan terkontrol; dataset terbatas|

Pola yang ditemukan:
  Metode dominan     : Hybrid deep learning + classical ML (terutama CNN + SVM atau CNN + LSTM)
  Dataset umum       : NSL-KDD (dipakai mayoritas studi sebagai benchmark)
  Limitasi berulang  : Hampir semua studi tidak menguji sistem secara real-time dengan capture paket langsung; sebagian besar hanya mengevaluasi performa pada dataset statis
GAP IDENTIFICATION

Gap 1: [Jenis:  Context / Deployment Gap]
  Deskripsi    : Mayoritas penelitian menguji model pada dataset statis (offline), bukan pada lalu lintas jaringan yang ditangkap langsung secara real-time menggunakan tools seperti Scapy.
  Bukti        :  Liu & Wang (2023) mengklaim "real-time" tetapi tetap menggunakan dataset NSL-KDD yang pra-rekam; Alrayes et al. (2024) tidak memiliki komponen deployment sama sekali.
  Signifikansi : Kondisi jaringan nyata bersifat dinamis dan tidak terdistribusi secara ideal seperti benchmark dataset — model yang hanya diuji secara offline belum tentu bekerja pada kondisi operasional.

Gap 2: [Jenis: Method Gap]
  Deskripsi    : Sistem deteksi berbasis rules statis (seperti Snort) tidak mampu mengenali pola serangan yang belum terdokumentasi (zero-day), namun integrasi model ML/DL dengan notifikasi administrator secara otomatis masih sangat jarang diteliti.
  Bukti        :S, Wahyuddin et al. (2025) menggunakan Snort + Telegram tetapi murni berbasis rules, bukan model yang bisa belajar. Tidak ada studi dalam matriks yang mengombinasikan keduanya (adaptive model + real-time alert).
  Signifikansi : Kecepatan respons insiden sangat bergantung pada otomasi notifikasi — tanpa ini, deteksi yang akurat sekalipun tidak langsung ditindaklanjuti.

Baseline Selection:
| Baseline | Relevansi | Representatif | Source |
|Kernel SVM (standalone)|Task sama: klasifikasi anomali lalu lintas jaringan|Digunakan sebagai metode perbandingan di banyak studi IDS|Ma et al., 2021|

|  CNN standalone (tanpa SVM) |  Arsitektur CNN untuk deteksi anomali jaringan berbasis NSL-KDD |  Dipakai oleh Liu & Wang (2023) dan Alrayes et al. (2024) |  Liu & Wang, 2023 |
```

---

## Latihan 1 — Concept-Centric Literature Table

Gunakan topik riset dari WS-02. Cari minimal 5 paper relevan menggunakan database akademik.

> **Panduan pencarian:**
> - Database: IEEE Xplore, ACM DL, Google Scholar, atau ResearchGate
> - Tulis query Boolean yang digunakan: contoh `("object detection" OR "image classification") AND ("edge computing") NOT ("medical")`. Dokumentasikan query secara eksplisit.
> - Akses gratis: buka Google Scholar → cari judul paper → klik [PDF] jika tersedia, atau akses lewat campus VPN

**Topik riset:** Deteksi anomali lalu lintas jaringan komputer berbasis hybrid CNN-SVM
**Query pencarian:** ("network anomaly detection" OR "intrusion detection") AND ("CNN" OR "convolutional neural network") AND ("SVM" OR "support vector machine")
**Database:** IEEE Xplore, Google Scholar, Computers & Security 

| # | Study | Tahun | Method | Dataset | Result | Limitasi |
|---|-------|-------|--------|---------|--------|----------|
| 1 | *Ma, Sun, Cui & Jin* | *2021* | *Kernel SVM* | *3 network datasets* | *Acc >99%* | *Tidak real-time; fitur manual* |
| 2 |Al Ghamdi |2023 |CNN + SVM |PCAP/Snort log |Optimal pasca normalisasi |Tanpa notifikasi otomatis |
| 3 |Berhane et al. | 2023| CNN + SVM| Credit card fraud| Akurasi meningkat signifikan| Domain bukan network traffic|
| 4 |Liu & Wang | 2023|CNN |NSL-KDD |Performa real-time baik |SVM tidak digunakan sebagai classifier akhir |
| 5 |Alrayes et al. |2024 |CNN Channel Attention |NSL-KDD |Akurasi kompetitif | Tidak ada deployment/real-time testing|

**Pola yang terlihat — Metode dominan:**CNN (standalone atau hybrid dengan SVM/LSTM)
**Limitasi yang berulang:** Tidak ada pengujian real-time dengan capture langsung; bergantung pada dataset benchmark yang mungkin tidak representatif terhadap kondisi jaringan aktual
---

## Latihan 2 — Gap Identification

Berdasarkan tabel di Latihan 1, identifikasi gap.

| Jenis Gap | Ditemukan? | Gap Statement |
|-----------|-----------|---------------|
| Performance Gap |  Ya  | *Akurasi SVM standalone menurun pada data tidak seimbang atau high-dimensional tanpa feature extraction otomatis* |
| Method Gap |  Ya   | Belum ada studi yang mengintegrasikan CNN-SVM dengan pipeline notifikasi real-time (Telegram Bot) untuk respons insiden|
| Data Gap |  Ya  | Hampir semua studi menggunakan NSL-KDD yang statis — tidak ada studi yang menggabungkan NSL-KDD (training) dengan capture traffic langsung (testing)|
| Context Gap | Ya  |Studi sebelumnya tidak menguji sistem pada skenario simulasi serangan multi-attacker dalam jaringan DHCP nyata |

**Gap utama yang dipilih:**Data + Context Gap — penggunaan data training statis (NSL-KDD) dikombinasikan dengan pengujian pada traffic yang ditangkap langsung secara real-time dari simulasi serangan nyata.

**Mengapa gap ini penting (bukan sekadar "belum ada yang meneliti")?**
> Sistem IDS hanya berguna jika terbukti bekerja pada kondisi operasional, bukan hanya pada benchmark. Jika model tidak pernah diuji pada traffic nyata, kita tidak tahu apakah distribusi fitur dari dataset pelatihan relevan dengan kondisi aktual jaringan yang diserang.
---

## Latihan 3 — Baseline Selection

Pilih 2 baseline dari literatur yang sudah dibaca.

| # | Baseline | Mengapa Relevan | Mengapa Representatif | Apakah SOTA? | Sumber |
|---|----------|----------------|----------------------|-------------|--------|
| 1 | *Kernel SVM (standalone)* | *Task identik: klasifikasi anomali jaringan* | *Dipakai sebagai pembanding di mayoritas paper IDS* | *Tidak, tapi strong baseline* | *Ma et al., 2021* |
| 2 |CNN standalone + NSL-KDD |Arsitektur dan dataset sama dengan paper ini |Digunakan di Liu & Wang (2023) dan Alrayes et al. (2024) |Mendekati SOTA untuk NSL-KDD | Liu & Wang, 2023|

**Apakah pemilihan baseline ini bisa dianggap straw man?** [ ] Ya / [ ] Tidak
> Justifikasi: Ya — karena kedua baseline ini merupakan metode yang benar-benar kompetitif dan banyak dikutip, bukan metode lemah yang sengaja dipilih agar hasil terlihat bagus.

---

## Refleksi

> Apa perbedaan antara "belum ada yang meneliti ini" (klaim tanpa bukti) dengan research gap yang valid? Bagaimana cara membuktikan bahwa sebuah gap benar-benar ada?

**Jawaban:**
>Klaim "belum ada yang meneliti" adalah argument from ignorance — sekadar menyatakan ketiadaan tanpa membuktikan bahwa ketiadaan tersebut penting. Gap yang valid harus menunjukkan tiga hal: (1) ada masalah nyata yang belum terpecahkan, (2) literatur yang ada terbukti tidak menyelesaikannya, dan (3) menyelesaikan gap tersebut memberi nilai praktis atau teoritis yang konkret.
> Cara membuktikan bahwa gap benar-benar ada:
Lakukan analisis sistematis terhadap literatur yang ada — bukan hanya membaca abstrak, tetapi memeriksa experimental setup dan limitation section setiap paper. Jika 5 dari 6 paper mengakui "we did not test on live traffic" dalam bagian limitasi mereka, itu adalah bukti empiris dari gap, bukan sekadar klaim. Seperti pada paper ini: gap real-time deployment dibuktikan dengan menunjukkan bahwa Liu & Wang (2023) menggunakan dataset pra-rekam meski mengklaim "real-time", dan tidak ada studi sebelumnya yang mengombinasikan Scapy capture + hybrid model + notifikasi otomatis dalam satu pipeline.
