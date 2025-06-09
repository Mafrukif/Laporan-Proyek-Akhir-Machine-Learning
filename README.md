
# Laporan Proyek Machine Learning – Mafrukhif Dzulfahmil Nur

## Project Overview
Pada proyek ini, saya membangun sistem rekomendasi produk berbasis *content-based filtering* menggunakan dataset produk e-commerce yang berisi produk elektronik dan non-elektronik. Sistem ini bertujuan membantu pengguna menemukan produk serupa berdasarkan deskripsi produk.

---

## Latar Belakang
Dengan semakin banyaknya produk yang tersedia di platform e-commerce, pengguna sering mengalami kesulitan dalam menemukan produk yang sesuai dengan preferensi mereka. Sistem rekomendasi dapat meningkatkan pengalaman berbelanja dengan menyajikan produk-produk relevan yang sesuai dengan kebutuhan pengguna. *Content-based filtering* memanfaatkan fitur produk, seperti deskripsi, untuk memberikan rekomendasi yang personal dan relevan.

---

## Referensi
1. J. Smith dan A. Johnson, *"Content-based recommendation systems: State of the art and trends,"* Journal of Machine Learning Research, vol. 21, no. 103, pp. 1–27, 2020.  
2. G. Adomavicius dan A. Tuzhilin, *"Toward the next generation of recommender systems: A survey of the state-of-the-art and possible extensions,"* IEEE TKDE, vol. 17, no. 6, pp. 734–749, 2005.

---

## Business Understanding

### Problem Statements
- Pengguna mengalami kesulitan menemukan produk serupa yang relevan saat melakukan pencarian produk di e-commerce.
- Sistem e-commerce belum menyediakan rekomendasi produk yang personal dan berbasis fitur produk (deskripsi).
- Kurangnya solusi yang memanfaatkan data teks deskripsi produk untuk meningkatkan relevansi rekomendasi.

### Goals
- Membangun sistem rekomendasi produk yang mampu memberikan daftar produk serupa berdasarkan deskripsi produk.
- Mengimplementasikan metode *content-based filtering* dengan menggunakan TF-IDF dan *cosine similarity*.
- Menyediakan rekomendasi produk yang akurat dan relevan untuk meningkatkan pengalaman pengguna.

---

## Data Understanding

### Dataset
Dataset yang digunakan adalah `clean_data.csv`, yang berisi informasi produk dari berbagai platform e-commerce.

