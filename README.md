# Thực hành: WAF trên Nginx (2/2)

Dựng OWASP Juice Shop (ứng dụng có lỗ hổng) và đặt Nginx + ModSecurity + OWASP CRS làm WAF đứng trước nó.

| Dịch vụ      | Cổng                    | Mô tả                                      |
|--------------|-------------------------|--------------------------------------------|
| `juice-shop` | http://localhost:3000   | Truy cập thẳng ứng dụng, **không có WAF**  |
| `waf`        | http://localhost:8080   | Đi qua Nginx + ModSecurity (OWASP CRS)     |

## Chạy

```bash
docker compose up -d
```

Đợi khoảng 30 giây cho Juice Shop khởi động xong.

## Các bước thử

| Thử              | Làm gì                                                                 | Kết quả mong đợi                       |
|------------------|------------------------------------------------------------------------|----------------------------------------|
| Không có WAF     | Mở `localhost:3000` → Login<br>Email: `' OR 1=1--` Mật khẩu: bất kỳ    | Vào được tài khoản admin!              |
| Có WAF           | Mở `localhost:8080`, đăng nhập y hệt                                   | 403 Forbidden, bị chặn                 |
| Đăng nhập đúng   | Qua `:8080` với `admin@juice-sh.op` / `admin123`                       | Vào bình thường                        |
| Xem lý do        | `docker compose logs waf`                                              | Thấy luật 942100 (SQL injection)       |

Lọc nhanh dòng log của luật 942100:

```bash
docker compose logs waf | grep 942100
```

## Dừng

```bash
docker compose down
```
