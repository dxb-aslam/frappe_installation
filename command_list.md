Install and Setup Requirements

```bash
sudo apt-get update
sudo apt-get install -y git
sudo apt-get install -y python3-dev python3-setuptools python3-pip virtualenv python3.10-venv
sudo apt-get install -y software-properties-common
sudo apt install -y mariadb-server
sudo mysql_secure_installation
sudo apt-get install -y libmysqlclient-dev
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
sudo systemctl restart mariadb
sudo apt-get install -y redis-server
sudo apt install -y curl
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.profile
nvm install 18
sudo apt-get install -y npm
sudo npm install -g yarn

sudo apt-get remove --purge -y wkhtmltopdf
sudo apt-get install -y xfonts-75dpi
wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.5-1/wkhtmltox_0.12.5-1.bionic_amd64.deb
sudo dpkg -i wkhtmltox_0.12.5-1.bionic_amd64.deb
sudo apt-get install -f -y
sudo -H pip3 install frappe-bench
```

Update Config

[server]
```ini
user = mysql
pid-file = /run/mysqld/mysqld.pid
socket = /run/mysqld/mysqld.sock
basedir = /usr
datadir = /var/lib/mysql
tmpdir = /tmp
lc-messages-dir = /usr/share/mysql
bind-address = 127.0.0.1
query_cache_size = 16M
log_error = /var/log/mysql/error.log
```
[mysqld]
```bash
innodb-file-format=barracuda
innodb-file-per-table=1
innodb-large-prefix=1
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
```

```ini
[mysql]
default-character-set = utf8mb4
```

```bash
bench init [bench_name] --frappe-branch version-15
```

```bash
bench get-app erpnext --branch version-15
bench get-app hrms --branch version-15
```

```bash
cd [bench_name]
```

```bash
bench new-site [site_name] --install-app hrms
```

```bash
sudo bench setup production [user]
```

```bash
bench restart
```

```bash
sudo chmod o+x /home/[user]
```

```bash
bench config dns_multitenant on
```

```bash
sudo apt install snapd
sudo snap install core
sudo snap refresh core
sudo apt-get remove certbot
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo certbot --nginx
```