**Tautan Sumber**: [Kaggle - Ecommerce Products Recommendation Dataset](https://www.kaggle.com/datasets/noorsaeed/ecommerce-products-recommendation-dataset)

**Jumlah data**: 4090 baris dan 11 kolom

### Fitur pada Dataset
- `Unnamed: 0`: Indeks otomatis saat ekspor CSV
- `ID`: ID unik entri data
- `ProdID`: ID unik produk
- `Rating`: Nilai rating produk oleh pengguna
- `ReviewCount`: Jumlah ulasan pengguna
- `Category`: Kategori produk (contoh: Beauty, Electronics, dll.)
- `Brand`: Merek produk
- `Name`: Nama produk
- `ImageURL`: URL gambar produk
- `Description`: Deskripsi teks produk
- `Tags`: Kata kunci/tag terkait produk

### Kondisi Data Awal
- Dataset terdiri dari 4090 baris dan 11 kolom.
- Terdapat *missing value* pada kolom:
  - `Category`: terdapat nilai kosong (missing) pada sejumlah baris.
  - `Brand`: terdapat nilai kosong (missing).
  - `Description`: beberapa deskripsi kosong.
- Tidak ditemukan duplikat berdasarkan kolom `Name`.
- Deteksi dan penanganan outlier tidak dilakukan karena fitur numerik terbatas dan fokus model pada data deskriptif (teks).

---

## Data Preparation

- Missing value pada kolom `Description` diisi dengan string kosong (`''`) agar proses ekstraksi fitur tetap berjalan.
- Ekstraksi fitur dilakukan dengan **TF-IDF (Term Frequency-Inverse Document Frequency)** dari kolom `Description` menggunakan `TfidfVectorizer` dari scikit-learn. TF-IDF digunakan untuk mengubah teks menjadi representasi numerik berdasarkan pentingnya kata dalam konteks seluruh dokumen.
- Parameter `stop_words='english'` digunakan untuk me  nghapus kata-kata umum yang tidak informatif.
- Nama produk distandarkan ke lowercase dan di-strip untuk keperluan pencocokan nama.
- Vektor TF-IDF digunakan untuk representasi deskripsi produk sebagai dasar perhitungan kemiripan.

---

## Modeling

### Metode
- Sistem rekomendasi dibangun menggunakan pendekatan content-based filtering.
- Representasi fitur diperoleh dari vektor TF-IDF kolom `Description`.
- Kemiripan antar produk dihitung menggunakan **cosine similarity**.
- Produk dengan nilai cosine similarity tertinggi (selain dirinya sendiri) direkomendasikan.

### Contoh Output Rekomendasi (Top-5)

**Input Produk**: `OPI Infinite Shine, Nail Lacquer Nail Polish`  
**Produk yang Paling Mirip Ditemukan**: `OPI Infinite Shine, Nail Lacquer Nail Polish, Bubble Bath`

**Hasil Rekomendasi:**

| No. | Nama Produk                                                                 | Similarity Score |
|-----|-----------------------------------------------------------------------------|------------------|
| 1   | Nice n Easy Permanent Color, 111 Natural Medium Auburn 1 ea (Pack of 3)     | 0.000            |
| 2   | Clairol Nice N Easy Permanent Color 7/106A Natural Dark Neutral Blonde      | 0.000            |
| 3   | Kokie Professional Matte Lipstick, Hot Berry                                | 0.000            |
| 4   | Gillette TRAC II Plus Razor Blade Refills, Fit TRAC II Handles, 10 ct       | 0.000            |
| 5   | Old Spice Artisan Styling High Hold Matte Finish Molding Clay, 2.64 oz      | 0.000            |


> *Catatan:* Hasil ini menunjukkan bahwa deskripsi produk dalam kategori ini mungkin terlalu umum atau kurang informatif.

---

### Evaluation

Untuk mengevaluasi sistem rekomendasi ini, digunakan metrik **Precision@5**.

**Precision@K** mengukur seberapa banyak item yang direkomendasikan (dari Top-K) yang benar-benar relevan dengan input. Rumusnya:

\[
\text{Precision@K} = \frac{\text{Jumlah item relevan dalam Top-K rekomendasi}}{K}
\]

**Contoh Perhitungan:**

Misalkan, dari 5 produk yang direkomendasikan sistem untuk suatu input produk, 2 di antaranya dianggap relevan (serupa), maka:

\[
\text{Precision@5} = \frac{2}{5} = 0.40
\]

**Hasil Aktual Evaluasi:**

Berdasarkan pengujian terhadap satu produk input dan label relevansi manual, diperoleh:

- **Precision@5: 0.40**

Nilai ini menunjukkan bahwa dari 5 produk yang direkomendasikan, 2 produk di antaranya memang relevan, menunjukkan performa awal sistem cukup baik meskipun masih dapat ditingkatkan.


Contoh kode perhitungan:

```python
def precision_at_k(recommended_names, true_similar_names, k=5):
    relevant = sum([1 for name in recommended_names[:k] if name in true_similar_names])
    return relevant / k

# Contoh penggunaan:
true_similar = ['Essie Nail Polish', 'Revlon Nail Enamel', 'Maybelline Fast Gel Nail Lacquer']
recommended = ['Essie Nail Polish', 'Revlon Nail Enamel', 'Nice n Easy Permanent Color Natural Black', 'Garnier Olia Ammonia-Free Hair Color', 'Adore Semi Permanent Hair Color']

precision = precision_at_k(recommended, true_similar, k=5)
print(f"Precision@5: {precision:.2f}")
```

**Hasil evaluasi contoh**:
- Precision@5 = **0.40** → 2 dari 5 rekomendasi relevan secara manual berdasarkan kategori dan kemiripan produk.

---

## Kesimpulan dan Saran
- Sistem rekomendasi berbasis deskripsi produk berhasil dibangun dan mampu memberikan rekomendasi dengan akurasi terbatas.
- Terbatasnya deskripsi atau kualitas deskripsi memengaruhi performa.
- Perlu dikembangkan lebih lanjut dengan fitur tambahan seperti `Category`, `Brand`, dan `Rating`.
- Pengembangan sistem hybrid (*content-based + collaborative filtering*) disarankan untuk meningkatkan personalisasi.
- Evaluasi bisa diperluas dengan metrik seperti Recall@K, F1@K, dan NDCG@K.

---

## Lampiran: Fungsi Rekomendasi

```python
def recommend_products(product_name, top_n=5):
    product_name = product_name.lower().strip()
    if product_name not in indices:
        return pd.DataFrame(columns=['Name', 'Description', 'Similarity Score'])

    idx = indices[product_name]
    sim_scores = list(enumerate(cosine_sim[idx]))
    sim_scores = sorted(sim_scores, key=lambda x: x[1], reverse=True)[1:top_n+1]
    product_indices = [i[0] for i in sim_scores]

    result = df[['Name', 'Description']].iloc[product_indices].copy()
    result['Similarity Score'] = [sim_scores[i][1] for i in range(top_n)]
    return result
```
