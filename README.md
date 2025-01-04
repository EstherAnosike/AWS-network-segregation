<h1> AWS Network Segregation </h1>

Step 1: Create a new VPC

- Go to Create VPC and set it up: 
- Name tag: test-VPC.
- IPv4 CIDR block: 172.16.0.0/16 (or another block).
- Tenancy: Default.
- Click Create.

Step 2: Create Subnets

<h3> Public Subnet: </h3>
- Name tag: frontend-subnet.
- VPC: Select test-VPC.
- Availability Zone: Choose one (e.g., London).
- CIDR block: 172.16.0.0/20.

- Click Create.

<h3> Private Subnet: </h3>
- Name tag: backend-subnet.
- VPC: Select test-VPC.
- Availability Zone: Choose one (e.g., London).
- CIDR block: 172.16.32.0/20.

- Click Create.

Step 3: Set up Internet Gateway

- Create Internet Gateway (IGW):
- Go to Internet Gateways > Create Internet Gateway.
- Name tag: test-ig.
- Click Create.

- Attach IGW to VPC:
- Select test-ig.
- Click Actions > Attach to VPC.
- Choose test-VPC.

Step 4: Create a NAT Gateway

- Create Elastic IP:
- Navigate to Elastic IP Addresses > Allocate Elastic IP Address.
- Click Allocate.

- Create NAT Gateway:
- Go to NAT Gateways > Create NAT Gateway.
- Name tag: test-ng.
- Subnet: Choose backend-subnet
- Elastic IP allocation ID: Select the Elastic IP.
- Click Create.

Step 5: Configure Route Tables

<h3> Public Route Table: </h3>
- Go to Route Tables > Create Route Table.
- Name tag: front-rt.
- VPC: Select test-VPC.
- Click Create.

- Add Routes: > Go to the Routes tab > Edit routes.
- Add:
- Destination: 0.0.0.0/0.
- Target: Internet Gateway (Select test-ig).
- Click Save routes.

- Associate Public Subnet: > Go to the Subnet Associations tab > Edit subnet associations.
- Select frontend-subnet.
- Click 'Save'.

<h3> Private Route Table: </h3>
- Go to Route Tables > Create Route Table.
- Name tag: back-rt.
- VPC: Select test-VPC.
- Click Create.

- Add Routes: > Go to the Routes tab > Edit routes.
- Add:
- Destination: 0.0.0.0/0.
- Target: NAT Gateway (Select test-ng).
- Click Save routes.

- Associate Private Subnet: > Go to the Subnet Associations tab > Edit subnet associations.
- Select backend-subnet.
- Click 'Save'.

Step 6: Lauch the Instances

<h4> Launch EC2 in Public Subnet: </h4>
- Go to EC2 Dashboard > Launch Instance.
- Choose an AMI (e.g., Amazon Linux 2).
- Choose an instance type (e.g., t2.micro).

- Configure Instance Details:
- Network: Select test-VPC.
- Subnet: Select frontend-subnet.
- Auto-assign Public IP: Enable.
- Launch the instance.

<h4> Launch EC2 in Private Subnet: </h4>
- Repeat the process above, but: 

- Subnet: Select backend-subnet.
- Auto-assign Public IP: Disable.

Step 7: Test Connectivity

<h4> Public Subnet Instance: </h4>
- Use the public IP to SSH or access the instance.

<h4> Private Subnet Instance: </h4>
- Access the private instance via the public instance using SSH or a bastion host.