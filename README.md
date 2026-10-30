# FAM Photonic — Capstone Biomedical Engineering

**Prototype for optical-based, non-invasive hemoglobin and total-cholesterol estimation**  
Program Studi Teknik Biomedis, Institut Teknologi Sumatera (ITERA)

> **Research prototype only — not a medical device for diagnosis.** The current firmware uses provisional estimation equations; no clinical accuracy or validity is established by the files provided in this repository.

## Ringkasan
FAM Photonic adalah proyek capstone yang merancang integrasi sensor **MAX30102** (sinyal merah/inframerah) dan **TCRT5000** (reflektansi inframerah) menggunakan **Arduino Uno**, dengan tampilan OLED. Tujuan penelitian adalah mengembangkan dan mengevaluasi pendekatan estimasi hemoglobin (Hb) dan kolesterol total tanpa pengambilan darah. **Kadar Hb dan kolesterol tidak dapat dipastikan hanya berdasarkan output prototipe tanpa kalibrasi serta validasi terhadap metode rujukan.**

## Isi repository

| Folder | Isi |
|---|---|
| `firmware/` | Sketch Arduino `final.ino` dari tim |
| `docs/` | Presentasi progres capstone dalam format PPTX dan PDF |
| `hardware/` | Tempat skema wiring, foto alat, dan desain casing; lihat `README.md` di folder |
| `data/` | Tempat dataset anonim hasil pengujian dan analisis, belum disertakan |
| `sop/` | Draf panduan pencatatan serta rencana kalibrasi/pengujian |

## Komponen & koneksi menurut firmware
- Arduino Uno (sesuai rancangan presentasi).
- MAX30102 via I²C hardware (`Wire`); untuk Uno, SDA=A4 dan SCL=A5.
- TCRT5000 analog output (AO) menuju A0.
- OLED SH1106 128×64 memakai software I²C: SDA=A2 dan SCL=A3.
- Pengaturan firmware sensor MAX30102: LED brightness `0x1F`, sample average `4`, mode LED `2`, sample rate `400`, pulse width `411`, ADC range `4096`.

**Catatan:** Periksa tegangan dan kompatibilitas logika setiap modul (terutama MAX30102) berdasarkan datasheet serta papan breakout yang digunakan. Jangan menghubungkan modul ke tegangan yang tidak didukung.

## Cara menjalankan firmware
1. Pasang Arduino IDE dan pilih board/port yang sesuai.
2. Instal pustaka **SparkFun MAX3010x Sensor Library** (header `MAX30105.h`) dan **U8g2** (header `U8g2lib.h`) melalui Library Manager.
3. Buka `firmware/final.ino` dengan Arduino IDE, lalu verifikasi dan unggah ke board.
4. Buka Serial Monitor pada **115200 baud** dan amati pembacaan sensor serta OLED.

## Status perhitungan (kode terlampir)
- Hb: nilai dihitung dari intensitas `red` dan `ir` menggunakan persamaan heuristik pada firmware; ditampilkan saat `ir > 50000`. Output dibatasi pada **11,0–17,5 g/dL**. Ini **bukan** hasil kalibrasi klinis.
- Kolesterol: `0.35 × ADC + 120.0` (`ADC` dari TCRT5000 di A0). Koefisien tersebut tertulis dalam kode, tetapi belum didukung dataset kalibrasi pada lampiran yang tersedia.
- Firmware membaca sensor, menampilkan perkiraan di OLED dan Serial Monitor, lalu menunda pembacaan selama 10 detik. Kode belum menerapkan moving-average filter dan prosedur regresi/validasi sebagaimana direncanakan di presentasi.
- Angka yang tampil **tidak boleh digunakan untuk keputusan medis**.

## Tahap pengembangan berikutnya
1. Verifikasi koneksi, catu daya, dan mutu sinyal.
2. Dokumentasi sistem akuisisi serta data mentah yang dapat direproduksi.
3. Kalibrasi estimasi terhadap metode rujukan yang relevan, dengan protokol dan persetujuan yang diperlukan.
4. Uji galat, bias, presisi, repeatability, dan generalisasi pada subjek yang tidak dipakai untuk kalibrasi.
5. Evaluasi batasan metode; jangan mengklaim kemampuan skrining klinis sebelum bukti memadai.

## Tim dan pembimbing
- M. Mughni Ass Shodiq
- M. Fadhil Ananta Putra
- Muhammad Albar

Pembimbing: Doni Bowo Nugroho, S.Pd., M.Sc.

## Catatan publikasi dan privasi
Jangan mengunggah data kesehatan yang dapat mengidentifikasi peserta, formulir persetujuan yang ditandatangani, atau informasi pribadi tanpa izin dan proses etika yang tepat. Pastikan persetujuan seluruh kontributor sebelum membuat repository publik. **Lisensi belum dipilih**, sehingga hak penggunaan kembali kode/dokumen belum diberikan secara otomatis.
