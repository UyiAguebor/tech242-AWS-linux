# Scripts Modified for User Data

```
#!/bin/bash
cd
sudo apt update -y
echo "Update Done!"
echo ""
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
echo "Update Done!"
echo ""
# install maven
sudo DEBIAN_FRONTEND=noninteractive apt install maven -y
echo "Maven Install Done!"
echo ""
# check maven is installed
mvn -v
echo "Maven Check Done!"
echo ""
# install JDK (java) 17
sudo DEBIAN_FRONTEND=noninteractive apt install openjdk-17-jdk -y
echo "Installed JDK Done!"
echo ""
# check JDK 17 is installed
java -version
echo "Java Check Done!"
echo ""
# copy the app code to this VM
sudo DEBIAN_FRONTEND=noninteractive apt install git -y
echo "Git Install Done!"
rm -rf repo
git clone https://github.com/UyiAguebor/tech242-jsonvoorhees-java-atlas-app.git repo
echo "Git Clone Done!"
# cd into the right folder & run the app
cd repo/
echo "cd Into app Done!"
cd springapi/
echo "cd into springapi Done!"
# run the application
mvn spring-boot:start
echo "The App is Running!"

# Define variables
DOMAIN="52.51.232.194" - change
TARGET_IP="52.51.232.194" - change
TARGET_PORT="5000" - change

# Install Apache
sudo DEBIAN_FRONTEND=noninteractive apt install apache2 -y

# Enable necessary Apache modules
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod proxy_balancer
sudo a2enmod lbmethod_byrequests

# Create a virtual host configuration file
sudo tee /etc/apache2/sites-available/reverse-proxy.conf > /dev/null <<EOL
<VirtualHost *:80>
    ServerName $DOMAIN

    ProxyPreserveHost On
    ProxyPass / http://$TARGET_IP:$TARGET_PORT/
    ProxyPassReverse / http://$TARGET_IP:$TARGET_PORT/

    ErrorLog \${APACHE_LOG_DIR}/error.log
    CustomLog \${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
EOL

# Enable the virtual host
sudo a2ensite reverse-proxy

# Reload Apache to apply changes
sudo systemctl reload apache2
```

# Another test

```
#!/bin/bash
cd
sudo apt update -y
echo "Update Done!"
echo ""
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
echo "Update Done!"
echo ""
# install maven
sudo DEBIAN_FRONTEND=noninteractive apt install maven -y
echo "Maven Install Done!"
echo ""
# check maven is installed
mvn -v
echo "Maven Check Done!"
echo ""
# install JDK (java) 17
sudo DEBIAN_FRONTEND=noninteractive apt install openjdk-17-jdk -y
echo "Installed JDK Done!"
echo ""
# check JDK 17 is installed
java -version
echo "Java Check Done!"
echo ""
# copy the app code to this VM
sudo DEBIAN_FRONTEND=noninteractive apt install git -y
echo "Git Install Done!"
rm -rf repo
git clone https://github.com/UyiAguebor/tech242-jsonvoorhees-java-atlas-app.git repo
echo "Git Clone Done!"
# cd into the right folder & run the app
cd repo/
echo "cd Into app Done!"
cd springapi/
echo "cd into springapi Done!"
# run the application
mvn spring-boot:start
echo "The App is Running!"

# Define variables
DOMAIN=$(curl ifconfig.me)
TARGET_IP=$(curl ifconfig.me)
TARGET_PORT="5000"

# Install Apache
sudo DEBIAN_FRONTEND=noninteractive apt install apache2 -y

# Enable necessary Apache modules
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod proxy_balancer
sudo a2enmod lbmethod_byrequests

# Create a virtual host configuration file
sudo tee /etc/apache2/sites-available/reverse-proxy.conf > /dev/null <<EOL
<VirtualHost *:80>
    ServerName $DOMAIN

    ProxyPreserveHost On
    ProxyPass / http://$TARGET_IP:$TARGET_PORT/
    ProxyPassReverse / http://$TARGET_IP:$TARGET_PORT/

    ErrorLog \${APACHE_LOG_DIR}/error.log
    CustomLog \${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
EOL

# Enable the virtual host
sudo a2ensite reverse-proxy

# Reload Apache to apply changes
sudo systemctl reload apache2
```

