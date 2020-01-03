# STUDENT FEE PAYMENT AUDITING SYSTEM

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
constraint no_of_yr_ck1 check (no_of_yr<=4)),
constraint no_of_yr_ck2 check (no_of_yr>=1)),
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


### Feature 2 : ADD SEMESTER and STUDENT

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

```

create table student
(
std_id varchar2(8),
std_name varchar2(30) NOT NULL,
course_id number,
stud_active number DEFAULT 1,
constraint std_id_pk primary key (std_id),
constraint course_id_stud_fk foreign key (course_id) references course(course_id),
constraint stud_active_ck check (stud_active in(1,0))
);

```

| stud_id | stud_name | course_id | stud_active |
|---------|-----------|-----------|-------------|
| 17MCA01 | ABILASH   | 1         | 1           |
| 17MCA02 | ABISHEK   | 2         | 1           |
| 17MCA03 | ANAND     | 3         | 1           |
| 17MCA04 | ARUN      | 4         | 1           |
| 17MCA05 | BALA      | 1         | 1           |
| 17MCA06 | BALAJI    | 2         | 1           |


### Feature 3: ADD FEE CATEGORY AND ASSIGN FEE TO EACH CATEGORY

To add fee categories and assign values to each category for each course

#### Query:

```

create table fee_category
( 
fee_category_id number, 
fee_category_name varchar2(100) not null,
constraint fee_category_id_pk primary key (fee_category_id),
constraint fee_category_name_uq unique (fee_category_name)
);

```

| fee_category_id | fee_category_name |
|-----------------|-------------------|
| 1               | admission         |
| 2               | verification      |
| 3               | tuition           |
| 4               | sports            |
| 5               | lab               |

```

create table course_fee 
(
course_fee_id number , 
course_id number, 
fee_category_id number, 
amount number not null,
constraint course_fee_id_pk primary key (course_fee_id),
constraint course_id_fee_fk foreign key(course_id) references course(course_id),
constraint fee_category_id_fk foreign key(fee_category_id) references fee_category(fee_category_id),
constraint amount_ck check (amount > 0)
);

```

 course_fee_id  | course_id | fee_category_id | amount |
|---------------|-----------|-----------------|--------|
| 1             | 1         | 1               | 500    |
| 2             | 4         | 3               | 600    |
| 3             | 4         | 2               | 200    |
| 4             | 2         | 4               | 300    |
| 5             | 3         | 2               | 10000  |


### Feature 4: MAKE ENTRY FOR PAYMENT THAT HAS BEEN MADE

To make entries for the fees paid by the students

#### Query:

```

create table payment
( 
payment_id number, 
payment_date date not null,
std_id number, 
sem_id number,
course_fee_id number, 
paid_amount  number,
constraint payment_id_pk primary key (payment_id),
constraint payment_date_ck check (payment_date>=SYSDATE),
constraint std_id_fk foreign key (std_id) references student(std_id),
constraint sem_id_payment_fk foreign key (sem_id) references semester(sem_id),
constraint course_fee_id_payment_fk foreign key (course_fee_id) references course_fee(course_fee_id),
constraint paid_amount_ck check(paid_amount>0)
);

```

### Feature 5: REPORT GENERATION

Various reports like Department wise, Department wise summary, Date wise, Date wise summary, Month wise summary, Semester wise summary, Academic year summary could be generated

#### Query : DATE WISE

```

select payment_id,payment_date,std_id,(select course_fee_name from course_fee c where c.course_fee_id=p.course_fee_id),paid_amount from payment p order by paid_date having sem_id=1;

```
