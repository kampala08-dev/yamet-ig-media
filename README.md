# yamet-ig-media

Media siap posting untuk Instagram **@yametpalembang** (YAMET CDC Palembang Dempo).

Repo ini publik karena Instagram Graph API hanya mau mengambil foto/video dari URL publik.
Isinya hanya konten yang memang akan diposting ke Instagram — jangan taruh data pasien,
dokumen internal, atau file lain di sini.

## Cara kerja

1. File diletakkan di `media/` (JPEG untuk foto/carousel, MP4 untuk Reels).
2. Workflow n8n **20 - IG Media** menyajikan file itu dengan Content-Type yang benar:
   `.../webhook/<id>/ig-media/<nama-file>`
3. Workflow n8n **21 - IG Publish** membuat container di Instagram, cek status,
   lalu publish — publish hanya dilakukan setelah disetujui.

## Aturan nama file

`huruf-kecil-angka-dan-strip.jpg` atau `.mp4` (maks. 80 karakter), misalnya
`gangguan-belajar-spesifik-01.jpg`. Untuk carousel, beri nomor urut 01–10.

Foto: JPEG, rasio 4:5 sampai 1.91:1, maks. 8 MB.
Reels: MP4 H.264 + AAC, 9:16, 3 detik–15 menit.

## Generator jadwal bergambar (`jadwal/`)

- `jadwal/index.html`: halaman generator di **https://jadwal.yametpalembang.io** (GitHub Pages; workflow
  `.github/workflows/pages.yml` menerbitkan folder `jadwal/` setiap ada perubahan di folder itu; DNS
  `jadwal` CNAME ke `kampala08-dev.github.io` di Hostinger). Di sana gambar diambil dari situsnya sendiri
  dan isian orang tua tersimpan otomatis di browser HP (autosave). Link lama
  `https://n8n-hc8x.srv2014501.hstgr.cloud/webhook/jadwal` (workflow n8n **24 - Jadwal Bergambar**)
  mengalihkan ke alamat ini; kartu wajah AI tetap dibuat oleh workflow itu. PDF-nya berisi papan jadwal tiga
  halaman menurut jam (pagi sebelum 11.00, siang 11.00–17.59, malam mulai 18.00; lebih dari 12
  kegiatan dalam satu bagian berlanjut ke halaman berikutnya), lalu halaman kartu untuk digunting.
  Daftar kegiatan di halaman selalu urut menurut jam (hari dihitung mulai 04.00): begitu jam diubah,
  kegiatan pindah ke tempatnya; tombol naik/turun hanya muncul untuk kegiatan yang jamnya sama.
  Foto ayah & ibu (opsional) tampil di judul papan. Saat membuat kartu wajah, foto itu ikut dikirim
  ke MiniMax sehingga kartu belajar bersama orang tua dan dengar dongeng memakai wajah ayah/ibu
  (si kecil tampak dari belakang, karena MiniMax hanya menerima satu wajah acuan per gambar).
  Tiap kartu bersama orang tua bisa dipilih "Bersama ibu" atau "Bersama ayah"; bila hanya satu foto
  diisi, kartu itu memakai ayah/ibu yang sesuai.
- `jadwal/foto/`: gambar bawaan gaya foto realistis (`<kegiatan>-L.jpg` anak laki-laki, `-P.jpg` perempuan),
  dibuat dengan Higgsfield GPT Image 2.5 (varian Sunburst) dari dua foto acuan anak buatan AI, jadi tiap
  kartu menampilkan anak yang sama. Anak digambarkan sedang fokus ke kegiatannya (tidak berpose ke kamera).
  Mandi dan ke toilet hanya berisi benda. Jilbab/mukena anak perempuan menutup semua rambut.
- `jadwal/ikon/`: gambar bawaan gaya kartun (pola nama sama), dibuat dengan MiniMax image-01;
  beberapa diperbaiki dengan GPT Image 2.5 (salat & mengaji anak perempuan, berdoa Buddha).
- Kartu ibadah per agama: Islam (`salat`, `mengaji`), Kristen (`doa_kristen`, `gereja`),
  Buddha (`doa_buddha`, `vihara`). Pilihan agama di halaman menukar kartu ini di template.
  Untuk Kristen/Buddha, kartu "Dengar dongeng" memakai gambar `dongeng_ayah` (ayah yang membacakan).
- Terapi di YAMET Dempo: BT, Preschool Skills, dan Terapi Wicara memakai satu gambar `terapi_kelas`;
  Sensori Integrasi `terapi_si` (ruang SI YAMET), Fisioterapi `terapi_fisio`. Foto dibuat dengan
  Higgsfield Seedream 5.0 Flash, kartunnya dengan MiniMax image-01.
