# SA Vehicle Licence Assistant

A professional **PySide6 desktop application** for South African motor vehicle licensing and registration document preparation. Built for licensing agencies, dealerships, and administrators who process ALV, NCO, and RLV forms daily.

[![Python](https://img.shields.io/badge/Python-3.11%2B-blue.svg)](https://python.org)
[![PySide6](https://img.shields.io/badge/PySide6-6.7%2B-green.svg)](https://pypi.org/project/PySide6/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20macOS-lightgrey.svg)]()
[![Build](https://img.shields.io/badge/Build-PyInstaller-orange.svg)](https://pyinstaller.org/)

---

## 🎯 Overview

**SA Vehicle Licence Assistant** eliminates manual form-filling for South African motor vehicle transactions. It combines a polished dark-theme UI, a Western Cape pricing calculator with historical tariffs, encrypted backups, and a commercial licensing system with seat management—all in a standalone Windows installer.

| | |
|---|---|
| ![Dashboard](screenshots/01_dashboard.png) | ![Customers](screenshots/02_customers_list.png) |
| **Dashboard** — Live stats, recent activity, global search | **Customers** — Paginated, searchable, filterable table with inline actions |

| | |
|---|---|
| ![Customer Drawer](screenshots/03_customer_drawer_overview.png) | ![Customer Drawer - Vehicles](screenshots/04_customer_drawer_vehicles.png) |
| **Customer Drawer — Overview** — Expandable sidebar with tabs: Overview, Vehicles, Documents, History | **Vehicles Tab** — Add/edit/delete vehicles inline, see licence expiry at a glance |

| | |
|---|---|
| ![Customer Drawer - Documents](screenshots/05_customer_drawer_documents.png) | ![Customer Drawer - History](screenshots/06_customer_drawer_history.png) |
| **Documents Tab** — Upload ID / proof of address, attach to generated bundles | **History Tab** — Transaction log with form counts and government cost estimates |

| | |
|---|---|
| ![Runners](screenshots/07_runners.png) | ![Pricing Calculator](screenshots/08_pricing_calculator.png) |
| **Runners / Transaction People** — Reusable runner profiles with search | **Pricing Calculator** — Western Cape tariffs (2020–present), pro-rata, penalties, arrears, early licensing |

| | |
|---|---|
| ![Settings - General](screenshots/13_settings_general.png) | ![Settings - Licence](screenshots/14_settings_licence.png) |
| **Settings — General** | **Settings — Licence & Seat Management** |

| | |
|---|---|
| ![Settings - Updates](screenshots/15_settings_updates.png) | ![Settings - Backup](screenshots/16_settings_backup.png) |
| **Settings — Auto-Updates** | **Settings — Encrypted Backup/Restore (Argon2id)** |

---

## ✨ Key Features

### 📋 Document Generation (ALV / NCO / RLV)
- **Fillable PDF templates** (PyMuPDF + ReportLab) — field-mapped, not coordinate-hacked
- **Combined PDF bundles** — selected forms + permission letter + client uploads (ID / proof of address) in one file
- **Company & individual clients** — CK/registration number maps to form identification field; proxy/representative details auto-filled
- **Multiple vehicles per customer** — one bundle per transaction
- **Reusable runners** — transaction people saved and reused

### 💰 Western Cape Pricing Calculator
- **Historical tariffs (2020 → present)** — forward fee uses application-date tariff; arrears/penalty use expiry-date tariff
- **Late renewal math** (per NRTA Reg 57 & 59):
  - Arrears = annual_fee × complete_months ÷ 12 (round **down**)
  - Penalty = 10% × annual_fee × penalty_months (round **up**, capped at 100% = 10 months)
  - RTMC fee = R72 flat per renewal
- **Early licensing (>60 days before expiry)** — pro-rata based on months used since issue date
- **Cross-tariff handling** — penalty months use the rate in effect for **each** month
- **Calendar-month math** via `dateutil.relativedelta` (handles month-end correctly: Jan 31 + 1 month = Feb 28/29)

### 🔐 Commercial Licensing (Ed25519 + Seat Management)
- **Offline key generation** — Ed25519-signed, base64url payload; private key **never** leaves your machine
- **2-seat limit per key** — installing on a 3rd PC evicts the oldest seat (rotation)
- **Feature flags** — `pricing`, `documents` embedded in key; gated at runtime
- **Offline grace period** (30 days) — runs offline if server unreachable, blocks after grace expires
- **Activation server** (Flask + Neon Postgres) — tiny, deployable to Render/Railway/Fly.io
- **Build-time constant embedding** — public key & server URL baked into the EXE; no `.env` in production

### 💾 Encrypted Backup / Restore
- **Argon2id** (OWASP-recommended params: t=3, m=64 MB, p=4) — v2 format
- **Legacy v1 (PBKDF2)** readable with migration warning
- **ZipSlip protection** — path traversal validation before extraction
- **Contents**: SQLite DB, fee table, all generated PDFs

### 🔄 Mandatory Auto-Updater
- **Checks on every launch** — blocks UI with progress dialog until complete
- **Downloads ZIP → applies → restarts** — no user interaction required
- **Rollback-safe** — leftover update files cleaned on next start
- **GitHub Releases** or any static HTTPS endpoint

### 🎨 Polished Dark UI (PySide6 + QSS)
- **Collapsible sidebar** with icons + page titles
- **Resizable 70/30 splitter** (Customers table ↔ Detail drawer) — persists per-user
- **Drawer rail mode** (64 px icon-only) — auto-collapses on narrow windows
- **Stat cards**, paginated tables, search + type filter, inline action buttons
- **Keyboard shortcuts**: `Ctrl+N` (new customer), `Ctrl+F` (search), `Ctrl+G` (generate), `Ctrl+P` (pricing), `Ctrl+B` (toggle drawer), `Delete` / `F2` / `Esc`

---

## 📸 Screenshots Gallery

| Page | Screenshot |
|------|------------|
| **Dashboard** | ![Dashboard](screenshots/01_dashboard.png) |
| **Customers List** | ![Customers](screenshots/02_customers_list.png) |
| **Drawer — Overview** | ![Drawer Overview](screenshots/03_customer_drawer_overview.png) |
| **Drawer — Vehicles** | ![Drawer Vehicles](screenshots/04_customer_drawer_vehicles.png) |
| **Drawer — Documents** | ![Drawer Documents](screenshots/05_customer_drawer_documents.png) |
| **Drawer — History** | ![Drawer History](screenshots/06_customer_drawer_history.png) |
| **Runners** | ![Runners](screenshots/07_runners.png) |
| **Pricing Calculator** | ![Pricing](screenshots/08_pricing_calculator.png) |
| **Settings — General** | ![Settings General](screenshots/13_settings_general.png) |
| **Settings — Licence** | ![Settings Licence](screenshots/14_settings_licence.png) |
| **Settings — Updates** | ![Settings Updates](screenshots/15_settings_updates.png) |
| **Settings — Backup** | ![Settings Backup](screenshots/16_settings_backup.png) |
| **Settings — Database** | ![Settings Database](screenshots/17_settings_database.png) |
| **Settings — Advanced** | ![Settings Advanced](screenshots/18_settings_advanced.png) |
| **Settings — Logs** | ![Settings Logs](screenshots/19_settings_logs.png) |
| **Add Customer Dialog** | ![Add Customer](screenshots/20_add_customer_dialog.png) |
| **Add Vehicle Dialog** | ![Add Vehicle](screenshots/21_add_vehicle_dialog.png) |
| **Dashboard Search** | ![Dashboard Search](screenshots/21_dashboard_search.png) |
| **Customers Search** | ![Customers Search](screenshots/22_customers_search.png) |
| **Runners Search** | ![Runners Search](screenshots/23_runners_search.png) |
| **Pricing with Data** | ![Pricing Data](screenshots/24_pricing_with_data.png) |
| **Drawer Rail Mode** | ![Rail Mode](screenshots/25_customer_drawer_rail_mode.png) |

---

## 📄 License

MIT License — see [LICENSE](LICENSE) for details.

---

## 🙏 Acknowledgements

- **Western Cape Department of Transport** — fee schedules & form layouts
- **PySide6 / Qt** — rock-solid cross-platform UI
- **PyMuPDF (fitz)** — best-in-class PDF manipulation
- **cryptography** — modern, audited primitives
- **dateutil** — calendar math that actually works

---

**Built with ❤️ for South African licensing professionals.**  
*If this saves you time, consider ⭐ starring the repo!*
