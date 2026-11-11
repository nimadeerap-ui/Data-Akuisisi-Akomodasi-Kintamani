# Akuisisi Data Akomodasi di Kecamatan Kintamani

Tugas Praktik BIFP-243 Akuisisi dan Manajemen Data, INSTIKI.
Nama/NIM: Era / 2501010007

## Tujuan
Mengakuisisi data akomodasi (hotel, guest house, hostel, motel, apartemen, chalet) di Kecamatan Kintamani dari OpenStreetMap, menyimpan respons mentah, membentuk dataset tabular terdokumentasi, dan menilai kelengkapannya terhadap angka resmi BPS.

## Sumber data
- OpenStreetMap melalui Overpass API (https://overpass-api.de/api/interpreter), lisensi ODbL.
- Pembanding: BPS Kabupaten Bangli, Kecamatan Kintamani Dalam Angka 2024, Tabel 6.1.1 (hal. 93-94), data tahun 2023 (hotel 19, penginapan 99; total 118).

## Tanggal akses
8 November 2026 (waktu UTC tepatnya ada di docs/provenance.json).

## Metode akuisisi
Data akomodasi diambil lewat Overpass API menggunakan Python (requests) dengan query pada docs/query_overpass.txt, dilengkapi timeout 120 detik dan retry berjeda. Respons mentah disimpan tanpa diubah di data/raw/, lalu diratakan menjadi data/interim/akomodasi_kintamani.csv tanpa kolom kontak. Batas wilayah Kecamatan Kintamani dicek dengan query terpisah (docs/query_batas_kintamani.txt) dan secara visual di overpass-turbo (tangkapan layar ada di reports/).

Catatan: docs/provenance.json dibuat dari berkas mentah yang sudah tersimpan; waktu akuisisi diambil dari stempel pada nama berkas.

## Hasil singkat
- Terkumpul 32 elemen akomodasi dari OpenStreetMap, dengan rincian jenis: hotel 16, guest_house 9, hostel 4, chalet 2, motel 1.
- Dibandingkan dengan angka BPS (118 akomodasi, data 2023), kelengkapannya sekitar 27.1%. Perbandingan ini kasar karena definisi hotel dan penginapan BPS tidak sama dengan tag OSM, dan tahun datanya berbeda.
- Dari verifikasi manual 10 sampel acak: 10 nama sesuai, 10 jenis sesuai, dan 10 lokasi sesuai (rincian di docs/verifikasi_sampel.csv).
- Temuan kualitas data: kolom desa kosong 100%, kolom bintang, jumlah_kamar, dan website hampir selalu kosong, dan sebagian akomodasi bernama "Guesthouse" bertag hotel.

## Batasan etika
- Hanya sumber yang diizinkan; tidak ada scraping OTA atau halaman Google Maps.
- Data kontak (telepon, email, media sosial) tidak diambil.
- Berkas di data/raw/ tidak diubah setelah disimpan (hash MD5 di provenance.json).
- Batas pemakaian Overpass dihormati.
- Detail: docs/data_ethics_checklist.csv.

## Atribusi
Data © OpenStreetMap contributors, tersedia di bawah lisensi ODbL (https://www.openstreetmap.org/copyright).

## Cara menjalankan ulang
1. Buka notebooks/akuisisi_akomodasi.ipynb di Google Colab.
2. Pastikan folder proyek berada di Google Drive: MyDrive/akomodasi_kintamani_2501010007. Jika mengambil dari GitHub, salin folder repository ke lokasi tersebut terlebih dahulu.
3. Pilih Runtime > Restart session and run all.
4. Jika berkas mentah sudah ada di data/raw/, notebook memakainya ulang dan tidak mengirim request baru. Untuk mengambil data baru, hapus berkas tersebut lebih dulu dan jangan menjalankan berulang-ulang.

## Struktur folder
- data/raw/: respons mentah Overpass (akomodasi dan batas wilayah)
- data/interim/akomodasi_kintamani.csv: dataset tabular
- notebooks/akuisisi_akomodasi.ipynb: notebook akuisisi
- docs/: query_overpass.txt, query_batas_kintamani.txt, provenance.json, data_dictionary.csv, data_ethics_checklist.csv, perbandingan_bps.csv, verifikasi_sampel.csv
- reports/: laporan PDF dan gambar peta
- README.md
