# Auto Scaling

AWS horizontal: More of the same Virtual Machine.

AWS Vertical: increase the size of the Virtual Machine.

![ASG Diagram](../readme-images/ASG.png)

# Steps

1. Start with VM
2. Create AMI(Copy of the disk)
3. Launch Template
4. Auto Scaling Group
   1. Scaling policy
   2. Metrics to use (Average CPU utilization)
5. Create the VMs (in Ireland Region but different availability zones)

load balancer and target group need to be deleted after ASG also

### 1. Create Auto Scaling Group

![Step 1](../readme-images/asg1.png)

- Description: Set the name for your Auto Scaling Group

### 2. Setup Availability Zones

![Step 2](../readme-images/asg2.png)

- Description: Set up availability zones in 1a, 1b, 1c (DevOpsStudent default)

### 3. Add a Load Balancer

![Step 3](../readme-images/asg3.png)

- Description: Application load balancer and make it Internet Facing

### 4. Turn on Elastic Load Balancing

![Step 4](../readme-images/asg4.png)

- Description: Allow for Elastic load balancing 

### 5. Instance Scaling

![Step 5](../readme-images/asg5.png)

- Description: Define how many instances is you desired, minimun and maximum

### 6. Scaling Policy

![Step 6](../readme-images/asg6-5.png)

- Description: Set your Scaling policy

### 7. Notifications

![Step 7](../readme-images/asg7.png)

- Description: For this case I left the notifications blank

### 8. Add tags

![Step 8](../readme-images/asg8.png)

- Description: Add additional tag