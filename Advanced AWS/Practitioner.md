# Revision notes

## Types of Cloud Computing

- On premises -> You manage Apps, Data, Runtime, Middleware, O/S, Virtualization, Servers, Storage, Networking.
- IaaS -> You manage Apps, Data, Runtime, Middleware, O/S.
- PaaS -> You manage Apps, Data.
- SaaS -> You manage nothing.

## factors that may affect your choice of aws region 

- compliance with data governance and legal requirements -> data doesnt leave a region without your explicit permission
- proximity to customers: reduce latency
- available services within a region new services arent available in every region
- Pricing varies region to region


## AWS Availability Zones(AZ)

- each region has 3-6 availability zones
- each AZ is one or more discrete data centers with redundant power, networking and connectivity
- they are sperate from eachother so isolated from disasters
- they are ocnnected with high bandwidth ultra low latency networking

## AWS Points of Presence (Edge Locations)

Amazon has 400+ Points of Presence in 90+ cities accross 40+ countries
Content is delivered to end users with lower latency

## Identity and Access Management
### IAM: Users & Groups

Users or Groups can be assigned JSON documents called policies

Policy Structure:

- Version   -> policy language version
- Id        -> an identifier for ther policy (optional)
- Statement -> one or more individual statements(required)
  - Sid       -> an identifier for the statement(optional)
  - Effect    -> whether the statment allows or denies access(Allow,Deny)
  - Principle -> account/user/role to which the policy is applied to
  - Action    -> list of actions this policy allows or denies
  - Resource  -> List of resources to which the actions applied to

Password policy:

- Strong Passwords = higher security for your account
- In AWS, you can setup a password policy


### IAM MFA(Multi Factor Authentication)

MFA = password you know + security device you own

even when password is stolen or hacked the account is not comprimised

MFA devices options in AWS

- Virtual MFA device(Google Authenticator,Authy)
- Universal 2nd Factor (U2F) Security key
- Hardware Key Fob MFA Device
- Hardware KeyFob MFA Device for AWS GovCloud US

### IAM Roles for Services

some AWS services will need you to perform actions on your behalf

To do so, we will assign permissions to AWS services with IAM Roles

### IAM Security Tools

IAM Credentials Report(account level) -> a report hat lists all your accounts users and the status of their various credentials

IAM Access Advisor(user level) -> Access advisor shows the service permissions granted to a user and when those services were last accessed

### Best Practices

- Dont use the root account except for AWS account setupo
- One physical uyser = One AWS user
- Assign users to groups and assign permissions to groups
- Strong password policy
- Use and enforce the use of MFA
- Create and use Roles for giving permissions to AWS services
- Use Access Keeys for programmatic Access(CLI/SDK)
- Audit permissions of your account using IAM Credentials Report & IAM Access Advisor
- Never share IAM users & Access Keys

### Shared Responsibility Model for IAM

AWS Responsibilities:
- Infrastructure
- Config and vulnerability analysis
- Complaince Validation

Your Responsibilities:
- Policy managementy and monitoring
- Enable MFA on all accounts
- Rotate all your keys often
- Use IAM tools to apply appropriate permissions
- Analyse access patterns and review permissions

## Elastic Compute Cloud (EC2)

m5.2xlarge

m -> instance class
5 -> generation (AWS improves them over time)
2xlarge -> size within the instance class

8 different types of EC2 instances:

- General Purpose -> greate for diversity of workload + balance between compute, memory and networking(t2.micro)
- Compute optimized -> high performance processors great for compute intensive tasks: dedicated gaming servers machine learning (C name)
- Memory Optimized -> fast performance for workloads that process large data sets in memory: database (R name)
- Storage Optimized -> great for storage-intensive tasks that require high, sequential read and write access to large data sets on local storage (I, D, H1)

### Security Groups

Ports

22 = SSH (secure shell) - log into Linux instance
21 = FTP (File Transfer Protocol) - upload files into a file share
22 = SFTP (Secure File Transfer Protocol) - upload files using SSH
80 = HTTP - access unsecured websites
443 =  HTTPS - access secured websites
3389 = RDP (Remote Desktop Protocol) - log into a Windows instance

### IAM Roles

Instead of inputing aws credentials through `aws configure` instead we can asign the EC2 an IAM Role so it can perform tasks on AWS

### Purchasing Options

#### On-Demand instances

pay for what you use -> linux, windows per second after first min/ other OS billed per hour
highest cost but no upfront payment
no long term commitment

recommended for short term and un-interrupted workloads

short workload, predictable pricing pay by second

#### Reserved (1 & 3 years) 

72% discount compared to on-demand
reserve a specific instance attribute(Instance Type, Region, Tenancy, OS)

- reserved instances - long workloads
- Convertible Reserved instances - long workloads with flexible instances

#### Savings Plans (1 & 3 years)

Discount on long term usage(72%)
commit to a certain type of usage($10/hour for 1 or 3 years)
usage beyond this is billed at On-Demand price

