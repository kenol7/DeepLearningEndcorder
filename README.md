# DeepLearningEndcorder
Muhammad Ainol Yakin/222401013090
📦 DATASET: SPORTS CLASSIFICATION
Dataset yang digunakan adalah gpiosenka/sports-classification dari Kaggle, berisi gambar berbagai jenis olahraga.
🏷 Kategori yang Dipakai:
bowling
volleyball
tennis
rugby

Langkah Awal:
Dataset didownload menggunakan kagglehub.
Hanya 4 kategori yang diambil, masing-masing gambarnya dipindahkan ke dalam folder baru bernama sports_balls untuk keperluan training.

🧾 STRUKTUR DATASET
Dataset dikustomisasi dalam class SportsBallDataset:
Membaca gambar dari folder yang sudah dipilih.
Melakukan transformasi (resize ke 128x128 dan diubah ke tensor).
Menghasilkan dua output:
Gambar asli (image)
Peta tepi (edge map) menggunakan Sobel filter, hasil dari deteksi tepi terhadap gambar grayscale.

🧠 ARSITEKTUR: EDGE-AUTOENCODER
🎯 Tujuan:
Model autoencoder yang tidak hanya merekonstruksi gambar, tetapi juga memperhatikan informasi tepi, agar hasil lebih tajam dan struktur objek lebih terlihat.
🧱 Encoder:
Conv2D(3 → 64) → ReLU → MaxPool
Conv2D(64 → 128) → ReLU → MaxPool
🔄 Decoder:
ConvTranspose(128 → 64) → ReLU
ConvTranspose(64 → 32) → ReLU
Conv2D(32 → 3) → Sigmoid (untuk mengembalikan nilai pixel antara 0 dan 1)
🌐 Edge Enhancement Path:
Setelah hasil rekonstruksi diperoleh:
Gambar dikonversi ke grayscale.
Diproses oleh jaringan CNN kecil untuk menghasilkan peta tepi sintetis.
Digabungkan dengan hasil rekonstruksi: 0.6 * hasil_rekonstruksi + 0.4 * peta_tepi

🧮 METRIK EVALUASI
Loss Function:
MSE (output vs input) + 0.3 * MSE(edge_output vs target_edge)
⇒ Supaya model juga belajar mempertahankan fitur tepi.

PSNR (Peak Signal-to-Noise Ratio)
⇒ Mengukur seberapa dekat hasil output terhadap target input (dalam skala logaritmik).

SSIM (Structural Similarity Index)
⇒ Mengukur kemiripan struktur antara gambar input dan output.

🏋️‍♂️ TRAINING LOOP
Dilatih selama 15 epoch.
Setiap epoch:
Data diproses dan model diupdate berdasarkan loss total.
Hasil setiap epoch:
Disimpan sebagai gambar (input, output, edge) ke folder results/
Dihitung nilai PSNR dan SSIM untuk visualisasi kualitas

📈 VISUALISASI METRIK
Grafik ditampilkan:
📉 Loss: Menunjukkan penurunan error selama training
🔊 PSNR: Semakin tinggi, hasil rekonstruksi makin bagus
🧩 SSIM: Nilai antara 0–1, makin mendekati 1 makin mirip strukturnya

🖼 VISUAL OUTPUT
Ditampilkan hasil visual akhir dari model:
Baris 1: Gambar input (asli)
Baris 2: Gambar hasil rekonstruksi
Baris 3: Edge map dari input

Setiap epoch hasil ini disimpan di results/comparison_epoch_*.png
