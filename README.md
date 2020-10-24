# Employee-Payroll-Database

#### Create Payroll Servie Database
```
CREATE payroll_service;
SHOW DATABASES;
USE payroll_service;
```
#### Create Employee Payroll Table
```
CREATE TABLE employee_payroll
(
 id	      INT UNSIGNED NOT NULL AUTO_INCREMENT,
 name         VARCHAR(50) NOT NUL,
 salary	      Double NOT NULL,
 startDate    DATE NOT NULL,
 PRIMARY KEY  (id)
);
```

#### Create Employee Payroll Data
```
INSERT INTO employee_payroll (name,salary,startDate) VALUES
	('Chandler', 1000000.00, '2018-01-03'),
	('Joey', 2000000.00, '2019-02-04'),
	('Rachel', 3000000.00, '2020-03-05');
```
#### Retrieve all Employee Payroll Data
```
SELECT * FROM employee_payroll;
```

#### Retrieve data for  particular employee
```
SELECT salary FROM employee_payroll WHERE name='Chandler';
SELECT * FROM employee_payroll WHERE startDate BETWEEN CAST('2018-01-01' AS DATE) AND DATE(NOW());
```
#### Add Gender to Employee Payroll Table
```
ALTER TABLE employee_payroll ADD gender CHAR(1) AFTER name;
UPDATE employee_payroll SET gender='F' WHERE name='Rachel';
UPDATE employee_payroll SET gender='M' WHERE name='Chandler' OR name='Joey';
```

