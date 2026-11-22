
<p align="center">
  <img width="2000" height="1342" alt="image" src="https://github.com/user-attachments/assets/6e47d054-8e34-4740-944d-0901b7f000f0" />

</p>

<p align="center">
  <b>A lightweight, Windows Server–style Linux for database servers.</b><br>
  Choose Microsoft SQL Server, MySQL and/or PostgreSQL during installation.
</p>

<p align="center">
  Developed by <b>Nilesh Mishra</b> · Based on Ubuntu 24.04 LTS · GPL-3.0
</p>

---

> **Status: early preview (v0.3).** LinSQL builds and boots, and SQL Server installs. It isn't production-ready yet: see [Known issues](#known-issues).

## Contents

- [What is LinSQL?](#what-is-linsql)
- [Features](#features)
- [Database servers](#database-servers)
- [System requirements](#system-requirements)
- [Installing LinSQL](#installing-linsql)
- [First login: database setup](#first-login-database-setup)
- [Using LinSQL](#using-linsql)
- [Building the ISO yourself](#building-the-iso-yourself)
- [Known issues](#known-issues)
- [Licensing](#licensing)
- [Credits](#credits)

---

## What is LinSQL?

LinSQL is a free, open-source Linux distribution for running database servers on ordinary PCs, VMs and older hardware. It looks and feels familiar to Windows Server administrators, stays light on memory, and lets you pick which database servers to install, in any combination, or none.

## Features

| | |
|---|---|
| 🗄️ **Your choice of databases** | Microsoft SQL Server 2025 / 2022, MySQL 8.4 / 9.7 LTS, PostgreSQL 15–18, chosen during installation |
| 🧠 **Memory planned for you** | Recommended memory split between the databases you choose, adjustable before anything is installed |
| 🖥️ **Windows Server–style desktop** | Taskbar, Start menu, Fluent theme and icons, window buttons on the right |
| 🛠️ **Server Manager** | Web-based admin console (Cockpit): services, storage, networking, users, logs, updates. Opens at login |
| 🔌 **Remote Desktop on by default** | Connect from Windows' built-in Remote Desktop (`mstsc`), limited to your local network |
| 🧰 **DBeaver included** | One GUI for SQL Server, MySQL and PostgreSQL, plus each database's command-line tools |
| 🌐 **Firefox** | Official Mozilla build (no Snap) |
| 🪶 **No bloat** | No office suite, games, media apps or Snap |
| 🔒 **Secure defaults** | Firewall on; database ports closed unless you allow them; strong passwords required |

## Database servers

Nothing is installed by default. Each database is downloaded **from its official source** at setup time, so you always get the latest security fixes.

| Database | Versions | Default | Source | Licence |
|---|---|---|---|---|
| Microsoft SQL Server | 2025 (Enterprise Developer, Standard Developer, Express, Evaluation, Standard, Enterprise) · 2022 (Developer, Express, Evaluation, Standard, Enterprise) | 2025 | Microsoft | Microsoft's terms, accepted by you |
| MySQL Community | 8.4 LTS, 9.7 LTS | 8.4 LTS | Oracle | GPL-2.0 |
| PostgreSQL | 18, 17, 16, 15 | 18 | PostgreSQL project | PostgreSQL Licence |

> ⚠️ **SQL Server 2022** isn't supported by Microsoft on Ubuntu 24.04, which LinSQL is based on. It's offered with that warning; use 2025 for supported installs.
>
> ⚠️ **SQL Server Developer editions** are free for **development and testing only**. For production use Express (within its limits) or a paid edition with your own licence.

### Default ports and admin accounts

| Database | Port | Admin account |
|---|---|---|
| SQL Server | 1433 | `sa` |
| MySQL | 3306 | `root` (this computer only); `dbadmin` from the network, if allowed |
| PostgreSQL | 5432 | `postgres` |

### Recommended memory

1.5 GB is kept for the system; the rest is split between the databases you choose (you can change every value):

| Databases | 8 GB PC | 16 GB PC | 32 GB PC |
|---|---|---|---|
| Any one | 6.5 GB | 14.5 GB | 30.5 GB |
| SQL Server + MySQL *or* PostgreSQL (60/40) | 3.9 + 2.6 GB | 8.7 + 5.8 GB | 18.3 + 12.2 GB |
| MySQL + PostgreSQL (50/50) | 3.25 + 3.25 GB | 7.25 + 7.25 GB | 15.25 + 15.25 GB |
| All three (50/25/25) | 3.25 / 1.6 / 1.6 GB | 7.25 / 3.6 / 3.6 GB | 15.25 / 7.6 / 7.6 GB |

## System requirements

| | Minimum | Recommended |
|---|---|---|
| CPU | 64-bit (x86-64), 2 cores, 2 GHz | 4 cores |
| RAM | 4 GB (one database) | 8 GB+ for two or three databases, 16 GB for comfortable use |
| Disk | 40 GB | 120 GB+ SSD |
| Filesystem for database files | ext4 or XFS | (Btrfs isn't supported by Microsoft for SQL Server) |
| Network | Internet during database setup | Wired Ethernet for a server |

The desktop itself uses roughly **350–500 MB** of RAM at idle.

## Installing LinSQL

1. **Download** `linsql-0.1-amd64.iso` and its `.sha256` file, then verify it:
   ```bash
   sha256sum -c linsql-0.1-amd64.iso.sha256
   ```
2. **Write it to a USB stick** (8 GB+) with balenaEtcher or Rufus (in Rufus choose **DD Image mode**), or attach it to a virtual machine.
3. **Boot** from it. On older PCs that boot to a black screen, set the BIOS to **Legacy first**.
4. On the LinSQL desktop, double-click **Install LinSQL**.
5. Go through language, location, keyboard, disk and user. Use a **strong password**: Remote Desktop is on by default.
6. On the **SQL Server**, **MySQL** and **PostgreSQL** pages, pick a version or **Don't install**.
7. Install, reboot, and remove the USB/ISO.

## First login: database setup

If you chose any databases, **LinSQL database setup** opens when you first log in (internet needed):

1. **Licence:** read and accept Microsoft's licence (SQL Server only); enter your product key for paid editions.
2. **Passwords:** set the admin password for each database (8+ characters, three of: capitals, small letters, digits, symbols).
3. **Network access:** allow or block connections from your local network, per database. The internet stays blocked either way.
4. **Storage (SQL Server):** keep the default folders, or choose separate folders for data, transaction logs and backups (ext4/XFS disks).
5. **Memory:** accept the recommended values or change them.
6. Enter your login password; LinSQL downloads, installs and configures everything, then shows the connection details.

Choose **Later** to postpone; it asks again at your next login. To add databases or re-balance memory at any time: **Start → Install or manage database servers**.

## Using LinSQL

### Connect to the databases

| Tool | How |
|---|---|
| **DBeaver** (taskbar) | New connection → choose the database → host `localhost` → admin user and password |
| SQL Server command line | `sqlcmd -S localhost -U sa -C` |
| MySQL command line | `mysql -u root -p` |
| PostgreSQL command line | `psql -U postgres -h localhost` |
| SSMS on a Windows PC | Server: LinSQL's IP address, SQL Server Authentication, `sa` (needs network access allowed) |

### Server Manager

Opens at login. Manage services (start/stop each database), storage, networking, users, logs and updates. From another PC on your network: `https://<LinSQL-IP>:9090`.

To stop it opening at login: **Start → Preferences → LXQt Settings → Session Settings → Autostart**, untick *Server Manager*.

### Remote Desktop

1. On LinSQL, find its address: `hostname -I`
2. On Windows: **Win + R** → `mstsc` → enter the address → log in with your LinSQL user.

From macOS, iOS or Android use Microsoft's *Windows App*; from Linux, Remmina.

> 🔒 Remote Desktop and Server Manager accept connections from **private local networks only**. Never forward port 3389 on your router; use a VPN for remote access.

### Useful checks

```bash
systemctl status mssql-server mysql postgresql --no-pager   # are the databases running?
free -h                                                       # memory use
sudo ufw status                                               # open ports
sudo tail -n 30 /var/log/linsql-db-setup.log                  # database setup log (no passwords)
```

### Backups

LinSQL doesn't back up your data automatically. Schedule backups to another disk: SQL Server `BACKUP DATABASE`, `mysqldump`, `pg_dump`.

## Building the ISO yourself

LinSQL is built from the official Lubuntu 24.04 ISO with **Cubic** on Ubuntu 24.04 Desktop. The build kit and step-by-step guide are in [`linsql-kit/`](linsql-kit/) — see [`linsql-kit/README.md`](linsql-kit/README.md).

In short:

1. Install Cubic and download Lubuntu 24.04.
2. Open the Lubuntu ISO in Cubic; on its terminal page run `bash /root/linsql-kit/build/linsql-customize.sh`.
3. Run `sudo bash ~/linsql-kit/host/linsql-brand-iso.sh ~/linsql-project` in a normal terminal.
4. Generate the ISO.

Database versions, repositories and memory rules live in one file: [`linsql-kit/overlay/etc/linsql/databases.conf`](linsql-kit/overlay/etc/linsql/databases.conf).

## Known issues

| Issue | Status |
|---|---|
| SQL Server could hang while applying its memory setting (builds before v0.3.1) | Fixed in kit v0.3.1: settings are applied before the first start |
| Some Lubuntu branding may still appear (boot screen, "Lubuntu Manual") | Being removed |
| SQL Server 2022 is unsupported by Microsoft on Ubuntu 24.04 | Offered with a warning; may be dropped if it fails testing |
| Some version settings are still to be confirmed (MySQL repository names, SQL Server 2025 edition names) | Marked `VERIFY` in `databases.conf` |
| No lockout after repeated failed Remote Desktop logins yet | Planned (fail2ban) |
| Storage folders can be chosen for SQL Server only | MySQL/PostgreSQL planned |
| Lubuntu's own support ends April 2027 (Ubuntu's core: April 2029) | Review before then |
| UEFI boot didn't work on one older laptop (ThinkPad T420) | Use Legacy boot there |

Found a problem? Please open an issue with a screenshot and, for database setup, the output of `sudo tail -n 50 /var/log/linsql-db-setup.log`.

## Licensing

- **LinSQL** (build scripts, configuration and artwork) is free software under the **GNU General Public License v3.0 or later**. See [`linsql-kit/LICENSE`](linsql-kit/LICENSE).
- **Microsoft SQL Server is not part of LinSQL.** It is downloaded from Microsoft only if you choose it, under Microsoft's licence terms, which you accept during setup. LinSQL can't grant SQL Server licences.
- MySQL Community (GPL-2.0) and PostgreSQL (PostgreSQL Licence) are downloaded from their official sources.
- Other components keep their own licences; see [`linsql-kit/CREDITS.md`](linsql-kit/CREDITS.md).

LinSQL is not affiliated with or endorsed by Microsoft, Oracle, Canonical or the PostgreSQL Global Development Group. Microsoft and SQL Server are trademarks of Microsoft; MySQL is a trademark of Oracle; Ubuntu is a trademark of Canonical.

## Credits

**Developer:** Nilesh Mishra

LinSQL builds on Ubuntu, Lubuntu, LXQt, Calamares, Cockpit, xrdp, DBeaver, Firefox and the Fluent theme by Vince Liuice. The logo was generated with Google Gemini; its penguin resembles Tux, created by Larry Ewing with The GIMP. Full list: [`linsql-kit/CREDITS.md`](linsql-kit/CREDITS.md).
