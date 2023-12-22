# What is a Virtual Public Cloud (VPCS)

**Definition:** A Virtual Public Cloud

**Availability Zone:** A Data Center

A VPC is a building/network shared with everyone else. Using the public cloud, a VPC is like an apartment in this building, with default subnets associated with availability zones representing the rooms in the apartment (3 for Ireland).

**Custom VPC:** Can be used to set public and private subnets.

- Public subnet = App/Api
- Private subnet = Database

TCP/IP Network: Every device needs an IP address. The IP address range is decided using a CIDR block (10.0.0.0), and subnet masks are not needed.

- 10.0.0.0/16: First two values/numbers are locked, so change the last two.
- 10.0.2.0/24: Public subnet
- 10.0.3.0/24: Private subnet

By default, when you set up a VPC, there is a router that handles the routing for your network. It uses a default route table that only allows routing within your VPC.

We need to create a new route table to allow requests from the outside (Internet Gateway, a door from inside VPC to out).

To come inside from the internet:

1. Public IP address to the Internet Gateway
2. Routed to the subnet
3. Then to the VM

<img src="../readme-images/vpc.png" alt="VPC Image" width="500"/>

**Traffic:**
- Insecure (outside VPC)
- Secure (within VPC)

**Create:**
1. VPC
2. Subnets
3. Internet Gateway
   - Attach to the VPC
4. Create route table
5. Associate table with subnet
6. Associate table with Internet Gateway
7. Check VPC setup correctly

## AWS Method:

1. Search for VPC in AWS
   
<img src="../readme-images/vpcselect.png" alt="AWS VPC Image" width="400"/>

2. Create VPC
   
<img src="../readme-images/createvpc.png" alt="Create VPC Image" width="400"/>

   
<img src="../readme-images/vpcfill.png" alt="VPC Create Form Image" width="400"/>

3. Subnets

<img src="../readme-images/createSubnets.png" alt="Create Subnet Image" width="400"/>

4.  Create subnet
   
   <img src="../readme-images/subnetfillform.png" alt="Subnet Fill Form Image" width="400"/>

5.  Internet Gateway

   <img src="../readme-images/igwcreate.png" alt="Create IGW Image" width="400"/>

6.  Internet Gateway fill
    
   <img src="../readme-images/igwfill.png" alt="IGW Fill Image" width="400"/>

7.  Attach Gateway to VPC
    
   <img src="../readme-images/igwattachvpc.png" alt="Attach IGW to VPC Image" width="400"/>

8.  Attach to VPC fill
    
   <img src="../readme-images/igwattachvpcfill.png" alt="Attach IGW to VPC Fill Image" width="400"/>

9.  Create Routes
    
   <img src="../readme-images/routescreate.png" alt="Create Route Image" width="400"/>

10. Route fill
    
   <img src="../readme-images/routefill.png" alt="Route Fill Image" width="400"/>

11. Associate route with public subnet
    
   <img src="../readme-images/associatesubnet.png" alt="Associate Subnet Image" width="400"/>

12. Associate fill
    
   <img src="../readme-images/associatesubnetfill.png" alt="Associate Subnet Fill Image" width="400"/>

13. Associate route with IGW
    
   <img src="../readme-images/routeigw.png" alt="Route IGW Image" width="400"/>

14. Associate route with IGW fill
    
   <img src="../readme-images/associateigwroutefill.png" alt="Associate IGW Route Fill Image" width="400"/>

15. Check VPC
    
   <img src="../readme-images/completevpc.png" alt="Final VPC Image" width="400"/>

Create DB VM with VPC

<img src="../readme-images/securitygroupvpc.png" alt="DB Create VPC Image"/>

Create App VM with VPC

<img src="../readme-images/appvpc.png" alt="App Create VPC Image"/>

Can't use VPC with old security group, so we have to create a new one.