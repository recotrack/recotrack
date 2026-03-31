# 🐳 Docker Commands Reference

Tài liệu nhanh cho các lệnh Docker & Docker Compose thường dùng.

## 📂 Trước tiên: Vào thư mục đúng

```bash
cd recotrack-web-config
# Tất cả lệnh docker-compose phải chạy từ thư mục chứa docker-compose.yml
```

## ✅ Khởi động & Dừng

### Khởi động

```bash
# Khởi động tất cả services (background)
docker-compose up -d

# Khởi động kèm rebuild images (nếu code có thay đổi)
docker-compose up -d --build

# Khởi động và xem logs (foreground)
docker-compose up

# Khởi động service cụ thể
docker-compose up -d client
docker-compose up -d server
```

### Dừng

```bash
# Dừng tất cả (containers vẫn tồn tại, data lưu giữ)
docker-compose stop

# Dừng service cụ thể
docker-compose stop client

# Dừng & xóa containers (images & volumes giữ lại)
docker-compose down

# Dừng & xóa mọi thứ (⚠️ Data bị xóa!)
docker-compose down -v

# Remove dangling images/containers/volumes
docker system prune
docker system prune -a        # Xóa cả unused images

# Remove nur unused volumes
docker volume prune
```

## 🔄 Restart

```bash
# Restart tất cả
docker-compose restart

# Restart service cụ thể
docker-compose restart server
docker-compose restart recommender-api

# Force restart (kill + start)
docker-compose kill
docker-compose up -d
```

## 📊 Kiểm tra trạng thái

```bash
# Xem tất cả containers & status
docker-compose ps

# Xem chi tiết services
docker-compose ps -a

# List tất cả services
docker-compose config --services
```

## 📝 Logs

```bash
# Xem logs tất cả services (realtime)
docker-compose logs -f

# Xem 50 line gần nhất
docker-compose logs --tail=50

# Xem logs service cụ thể
docker-compose logs -f client
docker-compose logs -f server
docker-compose logs -f module-server
docker-compose logs -f recommender-api

# Xem logs kèm timestamps
docker-compose logs -t

# Xem logs từ thời điểm cụ thể (5 phút gần đây)
docker-compose logs --since 5m

# Lưu logs vào file
docker-compose logs > logs.txt
```

## 🔧 Chạy commands trong container

```bash
# Vào bash shell của container
docker-compose exec server bash
docker-compose exec client sh
docker-compose exec recommender-api bash

# Chạy command cụ thể
docker-compose exec server npm run build
docker-compose exec server npm test

# Chạy với user cụ thể
docker-compose exec -u root server apt-get update

# Không interactive (chỉ output kết quả)
docker-compose exec -T server npm run migrate
```

## 🔨 Build & Rebuild

```bash
# Build tất cả images
docker-compose build

# Build service cụ thể
docker-compose build server
docker-compose build client

# Build without cache (rebuild from scratch)
docker-compose build --no-cache

# Build kèm rebuild deps
docker-compose build --no-cache --force-rm

# Xem images đã tạo
docker images | grep recotrack
```

## 📦 Quản lý services

```bash
# Xem service status
docker-compose ps

# Xem running processes của service
docker-compose top server

# Inspect service details
docker-compose config

# Validate docker-compose.yml
docker-compose config --quiet && echo "✅ Valid"

# Show image used by service
docker-compose images
```

## 🌐 Network

```bash
# List networks
docker network ls

# Inspect recsys-network
docker network inspect recsys-network

# Test connectivity giữa containers
docker-compose exec server ping recommender-api
docker-compose exec recommender-api ping server
```

## 💾 Volumes & Data

```bash
# List volumes
docker volume ls

# Inspect volume
docker volume inspect <volume-name>

# Xem file trong volume
docker volume inspect postgres_data

# Backup volume
docker run --rm -v postgres_data:/data -v $(pwd):/backup \
  alpine tar czf /backup/postgres_data.tar.gz -C /data .

# Restore volume (từ backup)
docker run --rm -v postgres_data:/data -v $(pwd):/backup \
  alpine tar xzf /backup/postgres_data.tar.gz -C /data
```

## 🐛 Debugging

```bash
# Xem resource usage (live)
docker stats

# Xem resource usage của service cụ thể
docker stats recotrack-server

# Xem error details
docker-compose logs --no-log-prefix mysql-server | grep -i error

# Check container health
docker-compose ps

# Inspect container
docker inspect <container-name>

# Check network connectivity
docker-compose exec server curl http://recommender-api:8000/health

# View container filesystems
docker-compose exec server ls -la /app
```

## 🧹 Cleanup

```bash
# Xóa stopped containers
docker container prune

# Xóa dangling images (không gắn với container nào)
docker image prune

# Xóa dangling volumes
docker volume prune

# Xóa tất cả unused (containers, images, networks, volumes)
docker system prune -a

# Free up space (careful!)
docker system prune -a --volumes
```

## 📋 Pull & Push images

```bash
# Download updated images
docker-compose pull

# Push images to registry
docker push my-registry/image-name:tag

# Tag image
docker tag old-image:tag new-registry/new-image:tag
```

## 🔐 Security

```bash
# Scan image for vulnerabilities
docker scan image-name

# Check running processes (security)
docker-compose top server

# View network packets (debugging)
docker-compose exec server tcpdump -i any -n port 3001
```

## 📊 Cheat Sheet nhanh

| Lệnh | Mục đích |
|------|---------|
| `docker-compose up -d` | Start all services |
| `docker-compose ps` | See status |
| `docker-compose logs -f` | Stream logs |
| `docker-compose down` | Stop all |
| `docker-compose exec server bash` | Vào terminal |
| `docker-compose restart` | Restart all |
| `docker compose build --no-cache` | Rebuild images |
| `docker stats` | System resources |
| `docker system prune` | Cleanup |

## ⚠️ Common Mistakes

❌ **Sai**: Chạy lệnh từ thư mục sai
```bash
cd recotrack  # ❌ WRONG - docker-compose.yml không ở đây
docker-compose ps
```

✅ **Đúng**:
```bash
cd recotrack-web-config  # ✅ CORRECT
docker-compose ps
```

❌ **Sai**: Quên `-d` flag
```bash
docker-compose up  # Terminal bị block, không thể dùng
```

✅ **Đúng**:
```bash
docker-compose up -d  # Chạy background, terminal vẫn dùng được
```

## 🆘 Nhanh chóng khắc phục

```bash
# Something not working?
docker-compose down                 # Stop all
docker system prune -a              # Clean everything
docker-compose up -d --build        # Fresh start
docker-compose logs -f              # Check logs

# Still broken?
docker-compose down -v              # ⚠️ Delete all data
docker-compose up -d --build        # Start fresh
```

---

**Pro tip**: Lưu file này để reference nhanh! 📌
