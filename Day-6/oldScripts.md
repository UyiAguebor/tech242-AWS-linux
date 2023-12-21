## Old DB Script
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
export DB_USER=root
export DB_PASS=root
DB_NAME=world
SQL_FILE=world.sql

# Import the dump file (create the 'world' database if it doesn't exist)
mysql -u"${DB_USER}" -p"${DB_PASS}" < "${SQL_FILE}"
sudo mysql -u root -proot -e "CREATE USER root@'%' IDENTIFIED BY 'root'; GRANT ALL PRIVILEGES ON *.* TO root@'%' WITH GRANT OPTION; FLUSH PRIVILEGES;"

echo "Script Finished Running!"

# config MySQL to allow any request
sudo sed -i 's/^bind-address\s*=.*$/bind-address = 0.0.0.0/' /etc/mysql/mysql.conf.d/mysqld.cnf
sudo systemctl restart mysql
echo "MySQL configured"
```

## Old App Script

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

# install maven
sudo DEBIAN_FRONTEND=noninteractive apt install maven -y
echo "Maven Install Done!"
echo ""

# install JDK (java) 17
sudo DEBIAN_FRONTEND=noninteractive apt install openjdk-17-jdk -y
echo "Installed JDK Done!"
echo ""

# Set environment variables
export DB_USER=root
export DB_PASS=root
export DB_HOST=jdbc:mysql://172.31.57.38:3306/world
export DB_PORT=3306

# clone from git
rm -rf repo
git clone https://github.com/UyiAguebor/tech242-2-tier-deploy-with-world-api.git repo

# recompile the app
cd repo
cd WorldProject

output=$(mysql -h 172.31.54.27 -u root -proot -e "USE world; SELECT 1" 2>&1)

if [ $? -eq 0 ]; then
    echo "Connected Successfully"
    mvn clean package spring-boot:start
else
    echo "Failed to connect"
    echo $output
fi

# Install apache2
sudo DEBIAN_FRONTEND=noninteractive apt install apache2 -y

# Start apache and enable
sudo systemctl start apache2
sudo systemctl enable apache2

# Enable mods
sudo a2enmod proxy
sudo a2enmod proxy_http

# Change file
file="/etc/apache2/sites-available/000-default.conf"
lines_to_add="\
    ProxyPreserveHost On\n\
    ProxyPass / http://localhost:5000/\n\
    ProxyPassReverse / http://localhost:5000/"

if grep -qF "$lines_to_add" "$file"; then
    echo "Lines are already present in the file."
else
    if grep -q "DocumentRoot /var/www/html" "$file"; then
        sudo sed -i -e "/DocumentRoot \/var\/www\/html/a $lines_to_add" "$file"
        echo "Lines added after DocumentRoot."
    else
        echo "DocumentRoot line not found in the file."
    fi
fi

# Restart apache2
sudo systemctl reload apache2
```

sudo chmod +x prov.sh