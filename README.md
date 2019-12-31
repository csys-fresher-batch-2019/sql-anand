# STUDENT FEE AUDITING SYSTEM

## Overview
 
  This system manages the fee paid by the students and this authenticates for the efficient processing over auditing by authorities at the end of each academic year.

## Features

### Feature 1 : ADD COURSES

To add departments, degrees preceeding to signify courses run by the institution.

#### Query:

```sql

create table department
(
dept_id number,
dept_name varchar2(20),
dept_active number DEFAULT 1,
constraint dept_id_pk primary key (dept_id),
constraint dept_name_uq unique (dept_name),
constraint dept_active_ck check (dept_active in (1,0))
);

````

| dept_id | dept_name | dept_active |
|---------|-----------|-------------|
| 1       | english   | 1           |
| 2       | tamil     | 1           |
| 3       | maths     | 1           |

```

create table degree
(
deg_id number,
deg_name varchar2(10),
no_of_yr number,
deg_active number DEFAULT 1,
constraint deg_id_pk primary key (deg_id),
constraint deg_name_uq unique (deg_name),
constraint no_of_yr_ck check (no_of_yr<=4)),
constraint deg_active_ck check (deg_active in(1,0)),
constraint deg_combine_uq unique(deg_name,no_of_yr)
);

```

| deg_id | deg_name | deg_active |
|--------|----------|------------|
| 1      | BA       | 1          |
| 2      | Bsc      | 1          |
| 3      | MA       | 1          |


```

create table course
(
course_id number,
deg_id number,
dept_id number,
course_active number DEFAULT 1,
constraint course_id_pk primary key (course_id),
constraint deg_id_fk foreign key (deg_id) references degree (deg_id),
constraint dept_id_fk foreign key (dept_id) references department(dept_id),
constraint course_comb_uk unique (deg_id,dept_id),
constraint course_active_ck check (course_active in(1,0))
);

``` 
| course_id | deg_id | dept_id | course_active |
|-----------|--------|---------|---------------|
| 1         | 1      | 1       | 1             |
| 2         | 1      | 2       | 1             |
| 3         | 2      | 3       | 1             |
| 4         | 3      | 1       | 1             |


### Feature 2 : ADD SEMESTER

To add semester to the built system so as to differentiate between semesters of payment.

#### Query:

```sql

create table semester
(
sem_id number,
sem_type number,
acc_yr_begin number not null,
constraint sem_id_pk primary key (sem_id),
constraint sem_type_ck check (sem_type in(0,1)),
constraint sem_comb unique (sem_type,acc_yr_begin)
);

```

| sem_id | sem_type | acc_yr_begin |
|--------|----------|--------------|
| 1      | 0        | 2019         |
| 2      | 1        | 2019         |
| 3      | 0        | 2020         |
| 4      | 1        | 2020         |
