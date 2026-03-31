# Recotrack - Recommendation System

Hệ thống đề xuất sản phẩm (Recommendation System) với architecture microservices dùng Docker Compose.

## 📋 Tổng quan dự án

Recotrack gồm 4 services chính chạy trong Docker:
- **Client**: React frontend (port 5174)
- **Web Config Server**: NestJS API quản lý cấu hình (port 3001)
- **Module Server**: NestJS API module chính (port 3000)
- **Recommender API**: Python FastAPI engine khuyến nghị (port 8000)

## ⚙️ Yêu cầu hệ thống

- **Docker**: phiên bản 20.10+
- **Docker Compose**: phiên bản 2.0+
- **Git**: để clone repository

### Kiểm tra cài đặt

```bash
docker --version
docker-compose --version
```

## 🚀 Hướng dẫn chạy nhanh

### 1. Clone repository

```bash
git clone <repository-url>
cd recotrack
```

### 2. Cấu hình environment

```bash
cd recotrack-web-config

# Copy .env.example thành .env
cp server/.env.example server/.env

# Mở server/.env và fill các giá trị cần thiết
# Xem file .env.example để biết các biến cần config
```

### 3. Chạy

```bash
docker-compose up -d
```

### 4. Kiểm tra

```bash
docker-compose ps
```

Nếu tất cả services show "Up" ✅ là thành công!

## 🌐 Truy cập ứng dụng

| Service | URL | Mục đích |
|---------|-----|---------|
| Client | http://localhost:5174 | Giao diện người dùng |
| Web Config API | http://localhost:3001 | API cấu hình (REST) |
| Module API | http://localhost:3000 | API module chính (REST) |
| Recommender API | http://localhost:8000 | API khuyến nghị (REST + WebSocket) |
| Docs | http://localhost:8000/docs | FastAPI documentation |

## 📂 Cấu trúc thư mục

```
recotrack/
├── README.md
├── recommendation-models/
│   ├── Dockerfile
│   ├── main.py
│   ├── config.py
│   ├── requirements.txt
│   └── models/
├── recotrack-server-module/
│   └── packages/
│       ├── docker-compose.yml
│       └── server/
│           ├── Dockerfile
│           ├── package.json
│           ├── prisma/
│           └── src/
└── recotrack-web-config/
    ├── docker-compose.yml           # 👈 Main docker-compose file
    ├── client/
    │   ├── Dockerfile
    │   ├── vite.config.ts
    │   └── src/
    └── server/
        ├── Dockerfile
        ├── package.json
        ├── prisma/
        └── src/
```

## 🛠️ Các lệnh thường dùng

### Khởi động services

```bash
# Khởi động toàn bộ services
docker-compose up -d

# Khởi động kèm rebuild images
docker-compose up -d --build

# Xem logs toàn bộ services
docker-compose logs -f

# Xem logs của service cụ thể
docker-compose logs -f recotrack-client
docker-compose logs -f recotrack-server
docker-compose logs -f recotrack-module-server
docker-compose logs -f recommender-api
```

### Dừng services

```bash
# Dừng toàn bộ services (giữ data)
docker-compose stop

# Dừng và xóa containers (giữ images)
docker-compose down

# Dừng, xóa containers, images, volumes (⚠️ Sẽ xóa toàn bộ data)
docker-compose down -v
```

### Restart services

```bash
# Restart toàn bộ
docker-compose restart

# Restart service cụ thể
docker-compose restart recotrack-server
```

### Chạy commands trong container

```bash
# Vào terminal của service
docker-compose exec recotrack-server bash

# Chạy database migration
docker-compose exec recotrack-server npm run migrate
```

## 🔧 Khắc phục sự cố

### Problem: Port đã được sử dụng

```bash
# Tìm process dùng port
netstat -ano | findstr :5174    # Trên Windows
lsof -i :5174                    # Trên macOS/Linux

# Kill process (Windows)
taskkill /PID <PID> /F

# Hoặc thay đổi port trong docker-compose.yml
```

### Problem: Containers không khởi động

```bash
# Kiểm tra logs chi tiết
docker-compose logs recotrack-server

# Rebuild images (xóa cache cũ)
docker-compose down
docker-compose up -d --build

# Kiểm tra .env files có đúng không
```

### Problem: Database connection error

```bash
# Đảm bảo DATABASE_URL đúng trong .env
# Format: postgresql://user:password@host:port/database

# Restart database service
docker-compose restart

# Kiểm tra database có tồn tại không
```

### Problem: Node modules missmatches

```bash
# Clean install từ đầu
docker-compose down
docker system prune -a
docker-compose up -d --build
```

## 📝 Phát triển (Development)

### Multi-file docker-compose (Optional)

Nếu muốn chạy từng service riêng:

```bash
# Chỉ chạy module server
cd recotrack-server-module/packages
docker-compose up -d

# Chỉ chạy recommender API
cd recommendation-models
docker build -t recommender-api .
docker run -p 8000:8000 recommender-api
```

### Hot reload

Clients hỗ trợ hot-reload qua volume mounting. Thay đổi code sẽ tự động load lại.

### Scripts hữu ích

```bash
# Xóa tất cả Docker data (⚠️ Cẩn thận!)
docker system prune -a

# Xem resource usage
docker stats

# Inspect network
docker network ls
docker network inspect recsys-network
```

## 📚 Tài liệu thêm

- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [NestJS Documentation](https://docs.nestjs.com/)
- [React Documentation](https://react.dev/)
- [FastAPI Documentation](https://fastapi.tiangolo.com/)

## 👥 Hỗ trợ

Nếu gặp vấn đề:
1. Kiểm tra logs: `docker-compose logs`
2. Xem mục "Khắc phục sự cố" ở trên
3. Đảm bảo Docker & Docker Compose đã cài đặt đúng
4. Thử rebuild: `docker-compose up -d --build`

---

**Happy coding hihihi!**
