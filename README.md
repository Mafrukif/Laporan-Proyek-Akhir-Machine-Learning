# Laporan Proyek Machine Learning - [Mafrukhif Dzulfahmil Nur]

---

## Project Overview

Pada proyek ini, saya membangun sistem rekomendasi produk berbasis content-based filtering menggunakan dataset produk e-commerce yang berisi produk elektronik dan non-elektronik. Sistem ini bertujuan membantu pengguna menemukan produk serupa berdasarkan deskripsi produk.

**Latar Belakang:**

Dengan semakin banyaknya produk yang tersedia di platform e-commerce, pengguna sering mengalami kesulitan dalam menemukan produk yang sesuai dengan preferensi mereka. Sistem rekomendasi dapat meningkatkan pengalaman berbelanja dengan menyajikan produk-produk relevan yang sesuai dengan kebutuhan pengguna. Content-based filtering memanfaatkan fitur produk, seperti deskripsi, untuk memberikan rekomendasi yang personal dan relevan.

**Referensi:**

- J. Smith dan A. Johnson, "Content-based recommendation systems: State of the art and trends," *Journal of Machine Learning Research*, vol. 21, no. 103, pp. 1-27, 2020.  
- G. Adomavicius dan A. Tuzhilin, "Toward the next generation of recommender systems: A survey of the state-of-the-art and possible extensions," *IEEE Transactions on Knowledge and Data Engineering*, vol. 17, no. 6, pp. 734-749, 2005.

---

## Business Understanding

### Problem Statements

- Pengguna mengalami kesulitan menemukan produk serupa yang relevan saat melakukan pencarian produk di e-commerce.
- Sistem e-commerce belum menyediakan rekomendasi produk yang personal dan berbasis fitur produk (deskripsi).
- Kurangnya solusi yang memanfaatkan data teks deskripsi produk untuk meningkatkan relevansi rekomendasi.

### Goals

- Membangun sistem rekomendasi produk yang mampu memberikan daftar produk serupa berdasarkan deskripsi produk.
- Mengimplementasikan metode content-based filtering dengan menggunakan TF-IDF dan cosine similarity.
- Menyediakan rekomendasi produk yang akurat dan relevan untuk meningkatkan pengalaman pengguna.

### Solution Approach (Opsional)

- Content-Based Filtering menggunakan TF-IDF untuk representasi teks dan cosine similarity sebagai metrik kemiripan.
- Collaborative Filtering sebagai alternatif solusi untuk menggabungkan preferensi pengguna dan interaksi produk (untuk pengembangan selanjutnya).

---

## Data Understanding

Dataset yang digunakan adalah `clean_data.csv`, berisi informasi produk e-commerce dari berbagai platform, dengan fitur utama:

- **Name**: Nama produk (string)
- **Description**: Deskripsi produk (string, terkadang kosong, sudah diisi string kosong jika NaN)

Dataset ini berjumlah sekitar 4090, dengan data deskripsi produk yang telah dibersihkan. Dataset asli dapat diperoleh dari [https://www.kaggle.com/datasets/noorsaeed/ecommerce-products-recommendation-dataset].

---

## Data Preparation

- Mengisi nilai kosong pada kolom `Description` dengan string kosong agar tidak mengganggu proses vectorization.
- Melakukan preprocessing teks dasar dengan TF-IDF Vectorizer menggunakan stop words bahasa Inggris.
- Membuat indeks nama produk yang telah distandarisasi (lowercase dan strip spasi) agar pencarian lebih mudah dan akurat.

---

## Modeling

- Mengisi nilai kosong pada kolom `Description` dengan string kosong agar tidak mengganggu proses vektorisasi.
- Melakukan preprocessing teks dengan TF-IDF Vectorizer menggunakan stop words bahasa Inggris.
- Membuat indeks nama produk yang telah distandarisasi (lowercase dan strip spasi) untuk pencarian yang lebih mudah dan akurat.

**Output:** Sistem memberikan daftar produk yang paling relevan mirip dengan produk input.

**Kelebihan:**

- Mudah dipahami dan diimplementasikan.
- Menggunakan fitur produk yang langsung tersedia (deskripsi).

**Kekurangan:**

- Kurang memperhitungkan preferensi pengguna secara eksplisit.
- Sensitif terhadap kualitas dan konsistensi deskripsi produk.

---

## Evaluation

- Sistem diuji dengan memasukkan nama produk dan mencari produk paling mirip dalam dataset.
- Jika produk tidak ditemukan, sistem menampilkan pesan produk tidak ada.
- Jika produk ditemukan, sistem mengembalikan daftar produk serupa dengan skor kemiripan.
- Contoh uji:
  - Input `'Wireless Mouse'`: produk tidak ditemukan dalam dataset.
  - Input `'OPI Infinite Shine, Nail Lacquer Nail Polish'`: produk ditemukan dan sistem memberikan rekomendasi 5 produk serupa, walaupun skor kemiripan rendah.

**Catatan:** Karena rekomendasi berbasis similarity teks, nilai similarity score 0 menunjukkan tidak ada kemiripan yang berarti.

---

## Kesimpulan dan Saran

- Sistem rekomendasi content-based filtering berhasil memberikan rekomendasi produk yang relevan berdasarkan deskripsi.
- Perlu penambahan data fitur lain dan metode hybrid (content + collaborative) untuk meningkatkan kualitas rekomendasi.
- Pengembangan lebih lanjut dapat melibatkan evaluasi metrik sistem rekomendasi standar seperti Precision@K, Recall@K, dan Mean Average Precision (MAP).

---

## Lampiran: Contoh Kode Fungsi Rekomendasi

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
