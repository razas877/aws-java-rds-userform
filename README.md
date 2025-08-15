# aws-java-rds-userform
A simple Java application on AWS EC2 that takes user input and stores it in an Amazon RDS MySQL database.

## Steps to Setup and Run
**1. Create EC2 Instance**
- Launch EC2 (Amazon Linux 2, t2.micro).
- Allow inbound ports:
  - **22 (SSH)** for remote login
  - **3306 (MySQL)** for database access

**2. Create RDS MySQL Database**
- Create MySQL database in Amazon RDS.
- In RDS Security Group → Add inbound rule:
  - Type: MySQL/Aurora
  - Source: `0.0.0.0/0` *(for demo only)*

**3. Connect RDS to MySQL Workbench**
- Use RDS endpoint, username, and password.
- Create database and table:

CREATE DATABASE userdb;
USE userdb;
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    age INT,
    mobile VARCHAR(15)
);

**4. Install Java on EC2**
sudo yum install java-17-amazon-corretto -y
sudo yum install java-17-amazon-corretto-devel -y
java -version

**5. Create Java Program on EC2**
Create UserDatabaseApp.java
Add your RDS credentials:
private String jdbcURL = "jdbc:mysql://<RDS-ENDPOINT>:3306/userdb";
private String jdbcUsername = "<USERNAME>";
private String jdbcPassword = "<PASSWORD>";

**6. Download MySQL Connector/J Driver**
wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-j-8.4.0.tar.gz
tar -xvzf mysql-connector-j-8.4.0.tar.gz

**7. Compile and Run Java Program**
javac -cp .:mysql-connector-j-8.4.0/mysql-connector-j-8.4.0.jar UserDatabaseApp.java
java  -cp .:mysql-connector-j-8.4.0/mysql-connector-j-8.4.0.jar UserDatabaseApp

**8. Verify in MySQL Workbench**
Check the users table — the record should be stored.

**Example Flow**
1)User runs program on EC2.
2)Enters Name, Age, Mobile.
3)Java program inserts data into RDS.
4)Data visible in MySQL Workbench.

