# App Script

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

# install MySQL client
sudo apt-get update -y
sudo DEBIAN_FRONTEND=noninteractive apt-get install mysql-client -y

# connect to MySQL server
#mysql -h 172.31.55.242 -u root -p

# Install Apache
echo "Installing Apache..."
echo ""
sudo DEBIAN_FRONTEND=noninteractive apt install apache2 -y
echo ""
sudo systemctl start apache2
sudo systemctl enable apache2
echo ""

# Enable necessary Apache modules
echo "Enabling Apache modules..."
sudo a2enmod proxy
sudo a2enmod proxy_http
echo ""
echo "Apache modules enabled"
echo ""
sudo systemctl reload apache2

# Create a virtual host configuration file by sed
if grep -q 'ProxyPass / http://localhost:5000/' /etc/apache2/sites-available/000-default.conf; then
    # The string exists, so nothing to do
    echo "Reverse proxy already configured."
else
    # reverse proxy not configured yet
    echo "Configuring reverse proxy..."
    sudo sed -i '/DocumentRoot \/var\/www\/html/ a\ProxyPreserveHost On\nProxyPass \/ http:\/\/localhost:5000\/\nProxyPassReverse \/ http:\/\/localhost:5000\/\n' /etc/apache2/sites-available/000-default.conf
fi
sudo systemctl reload apache2

# copy the app code to this VM
echo "Installing Git..."
echo ""
sudo DEBIAN_FRONTEND=noninteractive apt install git -y
echo ""
echo "Git install complete"
echo ""
rm -rf repo
echo "Cloning repository..."
echo ""
git clone https://github.com/UyiAguebor/tech242-2-tier-deploy-with-world-api.git repo
echo ""
echo "Repository cloned."
echo ""

# cd into correct directory NB. Include in user data from this line when launching instance from AMI
cd repo/WorldProject

# assign environment variables
export DB_HOST=jdbc:mysql://DB_IP:3306/world
export DB_USER=root
export DB_PASS=root

# check connection successful before running the application
echo "Running app..."
echo ""

output=$(mysql -h DB_IP -u root -proot -e "USE world; SELECT 1" 2>&1)

if [ $? -eq 0 ]; then
    echo "Connected Successfully"
    mvn clean package spring-boot:start
else
    echo "Failed to connect"
    echo $output
fi

echo ""
echo "App running successfully"
```