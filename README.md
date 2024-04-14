###An application for managing employee profiles using AWS services:

![aws logo](https://github.com/AbhiGundim/AWS-Enabled-Employee-Profile-Management-System/assets/124610756/b9d3d8fa-3bb4-42da-9c97-2b39ecf44e56)


![pr21](https://github.com/AbhiGundim/AWS-Enabled-Employee-Profile-Management-System/assets/124610756/344b693b-460c-49e8-b2e2-4af56a65b878)

![pr22](https://github.com/AbhiGundim/AWS-Enabled-Employee-Profile-Management-System/assets/124610756/aa596248-3cc7-4711-9d3a-2564b3739ee9)

![pr23](https://github.com/AbhiGundim/AWS-Enabled-Employee-Profile-Management-System/assets/124610756/205bd324-f8d0-49cf-9a45-f766eeef7bfb)

![pr24](https://github.com/AbhiGundim/AWS-Enabled-Employee-Profile-Management-System/assets/124610756/4e55cadd-b381-41b0-ba87-0098bf0c6192)


### AWS Setup:

#### 1. VPC Setup:
- **Create a VPC**:
  - Navigate to the VPC dashboard.
  - Click on "Create VPC".
  - Name: `vpc-project`
  - IPv4 CIDR Block: `20.20.0.0/16`
  - Tenancy: Default

- **Create Subnets**:
  - **Public Subnets**:
    - Subnet 1:
      - Name: `public-01`
      - Availability Zone: `us-east-1a`
      - IPv4 CIDR Block: `20.20.1.0/24`
    - Subnet 2:
      - Name: `public-02`
      - Availability Zone: `us-east-1b`
      - IPv4 CIDR Block: `20.20.2.0/24`
  - **Private Subnet**:
    - Subnet 3:
      - Name: `private-01`
      - Availability Zone: `us-east-1a`
      - IPv4 CIDR Block: `20.20.3.0/24`

#### 2. Internet Gateway (IGW) and Route Table:
- **Create Internet Gateway**:
  - Name: `igw-project`
  - Attach to VPC `vpc-project`

- **Route Table Configuration**:
  - **Public Route Table (`public-RT`)**:
    - Associate with `public-01` and `public-02` subnets.
    - Add route `0.0.0.0/0` to IGW `igw-project`.

- **Private Route Table (`private-RT`)**:
  - Associate with `private-01` subnet.

#### 3. NAT Gateway:
- **Create NAT Gateway**:
  - Allocate Elastic IP.
  - Attach to `public-01` subnet.

#### 4. EC2 Instances:
- **Launch EC2 Instances**:
  - **Public Instance (`public-project`)**:
    - AMI: Ubuntu
    - Subnet: `public-01`
    - Security Group: `project` (Allow HTTP from anywhere)
  - **Private Instance (`private`)**:
    - AMI: Ubuntu
    - Subnet: `private-01`
    - Security Group: `project`

#### 5. S3 Bucket:
- **Create S3 Bucket**:
  - Name: `myproject-abhig`
  - Region: `US East (N. Virginia)`
  - Configuration:
    - Enable versioning
    - Block all public access
    - Server-side encryption: SSE-S3

#### 6. RDS Database:
- **Create RDS Database**:
  - Engine: MySQL
  - Templates: Free tier
  - Master Username/Password: `admin`/`admin123`
  - VPC: `vpc-project`
  - Security Group: `project`
  - Disable Backup

#### 7. DynamoDB Table:
- **Create DynamoDB Table**:
  - Table Name: `employee_image_table`
  - Partition Key: `empid` (String)

#### 8. IAM Role:
- **Create IAM Role**:
  - Role Name: `myproject-role`
  - Trusted Entity: EC2
  - Attach Policies: `AmazonS3FullAccess`, `AmazonRDSFullAccess`, `AmazonDynamoDBFullAccess`

#### 9. Load Balancer:
- **Create Application Load Balancer**:
  - Name: `project-LB`
  - VPC: `vpc-project`
  - Listener & Routing:
    - Create Target Group (`pro-tar`) with private instance.

### Application Deployment:

#### 1. Connect to Public Instance:
- **SSH into Public Instance**:
  ```bash
  ssh -i key.pem ubuntu@<public-instance-IP>
  ```
  
#### 2. Setup Environment on Public Instance:
- **Install Prerequisites**:
  ```bash
  sudo apt update
  sudo apt install mysql-client python3 python3-flask python3-pymysql python3-boto3
  ```

#### 3. Configure and Run Application:
- **Clone Repository**:
  ```bash
  git clone https://github.com/sauravinside/aws-code-main
  ```

- **Configure Application**:
  - Navigate to the `aws-code-main` directory.
  - Create `config.py` and enter necessary details (e.g., RDS endpoint, database credentials).

- **Run Application**:
  - Execute the application:
    ```bash
    python3 EmpApp.py
    ```

#### 4. Validate Application:
- Access the Load Balancer DNS in a web browser.
- Interact with the application to input employee data.
- Verify data storage and retrieval across services (RDS, DynamoDB, S3).

### Additional Notes:
- Ensure proper security configurations and access controls are implemented throughout the setup.
- Regularly monitor and manage resources to optimize costs and performance.

By following these steps, you can successfully set up and deploy the employee profile management application using various AWS services as described in your project agenda. Adjustments may be needed based on specific requirements and configurations.

![prj1](https://github.com/AbhiGundim/AWS-Enabled-Employee-Profile-Management-System/assets/124610756/e50c98a6-9591-40dc-82cf-5e56f6cdcf33)

![prj2](https://github.com/AbhiGundim/AWS-Enabled-Employee-Profile-Management-System/assets/124610756/6b2e4219-8d14-476c-9e7c-c234586f3c6e)


To complement the steps outlined for setting up an application using AWS services, here are some additional resources and links that can provide further guidance and detailed documentation on each service and task involved in your project:

### AWS Documentation and Guides:

1. **Amazon Virtual Private Cloud (VPC):**
   - [AWS VPC Documentation](https://docs.aws.amazon.com/vpc/)
   - [VPC Setup Guide](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)

2. **Amazon EC2 Instances:**
   - [Amazon EC2 Documentation](https://docs.aws.amazon.com/ec2/)
   - [EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)

3. **Amazon S3 (Simple Storage Service):**
   - [Amazon S3 Documentation](https://docs.aws.amazon.com/s3/)
   - [Getting Started with Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/gsg/GetStartedWithS3.html)

4. **Amazon RDS (Relational Database Service):**
   - [Amazon RDS Documentation](https://docs.aws.amazon.com/rds/)
   - [Creating an RDS DB Instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.MySQL.html)

5. **Amazon DynamoDB:**
   - [Amazon DynamoDB Documentation](https://docs.aws.amazon.com/dynamodb/)
   - [Getting Started with DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)

6. **Identity and Access Management (IAM):**
   - [AWS IAM Documentation](https://docs.aws.amazon.com/IAM/)
   - [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)

7. **Elastic Load Balancing (ELB):**
   - [Elastic Load Balancing Documentation](https://docs.aws.amazon.com/elasticloadbalancing/)
   - [Application Load Balancer Guide](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html)

8. **AWS CLI (Command Line Interface):**
   - [AWS CLI Documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)
   - [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)

### Additional Learning Resources:

- [AWS Training and Certification](https://aws.amazon.com/training/)
  - Offers free and paid training courses, labs, and certification paths for various AWS services.

- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
  - Best practices and design principles for building secure, high-performing, resilient, and efficient applications on AWS.

- [AWS Architecture Center](https://aws.amazon.com/architecture/)
  - Provides architecture diagrams, design patterns, and reference architectures for building applications on AWS.

- [AWS Developer Forums](https://forums.aws.amazon.com/)
  - Community-driven forums to ask questions, share experiences, and learn from other AWS users.

### GitHub Repository:

The GitHub repository link you provided contains specific code (`EmpApp.py`) for the application. Ensure you review and understand the code to integrate it correctly with your AWS setup.

- [GitHub Repository](https://github.com/sauravinside/aws-code-main)

### Conclusion:

These resources, combined with the detailed steps provided earlier, will assist you in effectively setting up, configuring, and deploying your application using AWS services. It's essential to refer to official AWS documentation and leverage community resources to ensure best practices and optimize your AWS environment. If you have specific questions or encounter challenges during implementation, feel free to explore the AWS community forums or seek further assistance from AWS support resources.


