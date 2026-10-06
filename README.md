# Multi-Automation Hub: AI Medical Triage & Server Monitoring 🚀

Repositori ini berisi kumpulan workflow otomatisasi enterprise-grade yang dibangun menggunakan **n8n orchestration engine**. Solusi di dalamnya dirancang untuk menyelesaikan masalah operasional riil di fasilitas kesehatan/klinik dan sistem pemantauan infrastruktur server backend.

---

## 📁 1. Proyek Utama: AI Medical Triage & Smart Booking System

Sistem bot pintar multi-channel (WhatsApp & Telegram) yang mengotomatiskan penyaringan awal keluhan medis (*triage*) pasien menggunakan AI (GPT-4o-mini), mengklasifikasikan tingkat urgensi, serta mengotomatiskan pencatatan jadwal konsultasi tanpa membebani staf admin.

### 🏥 Catatan Desain Klinis (Medical Design Notes)
Sebagai praktisi yang aktif bekerja di Rumah Sakit, saya merancang sistem ini dengan logika klinis yang aman agar tidak melanggar batasan etika medis digital:
*   **Strict Medical Guardrails (Anti-Malpraktik):** Prompt AI dikunci secara ketat dengan instruksi *Zero-Tolerance* terhadap pemberian diagnosis pasti dan peresepan obat. AI hanya diizinkan melakukan klasifikasi tingkat keparahan (*screening* awal).
*   **Logika Penanganan Non-Medis:** Jika ada pasien yang hanya menyapa ("Halo", "P", dll.) atau bertanya harga, AI tidak akan bingung mencari gejala, melainkan langsung menetapkan status `NON_MEDIS` dan mengarahkannya ke admin manusia.
*   **Fail-Safe Alarm System:** Sistem dilengkapi dengan `Error Trigger` global. Jika API WhatsApp/Telegram putus atau kuota OpenAI habis, sistem akan langsung mengirim alarm darurat terpisah ke Telegram internal admin agar tidak ada pasien kritis yang terabaikan.

### 🛠️ Alur Kerja & Komponen Teknis
*   **Ingestion & Normalization:** Menerima pesan dari Telegram Trigger dan WhatsApp Webhook (via WAHA API). Masuk ke **JavaScript Code Node** untuk menormalisasi struktur data yang berbeda menjadi format JSON seragam (`platform`, `chat_id`, `user_name`, `message_text`).
*   **AI Orchestration:** Meneruskan data bersih ke **LangChain Chained LLM** yang terintegrasi dengan **OpenAI GPT-4o-mini**.
*   **Output Parsing:** Node JavaScript memvalidasi output AI untuk memastikan formatnya berupa JSON murni.
*   **Conditional Routing:** 
    *   **Status DARURAT:** Mengirim respons prioritas dengan *disclaimer* medis tegas ke pasien dan mencatat data ke **Google Sheets** dengan status `DARURAT_COMPLETED`.
    *   **Status SEDANG/RINGAN:** Mengirim instruksi saran awal yang aman dan mengubah status menjadi `MENUNGGU_JADWAL`.
    *   **Status NON_MEDIS:** Otomatis membalas menggunakan template ramah untuk diarahkan ke admin operasional.

### 📄 Berkas Workflow
*   File konfigurasi n8n dapat diunduh di repositori ini: `AI_Medical_Triage_System.json`

---

## ⚡ 2. Proyek Live Demo: Server Email Error Monitoring System

Sistem monitoring *real-time* yang menangkap kegagalan pengiriman email dari server internal (misal: SMTP timeout) melalui Webhook, lalu memformat data JSON tersebut menjadi notifikasi darurat berbasis HTML yang rapi ke Telegram tim teknis untuk mempercepat respons perbaikan (*SLA*).

### 🛠️ Komponen Teknis
*   **Webhook Ingestion:** Menerima payload POST JSON dari sistem pemantau server.
*   **Formatting Node:** Mengubah variabel waktu ke Zona Asia/Jakarta dan membungkus teks pesan ke format elemen HTML Telegram (`<b>`, `<code>`, `<i>`).
*   **Notification Engine:** Telegram Bot API (Parse Mode: HTML).

### 🧪 COBA LIVE DEMO INTERAKTIF (Uji Coba Sistem Langsung)
Anda bisa menguji performa alur kerja sistem monitoring ini secara *real-time* dengan langkah berikut:
1.  **Aktifkan Bot Penerima:** Buka Bot Telegram ini [t.me/email_alert_monitor_bot](https://t.me) lalu klik **`/start`**.
2.  **Kirim Trigger Simulasi Error:** Buka alat simulator API eksternal ini di browser HP atau Laptop Anda: [ReqBin Simulator](https://reqbin.com)
3.  **Eksekusi:** Klik tombol **"Send"** di halaman ReqBin tersebut untuk mengirimkan payload simulasi error server ke webhook n8n saya.
4.  **Cek Hasilnya:** Notifikasi darurat terformat HTML yang rapi akan langsung masuk ke Telegram Anda dalam waktu kurang dari 1 detik!

### 📄 Berkas Workflow
*   File konfigurasi n8n dapat diunduh di repositori ini: `Server_Email_Error_Monitoring.json`

---

## 🚀 Cara Menggunakan Workflow Ini di n8n Anda
1.  Unduh file `.json` pilihan Anda dari repositori ini.
2.  Buka dashboard n8n Anda, buat alur kerja (*workflow*) baru yang kosong.
3.  Klik menu tiga titik di pojok kanan atas, pilih **Import from File**, lalu pilih file `.json` yang sudah diunduh.
4.  Sesuaikan bagian *Credentials* (API Key OpenAI, Akun Telegram, atau Google Sheets) dengan milik Anda sendiri.

---

### 🤝 Kontak & Kolaborasi
*   **LinkedIn:** [www.linkedin.com/in/dilfuahsanzahrudin](https://linkedin.com)
*   **Email:** dilfu.ahsan@gmail.com
