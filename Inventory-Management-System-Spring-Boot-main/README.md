
# 🚀 Java Project Deployment on AWS EC2 (Ubuntu)

Deploy your Java (Spring Boot) web application on an AWS EC2 instance running Ubuntu. This guide includes setup with Nginx as the reverse proxy and Amazon RDS (MySQL) for database integration.

---

## ⚙️ Requirements

- AWS EC2 (Ubuntu)
- Amazon RDS (MySQL)
- Java (JDK 17)
- Maven
- Git
- MySQL Client (installed locally for testing)

---

## 🛠️ AWS Setup: VPC, Subnets, Route Table, Internet Gateway and EC2 Instance

## 1. Create a VPC
# Set the following:
   - **VPC Name:** `vpc-project`
   - **IPv4 CIDR block:** `10.0.0.0/24`
   - Set **Tenancy** as `Default`.
   - Click **Create VPC**.

![image alt](https://github.com/riyaj-2002/Inventory-Management-System-Spring-Boot-project/blob/259442aa8934f53c449f5f2d36b8aeb72c1369e9/Inventory-Management-System-Spring-Boot-main/Screenshot%202025-04-23%20204426.png)   

## 2. Create Subnets
### Public Subnet:
# Set the following for the public subnet:
   - **VPC**: Select the VPC.
   - **Subnet name**: `pub-sub-project`
   - **CIDR block**: `10.0.0.1/24`
# Click **Create Subnet**.

### Private Subnet:
# Set the following for the private subnet:
   - **VPC**: Select the VPC.
   - **Subnet name**: `pri-sub-project`
   - **CIDR block**: `10.0.0.128/24`
# Click **Create Subnet**.

![image alt](https://github.com/riyaj-2002/Inventory-Management-System-Spring-Boot-project/blob/f42a1364b34b1f0b437a37544a7f05a12d394e56/Inventory-Management-System-Spring-Boot-main/Screenshot%202025-04-23%20204445.png)

## 3. Create an Internet Gateway (IGW)
# Set the following for the private subnet:
  - **IGW Name:** `igw-project`
# Attach the IGW:
   - Select the VPC and click **Attach**.

![image alt](https://github.com/riyaj-2002/Inventory-Management-System-Spring-Boot-project/blob/f42a1364b34b1f0b437a37544a7f05a12d394e56/Inventory-Management-System-Spring-Boot-main/Screenshot%202025-04-23%20204548.png)

## 4. Create a Route Table (RT)
# Set the following:
  - **RT Name:** `rt-project`
  - Add a route: `0.0.0.0/0` → Target: `igw-project`.
  - Associate the Route Table with the Public Subnet and Private Subnet

![image alt](https://github.com/riyaj-2002/Inventory-Management-System-Spring-Boot-project/blob/f42a1364b34b1f0b437a37544a7f05a12d394e56/Inventory-Management-System-Spring-Boot-main/Screenshot%202025-04-23%20204525.png)

---

## 🛠️ EC2 Instance Setup

## 1. Launch EC2 Instance
# Set the following:
   - **InstanceType:** t2.medium
   - **KeyName:** key.pair.pem
   - **VPC:** vpc-project
   - **SUBNET:** pub-sub-project
   - **SECURITY GROUP:** SSH (port 22) , HTTP (port 80) , HTTPS (port 443) ,  
    MySQL  (3306)

![image alt](https://github.com/riyaj-2002/Inventory-Management-System-Spring-Boot-project/blob/f42a1364b34b1f0b437a37544a7f05a12d394e56/Inventory-Management-System-Spring-Boot-main/Screenshot%202025-04-25%20165620.png)

---


## 🛠️ RDS Setup (MySQL)

### 1. Create RDS DB Instance
- **Engine:** MySQL
- **DB Identifier:** `javadb`
- **Master Username/Password:** `admin / btxxxxxx`
- **VPC:** `vpc-project`
- **Subnet Group:** 'pub-sub-project'
- **Public Access:** NO
- **Security Group:** Allow access from EC2’s security group
- Attach the RDS to EC2 instance
### 2. Note Down RDS Endpoint
Format:  
```
javadb.xxxxxxxxxxxx.<ap-south-1>.rds.amazonaws.com:3306
```

---

![image alt](https://github.com/riyaj-2002/Inventory-Management-System-Spring-Boot-project/blob/f42a1364b34b1f0b437a37544a7f05a12d394e56/Inventory-Management-System-Spring-Boot-main/Screenshot%202025-04-25%20165849.png)


![image alt](https://github.com/riyaj-2002/Inventory-Management-System-Spring-Boot-project/blob/f42a1364b34b1f0b437a37544a7f05a12d394e56/Inventory-Management-System-Spring-Boot-main/Screenshot%202025-04-25%20162350.png)


## 🛠️ EC2 Configuration & Project Setup

```bash
# 1. SSH into EC2
ssh -i "key.pair.pem" ubuntu@3.108.42.210

# 2. Update & Install Java, Maven, Git
sudo apt update && sudo apt upgrade -y
sudo apt install openjdk-17-jdk maven git -y

# 3. (Optional) Install MySQL Client (for testing RDS connection)
sudo apt install mysql-client -y

# 4. Clone Java Project
git clone https://github.com/riyaj-2002/Inventory-Managemet-System-Spring-Boot-project.git
cd your-java-project

# 5. Build the Project
mvn clean package
```

---

## ⚙️ Configure Spring Boot with RDS (MySQL)

In `src/main/resources/application.properties`:

```properties
# Replace with your actual RDS endpoint and DB credentials
spring.datasource.url=jdbc:mysql://javadb.xxxxxxxxxxx<region>.rds.amazonaws.com:3306/javadb? 
    useSSL=false&serverTimezone=UTC&useLegacyDatetimeCode=false&allowPublicKeyRetrieval=true
spring.datasource.username=admin
spring.datasource.password=btxxxxxx
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
```

---

## 🚀 Run the Application

```bash
# Replace with your built JAR name
java -jar target/Inventory-Managemet-System-Spring-Boot-main.jar
```

---

## ✅ Application is Live!

Visit:  
**http://3.108.42.210:8080** 

---

![image alt](https://github.com/riyaj-2002/Inventory-Management-System-Spring-Boot-project/blob/f42a1364b34b1f0b437a37544a7f05a12d394e56/Inventory-Management-System-Spring-Boot-main/Screenshot%202025-04-25%20165551.png)

![image alt](https://github.com/riyaj-2002/Inventory-Management-System-Spring-Boot-project/blob/f42a1364b34b1f0b437a37544a7f05a12d394e56/Inventory-Management-System-Spring-Boot-main/Screenshot%202025-04-25%20170013.png)



