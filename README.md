# Senlabs Infra

Infrastruktur reverse proxy dan SSL management pusat untuk server Senlabs menggunakan **Nginx Proxy Manager (NPM)**.

---

## 🏗️ Arsitektur

```
Internet
    │
    ▼
[Nginx Proxy Manager] (repo ini)
    ├── 80 / 443 (Let's Encrypt SSL)
    └── 81 (Admin UI, loopback 127.0.0.1)
    │
    ├── mock.senlabs.web.id      ──► 127.0.0.1:3000  (Mock API Frontend)
    ├── mock.senlabs.web.id/api/ ──► 127.0.0.1:8080  (Mock API Backend)
    └── portainer.senlabs.web.id ──► host-gateway:9000 (Portainer Standalone)
```

---

## 🚀 Setup di VPS

### 1. Prasyarat
- Docker & Docker Compose terpasang.
- Port `80` dan `443` belum digunakan oleh service lain (stop service nginx lama jika ada).

### 2. Menjalankan Nginx Proxy Manager
Clone dan jalankan compose:
```bash
cd ~/senlabs-infra
docker compose up -d
```

### 3. Mengakses Admin UI (Port 81)
Port admin `81` dibind ke `127.0.0.1:81` demi keamanan. Akses dilakukan via SSH Port Forwarding dari komputer lokal:

```bash
ssh -L 81:127.0.0.1:81 ubuntu@43.157.201.197
```

Buka browser dan akses:
**`http://localhost:81`**

**Default Credentials:**
- **Email:** `admin@example.com`
- **Password:** `changeme`
*(Ganti kredensial segera setelah login pertama kali)*

---

## ⚙️ Konfigurasi Proxy Hosts di NPM UI

### 1. Mock API Server (`mock.senlabs.web.id`)
1. Buka **Proxy Hosts** $\rightarrow$ **Add Proxy Host**.
2. **Details Tab:**
   - **Domain Names:** `mock.senlabs.web.id`
   - **Scheme:** `http`
   - **Forward Hostname / IP:** `127.0.0.1` (atau `host-gateway`)
   - **Forward Port:** `3000`
   - Aktifkan: **Block Common Exploits**, **Websockets Support**
3. **Custom Locations Tab:**
   - **Location:** `/api/`
   - **Scheme:** `http`
   - **Forward Hostname / IP:** `127.0.0.1` (atau `host-gateway`)
   - **Forward Port:** `8080`
4. **SSL Tab:**
   - Pilih **Request a new SSL Certificate**.
   - Aktifkan: **Force SSL**, **HTTP/2 Support**, **HSTS Enabled**.
   - Setujui Terms of Service Let's Encrypt.
5. Klik **Save**.

---

### 2. Portainer (`portainer.senlabs.web.id`)
1. Buka **Proxy Hosts** $\rightarrow$ **Add Proxy Host**.
2. **Details Tab:**
   - **Domain Names:** `portainer.senlabs.web.id`
   - **Scheme:** `http`
   - **Forward Hostname / IP:** `host-gateway`
   - **Forward Port:** `9000`
   - Aktifkan: **Block Common Exploits**, **Websockets Support**
3. **SSL Tab:**
   - Pilih **Request a new SSL Certificate**.
   - Aktifkan: **Force SSL**, **HTTP/2 Support**, **HSTS Enabled**.
   - Setujui Terms of Service Let's Encrypt.
4. Klik **Save**.
