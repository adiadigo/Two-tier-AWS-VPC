# Two-tier-AWS-VPC
This simple mini project describes deployment of a two-tier (Public, Private) AWS VPC and establishing connection between the instances deployed in the two subnets

## Architecture Overview

- **Custom VPC** (`10.0.0.0/16`)
- **Public Subnet** (`10.0.1.0/24`) with a Bastion EC2 instance
- **Private Subnet** (`10.0.2.0/24`) with an internal EC2 instance
- **Internet Gateway** attached to public subnet only
- **Custom Route Tables** for public and private routing
- **Security Groups** enforcing access at instance level
- **Network ACLs** restricting subnet-level traffic

## Access Flow

1. SSH into the public EC2 (bastion host) from your local system.
2. From the bastion, SSH into the private EC2 instance using internal IP.
3. Private EC2 has **no public IP** and is only accessible internally.

## Key Configurations

### VPC & Subnets
- VPC CIDR: `10.0.0.0/16`
- Public Subnet: `10.0.1.0/24` (auto-assign public IP enabled)
- Private Subnet: `10.0.2.0/24` (no public access)

### Internet Gateway
- Connected to VPC
- Public route table has:  
  `0.0.0.0/0 → Internet Gateway`

### Route Tables
- Public Route Table: Associated with public subnet
- Private Route Table: Local route only, no IGW

### Security Groups
- Public EC2 SG: Allows SSH from user's IP (`22`)
- Private EC2 SG: Allows SSH only from public EC2 SG

### NACLs
- **Public Subnet NACL**
  - Inbound: Allow `22` and ephemeral ports from `0.0.0.0/0`
  - Outbound: Allow ephemeral ports + `22`

- **Private Subnet NACL**
  - Inbound: Allow `22` from `10.0.1.0/24`
  - Outbound: Allow `1024-65535` to `10.0.1.0/24`

## Learning Outcomes

- VPC design and subnetting
- Route tables vs Security Groups vs NACLs
- Bastion host setup for internal access
- Manual troubleshooting and debugging (SSH hangs, port rules, role assignments)

## What's Next?

This project will be extended with:
- VPC peering across AWS regions (Mumbai → Oregon)
- Inter-region routing and secure internal EC2 com![Screenshot 2025-06-24 183712](https://github.com/user-attachments/assets/10f![Screenshot 2025-06-24 194109](https://github.com/user-attachments/assets/301035b2-5531-44bf-a4d9-4cbcfba53e04)
5e668-8fc2-480b-9629-5796ae898289)
munication![Screenshot 2025-06-24 192536](https://github.com/user-attachments/assets/15183970-5980-41ac-8fdd-0009d7f31481)


## Screenshots![Screenshot 2025-0![Screenshot 2025-![Screenshot 2025-06-24 153420](https://github.com/user-attachments/assets/35a232d2-26f9-4567-8457-dd981103b927)
06-24 152439](https://github.com/user-attachments/assets/cf583913-ec68-48df-b220-eec7bbb46ef7)
6-24 152439](https://github.com/user-attachments/assets/863dfc3d-24d0-4020-a212-187c636f2c53)


