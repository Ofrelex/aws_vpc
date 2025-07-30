# Network Mastery with AWS VPC
Step-by-step process for the project "Network Mastery with AWS VPC", designed to help you or your team at GatoGrowFast.com build a secure and well-structured cloud network using AWS VPC.

### Project Title: Network Mastery with AWS VPC
#
## Phase 1: Introduction & Planning
### Goal:
Understand what a VPC is and how its components (subnets, gateways, route tables, etc.) work together to form a secure network.
#
## Phase 2: VPC Setup and Configuration
#
### Step 1: Create a Virtual Private Cloud (VPC)
> Create a virtual network environment that mimics a data center for GatoGrowFast.com.
* Go to AWS Console > VPC
* Click "Create VPC"
* Name it: `GatoMainVPC`
* IPv4 CIDR block: `10.0.0.0/16`
* Leave other settings as default
* Click Create VPC

![AWS](img/1_aws.png)

![VPC](img/2_vpc.png)

![Create_VPC](img/3_create_vpc.png)

![VPC](img/4_created_vpc.png)
#
### Step 2: Create Public and Private Subnets
> Divide your VPC into logical segments.
* Navigate to Subnets > Create Subnet
* Select VPC: `GatoMainVPC`
* Create at least 2 subnets in different Availability Zones:
  * Public Subnet A  – `10.0.1.0/24`
  * Private Subnet A – `10.0.2.0/24`
* Name and tag your subnets accordingly
* Enable Auto-assign public IP for public subnet only

![Subnet](img/5_subnet.png)

![Subnet](img/6_sub_vpc.png)

![Public_Subnet](img/7_sub_public.png)

![Private_Subnet](img/8_sub_private.png)

#
### Step 3: Create and Attach an Internet Gateway (IGW)
> Allows instances in public subnet to access the internet.
* Go to Internet Gateways > Create
* Name it: `Gato-IGW`
* Attach it to `GatoMainVPC`

![Gateways](img/9_gateways.png)

![](img/10_create_gateway.png)

![](img/11_attach_vpc.png)

#
### Step 4: Configure Route Tables for Public Subnet
> Define routing rules to connect public subnet to the internet.
* Go to Route Tables
* Locate the main route table or create a new one (e.g., `Gato-Public-RT`)
* Edit routes:
  * Add route: `0.0.0.0/0` → Target: Internet Gateway
* Associate the route table with Public Subnet

![Route_Table](img/12_route_table.png)

![](img/13_create_RT.png)

![](img/14_route_t.png)

![](img/15_RT.png)
#
### Step 5: Create NAT Gateway for Private Subnet
> Allows private subnet instances to access the internet for updates, etc., without being directly exposed.
* Go to Elastic IPs > Allocate Elastic IP
* Go to NAT Gateways > Create NAT Gateway
  * Subnet: Public Subnet
  * Elastic IP: select the one you just allocated
* Name it: `Gato-NATGW`

![Elastic_IP](img/16_elastic_ip.png)

![](img/17_nat.png)

![](img/18_NAT_Gateways.png)


#
### Step 6: Configure Route Table for Private Subnet
> Use NAT Gateway to access the internet indirectly.
* Create or modify a route table (e.g., `Gato-Private-RT`)
* Add route: `0.0.0.0/0` → Target: NAT Gateway
* Associate the route table with Private Subnet

![Private](img/19_private_associate.png)

#
## Phase 3: VPC Peering for Inter-VPC Communication
### Step 7: Create Another VPC
> Simulate a second environment (e.g., Dev/Test).
* Name: `GatoDevVPC`
* CIDR: `10.1.0.0/16`

![VPC](img/20_VPC.png)

#
### Step 8: Create VPC Peering Connection
> Establish communication between VPCs.
* Go to Peering Connections > Create
  * Requester: `GatoMainVPC`
  * Accepter: `GatoDevVPC`
* After creation, click "Accept" on the Accepter side

![Peering](img/21_peering.png)

![Accept](img/22_acceptance.png)
#
### Step 9: Update Route Tables for Peering
> Allow subnets in both VPCs to communicate.
* In both VPC route tables (Main and Dev):
  * Add route to each other's CIDR blocks
    * In `GatoMainVPC`: `10.1.0.0/16` → Peering Connection
    * In `GatoDevVPC`: `10.0.0.0/16` → Peering Connection

#
## Phase 4: Secure Connectivity and Validation
### Step 10: Configure Network ACLs and Security Groups
> Ensure proper access control.
* Use Security Groups to allow/deny specific ports (e.g., allow SSH, HTTP)
* Set Network ACLs to control traffic at the subnet level
#
### Step 11: Launch EC2 Instances
> Test public and private connectivity
* Public Subnet: Launch an EC2 instance and verify internet access
* Private Subnet: Launch another instance and test:
  * Can it reach internet? (via NAT)
  * Can it reach public subnet?
  * Can it communicate with instance in `GatoDevVPC`?
#
## Project Wrap-up: Review & Documentation
#
### Step 12: Document Network Architecture
> Create a simple diagram or write-up showing:
* VPCs and CIDRs
* Subnet allocations
* IGW, NAT Gateway
* Route tables and their targets
* Peering setup
