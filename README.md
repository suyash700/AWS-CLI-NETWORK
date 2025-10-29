# AWS-CLI-NETWORK

# 🌐 AWS CLI Network Architecture Setup — Region: ap-south-2

## 🧭 Overview
This project demonstrates how to **create and configure a complete AWS network architecture** using **AWS CLI only**, including VPC, subnets, route tables, security groups, and EC2 instances (Ubuntu).  
All resources are provisioned in **`ap-south-2` (Hyderabad)**.

---

# Network architecture diagram

<img width="1602" height="773" alt="image" src="https://github.com/user-attachments/assets/83018257-5914-4d2b-9575-745b28211bcf" />


## 🏗️ Architecture Components

| Resource Type | Name | Description |
|----------------|------|--------------|
| VPC | `assignment-vpc` | Main Virtual Private Cloud |
| Public Subnet | `public-web-subnet` | Hosts the web EC2 instance |
| Private Subnet | `private-db-subnet` | Hosts the database EC2 instance |
| Internet Gateway | `assignment-igw` | Provides Internet access to public subnet |
| Public Route Table | `public-rtb` | Routes traffic from the public subnet to the IGW |
| Security Group (Public) | `public-web-sg` | Allows HTTP (80) and SSH (22) |
| Security Group (Private) | `private-db-sg` | Allows DB access only from public subnet |
| EC2 (Public) | `web-server-instance` | Ubuntu web server in the public subnet |
| EC2 (Private) | `db-server-instance` | Ubuntu DB server in the private subnet |

---

## ⚙️ Step-by-Step AWS CLI Commands

### 1️⃣ Create VPC

aws ec2 create-vpc \
  --cidr-block 10.0.0.0/16 \
  --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=assignment-vpc}]' \
  --region ap-south-2

<img width="1919" height="900" alt="Screenshot 2025-10-28 182820" src="https://github.com/user-attachments/assets/5da5c3d5-2e1a-4f29-bad1-d511431b5193" />

### VERIFY FROM CONSOLE

<img width="1919" height="977" alt="Screenshot 2025-10-28 183052" src="https://github.com/user-attachments/assets/669aeaa2-ca28-4bab-813f-6497f4b2affd" />

------

### 2️⃣ Create Subnets

Public Subnet:

aws ec2 create-subnet \
  --vpc-id <your-vpc-id> \
  --cidr-block 10.0.1.0/24 \
  --availability-zone ap-south-2a \
  --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=public-web-subnet}]' \
  --region ap-south-2

<img width="1919" height="937" alt="Screenshot 2025-10-28 183148" src="https://github.com/user-attachments/assets/18d1c58a-8c1d-4110-903e-986f9a82e96a" />

### VERIFY FROM CONSOLE

<img width="1919" height="982" alt="Screenshot 2025-10-28 183204" src="https://github.com/user-attachments/assets/590541c7-c976-4c4f-8a6c-a75a6e41c745" />

------

### Private Subnet:

aws ec2 create-subnet \
  --vpc-id <your-vpc-id> \
  --cidr-block 10.0.2.0/24 \
  --availability-zone ap-south-2a \
  --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=private-db-subnet}]' \
  --region ap-south-2

<img width="1897" height="895" alt="Screenshot 2025-10-28 183245" src="https://github.com/user-attachments/assets/cff0a56a-7a5d-45f4-aa69-434274be5dea" />


### VERIFY FROM CONSOLE

<img width="1917" height="964" alt="Screenshot 2025-10-28 183310" src="https://github.com/user-attachments/assets/b500d0c1-4e05-43c9-a6de-b8b7e60779d6" />

------

### 3️⃣ Create and Attach Internet Gateway

aws ec2 create-internet-gateway \
  --tag-specifications 'ResourceType=internet-gateway,Tags=[{Key=Name,Value=assignment-igw}]' \
  --region ap-south-2
  
<img width="1566" height="400" alt="Screenshot 2025-10-28 183337" src="https://github.com/user-attachments/assets/85fbefc2-ae23-477c-a7b8-c8a68fa19b10" />

