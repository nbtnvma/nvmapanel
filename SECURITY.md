# Security policy

## Reporting a vulnerability

Please do **not** open a public issue. Send a description, the version (`nvma version`) and the steps to reproduce through
the NVMA support channel (https://t.me/NVMAsite) or the contact page on https://nvma.site.

We answer within 72 hours and publish the fix together with a note in the [changelog](CHANGELOG.md).

## Supported versions

Only the latest release receives security fixes. Updating is one click in the panel (Panel settings → Updates).

## Design notes

- The web interface runs as an unprivileged user; every system change goes through an agent that accepts only a fixed set
  of typed operations.
- The panel listens on a random port under a secret path, with TOTP two-factor authentication, a strict Content Security
  Policy and an audit log.
- Every website runs as its own Linux user, isolated from the other websites on the server.
- Releases are signed, and the installer and the updater verify the package before installing it.
