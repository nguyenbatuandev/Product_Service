# Product Service API

# 🚀 Product Service API

## ⚡ Quick Start

### 1. Clone Repository
```bash
git clone <repository-url>
cd Product_Service
```

### 2. Chạy Dự Án với Docker
```bash
docker-compose up --build
```

API sẽ chạy tại: `http://localhost:8082`

---

## 📚 API Endpoints

**Base URL:** `http://localhost:8082`  
**Auth Header:** `Authorization: Bearer <jwt-token>`

### Public Endpoints
- **Get All Products**
  ```
  GET /api/v1/products
  ```
- **Get Product by ID**
  ```
  GET /api/v1/product-id/{id}
  ```
- **Search Product by Name**
  ```
  GET /api/v1/product-name/{name}
  ```

### Protected Endpoints (Partner/Admin Only)
- **Create Product**
  ```
  POST /api/v1/products
  Authorization: Bearer <token>
  {
    "name": "iPhone 15 Pro",
    "description": "Latest iPhone",
    "price": 29999000,
    "image": "https://example.com/image.jpg"
  }
  ```
- **Update Product**
  ```
  PATCH /api/v1/products/{id}
  Authorization: Bearer <token>
  {
    "name": "Updated Name",
    "price": 27999000
  }
  ```
- **Delete Product**
  ```
  DELETE /api/v1/products/{id}
  Authorization: Bearer <token>
  ```

### Admin Endpoints
- **Admin Delete Product**
  ```
  DELETE /api/admin/delete/{id}
  Authorization: Bearer <admin-token>
  ```

---

## 🐳 Lệnh Docker

- **Khởi chạy container**
  ```bash
  docker-compose up --build
  ```
- **Chạy background**
  ```bash
  docker-compose up -d --build
  ```
- **Dừng container**
  ```bash
  docker-compose down
  ```
- **Xem logs**
  ```bash
  docker-compose logs -f app
  ```
- **Reset (xóa data)**
  ```bash
  docker-compose down -v
  ```

---


## 📁 Cấu trúc dự án

```
Product_Service/
├── main.go                 # Entry point
├── go.mod                  # Go modules
├── go.sum                  # Dependencies checksums
├── Dockerfile              # Docker image definition
├── docker-compose.yml      # Docker services configuration
├── .env                    # Environment variables (local)
├── README.md               # Documentation
│
└── internal/               # Internal packages
    ├── cache/              # Redis cache layer
    ├── config/             # Configuration management
    ├── database/           # Database connection
    ├── entity/             # Data models & DTOs
    ├── handler/            # HTTP handlers
    ├── interface/          # Interfaces
    ├── middleware/         # HTTP middlewares
    ├── repository/         # Data access layer
    └── service/            # Business logic
```

---

## 🎯 Tính năng chính

- ✅ **Quản lý sản phẩm**: Tạo, sửa, xóa, xem sản phẩm
- ✅ **Tìm kiếm**: Tìm sản phẩm theo tên, ID
- ✅ **Phân quyền**: JWT với role-based access (Admin, Partner, Buyer)
- ✅ **Redis Cache**: Cache sản phẩm để tăng hiệu năng
- ✅ **Database**: PostgreSQL với GORM ORM
- ✅ **Security**: Input validation, SQL injection prevention
- ✅ **CORS**: Cross-Origin Resource Sharing middleware
- ✅ **Environment Config**: Cấu hình qua file .env

## Test API


### 1. Public APIs (Không cần JWT token)

**Lấy tất cả sản phẩm:**
```bash
curl -X GET http://localhost:8082/api/v1/products
```

**Lấy sản phẩm theo ID:**
```bash
curl -X GET http://localhost:8082/api/v1/product-id/f6ae861f-3828-4a26-9f70-631cde84b563
```

**Tìm sản phẩm theo tên:**
```bash
curl -X GET http://localhost:8082/api/v1/product-name/iPhone
```

### 2. Protected APIs (Cần JWT token - Partner/Admin)

**Tạo sản phẩm mới:**
```bash
curl -X POST http://localhost:8082/api/v1/products \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -d '{
    "name": "iPhone 15 Pro Max",
    "description": "Điện thoại flagship mới nhất từ Apple",
    "price": 34999000,
    "image": "https://example.com/iphone15promax.jpg"
  }'
```

**Cập nhật sản phẩm:**
```bash
curl -X PATCH http://localhost:8082/api/v1/products/f6ae861f-3828-4a26-9f70-631cde84b563 \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -d '{
    "name": "iPhone 15 Pro Max Updated",
    "description": "Mô tả đã được cập nhật",
    "price": 32999000,
    "image": "https://example.com/iphone15promax-new.jpg"
  }'
```

**Xóa sản phẩm:**
```bash
curl -X DELETE http://localhost:8082/api/v1/products/f6ae861f-3828-4a26-9f70-631cde84b563 \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

### 3. Admin Only APIs

**Admin xóa sản phẩm:**
```bash
curl -X DELETE http://localhost:8082/api/admin/delete/f6ae861f-3828-4a26-9f70-631cde84b563 \
  -H "Authorization: Bearer YOUR_ADMIN_JWT_TOKEN"
```

---

## 🔍 Kiểm tra Redis Cache

```bash
# Kết nối Redis
redis-cli -h localhost -p 6379

# Xem tất cả keys
KEYS *

# Xem giá trị của key
GET product:id:your-product-id
```
