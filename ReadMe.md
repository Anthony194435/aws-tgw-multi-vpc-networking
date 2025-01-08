# Project Name: AWS Transit Gateway Setup for Multi-VPC Connectivity

## Project Description
This project demonstrates the setup of an **AWS Transit Gateway** to enable seamless communication between five Virtual Private Clouds (VPCs). The Transit Gateway acts as a centralized routing hub, simplifying network management and reducing interconnectivity costs compared to traditional peering. Each VPC in this setup is assigned a unique CIDR block, and appropriate route tables are configured to ensure smooth data flow.

## System Architecture Diagram
Below is the high-level architecture illustrating the AWS Transit Gateway connecting five VPCs:

![System Architecture](path/to/architecture-image.png)

## VPC and CIDR Configuration
The following are the CIDR blocks assigned to each VPC:

| VPC Name         | CIDR Block       | Purpose                  |
|------------------|------------------|--------------------------|
| VPC-prod         | 172.31.0.0/16    | Production Environment    |
| VPC-staging      | 10.1.0.0/16     | Staging Environment       |
| VPC-Dev          | 10.2.0.0/16     | Development Environment   |
| VPC-Analytics    | 10.3.0.0/16     | Analytics and Reporting   |
| VPC-shared       | 10.4.0.0/16     | Shared Services           |

## Steps to Set Up AWS Transit Gateway

### Step 1: Create the Transit Gateway
1. Open the **AWS Management Console** and navigate to the **VPC** service.
2. Select **Transit Gateways** and click **Create Transit Gateway**.
3. Provide the following configurations:
   - **Name**: Multi-VPC-Transit-Gateway
   - **Description**: Transit Gateway for Prod, Dev, Shared, Staging, Analytic VPCs
   - **Amazon ASN**: defualt
   - **DNS Support**: Enabled
   - **VPN ECMP Support**: Enabled
4. Click **Create Transit Gateway** and note the **Transit Gateway ID** (e.g., tgw-xxxxxxxxxxxx).

### Step 2: Attach VPCs to the Transit Gateway
For each VPC, follow these steps:

1. Navigate to **Transit Gateway Attachments** in the **VPC Console**.
2. Click **Create Transit Gateway Attachment** and configure:
   - **Transit Gateway ID**: Select the created Transit Gateway.
   - **Attachment Type**: VPC
   - **VPC ID**: Select the VPC to attach.
   - **Subnet IDs**: Choose one or more subnets from the VPC.
3. Repeat the process for all five VPCs.

### Step 3: Update Route Tables
To enable communication between VPCs, update the route tables for each VPC:

1. For each VPC, navigate to **Route Tables** in the **VPC Console**.
2. Select the route table associated with the VPC and click **Edit Routes**.
3. Add the following routes:
   - **Destination**: CIDR of the other VPCs (e.g., for VPC-Prod, add routes for 10.1.0.0/16, 10.2.0.0/16, etc.)
   - **Target**: Transit Gateway ID (e.g., tgw-xxxxxxxxxxxx)
4. Save the changes.

### Step 4: Configure Transit Gateway Route Table
1. Navigate to the **Transit Gateway Route Tables** section in the **VPC Console**.
2. Create a new route table or use the default route table.
3. Add routes for each VPC CIDR block with the corresponding **Attachment ID**.
4. Propagate and associate the route table with each Transit Gateway attachment.

### Step 5: Test Connectivity
1. Launch EC2 instances in each VPC.
2. Ensure instances are in subnets associated with the Transit Gateway.
3. Use **ping** or similar tools to test connectivity between instances in different VPCs.

## Troubleshooting
- **Transit Gateway Attachment Issues**:
  - Ensure subnets selected for attachment are in different Availability Zones.
  - Verify that the VPC route tables point to the Transit Gateway.

- **Routing Issues**:
  - Check Transit Gateway route table propagation and association settings.
  - Ensure no overlapping CIDR blocks between VPCs.

- **Security Groups and Network ACLs**:
  - Verify that security groups and NACLs allow inbound and outbound traffic between VPCs.

## Conclusion
This setup provides centralized, scalable, and cost-effective inter-VPC communication using AWS Transit Gateway. This architecture is ideal for multi-environment setups, such as production, staging, development,Analytics, and shared services.




### Note!!!
Shared-server, public IP: 3.95.5.152
prod-server public IP: 3.234.221.162
Analytics public IP: 54.157.226.202
Dev-server public IP: 54.152.76.134
Staging-server public IP: 3.84.38.9