# Deploy

## 1. EC2 instance

1. name 
2. Leave the image as the default Ubunto
3. Leave the instance as t2.micro
4. Key pair
5. network - new security group, ssh traffic, my IP, allow http traffic, edit, security group name: tech242-uyi-allow-SSH-my-IP-HTTP-5000, port for java app default =8080, our custom is 5000  HTTP is 80, add new tcp rule port range 5000 anywhere


## 2. SSH into Instance
1. nano a script - plan

```
    # update & upgrade - everytime you start a new VM
    sudo apt update -y
    sudo apt upgrade -y

    # install maven
    sudo apt-get install --yes maven

    # check maven is installed
    mvn -v

    # install JDK (java) 17
    sudo apt-get install --yes openjdk-17-jdk

    # check JDK 17 is installed
    java -version 
```

2. test manually - issue when upgrading asks a question `sudo apt upgrade -y` so do sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
3. install maven `sudo apt install maven -y` - another issue when installing maven asks for user input `sudo DEBIAN_FRONTEND=noninteractive apt install maven -y`
4. check maven is installed using `mvn -v`
5. install jdk 17 `sudo apt install openjdk-17-jdk -y` 
6. check java is installed `java -version` or `javac -version`
7. to copy code `scp -i "~/.ssh/tech242.pem" -r test ubuntu@ec2-34-243-73-158.eu-west-1.compute.amazonaws.com:~` `scp -i "pem file path" -r {filename} {host}`
8. the code i used to copy the file to my VM: `scp -i "~/.ssh/tech242.pem" -r jsonvoorhees-java-atlas-app ubuntu@ec2-3-252-147-204.eu-west-1.compute.amazonaws.com:~`
9. cd into the app folder
10. cd into springapi
11. `mvn spring-boot:run` in script we do `mvn spring-boot:start` change start for `stop` to stop it
12. test its running using ip and add /{port} at the end username: admin password: password

## my finished script

```
#!/bin/bash
# update & upgrade - everytime you start a new VM
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
git clone https://github.com/UyiAguebor/tech242-jsonvoorhees-java-atlas-app.git
echo "Git Clone Done!"
# cd into the right folder & run the app
cd tech242-jsonvoorhees-java-atlas-app/
echo "cd Into app Done!"
cd springapi/
echo "cd into springapi Done!"
# run the application
mvn spring-boot:start
echo "The App is Running!"
```

## Maven

- maven runs a web server/service to display/render a web page called tom-cat
- tom-cat is then used based on app.properties which port to run the app on
- we are going to install another one called Apache (another is nginx) web server
- we may want to run two web servers as we dont want users to have to put the port instead we want the app to be on the default HTTP port which is port 80 and then be redirected to port 5000
- apache allows for redirections. (setting up a reverse proxy).