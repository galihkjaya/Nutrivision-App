# NutriVision 🥗 — Sistem Analisis Gizi & Deteksi Makanan Visual Berbasis AI

NutriVision adalah platform digital canggih berbasis kecerdasan buatan (*Artificial Intelligence*) yang menggabungkan kekuatan komputasi awan dan model pembelajaran mendalam (*deep learning*) untuk mengenali hidangan kuliner, memperkirakan kandungan makronutrisi, serta mencatat asupan kalori harian secara instan melalui jepretan kamera. Proyek ini dikembangkan sebagai capstone project orisinal yang menggabungkan arsitektur web modern dengan teknik computer vision termutakhir.

---

## 📸 Deskripsi Proyek & Latar Belakang

Dalam era modern dengan mobilitas tinggi, menjaga keseimbangan pola makan sehat menjadi tantangan besar bagi masyarakat urban. Masalah utama yang sering dihadapi adalah kesulitan dalam memantau asupan gizi harian secara konsisten dan akurat. Metode pencatatan manual tradisional sering kali tidak praktis, membosankan, dan rentan terhadap kesalahan estimasi.

**NutriVision** hadir sebagai solusi teknologi mutakhir untuk menjembatani jurang pemisah tersebut. Dengan memanfaatkan kekuatan **Visual Scan AI**, pengguna hanya perlu mengambil foto piring makanan mereka untuk memperoleh analisis nutrisi mendalam secara seketika. Sistem memecah hidangan menjadi komponen makronutrisi esensial (Karbohidrat, Protein, Lemak, dan Kalori) serta menyimpannya ke dalam sistem database log terenkripsi demi menyajikan visualisasi progres kebugaran yang nyata dari waktu ke waktu.

---

## 🛠️ Teknologi Utama

NutriVision dibangun di atas pilar teknologi mutakhir dengan pendekatan type-safe di sisi klien dan performa tangguh di sisi peladen:

1. **Laravel 13 (Backend Engine):** Berperan sebagai RESTful API dan database orchestrator utama. Memanfaatkan fitur-fitur mutakhir Laravel 13, seperti optimalisasi rute, middleware autentikasi tingkat tinggi melalui Laravel Fortify (dengan dukungan 2FA & Google OAuth), dan pemrosesan antrean data secara asinkron.
2. **React TS (Frontend Interface):** Dibangun menggunakan TypeScript yang *strict* untuk memastikan keandalan komponen UI tanpa adanya kesalahan tipe data selama fase kompilasi produksi.
3. **Inertia.js (Seamless Bridge):** Menghubungkan secara langsung *Inertia Controller* di Laravel dengan halaman komponen React di frontend tanpa memerlukan setup API RESTful routing yang kompleks di kedua sisi, menciptakan kenyamanan SPA (*Single Page Application*) dengan kesederhanaan monolitik.
4. **Tailwind CSS v4 (Styling Engine):** Mengimplementasikan sistem desain responsif premium dengan estetika modern, dukungan transisi mikro yang halus, serta kemampuan Light/Dark mode adaptif yang selaras dengan preferensi sistem perangkat pengguna.
5. **TensorFlow & Python FastAPI (AI Inference Engine):** Menjalankan pemrosesan model kecerdasan buatan secara asinkron untuk mendeteksi *brand* masakan dan jenis kuliner secara *real-time* via Hugging Face Space.

---

## 📐 Arsitektur Sistem & Alur Deteksi Objek Gizi

Sistem ini memisahkan peran penyajian data UI dan proses komputasi berat model AI secara terdistribusi. Berikut adalah bagan alur proses deteksi ketika pengguna memindai hidangan mereka:

