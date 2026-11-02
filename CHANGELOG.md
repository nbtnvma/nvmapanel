# Changelog

## 0.3.2 — 2026-11-02

Nâng cấp: **Cài đặt → Cập nhật → Cập nhật ngay**.

**Sửa**
- Cấp SSL ngay sau khi vừa tạo bản ghi DNS: panel hỏi thẳng máy chủ DNS của tên miền (ví dụ Cloudflare) thay vì máy chủ DNS
  của nhà cung cấp VPS. Trước đây máy chủ DNS của cloud (Oracle Cloud…) nhớ kết quả "không có tên miền" tới 30 phút nên
  SSL phải chờ, dù bản ghi đã đúng. Áp dụng cho website, tên miền của khách và nút kiểm tên miền panel.
- Tên miền bật proxy Cloudflare trên máy không có IPv6: bước kiểm trước khi cấp SSL thử địa chỉ IPv4 trước rồi lần lượt từng
  địa chỉ, thay vì chỉ địa chỉ đầu tiên.
- Thông báo rõ khi tên miền **chưa có bản ghi DNS** ("Chưa thấy bản ghi DNS của …; nếu vừa tạo, đợi vài phút"), tách khỏi
  trường hợp bản ghi trỏ sang máy khác. Hiện bằng ngôn ngữ của tài khoản trong tab SSL và thẻ Tên miền của khách; panel
  vẫn tự thử lại mỗi 15 phút trong 48 giờ.

## 0.3.1 — 2026-11-01

Nâng cấp: **Cài đặt → Cập nhật → Cập nhật ngay**.

**Bảo mật**
- `trust_proxy` chỉ tin `X-Forwarded-For` khi proxy gửi kèm `proxy_secret` (header `X-NVMA-Proxy`); thiếu thì bỏ qua và ghi cảnh báo.
  Trước đây một chương trình chạy trên chính máy chủ có thể tự chọn IP khi `trust_proxy` được bật tay.
- Bước tự kiểm DNS trước khi cấp SSL không còn gọi tới địa chỉ nội bộ (loopback, mạng riêng, link-local) và không trả
  chi tiết phản hồi về cho người dùng.
- Admin xem và thu hồi được API token của mọi tài khoản (Tài khoản → API token của mọi tài khoản).

**Mới**
- **Đổi tên đăng nhập** (Tài khoản → biểu tượng đổi tên, hoặc `nvma user rename <tên> <tên-mới>` qua SSH). Mật khẩu, 2FA,
  API token, website và database giữ nguyên; phiên đang mở không bị đăng xuất.
- Tài khoản quản trị đầu tiên của bản cài mới tên **`nvma`** (trước đây `admin`; máy đã cài giữ nguyên tên). Đặt tên khác
  khi cài: `--admin <tên>`. Xem tên tài khoản: `nvma user list`.

**Sửa**
- Oracle Linux 9 trên máy 1 GB (OCI Always Free E2.1.Micro): trình cài tắt kho `ol9_oci_included` và `ol9_ksplice` khi máy
  dưới 2 GB RAM (metadata khoảng 190 MB làm `dnf` hết bộ nhớ, máy treo), và tắt kdump khi máy dưới 2,5 GB RAM (image giữ
  448 MB cho kdump, máy 1 GB chỉ còn 498 MB). RAM trở lại sau lần khởi động lại tiếp theo. Bật lại kho:
  `dnf config-manager --set-enabled <tên kho>`.

## 0.3.0 — 2026-10-29

Nâng cấp từ 0.2.0: **Cài đặt → Cập nhật → Cập nhật ngay**.

**Mọi VPS, không chỉ cloud lớn**
- **Debian 12 và 13** (amd64, arm64). PHP nhiều phiên bản từ packages.sury.org, tự cập nhật bảo mật; MySQL chính thức trên
  amd64 (Debian ARM chỉ có MariaDB vì Debian và Oracle đều không có gói MySQL cho ARM); Docker từ kho chính thức; Dovecot 2.4
  của Debian 13 được cấu hình theo cú pháp mới.
- Trình cài dừng sớm và nói rõ lý do khi máy là container OpenVZ/LXC, ổ trống dưới 2 GB, cổng 80/443 đang bị chương trình
  khác dùng, hoặc đã có panel khác (thêm CloudPanel, FastPanel, ISPConfig, Webmin, VestaCP); nhắc khi RAM dưới 1 GB.
