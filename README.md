# Website bán đồ nội thất

Dự án đồ án tích hợp của nhóm Trần Đức Lương, Nguyễn Như Kiên và Phan Thế Kiệt.

## Công nghệ nền tảng

- PHP 8.3 và Laravel 11
- MySQL 8
- Docker Compose
- phpMyAdmin
- GitHub Actions

## Khởi động bằng Docker

```bash
cp .env.example .env
docker compose up -d --build
docker compose exec app composer install
docker compose exec app php artisan key:generate
docker compose exec app php artisan migrate
```

- Website: http://localhost:8000
- phpMyAdmin: http://localhost:8080
- MySQL trong Docker: database `furniture`, user `furniture`, password `secret`

## Nhánh làm việc

- `main`: mã nguồn đã tích hợp và ổn định.
- `Duc_Luong`: phần việc của Trần Đức Lương.
- `Nhu_Kien`: phần việc của Nguyễn Như Kiên.
- `Phan_Kiet`: phần việc của Phan Thế Kiệt.

Không commit file `.env` hoặc thông tin bí mật lên GitHub.
