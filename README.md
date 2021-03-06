# Laporan Proyek Machine Learning - Adi Putra Sinaga

>## Project Overview

Domain proyek Machine Learning untuk sistem rekomendasi ini adalah mengenai rekomendasi restoran dengan judul "Recommend top restaurants based on preference".

Menurut [Wikipedia](https://id.wikipedia.org/wiki/Rumah_makan), `Restoran` atau rumah makan adalah istilah umum untuk menyebut usaha [Gastronomi](https://id.wikipedia.org/wiki/Gastronomi) yang menyajikan hidangan kepada masyarakat dan menyediakan tempat untuk menikmati hidangan tersebut serta menetapkan tarif tertentu untuk makanan dan pelayanannya. Meski pada umumnya rumah makan menyajikan makanan di tempat, tetapi ada juga beberapa yang menyediakan layanan take-out dining dan delivery service sebagai salah satu bentuk pelayanan kepada konsumennya. Rumah makan biasanya memiliki spesialisasi dalam jenis makanan yang dihidangkannya. Sebagai contoh yaitu rumah makan chinese food, rumah makan Padang, rumah makan cepat saji (fast food restaurant) dan sebagainya.

Tentunya dengan perkembangan teknologi yang semakin pesat, dapat membawa dampak yang baik terhadap perekonomian. Pada sebuah restoran, kita dapat menerapkan yang namanya sistem rekomendasi. Apa itu sistem rekomendasi? [Sistem Rekomendasi](https://en.wikipedia.org/wiki/Recommender_system)  adalah subkelas sistem penyaringan informasi yang berupaya memprediksi "peringkat" atau "preferensi" yang akan diberikan kepada pengguna pada suatu item/produk. Sistem ini dapat digunakan untuk memberikan rekomendasi secara personal tergantung pola interaksi masing-masing pengguna. Sangat bermanfaat bukan?

Proyek ini penting untuk diselesaikan agar pelanggan/user mendapatkan rekomendasi restoran dengan penilaian kumulatif terbaik. Dari pihak restoran, sistem pemberi rekomendasi ini sangat penting untuk meningkatkan kualitas pelayanan restoran dan jumlah pelanggan.


>## Business Understanding

### Problem Statements

Kita ingin mempergunakan data preferensi dari pengguna/user untuk meningkatkan kualitas layanan agar memperoleh penilaian yang baik dari pengguna/user. Namun, kita memiliki  masalah terkait:
- Bagaimana membuat model machine learning untuk merekomendasikan ID restoran berdasarkan rating/penilaian tertinggi User?
- Bagaimana cara membuat model Machine Learning agar merekomendasikan ID restoran lain yang mungkin akan disukai dan belum pernah dikunjungi oleh user?

### Goals

Berdasarkan masalah yang telah disebutkan pada bagian `Problem Statements`, berikut adalah tujuan yang ingin kita capai:
- Membuat model Machine learning yang dapat merekomendasikan ID restoran berdasarkan penilaian tertinggi pengguna/user dengan teknik *Popularity based Recomender Model*
- Membuat model Machine Learning untuk merekomendasikan ID restoran lain yang mungkin akan disukai dan belum pernah dikunjungi oleh user dengan teknik Collaborative Filtering: SDV.

### Solution statements

Untuk menyelesaikan masalah yang telah disebutkan pada bagian `Problem Statements`, kita akan menggunakan pendekatan sistem rekomendasi dengan teknik **Popularity Based Recommender Model** dan **Collaborative Filtering Model**. 
- `Popularity based Recommender Model`: Rekomendasi ini didasari apa yang sedang populer/trend. Ini akan berguna ketika kita tidak memiliki data historis sebagai referensi didalam merekomendasikan suatu produk terhadap pengguna.

- `Collaborative Filtering Model`: Sistem merekomendasikan sejumlah restoran berdasarkan rating yang telah diberikan sebelumnya. Dari data rating pengguna, kita akan mengidentifikasi restoran-restoran yang mirip dan belum pernah dikunjungi oleh pengguna untuk direkomendasikan. Teknik ini menggunakan model based collaborative filtering : SVD (Singular Value Decomposition)

>## Data Understanding
<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/kaggle-dataset-preview.png?raw=true"/>
</p>

Dataset ini digunakan untuk keperluan studi/pendidikan dimana diharapkan dapat menghasilkan daftar restoran teratas sesuai dengan preferensi konsumen dan menemukan fitur signifikan. 
Dataset diperoleh dari [Kaggle](https://www.kaggle.com/). ***Kaggle*** merupakan platform penyedia dataset untuk data science. Untuk proyek ini, dataset yang kita pakai yaitu:
[Recommend top restaurants based on preference](https://www.kaggle.com/uciml/restaurant-data-with-consumer-ratings)

Pada proyek ini, kita hanya akan menggunakan file `rating_final.csv`. Kita menggunakan dataset `rating_final.csv` untuk merekomendasikan restoran berdasarkan popularitas & berdasarkan Collaborative yaitu peringkat/rating yang diberikan oleh setiap pengguna.
Pada  `rating_final.csv` terdapat 5 fitur :
- `userID`  Nominal, merupakan ID Pengguna yang memberikan penilaian 
- `placeID` Nominal, merupakan ID Restoran yang akan dinilai 
- `rating`  Numeric 0 - 2, merupakan penilaian restoran menurut user 
- `food_rating`  Numeric 0 - 2, merupakan penilaian makanan restoran menurut user
- `service_rating`  Numeric 0 - 2, merupakan penilaian pelayanan restoran menurut user

Kita juga dapat melihat pada tahap eksplorasi data untuk memahami variabel pada dataset serta korelasi antar variabel seperti informasi dibawah ini:

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/df-info.png?raw=true"/>
</p>
<p align="center">
  Pada gambar diatas, kita dapat mengetahui bahwa file rating_final.csv memiliki 1161 entri
</p>
<hr>

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/df-describe.png?raw=true"/>
</p>

<p align="center">
  Pada gambar diatas, disimpulkan bahwa nilai maksimum dan minimum untuk seluruh rating adalah 2 dan 0
</p>
<hr>

Pemahaman terhadap variabel pada data dan korelasinya akan membantu kita dalam menentukan pendekatan atau algoritma yang cocok untuk data kita. Idealnya, kita melakukan eksplorasi data terhadap seluruh variabel. Untuk memahami dataset secara detail, penting bagi kita untuk melakukan Exploratory Data Analysis (EDA), seperti pada langkah berikut :
* Jumlah pengguna unik, restoran unik, no. rating, food_ratings, service_ratings
* Berapa kali pengguna menilai
* Berapa kali restoran dinilai
* Bagaimana distribusi peringkat untuk makanan, layanan

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/unique-info.png?raw=true"/>
</p>

Pada gambar diatas, terdapat: 
* 138 user unik, 
* 130 restoran unik,
* rating/penilaian diberikan untuk makanan dan layanan sebanyak 1161 kali
<hr>

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/most-rated-users.png?raw=true"/>
</p>

Pada gambar diatas, terlihat bahwa: 
* `U1061` memberikan rating sebanyak 18 kali, 
* `U1135` memberikan rating sebanyak 14 kali,
* hingga `U1039` memberikan rating sebanyak 3 kali
<hr>

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/id-resto.png?raw=true"/>
</p>

Pada gambar diatas, merupakan banyak ID restoran yang diberi rating/penilaian. Untuk ID restoran: 
* `135085` dinilai sebanyak 36 kali, 
* `132834` dinilai sebanyak 25 kali, 
* hingga `135011` dinilai sebanyak 3 kali
<hr>

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/dist-rating.png?raw=true"/>
</p>

Pada visualisasi data `rating` diatas terlihat bahwa: 
* `rating 0` : &plusmn;250 kali, 
* `rating 1` : &plusmn;400 kali, 
* `rating 2` : &plusmn;500 kali. 

Bila ditotalkan, hasilnya adalah 1161 data entri/masukan.

<hr>

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/food-rating.png?raw=true"/>
</p>

Pada visualisasi data `food_rating` diatas terlihat bahwa: 
* `rating 0` : &plusmn;250 kali, 
* `rating 1` : &plusmn;400 kali, 
* `rating 2` : &plusmn;500 kali. 

Bila ditotalkan, hasilnya adalah 1161 data entri/masukan.
<hr>

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/serve-rating.png?raw=true"/>
</p>

Pada visualisasi data `service rating` diatas terlihat bahwa: 
* `rating 0` : &plusmn;300 kali, 
* `rating 1` : &plusmn;400 kali, 
* `rating 2` : &plusmn;400 kali. 

Bila ditotalkan, hasilnya adalah 1161 data entri/masukan.
<hr>

**Setelah melakukan Exploratory Data Analysis (EDA), kita memperoleh hasil :**
- 130 restoran dinilai minimal 3 kali dalam skala 0 hingga 2
- 138 pengguna telah memberi peringkat minimal 3 kali

Untuk merekomendasikan restoran dengan preferensi teratas, kita dapat meminta setiap pengguna memberi peringkat terhadap semua restoran. Namun tentunya hal tersebut sedikit sulit dicapai. Solusinya adalah dengan memprediksi peringkat yang akan diberikan pengguna terhadap restoran.

>## Data Preparation

Sebelumnya kita akan memuat semua data rating yang diberikan pengguna pada sebuah variabel, `data_final`:
 <p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/sd1.png?raw=true"/>
</p>

Pada tahap `Data Preparation`, kita dapat melakukan:

- `Mengubah bentuk data menjadi bentuk matriks` : hal ini diperlukan untuk mempermudah model dalam teknik modeling Collaborative filtering : SVD (Singular Value Decomposition). Seperti pada gambar dibawah:
 <p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/sd2.png?raw=true"/>
</p>

- `Melakukan kalkulasi Densitas matriks` : Hal tersebut merupakan suatu cara untuk mengetahui banyak kemungkinan peringkat yang diberikan dan berapa tepatnya peringkat yang diberikan. 

  - Banyak rating yang diberikan:
     <p align="center">
      <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/matrik1.png?raw=true"/>
     </p>
  - Total banyaknya rating yang bisa diberikan:
     <p align="center">
      <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/matrik2.png?raw=true"/>
     </p>
  - Menghitung Densitas matriks:
     <p align="center">
      <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/matrik3.png?raw=true"/>
     </p>

- `Mengatasi missing values` : missing value ini adalah hilangnya beberapa data entri/masukan pada dataset. 

<p align="justify">
  Ada beberapa teknik untuk mengatasi missing value, antara lain, menghapus atau melakukan drop terhadap data yang hilang, menggantinya dengan mean atau median, serta memprediksi dan mengganti nilainya dengan teknik regresi. Setiap teknik tentu memiliki kelebihan dan kekurangan. Selain itu, penanganan missing value juga bersifat unik tergantung kasusnya. 
</p>

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/missing-value.png?raw=true"/>
</p>

Pada proyek ini, kita dapat melihat bahwa variabel data_final tidak memiliki nilai yang hilang (missing value). Dapat disimpulkan bahwa data telah bersih dan telah siap untuk dimasukkan ke dalam pemodelan.

- `Melakukan pembagian data (split data)` : Setelah data dapat dipastikan bersih atau tidak mengandung `missing values`, kita dapat melakukan proses `split data`. Jika diperhatikan, pada tahap `Evaluation` terdapat kode program seperti seperti gambar dibawah: 
<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/train-test-split.png?raw=true"/>
</p>

Proses tersebut bertujuan untuk membagi data untuk data latih(train) dan data uji(test). Sebenarnya hal tersebut merupakan bagian dari tahap `Preparation`. Saya menempatkan kode tersebut pada tahap `Evaluation` untuk memudahkan kita mengamati proses evaluasi yang dimulai dari tahap `split data`.

>## Modeling and Results

Dalam Machine Learning kita  sering mendengar tentang metode Random Forest yang digunakan untuk menyelesaikan permasalahan. Metode Random Forest merupakan salah satu metode dalam Decision Tree. Random Forest adalah kombinasi dari  masing-masing tree yang baik kemudian dikombinasikan  ke dalam satu model. Random Forest bergantung pada sebuah nilai vector random dengan distribusi yang sama pada semua pohon yang masing masing Decision Tree memiliki kedalaman yang maksimal. Oleh karena itu, prinsip dasar random forest mirip dengan Decision Tree. Masing-masing Decision Tree akan menghasilkan output yang bisa saja berbeda-beda. Random Forest ini akan melakukan voting untuk menentukan hasil mayoritas dari semua Decision Tree. Sederhananya, Random Forest akan memberikan output berupa mayoritas hasil dari semua Decision Tree ([dqlab](https://dqlab.id/macam-algoritma-klasifikasi-machine-learning-yang-penting-untuk-diketahui)).

Pada tahap modeling ini, kita menggunakan pendekatan Random Forest untuk Content Based Recommendation yaitu Popularity Based Recommender Model dan teknik Singular Value Decomposition untuk Model Collaborative filtering.

- `Popularity Based Recommender Model` : Rekomendasi ini didasari apa yang sedang populer/trend. Teknik ini merekomendasikan suatu produk berdasarkan rating tertinggi agar pelanggan lebih mudah dalam menilai suatu produk. Hasil dari teknik ini adalah daftar peringkat dari nilai rating restoran tertinggi, sehingga menandakan bahwa restoran tersebut sangat populer.

  * **Kelebihan**:  berguna ketika kita tidak memiliki data historis sebagai referensi didalam merekomendasikan suatu produk terhadap pengguna. 
  * **Kekurangan**: karena rekomendasi ini berdasarkan popularitas dan tidak dipersonalisasi, sehingga rekomendasi tetap sama untuk semua pengguna.

Beberapa hal yang akan kita dilakukan:

- Memperoleh jumlah pengguna yang telah memberi peringkat dari nilai rating restoran tertinggi
- Memberi peringkat berdasarkan skor
- Merekomendasikan tempat paling populer

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/num-user.png?raw=true"/>
</p>

Berdasarkan output diatas, terlihat bahwa fitur yang akan digunakan yaitu placeID dan userID. Fitur score merupakan count dari userID yang berarti banyaknya user memilih placeID atau ID restoran.
<hr>

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/score-value.png?raw=true"/>
</p>

Pada gambar diatas, sistem pemeringkatan dari yang teratas hingga seterusnya menurun
<hr>

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/most-popular.png?raw=true"/>
</p>

Dari output diatas dapat disimpulkan bahwa ID Restoran yang mendapat peringkat 1 yaitu ID restoran `135085` dengan nilai 36 dari para user. Sistem telah merekomendasikan restoran terpopuler berdasarkan preferensi user.
<hr>

- `Collaborative Filtering Model` : model akan merekomendasikan sejumlah restoran berdasarkan rating yang telah diberikan sebelumnya. Dari data rating pengguna, kita akan mengidentifikasi restoran-restoran yang mirip dan belum pernah dikunjungi oleh pengguna untuk direkomendasikan. Teknik ini menggunakan model Based Collaborative filtering : SVD (Singular Value Decomposition). 

Algoritma SVD digunakan untuk mendekomposisi matrik yang bertujuan agar meningkatkan scalability dari sistem.
Algoritma SVD dapat dikolaborasikan dengan algoritma item-based untuk menghasilkan prediksi dan algoritma SVD menghasilkan nilai prediksi dengan nilai error yang lebih kecil dibanding dengan algoritma plain item-based.

Beberapa hal yang akan kita lakukan:

- Mengubah data menjadi tabel pivot -> Format diperlukan untuk model colab
- Membuat kolom user_index untuk menghitung no.pengguna -> mengubah konvensi penamaan pengguna dengan menggunakan penghitung
- Menerapkan metode SVD pada matriks sparse besar -> untuk memprediksi peringkat untuk semua restoran yang tidak diberi peringkat oleh pengguna
- Memprediksi peringkat untuk semua restoran yang tidak dinilai oleh pengguna menggunakan SVD
- Membungkus semuanya menjadi sebuah fungsi

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/cf1.png?raw=true"/>
</p>

Kita mengubah data yang kita pakai menjadi bentuk pivot table, karena format ini dibutuhkan untuk model
<hr>

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/cf2.png?raw=true"/>
</p>

Selanjutnya adalah membuat kolom `user_index` untuk menghitung no. pengguna. Kita ubah konvensi penamaan pengguna dengan menggunakan penghitung
<hr>

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/cf3.png?raw=true"/>
</p>

Gambar diatas adalah tampilan data yang telah berbentuk pivot table
<hr>

Setelah itu kita menerapkan metode SVD pada matrikse sparse besar, ini digunakan untuk memprediksi peringkat untuk semua resto yang tidak diberi peringkat oleh pengguna. 
<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/cf4.png?raw=true"/>
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/cf5.png?raw=true"/>
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/cf6.png?raw=true"/>
</p>
Output diatas adalah hasil penerapan SVD dengan 3 parameter, yaitu matriks ortogonal, nilai tunggal dan transpose dari matriks ortogonal
<hr>

Dapat kita lihat bahwa untuk matriks sparse, dapat menggunakan fungsi sparse.linalg.svds() untuk melakukan dekomposisi. SVD berguna untuk: kompresi data, pengurangan kebisingan mirip dengan Analisis Komponen Utama dan pengindeksan Semantik Laten (LSI), digunakan dalam pengambilan dokumen dan kesamaan kata dalam penambangan teks.

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/cf7.png?raw=true"/>
</p>

Selanjutnya memprediksi peringkat untuk semua restoran yang tidak dinilai oleh pengguna menggunaakn SVD, hasil dari prediksi rating terlihat di output diatas.

Selanjutnya adalah membungkuskan semua menjadi fungsi.
Hal yang harus dilakukan adalah :
- Membuat fungsi untuk merekomendasikan tempat dengan peringkat prediksi tertinggi
- Menggunakan fungsi untuk merekomendasikan tempat berdasarkan ID pengguna, peringkat sebelumnya, peringkat yang diprediksi, dan jumlah tempat yang direkomendasikan

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/cf8.png?raw=true"/>
</p>

Seperti pada output diatas, terlihat bahwa ada 10 `ID restoran` yang direkomendasikan untuk pelanggan atau `user id`  50. Sistem akan merekomendasikan ID restoran pada user dengan prediksi yang telah diperhitungkan sehingga pelanggan telah mendapatkan 10 rekomendasi tempat restoran yang paling sesuai.

>## Evaluation
Pada tahap ini, kita menggunakan metrik evaluasi Akurasi, Presisi, dan RMSE (Root Mean Squared Error).

- `Akurasi` adalah salah satu metrik untuk mengevaluasi model klasifikasi. Secara informal, akurasi adalah sebagian kecil dari prediksi model kami yang benar. Secara formal, akurasi memiliki definisi sebagai berikut:

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/formula-accuracy.png?raw=true"/>
</p>
<p align="center">
  Untuk klasifikasi biner, akurasi juga dapat dihitung dalam hal positif dan negatif sebagai berikut:
</p>

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/formula-akurasi.png?raw=true"/>
</p>

<p align="center">
  Dimana TP = Positif Benar, TN = Negatif Benar, FP = Positif Palsu, dan FN = Negatif Palsu.
</p>

Pada proyek ini, pada model Popularity Based Recommender System, nilai akurasi menggunakan data dari variabel data_sort['score']. Berikut adalah nilai metrik evaluasi akurasi pada model popularity based recommender system :

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/eval2.png?raw=true"/>
</p>
<hr>

- `Presisi` adalah rasio prediksi benar positif dibandingkan dengan keseluruhan hasil yang diprediksi positif. 

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/formula-presisi.png?raw=true"/>
</p>

<p align="center">
  Berikut adalah nilai metrik evaluasi presisi pada model Popularity Based Recommender System:
</p>

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/pre-score.png?raw=true"/>
</p>
<hr>


- ` Root Mean Square Deviation (RMSD) atau Root Mean Square Error (RMSE) ` adalah aturan penilaian kuadrat yang juga mengukur besarnya rata-rata kesalahan. Sama seperti MAE, semakin rendahnya nilai root mean square error juga menandakan semakin baik model tersebut dalam melakukan prediksi. RMSE adalah aturan penilaian kuadrat yang juga mengukur besarnya rata-rata kesalahan. Sama seperti MAE, semakin rendahnya nilai root mean square error juga menandakan semakin baik model tersebut dalam melakukan prediksi.

Rumus RMSE :

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/formula-rmse.png?raw=true"/>
</p>

Nilai RMSE SVD model diatas merupakan hasil perhitungan dari: 
> RMSE = round((((rmse_data.Avg_actual_rating - rmse_data.Avg_predicted_ratings) ** 2).mean() ** 0.5),5)

<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/eval3.png?raw=true"/>
</p>

 Berikut adalah hasil visualisasi rate error rmse pada variabel rmse_data :
 
<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/eval4.png?raw=true"/>
</p>
 
<p align="center">
  <img src="https://github.com/adiputrasinaga-cmd/recommendation-system/blob/main/img/eval5.png?raw=true"/>
</p>
 
> ## Conclusion
Model rekomendasi berdasarkan `Popularity Based Recommender Model` tidak dipersonalisasi dan  didasarkan pada jumlah frekuensi, yang mungkin tidak sesuai untuk pengguna. Model merekomendasikan 5 tempat yang sama untuk semua pengguna tetapi model berbasis Collaborative Filtering dapat merekomendasikan seluruh daftar yang berbeda berdasarkan rating yang pengguna berikan.

Model rekomendasi berdasarkan `Collaborative Filtering`  adalah sistem rekomendasi yang dipersonalisasi, rekomendasi didasarkan pada perilaku/interaksi pengguna di masa lalu (historis) dan tidak bergantung pada informasi tambahan apa pun.

> ## Closing Statement
<p align="justify">
Sekian hasil dari laporan proyek machine learning, Recommendation System ini. Bilamana didalam penyampaian serta penjelasan yang kurang berkenaan, saya memohon maaf. Atas waktu dan perhatiannya, saya ucapkan Terima kasih telah membaca laporan ini. Semoga dapat memberi manfaat bagi kita semuanya.
</p>
