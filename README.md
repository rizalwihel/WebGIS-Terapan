# WebGIS-Terapan

```markdown
# Bekasi FloodGuard AI-DSS API Documentation 🌊🤖

Selamat datang di repositori Backend **Bekasi FloodGuard AI-DSS**. Proyek ini merupakan sistem pendukung keputusan (Decision Support System) berbasis WebGIS untuk mitigasi bencana banjir di Kabupaten Bekasi, mengintegrasikan data cuaca real-time, analisis spasial PostGIS, dan kecerdasan buatan (AI).

**Backend Engineer:** Muhamad Rizal Wihel  
**Status Proyek:** Development - Version 2.1 (Forecasting Enabled)

---

## 🚀 Arsitektur Sistem
API ini dibangun menggunakan **n8n** sebagai orkestrator workflow, **PostgreSQL (PostGIS)** sebagai database spasial, dan **Groq/Llama 3.1** sebagai engine analisis mitigasi bencana.

**Base URL:** `http://localhost:5678/webhook/` (Local Development)

---

## 📡 Daftar Endpoint API

### 1. Daftar Kecamatan
Memberikan katalog seluruh kecamatan yang tersedia untuk mengisi komponen pencarian atau dropdown.
- **Endpoint:** `GET /api/kecamatan`
- **Response Format:**
  ```json
  [
    { "id": 1, "nama": "Babelan" },
    { "id": 2, "nama": "Setu" }
  ]
  ```

### 2. Peta Kerawanan (GeoJSON)
Menyediakan data spasial poligon kecamatan beserta status kerawanan untuk dirender menggunakan Leaflet.js.
- **Endpoint:** `GET /peta-banjir`
- **Properties Spasial:**
  - `kecamatan`: Nama wilayah.
  - `curah_hujan`: Angka curah hujan (mm).
  - `status`: Klasifikasi (Sangat Rawan, Waspada, Aman).

### 3. Statistik Ringkasan (Top Bar)
Digunakan untuk mengisi bar informasi di bagian atas dashboard.
- **Endpoint:** `GET /stats-ringkasan`
- **Response Example:**
  ```json
  [
    {
      "total_kecamatan": 23,
      "jumlah_rawan": 5,
      "jumlah_waspada": 8,
      "rata_rata_hujan": 32.5
    }
  ]
  ```

### 4. Riwayat & Prediksi Curah Hujan
Menampilkan tren hujan 24 jam terakhir dan 12 jam ke depan (Forecasting).
- **Endpoint:** `GET /riwayat-kecamatan`
- **Query Params:** `?kecamatan=NamaKecamatan`
- **Response Structure:** `{"data": [...]}`

### 5. Analisis Mitigasi Pakar AI
Menghasilkan rekomendasi tindakan mitigasi spesifik wilayah berbasis AI (LLM).
- **Endpoint:** `GET /api-mitigasi`
- **Query Params:** `?nama=NamaKecamatan`
- **Response Example:**
  ```json
  {
    "rekomendasi_ai": "Berdasarkan curah hujan 65mm di Babelan, segera evakuasi ke..."
  }
  ```

### 6. Reverse Geocoding (Cek Lokasi GPS)
Mengidentifikasi wilayah kecamatan berdasarkan koordinat latitude dan longitude user.
- **Endpoint:** `GET /search-lokasi`
- **Query Params:** `?lat=-6.xxx&lng=107.xxx`

---

## 🛠️ Tech Stack
- **Database:** PostgreSQL 16 + PostGIS (Spatial Analysis)
- **Engine:** n8n Workflow Automation
- **AI Model:** Llama 3.1-8b via Groq Cloud API
- **Data Source:** Open-Meteo API (Forecast & Past Hours)

---

## 📝 Catatan Integrasi untuk Frontend (Janssen)
1. **CORS:** API sudah dilengkapi header `Access-Control-Allow-Origin: *`.
2. **Parsing Data:** Khusus untuk endpoint `/riwayat-kecamatan`, pastikan melakukan parsing pada indeks pertama array: `res.data[0].data`.
3. **Trigger Manual:** Pastikan melakukan sinkronisasi data manual di dashboard n8n jika data cuaca perlu diperbarui secara instan.

---
*Dikembangkan untuk tugas akhir Sistem Informasi Geografis Terapan (WebGis)*
```

---

### **Logbook Individu (Minggu ke-7: Project Documentation)**

Aktivitas pembuatan dokumentasi ini sangat penting sebagai bukti kerja tim yang baik:

| Minggu | Aktivitas Utama (Backend) | Output Spesifik | Tools/Teknologi | Bukti (SS) | Kendala | Solusi |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **7** | Finalisasi arsitektur API dan penyusunan dokumentasi teknis (README.md). | File dokumentasi API komprehensif untuk kolaborasi tim Frontend. | Markdown, GitHub, n8n. | [SS GitHub Repo] | Kesulitan dalam menyelaraskan format data JSON yang kompleks bagi tim Frontend. | Menyusun dokumentasi spesifikasi *endpoint* yang mencakup parameter *input* dan contoh *output* JSON secara detail. |

**Rizal**, file ini tinggal kamu *copy* ke Notepad, simpan dengan nama `README.md`, dan unggah ke repositori GitHub proyekmu. Janssen pasti akan sangat terbantu dengan ini. Ada bagian lain yang ingin kamu tambahkan?
