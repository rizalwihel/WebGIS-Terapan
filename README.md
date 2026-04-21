
### **README.md**

markdown
# 🌊 Backend API Documentation

Selamat datang di dokumentasi API **Bekasi FloodGuard AI-DSS**. Repository ini berisi dokumentasi *endpoint* yang dikembangkan menggunakan **n8n** sebagai *automation engine* dan **PostgreSQL/PostGIS** sebagai database spasial.

API ini dirancang untuk mendukung sistem monitoring banjir di Kabupaten Bekasi dengan fitur *Early Warning System* (EWS) dan integrasi Pakar AI.


## 🚀 Informasi Dasar
- **Base URL:** `http://localhost:5678/webhook`
- **Format Data:** JSON
- **CORS:** Diaktifkan (Access-Control-Allow-Origin: *)
- **Author:** Muhamad Rizal Wihel (Backend Developer)
- **Target Integrasi:** Janssen (Frontend/UI Developer)


### 🛠️ Daftar Endpoint API

### 1. Daftar Semua Kecamatan
Digunakan untuk mengisi menu *dropdown* atau fitur *search* pada dashboard.
- **Endpoint:** `/api/kecamatan`
- **Metode:** `GET`
- **Response Contoh:**
  ```json
  [
    { "id": 1, "nama": "Babelan" },
    { "id": 2, "nama": "Cikarang Pusat" }
  ]


### 2. Data Spasial Kerawanan (GeoJSON)
Endpoint utama untuk merender poligon kecamatan di peta Leaflet.js.
- **Endpoint:** `/peta-banjir`
- **Metode:** `GET`
- **Atribut Properti:**
  - `kecamatan`: Nama wilayah.
  - `curah_hujan`: Angka curah hujan terbaru (mm).
  - `status`: Klasifikasi (Sangat Rawan, Waspada, Aman).
- **Response Contoh:**
  ```json
  {
    "type": "FeatureCollection",
    "features": [
      {
        "type": "Feature",
        "geometry": { "type": "Polygon", "coordinates": [...] },
        "properties": {
          "kecamatan": "Babelan",
          "curah_hujan": 45.5,
          "status": "Waspada"
        }
      }
    ]
  }
  ```

### 3. Ringkasan Statistik (Dashboard Header)
Memberikan data agregat untuk ditampilkan di bagian atas website.
- **Endpoint:** `/stats-ringkasan`
- **Metode:** `GET`
- **Response Contoh:**
  ```json
  [
    {
      "total_kecamatan": 23,
      "jumlah_rawan": 5,
      "jumlah_waspada": 3,
      "rata_rata_hujan": 12.45
    }
  ]
  ```

### 4. Riwayat & Prediksi Curah Hujan (Tren 24 Jam)
Memberikan data *time-series* (12 jam terakhir & 12 jam prediksi masa depan).
- **Endpoint:** `/riwayat-kecamatan`
- **Metode:** `GET`
- **Parameter:** `?kecamatan=NamaKecamatan`
- **Catatan Penting:** Data dibungkus dalam objek `data` di dalam array pertama.
- **Response Contoh:**
  ```json
  [
    {
      "data": [
        { "curah_hujan": 0.5, "waktu": "2026-04-20T10:00:00" },
        { "curah_hujan": 1.2, "waktu": "2026-04-20T11:00:00" }
      ]
    }
  ]
  ```

### 5. Rekomendasi Mitigasi AI (Groq/Llama 3.1)
Menghasilkan saran pakar mitigasi secara dinamis berdasarkan data cuaca wilayah.
- **Endpoint:** `/api-mitigasi`
- **Metode:** `GET`
- **Parameter:** `?nama=NamaKecamatan`
- **Response Contoh:**
  ```json
  {
    "rekomendasi_ai": "Berdasarkan curah hujan 45mm di Babelan, disarankan untuk memantau pintu air..."
  }
  ```

### 6. Identifikasi Lokasi (Reverse Geocoding)
Menentukan nama kecamatan berdasarkan koordinat klik pada peta atau GPS.
- **Endpoint:** `/search-lokasi`
- **Metode:** `GET`
- **Parameter:** `?lat=xx&lng=yy`
- **Response Contoh:**
  ```json
  [
    { "kecamatan": "Babelan" }
  ]
  ```

---

## 🔄 Alur Kerja Data (Backend Logic)

Sistem ini menjalankan automasi data sebagai berikut:
1. **Data Ingestion:** Menarik data dari Open-Meteo API setiap kali *trigger* manual diaktifkan.
2. **Spatial Processing:** Menggunakan fungsi `ST_Contains` dari PostGIS untuk memetakan koordinat ke poligon kecamatan.
3. **AI Logic:** Menggunakan Groq AI (Llama 3.1) untuk menganalisis risiko banjir secara *real-time*.
4. **Forecasting:** Menyediakan data 12 jam ke depan untuk fungsi peringatan dini.

---

## ⚡ Teknologi yang Digunakan
- **Automation:** n8n
- **Database:** PostgreSQL + PostGIS extension
- **LLM:** Groq AI (Llama 3.1 8B Instant)
- **GIS Tools:** QGIS (Data Preparation)

---
*Dokumentasi ini dibuat untuk mempermudah kolaborasi tim Backend dan Frontend dalam proyek Bekasi FloodGuard.*
```

---
