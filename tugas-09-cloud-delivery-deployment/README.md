# Tugas 9 (Pekan 9) — Cloud Delivery & Deployment Model

**Materi terkait:** Karakteristik esensial cloud computing (NIST SP 800-145), Deployment Model (Public/Private/Hybrid/Community), Service Model (IaaS/PaaS/SaaS).

> Bagian 2 mata kuliah ini murni **analisis & desain di atas kertas** — tidak ada biaya cloud sama sekali, karena tugas berhenti di tahap rekomendasi, bukan implementasi nyata (implementasi baru mulai Tugas 11).

## Studi Kasus

Pemerintah ingin membangun **Sistem Data Kependudukan** (nama, NIK, alamat, data biometrik) yang:
- Sangat **rahasia** (data pribadi warga negara, diatur UU Perlindungan Data Pribadi).
- Diakses oleh **ribuan instansi** (dukcapil daerah, kepolisian, bank untuk verifikasi KYC) dengan pola akses yang **tidak rata** (lonjakan saat musim pemilu/pendaftaran sekolah).
- Butuh **skalabilitas** agar tidak downtime saat lonjakan akses, tapi **tidak boleh** datanya berada di infrastruktur yang tidak terkendali penuh oleh negara.

## Tugas Kelompok

1. **Rekomendasikan Deployment Model** (Public / Private / Hybrid / Community Cloud) yang paling tepat untuk sistem ini. Jelaskan alasannya berdasarkan **karakteristik NIST** (on-demand self-service, broad network access, resource pooling, rapid elasticity, measured service) dikaitkan dengan kebutuhan kerahasiaan vs skalabilitas.
2. **Rekomendasikan Service Model** (IaaS / PaaS / SaaS) yang tepat untuk komponen-komponen berbeda dari sistem ini (boleh berbeda service model untuk komponen berbeda, mis. database inti vs portal publik untuk pengecekan status).
3. Jelaskan **minimal 2 risiko spesifik** dari pilihan kalian dan mitigasinya (mis. jika Hybrid Cloud dipilih: risiko sinkronisasi data antara private & public segment).
4. Bandingkan singkat dengan **satu alternatif yang kalian tolak** — kenapa alternatif itu kurang tepat (mis. kenapa Public Cloud murni ditolak, atau kenapa Private Cloud murni dianggap kurang skalabel).

## Analisis 
### 1. Rekomendasi Deployment Model (Ditulis oleh Aziz Faadihilah)

Berdasarkan kebutuhan sistem, kelompok kami memilih **Hybrid Cloud**, yaitu gabungan antara Private Cloud dan Community Cloud. Pemilihan ini didasarkan pada kebutuhan untuk menjaga kerahasiaan data kependudukan sekaligus mendukung akses dari berbagai instansi.

Pembagian penggunaannya adalah sebagai berikut:

- **Private Cloud:** Digunakan untuk menyimpan database utama, seperti nama, NIK, alamat, dan data biometrik. Pengelolaannya berada dalam kendali pemerintah agar informasi kependudukan tidak ditempatkan pada infrastruktur yang tidak dikuasai negara.
- **Community Cloud:** Digunakan sebagai lingkungan bersama untuk layanan API, integrasi, dan akses antarinstansi seperti Dukcapil, kepolisian, serta bank yang memiliki kewenangan.

Penerapan lima karakteristik esensial NIST pada sistem ini:

| Karakteristik NIST | Penerapan pada Sistem |
|---|---|
| On-demand self-service | Pemerintah dapat menambah kapasitas komputasi atau penyimpanan ketika kebutuhan meningkat. |
| Broad network access | Instansi dari berbagai daerah dapat mengakses layanan melalui jaringan yang aman. |
| Resource pooling | Sumber daya Community Cloud dapat digunakan bersama oleh berbagai instansi sesuai hak aksesnya. |
| Rapid elasticity | Kapasitas layanan dapat ditingkatkan ketika terjadi lonjakan permintaan, misalnya saat pemilu atau pendaftaran sekolah. |
| Measured service | Penggunaan sumber daya dan aktivitas akses setiap instansi dapat dipantau untuk membantu pengelolaan sistem. |

Dengan pembagian tersebut, data utama tetap berada dalam Private Cloud, sedangkan Community Cloud membantu menangani kebutuhan akses bersama. Kedua lingkungan harus tetap berada dalam kendali pemerintah, termasuk pengelolaan akses dan keamanannya.

### 2. Rekomendasi Service Model (Ditulis oleh Aziz Faadihilah)

Untuk mendukung fungsi sistem yang berbeda, kelompok kami memilih kombinasi IaaS, PaaS, dan SaaS. Pembagiannya disesuaikan dengan kebutuhan pengelolaan setiap komponen.

| Komponen Sistem | Service Model | Alasan |
|---|---|---|
| Database utama kependudukan | IaaS | Membutuhkan kendali terhadap server, penyimpanan, sistem operasi, dan konfigurasi keamanan. |
| Penyimpanan data biometrik | IaaS | Membutuhkan pengelolaan penyimpanan dan keamanan yang lebih terkontrol. |
| API dan integrasi antarinstansi | PaaS | Memudahkan pengembangan dan pengelolaan layanan yang menghubungkan berbagai instansi. |
| Message queue | PaaS | Membantu mengatur antrean permintaan ketika banyak instansi mengakses sistem bersamaan. |
| Portal layanan instansi | SaaS | Instansi dapat menggunakan aplikasi tanpa harus mengelola server sendiri. |
| Portal pengecekan status masyarakat | SaaS | Masyarakat dapat menggunakan layanan melalui browser tanpa perlu memasang aplikasi khusus. |

