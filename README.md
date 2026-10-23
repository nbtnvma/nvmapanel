<p align="center"><img src=".github/logo.png" alt="NVMA" width="96" height="96"></p>

# NVMA Panel

**Control panel nhẹ cho VPS Linux** — website, database, email, ứng dụng Node/Go và game server trên một giao diện.
*A lightweight control panel for Linux servers — websites, databases, e-mail, Node/Go apps and game servers.*

[nvma.site](https://nvma.site) · [Bản quyền / License](https://license.nvma.site/products/nvma-panel) · [Báo lỗi / Issues](../../issues)

> Repository này chỉ chứa **bản cài đặt và bản phát hành** (giống cách Laragon phát hành). Mã nguồn không công khai.
> *This repository only hosts the installer and release binaries. The source code is private.*

## Cài đặt / Install

Trên VPS **mới cài** Ubuntu 22.04 / 24.04, AlmaLinux 9, Rocky Linux 9 hoặc Oracle Linux 9 (amd64/arm64, tối thiểu 512 MB RAM):

```bash
curl -fsSL https://github.com/nbtnvma/nvmapanel/releases/latest/download/install.sh | sudo bash
```

> **Bản hiện tại: 0.2.0-beta.2 (thử nghiệm).** Chưa được kiểm thử rộng trên máy chủ thật — hãy dùng VPS thử nghiệm.
> *Current release: 0.2.0-beta.2 (beta) — please use a test server.* Cài một phiên bản cụ thể / a specific version:
> `curl -fsSL https://github.com/nbtnvma/nvmapanel/releases/download/v0.2.0-beta.2/install.sh | sudo bash`
>
> Trên **Oracle Cloud, AWS, Google Cloud, Azure**: mở thêm cổng 80, 443 và cổng panel ở tường lửa của nhà cung cấp
> (Security List / Security Group…). *On clouds, also open ports 80, 443 and the panel port in the provider firewall.*

Cài xong, địa chỉ panel và mật khẩu được in ra màn hình và lưu ở `/root/nvma-credentials.txt`.
*The panel URL and password are printed at the end and saved to `/root/nvma-credentials.txt`.*

Tuỳ chọn / options: `--port`, `--entry`, `--admin`, `--password`, `--php 8.3|none`, `--email`, `--domain`, `--no-firewall`, `--swap`, `--upgrade` (xem `--help`).

### Kiểm tra bản phát hành / Verify a release

Mỗi bản phát hành có `nvma-panel-linux.tar.gz`, `.sha256` và chữ ký Ed25519 `.sig`. `install.sh` tự kiểm tra cả hai.

```bash
sha256sum -c nvma-panel-linux.tar.gz.sha256
```

## Nâng cấp / Upgrade

```bash
curl -fsSL https://github.com/nbtnvma/nvmapanel/releases/latest/download/install.sh | sudo bash -s -- --upgrade
```

Website, database và cấu hình được giữ nguyên. *Websites, databases and settings are kept.*

## Cứu hộ / Rescue (SSH, root)

| Việc | Lệnh |
|---|---|
| Xem URL panel | `nvma panel info` |
| Quên mật khẩu | `nvma user passwd admin` |
| Mất 2FA | `nvma user 2fa-reset admin` |
| Đổi cổng / đường dẫn | `nvma panel port 28417` · `nvma panel path reset` |
| Bị khoá IP | `nvma panel allowlist clear` |

## Phiên bản / Editions

**Free** (không cần key) · **Pro** · **Business** · **Agency** — xem [license.nvma.site](https://license.nvma.site/products/nvma-panel).
Hết hạn license chỉ khoá tính năng trả phí; website và ứng dụng của bạn vẫn chạy bình thường.

## Bảo mật / Security

Báo lỗ hổng: xem [SECURITY.md](SECURITY.md). Vui lòng không mở issue công khai cho lỗ hổng bảo mật.

## Giấy phép / License

Bộ cài, script và tài liệu trong repo này: **MIT**. Phần mềm NVMA Panel (binary trong Releases): **EULA độc quyền** —
bản Free dùng miễn phí không giới hạn số máy chủ. Xem [LICENSE](LICENSE).
*Installer, scripts and docs: MIT. NVMA Panel binaries: proprietary EULA (Free edition free to use). See [LICENSE](LICENSE).*
