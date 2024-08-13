# frappe_installation

`sudo apt-get install git;<br>
sudo apt-get install python3-dev python3-setuptools python3-pip virtualenv python3.10-venv;
sudo apt-get install software-properties-common;
sudo apt install mariadb-server;
sudo mysql_secure_installation;`

`sudo apt-get install libmysqlclient-dev;
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf`

`[server]
user = mysql
pid-file = /run/mysqld/mysqld.pid
socket = /run/mysqld/mysqld.sock
basedir = /usr
datadir = /var/lib/mysql
tmpdir = /tmp
lc-messages-dir = /usr/share/mysql
bind-address = 127.0.0.1
query_cache_size = 16M
log_error = /var/log/mysql/error.log`

`[mysqld]
innodb-file-format=barracuda
innodb-file-per-table=1
innodb-large-prefix=1
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci`      
 
`[mysql]
default-character-set = utf8mb4`


`sudo service mysql restart;
sudo apt-get install redis-server;
sudo apt install curl;
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash;
source ~/.profile;
nvm install 20.16.0;
sudo apt-get install npm;
sudo npm install -g yarn;
sudo apt-get install xvfb libfontconfig wkhtmltopdf;
sudo -H pip3 install frappe-bench;
bench init sandbox_uae --frappe-branch version-15;`

`bench get-app erpnext --branch version-15;
bench get-app hrms --branch version-15;
bench new-site blueline-cloud.sandbox-uae.dxbitz.com --install-app hrms;`

bench config dns_multitenant on

sudo bench setup production -;
bench restart;
sudo bench setup production -;

sudo chmod o+x /home/-;



sudo apt install snapd;
sudo snap install core; sudo snap refresh core;

sudo apt-get remove certbot;

sudo snap install --classic certbot;
sudo ln -s /snap/bin/certbot /usr/bin/certbot;

sudo certbot --nginx;


sudo certbot renew --dry-run;

sudo nano /etc/iptables/rules.v4

sudo iptables-restore < /etc/iptables/rules.v4

sudo iptables -L INPUT



SPAWN ERROR:
bench setup socketio

bench setup supervisor

bench setup redis

sudo supervisorctl reload

sudo service supervisor stop all

sudo service supervisor start all

sudo service supervisor restart
