# Changelog

## 0.2.0 — 2026-10-26

Bản chính thức đầu tiên của dòng 0.2 (gồm mọi thứ của 0.2.0-beta.1 và beta.2, liệt kê ở cuối mục này).
Đã chạy thật trên Ubuntu 24.04 ARM (AWS EC2): website Laravel + MySQL + Redis + worker + Reverb, SSL, Cloudflare,
SFTP/FTPS, sao lưu, license Pro.

**Mới so với beta.2**
- **phpMyAdmin vào thẳng**, chỉ thấy database của tài khoản; chạy trên **cổng riêng (cổng panel + 1)** để lỗi của công cụ
  không chạm được panel. Mở thêm cổng này trên tường lửa của cloud.
- **Quản lý tệp kiểu cPanel**: cây thư mục, chọn nhiều, sao chép/di chuyển, nén .zip, giải nén .zip/.tar/.tar.gz, lọc,
  sắp xếp, menu chuột phải, phím tắt.
- **PHP nâng cao**: thêm dòng php.ini theo phiên bản, bật/tắt/cài extension, `disable_functions` riêng từng site (admin).
- **Cập nhật 1 bấm** (Cài đặt → Cập nhật): kiểm chữ ký bản phát hành, sao lưu cấu hình trước, tự quay lại bản cũ nếu lỗi.
- **Sao lưu cấu hình panel** mỗi ngày (`/var/backups/nvma`), khôi phục bằng `nvma panel restore`.
- **Sao lưu đám mây mã hoá** (rclone crypt); đích SFTP dùng khoá SSH riêng của panel (hiện khoá công khai trong form).
- **Email**: chế độ đầy đủ có thể gửi thư ra qua dịch vụ trung gian (khi cloud chặn cổng 25); hướng dẫn mở cổng 25 theo
  từng nhà cung cấp (AWS, Oracle, Google, Azure, DigitalOcean, Hetzner, Vultr, Linode).
- **Bảo mật**: Terminal hỏi lại mật khẩu mỗi lần mở (tắt được hẳn); phiên đăng nhập tối đa 7 ngày, xem và đăng xuất từng
  phiên; mã 2FA không dùng lại được; **fail2ban bật mặc định** (SSH, đăng nhập panel, mail, FTP); `mail()` của một site
  không giả được tên miền khác; mỗi site có thư mục tạm riêng; cổng Docker theo tường lửa panel; quyền MySQL không còn
  khớp nhầm database của tài khoản khác (dấu `_`).
- **Rà soát bảo mật trước bản chính thức**: webmail chạy pool PHP riêng (không đọc được phiên phpMyAdmin); user MySQL của
  phpMyAdmin không thể bị tài khoản khác chiếm trước; đổi vai trò tài khoản đăng xuất mọi phiên; xem khoá mã hoá sao lưu
  cần cả mã 2FA; sao lưu/khôi phục cấu hình panel không đi theo link.
- `nvma license diagnose`: kiểm tra từng bước trả lời của máy chủ license.

**Sửa lỗi so với beta.2**
- Cloudflare proxied báo lỗi 525.
- Bật "Tự động cấp SSL" cho site có sẵn không cấp chứng chỉ.
- `npm` của user site không chạy được; cron `cd … && php artisan` dùng sai phiên bản PHP.
- License: gói có giới hạn bị báo nhầm "sai chữ ký"; key không có feature bị hạ về Free sau khi khởi động lại.
- Đặt tên miền panel đang bật proxy Cloudflare thì không vào được: nay có cảnh báo và cách sửa.
- So sánh phiên bản thử nghiệm (beta.3 > beta.2).
- Cài Webmail lỗi "no SnappyMail tarball" khi bản SnappyMail mới nhất không kèm gói.

**Đã có từ beta.1/beta.2**

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
- Cloud (Oracle Cloud, AWS, Google Cloud, Azure): tự nhận IP công khai khi máy nằm sau NAT (DNS, email, PTR), nhắc mở cổng ở tường lửa nền tảng, gỡ luật iptables REJECT có sẵn của image Ubuntu trên Oracle Cloud. Hỗ trợ Oracle Linux 9. MySQL trên Ubuntu ARM dùng gói của Ubuntu.
- Bảo mật: rà soát toàn bộ và sửa các lỗi cô lập giữa website (trình quản lý file, log, FTP, symlink, khôi phục sao lưu), quyền user Linux, quyền DNS, Redis có mật khẩu, giới hạn đăng nhập, ràng buộc người gửi email. Tự cập nhật bảo mật hệ điều hành.
- Kiểm tra cổng (Tường lửa → Kiểm tra cổng): dịch vụ đang lắng nghe, tường lửa có cho phép không, cảnh báo dịch vụ nội bộ bị mở ra ngoài, và thử kết nối qua IP công khai (đi qua tường lửa của cloud).

## 0.1.0 — MVP
- Engine riêng (Go), web không root + agent root, SQLite, Ubuntu 22.04/24.04, AlmaLinux 9, Rocky 9.
- Website PHP/tĩnh/proxy, Let's Encrypt, PHP 7.4–8.4, MariaDB, PostgreSQL, Redis, Node.js, Docker, Fail2ban.
- Apps (Pro), cron, tường lửa, quản lý file, terminal, 2FA, nhật ký, license Free/Pro/Business/Agency.
