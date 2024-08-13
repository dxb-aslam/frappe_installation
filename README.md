## Frappe Installation Guide

### Step 1: Install Required Packages

To begin, install the necessary packages for setting up Frappe on your system:

```bash
sudo apt-get install git
sudo apt-get install python3-dev python3-setuptools python3-pip virtualenv python3.10-venv
sudo apt-get install software-properties-common
```

### Step 2: Install and Configure MariaDB

1. Install MariaDB server:

    ```bash
    sudo apt install mariadb-server
    ```

2. Secure your MariaDB installation by running:

    ```bash
    sudo mysql_secure_installation
    ```

3. Install the MySQL client development libraries:

    ```bash
    sudo apt-get install libmysqlclient-dev
    ```

### Step 3: Configure MariaDB for Frappe

1. Open the MariaDB server configuration file:

    ```bash
    sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
    ```

2. Update the configuration with the following settings:

    ```ini
    [server]
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

    [mysqld]
    innodb-file-format=barracuda
    innodb-file-per-table=1
    innodb-large-prefix=1
    character-set-client-handshake = FALSE
    character-set-server = utf8mb4
    collation-server = utf8mb4_unicode_ci      

    [mysql]
    default-character-set = utf8mb4
    ```

3. Save and exit the editor.

### Step 4: Restart MariaDB

Finally, restart the MariaDB service to apply the changes:

```bash
sudo systemctl restart mariadb
```

---

This documentation will guide users through installing and configuring Frappe with MariaDB on their system. You can include this in your GitHub repository's README or a separate installation guide file.