Kombinasi ini dipilih karena tidak semua komponen membutuhkan tingkat pengelolaan infrastruktur yang sama. Database dan biometrik membutuhkan kendali lebih besar, sedangkan API serta portal lebih mengutamakan kemudahan pengembangan dan penggunaan. SaaS dalam rancangan ini tetap dapat dijalankan pada infrastruktur milik pemerintah.

### 3. Risiko dan Mitigasi

Penggunaan Hybrid Cloud memiliki beberapa risiko yang perlu diperhatikan, terutama karena sistem menyimpan data pribadi dan digunakan oleh banyak instansi.

*Risiko 1: Kebocoran data melalui API*

Banyaknya instansi yang terhubung dapat meningkatkan risiko penyalahgunaan akses. Misalnya, suatu instansi memperoleh informasi penduduk yang sebenarnya tidak termasuk dalam kewenangannya.

Mitigasi:
- Menerapkan autentikasi dan otorisasi pada setiap permintaan API.
- Menggunakan enkripsi komunikasi.
- Membatasi akses berdasarkan peran dan kewenangan instansi.
- Mencatat aktivitas akses melalui audit log.

*Risiko 2: Ketidaksinkronan data*

Data kependudukan dapat berubah, seperti perubahan alamat atau informasi administrasi. Apabila pembaruan tidak tersampaikan dengan baik antara layanan dan database utama, instansi bisa menggunakan informasi yang sudah tidak sesuai.

Mitigasi:
- Menjadikan Private Cloud sebagai sumber data utama.
- Menggunakan API untuk mengambil informasi terbaru.
- Menerapkan mekanisme sinkronisasi secara terkontrol.
- Melakukan pemeriksaan kesesuaian data secara berkala.

*Risiko 3: Lonjakan akses saat periode tertentu*

Pada saat pemilu atau pendaftaran sekolah, jumlah permintaan dari berbagai instansi dapat meningkat secara bersamaan. Hal ini berpotensi memperlambat layanan atau menyebabkan sistem tidak dapat diakses.

Mitigasi:
- Menerapkan autoscaling pada layanan aplikasi.
- Menggunakan message queue untuk mengatur antrean permintaan.
- Menerapkan rate limiting agar permintaan tidak membebani sistem secara berlebihan.
- Memantau penggunaan sumber daya secara berkala.

### 4. Perbandingan dengan Alternatif yang Ditolak

Alternatif yang tidak dipilih adalah *Public Cloud murni*. Model ini sebenarnya memiliki keunggulan dalam hal skalabilitas dan kemudahan penyediaan sumber daya. Namun, terdapat pertimbangan terkait pengelolaan infrastruktur karena layanan Public Cloud umumnya melibatkan penyedia eksternal.

Hal tersebut menjadi pertimbangan penting karena Sistem Data Kependudukan menyimpan informasi sensitif, termasuk NIK dan data biometrik, yang harus tetap berada dalam kendali penuh pemerintah.

Perbandingan kedua model:

| Aspek | Hybrid Cloud | Public Cloud |
|---|---|---|
| Penyimpanan data utama | Private Cloud dalam kendali pemerintah | Infrastruktur penyedia layanan |
| Akses antarinstansi | Melalui Community Cloud | Melalui layanan Public Cloud |
| Skalabilitas | Memanfaatkan sumber daya bersama dan penyesuaian kapasitas | Memanfaatkan kapasitas dari penyedia layanan |
| Kendali infrastruktur | Dapat dipertahankan pemerintah melalui pengelolaan kedua lingkungan cloud | Sebagian pengelolaan infrastruktur berada pada penyedia layanan |

Berdasarkan perbandingan tersebut, kelompok kami lebih memilih Hybrid Cloud karena dinilai lebih sesuai dengan kebutuhan kerahasiaan data dan kendali infrastruktur pemerintah. Public Cloud murni tidak dipilih bukan karena selalu tidak aman, melainkan karena pengelolaannya kurang sesuai dengan persyaratan khusus pada studi kasus ini.

## Struktur Submission

```
tugas-09-cloud-delivery-deployment/
├── README.md      # Analisis lengkap (isi dengan poin 1-4 di atas)
├── JURNAL.md
└── bukti/          # (opsional) diagram pendukung
```

## Rubrik Penilaian (Tugas 9)

| Komponen | Bobot | Kriteria |
|---|---|---|
| Ketepatan rekomendasi deployment model | 30% | Argumen berbasis karakteristik NIST, bukan sekadar "private cloud lebih aman" tanpa detail |
| Ketepatan rekomendasi service model | 25% | Pemetaan komponen sistem ke IaaS/PaaS/SaaS masuk akal |
| Analisis risiko & mitigasi | 25% | Risiko spesifik ke konteks data kependudukan (bukan risiko generik) |
| Perbandingan dengan alternatif yang ditolak | 20% | Argumen penolakan jelas dan berdasar |

## Batasan Penggunaan AI (Level 2)

Kebijakan **Level 2 (AI Assisted Idea Generation & Structuring)** berlaku — lihat [`../RUBRIK-UMUM.md`](../RUBRIK-UMUM.md). Boleh bertanya ke AI gambaran umum karakteristik NIST/deployment model untuk brainstorming; **tidak boleh** meminta AI menuliskan rekomendasi/analisis akhir yang tinggal ditempel ke `README.md`. Catat pemakaian AI di "Log Penggunaan AI" pada `JURNAL.md`.

- Jawaban yang terasa seperti "template" (bisa dipakai untuk sistem cloud apa saja tanpa menyebut spesifik kependudukan/data pribadi/regulasi) akan dinilai rendah pada komponen ketepatan rekomendasi.
- `README.md` wajib membahas minimal satu skenario tandingan yang kalian buat sendiri (mis. "bagaimana jika instansi daerah tidak punya infrastruktur private cloud sendiri?") beserta jawabannya.
