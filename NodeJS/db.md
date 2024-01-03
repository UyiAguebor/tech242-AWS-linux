
# Update and Upgrade

sudo apt update -y
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y

sudo apt-get install -y mongodb-org=3.2.22 mongodb-org-server=3.2.22 mongodb-org-shell=3.2.22 mongodb-org-mon$

sudo service mongod start

sudo nano /etc/mongod.conf

sudo service mongod restart

sudo service mongod status

sudo systemctl enable mongod






