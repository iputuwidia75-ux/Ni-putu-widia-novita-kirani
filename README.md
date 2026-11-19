# Akuisisi Data Akomodasi Pariwisata Kabupaten Badung (RTM Tugas 1)

- **Nama:** Ni Putu Widia Novita Kirani
- **NIM:** 2501010165
- **Program Studi:** Informatika — Institut Bisnis dan Teknologi Indonesia (INSTIKI)
- **Mata Kuliah:** Data Acquisition & Management
- 
## 1. Sumber Data
- **Nama Sumber:** OpenStreetMap (OSM) melalui Overpass API — Kategori Akomodasi Pariwisata Kabupaten Badung
- **URL Endpoint API:** `[https://overpass-api.de/api/interpreter](https://overpass-api.de/api/interpreter)`
- **Penerbit / Pengelola Data:** Komunitas Kontributor OpenStreetMap / OpenStreetMap Foundation (OSMF)
- **Alasan Pemilihan Sumber:** OpenStreetMap menyediakan data spasial dan direktori fasilitas pariwisata terbuka yang terstruktur, legal diakses tanpa API key, serta relevan dengan sektor pariwisata di Kabupaten Badung, Bali.

## 2. Tanggal dan Jam Akses
- **Tanggal Akses:** 15 November 2026 (WITA / UTC+8 — detail stempel waktu ISO-8601 tercatat pada `docs/provenance.json`).

## 3. Metode Akuisisi
- **Metode:** REST API (`POST` request ke Overpass API menggunakan Overpass QL).
- **Endpoint Utama:** `[https://overpass-api.de/api/interpreter](https://overpass-api.de/api/interpreter)` (dilengkapi mekanisme fallback ke `overpass.kumi.systems` dan `maps.mail.ru` dengan pengecekan status code HTTP).
- **Parameter Query Overpass QL:** Format keluaran `[out:json][timeout:60]`, area `name="Badung"` (`admin_level="5"`), filter `node` dan `way` dengan tag `tourism` (`hotel|resort|guest_house|hostel|villa|chalet`), keluaran `out center tags;`.

## 4. Batasan Etika
- **Pemeriksaan robots.txt:** Telah diperiksa menggunakan `urllib.robotparser` dengan hasil `Diizinkan: True`.
- **Lisensi & Atribusi:** Open Database License (ODbL) — © OpenStreetMap contributors.
- **Jeda Antar-Request & Timeout:** Menerapkan jeda `1.5` hingga `2` detik (`time.sleep`) antar-permintaan HTTP serta batas waktu koneksi `timeout=65` detik agar tidak membebani server publik.
- **Identitas Pengakses:** Menggunakan header `User-Agent` jujur yang mencantumkan nama mahasiswa dan email kampus (`2501010165@instiki.ac.id`).
- **Data yang Sengaja Tidak Diambil:** Seluruh atribut yang berpotensi memuat data pribadi perorangan (seperti `phone`, `contact:phone`, `email`, `contact:email`, `operator`, dan akun media sosial pribadi) tidak diekstrak ke dalam dataset sesuai UU PDP No. 27 Tahun 2022.

## 5. Penjelasan Kolom Dataset
Berkas `data/raw/overpass_badung_akomodasi_20261115.csv` terdiri dari **2.540 record** dan **10 kolom**:
1. `osm_id`: Nomor identifikasi unik objek pada basis data OpenStreetMap.
2. `tipe_objek`: Tipe elemen spasial pada OpenStreetMap (`node` untuk titik koordinat tunggal atau `way` untuk area/bangunan poligon).
3. `nama_akomodasi`: Nama resmi hotel, resort, vila, pondok wisata, atau penginapan yang terdaftar.
4. `kategori_wisata`: Klasifikasi jenis akomodasi berdasarkan tag `tourism` (`hotel`, `resort`, `guest_house`, `hostel`, `villa`, atau `chalet`).
5. `bintang`: Klasifikasi kelas bintang hotel (`stars`), jika tersedia pada metadata objek.
6. `kecamatan_kota`: Informasi wilayah kecamatan/kota lokasi akomodasi di Kabupaten Badung.
7. `alamat_jalan`: Nama jalan lokasi akomodasi (`addr:street`), jika tersedia.
8. `website`: Tautan situs web resmi akomodasi pariwisata, jika tersedia.
9. `latitude`: Koordinat garis lintang (derajat desimal WGS84).
10. `longitude`: Koordinat garis bujur (derajat desimal WGS84).

## 6. Cara Menjalankan Ulang Notebook
1. Pastikan Python 3.9+ telah terpasang beserta pustaka `requests`, `pandas`, dan `jupyter` (`pip install requests pandas jupyter`).
2. Buka berkas `notebooks/akuisisi_overpass_badung.ipynb` menggunakan Jupyter Notebook, VS Code, atau Google Colab.
3. Jalankan seluruh sel dari awal hingga akhir (**Run All**).
4. Hasil akuisisi mentah akan tersimpan otomatis di `data/raw/overpass_badung_akomodasi_<YYYYMMDD>.csv` beserta metadata integritas di `docs/provenance.json`.
