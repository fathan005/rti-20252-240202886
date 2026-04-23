# WS-01: Distorsi & Paradigma

> **Bab 1 — Research Mindset in IT**

---

## Ringkasan Materi

### Research Trust Model

Pengetahuan ilmiah tidak muncul langsung dari kenyataan. Ia melewati **6 tahap transformasi** yang masing-masing rawan distorsi:

```
Reality → Data → Processing → Analysis → Inference → Knowledge
```

Etika mencegah distorsi yang disengaja (fabrikasi, cherry-picking). Validitas mendeteksi distorsi yang tidak disengaja (confounding variable, sampling bias).

### Tiga Jenis Validitas

| Jenis | Pertanyaan | Contoh Ancaman |
|-------|-----------|----------------|
| **Internal Validity** | Apakah hubungan kausal benar ada? | Confounding variable |
| **External Validity** | Apakah bisa digeneralisasi? | Dataset terlalu homogen |
| **Construct Validity** | Apakah mengukur hal yang benar? | Metrik tidak sesuai klaim |

### Paradigma Riset

Mata kuliah ini menggunakan pendekatan **Positivist** (fenomena TI bisa diukur objektif melalui eksperimen terkontrol) diperkuat **Design Science Research** (DSR). Penting untuk membedakan keduanya:

| Paradigma | Cara Kerja | Contoh di TI |
|-----------|-----------|---------------|
| **Positivis** | Uji hipotesis dengan eksperimen terkontrol | Apakah CNN lebih akurat dari RF pada dataset X? |
| **Design Science Research** | Bangun artefak (sistem/model/framework) untuk menguji proposisi | Dapatkah arsitektur hybrid CNN+LSTM membuktikan peningkatan recall ≥5%? |
| **Interpretivis** | Pahami makna melalui konteks & kualitatif | Bagaimana peneliti manafsirkan anomali data sensor IoT? |

Dalam DSR, artefak **bukan tujuan akhir** — ia adalah instrumen untuk menghasilkan pengetahuan. Pertanyaan riset tetap harus difalsifikasi.

### Mode Berpikir Peneliti

**Curious** (mempertanyakan fenomena) → **Critical** (mengevaluasi klaim berdasarkan bukti) → **Systematic** (merancang investigasi terstruktur dan reproducible).

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan | Membuat sistem yang bekerja | Menghasilkan pengetahuan yang valid |
| Pertanyaan khas | "Bagaimana membuatnya jalan?" | "Apakah klaim ini benar?" |
| Ukuran sukses | Sistem berfungsi, client puas | Hipotesis terjawab, temuan tervalidasi |
| Kegagalan | Harus dihindari | Harus dilaporkan (negative result = kontribusi) |

### Istilah Penting

- **Research Mindset** — Pola pikir yang menuntut bukti dan mempertanyakan asumsi
- **Research Ethics** — Prinsip perilaku: kejujuran, objektivitas, keterbukaan, akuntabilitas
- **HARKing** — Hypothesizing After Results are Known — merumuskan hipotesis setelah melihat data
- **Falsifiability** — Hipotesis harus bisa dibuktikan salah

---

## Template A.1 — Research Mindset Self-Assessment

```
Nama Peneliti    : ____________________
Tanggal          : ____________________

1. Ketika membaca klaim "metode X 95% akurat":
   - Pertanyaan pertama saya: ____________________
   - Data yang dibutuhkan untuk verifikasi: ____________________

2. Posisi paradigma:
   - Pendekatan: [ ] Positivis  [ ] Interpretivis  [ ] Design Science  [ ] Mixed
   - Alasan: ____________________

3. Identifikasi distorsi:
   - Asumsi tersembunyi: ____________________
   - Sumber bias potensial: ____________________
   - Langkah mitigasi: ____________________

4. Komitmen etika:
   - Data yang tidak akan dimanipulasi: ____________________
   - Batasan yang diakui sejak awal: ____________________
```

---

## Latihan 1 — Identifikasi Distorsi

Pilih satu paper riset di bidang TI yang mengklaim "metode X meningkatkan performa." Telusuri setiap tahap Research Trust Model.