- commitment to an amount of usage, long workload

#### Spot Instances 

discount 90% compared to On-Demand
can lose instance at any time if your max price is less than the current spot price
useful for workloads that are resilient to failure
not suitable for critical jobs or databases
- short workloads, cheap, can lose instances (less reliable)

#### Dedicated Hosts 

A physical server with EC2 insance capacity fully dedicated to your use
- book an entire physical server, control instance placement

#### Dedicated instances

Instance runs on hardware thats dedicated to you
May share hardware with other instances in same account

#### Capacity Reservations 
reserve On-Demand instances capacity in a specific AZ for any duration
you always have access to EC2 capacity when you need it
no time commitment
- reserve capacity in a specific AZ for any duration


### Shared Responsibility

AWS
- infrastructure
- isolation on physcial hosts
- replacing faulty hardware
- Compliance validation

Yours
security group rules
Operating-system patches and updates
software and utilities installed on the EC2 instance
IAM Roles and IAM user access management
Data security on your instance

### Elastic Block Store (EBS)

An EBS Volume is a network drive you can attach to your instances while they run

It allows your instances to persist data, even after their termination

They can only be mounted to one instance at a time

Locked to an Availability Zone

Have a provisioned capacity

Delete on termination (ticked for a new volume)

#### EBS Snapshot

Backup of EBS Volume at a point in time
Can copy snapshots across AZ or Region

EBS Snapshot Archive tier is 75% cheaper
Snapshot Recycle bin (specify retention)

### Amazon Machine Image

Public AMI
Your own AMI
An AWS Marketplace AMI

### EC2 Image Builder

Used to automate the creation of Virtual Machines or container images

EC2 Image builder -> create Builder EC2 instance -> Build Components applied -> New AMI -> Test EC2 Instance -> Test suite is ran -> AMI is distributed

### EC2 Instance Store

EBS volumes are network drives with good but limited performance
If you need high-performanmce hardware disk, use EC2 instance Store

Better I/O performance
EC2 Instance Store lose thier storage if theyre stopped
Good for buffer/cache/scratch data
Risk of data loss if hardware fails
Backup is your responsibility

### Elastic File System (EFS)

Managed Network File System (NFS) that can be mounted on 100s of EC2
EFS works with Linux EC2 instances in multi-AZ
Highly available, scalable, expensive

#### EFS Infrequent Access (EFS-IA)

Storage class that is cost optimized for files not accessed everyday
up to 92% lower cost compared to EFS standard

### Shared Responsibility

AWS
Infrastructure
Replication for data for EBS volumes
Replacing faulty hardware
Ensuring their employees cannot access your data

Your
Backup
Setting up data encryption
Responsibility of any data on the drives
Understanding the risk of using EC2 Instance Store

### FSx

Launch 3rd party high-performace file system

#### Amazon FSx for Windows File Server

A fully managed, highly reliable, and scalable windows native shared file system.

#### Amazon FSx for Lustere

A fully managed, high-performance, scalable file storage for High Performace Computing (HPC)


## Elastic Load Balancing (ELB) & Auto Scaling Groups (ASG)

Vertical Scability -> bigger VM -> t2.nano to u-12tb1
Horizontal Scalability -> More of the same VM -> ASG + Load Balancer

High Availability -> running application in atleast 2 Availability Zones

Scalability: ability to accomodate a larger load by making the hardware stronger or by adding nodes

Elasticity: once a system is scalable, elasticity means that ther will be some "auto-scaling" so that the system can scale based on the load

Agility: new IT resources are only a click away which means that you reduce the time to make those resources available to your developers from weeks to just minuites

### ELB

load balancers are servers that forward internet traffic to multiple servers downstream

4 Types of Load Balancers:

Application Load Balancer (HTTP/HTTPS/gRCP only) static DNS (URL) - layer 7 
Network Load Balancer - ultra high performance allows TCP/UDP Static IP- layer 4
Gateway Load Balancer - Route Traffic to firewalls on EC2 Intrusion detection - layer 3
Classing Load Balancer - layer 4 & 7

### ASG

Scaling Strategies

Manual Scaling: Update the size of an ASG manually

Dynamic Scaling: Respond to changing demand -> simple scaling = CPU CloudWatch trigger OR target Tracking Scaling i want avg CPU usage to be 50% OR Scheduled Scaling OR Predictive Scaling ML

## S3

Simple Storage Service

Stores Objects (files) in Buckets must be unique globally -> no uppercase 3-63

Objects have a key
the key is the full path to the file

### S3 Security

User based -> IAM Policies

- Resource-Based
  - Bucket policies -> JSON based policies
  - Object Access Control List (ACL)

### S3 Versioning

you can version your files in Amazon S3
Same key overwrite will change the version

### S3 Replication

enable versioning
Cross-Region Replication (CRR)
Same-Region Replicatrion (SRR)

### S3 Durability and Availability

