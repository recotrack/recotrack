# 🚀 QUICKSTART - Chạy Recotrack trong 2 phút

## Prerequisites

- Docker desktop hoặc Docker + Docker Compose

## ⚡ Run it!

```bash
# Clone
git clone <repo-url>
cd recotrack/recotrack-web-config

# Setup environment
cp server/.env.example server/.env
# → Mở server/.env, fill các biến theo file .env.example

# Run
docker-compose up -d

# Check
docker-compose ps
```

Xong! Truy cập:
- **Client**: http://localhost:5174
- **API**: http://localhost:3001
- **Docs**: http://localhost:8000/docs

---

## 🆘 Lỗi?

```bash
# Xem logs
docker-compose logs

# Rebuild
docker-compose down -v
docker-compose up -d --build
```

Xem [README.md](README.md) hoặc [DOCKER_COMMANDS_REFERENCE.md](DOCKER_COMMANDS_REFERENCE.md) để fix.

