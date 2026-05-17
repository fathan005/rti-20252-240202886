# WS-05: Variabel & Metrik

> **Bab 5 — Metric, Measurement & Data**

---

## Ringkasan Materi

### Measurement Alignment Model

Setiap pengukuran yang valid harus bisa ditelusuri melalui rantai ini tanpa lompatan logis:

```
Problem → Concept → Variable → Metric → Data → Result
```

### Operationalization = Keputusan Desain

Menerjemahkan konsep abstrak menjadi variabel terukur bukan proses mekanis. "Code quality" yang diukur via SonarQube code smells membawa asumsi implisit. Setiap operasionalisasi harus didokumentasikan dan dijustifikasi.

### Empat Tipe Data (NOIR)

| Tipe | Ciri | Contoh | Operasi Valid |
|------|------|--------|---------------|
| **Nominal** | Kategori, tanpa urutan | Jenis algoritma (RF, SVM, CNN) | Modus, chi-square |
| **Ordinal** | Urutan, interval tidak sama | Skala Likert (1-5) | Median, Spearman |
| **Interval** | Jarak bermakna, tanpa nol absolut | Suhu Celsius | Mean, Pearson, t-test |
| **Ratio** | Jarak bermakna + nol absolut | Waktu eksekusi (ms) | Semua operasi |

Tipe data menentukan uji statistik yang valid. Kebanyakan metrik performa TI = ratio; persepsi pengguna = ordinal.

### Kriteria Pemilihan Metrik

- **Representative** — Mewakili konsep yang diteliti
- **Sensitive** — Cukup peka menangkap perbedaan bermakna (hindari ceiling effect)
- **Feasible** — Bisa dikumpulkan dalam batasan waktu dan biaya

### Pre-registration

Metrik harus ditentukan **sebelum** eksperimen. Memilih metrik setelah melihat data = **p-hacking**. Metrik tambahan yang ditemukan kemudian dilaporkan sebagai *exploratory*, bukan *confirmatory*.

### Primary vs Secondary Metric

- **Primary Metric** — Langsung terikat ke hipotesis, menentukan kesimpulan
- **Secondary Metric** — Pendukung, dilaporkan di samping primary; statusnya suplementer

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Pemilihan metrik | Berdasarkan kebiasaan/tool yang ada | Berdasarkan construct validity |
| Anomali | Dihapus untuk laporan bersih | Diinvestigasi — bisa jadi temuan |
| Kapan dipilih | Setelah sistem jadi (monitoring) | Sebelum eksperimen (by design) |

### Istilah Penting

- **Operationalization** — Transformasi konsep abstrak menjadi variabel terukur
- **Construct Validity** — Sejauh mana pengukuran benar-benar mengukur konsep yang dimaksud
- **Measurement Scale** — Klasifikasi data (NOIR) yang menentukan analisis valid
- **Multi-metric Evaluation** — Menggunakan beberapa metrik untuk menangkap konsep kompleks

---

## Template A.5 — Definisi Variabel, Metrik & Justifikasi

```
VARIABLE & METRIC DEFINITION

Research Question: Apakah model hybrid CNN–SVM mampu mendeteksi anomali lalu lintas jaringan secara real-time dengan akurasi lebih tinggi dibandingkan metode konvensional berbasis rules statis?

| Variabel | Tipe | Konsep | Metrik | Skala | Satuan | Cara Mengukur | Justifikasi |
|----------|------|--------|--------|-------|--------|---------------|-------------|
|  Algoritma klasifikasi| IV   |Pendekatan model ML yang digunakan|Jenis model: CNN–SVM vs. rule-based Snort | Nominal | —   | Kategorikal, ditetapkan sebelum eksperimen |  Variabel manipulasi utama; menentukan apakah hybrid DL lebih unggul dari rules statis  |

|  Akurasi deteksi anomali | DV   | Kemampuan model mengenali paket anomali vs normal | Accuracy, Precision, Recall, F1-Score	 | Ratio | % (0–100)  | Dihitung dari confusion matrix pada data scapy real-time| F1-Score diprioritaskan karena data imbalanced (anomali ≠ normal); akurasi saja misleading |

|Spesifikasi perangkat keras | CV   | Kapasitas komputasi yang tersedia  | RAM, CPU — dikontrol konstan |  Ratio | GB / GHz | Digunakan laptop/PC spesifikasi tetap selama eksperimen | Performa model sensitif terhadap sumber daya; dikontrol agar hasil tidak bias hardware |

Alignment Check:
  RQ →Efektivitas CNN–SVM, Concept →Deteksi anomali, Variable →Algoritma (IV), Akurasi (DV), Metric → F1-Score, Paket anomali/5 min,Data → Log scapy real-time ,Result
  [ya ] Setiap langkah terdokumentasi
  [ ya] Tidak ada "lompatan logis"
  [ ya] Metrik mengukur apa yang dimaksud (construct validity)
```

---

## Latihan 1 — Operationalization Chain

Gunakan RQ dari WS-04. Definisikan variabel dan metriknya.

**RQ:** Apakah model hybrid CNN–SVM dapat mengklasifikasikan lalu lintas jaringan sebagai normal atau anomali secara real-time menggunakan data yang direkam oleh Scapy?

| Variabel | Tipe | Konsep Abstrak | Metrik Konkret | Skala (NOIR) | Satuan |
|----------|------|---------------|----------------|-------------|--------|
| Jenis model klasifikasi | *IV* | Pendekatan ML yang digunakan untuk klasifikasi anomali | Kategorikal: CNN–SVM hybrid vs. rules-based Snort 
| Nominal	 | —|

