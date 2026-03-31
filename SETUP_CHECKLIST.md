# ✅ Setup - Từng bước

## 📋 Yêu cầu

- [ ] Docker Desktop cài đặt
- [ ] Docker Compose cài đặt (đi kèm Docker Desktop)
- [ ] Git cài đặt

---

## 🚀 Bước 1 - Chuẩn bị

```bash
# Clone
git clone <repo-url>
cd recotrack/recotrack-web-config

# Copy env template
cp server/.env.example server/.env

# Edit .env - fill các biến theo file .env.example
# Ví dụ config tối thiểu:
# - DATABASE_URL
# - JWT_ACCESS_SECRET
# - JWT_REFRESH_SECRET
```

---

## 🚀 Bước 2 - Run

```bash
# Khởi động services
docker-compose up -d

# Kiểm tra
docker-compose ps
```

---

## 🌐 Bước 3 - Truy cập

| Service | URL |
|---------|-----|
| Client | http://localhost:5174 |
| API | http://localhost:3001 |
| Module | http://localhost:3000 |
| Docs | http://localhost:8000/docs |

---

## 📝 Lệnh hữu ích

```bash
# Log
docker-compose logs -f

# Restart
docker-compose restart

# Stop
docker-compose stop

# Clean
docker-compose down -v
```

---

## 🆘 Xử lý lỗi

### Port conflict
```bash
# Find process
netstat -ano | findstr :5174
# Kill it
taskkill /PID <PID> /F
```

### Container error
```bash
docker-compose logs
docker-compose down -v
docker-compose up -d --build
```

### Database connection
- Check .env file
- Try `localhost` hoặc `host.docker.internal`

---

Xem [README.md](README.md) hoặc [DOCKER_COMMANDS_REFERENCE.md](DOCKER_COMMANDS_REFERENCE.md) để tìm hiểu thêm.