- Toilet training (`toilet_training`) selalu memakai gambar kartun (MiniMax), juga di gaya foto.
- Kegiatan harian dari jadwal anak terapi YAMET: `snack_time`, `cuci_tangan`, `belajar_ortu`
  (bermain dan belajar bersama ibu; untuk Kristen/Buddha memakai `belajar_ortu_ayah`), `motorik_kasar`,
  `jalan_pagi`. Fotonya dibuat dengan Higgsfield Seedream 5.0 Flash, kartunnya dengan MiniMax image-01.
  Template "Hari terapi" mengikuti jadwal harian anak yang terapi pagi di YAMET, dan "Libur terapi"
  mengikuti jadwal akhir pekannya (jalan pagi, lalu bermain bersama orang tua).
- Kartu urutan langkah (`jadwal/foto/urutan/`, `jadwal/ikon/urutan/`, pola `<urutan>-<L|P>-<01..10>.jpg`):
  toilet training (selalu kartun; langkah buka/pakai celana dan membersihkan diri memakai gambar benda,
  tanpa sosok anak), cuci tangan & makan, gosok gigi. Langkahnya mengikuti lembar latihan terapis YAMET.
  Dibuat dengan Seedream 5.0 Lite (jatah Unlimited di aplikasi web Higgsfield) sebagai satu gambar grid
  5 x 2 per urutan, lalu dipotong per panel; beberapa langkah diganti foto close-up tangan. Di PDF, tiap
  urutan yang dipilih menjadi satu halaman: kartu bernomor dan tabel centang Senin–Minggu.
- Kegiatan tambahan: `berenang`, `les_piano`, `ekskul` (ekstrakurikuler, digambarkan kegiatan Pramuka),
  `bela_diri` (karate). Foto dan kartunnya dibuat dengan Seedream 5.0 Lite dari anak acuan yang sama
  (`jadwal/acuan/anak-L.jpg`, `anak-P.jpg`, foto anak buatan AI). Kartu wajah AI untuk kegiatan ini hanya
  dibuat bila kegiatannya ada di jadwal.
- Kartu AAC Emosi (`jadwal/emosi/kartu-aac-emosi-<L|P|P-jilbab>.jpg`, pratinjau `...-kecil.jpg`): papan
  24 emosi bernomor (hijau ✓ perasaan nyaman, merah ✗ tidak nyaman), tiap versi dibuat sekali render dengan
  Higgsfield GPT Image 2.5 dari `tata-letak-papan-emosi.png` (tata letak teks saja) dan anak acuan yang sama
  dengan kartu foto. Halaman memilih versinya otomatis: laki-laki, perempuan, atau perempuan berjilbab bila
  agamanya Islam. Bila dicentang, papan ini menjadi halaman A4 mendatar di akhir PDF.
- Kartu bergambar wajah anak dibuat lewat `POST /webhook/jadwal/buat` (butuh kode akses sekali pakai dan email
  orang tua): semua kegiatan sekaligus dengan MiniMax image-01, mengikuti gaya yang dipilih (foto realistis atau kartun);
  kegiatan tambahan hanya bila ada di jadwal.
  Pada gaya realistis, kartu mandi dan ke toilet tetap memakai gambar bawaan berisi benda, tanpa anak.
  Kartu ibadah selalu memakai gambar bawaan (MiniMax belum bisa menjamin gerakan ibadah dan jilbab yang benar).
- Antrean & email (workflow n8n **24** dan **25**): permintaan langsung dijawab `{token, posisi, perkiraan}` lalu menunggu
  giliran di Data Table *Antrean Jadwal Bergambar*. Satu permintaan diproses dalam satu waktu karena MiniMax dibatasi
  ~10 gambar/menit; maksimal 6 permintaan aktif, 900 kartu per 24 jam, menyerah (email maaf, kode tetap berlaku) bila
  antre lebih dari 45 menit; satu kode tidak bisa dipakai dua permintaan sekaligus. Kartu yang jadi dan isian jadwal (tanpa
  foto) disimpan sementara di Data Table *Hasil Kartu Jadwal*, lalu link `https://jadwal.yametpalembang.io/?hasil=<token>`
  dikirim dari `admin@yametpalembang.io` (alias kotak riokampala@, kredensial SMTP n8n "SMTP YAMET admin").
  Halaman yang tetap dibuka mengecek status lewat `POST /webhook/jadwal/hasil` `{token, aksi: cek|ambil|hapus}` dan
  memasukkan kartu sendiri; link email juga memulihkan jadwal bila dibuka di HP/browser lain. Setelah kartu tersimpan di
  HP (IndexedDB, bisa dihapus dengan tombol "Hapus kartu wajah dari HP ini"), salinan di server langsung dihapus;
  pembersihan tiap jam di workflow 25 menghapus sisa kartu dan antrean yang lebih dari 3 hari dan menandai permintaan
  yang terhenti (email maaf). Foto anak tidak disimpan di repo ini, di Data Table, maupun di riwayat n8n (riwayat
  eksekusi kedua workflow dimatikan; foto hanya ada di memori eksekusi selama antre).
