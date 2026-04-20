# Analisis sentimen review aplikasi Shopee di Google Playstore
Proyek ini melakukan analisis sentimen terhadap ulasan pengguna aplikasi Shopee di Google Play Store menggunakan metode machine learning dengan tiga skema pelatihan yang berbeda.

Dataset diperoleh melalui scraping ulasan aplikasi Shopee (`com.shopee.id`) dari Google Play Store menggunakan library `google-play-scraper`. Ulasan kemudian diproses dan dilabeli secara otomatis menggunakan lexicon sentimen bahasa Indonesia, dan dilatih menggunakan tiga kombinasi model dan ekstraksi fitur yang berbeda.

## Alur Kerja

1. Scraping, mengambil 10.000 ulasan terbaru aplikasi Shopee dari Google Play Store dan menyimpannya sebagai `dataset.csv`
2. Load Dataset & Lexicon, membaca dataset hasil scraping serta lexicon kata positif dan negatif dari file `.tsv`
3. Preprocessing Lexicon, melakukan normalisasi, penghapusan frasa (hanya kata tunggal), dan stemming pada kata-kata di lexicon agar sesuai dengan format teks ulasan
4. Preprocessing Teks:
   - Konversi emoji ke teks
   - Penghapusan URL dan angka
   - Case folding (huruf kecil)
   - Tokenisasi
   - Normalisasi pengulangan karakter
   - Normalisasi slang/singkatan
   - Penghapusan stopwords
   - Stemming menggunakan Sastrawi
5. Labelling, pelabelan otomatis menggunakan lexicon sentimen dengan mempertimbangkan negasi (`tidak`, `bukan`, `gak`, dll.) dan intensifier (`sangat`, `banget`, `sekali`, dll.) untuk menghasilkan label positif, negatif, atau netral
6. Split Data, membagi dataset menjadi dua skema pembagian: 80/20 untuk Skema 1 & 3, dan 70/30 untuk Skema 2
7. Balancing, melakukan oversampling pada kelas minoritas (negatif dan netral) menggunakan resample agar distribusi data training seimbang
8. Pelatihan Model, melatih tiga skema model dengan kombinasi algoritma dan ekstraksi fitur yang berbeda
9. Evaluasi, mengukur performa tiap skema menggunakan accuracy score dan classification report (precision, recall, f1-score)
10. Inference, menguji model dengan kalimat baru untuk memverifikasi prediksi sentimen secara langsung

## Skema Pelatihan

| Skema | Model | Ekstraksi Fitur | Pembagian Data | Akurasi |
|-------|-------|-----------------|----------------|---------|
| 1 | Logistic Regression | TF-IDF | 80/20 | 85.4% |
| 2 | SVM (LinearSVC) | TF-IDF | 70/30 | 86.4% |
| 3 | Logistic Regression | CountVectorizer | 80/20 | 85.1% |

## Kelas Sentimen

- Positif: Ulasan yang mengandung kata-kata bernada positif
- Negatif: Ulasan yang mengandung kata-kata bernada negatif
- Netral: Ulasan yang tidak mengandung kata sentimen atau skornya seimbang

## Cara Menjalankan

1. Jalankan `scraping.py` untuk mengambil dataset terbaru
2. Siapkan file lexicon `positive.tsv` dan `negative.tsv`
3. Buka dan jalankan semua cell di `training.ipynb` secara berurutan