```mermaid
sequenceDiagram
    autonumber
    actor User as Pengguna (Browser)
    participant FE as Frontend React TS
    participant BE as Backend Laravel 13
    participant DB as Database (PostgreSQL/SQLite)
    participant AI as AI API (FastAPI TensorFlow)

    User->>FE: Jepret / Unggah Foto Makanan
    FE->>BE: POST /api/scan (Multipart Form Data)
    BE->>BE: Validasi Format & Enkripsi Gambar
    BE->>AI: POST /predict (Forward Image Binary)
    Note over AI: Deteksi Brand Masakan (EfficientNet)<br/>Deteksi Komponen Menu (FCOS + NMS)
    AI-->>BE: Kirim Prediksi JSON (Label Key & Skor Akurasi)
    BE->>DB: Query Tabel 'nutrition' berdasarkan Key yang Terdeteksi
    DB-->>BE: Data Nutrisi Referensi Per Porsi
    BE->>BE: Hitung Akumulasi Kalori & Makronutrisi Total (Serving Qty)
    BE->>DB: Simpan Log ke Tabel 'result' (Session User ID)
    DB-->>BE: Konfirmasi Penyimpanan
    BE-->>FE: HTTP 201 Created (Hasil Deteksi & Statistik Terkini)
    FE->>User: Animasikan Progress Bar Gizi & Tampilkan Detail Nutrisi
```

### Penjelasan Teknis Model Kecerdasan Buatan:
* **Model 1 (Brand Classifier):** Mengidentifikasi sumber penyedia atau gaya masakan (misalnya menu warung lokal, restoran cepat saji tertentu) memanfaatkan model klasifikasi berbasis **EfficientNet** untuk mengenali fitur visual unik dengan jumlah parameter efisien.
* **Model 2 (Menu Object Detector):** Mendeteksi koordinat lokasi bahan makanan di dalam piring masakan secara simultan menggunakan pendekatan **FCOS (Fully Convolutional One-Stage Object Detection)**. Model membagi gambar ke dalam bentuk sel fitur bertingkat (*feature maps*), memprediksi probabilitas kelas bahan, peta *centerness* objek, serta jarak tepi kotak pembatas (*bounding boxes*).
* **Non-Maximum Suppression (NMS):** Dilakukan untuk membuang kotak pembatas tumpang tindih yang redundan berdasarkan skor ambang batas *Intersection over Union* (IoU) tertentu, menyisakan objek hidangan terbaik untuk dianalisis gizinya.

---

## 🌟 Fitur Utama Sistem

* **Visual Scan AI Real-time:** Dukungan WebRTC terintegrasi yang memungkinkan akses kamera instan langsung dari peramban untuk mengambil foto masakan.
* **Multi-Food Detection Aggregation:** Sistem mampu mengidentifikasi beberapa jenis komponen makanan dalam satu piring sekaligus (misal mendeteksi ayam goreng beserta kentang goreng) dan secara otomatis mengakumulasikan seluruh nilai kalorinya dalam satu log makanan tunggal.
* **Konsultasi Gizi "Tanya AI":** Chatbot cerdas interaktif di sisi klien yang dirancang untuk memberikan edukasi seputar diet seimbang, nilai kalori lokal, dan rekomendasi aktivitas olahraga sehat pembakar kalori.
* **Sistem Keamanan Akun Berlapis:** Proteksi autentikasi tangguh dengan pilihan konfirmasi kata sandi, sistem Autentikasi Dua Faktor (2FA) bawaan, serta integrasi masuk instan dengan akun Google (Google OAuth).
* **Dashboard Progres Dinamis:** Grafik visualisasi harian, mingguan, hingga bulanan yang ditarik secara dinamis dari database untuk memantau asupan gizi secara presisi.

---

## 🚀 Langkah Instalasi & Konfigurasi Lingkungan

Ikuti panduan di bawah ini untuk memasang dan menjalankan proyek NutriVision di komputer lokal Anda:

### 📋 Prasyarat Sistem
* **PHP:** Versi `>= 8.3` (pastikan ekstensi `sqlite`, `pdo`, `mbstring`, `openssl`, dan `curl` aktif)
* **Node.js:** Versi `>= 20.x`
* **Composer:** Versi `>= 2.x`
* **Database:** SQLite (default) atau PostgreSQL / MySQL

### 🛠️ Langkah Pengaturan

1. **Unduh Repositori & Masuk ke Direktori Kerja:**
   ```bash
   git clone https://github.com/ridhwananang/nutrivition-071.git nutrivision
   cd nutrivision
   ```

