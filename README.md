# SQL_Assignment
This project demonstrates a comprehensive application of MySQL for solving a wide range of database problems using the Classic Models dataset and custom-created schemas. The assignment covers fundamental to advanced SQL concepts, ensuring a full-spectrum demonstration of query optimization, data manipulation, and database design.


-- First Assignment Question

-- (a)

use assignment_mysql;

select * from employees;

select employeenumber, firstname, lastname from employees where jobtitle = "sales rep" and reportsto = 1102;

-- (b)

select * from products;

select distinct productline from products where productline like "%cars";


-- Second Assignment Questions

select * from customers;

select customernumber, customername, 
case when country in ("USA","Canada") then "North America"
when country in ("UK", "France", "Germany") then "Erope"
else "Others" end as CustomerSegment from customers;


-- Third Assignment Question

-- (a)

select * from orderdetails;

select productcode, sum(quantityordered) Total_ordered from orderdetails group by 1 order by 2 desc limit 10;

-- (b)

select * from payments;

select monthname(paymentdate) payment_month, count(*) num_payments from payments group by 1 having count(*) > 20 order by 2 desc;


-- Forth Assignment Question

-- (a)

create database customers_orders;

use customers_orders;

create table customers (customer_id int primary key auto_increment, first_name varchar(50) not null, last_name varchar(50) not null, 
email varchar(255) unique, phone_number varchar(20));

select * from customers;

desc customers;

-- (b)

create table orders (order_id int primary key auto_increment, customer_id int, foreign key(customer_id) references customers(customer_id), 
order_date date, total_amount decimal(10,2) check(total_amount > 0));

select * from orders;

desc orders;


-- Fifth Assignment Question

use assignment_mysql;

select * from customers;

select * from orders;

select a.country, count(*) order_count from customers as a join orders as b on a.customernumber = b.customernumber group by 1
order by 2 desc limit 5; 


-- Sixth Assignment Question

create table project (EmployeeID int primary key auto_increment, FullName varchar(50) not null, 
Gender varchar(10) check(Gender in ("Male","Female")), ManagerID int);

desc project;

insert into project (FullName, Gender, ManagerID) values ("Pranaya", "Male", 3), ("Priyanka", "Female", 1), ("Preety", "Female", null), ("Anurag", "Male", 1),
("Sambit", "Male", 1), ("Rajesh", "Male", 3), ("Hina", "Female", 3);

select * from project;

select b.FullName Emp_name, a.FullName Manager_Name from project as a join project as b on a.EmployeeID = b.ManagerID;


-- Seventh Assignment Question

create table facility (Facility_ID int, Name varchar(50), State varchar(50), Country varchar(50));

desc facility;

alter table facility modify column Facility_ID int primary key auto_increment;

desc facility;

alter table facility add column City varchar(50) after Name;

desc facility;

alter table facility modify column City varchar(50) not null;

desc facility;


-- Eighth Assignment Question

select * from productlines;

select * from products;

select * from orderdetails;

select * from orders;

create view product_category_sales as (select a.productLine, sum(c.quantityOrdered*c.priceEach) total_sales, count(distinct c.ordernumber) number_of_orders 
from productlines as a join products as b on a.productline = b.productline join orderdetails as c on b.productcode = c.productcode join
orders as d on c.ordernumber = d.ordernumber group by 1);

select * from product_category_sales;


-- Ninth Assignment Question

select * from customers;

select * from payments;

call Get_country_payments(2003,"France");


-- Tenth Assignment Question

-- (a)

select * from customers;

select * from orders;

select customerName, count(*) Order_count, dense_rank() over(order by count(*) desc) order_frequency_rnk
from customers as a join orders as b on a.customernumber = b.customernumber group by 1; 

-- (b)

select * from orders;

select Year, Month, Total_orders, concat((Total_orders - Prv_orders)*100/Prv_orders, "%") YOY_Change from (
select row_number() over(partition by year(orderdate) order by year(orderdate)), year(orderdate) Year, monthname(orderdate) Month, 
count(*) Total_orders, lag(count(*)) over() Prv_orders from orders group by 2,3) ab;


-- Eleventh Assignment Question

select * from products;

select productline, count(*) Total from products where buyprice > (select avg(buyprice) from products) group by 1 order by 2 desc;


-- Twelth Assignment Question

create table Emp_EH (EmpID int primary key, EmpName varchar(50), EmailAddress varchar(50));

desc Emp_EH;

insert into Emp_EH values (101, "Amitabh", "abcd@gmail.com"), (102, "Suraj", "abcde@gmail.com");

select * from Emp_EH;

insert into Emp_EH values (null, "Abhisek", "abcdef@gmail.com"); -- Error Cod: 1048. EmpID cannot be null

insert into Emp_EH values (101, "Suresh", "abcdefg@gmail.com"); -- Error Code: 1062. Duplicate entry 101 fro primary key

create table Error_Emp_EH (Sno int primary key auto_increment, Message text, TOT datetime);

desc Error_Emp_EH;

call Error_Emp_EH(null, "Abhisek", "abcdef@gmail.com"); -- goes to Error Handler table Error_Emp_EH

call Error_Emp_EH(101, "Suresh", "abcdefg@gmail.com"); -- goes to Error Handler table Error_Emp_EH

call Error_Emp_EH(103, "Abhisek", "abcdef@gmail.com"); -- goes to actual table Emp_EH

call Error_Emp_EH(104, "Suresh", "abcdefg@gmail.com"); -- goes to actual table Emp_EH

select * from Emp_EH;

select * from Error_Emp_EH;


-- Thirteenth Assignment Question

create table Emp_BIT (Name varchar(50), Occupation varchar(50), Working_date date, Working_hours int);

desc Emp_BIT;

insert into Emp_BIT values ("Robin", "Scientist", '2020-10-04', 12), ("Wamer", "Engineer", '2020-10-04', 10),
("Peter", "Actor", '2020-10-04', 13), ("Marco", "Doctor", '2020-10-04', 14), ("Brayden", "Teacher", '2020-10-04', 12),
("Antonio", "Business", '2020-10-04', 11);

select * from Emp_BIT;

insert into Emp_BIT values ("Amitabh", "Data Engineer", '2025-08-07', -10);

select * from Emp_BIT;

-- MySQL Assignment Completed
