# Levels of Automation

1. Manually test - you need to .ssh
2. Script - you need to .ssh
3. User data -  is a palce in the advanced setting in an EC2 instance where you can put you script without .ssh
   1. it runs as the root user - every command you run will have super user permission.
   2. it runs at the root directory
   3. worry about where to clone your repo `git clone {link} {folder to place it in}` it will clone to the root directory
4. Amazon Machine Image - Make it faster with an AMI - We can create our own AMI's
   1.  Snapshot of your disk