# My Database VM Script

```
#!/bin/bash

# Update VM
sudo apt update -y
echo "Update Done!"
echo ""

# Upgrade VM
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
echo "Update Done!"
echo ""

# install MySQL

sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password password root'
sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password root'
sudo apt-get update
sudo DEBIAN_FRONTEND=noninteractive apt install -y mysql-server
sudo systemctl enable mysql-server

# Download world.sql from repo
rm world.sql
wget https://raw.githubusercontent.com/UyiAguebor/tech242-2-tier-deploy-with-world-api/main/world.sql
echo "script downloaded"

sudo chmod +x world.sql

# MySQL connection parameters
export DB_USER="root"
export DB_PASS="root"
DB_NAME="world"
SQL_FILE="/home/ubuntu/world.sql"

# Import the dump file (create the 'world' database if it doesn't exist)
mysql -u"${DB_USER}" -p"${DB_PASS}" < "${SQL_FILE}"

echo "Script Finished Running!"

# config MySQL to allow any request
sudo sed -i 's/^bind-address\s*=.*$/bind-address = 0.0.0.0/' /etc/mysql/mysql.conf.d/mysqld.cnf
sudo systemctl restart mysql
echo "MySQL configured"
```
