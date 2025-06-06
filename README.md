# 🧠 Laporan Proyek Machine Learning - Mafrukhif Dzulfahmil Nur

## 📌 Project Overview

Dalam era digital saat ini, jumlah produk yang tersedia di platform e-commerce semakin melimpah. Hal ini menyulitkan pengguna dalam menemukan produk yang relevan dengan kebutuhan atau preferensinya. Oleh karena itu, sistem rekomendasi menjadi solusi penting dalam meningkatkan pengalaman pengguna dan efisiensi pencarian produk.

Sistem rekomendasi dapat membantu:
- Meningkatkan kepuasan pengguna dengan memberikan saran produk yang relevan
- Meningkatkan konversi penjualan dengan strategi pemasaran yang lebih personal
- Mengelola katalog produk yang besar secara efisien

Menurut Jannach et al. (2016), sistem rekomendasi telah terbukti meningkatkan engagement pengguna dan pendapatan perusahaan secara signifikan di berbagai platform e-commerce.

> Referensi: Jannach, D., Adomavicius, G., Tuzhilin, A., & Karimi, M. (2016). *Recommendation systems – Challenges, insights and research opportunities.* Elsevier Journal of Decision Support Systems, 109.

---

## 🧠 Business Understanding

### Problem Statements

1. Pengguna kesulitan menemukan produk yang relevan dari ribuan pilihan di e-commerce.
2. Platform e-commerce memerlukan sistem otomatis untuk merekomendasikan produk kepada pengguna.

### Goals

1. Membangun sistem rekomendasi produk berbasis konten (content-based filtering) untuk memberikan rekomendasi berdasarkan deskripsi produk.
2. Menghasilkan daftar top-N rekomendasi produk yang mirip dengan produk pilihan pengguna.

### Solution Approach

- **Pendekatan 1: Content-Based Filtering**
  - Menggunakan TF-IDF pada deskripsi produk untuk mengukur kemiripan antar produk menggunakan cosine similarity.
- **Pendekatan 2 (Opsional): Collaborative Filtering**
  - Jika tersedia data interaksi pengguna, bisa digunakan untuk meningkatkan rekomendasi berbasis perilaku.

---

## 📊 Data Understanding

Dataset yang digunakan berjudul **"Ecommerce Product Recommendation Dataset"** dan berasal dari [Kaggle](https://www.kaggle.com/datasets/noorsaeed/ecommerce-products-recommendation-dataset).

### Statistik dan Informasi Data

- Jumlah data produk: 2300+
- Fitur utama: `Name`, `Category`, `Price`, `Rating`, `Reviews`, `Description`
- Fokus utama pada fitur `Description` sebagai basis content-based filtering

### Contoh Variabel

- **Name**: Nama produk (contoh: "Bluetooth Headphones")
- **Description**: Uraian atau fitur produk
- **Category**: Kategori produk (contoh: "Electronics")
- **Rating**: Skor rating dari pengguna (0.0 - 5.0)
- **Reviews**: Jumlah ulasan dari pengguna

---

## 🧹 Data Preparation

Langkah-langkah yang dilakukan:
1. Menghapus data kosong pada kolom `Description`
2. Membersihkan dan menstandardisasi teks
3. Mapping nama produk ke index untuk keperluan pencarian cepat
4. Menurunkan case dan menghapus spasi agar pencarian tidak sensitif

**Tujuan Preparation:**
- Data siap digunakan dalam vektorisasi TF-IDF
- Mencegah error saat pencarian produk
- Memastikan deskripsi tersedia untuk seluruh produk

---

## 🔧 Modeling

### Pendekatan: Content-Based Filtering

- **Model**: TF-IDF Vectorizer + Cosine Similarity
- **Algoritma**:
  1. Transformasi `Description` menjadi TF-IDF matrix
  2. Hitung cosine similarity antar seluruh produk
  3. Ambil top-N skor tertinggi dari hasil similarity (kecuali dirinya sendiri)

### Contoh Hasil Rekomendasi untuk 'Bluetooth Headphones':

```text
Rekomendasi produk mirip dengan 'Bluetooth Headphones':
1. Wireless Noise Cancelling Headphones (Score: 0.789)
2. Bluetooth Earbuds with Mic (Score: 0.751)
3. Over-Ear Bluetooth Headphones (Score: 0.744)
4. Portable Bluetooth Speaker (Score: 0.702)
5. Noise Cancelling Bluetooth Headset (Score: 0.691)
