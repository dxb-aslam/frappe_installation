# Server Optimization Guide for ERPNext Hosting

This document summarizes the key performance optimizations done on the server (Oracle Ampere A1, 2 vCPU, 12 GB RAM) to improve MariaDB performance and system stability for ERPNext.

---

## ✅ 1. MariaDB Optimization (`mysqld` Configuration)

### 📍 File Edited:
`/etc/mysql/mariadb.conf.d/50-server.cnf`

### 🧾 Changes Made Under `[mysqld]` Section:
```ini
# InnoDB
innodb_buffer_pool_size = 6G        # ~55% of RAM, keeps plenty for Frappe & OS
innodb_buffer_pool_instances = 3    # 2GB each
innodb_log_file_size = 512M
innodb_flush_log_at_trx_commit = 2
innodb_flush_method = O_DIRECT

# Connections & memory
max_connections = 150               # 300 is overkill for ERPNext; lowers per-conn memory usage
max_allowed_packet = 128M           # 256M not needed unless importing huge docs
tmp_table_size = 128M
max_heap_table_size = 128M

# Query cache (disable on modern MySQL/MariaDB)
query_cache_type = 0
query_cache_size = 0

# Thread handling
thread_cache_size = 50
table_open_cache = 2048
open_files_limit = 65535

```

### 🔄 After Editing:
Restart MariaDB to apply changes:
```bash
sudo systemctl restart mariadb
```

### ✅ Live Verification (via MySQL shell):
```sql
SHOW VARIABLES LIKE 'innodb_buffer_pool_size';
SHOW VARIABLES LIKE 'max_connections';
SHOW VARIABLES LIKE 'max_allowed_packet';
```

---

## ✅ 2. Swap File Setup (2 GB Swap)

### 📍 Purpose:
Acts as a memory overflow buffer to prevent crashes when RAM usage spikes.

### 🧾 Commands Used:
```bash
# 1. Create a 2 GB swap file
sudo fallocate -l 2G /swapfile

# 2. Set secure permissions
sudo chmod 600 /swapfile

# 3. Format the file as swap
sudo mkswap /swapfile

# 4. Enable the swap file
sudo swapon /swapfile

# 5. Make it persistent across reboots
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

### 🔧 Optional (for better swap behavior):
```bash
sudo sysctl vm.swappiness=10
echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
```

### ✅ Verify:
```bash
free -h
swapon --show
```

Expected output:
- Swap: `2.0Gi` total
- Usage: `0B` unless under memory pressure

---

## 📌 Summary

| Component | Action Taken                         |
|----------|---------------------------------------|
| MariaDB  | Increased buffer pool, connections    |
| MariaDB  | Set temp table & packet size limits   |
| MariaDB  | Reduced flush overhead                |
| OS       | Created 2 GB swap file                |
| OS       | Swappiness set to 10 (minimal usage)  |

---

## 🗂 For Future Enhancements (Optional, Not Applied Yet)

- Enable `slow_query_log` for SQL diagnostics
- Use `mysqltuner` for ongoing tuning
- Add Redis and Gunicorn tuning
- Configure gzip + caching in nginx

---

_Last updated: August 2025_
