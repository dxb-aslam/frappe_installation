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

2. **Update the Configuration**:
   
   - **For the `[server]` and `[mysqld]` sections**: Copy and paste the following settings into their respective sections:
   
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
    ```

   - **At the end of the file**, add the following section:

    ```ini
    [mysql]
    default-character-set = utf8mb4
    ```

3. Save and exit the editor.

### Step 4: Restart MariaDB

Finally, restart the MariaDB service to apply the changes:

```bash
sudo systemctl restart mariadb
```

### Step 5: Install Redis Server

Redis is required for caching and background job processing in Frappe. Install it by running:

```bash
sudo apt-get install redis-server
```

### Step 6: Install cURL

cURL is necessary for downloading files from the internet. Install it with:

```bash
sudo apt install curl
```

### Step 7: Install Node Version Manager (NVM) and Node.js

1. Install NVM by running the following command:

    ```bash
    curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
    ```

2. Load NVM into the current shell session:

    ```bash
    source ~/.profile
    ```

3. Install Node.js version 20.16.0 using NVM:

    ```bash
    nvm install 20.16.0
    ```

### Step 8: Install npm and Yarn

1. Install npm (Node Package Manager):

    ```bash
    sudo apt-get install npm
    ```

2. Install Yarn globally using npm:

    ```bash
    sudo npm install -g yarn
    ```

### Step 9: Install Additional Dependencies

Frappe requires some additional packages for PDF generation and other functionalities:

```bash
sudo apt-get install xvfb libfontconfig wkhtmltopdf
```

### Step 10: Install Frappe Bench

Finally, install the Frappe Bench CLI tool using pip3:

```bash
sudo -H pip3 install frappe-bench
```

### Step 11: Create a New Bench

To create a new bench, use the following command, specifying the Frappe branch you want to use:

```bash
bench init [bench_name] --frappe-branch version-15
```

Replace `[bench_name]` with the desired name for your bench.

### Step 12: Install ERPNext and HRMS

After initializing the bench, install the ERPNext and HRMS apps using the following commands:

```bash
bench get-app erpnext --branch version-15
bench get-app hrms --branch version-15
```

### Step 13: Create a New Site

Navigate to your bench directory:

```bash
cd [bench_name]
```

Then, create a new site and install ERPNext and HRMS with the following command:

```bash
bench new-site [site_name] --install-app hrms
```

Replace `[site_name]` with the desired name for your site. This command will create the site and install both ERPNext and HRMS along with it.

### Conclusion

You have now successfully installed all the necessary components to set up Frappe on your system. You can proceed with creating a new Frappe site or configuring your development environment as needed.
