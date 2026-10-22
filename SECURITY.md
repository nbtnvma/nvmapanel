# Chính sách bảo mật / Security policy

## Báo cáo lỗ hổng / Reporting a vulnerability

Vui lòng **không** mở issue công khai. Gửi mô tả, phiên bản (`nvma version`) và cách tái hiện tới
kênh hỗ trợ của NVMA (https://t.me/NVMAsite) hoặc qua trang liên hệ trên https://nvma.site.
Chúng tôi phản hồi trong 72 giờ và công bố bản vá cùng ghi chú trong CHANGELOG.

*Please do not open a public issue. Send a description, the version (`nvma version`) and steps to
reproduce through the NVMA support channel (https://t.me/NVMAsite) or the contact page on https://nvma.site.
We answer within 72 hours and publish the fix with a CHANGELOG note.*

## Phiên bản được hỗ trợ / Supported versions

Chỉ bản phát hành mới nhất nhận bản vá bảo mật. *Only the latest release receives security fixes.*

## Thiết kế / Design notes

- Giao diện web chạy bằng user không có quyền root; mọi thay đổi hệ thống đi qua một agent nhận thao tác có kiểu.
- Panel nghe trên cổng ngẫu nhiên kèm đường dẫn bí mật, có 2FA TOTP, CSP chặt và nhật ký thao tác.
- Bản phát hành được ký Ed25519 và kiểm tra SHA-256 khi cài.
