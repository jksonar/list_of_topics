
Amazon Virtual Private Cloud (VPC) involves understanding how to create and 
manage an isolated virtual network within the AWS cloud and the core components that 
govern its connectivity and security. 
---
## Core Concepts and Components
A VPC creates a logically isolated network environment within AWS. Key components include: 

* **IP Addressing & CIDR**: VPCs use CIDR blocks (e.g., 10.0.0.0/16) for their IP address 
range, which is then divided into subnets.
* **Subnets**: Subnets are sections of the VPC's IP range located within an Availability Zone.
    * **Public Subnet**: Routes traffic to an Internet Gateway (IGW) for internet access (requires a public IP).
    * **Private Subnet**: Does not have a direct route to an IGW. Instances can access the internet via a NAT Gateway but cannot receive unsolicited inbound internet traffic.
* **Internet Gateway (IGW)**: Connects the VPC to the internet.
* **NAT Gateway**: Allows instances in private subnets to initiate outbound internet traffic.
* **Route Tables**: Control network traffic direction.
* **Security Groups**: Instance-level firewalls controlling traffic.
* **Network ACLs**: Subnet-level firewalls that are stateless. 

## Advanced Connectivity & Security

* **VPC Peering**: Connects two VPCs privately.
* **VPC Endpoints**: Private connections to supported AWS services.
* **AWS Site-to-Site VPN**: Connects a VPC to an on-premises data center.
* **VPC Flow Logs**: Captures IP traffic data for analysis. 

## Practical Study Tips

* Gain hands-on experience by building a VPC using the AWS console or CLI.
* Utilize the official Amazon VPC User Guide for detailed information.
* Visualize your VPC components and traffic flow with the "Resource map" feature.
* Explore video tutorials for visual learning and setup walkthroughs

---
| VPC Component                                 | What it is                                    |
| ----------------------------------------------| ---------------------------------------------- |
| Virtual Private Cloud(VPC)                    | A logically isolated virtual network in the AWS cloud |
| Subnet                                        | A segment of a VPC's IP address range where you can place groups of isolated resources |
| Internet Gateway/Egress-only Internet Gateway | The Amazon side of a connection to the public internet for IPv4/IPv6 |
| Router                                        | Router interconnect subnets and direct traffic between internet gateways, virtual private gateways,NAT gateways, and subnets|
|Peering Connection                             |Direct connection between two VPCs|
|VPC Endpoints                                  |Private connection to public AWS services|
|NAT Instance                                   |Enables internet access for EC2 instances in private subnets (managed by you)|
|NAT Gateway                                    |Enables internet access for EC2 instances in private subnets (managed by AWS)|
|Virtual Private Gateway                        |The Amazon VPC side of a Virtual Private Network (VPN) connection|
|Customer Gateway                               |Customer side of a VPN connection|
|AWS Direct Connect                             |High speed,high bandwith,private network connection from customer to aws|
|Security Group                                 |Instance-level firewall|
|Network ACL                                    |Subnet-level firewall|