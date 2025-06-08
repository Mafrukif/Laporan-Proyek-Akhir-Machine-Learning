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

1. **Unnamed: 0** – Indeks baris otomatis (dihasilkan saat export CSV).
2. **ID** – ID unik untuk setiap entri data.
3. **ProdID** – ID unik produk.
4. **Rating** – Rating pengguna terhadap produk.
5. **ReviewCount** – Jumlah ulasan yang diberikan untuk produk.
6. **Category** – Kategori produk (misalnya: Beauty, Electronics, dll.).
7. **Brand** – Merek produk.
8. **Name** – Nama produk.
9. **ImageURL** – URL gambar produk.
10. **Description** – Deskripsi produk (string teks, beberapa awalnya kosong).
11. **Tags** – Tag/kata kunci yang terkait dengan produk.

Dataset ini berjumlah baris sekitar 4090 dan jumlah kolom sebanyak 11, dengan data deskripsi produk yang telah dibersihkan. Dataset asli dapat diperoleh dari [https://www.kaggle.com/datasets/noorsaeed/ecommerce-products-recommendation-dataset].

---

## Data Preparation

- Nilai kosong pada kolom `Description` diisi dengan string kosong (`''`) agar tidak mengganggu proses ekstraksi fitur.
- Preprocessing teks menggunakan **TF-IDF Vectorizer**:
  - Menggunakan `stop_words='english'`
  - Tokenisasi otomatis oleh `TfidfVectorizer` dari Scikit-learn
- Semua nama produk distandarkan menjadi lowercase dan strip spasi untuk keperluan pencarian indeks nama.
- TF-IDF menghasilkan vektor representasi teks yang digunakan untuk menghitung kemiripan antar deskripsi.

---

## Modeling

Sistem rekomendasi dibangun menggunakan metode **Content-Based Filtering** dengan pendekatan sebagai berikut:

- Menggunakan **TF-IDF vector** dari kolom `Description` untuk representasi fitur.
- Mengukur kemiripan antar produk menggunakan **cosine similarity** dari hasil vektorisasi TF-IDF.
- Rekomendasi dihitung berdasarkan produk yang memiliki skor kemiripan tertinggi terhadap produk input (kecuali dirinya sendiri).

### Contoh Output Rekomendasi (Top-5):

Input produk: `'OPI Infinite Shine, Nail Lacquer Nail Polish'`  
Output rekomendasi:

| Name                                         | Similarity Score |
|----------------------------------------------|------------------|
| OPI Nail Lacquer, Bubble Bath, 0.5 fl oz     | 0.109            |
| Sally Hansen Miracle Gel Nail Polish         | 0.065            |
| Essie Nail Polish                            | 0.059            |
| Revlon Nail Enamel                           | 0.057            |
| Maybelline Fast Gel Nail Lacquer             | 0.048            |


**Kelebihan:**

- Mudah dipahami dan diimplementasikan.
- Menggunakan fitur produk yang langsung tersedia (deskripsi).

**Kekurangan:**

- Kurang memperhitungkan preferensi pengguna secara eksplisit.
- Sensitif terhadap kualitas dan konsistensi deskripsi produk.

---

## Evaluation

Evaluasi dilakukan dengan dua pendekatan:

1. **Uji Manual**: 
   - Menginput nama produk dan menilai kualitas produk yang direkomendasikan.
   - Jika produk tidak ditemukan, sistem menampilkan pesan.
   - Jika ditemukan, menampilkan top-N produk serupa.

2. **Metrik Evaluasi Formal**:  
   Untuk mengukur performa sistem rekomendasi, digunakan **Precision@5**, yaitu rasio produk relevan dari 5 rekomendasi teratas.  

   Contoh hasil evaluasi:

   - Produk input: `'Essie Nail Polish'`
   - Dari 5 rekomendasi, 3 produk merupakan produk sejenis dalam kategori Nail Polish.
   - Precision@5 = 3 / 5 = **0.60**

   Metrik ini relevan untuk mengevaluasi seberapa banyak rekomendasi yang benar-benar relevan bagi pengguna berdasarkan konteks produk yang mirip secara semantik.

---

## Kesimpulan dan Saran

- Sistem rekomendasi content-based filtering berhasil mengidentifikasi produk yang mirip berdasarkan deskripsi.
- Hasil rekomendasi cukup baik meskipun terbatas pada fitur teks saja.
- Perlu ditambahkan fitur lain (kategori, brand, rating) agar sistem lebih akurat.
- Dapat dikembangkan lebih lanjut menjadi **hybrid recommender system** (menggabungkan content-based dan collaborative).
- Evaluasi sistem dapat lebih ditingkatkan dengan menambahkan metrik seperti Recall@K, F1@K, dan NDCG@K.

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
