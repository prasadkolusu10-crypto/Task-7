# Task 7: Creating Views in MySQL

## Objective
Learn how to create and use **views** in MySQL to simplify complex queries, enable abstraction, and enhance data security.

## What is a View?
A **view** is a virtual table created from a `SELECT` query. It does not store data but presents it in a customized way.

## Steps
 create database company;
Query OK, 1 row affected (0.01 sec)
use company;
Database changed
show tables;
create table employees (emp_id int primary key,name varchar(50),department_id int,salary decimal(10, 2));
Query OK, 
create table departments (department_id int primary key,department_name varchar(50));
Query OK, 

 desc employees;
+---------------+---------------+------+-----+---------+-------+
| Field         | Type          | Null | Key | Default | Extra |
+---------------+---------------+------+-----+---------+-------+
| emp_id        | int           | NO   | PRI | NULL    |       |
| name          | varchar(50)   | YES  |     | NULL    |       |
| department_id | int           | YES  |     | NULL    |       |
| salary        | decimal(10,2) | YES  |     | NULL    |       |
+---------------+---------------+------+-----+---------+-------+

desc departments;
+-----------------+-------------+------+-----+---------+-------+
| Field           | Type        | Null | Key | Default | Extra |
+-----------------+-------------+------+-----+---------+-------+
| department_id   | int         | NO   | PRI | NULL    |       |
| department_name | varchar(50) | YES  |     | NULL    |       |
+-----------------+-------------+------+-----+---------+-------+

mysql> insert into departments values(1, 'HR');
Query OK, 1 row affected (0.03 sec)

mysql> insert into departments values(2, 'IT');
Query OK, 1 row affected (0.01 sec)

mysql> insert into departments values(3, 'Finance');
Query OK, 1 row affected (0.01 sec)

mysql> select * from departments;
+---------------+-----------------+
| department_id | department_name |
+---------------+-----------------+
|             1 | HR              |
|             2 | IT              |
|             3 | Finance         |
+---------------+-----------------+

mysql> insert into employees values(1, 'Prasad', 1, 50000);
Query OK, 1 row affected (0.03 sec)

mysql> insert into employees values(2, 'Raju', 2, 60000);
Query OK, 1 row affected (0.02 sec)

mysql> insert into employees values(3, 'Ashok', 2, 70000);
Query OK, 1 row affected (0.01 sec)

mysql> insert into employees values(4, 'Krishna', 3, 55000);
Query OK, 1 row affected (0.01 sec)

mysql> select * from employees;
+--------+---------+---------------+----------+
| emp_id | name    | department_id | salary   |
+--------+---------+---------------+----------+
|      1 | Prasad  |             1 | 50000.00 |
|      2 | Raju    |             2 | 60000.00 |
|      3 | Ashok   |             2 | 70000.00 |
|      4 | Krishna |             3 | 55000.00 |
+--------+---------+---------------+----------+

 create view employee_view AS select e.emp_id,e.name AS employee_name,d.department_name,e.salary from employees e join departments d ON e.department_id = d.department_id;
Query OK

 select * from employee_view;
+--------+---------------+-----------------+----------+
| emp_id | employee_name | department_name | salary   |
+--------+---------------+-----------------+----------+
|      1 | Prasad        | HR              | 50000.00 |
|      2 | Raju          | IT              | 60000.00 |
|      3 | Ashok         | IT              | 70000.00 |
|      4 | Krishna       | Finance         | 55000.00 |
+--------+---------------+-----------------+----------+

 create view employee_public_view AS select name AS employee_name, department_id from  employees;
Query OK
select * from employee_public_view;
+---------------+---------------+
| employee_name | department_id |
+---------------+---------------+
| Prasad        |             1 |
| Raju          |             2 |
| Ashok         |             2 |
| Krishna       |             3 |
+---------------+---------------+

 update employee_view SET salary = 80000 WHERE emp_id = 1;
Query OK
select * from employee_view;
+--------+---------------+-----------------+----------+
| emp_id | employee_name | department_name | salary   |
+--------+---------------+-----------------+----------+
|      1 | Prasad        | HR              | 80000.00 |
|      2 | Raju          | IT              | 60000.00 |
|      3 | Ashok         | IT              | 70000.00 |
|      4 | Krishna       | Finance         | 55000.00 |
+--------+---------------+-----------------+----------+
