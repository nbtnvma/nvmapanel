<p align="center"><img src=".github/logo.png" alt="NVMA" width="96" height="96"></p>

# NVMA Panel

**Control panel nhẹ cho VPS Linux** — website, database, email, ứng dụng Node/Go và game server trên một giao diện.
*A lightweight control panel for Linux servers — websites, databases, e-mail, Node/Go apps and game servers.*

[nvma.site](https://nvma.site) · [Bản quyền / License](https://license.nvma.site/) · [Báo lỗi / Issues](../../issues)

> Repository này chỉ chứa **bản cài đặt và bản phát hành**.
> *This repository only hosts the installer and release binaries.*

## Cài đặt / Install

Trên VPS hoặc máy chủ riêng **mới cài** Ubuntu 22.04 / 24.04, Debian 12 / 13, AlmaLinux 9, Rocky Linux 9 hoặc Oracle Linux 9
(amd64/arm64, KVM/Xen/VMware/Hyper-V hoặc máy thật, tối thiểu 512 MB RAM — khuyên dùng 1 GB):
*On a freshly installed VPS or dedicated server (not an OpenVZ/LXC container):*

```bash
curl -fsSL https://github.com/nbtnvma/nvmapanel/releases/latest/download/install.sh | sudo bash
```

> **Bản hiện tại: 0.3.4.** Đã chạy thật trên Ubuntu 22.04 / 24.04, Debian 12 / 13, AlmaLinux 9, Rocky Linux 9 và
> Oracle Linux 9 (amd64; Ubuntu 24.04 và Debian 13 cả arm64). Báo lỗi giúp tại [Issues](../../issues).
> *Current release: 0.3.4, run on real Ubuntu 22.04/24.04, Debian 12/13, AlmaLinux 9, Rocky Linux 9 and Oracle Linux 9
> servers (amd64; Ubuntu 24.04 and Debian 13 on arm64 too).* Cài một phiên bản cụ thể / a specific version:
> `curl -fsSL https://github.com/nbtnvma/nvmapanel/releases/download/v0.3.4/install.sh | sudo bash`
>
> Nếu nhà cung cấp có **tường lửa trong trang quản trị** (Oracle Cloud, AWS, Google Cloud, Azure luôn có; nhiều VPS có
> tuỳ chọn Cloud Firewall): mở cổng 80, 443, cổng panel (in ra khi cài) và **cổng panel + 1** (phpMyAdmin/Adminer) ở đó.
> *If your provider has a firewall in its dashboard (always on Oracle Cloud, AWS, Google Cloud, Azure), allow ports 80,
> 443, the panel port and the panel port + 1 there too.*

Cài xong, địa chỉ panel và mật khẩu được in ra màn hình và lưu ở `/root/nvma-credentials.txt`.
*The panel URL and password are printed at the end and saved to `/root/nvma-credentials.txt`.*

Tuỳ chọn / options: `--port`, `--entry`, `--admin`, `--password`, `--php 8.3|none`, `--email`, `--domain`, `--no-firewall`, `--swap`, `--upgrade` (xem `--help`).

### Kiểm tra bản phát hành / Verify a release

Mỗi bản phát hành có `nvma-panel-linux.tar.gz`, `.sha256` và chữ ký Ed25519 `.sig`. `install.sh` tự kiểm tra cả hai.

```bash
sha256sum -c nvma-panel-linux.tar.gz.sha256
```

## Nâng cấp / Upgrade

Từ 0.2.0: **Cài đặt → Cập nhật & sao lưu cấu hình → Cập nhật ngay** (gói có chữ ký, tự quay lại bản cũ nếu lỗi).
*From 0.2.0: Settings → Updates → Update now (signed package, automatic rollback).* Hoặc / or:

```bash
curl -fsSL https://github.com/nbtnvma/nvmapanel/releases/latest/download/install.sh | sudo bash -s -- --upgrade
```

Website, database và cấu hình được giữ nguyên. *Websites, databases and settings are kept.*

## Cứu hộ / Rescue (SSH, root)

| Việc | Lệnh |
|---|---|
| Xem URL panel | `nvma panel info` |
| Xem tên tài khoản | `nvma user list` (từ 0.3.1 tài khoản đầu tiên tên `nvma`, bản trước là `admin`) |
| Quên mật khẩu | `nvma user passwd nvma` |
| Mất 2FA | `nvma user 2fa-reset nvma` |
| Đổi tên đăng nhập | `nvma user rename nvma ten-moi` |
| Đổi cổng / đường dẫn | `nvma panel port 28417` · `nvma panel path reset` |
| Bị khoá IP | `nvma panel allowlist clear` |
| Khôi phục cấu hình panel | `nvma panel restore /var/backups/nvma/panel-….tar.gz` |
| Kiểm tra license | `nvma license diagnose` |

## Phiên bản / Editions

**Free** (không cần key) · **Pro** · **Business** · **Agency** — xem [license.nvma.site](https://license.nvma.site/).
Hết hạn license chỉ khoá tính năng trả phí; website và ứng dụng của bạn vẫn chạy bình thường.

## Bảo mật / Security

Báo lỗ hổng: xem [SECURITY.md](SECURITY.md). Vui lòng không mở issue công khai cho lỗ hổng bảo mật.

## Giấy phép / License

Bộ cài, script và tài liệu trong repo này: **MIT**. Phần mềm NVMA Panel (binary trong Releases): **EULA độc quyền** —
bản Free dùng miễn phí không giới hạn số máy chủ. Xem [LICENSE](LICENSE).
*Installer, scripts and docs: MIT. NVMA Panel binaries: proprietary EULA (Free edition free to use). See [LICENSE](LICENSE).*
