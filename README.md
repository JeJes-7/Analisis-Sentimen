# Analisis Sentimen Ulasan Aplikasi Bibit di Google Play Store

Proyek ini mengklasifikasikan sentimen (positif/negatif) ulasan pengguna aplikasi
Bibit menggunakan pipeline NLP dan membandingkan empat algoritma machine learning
untuk menentukan model terbaik.

Presentasi: https://canva.link/ur8kkmgktivykwm

## Latar Belakang
Ulasan pengguna di Google Play Store mencerminkan pengalaman dan kepuasan pengguna,
tetapi jumlahnya terlalu besar untuk diolah manual. Proyek ini membangun klasifikasi
sentimen otomatis dan membandingkan beberapa algoritma dalam satu alur yang sama.

## Data
- Sumber: Google Play Store (`com.bibit.bibitid`), diambil dengan `google-play-scraper`
- Jumlah: 5.000 ulasan terbaru, 25 Maret 2025 – 27 Oktober 2026
- Label otomatis dari rating: 4–5 = Positif, 1–2 = Negatif, 3 = Netral (dikeluarkan)
- Data akhir: 4.696 ulasan (3.488 positif / 74,3%, 1.208 negatif / 25,7%)

## Alur Penelitian
1. Scraping dan pelabelan berdasarkan rating
2. Data understanding (distribusi rating, sentimen, kualitas data)
3. Preprocessing teks: cleaning, case folding, normalisasi slang, tokenisasi,
   stopword removal, stemming (Sastrawi)
4. TF-IDF (unigram + bigram, max 5.000 fitur), di-fit hanya pada data latih
5. Pemodelan dengan Pipeline pada rasio 60:40, 70:30, 80:20, 90:10
6. Evaluasi: akurasi, presisi/recall/F1 macro, recall per kelas, confusion matrix

Ketidakseimbangan kelas ditangani dengan `class_weight='balanced'` (Random Forest,
Logistic Regression, Decision Tree) dan `fit_prior=False` (Naïve Bayes).

## Hasil (rasio 90:10)
| Algoritma | Akurasi | F1 Macro | Recall Negatif | Recall Positif |
|---|---|---|---|---|
| Naïve Bayes | 86,60% | 84,50% | 96,69% | 83,09% |
| Random Forest | 91,06% | 88,44% | 84,30% | 93,41% |
| Logistic Regression | 90,21% | 88,00% | 91,74% | 89,68% |
| Decision Tree | 88,30% | 84,66% | 76,86% | 92,26% |

Rasio 90:10 menghasilkan F1 macro tertinggi pada keempat algoritma. Logistic
Regression dipilih sebagai model paling sesuai karena konsisten di semua rasio
(F1 macro 86,83–88,00%) dan paling seimbang antar kelas. Random Forest unggul
tipis pada 90:10, tetapi selisihnya kecil karena data uji hanya 470 ulasan.

## Keterbatasan
- Label berasal dari rating, sehingga dapat mengandung noise
- Evaluasi memakai satu kali pembagian data (`random_state=42`)
- Data terbatas pada 5.000 ulasan terbaru, bukan seluruh ulasan sejak rilis

Untuk pengembangan selanjutnya mungkin bisa ditambahkan penggunaan aplikasi sederhana (Streamlit/Flask) atau dashboard agar hasil sentimen bisa dipantau secara berkala, pertimbangkan 3 kelas, serta penanganan data imbalance yg lain.

## Penulis
Jesika Yudiani Putri
