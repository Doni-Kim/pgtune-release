# pgtune — a real-time PostgreSQL monitor (free)

![.NET 11](https://img.shields.io/badge/.NET-11.0-512BD4)
![C# 15](https://img.shields.io/badge/C%23-15-239120)
![Blazor Hybrid](https://img.shields.io/badge/Blazor-Hybrid-5C2D91)
![PostgreSQL 14+](https://img.shields.io/badge/PostgreSQL-14%2B-336791)
![Windows x64](https://img.shields.io/badge/Windows-x64-0078D6)

**[Download the latest release](https://github.com/Doni-Kim/pgtune-release/releases/latest)** ·
[한국어 설명](README.ko.md) · [Manual (Korean, HTML)](pgtune.html)

pgtune is a desktop monitor for PostgreSQL. This release is a full rewrite of the UI and the
diagnostics behind it. Free to use, no strings attached.

## Screenshots

| Live dashboard | Top SQL |
|---|---|
| ![Dashboard](screenshots/dashboard.png) | ![Top SQL](screenshots/top-sql.png) |

| Lock chains | History |
|---|---|
| ![Locks](screenshots/locks.png) | ![History](screenshots/history.png) |

![Alerts](screenshots/alerts.png)

## Install

- Unzip, keep the folder together, and run `pgtune.exe` (single-file publish).
- No .NET install needed — the runtime is inside the executable.
- The only thing to set up is `pgtune.json` next to the executable.

## What it does

- **Live dashboard** — sessions, throughput, wait events, trend graphs.
- **Panels** — Top SQL, lock chains, index diagnostics, VACUUM/XID, replication status.
- **Alerts** — connection saturation, deadlocks, replication lag and more, with your own thresholds.
- **History** — press `L` to log every snapshot into a local SQLite file, then `H` to look back.
  - Ranges: 1 hour / 6 hours / 24 hours / 1 week / 1 month / all.
  - Ten metrics: TPS, cache hit ratio, active, waiting, total connections, idle in transaction,
    rollbacks, deadlocks, temp file throughput, buffers written by backends.
  - Hover the chart to read the value at that moment; the shaded band shows the min–max of each column.
  - Old rows are trimmed automatically (30 days of metrics, 7 days of sessions by default; configurable).
- **Excel export** — built on ClosedXML, so the `.xlsx` is written even without Excel installed,
  and opens automatically when Excel is there.
- Press `F1` for the keyboard shortcuts.

The bundled `pgtune.html` is the full manual (in Korean).

## Requirements and limits

- **Windows only.** The UI is web-based (Blazor Hybrid), so it does not run standalone on Linux or macOS.
- PostgreSQL 14 and newer.
- The code is obfuscated with ConfuserEx — a free tool, so do not expect strong protection.

## Blank window? (WebView2)

If the window opens but stays blank, the WebView2 runtime is missing.

- **Windows 11** — built into the OS, always present.
- **Windows 10** — shipped through Windows Update since 2021, so it is there on most machines.
  A PC that has not been updated in a long time, or a special edition such as LTSC, may not have it.
- **Windows Server (2016/2019/2022)** — often not included; install it separately.

Install Microsoft's "Evergreen Standalone Installer"
(`MicrosoftEdgeWebView2RuntimeInstallerX64.exe`) from
https://developer.microsoft.com/microsoft-edge/webview2/

## If something breaks

Errors are written to `pgtune.log` next to the executable.

- **Bugs and questions** — open an [issue](https://github.com/Doni-Kim/pgtune-release/issues).
  Please do not attach the log there: it holds no passwords, but it can contain server addresses and SQL text.
- **The log file**, or anything you would rather not post in public — mail it to **doniikim@gmail.com**.

## Built with

- .NET 11.0 (x64), C# 14, Blazor Hybrid
- Npgsql · Microsoft.Data.Sqlite · ClosedXML · Microsoft.Web.WebView2 · Microsoft.AspNetCore.Components.WebView.WindowsForms
- Copyright notices and license texts of these bundled components: [THIRD-PARTY-NOTICES.txt](THIRD-PARTY-NOTICES.txt) (also inside the zip)

## pgtune.json

```json
{
  "databases": [
    {
      "userId": "postgres",
      "password": "postgres",
      "server": "127.0.0.1",
      "port": "5432",
      "database": ""
    }
  ],
  "interval": 4
}
```

- Write `password` in plain text — it is encrypted on the first run and stored back.
- Leave `database` empty to pick a database from a list after connecting.
- Sections such as `alerts`, `topSql` and `logRetention` are optional. pgtune fills them in with
  defaults when it closes, so you can see what is there to tune.

## Monitoring account

A superuser is not required. These two grants make every screen show the same as for a superuser:

```sql
CREATE ROLE pgtune_monitor LOGIN PASSWORD '...';
GRANT pg_monitor TO pgtune_monitor;          -- other users' sessions and queries, sizes, settings
GRANT pg_signal_backend TO pgtune_monitor;   -- only if you want Ctrl+K (cancel a session)
```

Without `pg_monitor`, PostgreSQL hides other users' sessions. pgtune says so instead of looking empty —
"N sessions of other users are hidden — grant pg_monitor …" above the session list, Connections and Locks.

## Terms

Free to use, at work or at home. Please do not redistribute the binary or reverse-engineer it.
The source is not published.

## Contact

DBMS Works — **doniikim@gmail.com**

Also available for Oracle → PostgreSQL / MySQL migration and database performance tuning work.
