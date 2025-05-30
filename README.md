# Laporan Proyek Machine Learning - Nurul Itsnaini MC229D5X0883

## Project Overview
    
Seiring dengan berkembangnya teknologi, penggunaan media sosial di masyarakat mengalami peningkatan secara signifikan. Munculnya teknologi dan aplikasi baru pada kehidupan masyarakat saat ini mendukung munculnya interaksi sosial melalui media sosial [(Furqon et al., 2018)](https://drive.google.com/drive/u/0/my-drive). Media sosial juga berperan sebagai tempat berkumpulnya komunitas dengan minat dan bakat serupa untuk saling berinteraksi dan mengenal satu sama lain.
    
Media sosial telah menjadi bagian yang tidak bisa dihindari dari kehidupan masyarakat Indonesia, terutama di kalangan generasi muda. Platform seperti Tiktok, Instagram, dan media sosial lainnya tidak hanya digunakan untuk hiburan saja, akan tetapi juga sebagai sumber informasi dan pembelajaran mandiri. Namun, dengan volume konten yang sangat besar, pengguna sering kali mengalami kesulitan dalam menemukan postingan yang relevan dan menarik bagi mereka. Hal ini dapat menyebabkan pengguna hanya terpapar pada konten yang sesuai dengan preferensi mereka sebelumnya [(Safitri et al., 2024)](https://drive.google.com/drive/u/0/my-drive).

Berdasarkan permasalahan tersebut, perlu dikembangkan sistem rekomendasi yang mampu menyajikan konten baru yang belum pernah dilihat oleh pengguna, namun tetap relevan dengan minat mereka serta diharapkan mampu meningkatkan keberagaman konten yang dikonsumsi pengguna, memperluas wawasan mereka, dan menghindarkan dari keterkungkungan dalam satu jenis informasi saja  [(Safitri et al., 2024)](https://drive.google.com/drive/u/0/my-drive).

## Business Understanding
Derasnya arus informasi di media sosial membuat pengguna kerap kesulitan dalam menemukan konten yang benar-benar sesuai dengan minat dan kebutuhan mereka. Hal ini menyebabkan pengguna hanya terpapar pada konten yang sesuai dengan preferensi mereka sebelumnya.

### Problem Statements
Berdasarkan kondisi yang telah diuraikan, akan dikembangkan sistem rekomendasi postingan media sosial untuk menjawab permasalahan berikut ini:
  1. Berdasarkan data mengenai pengguna, bagaimana membuat sistem rekomendasi postingan media sosial yang dipersonalisasi dengan teknik _content-based filtering?_
  2. Berdasakan data viewer yang dimiliki, bagaimana media sosial dapat merekomendasikan postingan lain yang mungkin disukai dan belum pernah dilihat oleh pengguna?

### Goals
Berdasarkan permasalahan yang telah diuraikan, akan dibuat sebuah sistem rekomendasi dengan tujuan sebagai berikut:
  1. Menghasilkan sejumlah rekomendasi postingan yang dipersonalisasi untuk pengguna.
  2. Menghasilkan sejumlah rekomendasi postingan yang sesuai dengan preferensi pengguna dan belum pernah dilihat sebelumnya.

### Solution statements
Berdasarkan tujuan yang ada, digunakan dua pendekatan sistem rekomendasi untuk mencapai tujuan tersebut, yaitu pendekatan **_conten-based filtering_** untuk menghasilkan sejumlah rekomendasi postingan yang dipersonalisasi untuk pengguna dan pendekatan **_collaborative filtering_** untuk menghasilkan sejumlah rekomendasi postingan yang sesuai dengan preferensi pengguna dan belum pernah dilihat sebelumnya, serta menggunakan **_Root Mean Square Error (RMSE)_** sebagai matrik evaluasi.

## Data Understanding
- Data yang digunakan pada proyek ini adalah post recommendation dataset yang diunduh melalui tautan berikut:
(https://www.kaggle.com/datasets/vatsalparsaniya/post-pecommendation?select=post_data.csv). Dataset ini memiliki 3 file yakni:
  1. post_data.csv
  2. user_data.csv
  3. view_data.csv

- Variabel-variabel pada Post Recommendation Dataset adalah sebagai berikut:
    - post_data : merupakan dataset postingan yang berisi rincian posting seperti judul, kategori dan post_id.
    - user_data : merupakan dataset pengguna berisi rincian pengguna seperti nama, user_id, jenis kelamin, foto profil, asal kota, dan juga akademik.
    - view_data : Dataset tampilan berisi pemetaan pengguna yang melihat postingan mana saja, yang berisi user_id, post_id, dan timestamp.

### Univariate Exploratory Data Analysis
- post_data memiliki **6000 baris** dan **3 kolom data**, yakni:
    1. **title** : merupakan kolom data yang berisi judul postingan
    2. **category** : merupakan kolom data yang berisi 20 kategori postingan, yakni: 'graphic', 'Craft', 'politics', 'political', 'Mathematics', 'zoology', 'business', 
       'dance', 'banking', 'HR management', 'art', 'science', 'Music', 'operating system', 'Fashion Design', 'programming','painting', 'photography', 'drawing', dan 'GST', akan tetapi pada jenis kategori politics dan political memiliki makna yang sama maka seharusnya digabung menjadi satu kategori politics atau political.
    4. **post_id** : merupakan kolom data yang berisi id dari konten.

![alt text](https://github.com/ainiits1/Subbmission_Sistem_Rekomendasi/blob/main/Screenshot%202025-05-29%20070206.png?raw=true)

Dari output 0 diatas memberikan informasi bahwa tidak ada data duplikat dalam data.

![alt text](https://github.com/ainiits1/Subbmission_Sistem_Rekomendasi/blob/main/Screenshot%202025-05-29%20070750.png?raw=true)

Output 0 di masing-masing kolom memberikan informasi bahwa tidak ada missing value dalam data.

- user_data memiliki **500 baris** dan **7 kolom data**, yakni:
    1. **user_id**: merupakan id pengguna terdaftar
    2. **first_name**: merupakan nama depan pengguna
    3. **last_name**: merupakan nama belakang pengguna
    4. **gender**: merupakan jenis kelamin pengguna
    5. **avatar**: merupakan link foto profil pengguna
    6. **city**: merupakan asal kota pengguna
    7. **academics**: merupakan jenjang/status akademik pengguna. Ada 2 kategori, yakni undergraduate dan graduate.

![alt text](https://github.com/ainiits1/Subbmission_Sistem_Rekomendasi/blob/main/Screenshot%202025-05-29%20070817.png?raw=true)

Dari output 0 diatas memberikan informasi bahwa tidak ada data duplikat dalam data.

![alt text](https://github.com/ainiits1/Subbmission_Sistem_Rekomendasi/blob/main/Screenshot%202025-05-29%20070824.png?raw=true)

Output 0 di masing-masing kolom memberikan informasi bahwa tidak ada missing value dalam data.


- view_data memiliki **71.800** baris dan **3 kolom** data, yakni:
    1. **user_id**: merupakan id pengguna yang melihat postingan
    2. **post_id**: merupakan id postingan yang dilihat pengguna
    3. **time_stamp**: merupakan waktu pengguna dalam melihat postingan

![alt text](https://github.com/ainiits1/Subbmission_Sistem_Rekomendasi/blob/main/Screenshot%202025-05-29%20070835.png?raw=true)

Dari output 0 diatas memberikan informasi bahwa tidak ada data duplikat dalam data.

![alt text](https://github.com/ainiits1/Subbmission_Sistem_Rekomendasi/blob/main/Screenshot%202025-05-29%20070842.png?raw=true)

Output 0 di masing-masing kolom memberikan informasi bahwa tidak ada missing value dalam data

## Data Preprocessing
Ada dua tahapan dalam data preprocessing pada proyek ini, yakni:
  1. **Menggabungkan data view dan data post**, hal ini dilakukan untuk menambah informasi konten/postingan apa saja yang sudah dilihat oleh pengguna. Penggabungan ini menggunakan **fungsi merge**, dan hasil penggabungan data view dan data post disimpan dalam variabel 'post_view' dengan **71.800 baris** dan **5 kolom data**, yakni:
      - user_id
      - post_id
      - timestamp
      - title
      - category
  3. **Menggabungkan data post_view dengan data user**, hal ini dilakukan untuk menambah informasi profil user ke dalam aktivitas postingan. Penggabungan ini **fungsi merge**, dan hasil penggabungan data post_view dan data user disimpan dalam variabel 'all_post' dengan **71.800 baris** dan **5 kolom data**, yakni:
     - user_id
     - post_id
     - timestamp
     - title
     - category
     - first_name
     - last_name
     - gender
     - avatar
     - city
     - academics

## Data Preparation
Adapun tahapan data preparation yang dilakukan yaitu:
1. **Mengecek missing value pada data hasil penggabungan**, yakni data 'all_post'. Hal ini dilakukan untuk mengecek ada atau tidaknya missing value setelah proses penggabungan pada data.
2. **Mengecek data duplikat pada data hasil penggabungan**, yakni data 'all_post'. Hal ini dilakukan untuk mengecek ada atau tidaknya data duplikat setelah proses penggabungan pada data.
3. **Menyamakan Jenis Postingan**. Variabel 'all_post' diurutkan berdasarkan post_id dan disimpan dalam variabel 'data_posting'. Dalam data posting terdapat 20 kategori jenis postingan, yakni: 'drawing', 'Mathematics', 'graphic', 'HR management', 'photography',
 'Fashion Design', 'zoology', 'art', 'business', 'painting', 'politics',
 'programming', 'banking', 'operating system', 'GST', 'dance', 'Craft',
 'science', 'Music', dan 'political', akan tetapi pada 20 jenis kategori tersebut terdapat 2 kategori yakni politics dan political yang memiliki makna yang sama. Oleh karena itu, pada tahap ini dilakukan penyamaan jenis postingan menjadi 1 kategori politics, sehingga jenis kategori menjadi 19 kategori.
4. **Mengubah semua kategori menjadi title case (kapital di awal kata)**. Hal ini dilakukan karena penulisan kategori masih tidak konsisten, ada yang huruf awal kecil dan ada juga yang hurul awal besar. Oleh karena itu, penulisan di awal huruf dari semua kategori disamakan yakni kapital di awal kata, dan kategori yang memiliki 2 kata terpisah diberi tanda '_' untuk memisahkan keduanya tanpa harus menggunakan spasi agar nantinya tidak terbaca menjadi 2 kategori.
5. **Mengurutkan data berdasarkan post_id dan menghapus data duplikat**. Variabel 'data_posting' diurutkan kembali berdasarkan post_id dan disimpan dalam variabel 'preparation'. Hal ini dilakukan untuk mengurutkan data dan menghapus data duplikat setelah tahap penyamaan jenis kategori postingan.
6. **Mengonversi data series menjadi bentuk list dan membuat dataframe baru untuk data tersebut**. Mengonversi data series post_id, title, dan categori menjadi bentuk list serta membuat dataframe untuk data tersebut dan disimpan dalam variabel 'post_new'. Hal ini dilakukan untuk mempersiapkan data agar bisa digunakan dalam sistem rekomendasi _content-based filtering_.
7. **TF-IDF Vectorizer untuk _Content-Based Filtering_**. Hal ini dilakukan pada 'data post_new' berdasarkan category_post. Hal ini dilakukan untuk mengubah kumpulan teks menjadi vektor angka yang bisa digunakan oleh model _content-based filtering_, dan selanjutnya mengubah vektor tf-idf dalam bentuk matriks dengan menggunakan fungsi todense.
8. **Encoding User dan Post_id untuk Collaborative Filtering**. Hal ini dilakukan pada 'data view' yang berisi user_id, post_id, dan time_stamp. Encoding user_id dan post_id untuk collaborative filtering digunakan untuk mengubah ID asli yang masih berupa string menjadi index numerik yang terurut dan efisien untuk model _collaborative filtering_.
9. **Membuat fitur 'interaction' dan membagi data training dan validasi untuk _collaborative filtering_**. Pembuatan fitur interaction =1 digunakan untuk membuat label interaksi positif antara user dan postingan karena pada data view tidak ada rating, dan selanjutnya membagi data training sebesar 80% dan data validasi sebesar 20%.

## Modeling and Result
Pada tahap ini, dilakukan pembuatan 2 sistem rekomendasi, yaitu menggunakan _content-based filtering_ dan _collaborative filtering_.
1. **_Content-based filtering_**
   - Pendekatan _content-based filtering_ menggunakan data  post_id, title_post, dan category untuk merekomendasikan judul postingan yang mirip dengan judul postingan yang disukai pengguna di masa lalu.
   - Ada 4 tahapan dalam pendekatan ini, yaitu:
     1. Mendefinisikan data post_new
     2. Membuat dataframe untuk melihat tf-idf matrix
     3. Cosine Similarity untuk menghitung derajat kesamaan antar postingan menggunakan fungsi ```cosine_similarity```
     4. Membuat dataframe dari variabel cosine_sim dengan baris dan kolom berupa judul postingan untuk melihat similiraty matrix pada setiap postingan
     5. Mendapatkan rekomendasi
     
        ![alt text](https://github.com/ainiits1/Subbmission_Sistem_Rekomendasi/blob/main/Screenshot%202025-05-29%20083426.png?raw=true)

        Berdasarkan output di atas, dapat diberikan rekomendasi 5 judul postingan dan semua postingan berkategori mathematics.
    - Kelebihan _content-based filtering_:
        - Penemuan konten yang lebih baik
        - Menawarkan personalisasi tingkat tinggi dalam rekomendasi
        - Membuat rekomendasi untuk pengguna dengan minat khusus
    - Kekurangan _content-based filtering_:
        - Masalah privasi
        - Kurangnya kemampuan untuk merekomendasikan item yang sangat berbeda dari yang telah disukai oleh pengguna
          
3. **_Collaborative filtering_**
    - Pendekatan _collaborative filtering_ menggunakan dataset view, yang berisi user_id, post_id dan time_stamp. Pendekatan ini dilakukan berdasarkan data interaksi pengguna pada postingan. Adapun tujuan pendekatan ini untuk merekomendasikan judul postingan yang mungkin disukai tetapi belum pernah dilihat oleh pengguna.
    - Ada 5 tahapan dalam pendekatan ini:
      1. Mendefinisikan data view
      2. Proses training, membuat ```class RecommenderNet(tf.keras.Model)```
         - Model RecommenderNet menghasilkan rekomendasi dengan merepresentasikan user dan postingan sebagai vektor embedding. Skor kesesuaian dihitung melalui dot product antara embedding, ditambah bias, lalu diproses dengan fungsi sigmoid untuk menghasilkan probabilitas interaksi. Rekomendasi diberikan dengan mengurutkan skor tertinggi untuk tiap user.
         - Parameter penting:
             - num_users: jumlah user unik
             - num_posts: jumlah postingan unik
             - embedding_size: ukuran vektor embedding
             - embeddings_regularizer: untuk mencegah overfitting
             - activation (sigmoid): mengubah skor menjadi probabilitas
      5. Visualisasi metrik
      6. Mendapatkan rekomendasi

         ![alt text](https://github.com/ainiits1/Subbmission_Sistem_Rekomendasi/blob/main/Screenshot%202025-05-30%20154446.png?raw=true)

         Berdasarkan output di atas, diperoleh daftar postingan dengan viewer tertinggi.

         ![alt text](https://github.com/ainiits1/Subbmission_Sistem_Rekomendasi/blob/main/Screenshot%202025-05-30%20154456.png?raw=true)

         Berdasarkan output di atas, diperoleh Top 10 rekomendasi postingan.
  - Kelebihan _collaborative filtering_:
      - Tidak membutuhkan informasi tentang item
      - Mampu menemukan pola tersembunyi
  - Kekurangan _collaborative filtering_:
      - Masalah "cold start" di mana sulit memberikan rekomendasi kepada pengguna baru atau item baru yang belum memiliki data peringkat yang cukup
      - Skalabilitas
      - Data sparsity
   
## Evaluation
1. Evaluasi untuk pendekatan _content-based filtering_
   - Adapun metrik evaluasi yang digunakan pada _content-based filtering_ adalah Precision@K dan Recall@K.
   - Formula Precision@K
  
      ![alt text](https://github.com/ainiits1/Subbmission_Sistem_Rekomendasi/blob/main/Screenshot%202025-05-30%20152113.png?raw=true)

   - Formula Recall@K
  
      ![alt text](https://github.com/ainiits1/Subbmission_Sistem_Rekomendasi/blob/main/Screenshot%202025-05-30%20152119.png?raw=true)
   
   - Output evaluasi
  
      ![alt text](https://github.com/ainiits1/Subbmission_Sistem_Rekomendasi/blob/main/Screenshot%202025-05-30%20151234.png?raw=true)

     Berdasarkan output di atas diperoleh nilai Precision@5 dan Recall@5 dari judul post 'How To Turn Your MATHEMATICS From Zero To Hero' masing masing adalah 100.00%.
     
2. Evaluasi untuk pendekatan _collaborative filtering_
Adapun metrik evaluasi yang digunakan pada proyek ini adalah _Root Mean Squared Error_ (RMSE).
- Formula RMSE

  ![alt text](https://github.com/ainiits1/Subbmission_Sistem_Rekomendasi/blob/main/Screenshot%202025-05-29%20105800.png?raw=true)
 
- Keterangan:

  ![alt text](https://github.com/ainiits1/Subbmission_Sistem_Rekomendasi/blob/main/Screenshot%202025-05-29%20105812.png?raw=true)
  
- Cara kerja:
  1. Menghitung selisih antara nilai aktual dan nilai prediksi
  2. Menguadratkan selisih tersebut
  3. Menghitung rata-rata kuadrat error
  4. Mengambil akar kuadrat dari rata-rata tersebut


Ouput proyek:

 ![alt text](https://github.com/ainiits1/Subbmission_Sistem_Rekomendasi/blob/main/Screenshot%202025-05-30%20153259.png?raw=true)

Berdasarkan output di atas, model hanya menggunakan 5 epoch dikarenakan pada epoch ke-5 sudah terlihat RMSE semakin kecil, yaitu 0,0245 dan error pada data validasi sebesar 0,0187.

### Visualisasi metrik RMSE
![alt text](https://github.com/ainiits1/Subbmission_Sistem_Rekomendasi/blob/main/Screenshot%202025-05-29%20105342.png?raw=true)

## Referensi
Furqon, A., Hermansyah, D., Sari, R., Sukma, A., Akbar, Y., & Aini Rakhmawati, N. (2018). Analisis Sosial Media Pemerintah Daerah Di Indonesia Berdasarkan Respons Warganet Analysis of Local Government Social Media in Indonesia Based on Netizen Response. Jurnal Sosioteknologi , 17(2), 177.

Safitri, D., Ramadhani, A., Tridewi, S., & Suciati, W. (2024). Joget Prabowo dan Filter Bubble : Tinjauan terhadap Respon Masyarakat Pasca Pemilu di YouTube CNN Indonesia. 6, 163–176.

