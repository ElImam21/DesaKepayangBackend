# Desa Kepayang Backend API

> REST API backend untuk sistem informasi Desa Kepayang, dibangun menggunakan Go dengan framework Gin, dilengkapi upload gambar via Cloudinary dan komunikasi real-time via WebSocket.

---

## Tentang Proyek

DesaKepayangBackend adalah backend API untuk website resmi Desa Kepayang. Sistem ini mengelola berbagai data desa mulai dari berita, data penduduk, struktur organisasi, hingga sambutan kepala desa — semua melalui REST API yang aman dengan autentikasi JWT dan CORS yang dikonfigurasi dengan baik.

Proyek ini juga dilengkapi dengan fitur **WebSocket** untuk komunikasi real-time (misalnya komentar live) dan integrasi **Cloudinary** untuk manajemen upload gambar/media.

---

## Fitur & Modul

| Modul | Deskripsi |
|---|---|
| **Admin** | Autentikasi & manajemen akun admin |
| **Sambutan Kepala Desa** | Kelola teks sambutan dari kepala desa |
| **Berita** | CRUD berita dan pengumuman desa |
| **Visi & Misi** | Kelola visi dan misi desa |
| **Struktur Desa** | Data struktur organisasi perangkat desa |
| **RT/RW** | Data wilayah RT dan RW |
| **Data Penduduk** | Manajemen data kependudukan (terhubung ke RT/RW) |
| **Info Desa** | Informasi umum desa |
| **Komentar** | Sistem komentar dengan dukungan WebSocket real-time |

---

## Tech Stack

| Teknologi | Versi | Fungsi |
|---|---|---|
| [Go](https://go.dev) | 1.24.4 | Bahasa pemrograman |
| [Gin](https://gin-gonic.com) | v1.10.1 | HTTP framework |
| [GORM](https://gorm.io) | v1.30.1 | ORM |
| [MySQL](https://mysql.com) | (via GORM driver) | Database |
| [JWT](https://jwt.io) | v5.2.3 | Autentikasi token |
| [bcrypt](https://pkg.go.dev/golang.org/x/crypto) | v0.40.0 | Hashing password |
| [Cloudinary](https://cloudinary.com) | v2.11.1 | Upload & manajemen gambar |
| [WebSocket](https://github.com/gorilla/websocket) | v1.5.3 | Komunikasi real-time |
| [CORS Middleware](https://github.com/gin-contrib/cors) | v1.7.6 | Konfigurasi CORS |
| [godotenv](https://github.com/joho/godotenv) | v1.5.1 | Manajemen env variables |

---

## Struktur Proyek

```
desa-kepayang-backend/
├── config/           # Inisialisasi DB dan Cloudinary
├── controllers/      # Handler untuk setiap route
├── helpers/          # Fungsi utilitas & helper
├── middleware/       # CORS, autentikasi JWT, dsb
├── models/           # Definisi struct & skema database
├── routes/           # Pendaftaran semua route
├── uploads/          # File upload sementara (lokal)
├── main.go           # Entry point & inisialisasi server
├── go.mod
└── go.sum
```

---

## Model Database

```
SambutanKepalaDesa
Admin
Berita
VisiMisi
StrukturDesa
RTRW
DataPenduduk  ──FK──► RTRW
InfoDesa
Komentar
```

`DataPenduduk` memiliki relasi foreign key ke tabel `RTRW` dengan aturan `ON DELETE RESTRICT` dan `ON UPDATE CASCADE`.

---

## Environment Variables

Buat file `.env` di root project:

```env
# Server
APP_PORT=8080

# Database
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=desa_kepayang

# JWT
JWT_SECRET=your_jwt_secret

# Cloudinary
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
```

---

## Memulai (Development)

### Prasyarat

- Go >= 1.21
- MySQL (lokal atau cloud)
- Akun [Cloudinary](https://cloudinary.com) (gratis tersedia)

### Instalasi

```bash
# Clone repository
git clone https://github.com/ElImam21/DesaKepayangBackend.git
cd DesaKepayangBackend

# Download dependencies
go mod tidy

# Buat file .env dan isi sesuai konfigurasi
cp .env.example .env
```

### Menjalankan Server

```bash
go run main.go
```

Server akan berjalan dan otomatis melakukan **AutoMigrate** — membuat semua tabel yang dibutuhkan di database.

### Build Binary

```bash
go build -o desa-kepayang-backend main.go
./desa-kepayang-backend
```

---

## API Routes

| Method | Endpoint | Modul |
|---|---|---|
| `POST/GET` | `/admin/...` | Admin & Auth |
| `GET/POST/PUT/DELETE` | `/sambutan/...` | Sambutan Kepala Desa |
| `GET/POST/PUT/DELETE` | `/berita/...` | Berita |
| `GET/POST/PUT/DELETE` | `/visi-misi/...` | Visi & Misi |
| `GET/POST/PUT/DELETE` | `/struktur/...` | Struktur Desa |
| `GET/POST/PUT/DELETE` | `/rtrw/...` | RT/RW |
| `GET/POST/PUT/DELETE` | `/penduduk/...` | Data Penduduk |
| `GET/POST/PUT/DELETE` | `/info-desa/...` | Info Desa |
| `GET/POST/DELETE` | `/komentar/...` | Komentar |
| `WS` | `/ws` | WebSocket real-time |

---

## Deployment

Backend Go membutuhkan server dengan akses runtime. Beberapa opsi yang sesuai:

- **VPS** — DigitalOcean, Vultr, Contabo
- **Railway** — support Go, harga terjangkau
- **Fly.io** — free tier tersedia untuk container kecil
- **Docker** — containerize untuk deploy ke mana saja

---

## Lisensi

Private — Hak cipta © 2026 Hakimi Junior. Semua hak dilindungi.
