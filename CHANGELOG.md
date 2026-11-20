# Changelog

All notable changes to NVMA Panel. Versions follow [Semantic Versioning](https://semver.org/).
To upgrade, open **Panel settings → Updates → Update now**, or run the install command with `--upgrade`.

## 0.3.4 — 2026-11-05

Lessons from moving real Laravel applications from cPanel hosting to cloud servers.

### Added
- **Laravel tab** on every PHP website:
  - checks the app: caches that still hold the old host's paths (the usual HTTP 500 after a move), missing `vendor/`,
    `.env` lines Laravel cannot read (unquoted spaces, `<…>` placeholders), empty `APP_KEY`, `public/storage` not linked or
    copied as a folder, `storage/` not writable, `APP_DEBUG` on in production;
  - checks the server side: `MAIL_MAILER=sendmail` on a server that cannot send mail, Redis in use without a password,
    `proc_open` disabled in PHP, workers started before the last `.env` change;
  - **Finish setup** button (clears the old host's caches, makes storage writable, `storage:link`, `optimize`,
    `filament:optimize`), plus optimize, optimize:clear, storage:link, migrate and key:generate, all run as the site's user;
  - the site's workers: restart them, add a Laravel queue, Horizon or Reverb worker;
  - **a Redis user per site** (reads and writes only the site's own keys, cannot flush other sites' data), with the `.env`
    lines to copy;
  - a viewer for `storage/logs`.
- After a Laravel app is extracted in the file manager, the panel points to the Laravel tab.
- Git deploys for Laravel also run `storage:link` and `filament:optimize`.
- **Uptime checks**: a health path per website (for example `/up`) is requested every 2 minutes; two failures in a row send
  a Telegram alert and a webhook (`site.down`, `site.up`).
- Worker presets are named after the website (`shop-queue`), so every Laravel site has its own queue and Reverb worker,
  with memory limits filled in (queue 256 MB, Horizon 512 MB, Reverb 256 MB) and `--timeout` for queues.
- Rescue command `nvma app list | start | stop | restart <name> | logs <name>`.
- `nvma panel login <name>` prints a one-time sign-in link, valid for 5 minutes (forgotten password, no 2FA device).
- **Sender relays per domain**: mail from each sending domain can go through that domain's own SMTP relay.
- Email page: a "next step" card after install, a warning when mail is queued while sending is off, the number of deferred
  messages with the reason, a **Retry now** button, a test message from each domain.
- Customer domains can include **every subdomain** (`*.domain`, wildcard certificate through Cloudflare DNS-01); up to 1,000
  domains per website; `GET …/domains` accepts `?q=&offset=&limit=`.
- The SSL tab checks every name the website serves: whether the certificate covers it (including `*.domain`) and whether it
  goes through Cloudflare, and suggests an Origin CA certificate for proxied names.
- **Custom nginx configuration** per website, edited on the Advanced tab (admin); the panel runs `nginx -t` before saving.
- HSTS can be turned off per website (for apps that send their own header).
- Cloudflare **account API tokens** are accepted; Cloudflare's own error message is shown.
- The overview lists websites without a backup schedule and creates a default one (daily, keep 7) in one click.
- **Adopt existing databases** (created outside the panel) into the Databases list.
- The website page shows the site ID for the API (admin); the API also accepts the domain: `api/sites/example.com`.

### Changed
- The Apps menu is now **Apps, workers & game servers**.
- The Email module installs only Postfix (enough for send-only mode); Dovecot and OpenDKIM are installed when full mode
  is chosen.
- Saving a relay tries the SMTP login at once and shows the error on the form (it can still be saved when the relay is
  temporarily unreachable); a password is required when a user name is set; passwords may contain spaces.

### Fixed
- Archives made on Windows were extracted with world-writable files (mode 666): files are now 644, folders 755, `.env` 600.
- Deleting a website with its files left `/home/<user>` behind, and the next user with the same UID inherited it.
- Deleting a website left its `schedule:run` cron job and its workers running and failing every minute.
- PHP websites: paths that look like static files but are served by Laravel (Livewire scripts, `/media/…` images,
  `sitemap.xml`) returned 404.
- The `/app` proxy path (Reverb) also caught `/apple`, `/app.js`…: it now matches only `/app` and `/app/…`.
- Websites with a sub-folder web root (`public`) showed 404 instead of the placeholder page before any code was uploaded.
- A false `postfix@-` failure reported while installing the Email module.
- Self-signed certificates now include `127.0.0.1` for programs that call the API from the same server.

## 0.3.3 — 2026-11-03

### Added
- **File manager trash**: deleted items go to the website's `.trash`, can be restored to their original place, deleted
  forever one by one or all at once, and are cleared after 30 days. Shift+Del or the right-click menu deletes at once.
- **Drag and drop to move**: drag files and folders (several at a time) onto a folder in the list, the folder tree or the
  path bar.
- **Open in its own tab**: the file manager in a separate browser tab, without the sidebar.

### Changed
- Archives with up to 200,000 entries can be extracted (was 20,000, too few for a Laravel app with `vendor/`).

### Fixed
- Importing SQL dumps made by MariaDB (cPanel hosting) into MySQL: the "sandbox mode" line of recent `mariadb-dump`
  versions is skipped and MariaDB-only `uca1400` collations become `utf8mb4_unicode_ci` (data in INSERT lines is untouched).
- Error messages with a number were shown as `%!d(float64=20000)`.

## 0.3.2 — 2026-11-02

### Fixed
- SSL is issued right after a DNS record is created: the panel asks the domain's own nameservers (for example Cloudflare)
  instead of the server provider's resolver. Some cloud resolvers remember "no such domain" for up to 30 minutes, which
  delayed certificates although the record was correct. Applies to websites, customer domains and the panel hostname check.
- Domains proxied by Cloudflare on servers without IPv6: the check before issuing tries IPv4 first and then every address,
  instead of only the first one.
- A clear message when a domain **has no DNS record yet**, separate from a record pointing to another server, shown in the
  account's language on the SSL tab and the customer domains card. The panel still retries every 15 minutes for 48 hours.

## 0.3.1 — 2026-11-01

### Security
- `trust_proxy` only trusts `X-Forwarded-For` when the proxy also sends the `proxy_secret` (header `X-NVMA-Proxy`);
  otherwise the header is ignored and a warning is logged. Before, a program running on the server itself could choose its
  own IP when `trust_proxy` had been turned on by hand.
- The DNS self-check before issuing SSL no longer connects to internal addresses (loopback, private networks, link-local)
  and no longer returns response details to the user.
- Admins can list and revoke the API tokens of every account (Accounts → API tokens of all accounts).

### Added
- **Rename a sign-in name** (Accounts → rename icon, or `nvma user rename <name> <new-name>` over SSH). Password, 2FA, API
  tokens, websites and databases are kept; open sessions stay signed in.

### Changed
- The first admin account of a new install is named **`nvma`** (it was `admin`; existing installs keep their name). Choose
  another name at install time with `--admin <name>`; list account names with `nvma user list`.

### Fixed
- Oracle Linux 9 on 1 GB servers: the installer disables the `ol9_oci_included` and `ol9_ksplice` repositories below 2 GB of
  RAM (their metadata made `dnf` run out of memory and hang the server) and disables kdump below 2.5 GB (the image reserves
  448 MB for it). The memory comes back after the next reboot. Re-enable a repository with
  `dnf config-manager --set-enabled <name>`.

## 0.3.0 — 2026-10-29

### Added
- **Debian 12 and 13** (amd64, arm64): several PHP versions from packages.sury.org with automatic security updates, official
  MySQL on amd64 (MariaDB on Debian ARM), Docker from the official repository, Dovecot 2.4 on Debian 13.
- **API tokens** (Account → API tokens): read or read/write, expiry date, IP limit, shown once; creating one asks for the
  password and 2FA again. They work for websites, databases, backups, apps, cron, jobs and customer domains; the Terminal,
  file manager, accounts and settings still need a sign-in.
- **Webhooks** (Pro, Monitoring → Alerts): HMAC-SHA256-signed JSON events for backups, deploys, failed services, expiring
  SSL, high CPU/RAM/disk, websites created or deleted, malware and new panel versions, with retries.
- **Customer domains** (Website → Overview, or the API): attach hundreds of customer domains to one website, each with its
  own Let's Encrypt certificate that waits for DNS for 48 hours and renews itself. A website can also serve **every
  subdomain** `*.domain` (admin) for multi-tenant apps.
- **Malware scan** (ClamAV module, Settings → Malware scan): nightly at 3:30 and on demand, lowest priority, never follows
  links; one-click quarantine; alerts through Telegram and webhooks.
- **White-label** (Business, Settings → Branding): name, tagline, logo, favicon and colour; the version number can be hidden.

### Changed
- The installer stops early with a clear reason on OpenVZ/LXC containers, with less than 2 GB of free disk, when ports
  80/443 are already in use, or when another panel is installed (CloudPanel, FastPanel, ISPConfig, Webmin and VestaCP are
  now detected too); it warns below 1 GB of RAM.
- The end of the install and the Firewall page remind you to open ports in the dashboard firewall of **any** provider.
  Alibaba Cloud and Tencent Cloud are recognised.

### Security
- A website cannot claim the hostname of the panel, the mail server or webmail (as a customer domain, alias or staging
  site); regular accounts cannot take subdomains of those, or customer domains attached to someone else's website.
- API tokens cannot create cron jobs that run as root; changing a password revokes all of the account's API tokens.
- Malware quarantine scans the exact file again before moving it, so a crafted file name cannot make the panel move another
  account's file.
- Webhook delivery errors no longer reveal internal connection details; logs keep only the host of a webhook URL.
- The MySQL repository key is pinned by SHA-256.

### Fixed
- **Full email mode** did not start on Ubuntu, Debian 12 and AlmaLinux/Rocky/Oracle Linux 9 (Dovecot 2.3 reported
  `Garbage after '{'`); on Ubuntu/Debian, OpenDKIM could not read the DKIM key, so outgoing mail was refused
  (`4.7.1 Service unavailable`). After upgrading, save the email settings once to apply the fix.
- **Installing MySQL on amd64** (Ubuntu, Debian, AlmaLinux/Rocky) failed because the repo.mysql.com signing key expired on
  2025-10-22; servers that already used that repository also had a broken `apt update`. The panel replaces the key on upgrade.
- Installing PHP on Debian could leave the php-fpm package failed (too many restarts) and report missing extensions.
- **AlmaLinux/Rocky/Oracle Linux 9**: the MySQL install stopped while switching root to socket login; MySQL's
  `validate_password` blocked database creation and phpMyAdmin; SELinux stopped Dovecot from writing mail to `/var/vmail`;
  `sudo nvma …` could not find the command.
- Deleting a website's last backup also removes its empty folder.
- The Webmail install no longer writes into a data folder that already belongs to the webmail user.
- The first phpMyAdmin install on a new server failed with an `nvmatools` group error.
- An interrupted install continues when run again instead of reporting "already installed"; the credentials are saved as
  soon as the admin account exists. `nvma setup` and `nvma module` no longer create locked folders under umask 077.

## 0.2.0 — 2026-10-26

The first stable release of the 0.2 line, run on a real Ubuntu 24.04 ARM server with Laravel websites, MySQL, Redis,
workers and Reverb, SSL, Cloudflare, SFTP/FTPS and backups.

### Added
- **phpMyAdmin opens signed in** and shows only the account's databases. It runs on **its own port (panel port + 1)** so a
  flaw in the tool cannot reach the panel; open that port in your provider's firewall too.
- **cPanel-style file manager**: folder tree, multi-select, copy/move, zip, extract .zip/.tar/.tar.gz, filter, sort,
  right-click menu, keyboard shortcuts.
- **Advanced PHP**: extra php.ini lines per version, install/enable/disable extensions, per-site `disable_functions` (admin).
- **One-click update** (Settings → Updates): verifies the signed release, backs up the configuration first and goes back to
  the previous version if the new one fails.
- **Daily panel configuration backups** (`/var/backups/nvma`), restored with `nvma panel restore`.
- **Encrypted cloud backups** (rclone crypt); SFTP targets use the panel's own SSH key (its public key is shown on the form).
- **Email**: full mode can send through a relay when the provider blocks port 25; per-provider instructions to get port 25
  opened (AWS, Oracle, Google, Azure, DigitalOcean, Hetzner, Vultr, Linode).

### Security
- The Terminal asks for the password each time it is opened (it can be disabled entirely); sessions last at most 7 days and
  can be listed and signed out one by one; a 2FA code cannot be used twice; **fail2ban is on by default** (SSH, panel login,
  mail, FTP); a site's `mail()` cannot forge another domain; every site has its own temporary folder; Docker ports follow
  the panel firewall; MySQL grants no longer match another account's database by accident (the `_` wildcard).
- Review before the stable release: webmail runs in its own PHP pool (it cannot read phpMyAdmin sessions); phpMyAdmin's
  MySQL user cannot be pre-created by another account; changing an account's role signs out all its sessions; viewing the
  backup encryption key also needs the 2FA code; panel configuration backup and restore never follow links.

### Fixed
- Websites proxied by Cloudflare returned error 525.
- Turning on "Issue SSL automatically" for an existing website did not issue a certificate.
- `npm` did not run for site users; cron jobs written as `cd … && php artisan` used the wrong PHP version.
- Setting a panel hostname that is proxied by Cloudflare locked you out: there is now a warning and a way back.
- Pre-release versions are compared correctly (beta.3 > beta.2).
- The Webmail install failed with "no SnappyMail tarball" when the newest SnappyMail release had no package attached.

## 0.2.0-beta.2 — 2026-10-23

### Added
- Cloud servers (Oracle Cloud, AWS, Google Cloud, Azure): the panel finds the public IP behind NAT (DNS, email, PTR),
  lists the ports to open in the platform firewall and removes the catch-all iptables REJECT rules of Oracle's Ubuntu
  images. Oracle Linux 9 is supported. MySQL on Ubuntu ARM uses Ubuntu's packages.
- **Port check** (Firewall → Port check): listening services, whether the firewall allows them, a warning when an internal
  service is exposed, and a connection test through the public IP (through the cloud firewall).

### Security
- Full review: stricter isolation between websites (file manager, logs, FTP, symlinks, backup restore), Linux user and DNS
  permissions, a Redis password, login rate limiting, mailboxes that send only as themselves, automatic OS security updates.

## 0.2.0-beta.1 — 2026-10-22

### Added
- **Laravel websites**: custom web root (`public/`), a Laravel template (scheduler added for you), PHP CLI and Composer per
  site.
- **Automatic SSL** on site creation (waits for DNS and retries for 48 hours) with Let's Encrypt, ZeroSSL, Google or a custom
  ACME provider, wildcards through Cloudflare, Cloudflare Origin CA and CSRs for commercial certificates.
- Per-domain PHP settings (memory, upload size, execution time…); site rules (redirects, password-protected folders, IP
  blocking, error pages, hotlink protection).
- MySQL 8.4 LTS (or MariaDB), phpMyAdmin, Adminer, large SQL imports (.sql/.sql.gz).
- Chrooted SFTP with SSH keys; FTP/FTPS accounts (Pure-FTPd).
- Email: send-only through SES/Resend/Brevo, or a full mail server (Postfix, Dovecot, DKIM, forwarders, autoreply) with
  SnappyMail webmail.
- Zone editor for Cloudflare DNS and PowerDNS: an A record with each website, one-click mail records.
- Scheduled local backups of websites and databases, restore and download; 24-hour resource charts; Cloudflare real IP.
- **Pro — Game & Realtime**: one-click Laravel realtime (PHP, MySQL, Redis, Horizon/queue, Reverb) and Node/Go realtime
  (Node, Redis, PostgreSQL/PostGIS, PgBouncer) stacks; multi-process workers, path-based WebSocket proxy, PHP-FPM
  performance modes, MySQL/Redis/Nginx tuning, request rate limiting; service monitoring, Telegram alerts, zero-downtime
  Git deploys with a webhook, staging clones, backups to S3/R2/B2/SFTP.
- One-line install from GitHub Releases with signed packages.

## 0.1.0 — 2026-10-21

- Own engine written in Go: a non-root web process and a root agent, SQLite; Ubuntu 22.04/24.04, AlmaLinux 9, Rocky Linux 9.
- PHP, static and proxy websites, Let's Encrypt, PHP 7.4–8.4, MariaDB, PostgreSQL, Redis, Node.js, Docker, Fail2ban.
- Apps (Pro), cron, firewall, file manager, terminal, 2FA, audit log; Free, Pro, Business and Agency plans.
