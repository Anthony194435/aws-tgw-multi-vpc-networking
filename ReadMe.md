# Project Name: AWS Transit Gateway Setup for Multi-VPC Connectivity

## Project Description
This project demonstrates the setup of an **AWS Transit Gateway** to enable seamless communication between five Virtual Private Clouds (VPCs). The Transit Gateway acts as a centralized routing hub, simplifying network management and reducing interconnectivity costs compared to traditional peering. Each VPC in this setup is assigned a unique CIDR block, and appropriate route tables are configured to ensure smooth data flow.

## System Architecture Diagram
Below is the high-level architecture illustrating the AWS Transit Gateway connecting five VPCs:
![Screenshot 2025-01-06 141445](https://github.com/user-attachments/assets/55836017-c94e-4b3b-b0ac-bf6cfff3b3a1)


## VPC and CIDR Configuration
The following are the CIDR blocks assigned to each VPC:

| VPC Name         | CIDR Block       | Purpose                  |
|------------------|------------------|--------------------------|
| VPC-prod         | 172.31.0.0/16    | Production Environment    |
| VPC-staging      | 10.1.0.0/16     | Staging Environment       |
| VPC-Dev          | 10.2.0.0/16     | Development Environment   |
| VPC-Analytics    | 10.3.0.0/16     | Analytics and Reporting   |
| VPC-shared       | 10.4.0.0/16     | Shared Services           |

![Screenshot 2025-01-06 142851](https://github.com/user-attachments/assets/e3d52d86-3ef0-47f1-8e9a-76f374eff412)


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
4. Click **Create Transit Gateway**

   ![Screenshot 2025-01-06 143518](https://github.com/user-attachments/assets/086b6173-6259-47f1-9ee0-62e39957bc5c)


### Step 2: Attach VPCs to the Transit Gateway
For each VPC, follow these steps:

![Screenshot 2025-01-06 150719](https://github.com/user-attachments/assets/a3e7973b-f703-4814-9066-aee966e9597b)


1. Navigate to **Transit Gateway Attachments** in the **VPC Console**.
2. Click **Create Transit Gateway Attachment** and configure:
   - **Transit Gateway ID**: Select the created Transit Gateway.
   - **Attachment Type**: VPC
   - **VPC ID**: Select the VPC to attach.
   - **Subnet IDs**: Choose one or more subnets from the VPC.
3. Repeat the process for all five VPCs.

![Screenshot 2025-01-06 151538](https://github.com/user-attachments/assets/062dd620-3ae9-400a-910b-a1f730e88c04)



### Step 3: Update Route Tables
To enable communication between VPCs, update the route tables for each VPC:

1. For each VPC, navigate to **Route Tables** in the **VPC Console**.
2. Select the route table associated with the VPC and click **Edit Routes**.
3. Add the following routes:
   - **Destination**: CIDR of the other VPCs (e.g., for VPC-Prod, VPC-Dev, VPC-Shared, VPC-Analytics, VPC-Staging add routes for 10.1.0.0/16, 10.2.0.0/16, etc.)
   - **Target**: Transit Gateway ID (e.g., tgw-xxxxxxxxxxxx)
4. Save the changes.
5. Just like the image shown below , you need to do the same to all VPCs

![Screenshot 2025-01-06 152014](https://github.com/user-attachments/assets/16924434-d965-4f72-aaf1-dc8dd1599211)


### Step 4: Configure Transit Gateway Route Table
1. Navigate to the **Transit Gateway Route Tables** section in the **VPC Console**.
2. Create a new route table or use the default route table.
3. Add routes for each VPC CIDR block with the corresponding **Attachment ID**.
4. Propagate and associate the route table with each Transit Gateway attachment.

![Screenshot 2025-01-06 154443](https://github.com/user-attachments/assets/41fb90fb-ce34-43e3-a798-b2ddc5ff80f4)


### Step 5: Test Connectivity
1. Launch EC2 instances in each VPC.
![Screenshot 2025-01-07 225713](https://github.com/user-attachments/assets/fced3df2-d017-4f44-8237-145b5e91cbf6)


3. Ensure instances are in subnets associated with the Transit Gateway.
4. Use **ping** or **Curl** to test connectivity between instances in different VPCs.


**This image shows connectivity between the Prod-vpc with the other four VPCs
![Screenshot 2025-01-07 224746](https://github.com/user-attachments/assets/d01e5273-bb73-49b5-a195-6c9252c1a163)

**This image shows connectivity between the Dev-vpc with the other four VPCs
![Screenshot 2025-01-07 225023](https://github.com/user-attachments/assets/5ee25ce4-2133-40a2-bbbd-9200ab8c5c6a)

**This image shows connectivity between the Analytics-vpc with the other four VPCs
![Screenshot 2025-01-07 225254](https://github.com/user-attachments/assets/8f82303e-256b-4c9f-b3a2-e9a6313f6f8f)

**This image shows connectivity between the Staging-vpc with the other four VPCs
![Screenshot 2025-01-07 225419](https://github.com/user-attachments/assets/a8b56eed-38eb-4be6-8352-eca8d655bcb1)

**This image shows connectivity between the Shared-vpc with the other four VPCs
![Screenshot 2025-01-07 231838](https://github.com/user-attachments/assets/96479de4-5764-4dd2-b5d0-e2e3f180e977)


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