2. **Pasang Dependensi Backend (Composer):**
   ```bash
   composer install
   ```

3. **Pasang Dependensi Frontend (NPM):**
   ```bash
   npm install
   ```

4. **Konfigurasi File Lingkungan (.env):**
   Salin file konfigurasi contoh dan buat kunci enkripsi aplikasi baru:
   ```bash
   cp .env.example .env
   php artisan key:generate
   ```

5. **Setup Database Lokal:**
   Aplikasi dikonfigurasi secara default untuk menggunakan basis data SQLite yang praktis. Buat file database kosong dan jalankan migrasi beserta pengisian data awal (*seeder*):
   ```bash
   # Di sistem Windows (PowerShell)
   New-Item -Path "database" -Name "database.sqlite" -ItemType "file" -Force
   
   # Jalankan migrasi dan seeder data referensi gizi
   php artisan migrate --seed
   ```

6. **Jalankan Aplikasi Dalam Mode Pengembangan:**
   Jalankan server lokal Laravel dan bundler Vite secara bersamaan menggunakan perintah terpadu:
   ```bash
   npm run dev
   ```
   Aplikasi kini dapat diakses melalui browser Anda di tautan: `http://127.0.0.1:8000`

---

## 🧪 Metode Pengujian (Automated Testing)

Untuk memastikan keandalan fungsionalitas backend dan integritas perhitungan kalori, NutriVision dilengkapi dengan suite pengujian otomatis komprehensif menggunakan **Pest PHP Framework**.

### Menjalankan Pengujian
Gunakan perintah artisan berikut untuk menjalankan pengujian di lokal:
```bash
php artisan test
```

### Cakupan Pengujian (*Test Coverage*):
Suite pengujian saat ini mencakup 44 kasus uji yang mencakup:
* **Dynamic Calculations Test:** Memverifikasi kalkulasi dinamis dashboard berdasarkan jumlah porsi makanan yang dikonsumsi harian pengguna.
* **Weekly & Monthly Historical Aggregates:** Menguji kebenaran algoritma visualisasi grafik mingguan dan bulanan agar terhindar dari bias data zona waktu lokal.
* **Multi-item Detection Storage:** Memastikan jika API mendeteksi lebih dari satu item makanan, relasi database Eloquent menyimpan setiap record gizi secara individu dan memformat nama gabungannya secara rapi.
* **Authentication & 2FA Flow:** Menjamin keamanan alur login, pendaftaran akun baru, konfirmasi kata sandi, serta integrasi Google OAuth.

---

## 🧠 Dokumentasi Model & Chatbot

Bagian ini menambahkan informasi baru terkait sumber model, cara model dipakai oleh aplikasi NutriVision, serta mekanisme chatbot yang saat ini digunakan.

### Google Drive Full Model Assets

Untuk aset model lengkap dan file mentahan pengembangan model, gunakan Google Drive berikut:

<https://drive.google.com/drive/folders/18IcDLcms48ljtpJn9aMkk88pGsJ2TkbT?usp=sharing>

Folder ini ditujukan untuk kebutuhan seperti:

* artifact training mentah
* `tfrecord`
* `saved_model`
* file pendukung eksperimen atau reproduksi pelatihan

### Hugging Face Ready-to-Use Models

Untuk model yang siap dipakai atau di-download langsung untuk inference, gunakan repository berikut:

<https://huggingface.co/galihkjaya/nutrivision-models>

Repository Hugging Face ini dipakai oleh implementasi inference untuk mengambil file seperti:

* `brand_map.json`
* `item_map.json`
* `brand_saved_model`
* `menu_saved_model`

### Mekanisme Penggunaan Model di Aplikasi Ini

Penggunaan model pada NutriVision saat ini berfokus pada alur aplikasi, bukan sebagai package terpisah.

