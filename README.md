# FreeBSD IP Blocklists Blocker

## Purpose
Downloads multiple external IP blocklists, merges them, sorts them, and loads them into persistent PF tables for automatic blocking of malicious sources.  
It dynamically adjusts `net.pf.request_maxcount` based on the amount of IPs inside of blocklist file.

---

## Usage
```sh
sudo fbsd-ipbl-blocker {daemon|update|list|restore}
```

## Options
- daemon → Run continuously, updating blocklists every 24 hours.
- update → Perform a one-time update of blocklists and reload PF.
- list → Show the current blocklist and count of IPs.
- restore → Restore the most recent pf.conf backup.

---

## Configuration Variables
You can override these defaults by editing the script:

| Variable        | Default                  | Description                                      |
|-----------------|--------------------------|--------------------------------------------------|
| PF_TABLE        | blocked_ips              | Base name of PF tables                           |
| PFCONF          | /etc/pf.conf             | PF configuration file                            |
| BACKUP_DIR      | /etc                     | Directory for pf.conf backups                    |
| BLOCKLIST       | /etc/pf.blocked_ips      | Path to merged blocklist file                    |
| TMPFILE         | /tmp/pf.blocked_ips.tmp  | Temporary file used during updates               |
| LASTFILE        | /tmp/pf.blocked_ips.last | Snapshot of last blocklist to detect changes     |
| SYSCTL_CONF     | /etc/sysctl.conf         | System configuration file for persistence        |
| URLS            | (list of IP feeds)       | External IP blocklist sources                    |

---

## Features
- Downloads multiple IP blocklists from trusted sources.
- Extracts valid IPv4 addresses and sorts them numerically.
- Ensures PF configuration contains persistent table definitions.
- Dynamically adjusts net.pf.request_maxcount via sysctl with margin.
- Updates /etc/sysctl.conf to persist net.pf.request_maxcount across reboots.
- Reloads PF and replaces table contents atomically.
- Provides daemon mode for daily refresh.
- Backup and restore functionality for pf.conf.

---

## Requirements
- FreeBSD system with root privileges.
- PF enabled and configured.
- Utilities: bash, fetch, grep, sort, uniq, wc, sed.

---

## Example
Run in daemon mode:
```sh
sudo fbsd-ipbl-blocker daemon
```

Perform a one-time update:
```sh
sudo fbsd-ipbl-blocker update
```

List blocked IPs:
```sh
sudo fbsd-ipbl-blocker list
```

Restore last pf.conf backup:
```sh
sudo fbsd-ipbl-blocker restore
```

---

## Installation
Clone the repository and run the installer:
```sh
git clone https://github.com/geobarrod/FBSD-IPBL-Blocker.git
cd fbsd-ipbl-blocker
sudo make install
```

---

## Uninstallation
To remove the fbsd-ipbl-blocker and service:
```sh
cd fbsd-ipbl-blocker
sudo make uninstall
```

---

## Changelog

### v1.0 — 2026-01-03
- Initial release of fbsd-ipbl-blocker.
- Features:
  - Dynamic adjustment of net.pf.request_maxcount.
  - Backup and restore of pf.conf.
  - Daemon mode for daily refresh.
  - rc.d service integration (ipbl_blocker).
