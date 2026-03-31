# Recotrack Web Config - Docker Compose

Thư mục này có `docker-compose.yml` để run toàn bộ hệ thống.

## 🚀 Chạy

### 1. Setup environment

```bash
cd recotrack-web-config

# Copy template
cp server/.env.example server/.env

# Edit .env - điền các biến theo file .env.example
```

### 2. Run

```bash
docker-compose up -d
```

### 3. Check status

```bash
docker-compose ps
```

---

## 🌐 Services

| Service | Port | URL |
|---------|------|-----|
| Client (React) | 5174 | http://localhost:5174 |
| Web Config API | 3001 | http://localhost:3001 |
| Module API | 3000 | http://localhost:3000 |
| Recommender API | 8000 | http://localhost:8000 |

---

## 📂 Cấu trúc

```
recotrack-web-config/
├── docker-compose.yml          # Main file
├── README.md
├── client/                     # React frontend
│   └── Dockerfile
└── server/                     # NestJS backend
    ├── Dockerfile
    └── .env.example            # Copy to .env
```

---

## 📝 Lệnh thường dùng

```bash
# Start
docker-compose up -d

# Stop
docker-compose stop

# Logs
docker-compose logs -f

# Restart
docker-compose restart

# Clean
docker-compose down -v
```

---

Xem [../README.md](../README.md) hoặc [../DOCKER_COMMANDS_REFERENCE.md](../DOCKER_COMMANDS_REFERENCE.md) để tìm hiểu thêm.

