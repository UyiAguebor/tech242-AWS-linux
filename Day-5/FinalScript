# Final Working Script of app with Reverse-Proxy

```
#!/bin/bash
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
rm -rf repo
git clone https://github.com/UyiAguebor/tech242-jsonvoorhees-java-atlas-app.git repo
echo "Git Clone Done!"

cd repo/
echo "cd Into app Done!"

cd springapi/
echo "cd into springapi Done!"

# run the application
sudo mvn spring-boot:start
echo "The App is Running!"

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

```
#!/bin/bash
cd repo/springapi
mvn spring-boot:start
```