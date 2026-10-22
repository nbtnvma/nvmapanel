# Changelog

## 0.2.0 — bản thử nghiệm 0.2.0-beta.1 (2026-10-22) / beta

**Free**
- Website Laravel: thư mục gốc tuỳ chọn (`public/`), mẫu Laravel (scheduler tự thêm), PHP CLI và Composer theo từng site.
- SSL tự cấp khi tạo website (chờ DNS và tự thử lại 48 giờ), chọn nhà cung cấp Let's Encrypt / ZeroSSL / Google / ACME tuỳ chỉnh, wildcard qua Cloudflare, Cloudflare Origin CA, tạo CSR cho chứng chỉ thương mại.
- PHP riêng cho từng domain (memory, upload, thời gian chạy…), quy tắc website (chuyển hướng, khoá thư mục bằng mật khẩu, chặn IP, trang lỗi, chống hotlink).
- MySQL 8.4 LTS (hoặc MariaDB), phpMyAdmin, Adminer, nhập file SQL lớn (.sql/.sql.gz).
- SFTP (chroot) với SSH key, tài khoản FTP/FTPS (Pure-FTPd).
- Email: chỉ gửi qua SES/Resend/Brevo hoặc đầy đủ (Postfix, Dovecot, DKIM, chuyển tiếp, trả lời tự động), webmail SnappyMail.
- Zone Editor: Cloudflare DNS và PowerDNS, tự tạo bản ghi A khi thêm website, tạo bản ghi email một lần bấm.
- Sao lưu website và database tại máy theo lịch, khôi phục, tải về. Biểu đồ tài nguyên 24 giờ. Cloudflare real IP.

**Pro — Game & Realtime**
- Cài nhanh cả bộ: Laravel realtime (PHP, MySQL, Redis, Horizon/queue, Reverb) và Node/Go realtime (Node, Redis, PostgreSQL/PostGIS, PgBouncer).
- Worker nhiều tiến trình, proxy theo đường dẫn (WebSocket), chế độ hiệu năng PHP-FPM, tinh chỉnh MySQL/Redis/Nginx, giới hạn request.
- Giám sát dịch vụ, cảnh báo Telegram, deploy Git không gián đoạn + webhook, nhân bản staging, sao lưu lên S3/R2/B2/SFTP.

**Chung**
- Giấy phép qua SDK chính thức của license.nvma.site 2.1: số tài khoản theo giới hạn của gói, thông báo khi có bản mới (thông tin đã ký).
- Cài đặt từ GitHub Releases hoặc bản sao trên license.nvma.site; chữ ký Ed25519 được kiểm tra ở cả hai nguồn.
- Logo NVMA chính thức.

## 0.1.0 — MVP
- Engine riêng (Go), web không root + agent root, SQLite, Ubuntu 22.04/24.04, AlmaLinux 9, Rocky 9.
- Website PHP/tĩnh/proxy, Let's Encrypt, PHP 7.4–8.4, MariaDB, PostgreSQL, Redis, Node.js, Docker, Fail2ban.
- Apps (Pro), cron, tường lửa, quản lý file, terminal, 2FA, nhật ký, license Free/Pro/Business/Agency.
