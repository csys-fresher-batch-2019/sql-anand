# STUDENT FEE MANAGEMENT SYSTEM

## Overview
 
  This system manages the fee paid by the students and this authenticates for the efficient processing over auditing by authorities at the end of each academic year.

## Features

### Feature 1 : ADD COURSES

To add departments, degrees preceeding to signify courses run by the institution.

#### Query:

~~~ sql

create table department
(
dept_id number,
dept_name varchar2(20),
dept_status number DEFAULT 1,
constraint dept_id_pk primary key (dept_id),
constraint dept_name_uq unique (dept_name),
constraint dept_status_ck check (dept_status in (1,0))
);

create table degree
(
deg_id number,
deg_name varchar2(10),
no_of_sem number,
deg_status number DEFAULT 1,
constraint deg_id_pk primary key (deg_id),
constraint deg_name_uq unique (deg_name),
constraint deg_status_ck check (deg_status in (1,0)),
constraint deg_combine_uq unique(deg_name,no_of_sem)
);

create table course
(
course_id number,
deg_id number,
dept_id number,
course_status number DEFAULT 1,
constraint course_id_pk primary key (course_id),
constraint deg_id_fk foreign key (deg_id) references degree (deg_id),
constraint dept_id_fk foreign key (dept_id) references department(dept_id),
constraint course_comb_uk unique (deg_id,dept_id),
constraint course_status_ck check (course_status in(1,0))
);

### Feature 2 : ADD SEMESTER

To add semester to the built system so as to differentiate between semester of payment made.

#### Query:

create table semester
(
sem_id number,
sem_type varchar2(5),
acc_yr_begin number,
constraint sem_id_pk primary key (sem_id),
constraint sem_type_ck check (sem_type in('ODD','EVEN')),
constraint sem_comb unique (sem_type,acc_yr_begin)
);