1. Pengguna mengunggah foto makanan dari aplikasi web.
2. Frontend mengirim gambar ke endpoint `POST /api/scan`.
3. Laravel memvalidasi file lalu meneruskan gambar ke service inference pada endpoint `/predict`.
4. Service inference menjalankan klasifikasi brand dan deteksi item makanan.
5. Hasil model dikembalikan dalam format label seperti `mcd-frenchfries` atau `kfc-friedchicken`.
6. Backend mencocokkan label tersebut ke kolom `key` pada tabel `nutrition`.
7. Sistem menghitung total kalori dan makronutrien berdasarkan `serving_qty`.
8. Hasil scan disimpan ke tabel `result` dan dikirim kembali ke frontend.

Implementasi saat ini mengacu pada:

* [inference.py](/d:/Kuliah/Bootcamp/DBS%20CC26/Capstone%20Project/nutrivition-071/inference.py) untuk proses load model dan inference
* [app/Http/Controllers/Api/ScanController.php](/d:/Kuliah/Bootcamp/DBS%20CC26/Capstone%20Project/nutrivition-071/app/Http/Controllers/Api/ScanController.php) untuk integrasi backend Laravel dengan service inference

### Menjalankan Service Inference Secara Lokal

Jika ingin mencoba service inference yang sama secara lokal:

1. Siapkan environment Python terpisah.
2. Install dependensi yang diperlukan.
3. Jalankan:

```bash
python inference.py
```

Secara default service akan berjalan di port `7860`. Jika ingin dipakai oleh aplikasi lokal, endpoint inference pada `ScanController` perlu diarahkan ke service lokal tersebut.

### Folder `ml-model`

Folder [ml-model](/d:/Kuliah/Bootcamp/DBS%20CC26/Capstone%20Project/nutrivition-071/ml-model) disediakan untuk menyimpan aset pengembangan model yang aman dimasukkan ke repository aplikasi, seperti:

* notebook eksperimen atau training
* daftar dependensi Python di [ml-model/requirements.txt](/d:/Kuliah/Bootcamp/DBS%20CC26/Capstone%20Project/nutrivition-071/ml-model/requirements.txt)

Notebook yang saat ini disertakan:

* [\[CC26\]_Nutrivision_Model.ipynb](</d:/Kuliah/Bootcamp/DBS%20CC26/Capstone%20Project/nutrivition-071/ml-model/[CC26]_Nutrivision_Model.ipynb>)

Catatan:

* file model besar, dataset, `tfrecord`, dan artefak training lengkap tetap lebih aman diletakkan di Google Drive atau Hugging Face
* pendekatan ini menjaga repository aplikasi tetap lebih ringan untuk deployment Laravel Cloud

### Chatbot yang Digunakan

NutriVision menggunakan chatbot internal bernama **NutriBot AI**.

Cara kerjanya saat ini:

1. Frontend mengirim pesan pengguna ke endpoint `POST /api/chat`.
2. Laravel menyusun *system prompt* khusus NutriVision.
3. Backend memanggil Groq Chat Completions API.
4. Sistem mencoba fallback model berikut:
   * `llama-3.3-70b-versatile`
   * `llama-3.1-8b-instant`
   * `gemma2-9b-it`
5. Jika pengguna login, chatbot dapat memanfaatkan ringkasan riwayat scan terbaru untuk memberi saran yang lebih personal.

Implementasi chatbot saat ini dapat dilihat di:

* [resources/js/components/AiChatbot.tsx](/d:/Kuliah/Bootcamp/DBS%20CC26/Capstone%20Project/nutrivition-071/resources/js/components/AiChatbot.tsx)
* [app/Http/Controllers/ChatController.php](/d:/Kuliah/Bootcamp/DBS%20CC26/Capstone%20Project/nutrivition-071/app/Http/Controllers/ChatController.php)

Ruang lingkup chatbot saat ini berfokus pada:

* edukasi nutrisi dan kalori
* penjelasan fitur NutriVision
* saran umum terkait diet, makanan, dan aktivitas
* respons kontekstual berbasis riwayat scan pengguna

### Catatan Deployment

Penambahan folder `ml-model` di repository ini ditujukan untuk dokumentasi dan aset ringan seperti notebook serta daftar dependensi. Folder ini tidak dipakai langsung oleh runtime web Laravel kecuali di masa depan ditambahkan integrasi baru yang memang memanggil aset tersebut.
