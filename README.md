
# Lami Cleaning Services Database

# Overview
Lami Cleaning Services database is a database system designed to manage client appointments, cleaning services, employees, payments, and feedback efficiently of Lami Cleaning Service Company. This database helps track service bookings, employee assignments, and customer interactions.

# Database Schema
The database consists of the following tables:

# 1. Clients
Stores customer information.
```sql
CREATE TABLE Clients (
    Client_ID INT PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE,
    Phone VARCHAR(15),
    Address TEXT
);
```

# 2. Cleaning_Services
Stores different cleaning services offered.
```sql
CREATE TABLE Cleaning_Services (
    Service_ID INT PRIMARY KEY AUTO_INCREMENT,
    Service_Name VARCHAR(100) NOT NULL,
    Description TEXT,
    Price DECIMAL(8,2)
);
```

# 3. Employees
Stores employee details and roles.
```sql
CREATE TABLE Employees (
    Employee_ID INT PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(100) NOT NULL,
    Phone VARCHAR(15),
    Role ENUM('Cleaner', 'Supervisor', 'Manager') DEFAULT 'Cleaner'
);
```

# 4. Appointments
Stores customer appointments and assigned employees.
```sql
CREATE TABLE Appointments (
    Appointment_ID INT PRIMARY KEY AUTO_INCREMENT,
    Client_ID INT,
    Service_ID INT,
    Employee_ID INT,
    Appointment_Date DATETIME,
    Status ENUM('Scheduled', 'Completed', 'Cancelled') DEFAULT 'Scheduled',
    FOREIGN KEY (Client_ID) REFERENCES Clients(Client_ID),
    FOREIGN KEY (Service_ID) REFERENCES Cleaning_Services(Service_ID),
    FOREIGN KEY (Employee_ID) REFERENCES Employees(Employee_ID)
);
```

# 5. Payments
Stores payment transactions for completed appointments.
```sql
CREATE TABLE Payments (
    Payment_ID INT PRIMARY KEY AUTO_INCREMENT,
    Client_ID INT,
    Appointment_ID INT,
    Amount DECIMAL(8,2),
    Payment_Date DATETIME DEFAULT CURRENT_TIMESTAMP,
    Payment_Method ENUM('Cash', 'Credit Card', 'Bank Transfer'),
    FOREIGN KEY (Client_ID) REFERENCES Clients(Client_ID),
    FOREIGN KEY (Appointment_ID) REFERENCES Appointments(Appointment_ID)
);
```

# 6. Feedback
Stores customer feedback on services received.
```sql
CREATE TABLE Feedback (
    Feedback_ID INT PRIMARY KEY AUTO_INCREMENT,
    Client_ID INT,
    Appointment_ID INT,
    Rating INT CHECK (Rating BETWEEN 1 AND 5),
    Comments TEXT,
    Feedback_Date DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (Client_ID) REFERENCES Clients(Client_ID),
    FOREIGN KEY (Appointment_ID) REFERENCES Appointments(Appointment_ID)
);
```

## Installation & Usage

# 1. Clone the Repository
```bash
git clone https://github.com/your-username/lami-cleaning-services-db.git
cd lami-cleaning-services-db
```

# 2. Import the Database
- Open MySQL Workbench or phpMyAdmin
- Create a new database:
```sql
CREATE DATABASE LamiCleaningServices;
USE LamiCleaningServices;
```
- Import `lami cleaning service db.sql` file:
```bash
mysql -u root -p LamiCleaningServices < db.sql
```

# 3. Verify the Tables
Run the following SQL command:
```sql
SHOW TABLES;
```

# 4. Sample Query
Fetch all scheduled appointments with client and service details:
```sql
SELECT a.Appointment_ID, c.Name AS Client, s.Service_Name, a.Appointment_Date, a.Status
FROM Appointments a
JOIN Clients c ON a.Client_ID = c.Client_ID
JOIN Cleaning_Services s ON a.Service_ID = s.Service_ID
WHERE a.Status = 'Scheduled';
```

# Features
 Manage client bookings & service history  
 Assign employees to appointments  
 Track payments & pending balances  
 Collect customer feedback & ratings  