freya version
```
#!/bin/bash
cd
sudo apt update -y
echo "Update Done!"
echo ""
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
echo "Update Done!"
echo ""
# install maven
sudo DEBIAN_FRONTEND=noninteractive apt install maven -y
echo "Maven Install Done!"
echo ""
# check maven is installed
mvn -v
echo "Maven Check Done!"
echo ""
# install JDK (java) 17
sudo DEBIAN_FRONTEND=noninteractive apt install openjdk-17-jdk -y
echo "Installed JDK Done!"
echo ""
# check JDK 17 is installed
java -version
echo "Java Check Done!"
echo ""
# copy the app code to this VM
sudo DEBIAN_FRONTEND=noninteractive apt install git -y
echo "Git Install Done!"
rm -rf repo
git clone https://github.com/UyiAguebor/tech242-jsonvoorhees-java-atlas-app.git repo
echo "Git Clone Done!"

# Install Apache
sudo DEBIAN_FRONTEND=noninteractive apt install apache2 -y

sudo systemctl start apache2
sudo systemctl enable apache2

# Enable necessary Apache modules
sudo a2enmod proxy
sudo a2enmod proxy_http

# Create a virtual host configuration file
VHOST_CONF="/etc/apache2/sites-available/000-default.conf"
cat <<EOF | sudo tee "$VHOST_CONF"> /dev/null
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/ww/html

    ProxyPreserveHost On
    ProxyPass / http://localhost:5000/
    ProxyPassReverse / http://localhost:5000/

    ErrorLog \${APACHE_LOG_DIR}/error.log
    CustomLog \${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
EOF
sudo systemctl restart apache2

cd repo/
echo "cd Into app Done!"
cd springapi/
echo "cd into springapi Done!"
# run the application
mvn spring-boot:start
echo "The App is Running!"
```

Ramon
```
#!/bin/bash
cd home/ubuntu
sudo apt update -y
echo "Update Done!"
echo ""
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
echo "Update Done!"
echo ""
# install maven
sudo DEBIAN_FRONTEND=noninteractive apt install maven -y
echo "Maven Install Done!"
echo ""
# check maven is installed
mvn -v
echo "Maven Check Done!"
echo ""
# install JDK (java) 17
sudo DEBIAN_FRONTEND=noninteractive apt install openjdk-17-jdk -y
echo "Installed JDK Done!"
echo ""
# check JDK 17 is installed
java -version
echo "Java Check Done!"
echo ""
# copy the app code to this VM
sudo DEBIAN_FRONTEND=noninteractive apt install git -y
echo "Git Install Done!"
rm -rf repo
git clone https://github.com/UyiAguebor/tech242-jsonvoorhees-java-atlas-app.git repo
echo "Git Clone Done!"

sudo DEBIAN_FRONTEND=noninteractive apt install apache2 -y

sudo systemctl start apache2
sudo systemctl enable apache2

sudo a2enmod proxy
sudo a2enmod proxy_http

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

sudo systemctl reload apache2

cd repo/
echo "cd Into app Done!"
cd springapi/
echo "cd into springapi Done!"
# run the application
mvn spring-boot:start
echo "The App is Running!"
```

sudo nano /etc/apache2/sites-available/000-default.conf

# paste these in 
    ProxyPreserveHost On
    ProxyPass / http://localhost:5000/
    ProxyPassReverse / http://localhost:5000/


new

```
#!/bin/bash
cd
sudo apt update -y
echo "Update Done!"
echo ""
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
echo "Update Done!"
echo ""
# install maven
sudo DEBIAN_FRONTEND=noninteractive apt install maven -y
echo "Maven Install Done!"
echo ""
# check maven is installed
mvn -v
echo "Maven Check Done!"
echo ""
# install JDK (java) 17
sudo DEBIAN_FRONTEND=noninteractive apt install openjdk-17-jdk -y
echo "Installed JDK Done!"
echo ""
# check JDK 17 is installed
java -version
echo "Java Check Done!"
echo ""
# copy the app code to this VM
sudo DEBIAN_FRONTEND=noninteractive apt install git -y
echo "Git Install Done!"
rm -rf repo
git clone https://github.com/UyiAguebor/tech242-jsonvoorhees-java-atlas-app.git repo
echo "Git Clone Done!"

sudo DEBIAN_FRONTEND=noninteractive apt install apache2 -y

sudo systemctl start apache2
sudo systemctl enable apache2

sudo a2enmod proxy
sudo a2enmod proxy_http

if grep -q 'your-search-string' /etc/apache2/sites-available/000-default.conf; then
    # The string exists, so nothing to do
    echo "Reverse proxy already configured."
else
    # reverse proxy not configured yet
    sed -i 'N a\your line of code' /etc/apache2/sites-available/000-default.conf
fi

sudo systemctl reload apache2

cd repo/
echo "cd Into app Done!"
cd springapi/
echo "cd into springapi Done!"
# run the application
mvn spring-boot:start
echo "The App is Running!"
```