| Akurasi klasifikasi anomali| DV |Seberapa tepat model membedakan paket normal vs anomali |F1-Score (primary), Accuracy, Precision, Recall (secondary) |Ratio |% (0–100) |

|Volume trafik per interval	 | CV |Intensitas lalu lintas jaringan yang diproses |Jumlah total paket yang dianalisis per 5 menit |Ratio |Paket/interval |

**Apakah ada lompatan logis dalam rantai?** [ ] Ya / [ ] Tidak
> Jika ya, di mana? Tidak ada lompatan logis.
---

## Latihan 2 — Evaluasi Metrik

Evaluasi metrik DV yang dipilih di Latihan 1 menggunakan 3 kriteria.

| Kriteria | Skor (1-5) | Justifikasi |
|----------|-----------|-------------|
| Representative | 4 - 5|F1-Score mewakili keseimbangan precision dan recall, tepat untuk dataset dengan distribusi kelas tidak seimbang (anomali vs normal). Namun tidak mencakup latensi deteksi yang juga penting untuk real-time IDS. |

| Sensitive |4 -5 |F1-Score peka terhadap perubahan performa — false positive dan false negative keduanya memengaruhi nilai. Pada 1958/2323 paket anomali (84%), potensi ceiling effect perlu dipantau; model bisa mencapai F1 tinggi secara artificial jika mayoritas label adalah anomali. |

| Feasible |5-5 |Dapat dihitung langsung dari confusion matrix log scapy setiap 5 menit menggunakan Python (sklearn.metrics). Tidak memerlukan instrumen tambahan atau keahlian domain khusus. |

**Apakah perlu secondary metric?** Ya 
> Jika ya, apa dan mengapa? Secondary metric diperlukan: Jumlah paket anomali per interval 5 menit (ratio, satuan: paket/interval) — memberikan gambaran temporal serangan yang tidak tertangkap F1-Score. Juga: Flag TCP dominan (S0 vs REJ, nominal) untuk memahami jenis perilaku anomali.

**Contoh kasus ceiling effect untuk metrik ini:**
> Contoh ceiling effect: Jika dalam suatu interval 99% paket adalah anomali (SYN Flood sangat deras), model yang selalu memprediksi "anomali" akan mendapat F1 ≈ 0.99 tanpa benar-benar belajar pola. Perlu dipantau melalui precision dan recall secara terpisah.
---

## Latihan 3 — Data Quality Check

Bayangkan data yang akan dikumpulkan dari eksperimen. Evaluasi 4 dimensi kualitas data.

| Dimensi | Pertanyaan | Jawaban | Strategi Mitigasi |
|---------|-----------|---------|------------------|
| Completeness | *Apakah semua data point terkumpul?* |Scapy merekam semua paket yang melewati interface dalam mode promiscuous. Namun ada risiko packet loss saat SYN Flood intensitas tinggi jika buffer penuh. |Monitor jumlah paket per interval; bandingkan dengan counter di switch. Tambahkan log jika terjadi drop pada proses capture.|

| Consistency | *Apakah ada kontradiksi internal?* |Parameter yang dikumpulkan (timestamp, IP src, IP dst, protocol, flag) bersifat deterministik dari header TCP/IP — inkonsistensi rendah. Risiko: jam sistem tidak tersinkronisasi antar PC penyerang dan target.|Sinkronisasi NTP di semua perangkat eksperimen sebelum simulasi dimulai. |

| Validity | *Apakah benar-benar mengukur yang dimaksud?* |Model dilatih pada NSL-KDD (simulasi lab tahun 1999 yang diperbarui) lalu diuji pada data real-time dari simulasi serangan sendiri. Distribusi data latih ≠ distribusi data uji (domain shift) — ancaman terhadap construct validity. 
|Lakukan validasi silang: uji model juga pada sebagian dataset NSL-KDD yang ditahan (hold-out set). Dokumentasikan perbedaan distribusi fitur antara NSL-KDD dan data scapy.|

| Representativeness | *Apakah sampel mewakili populasi target?* |Simulasi hanya menggunakan 3 jenis serangan (SYN Flood SSH/FTP/HTTP) dari 2 perangkat penyerang di jaringan lokal terkontrol. Tidak mencakup zero-day, MITM, atau serangan dari internet nyata. |Perluas skenario serangan di penelitian lanjutan; dokumentasikan batasan generalisasi ini secara eksplisit di bagian limitasi paper.|

---

## Refleksi

> Mengapa memilih metrik setelah melihat data dianggap p-hacking? Apa bedanya dengan eksplorasi data yang sah?

**Jawaban:**
> Ketika peneliti memilih metrik setelah melihat hasil, mereka secara implisit memilih metrik yang kebetulan mendukung kesimpulan yang diinginkan. Dalam konteks penelitian ini: jika akurasi menunjukkan 84% (1958/2323) tetapi F1-Score lebih rendah karena false positive tinggi, peneliti yang tidak jujur bisa memilih "akurasi" setelah melihat data — padahal F1 adalah metrik yang lebih tepat untuk data imbalanced. Ini memanipulasi probabilitas kesimpulan positif tanpa benar-benar meningkatkan performa model.
> Perbedaan dengan eksplorasi data yang sah: eksplorasi boleh menemukan metrik tambahan setelah melihat data, namun wajib dilaporkan sebagai exploratory/secondary — bukan sebagai bukti konfirmatori hipotesis utama. Dalam paper CNN–SVM ini, jika penulis menemukan bahwa "flag S0 dominan" hanya setelah melihat data, temuan itu tetap valid sebagai insight eksplorasi, bukan sebagai uji hipotesis yang direncanakan.
