# My Database VM Script

```
#!/bin/bash

cd
echo ""
echo "Updating..."
echo ""
sudo apt update -y
echo ""
echo "Update complete"
echo ""
echo "Upgrading..."
echo ""
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
echo ""
echo "Upgrade complete"
echo ""

# install maven
echo "Installing Maven..."
echo ""
sudo DEBIAN_FRONTEND=noninteractive apt install maven -y
echo ""
echo "Maven install complete"
echo ""

# check maven is installed
echo "Confirming Maven installation..."
mvn -v
echo "Maven installation confirmed"
echo ""

# install JDK (java) 17
echo "Installing JDK Java 17..."
echo ""
sudo DEBIAN_FRONTEND=noninteractive apt install openjdk-17-jdk -y
echo ""
echo "JDK Java 17 complete"
echo ""

# check JDK 17 is installed
echo "Confirming Java installation..."
java -version
echo "Java installation confirmed"
echo ""

# update password and install MySQL
sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password password root'
sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password root'
sudo apt-get update -y
sudo DEBIAN_FRONTEND=noninteractive apt-get install mysql-server -y

# start the MySQL service
sudo service mysql start

# ensure MySQL starts automatically at boot
sudo systemctl enable mysql

# download world.sql from repo
rm world.sql
wget https://raw.githubusercontent.com/FThompsonSG/world_api_app_code/main/world.sql

# run world.app
mysql -u root -proot < world.sql

# configure MySQL to accept any connections with root username and root password
sudo sed -i 's/^bind-address\s*=\s*127\.0\.0\.1/bind-address = 0.0.0.0/' /etc/mysql/mysql.conf.d/mysqld.cnf
sudo service mysql restart

# create root user and grant privileges if one does not already exist
sudo mysql -u root -proot -e "CREATE USER IF NOT EXISTS 'root'@'%' IDENTIFIED BY 'root'; GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION; FLUSH PRIVILEGES;"
```