- Cuối quá trình cài và trang Tường lửa nhắc mở cổng ở tường lửa trong trang quản trị của **mọi** nhà cung cấp. Nhận diện
  thêm Alibaba Cloud, Tencent Cloud.

**Mới**
- **API token** (Tài khoản → API token): quyền đọc hoặc đọc/ghi, hạn dùng, giới hạn IP, chỉ hiện một lần, tạo token cần nhập
  lại mật khẩu + 2FA. Dùng được cho website, database, sao lưu, ứng dụng, cron, tác vụ, tên miền của khách; Terminal, quản lý
  file, tài khoản, cài đặt và license vẫn cần đăng nhập.
- **Webhook** (Pro, Giám sát → Cảnh báo): sự kiện JSON ký HMAC-SHA256 cho sao lưu, deploy, dịch vụ lỗi, SSL sắp hết hạn,
  CPU/RAM/ổ đĩa cao, website tạo/xoá, mã độc, bản panel mới; tự gửi lại khi lỗi.
- **Tên miền của khách** (Website → Tổng quan, hoặc qua API): gắn hàng trăm tên miền riêng vào một website, mỗi tên một chứng
  chỉ Let's Encrypt tự cấp, chờ DNS 48 giờ, tự gia hạn. **Website nhận mọi tên miền con** `*.domain` (admin) cho ứng dụng
  nhiều khách.
- **Quét mã độc** (module ClamAV, Cài đặt → Quét mã độc): mỗi đêm lúc 3:30 và theo yêu cầu, ưu tiên thấp nhất, không đi theo
  link; cách ly file một cú nhấp; báo qua Telegram và webhook.
- **White-label** (Business, Cài đặt → Thương hiệu riêng): tên, dòng giới thiệu, logo, favicon, màu; ẩn số phiên bản.

**Bảo mật** (rà soát toàn bộ phần mới trước khi phát hành)
- Website không thể nhận tên miền của panel, máy chủ mail hay webmail (kể cả khi gắn làm tên miền của khách, alias, staging);
  tài khoản thường không lấy được tên miền con của chúng hay tên miền khách đã gắn vào website của người khác.
- API token không tạo được cron chạy bằng root; đổi mật khẩu thì mọi API token của tài khoản bị thu hồi.
- Cách ly mã độc quét lại đúng file trước khi di chuyển, nên tên file giả mạo không thể khiến panel di chuyển file của người khác.
- Lỗi gửi webhook không còn lộ chi tiết kết nối nội bộ; log chỉ giữ tên máy của URL webhook.
- Khoá repo MySQL được ghim bằng SHA-256.

**Sửa lỗi**
- **Email chế độ đầy đủ** không khởi động được trên Ubuntu, Debian 12 và AlmaLinux/Rocky/Oracle Linux 9 (Dovecot 2.3 báo
  `Garbage after '{'`); trên Ubuntu/Debian, OpenDKIM còn không đọc được khoá DKIM nên mọi thư gửi đi bị từ chối
  (`4.7.1 Service unavailable`). Sau khi nâng cấp, lưu lại cài đặt email một lần để áp dụng.
- **Cài MySQL trên máy amd64** (Ubuntu, Debian, AlmaLinux/Rocky) lỗi vì khoá ký của repo.mysql.com hết hạn ngày 22/10/2025;
  máy đã cài MySQL từ repo này còn bị hỏng `apt update` (và cập nhật bảo mật tự động). Panel tự thay khoá mới khi nâng cấp.
- Cài PHP trên Debian có thể làm gói php-fpm lỗi (khởi động lại quá nhiều lần) và báo nhầm thiếu extension.
- **AlmaLinux/Rocky/Oracle Linux 9**: cài MySQL dừng ở bước chuyển root sang đăng nhập qua socket; `validate_password` của
  MySQL chặn tạo database và phpMyAdmin; SELinux chặn Dovecot ghi thư vào `/var/vmail`; `sudo nvma …` không tìm thấy lệnh.
- Xoá bản sao lưu cuối cùng của một site thì xoá luôn thư mục rỗng của site đó.
- Cài Webmail không còn ghi vào thư mục dữ liệu đã thuộc user webmail.
- Cài phpMyAdmin lần đầu trên máy mới báo lỗi nhóm `nvmatools`.
- Trình cài bị dừng giữa chừng: chạy lại sẽ cài tiếp thay vì báo "đã cài"; thông tin đăng nhập được lưu ngay khi tạo
  tài khoản admin. Lệnh `nvma setup`/`nvma module` không còn tạo thư mục khoá kín khi shell root đặt umask 077.

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
