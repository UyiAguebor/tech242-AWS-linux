# How to make an AMI

## 1. Go to a working instance of your VM
## 2. Actions
## 3. Images...
## 4. Create Image
   ### 1. Add name, desc, tag.
## 5. Create an instance from the AMI with user data:
   ### 1. #!/bin/bash
   ### 2. cd repo/springapi
   ### 3. mvn spring-boot:start

## to remove an ami 
1. deregister it
2. delete the snapshot 

#!/bin/bash
cd repo/springapi
mvn spring-boot:start