### VERIFY FROM CONSOLE
 <img width="1915" height="967" alt="Screenshot 2025-10-28 183347" src="https://github.com/user-attachments/assets/290a2c1f-bd8d-4073-81e3-0c7eecfbc51d" />


------

aws ec2 attach-internet-gateway \
  --internet-gateway-id <your-igw-id> \
  --vpc-id <your-vpc-id> \
  --region ap-south-2

<img width="884" height="106" alt="Screenshot 2025-10-28 183517" src="https://github.com/user-attachments/assets/b4be9ebb-1b86-4b28-aa26-b4b2be70dff7" />

### VERIFY FROM CONSOLE

<img width="1919" height="940" alt="Screenshot 2025-10-28 183540" src="https://github.com/user-attachments/assets/832166ef-7f9d-43f4-81da-51a5616ae6ff" />

------


### 4️⃣ Create Route Table and Associate with Public Subnet

aws ec2 create-route-table \
  --vpc-id <your-vpc-id> \
  --tag-specifications 'ResourceType=route-table,Tags=[{Key=Name,Value=public-rtb}]' \
  --region ap-south-2

<img width="1412" height="710" alt="Screenshot 2025-10-28 183628" src="https://github.com/user-attachments/assets/a4154e25-4521-44e0-ae43-55e420d5057b" />

<img width="1919" height="973" alt="Screenshot 2025-10-28 183833" src="https://github.com/user-attachments/assets/7e7e20a0-563c-451d-8165-89c2f3570480" />

<img width="1919" height="970" alt="Screenshot 2025-10-28 183650" src="https://github.com/user-attachments/assets/944e959b-1d65-4113-a345-a7e38a4db575" />

------

## CREATE ROUTE

aws ec2 create-route \
  --route-table-id <your-rtb-id> \
  --destination-cidr-block 0.0.0.0/0 \
  --gateway-id <your-igw-id> \
  --region ap-south-2

<img width="708" height="202" alt="Screenshot 2025-10-28 183741" src="https://github.com/user-attachments/assets/a8ddf245-2188-483a-8419-b8b378a8911f" />

### VERIFY FROM CONSOLE

<img width="1919" height="973" alt="Screenshot 2025-10-28 183833" src="https://github.com/user-attachments/assets/e99a940e-9b9f-4b0e-a16d-f50facc7a386" />


------

aws ec2 associate-route-table \
  --subnet-id <your-public-subnet-id> \
  --route-table-id <your-rtb-id> \
  --region ap-south-2

<img width="794" height="256" alt="Screenshot 2025-10-28 183958" src="https://github.com/user-attachments/assets/e4205946-e773-4d32-9296-54443af98035" />

### VERIFY FROM CONSOLE

<img width="1919" height="972" alt="Screenshot 2025-10-28 184035" src="https://github.com/user-attachments/assets/590f1d39-9a14-42c0-90c7-57579b140284" />

------

### 5️⃣ Enable Auto-assign Public IP for Public Subnet

<img width="801" height="94" alt="Screenshot 2025-10-28 184301" src="https://github.com/user-attachments/assets/5d3bd1c4-9c96-42c2-b4c1-cf4ac55be0ef" />

------

### 6️⃣ Create Security Groups

Public Web SG:

aws ec2 create-security-group \
  --group-name public-web-sg \
  --description "Allow HTTP and SSH" \
  --vpc-id <your-vpc-id> \
  --region ap-south-2

<img width="1418" height="244" alt="Screenshot 2025-10-28 184431" src="https://github.com/user-attachments/assets/c527f75c-9caa-4af4-8a7f-767eae84a41b" />

### VERIFY FROM CONSOLE

<img width="1919" height="980" alt="Screenshot 2025-10-28 184506" src="https://github.com/user-attachments/assets/f548e537-f1ac-443b-b2a8-342460d2300c" />

------

### Allow HTTP and SSH:

aws ec2 authorize-security-group-ingress \
  --group-id <your-public-sg-id> \
  --protocol tcp --port 22 --cidr 0.0.0.0/0 \
  --region ap-south-2

aws ec2 authorize-security-group-ingress \
  --group-id <your-public-sg-id> \
  --protocol tcp --port 80 --cidr 0.0.0.0/0 \
  --region ap-south-2

