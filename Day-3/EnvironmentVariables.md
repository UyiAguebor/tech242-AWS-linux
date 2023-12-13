# Environment Variables - used to store sensative information

### A variable is something you want to store in memory

## We want to be able to store our own environment variables

- we want our applications to look for the senative information 
- in production you would never hard code credentials


## Store a variable in our VM

`MYNAME=uyi `

## create Environment Variable

`export MYNAME=Uyi`

## What happens when you log out?

### If you LOG OUT your Environment variable is not persistant across all sessions so it will be lost when you log back in.

if we want to change this

`ls -a`

we see the file: .bashrc

### edit the file using this command:

`nano .bashrc`

then we add export MYNAME=Uyi_is_persistant to the bottom of the file.

