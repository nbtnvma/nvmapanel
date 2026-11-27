<p align="center"><img src="https://app.nvma.site/controlpanel/assets/icon-192.png" alt="NVMA Panel" width="96" height="96"></p>

# NVMA Panel

**The lightweight control panel for your VPS/Server.** Websites, databases, email, Node/Go apps and game servers from one
clean interface.

[Website](https://app.nvma.site/controlpanel/) · [Guide](https://app.nvma.site/controlpanel/docs.php) · [Plans](https://license.nvma.site/) · [Changelog](CHANGELOG.md) · [Issues](../../issues) · [Tiếng Việt](#tiếng-việt)

> This repository hosts the installer, the release packages and the issue tracker.

## Install

On a **freshly installed** VPS, cloud server or dedicated server running Ubuntu 22.04 / 24.04, Debian 12 / 13, AlmaLinux 9,
Rocky Linux 9 or Oracle Linux 9 (amd64 or arm64; KVM, Xen, VMware, Hyper-V or bare metal; 512 MB of RAM, 1 GB recommended;
not an OpenVZ/LXC container):

```bash
curl -fsSL https://github.com/nbtnvma/nvmapanel/releases/latest/download/install.sh | sudo bash
```

The panel address and the admin password are printed at the end and saved to `/root/nvma-credentials.txt`.

- **Current release: 0.4.0.** A specific version:
  `curl -fsSL https://github.com/nbtnvma/nvmapanel/releases/download/v0.4.0/install.sh | sudo bash`
- If your provider has a **firewall in its dashboard** (always on Oracle Cloud, AWS, Google Cloud and Azure; optional at
  many VPS providers), allow ports 80, 443, the panel port (printed by the installer) and **the panel port + 1**
  (phpMyAdmin/Adminer) there too.
- Options: `--port`, `--entry`, `--admin`, `--password`, `--php 8.4|none`, `--email`, `--domain`, `--no-firewall`, `--swap`,
  `--upgrade` (see `--help`).

Every release ships `nvma-panel-linux.tar.gz` with a `.sha256` checksum and a `.sig` signature; `install.sh` verifies both
before installing. To check a download yourself:

```bash
sha256sum -c nvma-panel-linux.tar.gz.sha256
```

## Upgrade

In the panel: **Panel settings → Updates → Update now**. The package is verified, the configuration is backed up first and
the panel goes back to the previous version if the new one fails. Or over SSH:

```bash
curl -fsSL https://github.com/nbtnvma/nvmapanel/releases/latest/download/install.sh | sudo bash -s -- --upgrade
```

Websites, databases and settings are kept.

## Rescue commands (SSH, as root)

| Task | Command |
|---|---|
| Show the panel address | `nvma panel info` |
| List account names | `nvma user list` (the first account is `nvma` since 0.3.1, `admin` on older installs) |
| Sign in without the password | `nvma panel login nvma` (one-time link, valid for 5 minutes) |
| Reset a password | `nvma user passwd nvma` |
| Turn off 2FA | `nvma user 2fa-reset nvma` |
| Turn off Sign in with NVMA ID | `nvma panel nvmaid off` (state: `nvma panel nvmaid`) |
| Rename a sign-in name | `nvma user rename nvma new-name` |
| Change the port / secret path | `nvma panel port 28417` · `nvma panel path reset` |
| Clear the IP allowlist | `nvma panel allowlist clear` |
| Restore the panel configuration | `nvma panel restore /var/backups/nvma/panel-….tar.gz` |
| Restart or inspect a worker | `nvma app list` · `nvma app restart <name>` · `nvma app logs <name>` |
| Run a command as a site's user | `nvma site exec example.com -- php artisan migrate --force` |

## Editions

**Free** (no key needed, unlimited websites) · **Pro** · **Business** · **Agency**. Compare and buy at
[license.nvma.site](https://license.nvma.site/). If a paid plan expires, your websites and apps keep running.

## Security

To report a vulnerability, see [SECURITY.md](SECURITY.md). Please do not open a public issue for security problems.

## License

Installer, scripts and documentation in this repository: **MIT**. The NVMA Panel software (binaries in Releases):
**proprietary EULA**; the Free edition is free to use on any number of servers. See [LICENSE](LICENSE).

---

## Tiếng Việt

**Control panel nhẹ cho VPS/Server của bạn.** Website, database, email, ứng dụng Node/Go và game server trên một giao diện
gọn gàng.

[Trang sản phẩm](https://app.nvma.site/controlpanel/) · [Hướng dẫn](https://app.nvma.site/controlpanel/docs.php?lang=vi) · [Bảng giá](https://license.nvma.site/) · [Nhật ký thay đổi](CHANGELOG.md) · [Báo lỗi](../../issues)

> Repository này chứa trình cài, các bản phát hành và nơi báo lỗi.

### Cài đặt

Trên VPS, cloud server hoặc máy chủ riêng **mới cài** Ubuntu 22.04 / 24.04, Debian 12 / 13, AlmaLinux 9, Rocky Linux 9 hoặc
Oracle Linux 9 (amd64 hoặc arm64; KVM, Xen, VMware, Hyper-V hoặc máy thật; RAM 512 MB, khuyên dùng 1 GB; không chạy trong
container OpenVZ/LXC):

```bash
curl -fsSL https://github.com/nbtnvma/nvmapanel/releases/latest/download/install.sh | sudo bash
```

Cài xong, địa chỉ panel và mật khẩu admin được in ra màn hình và lưu ở `/root/nvma-credentials.txt`.

- **Bản hiện tại: 0.4.0.** Cài một phiên bản cụ thể:
  `curl -fsSL https://github.com/nbtnvma/nvmapanel/releases/download/v0.4.0/install.sh | sudo bash`
- Nếu nhà cung cấp có **tường lửa trong trang quản trị** (Oracle Cloud, AWS, Google Cloud, Azure luôn có; nhiều nhà cung cấp
  VPS có tuỳ chọn này), mở cả ở đó các cổng 80, 443, cổng panel (trình cài in ra) và **cổng panel + 1** (phpMyAdmin/Adminer).
- Tuỳ chọn: `--port`, `--entry`, `--admin`, `--password`, `--php 8.4|none`, `--email`, `--domain`, `--no-firewall`, `--swap`,
  `--upgrade` (xem `--help`).

Mỗi bản phát hành có `nvma-panel-linux.tar.gz` kèm mã `.sha256` và chữ ký `.sig`; `install.sh` kiểm tra cả hai trước khi
cài. Tự kiểm tra file đã tải:

```bash
sha256sum -c nvma-panel-linux.tar.gz.sha256
```

### Nâng cấp

Trong panel: **Cài đặt panel → Cập nhật → Cập nhật ngay**. Gói được kiểm tra, cấu hình được sao lưu trước, và panel tự quay
về bản cũ nếu bản mới lỗi. Hoặc qua SSH:

```bash
curl -fsSL https://github.com/nbtnvma/nvmapanel/releases/latest/download/install.sh | sudo bash -s -- --upgrade
```

Website, database và cấu hình được giữ nguyên.

### Lệnh cứu hộ (SSH, quyền root)

| Việc | Lệnh |
|---|---|
| Xem địa chỉ panel | `nvma panel info` |
| Xem tên tài khoản | `nvma user list` (từ 0.3.1 tài khoản đầu tiên tên `nvma`, bản cài cũ hơn là `admin`) |
| Đăng nhập không cần mật khẩu | `nvma panel login nvma` (link dùng một lần, hạn 5 phút) |
| Đặt lại mật khẩu | `nvma user passwd nvma` |
| Tắt 2FA | `nvma user 2fa-reset nvma` |
| Tắt đăng nhập bằng NVMA ID | `nvma panel nvmaid off` (xem trạng thái: `nvma panel nvmaid`) |
| Đổi tên đăng nhập | `nvma user rename nvma ten-moi` |
| Đổi cổng / đường dẫn bí mật | `nvma panel port 28417` · `nvma panel path reset` |
| Xoá danh sách IP được phép | `nvma panel allowlist clear` |
| Khôi phục cấu hình panel | `nvma panel restore /var/backups/nvma/panel-….tar.gz` |
| Khởi động lại hoặc xem worker | `nvma app list` · `nvma app restart <tên>` · `nvma app logs <tên>` |
| Chạy lệnh bằng user của website | `nvma site exec example.com -- php artisan migrate --force` |

### Phiên bản

**Free** (không cần key, không giới hạn website) · **Pro** · **Business** · **Agency**. So sánh và mua tại
[license.nvma.site](https://license.nvma.site/). Gói trả phí hết hạn thì website và ứng dụng của bạn vẫn chạy bình thường.

### Bảo mật

Báo lỗ hổng: xem [SECURITY.md](SECURITY.md). Vui lòng không mở issue công khai cho lỗi bảo mật.

### Giấy phép

Trình cài, script và tài liệu trong repo này: **MIT**. Phần mềm NVMA Panel (binary trong Releases): **EULA độc quyền**; bản
Free dùng miễn phí trên số máy chủ không giới hạn. Xem [LICENSE](LICENSE).
