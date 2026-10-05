# senlabs-infra

Infrastruktur Gateway & Reverse Proxy untuk semua project di ekosistem Senlabs VPS.

## Arsitektur

```text
Internet
   │
   ▼
[ Nginx (Port 80/443 + SSL Let's Encrypt) ] ── host-gateway ──► [ Docker Container Apps ]
```

## Daftar Domain & Subdomain

| Subdomain | Project | Routing Proxy |
| :--- | :--- | :--- |
| `senlabs.web.id` | [`my-portofolio`](../my-portofolio) | `http://host-gateway:3004` |
| `mock.senlabs.web.id` | [`mock-api`](../mock-api) | Frontend: `:3000`, API: `:8080` |
| `habit.senlabs.web.id` | [`habit-apps`](../habit-apps) | Frontend: `:3001`, API: `:8081` |
| `asynq.senlabs.web.id` | [`asynqmon-hub`](../asynqmon-hub) | Frontend: `:3002` |
| `dbviewer.senlabs.web.id` | [`database-query-viewer`](../database-query-viewer) | Frontend: `:3003`, API: `:8083` |
| `menu.senlabs.web.id` | [`go-menu`](../go-menu) | Frontend: `:3005`, API: `:8085` |
| `jobs.senlabs.web.id` | [`jobs-scheduler`](../jobs-scheduler) | Frontend: `:3006`, API: `:8086` |
| `microservice.senlabs.web.id`| [`grpc-microservice-simulation`](../grpc-microservice-simulation) | Frontend: `:3007`, API: `:8087` |
| `sshark.senlabs.web.id` | [`sshark - ssh viewer`](../sshark%20-%20ssh%20viewer) | App: `:8088` |
| `picoclaw.senlabs.web.id` | PicoClaw Internal | `:18800` |
| `portainer.senlabs.web.id` | Portainer Manager | `https://host-gateway:9443` |

---

## Panduan Menjalankan di VPS

1. **Jalankan Nginx Reverse Proxy:**
   ```bash
   cd infra-files
   docker compose up -d
   ```

2. **Reload Konfigurasi Nginx (setelah update config):**
   ```bash
   docker compose exec nginx nginx -s reload
   ```

3. **Detail Aturan Deployment Lengkap:**
   Lihat [`DEPLOYMENT-RULES.md`](../DEPLOYMENT-RULES.md) di root direktori.
