GITHUB SETUP (jalankan dari terminal pada folder ethereum-data-mining)

1. Buat repository kosong di GitHub, misalnya bernama ethereum-data-mining.
   Untuk langkah awal yang sederhana, jangan centang opsi menambahkan README, .gitignore, atau license jika file tersebut sudah tersedia secara lokal.

2. Periksa file yang akan masuk ke commit:
   git status --short
   git check-ignore -v "data/raw/Ethereum Historical Data.csv"

3. Inisialisasi Git dan buat commit pertama:
   git init
   git branch -M main
   git add README.md requirements.txt .gitignore notebooks/ data/processed/.gitkeep data/raw/.gitkeep
   git status
   git commit -m "Add Ethereum preprocessing project"

4. Hubungkan ke repository GitHub (ganti URL dengan URL repository milikmu):
   git remote add origin https://github.com/USERNAME/ethereum-data-mining.git
   git remote -v
   git push -u origin main

5. Setelah sumber dan lisensi data diverifikasi:
   - Lengkapi bagian sumber dan lisensi di README.md.
   - Baca ketentuan redistribusi sumber data.
   - Jika boleh memublikasikan hasil olahan, hapus baris data/processed/*.csv dari .gitignore.
   - Jalankan ulang notebook jika perlu, lalu:
       git add README.md .gitignore data/processed/ethereum_cleaned.csv data/processed/ethereum_model_ready.csv
       git status
       git commit -m "Add processed Ethereum datasets"
       git push

Catatan: jangan jalankan `git add .` sebelum memeriksa `git status`, agar data mentah atau file besar tidak ikut terunggah tanpa sengaja.
