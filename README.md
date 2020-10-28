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
#### Use Database Functions
```
SELECT SUM(salary) FROM employee_payroll WHERE gender='M' GROUP BY gender;
SELECT SUM(salary) FROM employee_payroll WHERE gender='F' GROUP BY gender;
SELECT AVG(salary) FROM employee_payroll WHERE gender='M' GROUP BY gender;
SELECT AVG(salary) FROM employee_payroll WHERE gender='F' GROUP BY gender;
SELECT gender, MAX(salary) FROM employee_payroll GROUP BY gender;
SELECT gender, MIN(salary) FROM employee_payroll GROUP BY gender;
SELECT gender, COUNT(name) FROM employee_payroll GROUP BY gender;
```

#### Add Phone, Address and Department to Employee Payroll Table
```
ALTER TABLE employee_payroll ADD phone VARCHAR(20) AFTER name;
ALTER TABLE employee_payroll ADD address VARCHAR(250) AFTER phone;
ALTER TABLE employee_payroll ADD department VARCHAR(20) NOT NULL DEFAULT 'TBD' AFTER address;
```

#### Add Payroll details to Employee Payroll Table
```
ALTER TABLE employee_payroll RENAME COLUMN salary TO basic_pay;
ALTER TABLE employee_payroll ADD deductions DOUBLE NOT NULL AFTER basic_pay;
ALTER TABLE employee_payroll ADD taxable_pay DOUBLE NOT NULL AFTER deductions;
ALTER TABLE employee_payroll ADD income_tax DOUBLE NOT NULL AFTER taxable_pay;
ALTER TABLE employee_payroll ADD net_pay DOUBLE NOT NULL AFTER income_tax;
```
#### Update Employee as part of Sales and marketing Department
```
UPDATE employee_payroll SET department='Sales' WHERE name='Rachel';
INSERT INTO employee_payroll 
(name, department, gender, basic_pay, deductions, taxable_pay, income_tax, net_pay, startDate) VALUES
('Rachel', 'Marketing', 'F', 4000000.00, 1000000.00, 3000000.00, 500000.00, 2500000.00, '2020-03-05');
```

#### Implement ER Diagram
```
DROP TABLE employee_payroll;

CREATE TABLE company (
    company_id INT NOT NULL PRIMARY KEY,
    company_name VARCHAR(50) NOT NULL
);

CREATE TABLE employee (
    employee_id INT UNSIGNED NOT NULL PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    company_id INT,
    phone_number VARCHAR(50) NOT NULL,
    address VARCHAR(250) NOT NULL,
    gender CHAR(1),
    start_date DATE NOT NULL,
    FOREIGN KEY (company_id) REFERENCES company (company_id)
);

CREATE TABLE department (
    department_id INT NOT NULL PRIMARY KEY,
    department_name VARCHAR(50) NOT NULL
);

CREATE TABLE payroll (
    employee_id INT UNSIGNED NOT NULL,
    basic_pay DOUBLE NOT NULL,
    deductions DOUBLE NOT NULL,
    taxable_pay DOUBLE NOT NULL,
    income_tax DOUBLE NOT NULL,
    net_pay DOUBLE NOT NULL,
    FOREIGN KEY (employee_id) REFERENCES employee (employee_id)
);

CREATE TABLE employee_department (
    employee_id INT UNSIGNED NOT NULL,
    department_id INT NOT NULL,
    FOREIGN KEY (employee_id) REFERENCES employee (employee_id),
    FOREIGN KEY (department_id) REFERENCES department (department_id)
);
```

#### Insert data into Tables
```
Insert INTO company VALUES (1, 'Company-A');

Insert INTO employee VALUES (101, 'Bill', 1 , 9876543210, 'Maharashtra', 'M', '2018-01-03');
Insert INTO employee VALUES (102, 'Terisa', 1 , 8765432109, 'Delhi', 'F', '2018-02-04');
Insert INTO employee VALUES (103, 'Charlie', 1 , 7654321098, 'Karnataka', 'M', '2018-03-05');
Insert INTO employee VALUES (104, 'Mark', 1 , 7689543210, 'Telangana', 'M', '2018-02-06');

Insert INTO department VALUES (50, 'Sales');
Insert INTO department VALUES (51, 'Marketing');
Insert INTO department VALUES (52, 'HR');

Insert INTO payroll VALUES (101,1000000.00, 100000.00, 900000.00, 100000.00, 800000.00);
Insert INTO payroll VALUES (102,2000000.00, 100000.00, 1900000.00, 100000.00, 1800000.00);
Insert INTO payroll VALUES (103,3000000.00, 100000.00, 2900000.00, 100000.00, 2800000.00);
Insert INTO payroll VALUES (104,4000000.00, 100000.00, 3900000.00, 100000.00, 3800000.00);

Insert INTO employee_department VALUES (101,50);
Insert INTO employee_department VALUES (101,51);
Insert INTO employee_department VALUES (102,51);
Insert INTO employee_department VALUES (103,50);
Insert INTO employee_department VALUES (102,50);
```


#### Retrieve Data from Table
```
SELECT employee.name, payroll.net_pay FROM payroll
INNER JOIN employee ON payroll.employee_id = employee.employee_id
WHERE employee.name = 'Bill';

SELECT gender, SUM(payroll.basic_pay) FROM employee
INNER JOIN payroll ON payroll.employee_id = employee.employee_id
GROUP BY employee.gender;

SELECT gender, MIN(payroll.basic_pay) FROM employee
INNER JOIN payroll ON payroll.employee_id = employee.employee_id
GROUP BY employee.gender;

SELECT gender, MAX(payroll.basic_pay) FROM employee
INNER JOIN payroll ON payroll.employee_id = employee.employee_id
GROUP BY employee.gender;

SELECT gender, COUNT(payroll.basic_pay) FROM employee
INNER JOIN payroll ON payroll.employee_id = employee.employee_id
GROUP BY employee.gender;
```