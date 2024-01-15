# Jenkins Pipeline

1. git push - trigger
2. gihub recieves the changes
3. github notifies Jenkins 
4. jenkins is listening (github tells when there is a change)

## Pipeline Structure

- Job 1 test code
- Job 2 merge dev
- Job 3 deploy

## Jenkins First Test Pipeline

1. Setup dev branch
2. Generate key pair in Bash terminal
3. Give github repo public key
4. Give jenkins private key
5. Connect to jenkins using EC2 link
6. Create a job
7. Select feelance
8. Select build steps and do execute shell
9. Type a command like `uname -a`
10. To make a pipeline go to Post-build Actions and select build other projects and select an existing project

## Job 1 test CI

1. Description
2. (Check) Discard old build and set the limit to 3
 ![Alt text](../readme-images/job1DiscardGit.png)
3. (Check) Github Project set to your repository link
4. Source Code management Git checked 
 ![Git](../readme-images/job1git.png)
5. Build Steps
 ![Build Steps](../readme-images/Job1BuildSteps.png)
6. Add Trigger

## Job 2

1. github project - with link
    ![Pic](../readme-images/job2git.png)
2. add git repo info - with credentials
3. post build actions - push only if build success
4. post build actions- merge results
5. post build actions - branch to push: main then target remote name: origin

## Job 3
1. source code management git
2. Build environment SSH agent -> add tech242 key
3. build steps: 
```
scp -o StrictHostKeyChecking=no -r ../uyi-jsonvh-job1-ci-test/springapi ubuntu@ec2-18-201-139-56.eu-west-1.compute.amazonaws.com:~
scp -o StrictHostKeyChecking=no -r ../uyi-jsonvh-job1-ci-test/TestResults ubuntu@ec2-18-201-139-56.eu-west-1.compute.amazonaws.com:~
ssh -o StrictHostKeyChecking=no ubuntu@ec2-18-201-139-56.eu-west-1.compute.amazonaws.com "cd ~/springapi; mvn clean package spring-boot:start"
```

