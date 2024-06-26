create table Flights(fno int primary key , source varchar(255),destination varchar(255),distance int,departure time,arrives time);

create table Aircraft(aid int primary key,aname varchar(255),cruisingrange int);

create table Employees(eid int primary key,ename varchar(255),salary decimal);

create table Certified(eid int,aid int,primary key(eid,aid),foreign key (eid) references Employees(eid),foreign key (aid) references Aircraft(aid));

insert into Flights values
-> (1,"City A","City B",500,"08:00:00","10:00:00"),
-> (2,"New york","London",3450,"22:00:00","10:00:00"),
-> (3,"Los angeles","Tokyo",5470,"16:00:00","20:00:00"),
-> (4,"Chicago","Miami",1180,"09:00:00","12:30:00"),
-> (5,"Paris","Dubai",5240,"21:30:00","06:00:00"),
-> (6,"Sydney","Singapore",6290,"11:00:00","17:00:00"),
-> (7,"Hongkong","Sanfrancisco",6900,"12:00:00","10:30:00"), -> (8,"Amsterdan","Toronto",3730,"13:00:00","15:00:00"),
-> (9,"Madrid","New york",3590,"08:30:00","11:00:00"),
-> (10,"Beijing","Moscow",3600,"10:00:00","14:00:00");
select * from Flights;


insert into Aircraft values -> (1,"Boeing 737",3000),
-> (2,"Airbus A320",3300),
-> (3,"Boeing 747",8000),
-> (4,"Boeing 777",7400),
(5,"Airbus A380",15200),
-> (6,"Cessna 172",1280),
-> (7,"Boeing 787",13800),
-> (8,"Airbus A350",8100),
-> (9,"Lockheed Martin F-22",1840), -> (10,"Bombardier CRJ200",1951);
select * from Aircraft;


insert into Employees -> values
-> (1,"John doe",120000),
-> (2,"Jane smith",95000), 
-> (3,"Ahmed khan",78000), 
-> (4,"Liu wei",65000),
-> (5,"Carlos lopez",72000), 
-> (6,"Anna ivanova",80000),
-> (7,"Aisha A1-hamadi",88000),
-> (8,"Thomas muller",76000),
-> (9,"Marie-claire dubois",82000),
-> (10,"Raj patel",91000);
select * from Employees;


insert into Certified values 
-> (1,1),
-> (2,3), 
-> (3,2), 
-> (4,4), 
-> (5,5), 
-> (6,6), 
-> (7,7), 
-> (8,8), 
-> (9,9),
-> (10,10);
select * from Certified;


mysql>selectDISTINCTeid
-> from Certified
-> join Aircraft on Certified.aid=Aircraft.aid 
-> where Aircraft.aname like "Boeing%";


mysql>selectDISTINCTename
-> from Employees
-> join Certified on Employees.eid=Certified.eid -> join Aircraft on Certified.aid=Aircraft.aid
-> where Aircraft.aname like "Boeing%";


mysql>selectaid
-> from Aircraft
-> where cruisingrange>=(select distance from Flights where
source="Bonn" and destination="Madras"); Empty set (0.00 sec)


mysql>selectfno
-> from Flights
-> where not exists(select * from Employees where
salary>100000 and not exists(select *from Certified where Certified.eid=Employees.eid and Certified.aid in (select DISTINCT aid from Flights)));


mysql>selectDISTINCTename
-> from Employees
-> join Certified on Employees.eid=Certified.eid
-> join Aircraft on Certified.aid=Aircraft.aid
-> where cruisingrange>3000 and Aircraft.aname not like
"Boeing%" and Employees.eid not in (select distinct eid from Certified join Aircraft on Certified.aid=Aircraft.aid where Aircraft.aname like "Boeing%");