> **Panduan pencarian paper:** Gunakan [IEEE Xplore](https://ieeexplore.ieee.org), [ACM Digital Library](https://dl.acm.org), atau Google Scholar. Pilih paper **tahun 2020 ke atas**, di topik yang Anda minati: deteksi anomali, klasifikasi citra, NLP, keamanan siber, IoT, dsb.
>
> **Contoh domain TI:** "Deteksi anomali lalu-lintas jaringan menggunakan CNN — akurasi meningkat 94% vs baseline SVM 87%." Distorsi potensial: apakah dataset normal/anomali seimbang? Apakah hanya diuji pada satu vendor traffic?

**Paper yang dipilih:**
> Judul:Implementasi Sistem Deteksi Anomali Berbasis Jaringan Menggunakan CNN dan SVM untuk Klasifikasikan Data Secara Real-time
> Penulis (Tahun): Arief Luqman Hadiyani, Bana Handaga (2025)
> Sumber/Link DOI:[ __________________________________
___](https://doi.org/10.32493/informatika.v10i2.52163)
| Tahap | Apa yang Dilakukan | Potensi Distorsi |
|-------|-------------------|-----------------|
| Reality → Data | *Merekam lalu lintas jaringan secara real-time menggunakan Scapy pada komputer target (Ubuntu PC3); serangan disimulasikan dari dua PC Kali Linux.* | *Lingkungan testbed lokal sangat terkontrol dan tidak mewakili jaringan produksi nyata. Serangan hanya tiga jenis (SSH, FTP, HTTP SYN Flood), sehingga data tidak mencakup variasi ancaman dunia nyata.* |
| Data → Processing |Ekstraksi sliding-window 2 detik, normalisasi MinMax, one-hot encoding, log-transformation untuk distribusi skewed. Model dilatih menggunakan dataset NSL-KDD. |NSL-KDD adalah dataset tahun 1999 (diperbaiki dari KDD'99) yang sudah ketinggalan zaman — pola serangannya tidak merepresentasikan ancaman modern. Sliding-window 2 detik dipilih tanpa justifikasi eksplisit mengapa bukan 1 atau 5 detik |
| Processing → Analysis |CNN digunakan sebagai feature extractor (konvolusi 1D dengan ReLU), output CNN diteruskan ke SVM sebagai classifier akhir. Model hybrid CNN-SVM dievaluasi pada data hasil tangkapan Scapy. |Tidak ada ablation study — tidak dibandingkan CNN saja vs SVM saja untuk membuktikan kontribusi masing-masing. Tidak ada k-fold cross-validation yang dilaporkan secara eksplisit. |
| Analysis → Inference |Laporan dihasilkan setiap 5 menit: grafik top-5 IP sumber anomali, target layanan, flag TCP, dan log tabel 1958 anomali dari 2323 total paket. |Tidak ada metrik standar evaluasi (accuracy, precision, recall, F1-score) yang dilaporkan secara kuantitatif di bagian hasil. Klaim "akurasi tinggi" di abstrak tidak didukung angka eksplisit dalam tabel performa |
| Inference → Knowledge |Kesimpulan: pendekatan hybrid CNN-SVM lebih baik dari rule-based Snort karena mampu mengenali pola baru tanpa rules statis. |Perbandingan dengan Snort hanya bersifat konseptual, tidak ada uji komparatif langsung. Generalisasi ke jaringan lain belum divalidasi — external validity rendah |

**Distorsi paling besar di tahap:** ________________________

**Dua distorsi spesifik yang teridentifikasi:**
1. Dataset training tidak sesuai konteks pengujian External Validity

Model dilatih pada NSL-KDD (berbasis file PCAP dari tahun 1999), tetapi diuji pada data real-time yang ditangkap Scapy di jaringan lokal tahun 2025. Distribusi fitur antar-domain ini berbeda signifikan — fenomena ini disebut dataset shift. Akurasi tinggi pada pengujian lokal belum tentu bertahan di jaringan dengan karakteristik berbeda.
2. Tidak ada baseline komparatif yang terukur Construct Validity

Paper mengklaim hybrid CNN-SVM lebih unggul dari metode konvensional (Snort/rule-based), namun tidak menyertakan eksperimen komparasi langsung dengan angka. Tanpa baseline yang diuji pada kondisi identik, klaim superioritas tidak dapat divalidasi secara ilmiah.

---

## Latihan 2 — Analisis Kasus Etika

Skenario: Seorang peneliti menemukan bahwa jika 3 data point outlier dihapus, hasil eksperimennya menjadi signifikan. Dengan outlier, hasilnya tidak signifikan.

| Perspektif | Analisis |
|------------|---------|
| Kejujuran ilmiah |Melaporkan semua hasil termasuk false positives dan false negatives secara eksplisit, bukan hanya total deteksi yang terlihat baik |
| Transparansi |Kode Python dan dataset disertakan via GitHub — ini praktik baik. Namun parameter threshold SVM dan CNN architecture detail perlu juga didokumentasikan |
| Peer review |Reviewer sebaiknya meminta tabel confusion matrix lengkap sebelum menyetujui klaim "akurasi tinggi" yang tidak disertai angka spesifik |

**Keputusan akhir dan justifikasi:**
> Peneliti harus menambahkan tabel evaluasi standar (precision, recall, F1 per kelas) dan mengungkap kondisi threshold yang digunakan — termasuk jika threshold dipilih setelah melihat data pengujian, yang merupakan bentuk ringan dari HARKing

---

## Latihan 3 — Posisi Paradigma

**Topik riset:** Deteksi anomali lalu lintas jaringan komputer secara real-time menggunakan model hybrid CNN-SVM

> **Skala 1–5:** 1 = tidak sesuai sama sekali dengan topik ini, 5 = sangat sesuai dan dominan digunakan pada riset bertopik serupa.

| Kriteria | Positivis | Interpretivis | Design Science |
|kesesuaian dengan topik
Topik kuantitatif, cocok untuk uji hipotesis dengan metrik terukur. Namun limitasi: paper tidak benar-benar merumuskan hipotesis falsifiable secara eksplisit di awal.|kesesuaian dengan topik
Tidak relevan — topik ini tidak meneliti makna, konteks sosial, atau pengalaman manusia. Limitasi: tidak dapat mengukur efektivitas secara numerik.|kesesuaian dengan topik
Tidak relevan — topik ini tidak meneliti makna, konteks sosial, atau pengalaman manusia. Limitasi: tidak dapat mengukur efektivitas secara numerik.|
| Kesesuaian dengan topik (1–5) | *Contoh: 4 — topik kuantitatif, cocok uji hipotesis* | *Contoh: 2 — topik tidak studi makna/konteks* | *Contoh: 5 — membangun artefak untuk uji klaim* |
| Jenis data yang dikumpulkan | *Metrik numerik, log eksperimen* | *Wawancara, observasi kualitatif* | *Hasil uji artefak, komparasi kinerja* |
| Limitasi paradigma | | | |

**Paradigma yang dipilih:** Design Science Research (DSR) — karena penelitian membangun artefak berupa model hybrid CNN-SVM dan sistem pemantauan real-time sebagai instrumen untuk membuktikan proposisi
**Alasan:** model machine learning yang dilatih pada NSL-KDD dapat mendeteksi anomali lalu lintas jaringan secara real-time dengan performa lebih baik dari pendekatan rule-based statis." Artefak adalah alat pengujian proposisi, bukan tujuan akhir.

---

## Refleksi

> Sebelum membaca materi ini, apakah pernah mempertanyakan klaim "95% akurat"? Setelah memahami rantai distorsi, pertanyaan apa yang sekarang akan diajukan saat membaca paper?

**Jawaban:**
Apa metrik akurasi persisnya (angka)? Apakah ada tabel confusion matrix?
Mengapa NSL-KDD masih relevan untuk 2025?
Bagaimana performa di jaringan berbeda?
Apakah threshold SVM ditentukan sebelum atau setelah melihat data uji?