<img width="1867" height="979" alt="Screenshot 2025-10-28 184612" src="https://github.com/user-attachments/assets/8869ed47-9d5d-410b-a0c4-2fa9c652c08e" />

### VERIFY FROM CONSOLE

<img width="1919" height="925" alt="Screenshot 2025-10-28 184633" src="https://github.com/user-attachments/assets/1be20591-6878-4677-b762-86d75971250b" />


------

Private DB SG:

aws ec2 create-security-group \
  --group-name private-db-sg \
  --description "Allow MySQL only from public subnet" \
  --vpc-id <your-vpc-id> \
  --region ap-south-2
  
<img width="1319" height="234" alt="Screenshot 2025-10-28 184734" src="https://github.com/user-attachments/assets/d9045bcb-67b2-4c80-b390-36bf7cdb22dc" />

### VERIFY FROM CONSOLE

<img width="1919" height="964" alt="Screenshot 2025-10-28 184804" src="https://github.com/user-attachments/assets/db6b5f7e-8b9a-4d4a-be05-277bd1793493" />

------

Allow traffic from Public SG:

aws ec2 authorize-security-group-ingress \
  --group-id <your-private-sg-id> \
  --protocol tcp --port 3306 --source-group <your-public-sg-id> \
  --region ap-south-2

  <img width="1554" height="498" alt="Screenshot 2025-10-28 184917" src="https://github.com/user-attachments/assets/42abcdf6-ede7-415b-8433-ccedf97a6d98" />

### VERIFY FROM CONSOLE

  <img width="1919" height="976" alt="Screenshot 2025-10-28 184928" src="https://github.com/user-attachments/assets/c0a6de3d-bd17-4ae1-8c1b-0f3351fad237" />

------


### 🔑 Create the Key Pair and Set Permissions

aws ec2 create-key-pair \
  --key-name mykeypair \
  --query "KeyMaterial" \
  --output text \
  --region ap-south-2 > mykeypair.pem

<img width="1577" height="105" alt="Screenshot 2025-10-28 185038" src="https://github.com/user-attachments/assets/d2a9e1a6-765b-438b-8c6d-59e2ef2e57dd" />

### VERIFY FROM CONSOLE

<img width="1917" height="969" alt="Screenshot 2025-10-28 185140" src="https://github.com/user-attachments/assets/6a677a50-dec3-4cf7-a049-8fb55b011539" />

------

### 7️⃣ Launch EC2 Instances

Public Web Instance (Ubuntu 22.04):

   aws ec2 run-instances \
  --image-id ami-0c6ef28989b434b51 \
  --instance-type t3.micro \
  --key-name mykeypair \
  --security-group-ids <your-public-sg-id> \
  --subnet-id <your-public-subnet-id> \
  --associate-public-ip-address \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=web-server-instance}]' \
  --region ap-south-2

<img width="1919" height="925" alt="Screenshot 2025-10-28 190156" src="https://github.com/user-attachments/assets/068b2212-3854-4f26-8cad-43e149cd127c" />

------

### Private DB Instance:

<img width="1918" height="560" alt="Screenshot 2025-10-28 190516" src="https://github.com/user-attachments/assets/73f59a3c-2c0e-44de-b7da-f24ebf4ded6b" />

------

### SSH to public Instance

<img width="1799" height="606" alt="Screenshot 2025-10-28 190726" src="https://github.com/user-attachments/assets/47dfa074-74e6-478b-a6e4-d9347f547c4d" />

------

### Ping private instance
<img width="633" height="93" alt="Screenshot 2025-10-28 190834" src="https://github.com/user-attachments/assets/402cdf28-73d8-47c5-9c50-02f562ff94e7" />

------

# Congratulations you have completed the Networking.

------

## ✅ Learning Outcomes

Create AWS VPCs and subnets via CLI

Configure Internet Gateways and Route Tables

Set up security rules for isolated networking

Launch and manage EC2 instances using AWS CLI

Understand clean deletion and resource dependency order

------

## 📚 Author : Suyash Dahitule
  