High Durability (11 9's): if you store 10,000,000 you can expect to lose a single object once every 10,000 years

Availability: Measures how readilty avaialble a service is

Varies depending on storage class

Example: S3 Standard has 99.99% availability =  not available 53 minutes a year

### S3 Storage Classes

- S3 Standard - General Purpose
  - 99.99% Availability
  - Used for frequent data access
  - low latencyt and high thgroughput
  - Sustain 2 concurrent facility failures
  - Use: Big data analytics, mobile & gamning applications

- S3 Infrequent Access
  - For data that is less frequently accessed but requires rapid access when needed
  - Lower cost than S3 Standard
  - Standard-IA 99.9% availability Use: Disaster Recovery, Backups
  - One Zone-IA High durability (11 9's) 99.5% Availability Use: Storing backup copies of on-premise data

- S3 Glacier Storage
  - Low-cost object storage meant for archiving / backup
  - Pricing: price for storage + object retrieval cost
  - S3 Glacier Instant Retrieval millisecond retrieval minimum storage duration of 90 days
  - S3 Glacier Flexible Retrieval -> Expidited (1 to 5 mins), Standard (3 to 5 hours), Bulk (5 to 12) - free min storage duration 90 days
  - S3 Glacioer Deep Dive Archive - for long term storage: standard (12 hours), Bulk(48 hours) Minimum storage duration of 180 days

- S3 Intelligent - Tiering
  - Small monthly monitoring and auto-tiering fee
  - moves objects automatically between Access Tiers based on usage
  - No retrieval charges
  - Frequent Access tier (automatic): default tier
  - Infrequent Access tier (automatic): objects not accessedf for 30 days
  - Archive instance Access tier (automatic): objects not accessed for 90 days
  - Archive Access tier (optional): configurable from 90 days to 700+ days
  - Deep Archive Access tier (optional): confi. from 180 days to 700+ days

### S3 Encryption

Server-Side Encryption (default) -> server Encrypts the file after receiving it

Client-Side Encyption -> Encrypts the file before uploading it

### S3 Access Analyzer for S3

Ensures that only intended people have access to hour S3 buckets
Powered by IAM Access Analyzer

### Shared Responsibility S3

AWS
- infrastructure
- config
- Compliance validation

You
- S3 Versioning
- S3 Bucket Policies
- S3 Replication Setup
- Logging and Monitoring
- S3 Storage Classes
- Data Encryption at rest and in transit

### AWS Snow Family

Highly-secure portable devices to collect and process data at the edge and migrate data into and out of AWS

Data migration: Snowcone, Snowball Edge, Snowmobile

Edge computing: Snowcone, Snowball Edge

if upload too large use aws snowball send back to aws to import/export to AWS S3 bucket

Devices

- Snowball Edge -> used to move TB or PB of data in or out of AWS
  - Storage Optimized -> 80 TB of HDD
  - Compute optimized -> 42 TB of HDD

- AWS Snowcone & Snowcone SSD
  - small portable computing 
  - used for edge computing storage and data
  - Snowcone: 8 TB of HDD Storage
  - Snowcone SSD: 14 TB of SSD Storage
  - must provide your own battery
  - send back by shipping or connect it to internet and used AWS DataSync

- AWS Snowmobile
  - Transfer exabytes of data 
  - Each snowmobile has 100 PB of capacity

#### Edge Computing

process data while its being created on an edge location
no internet
no easy access to computing power

Use: 
preprocess data
machine learning at the edge
Transcodeing media streams

Snowcone & Snowcone SSD (2cpu 4gb)
Snowball Edge Compute optimized (10vcpu optional gpu)
Snowball Edge Storage Optimized ( 40 vcpu)

All can run EC2 instances 

AWS OpsHub to manage your snow family device

#### Pricing

- Snowball Edge Pricing
  - Pay for usage and data transfer out of aws
  - Data transfer in S3 is free
  - On Demand one time fee for 10 days or 15 days of usage
  - Commited upfront pay in advance for monthly 1-year and 3-years of usage up to 62% discounted pricing

### Hybrid Cloud for Storage

AWS is pushing for "hybrid cloud"
part of your infra is on premesis
part fo your infra is 

### AWS Storage Gateway

Hybrid solution to bridge between on-premise data and cloud data in S3

## Databases & Analytics

databases allow you to stucture the data you store

Relational Database -> links between them SQL

NoSQL Databases -> built for specific data models and have flexible schemas for building modern applications
  - JSON is a data example
  - fields can change over time

AWS offers use to manage different databases
  - Quick provisioning, High Availability, Virtical and Horizontal Scaling
  - Automated Bakcup & Restore, Operations, Upgrades
  - Operating System Patching is handled by AWS
  - Monitoring, alerting


### AWS RDS & Aurora

#### RDS

RDS stands for Relational Database Service (SQL) aws managed -> Allows you to create databases in the cloud that are managed by AWS (postgres,mysql)

RDS Deployments: 
- Read Replicas
  - Scale the read workload of your DB
  - can create up to 15 Read Replicas
  - Data is only written to the main DB
- Multi-AZ
  - failover in case of AZ outage
  - can only have 1 AZ as a failover
- Multi-Region
  -  Multi-Region Read Replicas but only write to one main DB
  -  better performance

#### Aurora

Aurora is a proprietary technology from AWS (not open sourced) (PostgreSQL & MySQL)
  - AWS cloud optimized and claims 5x performance improvment over MySQL on RDS and 3x performance of Postgres on RDS
  - Automatically grows in increments of 10GB
  - Cost More than RDS
  - Not in the free tier

Aurora Serverless
  -  Automated database instantiation and auto-scaling based on actual usage
  -  PostgreSQL & MySQL supported
  -  Pay per second cost effective
  -  good for unpredictable workloads

### ElastiCache

ElastiCache is to get managed Redis or Memcached

Caches are in-memory databases with high performance, low latency

Helps reduce load off databases for read intensive workloads

### DynamoDB

Fully managed highly available with replication across 3 AZ

NoSQL database - not a relational database

Scales to massive workloads distributed "serverless" database

Millions of requests per seconds, trillions of row, 100s of TB of storage

Single digit millisecond latency - low latyency retrieval

DynamoDB Accelerator - DAX fully managed in memory cache for DynamoDB 10X Performance

#### Global Tables

Make a DynamoDB table accessible with low latency in multiple-regions

Active-Active replication (read/write to any AWS Region)

### Redshift

Redshift is based on PostgreSQL but its not used for Online Transactual Processing

Online analytical processing (analytics and data warehousing)

Columnar storage of data

#### Serverless

Automatically provisions and scales data warehouse underlying capacity

### Amazon Elastic MapReduce (EMR)

Hadoop clusters(Big Data) to analyse and process vast amount of data
clusters made up of hundreds of EC2 instances
Use Cases data processing machine learning, web indexing

### Amazon Athena

Serverless and perform analytics against S3 objects

### Amazon QuickSight

Serverless machine learning service to create interactive dashboards

### DocumentDB

DocumentDB is AWS's implementation of MongoDB (NoSQL)

### Neptune

Fully managed graph database
A popular graph dataset

### Timnestream

time series database

### Quantum Ledger Database(QLDB)

recording financial transactions
used tyo review history of all the changes made to your application
immutable system
No concept of decentralization

### Managed Blockchain

Blockchain makes it possible to build applications where mutiple parties can execute transactions without the need for a trusted, central authority

decentralized

### AWS Glue

Managed Extract, transform and load service

serverless

### Database Migration Service

allows you to migrate from one database to another

## Other Compute Services
### Docker

Apps packaged in container so it can run the same regardless of where theyre ran

### ECS

Elastic Container Service -> Launch Docker containers on AWS

you must provision and maintain the infrastructure

### Fargate

Launch docker containers on AWS
you do not need to provision the infractructure + serverless offering

### ECR

Elastic Container Registry

this is where you store your Docker images

### Serverless

paradigm in which developers dont have to manage servers anymore they just deploy their applications and dont manage or provision the servers

there are still servers in the background

### Lambda

Virtual Functions - no servers to manage -> function as a service

short executions

run on demand

event driven

Pricing
- pay per calls
- pay per duration
- cheap

### Amazon API Gateway

REST API

Allows devs to easily manage and create secure APIs

Serverless and scalable

### AWS Batch

fully managed batch processing at any scale

Dynamically launch EC2 instances or Spot instances

defined as Docker imaged and run on ECS

### Amazon Lightsail

virtual servers storage databases and networking

great for people with little cloud experience

low price

## Deployments & managing Infrastructure at scale
### Cloud Formation

Declarative way of outlining your AWS infrastructure for any resources.

right order and exact configuration 

infrastructure as code

supports almost all AWS resources

JSON & YAML

### AWS Cloud Development Kit(CDK)

define cloud infrastructure using a familair language
then compiled into JSON/YAML
you can deploy infractructure and application runtime code together

### Elastic Beanstalk

Platform as a Service 

deploys services for you and all you need to do is provide code

### AWS CodeDeploy

deploy our application automatically

works with EC2 instances
Works with ON-premises servers

upgrade

### AWS CodeCommit

code in a repository using the Git technology

github

### CodeBuild

allows you to build code in the cloud

compiles source code run tests and produces packages that are ready to be deployed

### CodePipeline

orchestrate different steps to have the code automatically pushed production

CICD

### CodeArtifact

software packages depends on eachother to be built

artifact management

### CodeStar

Unified UI easily manage software development activities in one place

### Cloud9

Cloud IDE can use in web browser

### System Manager (SSM)

manages EC2 and On-Premises system at scale
another Hybrid service
patch fleet of EC2 instances or on premise servers
or run commands across an entire fleet of servers

#### Session Manager

start secure shell on ec2 or premise servers with no port 22 or ssh access

#### Perameter Store

secure storage for config and secrets -> API Keys, passwords, configs
serverless
iam permission

## Leveraging the AWS Global Infrastructure

a global application is an aplication deployed in a multiple geographies

this could be on regions and or edge locations

Decreased latency

Disaster Recovery (fail-over)

attack protection

Global Apps:
- Global DNS
- Global Content Delivery Network (CDN): CloudFront
- S3 Transfer Acceleration
- AWS Global Accelerator

### Global DNS: Route 53
Domain Name System

Simple Routing Policy -> no health checks gets ip adress and returns to browser

Weighted routing policy -> Distribute traffic + heath checks

Latency Routing Policy -> if they are close they will be routed to the closest server

Failover Routing Policy (Disaster recovery)-> healthcheck on primary and switch to failover if it fails

### CloudFront

Content Delivery Network

improves read performance, content is cached at the edge

DDoS protection

Origins:
S3 bucket enhanced security with cloudfront origin access control
Custom Origin(HTTP)

static content that must be available everywhere

### S3 Transfer Acceleration

increase transfer speed by transfering file to an AWS edge location which will forward the data to the S3 bucket in the target region

### AWS Global Accelerator

improve global application availability and performance using AWS global network 

2 Anycase IP are created for your application and traffic is sent through edge locations

### AWS Outpost

Hybrid Cloud -> businesses that keep on on-premises infra alongside a cloud infra

you order Outpost that are server rackks that come pre-loaded with AWS services

### AWS WaveLength

Wavelength Zones are infra deployments embedded within telecommunication providers datacenters at ther edge of the 5G Network

ultra low latency applications throug 5G networks

### AWS Local Zones

place AWS compute, storage, database and other selected AWS services close to end users to run latency sensitive applications

Extension of an AWS Region

### Global Application Architecture

- Single Region, Single AZ (easy)
  - Low Availability
  - Low Latency

- Single Region, Multi-AZ
  - High Availability
  - Low Latency

- Multi Region, Active-Passive
  - High Global Reads' Latency
  - Low Global Writes Latency

- Multi Region, Active-Active
  - High Global Reads' Latency
  - High Global Writes Latency

## Cloud Integrations

Synchronous communications -> application to application

Asynchronous / Event based -> application to queue to application


### Simple Queue Service (SQS)

Queue model -> used to decouple applications

serverless and messages are deleted after theyre read by consumers

### Kinesis

kenisis = real-time big data streaming

managed service to collect, process, and analyze real-time streaming data at any scale

### Simple Notification Service(SNS)

one message to many recievers

event publisher only sends message to one SNS topic

as many subscribers will listen to SNS topic notifications

each subscriber will get all of the messages

### Amazon (MQ)

When migrating to the cloud you may want to use MQ

MQ is a managed message broker service for ActiveMQ and RabbitMQ in the cloud

## Cloud Monitoring

### CloudWatch Metrics

metric is a variable to monitor and they have timestamps

you can create cloudwatch dashboards of metrics

EC2 instances -> cpu utilization

### CloudWarch Alarm

alarms are used to trigger notifications for any metric

Alarm actions
Auto Scaling
EC2 Actions
SNS notifications

### CloudWatch Logs

can collect logs from cloudwatch log agents: on EC2 machines or on-premises servers

real time monitoring of logs

### EventBridge

react to events happening

Schedule every hour -> trigger script on Lambda function

schema registry to model event

can also archive events

### CloudTrail

provides governance compliance and audit for your AWS Account

History of events / API calls made within your AWS Account 

a trail can be appplied to all regions or a single region

### X-Ray

tracing and visual analysis of our applications
pinpoint service issues

### CodeGuru

automated code reviews (Reviewer) and application performance recommendations (Profiler)

### Health Dashboard

Service history -> shows all regions all services health

Your Account -> alerts and remediation guidance when AWS is experiencing events that may impact you.
personalized view into the performance and availability of the aws services underling your AWS resources
relevant and timely information
global service

## VPC & Networking

- IPv4 -> Internet Protocol version 4 (4.3 billion addresses)
  - public Ipv4 can be used on the internet (not constant)
  - Private Ipv4 can be used on private networks (constant)

- Elastic IP allows you to attach a fixed public IPv4 address to EC2 Instance
  - has on going cost if not attached to the EC2 instance or if the EC2 instance is stopped

- IPv6 -> Internet protocol version 6 (3.4 x 10^38 addresses)
  - each IP is public
 
### VPC

Virtual Private Cloud: private network to deploy your resources linked to a specific region

Subnets -> allows you to partition you network inside your VPC
  - public subnet is a subnet that is accessible from the internet
  - private subnet is a subnmet that is not accessible from the internet
  - to define access to the internet and between subnets we use route tables
  - intenet gatewat helps our VPC connect with the internet
  - NAT Gateways & NAT Instances allow your instances in your private subnets to access the internet while still raimaining private

### Network ACL & Security Groups

NACL -> a firewall which controlls traffic from and to subnet + attatched at the subnet level

Security groups a firewall that controls traffic to and from an ENI / ec2 instance

### VPC flow logs

capture information about IP traffic going into your interfaces 
  - VPC flow logs
  - Subnet flow logs
  - Elastic Network Interface flow logs

### VPC Peering

connect two VPC pirvatly using AWS network

VPC peering connection is not transitive

### VPC Endpoints

allows you to connect to AWS using private network

VPC Endpoint gateway: S3 & DynamoDB
vpc Endpoint Interface: the rest

### PrivateLink (VPC Endpoint Service)

allows you to connect a service to 1000s of VPCs

### Site to Site VPN & Direct Connect

site to site vpn -> connect an on premises VPN to AWS and goes over the public internet
  - customer gateway on ur prem
  - virtual private Gateway in aws vpc 

Direct Connect -> establish a physical connection between on-premises and aws goes over a private network + takes a month

### AWS Client VPN

connect from yuor computer using OpenVPN to you private network and on-premises

### Transit Gatweway

for having a transitive peering between thousands of VPC and on-premises hub and spoke connection

## AWS Security & Compliance

Shared Responsibility
AWS responsibility -> security of the cloud
Customer -> security in the cloud

### Distributed Denial of Service (DDOS)

attacker -> launch multiple master servers -> launch bots -> request application server

- AWS Shield Standard
  - protect against DDOS attacks for your website and applications for all customers at no additional cost

- AWS Shield Advanced
  - 24/7 premium DDoS protection

- AWS WAF
  - Filter specific requests based on rules
  - protects your web application from common web exploits (Layer 7)
  - deploy on  application load balancer, api gateway and cloudfront

- CloudFront and Route 53
  - availability protection
  - combines with shield provides attack mitagation at the edge


protect against DDOS attacs for your website and applications for all customers at no additional cost

#### AWS Network Firewall

protect your entire VPC
From layer 3 - layer 7

#### Firewall manager

manage security rules in all accounts of an AWS Organisation

#### Penetration testing

carry our security assesments or penetration test aagainst their AWS infrastructure without prior approval for 8 services

#### Data at rest vs. Data in transit

at rest: data stored or archived on a device

in transit: data being moved from one location to another -> means data transferred on the network

for this we use encryption keys

### Key Management Service (KMS)

encryption

KMS = AWS manages the encryption keys for us

Customer Managed Key -> create manage and used by customer
AWS managed key -> used by AWS services
AWS Owned key -> Collection of keys that a service owns and managed to use in multiple accounts
CloudHSM Keys -> keys generated from your own CloudHSM hardware device

### CloudHSM

AWS provides encryption hardware
so we manage our own encryption keys entirely

### AWS Certificate Manager (ACM)

Lets you easily provision, manage, and deploy SSL/TLS Certificates

### AWS Secrets Manager

storing secrets

force rotation of secrets every x days

itegration with Amazon RDS

### AWS Artifact

provides customers with on-demand access to AWS compliance documentation and AWS agreements

Support internal audit or compliance

### Amazon GuardDuty

intelligent threat discovery to protect your AWS account

can setup EventBridge rules to be notified in case of findings

can protect against cryptocurrency attacks (has a dedicated finding for it).

### Amazon Inspector

automated security Assessments
for ec2 instances
for container images push to amazon ECR
for Lambda functions

### AWS Config

helps with auditing and recording compliance of your AWS resources

helps recording configurations and changes over time

### Amazon Macie

machine learning and pattern matching to discover and protect your sensitive data in AWS

alert you to sensitive data such as peronslly identifiable information

### AWS Security Hub

central security tool to manage security across several AWS accounts and automate security checks

### Amazon Detective

analyzes investigates and quickly identifies the root cause of security issues or suspicious activities

automatically collects and processes events from vpc flow logs...

### AWS Abuse

report suspected AWS resources used for abusive or illegal purposes

### Root user privileges

only root user can change account settings
close your aws account
change or cancel AWS Support plan
register as a seller in the reserved instance marketplace

### IAM Access Analyzer

find out which resources are shared externally
define zone of trust -> anything outside this will be reported/flagged

## Machine Learning

### Amazon Rekognition

find objects people text scenes in images and videos using ML

Facial analysis and facial search

### Transcribe

convert speach into text when you pass in audio#

deep learning process called automatic speach recognition

automatically remove personallyt identifiable information using redaction

support automatic langauge identification for multi-lingual audio

### Polly

turn text into speach using deep learning

### Translate

naturaly and accurate langauge translation 

allows you to localize content for intenational users

### Lex + Connect

Lex -> powers Alexa -> natural language understanding

connect -> virtual contact center

### Comprehend

natural language processing

### SageMaker

for devs and data scientists to build ML models

### Forecast

ML to deliver highly accurate forecasts

### Kendra

document search service

allows you to extract answers from within a document

### Personalize

ML-service to build apps with real-time personalized recommendations

### Textract

used to extract texts

## Account Management, Billing & Support

### Organisation

global service allows to manage muultiple AWS accounts

consolidated billing

aggregated usage

polling of reserved ec2 instances

api is available to automade AWS account creation

can restrict account privileges using service control policies

#### Service Control Policies (SCP)

applied to all the Users and Roles of the Account including the root

#### Consolidated Billing

when enabled it gives combined usage for discounts

one bill for all aws accounts in the AWS Organisation

### Control Tower

easy way to setup and govern a secure and compliant multi-account AWS environment

### Resource Access Manager (RAM)

share AWS resources you own with other AWS accounts

### AWS Service Catalog

users that are new to AWS have too many options and may create stacks that are not compliant / in line with the rest of the organization

some users just want a quick self-service portal to launch a set of authorized products pre-defined by admins

### Pricing Models in AWS

#### AWS in general
Pay as you go
save when you reserve
pay less by using more
pay less as AWS grows

#### EC2

only charged for what you use
on demand instances 
reserved instances
spot instances
dedicated host
savings plans

#### Lambda & ECS

Lambda
- pay per call
- pay per duration

ECS
- pay for aws resources stored and created in you application

Fargate
- pay for vCPU and memory resources allocated to your applications in your containers

#### S3
storage class

#### EBS

volume type
storage volume in GB per month provisionned

#### RDS

per hour billing

#### CloudFront

different across different grographic regions
requests

### Saving plan overview

commit a certain amount per hour for 1 or 3 years

#### EC2 Saving plan
72%

commit to usage of individual instance families in a regoin (c5 or m5)

#### Compute savings plan

commit certain amount 66% discount compared to on demand

#### Machine Learning savings plan

for sagemaker

### Compute Optimizer Overview

reduce costs and improve performance by recommending optimal AWS resources for your workloads

machine learning analyze configurations and their utilization CloudWatch metrics

### Billing and Costing Tools

#### Pricing calculator

Estimating costs in the cloud -> 

#### Tracking costs in the cloud 

- Billing dashboard show all costs for the month
- Cost Allocation Tag -> track you AWS costs on a detailed level
  - AWS generated tags
  - User-defined tags

- Cost and Usage Reports
  - the most comprehensive set of AWS cost and usage data available

- Cost Explorer
  -  forecast usage up to 12 months based on previous usage

#### Monitoring against cost plans 

Billing Alarms in CloudWatch us-east-1

#### AWS budgets

send alarms when costs exceeds the budget

### Cost Anomaly Detection

continously monitor your cost and usage using ML to detect unusual spends

### Service Quotas

notify when you're close to a service quota value threshold

### Trusted Advisor

high level account assessment

full set of checks you need Business & Enterprise plan

### Support plans pricing

- basic support -> free + customer service & commnunities + 8 advisor checks
- Developer Support Plan -> business hours email access
- Business support plan -> 24/7 phone email and chat access all trusted advisor checks
- Enterpirse ON-ramp support plan -> Technical account managers + concierge support team + infrastructure event management, well-architected and operations reviews
- Enterprise support plan -> Designated technical account manager


## Advanced Identity

### AWS Security Token Service (STS)

enables you to create temporary limited-privileges credentials to access your aws resources

### Cognito

provide identity for your web and mobile applications users
database for them

### Microsoft Active Directory

database of objects

### AWS Directory Services

Microsoft Active Directory

### AWS IAM Identity Center

one login for all your AWS accounts in AWS Organizations

## Other Services

### Amazon Workspaces

great to eliminate management of on-premise Virtual Desktop Infrastructure (VDI)

### Appstream 2.0

desktop application streaming service

the application is delivered from within a web broswer

### IoT Core

allows you to connect Iot devices to ther AWS clouds
serverless secure and scalable

### AppSync

build a backend for mobile ande web apps in realtime
makes use of GraphQL

### Amplify

a set of tools and services that helps you develop and deploy scalable full stack web and mobile applications

### AWS Application composer

visually design and build serverless applications quickly on AWS

### Device Farm Overview

fully managed service that tests your web and mobile apps against desktop browser real mobile devices, and tablets

### AWS Backup

fully managed service to centrally manage and automate backups across AWS services

### Disaster recovery strategies

cheapest -> Backup and Restore

Pilot light -> core functions of the app

warm standby -> full version of the app at minimum size

Multi-site -> full version of the app  at full size

### AWS Elastic Disaster Recovery (DRS)

Quickly and easily recoever your physcal virtual and clodua based server into aws

### DataSync

move larger amount of data from on-premises to AWS

Replication tasks are incrmeental after the first full load

### Application Discrovery Service & Application migration

plan migration using discovery service -> Agentless Discovery connector + 


Application Migration Service -> easily move from premises to the cloud

### AWS Migration evaluator

helps you build a data driven business case for migration to AWS (PLAN)

### Migration Hub

central location to collect server and application inventory data for the assessment planning and tracking of migrations to AWS

### Fault injection simulator

A fully managed service for running fault injection experiments on AWS workloads

Based on Chaos Engineering -> stressing an application by creating disruptive events  

### Step functions

Build serverless visual Workflow to orchestrate your lambda functions

### Ground station

fully managed service that lets you control satellite communications process data and scale your satellite operations

### AWS Pinpoint

scalable 2-way outbound/inbound marketing communication service

## AWS Architecting & Ecosystem

stop guessing capacity needs and use autoscaling

test systems at production scale

#### Cloud best Practices

Scalability: verticle and horizontal

disposable resources: servers should be disposable & easily configured

Automation: Serverless, infrastructure as a service. Auto Scaling

Loose Coupling: Monolith are applications that do more and more overtime -> break down

services not servers
### Pillar 1 Operational Excellence

ability to run and monitor systems to delvier buisiness value and to continually improve supporting processes and procedures

perform operations as code
annotate
frequent small reversible changes
refine operations procedures frequently
anticipate failure

cloudformation 
config
cloudtrail
cloudwatch
aws xray
### Pillar 2 Security

improves the ability to protect information systems and assets while delivering business value through risk assessments and mitigation strategies

implement a strong identity foundation
enable tradeability
apply security at allm layers
automate security best practices
protect data in transit and at rest
keep people away from data
prepare for security events

IAM , Organization
config cloudtrail cloudwatch
cloudfront vpc shield

### Pillar 3 Reliability

abbility of a system to recover from infra or service disruption

test recovery procedures
automatiucally recover from failure
scale horizontally to increase aggregate system availability
stop guessing capacity
manage chnage in automation

asg
cloudwa

### Pillar 4 Performance Efficiency

includes the ability to use computing resources effieciently

democratize advanced tech
go global in minutes
use serverless architectures
experiment more often
mechinical symphany

### Pillar 5 Cost Optimization

ability to run systems to deliver business value at the lowest price point

adopt a consumption mode
measure overall efficiency
stop spening money on data center operations
analytze and attribute expenditure
use managed and aplication level services to reduce cost of ownership

### Pillar 6 Sustainability

minimizing the enviromental impact of running cloud workloads

- understant your impact
- establish sustainability goals
- maximize utilization
- anticipate and adopt new  more efficient hardware and software offerings
- use managed services
- reduce the downstream impact of your cloud workloads

### AWS Well-Architected Tool

review your architecture against the 6 pillars and adopt architectural best practices

### AWS Cloud Adoption Framework (CAF)

ebook to help build and execute a comprehensive plan for your digital transformation through innovative use of AWS

Business Capabilities:
- Business -> helps ensure cloud investments accelerate your digital transofmration ambitions and business outcomes
- People -> bridge between tech and business
- Governance -> orchestrate your cloud initiatives while maximizing organizational benefits and minimizing tansformatioin related risks

Technical Capabilities:
- Platform -> helps build enterprise grade scalable hybrid cloud platform
- Security -> security perspective helps you achieve the confidentiality integrity and availability of your data and cloud workloads
- Operations -> Operations perspective helps ensure that your cloud services are delivered at a level that meets the needs of your business

Technology -> using the cloud to migrate and modernize legacy infra apps data and analytics platforms

process -> digitizing automating and optimizing everything

organizations -> reimagining yourt operating model

Product -> reimagining your busniess nmodel by creating new value propositions and revenue models


Transformation phases 
- envision -> demonstrate how cloud accelerated business
- align -> identify capability gaps across 6 AWS CAF Perspectives
- launch -> build and deliver pilot initiatives in production and demonstrate incremental business value
- scale -> expland pilot initiatives to the desired scale while realizing the desired business benefits

### Right Sizing

choosing the most powerful ec2 instance may not be the right choice

start off small as scaling up is easy

before cloud migration

continously after cloud onboarding process

### AWS Ecosystem - Free resources

AWS Blogs
AWS Forums
AWS Whitepapers and guides
AWS partner solutions
AWS Solutions

AWS Marketplace digital catalog with thoussants of software listings from independent software vendors

AWS Training -> digital private academy...

AWS Prodessional Service & Partner Network
- APN Techonolgy Partners -> providing hardware connectivity and software
- APN Consulting Partners -> professional services firm to help build on AWS
- APN Training Partners find who can help you learn aws
- Competency program: granted to APN partners who have demonstrated techinical proficiency andf proven customer success in specialized solution areas
- AWS Navigate program: help partners become better partners

### AWS IQ & re:Post

IQ quickly find professional help for your AWS projects

re:post is a community forum aws manages q&a service -> if premium and no response a aws support engineer will help

### AWS Knowledge Center

most frequent and common questions asked in re:Post

### AWS Managed Services (AMS)

team of people that provide infra and application support

AMS offers a teamn of AWS expers who manage and operate your infrastructure for security

