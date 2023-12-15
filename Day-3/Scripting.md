# <u>Making and running Scripts</u>

## Creating a script file

Script files in bash end with .sh 

Commands

`nano provision.sh`

"#" in a script means its a comment and the string will be ignored

At top of every bash script we have to tell linux which type of shell interpreter to use to process the command shabang "#!/bin/bash"

## My provision.sh script

### <u>What it contains</u>

```
# update - goes and gets new packages |  "sudo apt update -y"

# upgrade - goes and installs new packages in os could break your instance | "sudo apt upgrade -y"

# install nginx - "sudo apt install nginx -y" check its running "sudo systemctl status nginx"

# restart nginx - you restart a service if youve made changes to it. "sudo systemctl restart nginx"

# enable nginx - enabled means service will run on startup "sudo systemctl enable nginx"
```

Test everything manually one command at a time to know if the command works manually before we try to automate

if you want to test if your vm has access to the internet try the "sudo apt update" command

## How to run a script

when you want to run a script you need to provide the path to the script

`./provision.sh` permission denied

so we do
`sudo chmod +x provision.sh` to give ourselves the execute permission for the file.

if we want to make the process clearer we use

`echo ""` in order to display what is going on when we run the script

rwx  - read write execute

### When running a script

- You want your script to run on a fresh VM.(testable)
- Idempotency meaning when we run a script that it runs the first time but also runs no matter how many times you run it AND wont mess things up. If it does its idempotent.