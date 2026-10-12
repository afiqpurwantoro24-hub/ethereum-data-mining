# Preprocessing Data Historis Ethereum

**Mata kuliah:** Data Mining  
**Metodologi:** CRISP-DM  
**Status sumber dan lisensi:** **belum terverifikasi** — lengkapi bagian ini sebelum memublikasikan data.

## 1. Deskripsi proyek

Proyek ini melakukan audit kualitas dan preprocessing pada dataset harga historis Ethereum dari file `Ethereum Historical Data.csv`. Tujuan analisis yang realistis adalah menyiapkan data untuk eksplorasi tren harga dan eksperimen forecasting harga penutupan (`Price`) pada tanggal berikutnya. Notebook ini **belum mengklaim model forecasting yang akurat**; evaluasi model harus dilakukan terpisah dengan pembagian data berdasarkan waktu.

## 2. Sumber dan lisensi data

- **Nama file:** `Ethereum Historical Data.csv`
- **Platform/sumber asli:** [belum terverifikasi — isi nama situs atau penyedia yang benar-benar menjadi sumber unduhan]
- **URL halaman dataset/unduhan:** [belum terverifikasi — tambahkan tautan sumber resmi]
- **Lisensi/izin redistribusi:** [belum terverifikasi — periksa ketentuan sumber sebelum mengunggah data mentah maupun hasil olahan]
- **Periode yang teramati dalam file:** 11 Oktober 2021–11 Oktober 2026
- **Ukuran awal:** 1.827 baris × 7 kolom

Jangan menyatakan data berlisensi terbuka tanpa bukti. Simpan sumber, tanggal unduh, dan ketentuan lisensi di bagian ini. File mentah dan hasil olahan dikecualikan dari Git untuk sementara sampai hak publikasinya dikonfirmasi.

## 3. Tujuan berdasarkan CRISP-DM

### Business Understanding

Menyiapkan data harga historis Ethereum agar dapat digunakan untuk memahami pola harga dan menjadi masukan eksperimen forecasting. Keberhasilan tahap ini diukur dari tipe data yang benar, kualitas data yang diaudit, keputusan preprocessing yang terdokumentasi, dan output yang dapat direproduksi — bukan dari klaim keuntungan investasi.

### Data Understanding

Kolom awal: `Date`, `Price`, `Open`, `High`, `Low`, `Vol.`, dan `Change %`. File menggunakan pemisah titik koma (`;`) dan pada saat dibaca semua kolom bertipe teks. Format volume memiliki akhiran seperti `K`, `M`, atau `B`, sedangkan harga memakai koma sebagai pemisah ribuan.

### Data Preparation

- Membaca CSV dengan delimiter `;` yang sesuai.
- Mengonversi `Date` menjadi datetime; mengonversi harga menjadi numerik.
- Mengonversi akhiran volume `K`, `M`, dan `B` menjadi pengali numerik.
- Mengonversi `Change %` menjadi angka dalam satuan persen (misalnya `1.25%` menjadi `1.25`, bukan proporsi `0.0125`).
- Menghapus hanya baris yang tanggalnya invalid dan baris yang identik sepenuhnya, jika ditemukan.
- Memeriksa konsistensi OHLC. Pelanggaran ditandai, bukan otomatis dibuang.
- Menggunakan IQR untuk menandai outlier, bukan menghapusnya otomatis.
- Membuat fitur kalender (`year`, `month`, `day_of_week`).
- Menyediakan dataset eksperimen dengan fitur scaling terpisah. Scaler di-fit pada bagian awal deret waktu untuk mengurangi risiko kebocoran informasi masa depan.

## 4. Ringkasan before/after

Hasil berikut berasal dari eksekusi notebook pada file yang tersedia di proyek ini.

| Tahap | Shape (baris, kolom) | Total missing | Baris duplikat | Catatan |
|---|---:|---:|---:|---|
| CSV dibaca dengan delimiter benar | 1.827 × 7 | 0 | 0 | Semua kolom awal bertipe `object` |
| Setelah parsing tipe data | 1.827 × 7 | 0 | 0 | Tanggal menjadi datetime; enam kolom lain numerik |
| Setelah audit/penambahan flag kualitas | 1.827 × 17 | 0 | 0 | Pelanggaran OHLC ditandai, tidak ditemukan pada audit ini |
| Dataset cleaned dengan fitur kalender | 1.827 × 20 | 0 | 0 | Nilai ekstrem tidak dihapus otomatis |
| Dataset model-ready | 1.827 × 25 | 0 | 0 | 1.461 baris awal untuk train dan 366 baris akhir untuk test pada pemisahan contoh |

Audit IQR menandai **120 nilai volume** dan **117 nilai `Change %`** sebagai kandidat outlier. Nilai pada kolom harga OHLC tidak ditandai oleh aturan IQR pada eksekusi ini. Flag outlier bukan bukti bahwa observasi salah; periksa konteks pasar sebelum memutuskan penanganan.

Tidak ada missing values pada file yang dibaca dan diparse dengan benar. Nilai volume berakhiran `K/M/B` bukan missing values; nilai tersebut dikonversi sesuai pengalinya. Karena itu, imputasi volume tidak diperlukan pada eksekusi ini.

## 5. Cara menjalankan

### Persiapan lokal

1. Pastikan Python 3.10 atau lebih baru tersedia.
2. Buat dan aktifkan virtual environment (opsional tetapi disarankan).
3. Pasang dependensi:

   ```bash
   pip install -r requirements.txt
   ```

4. Letakkan file sumber bernama persis `Ethereum Historical Data.csv` di `data/raw/`.
5. Buka Jupyter dan jalankan notebook dari atas ke bawah:

   ```bash
   jupyter notebook notebooks/ethereum_preprocessing_crisp_dm.ipynb
   ```

6. Output lokal ditulis ke `data/processed/ethereum_cleaned.csv` dan `data/processed/ethereum_model_ready.csv`.

### Google Colab

Unggah notebook ke Colab, lalu jalankan sel secara berurutan. Jika path relatif tidak tersedia, notebook menyediakan opsi unggah file. Unduh output sebelum sesi Colab berakhir atau simpan ke Google Drive.

## 6. Struktur folder

```text
ethereum-data-mining/
├── data/
│   ├── raw/                 # CSV asli (tidak di-commit secara default)
│   └── processed/           # output preprocessing (periksa lisensi sebelum publikasi)
├── notebooks/
│   └── ethereum_preprocessing_crisp_dm.ipynb
├── README.md
├── requirements.txt
└── .gitignore
```

## 7. Batasan dan keputusan

- Rentang tanggal dan statistik di atas menjelaskan file yang sedang dianalisis, bukan jaminan bahwa sumbernya resmi atau lengkap.
- Harga ekstrem pada aset kripto dapat merupakan pergerakan pasar yang valid. Notebook hanya memberi flag IQR dan tidak menghapusnya otomatis.
- Fitur scaling hanya untuk eksperimen model. Gunakan kolom harga asli untuk interpretasi.
- Pembagian train/test untuk deret waktu harus kronologis; jangan mengacak baris.
- Sebelum memublikasikan CSV, verifikasi asal data, lisensi, dan ketentuan redistribusi.

## 8. Lingkungan

Versi pustaka yang dipakai dicantumkan di `requirements.txt`. Untuk reproduksibilitas yang lebih ketat, catat versi Python dan sistem operasi saat notebook dijalankan.
