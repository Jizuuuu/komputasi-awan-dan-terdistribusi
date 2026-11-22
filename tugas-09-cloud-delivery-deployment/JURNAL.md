# Jurnal Proses — Tugas 9

## Diskusi deployment model

- Opsi yang dipertimbangkan adalah Private Cloud, Public Cloud, Community Cloud, dan Hybrid Cloud. Private Cloud memiliki kelebihan dalam hal kendali terhadap data, tetapi pengelolaan kapasitas untuk melayani ribuan instansi menjadi pertimbangan. Public Cloud menawarkan skalabilitas, tetapi perlu mempertimbangkan kendali pemerintah terhadap infrastruktur. Community Cloud mendukung penggunaan sumber daya bersama oleh beberapa instansi.
- Setelah mempertimbangkan kebutuhan kerahasiaan dan skalabilitas, kelompok mengarahkan pilihan pada Hybrid Cloud, yaitu kombinasi Private Cloud dan Community Cloud. Private Cloud digunakan untuk menyimpan data utama kependudukan, sedangkan Community Cloud mendukung layanan bersama dan integrasi antarinstansi.
- Pemilihan ini juga mempertimbangkan bahwa kedua lingkungan cloud harus tetap berada dalam kendali pemerintah, terutama dalam pengelolaan data dan hak akses.

## Diskusi service model per komponen

- Kelompok kami mempertimbangkan penggunaan kombinasi IaaS, PaaS, dan SaaS karena setiap komponen memiliki kebutuhan pengelolaan yang berbeda.
- IaaS dipilih untuk database utama dan penyimpanan biometrik karena membutuhkan kendali lebih besar terhadap infrastruktur dan penyimpanan.
- PaaS dipertimbangkan untuk API, integrasi antarinstansi, dan message queue karena dapat membantu pengembangan serta pengelolaan layanan.
- SaaS digunakan untuk portal instansi dan portal pengecekan status masyarakat agar pengguna dapat mengakses aplikasi tanpa mengelola server sendiri.
- Pembagian tersebut disesuaikan dengan fungsi setiap komponen dan kebutuhan keamanan sistem.

## Log Penggunaan AI (Level 2)

| Tanggal | Tool AI | Prompt yang diberikan | Ringkasan saran/ide AI | Bagaimana diolah jadi tulisan/kode sendiri |
|---|---|---|---|---|
| 22-11-2026 | ChatGPT | Apa perbedaan deployment model Public, Private, Hybrid, dan Community Cloud serta model yang sesuai untuk sistem data kependudukan? | Hybrid Cloud dapat dipertimbangkan untuk menggabungkan kendali data melalui Private Cloud dan penggunaan sumber daya bersama melalui Community Cloud. | Ide tersebut digunakan sebagai bahan pertimbangan. Kelompok membandingkan kelebihan dan kekurangan setiap model berdasarkan kebutuhan sistem. |
| 22-11-2026 | ChatGPT | Bagaimana penerapan lima karakteristik esensial NIST pada sistem data kependudukan yang diakses banyak instansi? | Setiap karakteristik NIST dapat dihubungkan dengan kebutuhan akses, pembagian sumber daya, peningkatan kapasitas, dan pemantauan penggunaan sistem. | Kelompok menghubungkan setiap karakteristik dengan contoh kasus seperti lonjakan akses saat pemilu dan penggunaan layanan oleh Dukcapil serta kepolisian. |
| 22-11-2026 | ChatGPT | Komponen apa saja yang cocok menggunakan IaaS, PaaS, dan SaaS dalam sistem data kependudukan? | IaaS dapat dipertimbangkan untuk database, PaaS untuk API dan integrasi, sedangkan SaaS untuk portal pengguna. | Hasil pemetaan dijadikan bahan diskusi untuk menentukan service model berdasarkan fungsi dan kebutuhan pengelolaan setiap komponen. |
| 22-11-2026 | ChatGPT | Apa saja risiko yang perlu dipertimbangkan dari penggunaan Hybrid Cloud untuk sistem data kependudukan? | Beberapa risiko yang perlu diperhatikan adalah kebocoran melalui API, ketidaksinkronan data, lonjakan akses, dan penyalahgunaan hak akses. | Kelompok memilih risiko yang paling relevan dengan studi kasus dan mengembangkan mitigasinya berdasarkan kebutuhan keamanan data kependudukan. |
