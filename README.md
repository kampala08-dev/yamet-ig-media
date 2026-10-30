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
  di `https://n8n-hc8x.srv2014501.hstgr.cloud/webhook/jadwal`.
- `jadwal/ikon/`: ikon kegiatan bergaya YAMET (`<kegiatan>-L.jpg` anak laki-laki, `-P.jpg` perempuan),
  dibuat dengan MiniMax image-01.
- Kartu bergambar wajah anak dibuat lewat `POST /webhook/jadwal/buat` (butuh kode akses).
  Foto anak tidak disimpan di repo ini maupun di riwayat n8n.
