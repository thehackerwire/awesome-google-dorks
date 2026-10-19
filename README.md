<div align="center">

# 🔎 Awesome Google Dorks & OSINT Search Operators

**A curated collection of advanced Google Dorks (Google Hacking Database queries) for ethical hackers, penetration testers, security auditors, and bug bounty researchers.**

[![Live Interactive Engine](https://img.shields.io/badge/Live_Engine-TheHackerWire-00ff41?style=for-the-badge&logo=google)](https://thehackerwire.com/hacking-tools/google-dorking-engine)
[![Total Queries](https://img.shields.io/badge/Queries-1000+-00e5ff?style=for-the-badge)](#curated-dork-categories)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)

[⚡ Launch Google Dorking Engine](https://thehackerwire.com/hacking-tools/google-dorking-engine) • [Shodan Dorks Engine](https://thehackerwire.com/hacking-tools/shodan-dorks) • [All Hacking Tools](https://thehackerwire.com/hacking-tools/)

</div>

---

> [!TIP]
> **Use the Free Interactive Web Engine**  
> Visit **[TheHackerWire Google Dorking Engine](https://thehackerwire.com/hacking-tools/google-dorking-engine)** to instantly inject your target domain (`site:target.com`), filter across 14 reconnaissance categories, build custom dorks with a live sandbox, and export comprehensive audit dossiers.

---

## 📑 Table of Contents

- [Core Google Dorking Operators](#-core-google-dorking-operators)
- [Curated Dork Categories](#-curated-dork-categories)
  - [1. Configuration Files & Environment Secrets](#1-configuration-files--environment-secrets)
  - [2. Exposed Database Backups & Dumps](#2-exposed-database-backups--dumps)
  - [3. Log Files & Error Dumps](#3-log-files--error-dumps)
  - [4. Administrative Portals & Dashboards](#4-administrative-portals--dashboards)
  - [5. Publicly Exposed Cloud Storage Buckets](#5-publicly-exposed-cloud-storage-buckets)
  - [6. Sensitive Directory Listings ("Index Of")](#6-sensitive-directory-listings-index-of)
  - [7. API Keys, Tokens & Private Certificates](#7-api-keys-tokens--private-certificates)
  - [8. Subdomain Enumeration](#8-subdomain-enumeration)
- [Defensive Countermeasures & Removal](#-defensive-countermeasures--removal)
- [Legal & Ethical Notice](#-legal--ethical-notice)

---

## 🔍 Core Google Dorking Operators

| Operator | Syntax Example | Function |
| :--- | :--- | :--- |
| `site:` | `site:target.com` | Restricts search to a specific domain or host |
| `filetype:` / `ext:` | `filetype:env` or `ext:sql` | Filters results by specific file extension |
| `inurl:` | `inurl:admin` | Locates specific strings anywhere in the URL path |
| `allinurl:` | `allinurl:login auth admin` | Requires all listed terms to appear in the URL |
| `intitle:` | `intitle:"Index of /"` | Matches keywords appearing in HTML `<title>` tags |
| `intext:` | `intext:"DB_PASSWORD"` | Matches exact text inside the web page body |
| `cache:` | `cache:target.com` | Displays Google's cached snapshot of the web page |
| `-` (Exclusion) | `site:target.com -www` | Excludes specific keywords or subdomains from results |

---

## 🗂️ Curated Dork Categories

*(Replace `TARGET.COM` with your authorized audit target)*

### 1. Configuration Files & Environment Secrets

```sh
# Exposed .env configuration files containing database credentials
site:TARGET.COM filetype:env "DB_PASSWORD"

# WordPress configuration files
site:TARGET.COM inurl:wp-config.php.bak OR inurl:wp-config.php.old

# Web server configuration files
site:TARGET.COM ext:conf OR ext:cnf "server" "listen"

# Docker Compose deployment configs
site:TARGET.COM filename:docker-compose.yml "environment:"

# Git configuration and commit logs exposed
site:TARGET.COM inurl:/.git/config
```

### 2. Exposed Database Backups & Dumps

```sh
# SQL database dumps containing raw inserts
site:TARGET.COM ext:sql "INSERT INTO" OR "CREATE TABLE"

# Compressed database archives
site:TARGET.COM ext:sql.gz OR ext:sql.tar.gz OR ext:dump

# MongoDB or Redis exported backups
site:TARGET.COM ext:json "users" "password_hash"

# SQLite database files
site:TARGET.COM ext:sqlite OR ext:sqlite3 OR ext:db
```

### 3. Log Files & Error Dumps

```sh
# Apache and Nginx web server access logs
site:TARGET.COM ext:log "GET /" "200"

# Application runtime crash logs
site:TARGET.COM ext:log "fatal error" OR "stack trace"

# Laravel debug logs with exception traces
site:TARGET.COM inurl:storage/logs/laravel.log

# Mail and SMTP transmission logs
site:TARGET.COM inurl:mail.log OR inurl:maillog
```

### 4. Administrative Portals & Dashboards

```sh
# Unprotected admin login interfaces
site:TARGET.COM inurl:admin OR inurl:login OR inurl:dashboard

# phpMyAdmin database management entrypoints
site:TARGET.COM inurl:phpmyadmin/index.php

# Grafana and Kibana observability portals
site:TARGET.COM intitle:"Grafana" OR intitle:"Kibana"

# Control panel portals (cPanel, WHM, Plesk)
site:TARGET.COM inurl:2083 OR inurl:2087 OR inurl:8443
```

### 5. Publicly Exposed Cloud Storage Buckets

```sh
# Target files hosted on open Amazon AWS S3 buckets
site:s3.amazonaws.com "TARGET.COM"

# Target files on Microsoft Azure Blob storage
site:blob.core.windows.net "TARGET.COM"

# Target files on Google Cloud Storage (GCS)
site:storage.googleapis.com "TARGET.COM"

# DigitalOcean Spaces buckets
site:digitaloceanspaces.com "TARGET.COM"
```

### 6. Sensitive Directory Listings ("Index Of")

```sh
# Open directories containing backups
site:TARGET.COM intitle:"Index of /" inurl:backup

# Open upload and attachment directories
site:TARGET.COM intitle:"Index of /" inurl:uploads

# Open configuration directories
site:TARGET.COM intitle:"Index of /" inurl:config OR inurl:conf
```

### 7. API Keys, Tokens & Private Certificates

```sh
# RSA and SSH private keys leaked online
site:TARGET.COM ext:key OR ext:pem "BEGIN RSA PRIVATE KEY"

# JSON Web Tokens (JWT) and bearer authorization headers
site:TARGET.COM ext:txt OR ext:json "eyJh" "Authorization: Bearer"

# AWS access keys and secret tokens
site:TARGET.COM "AKIA" "aws_secret_access_key"

# Google Cloud service account JSON keys
site:TARGET.COM ext:json "private_key_id" "type\": \"service_account"
```

### 8. Subdomain Enumeration

```sh
# Finding subdomains while excluding www
site:*.TARGET.COM -www

# Discovering internal and developer subdomains
site:*.TARGET.COM inurl:dev OR inurl:staging OR inurl:test OR inurl:internal
```

---

## 🛡️ Defensive Countermeasures & Removal

1. **`robots.txt` is not access control**: Disallowing paths in `robots.txt` merely asks search engines not to crawl; it does not protect files from being accessed.
2. **Proper Authentication**: Require HTTP Basic or SSO authentication on all staging, administration, and logging portals.
3. **Google Search Console Removal**: Request immediate de-indexing of accidentally exposed URLs via Google Search Console's *Removals* tool.
4. **Automated Dork Auditing**: Regularly run target audits using [TheHackerWire Google Dorking Engine](https://thehackerwire.com/hacking-tools/google-dorking-engine) to find leaks before adversaries do.

---

## ⚖️ Legal & Ethical Notice

Google Dorking utilizes public search indexing. However, attempting to gain unauthorized access to discovered endpoints, bypassing authentication, or downloading non-public confidential data without permission is illegal under international cyber law. Use these dorks only for authorized defense and authorized penetration testing.

---

<div align="center">

**Maintained by [TheHackerWire](https://thehackerwire.com)**  
*Cybersecurity Intelligence, Exploits & Online Tool Suite*

[Google Dorking Engine](https://thehackerwire.com/hacking-tools/google-dorking-engine) • [Shodan Dorks Engine](https://thehackerwire.com/hacking-tools/shodan-dorks) • [Online Tools Directory](https://thehackerwire.com/hacking-tools/)

</div>
