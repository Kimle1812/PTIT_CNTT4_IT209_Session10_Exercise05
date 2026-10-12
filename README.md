# Bài 5: Docker Compose và biến môi trường .env

## 1. Mục tiêu

- Quản lý nhiều container bằng Docker Compose CLI v2.
- Sử dụng file `.env` để cấu hình biến môi trường.
- Sử dụng `.env.example` để cung cấp cấu hình mẫu.
- Tự động tạo network và volume cho hệ thống.

## 2. Công nghệ sử dụng

- Docker Engine
- Docker Compose CLI v2
- Backend từ bài tập EX4
- PostgreSQL 15 Alpine

## 3. Cấu trúc thư mục

```text
ex5/
├── Dockerfile
├── docker-compose.yml
├── .env
├── .env.example
├── .gitignore
├── README.md
└── server.js
```

## 4. Mô hình hệ thống

Hệ thống gồm hai dịch vụ:

- Backend: xây dựng từ Dockerfile và cung cấp API trên cổng 8080 trong container.
- Database: sử dụng PostgreSQL 15 Alpine, lưu dữ liệu trong volume `pgdata`.

Hai dịch vụ kết nối qua network `backend-net`. Backend sử dụng hostname `database` để kết nối PostgreSQL.

## 5. Cấu hình môi trường

File `.env` chứa các biến:

- `DB_NAME`: tên database.
- `DB_USER`: tài khoản database.
- `DB_PASSWORD`: mật khẩu database.
- `PORT`: cổng truy cập backend trên máy host.

File `.env.example` cung cấp cấu hình mẫu để các thành viên có thể tự tạo file `.env`.

File `.env` được thêm vào `.gitignore` để tránh đưa thông tin nhạy cảm lên GitHub.

## 6. Khởi chạy hệ thống

Mở terminal tại thư mục chứa `docker-compose.yml`.

Kiểm tra cấu hình:

```bash
docker compose config
```

Khởi động hai dịch vụ:

```bash
docker compose up -d
```

Kiểm tra trạng thái:

```bash
docker compose ps
```

Xem log của các dịch vụ:

```bash
docker compose logs -f
```

Xem log riêng backend:

```bash
docker compose logs -f backend
```

Xem log riêng database:

```bash
docker compose logs -f database
```

## 7. Dừng hệ thống

```bash
docker compose down
```

Lệnh trên dừng và xóa các container cùng network do Compose quản lý. Volume `pgdata` vẫn được giữ lại mặc định.

Nếu muốn xóa cả dữ liệu database đã lưu, có thể sử dụng:

```bash
docker compose down -v
```

Chỉ chạy lệnh này khi thực sự muốn xóa dữ liệu PostgreSQL.

## 8. Kết quả mong đợi

- Docker Compose đọc cấu hình từ `docker-compose.yml` và `.env`.
- Hai container backend và database được tạo.
- Network `backend-net` được tạo tự động.
- Volume `pgdata` được tạo để lưu dữ liệu PostgreSQL.
- Cả hai dịch vụ chạy ổn định và hiển thị trạng thái `Up` trong `docker compose ps`.

## 9. Kết luận

Đã xây dựng cấu hình Docker Compose để quản lý backend và database, tách cấu hình môi trường bằng `.env`, cung cấp file `.env.example` và cấu hình `.gitignore` nhằm hạn chế lộ thông tin nhạy cảm.
