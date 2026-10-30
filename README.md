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

- `jadwal/index.html`: halaman generator, disajikan oleh workflow n8n **24 - Jadwal Bergambar**
  di `https://n8n-hc8x.srv2014501.hstgr.cloud/webhook/jadwal`. PDF-nya berisi papan jadwal tiga
  halaman menurut jam (pagi sebelum 11.00, siang 11.00–17.59, malam mulai 18.00; lebih dari 12
  kegiatan dalam satu bagian berlanjut ke halaman berikutnya), lalu halaman kartu untuk digunting.
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
- Kartu bergambar wajah anak dibuat lewat `POST /webhook/jadwal/buat` (butuh kode akses sekali pakai):
  semua kegiatan sekaligus dengan MiniMax image-01, mengikuti gaya yang dipilih (foto realistis atau kartun).
  Pada gaya realistis, kartu mandi dan ke toilet tetap memakai gambar bawaan berisi benda, tanpa anak.
  Kartu ibadah selalu memakai gambar bawaan (MiniMax belum bisa menjamin gerakan ibadah dan jilbab yang benar).
  Foto anak tidak disimpan di repo ini maupun di riwayat n8n.
