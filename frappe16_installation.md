# ⚙️ Frappe / ERPNext v16 – Ubuntu Quick Install (Safe Version)

> This version **explicitly includes C build tools** to avoid
> `mysqlclient → error: command 'cc' failed`

---

## 1️⃣ System Update

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 2️⃣ Core Build & Python Dependencies (**CRITICAL**)

> **This section prevents your exact failure**

```bash
sudo apt install -y \
  build-essential gcc pkg-config \
  python3 python3-dev python3-pip python3-venv \
  git curl
```

✅ Ensures:

* `cc` exists
* C extensions compile
* `mysqlclient` builds cleanly

---

## 3️⃣ Database & Cache

```bash
sudo apt install -y mariadb-server redis-server
```

MariaDB config (`/etc/mysql/conf.d/frappe.cnf`):

```ini
[mysqld]
character-set-server=utf8mb4
collation-server=utf8mb4_unicode_ci
```

Restart:

```bash
sudo systemctl restart mariadb
```

---

## 4️⃣ MariaDB Client Headers (**Required for mysqlclient**)

```bash
sudo apt install -y \
  libmariadb-dev libmariadb-dev-compat
```

---

## 5️⃣ Node, Yarn, PDF Engine

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
sudo npm install -g yarn

sudo apt install -y wkhtmltopdf xvfb libfontconfig
```

---

## 6️⃣ Install Bench

```bash
python3 -m pip install --upgrade pip
python3 -m pip install frappe-bench --user
export PATH="$HOME/.local/bin:$PATH"
```

---

## 7️⃣ Initialize Bench (Safe)

```bash
bench init frappe-bench \
  --frappe-branch version-16 \
  --python python3
```

✔️ Will **not fail** on `mysqlclient`
✔️ Compiler + MariaDB headers already present

---

## 8️⃣ Create Site & Install ERPNext

```bash
cd frappe-bench
bench new-site site1.local

bench get-app erpnext --branch version-16
bench --site site1.local install-app erpnext
```

---

## 9️⃣ Run (Dev)

```bash
bench start
```

---

## 🔍 Sanity Checks (Optional but Smart)

```bash
which cc
cc --version
python3 --version
node --version
```

---

## ⚠️ Important Notes

* **Do NOT skip `build-essential`** → 
* Always install **libmariadb-dev BEFORE bench init**
* ARM / aarch64 servers **must compile mysqlclient**
