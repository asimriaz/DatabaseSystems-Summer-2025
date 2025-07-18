### Retrieving Data Using the SQL SELECT Statement 
```sql
SELECT *|{[DISTINCT] column|expression [alias],...} 
FROM    table;
```
- `SELECT` identifies the columns to be displayed.
- `FROM` identifies the table containing those columns.

#### Selecting All Columns

```sql
SELECT *
FROM   departments;
```
```sh
 department_id |   department_name    | manager_id | location_id
---------------+----------------------+------------+-------------
            10 | Administration       |        200 |        1700
            20 | Marketing            |        201 |        1800
            30 | Purchasing           |        114 |        1700
            40 | Human Resources      |        203 |        2400
            50 | Shipping             |        121 |        1500
            60 | IT                   |        103 |        1400
            70 | Public Relations     |        204 |        2700
            80 | Sales                |        145 |        2500
            90 | Executive            |        100 |        1700
           100 | Finance              |        108 |        1700
           110 | Accounting           |        205 |        1700
           120 | Treasury             |            |        1700
           130 | Corporate Tax        |            |        1700
           140 | Control And Credit   |            |        1700
           150 | Shareholder Services |            |        1700
           160 | Benefits             |            |        1700
           170 | Manufacturing        |            |        1700
           180 | Construction         |            |        1700
           190 | Contracting          |            |        1700
           200 | Operations           |            |        1700
           210 | IT Support           |            |        1700
           220 | NOC                  |            |        1700
           230 | IT Helpdesk          |            |        1700
           240 | Government Sales     |            |        1700
           250 | Retail Sales         |            |        1700
           260 | Recruiting           |            |        1700
           270 | Payroll              |            |        1700
(27 rows)
```

```sql
SELECT department_id, location_id 
FROM   departments;
```

```sh
 department_id | location_id
---------------+-------------
            10 |        1700
            20 |        1800
            30 |        1700
            40 |        2400
            50 |        1500
            60 |        1400
            70 |        2700
            80 |        2500
            90 |        1700
           100 |        1700
           110 |        1700
           120 |        1700
           130 |        1700
           140 |        1700
           150 |        1700
           160 |        1700
           170 |        1700
           180 |        1700
           190 |        1700
           200 |        1700
           210 |        1700
           220 |        1700
           230 |        1700
           240 |        1700
           250 |        1700
           260 |        1700
           270 |        1700
(27 rows)
```
 #### Concatenation Operator
 A concatenation operator:
 - Links columns or character strings to other columns
 - Is represented by two vertical bars (||)
 - Creates a resultant column that is a character expression

```sql
SELECT last_name||job_id AS "Employees" 
FROM employees;
```

```sh
      Employees
---------------------
 KingAD_PRES
 KochharAD_VP
 De HaanAD_VP
 HunoldIT_PROG
 ErnstIT_PROG
 AustinIT_PROG
 -- More  --
 (107 rows)
```

#### Literal Character Strings
- A literal is a character, a number, or a date that is included in the SELECT statement.
- Date and character literal values must be enclosed within single quotation marks.
- Each character string is output once for each row returned.

#### Using Literal Character Strings

```sql
SELECT last_name ||' is a '||job_id AS "Employee Details"
FROM   employees;
```
```
     Employee Details
---------------------------
 King is a AD_PRES
 Kochhar is a AD_VP
 De Haan is a AD_VP
 Hunold is a IT_PROG
 Ernst is a IT_PROG
 Austin is a IT_PROG
 Pataballa is a IT_PROG
 Lorentz is a IT_PROG
 -- More  --
 Whalen is a AD_ASST
 Hartstein is a MK_MAN
 Fay is a MK_REP
 Mavris is a HR_REP
 Baer is a PR_REP
 Higgins is a AC_MGR
 Gietz is a AC_ACCOUNT
(107 rows)
```

#### Duplicate Rows
The default display of queries is all rows, including duplicate rows.

```sql
SELECT department_id 
FROM   employees;
```
```sh
 department_id
---------------
            90
            90
            90
            60
            60
            60
-- More  --
```

```sql
SELECT DISTINCT department_id 
FROM   employees;
```
```sh
 department_id
---------------
            70
            80
            20
            10

            90
           100
           110
            30
            50
            40
            60
(12 rows)
```


#### Displaying the Table Structure
- Use the `\d` command to display the structure of a table.
- Or, select the table in the Connections tree and use the Columns tab to view the table structure.

```sql
\d employees
```

```sh
                                                 Table "hr.employees"
     Column     |            Type             | Collation | Nullable |                    Default
----------------+-----------------------------+-----------+----------+------------------------------------------------
 employee_id    | integer                     |           | not null | nextval('employees_employee_id_seq'::regclass)
 first_name     | character varying(20)       |           |          |
 last_name      | character varying(25)       |           | not null |
 email          | character varying(25)       |           | not null |
 phone_number   | character varying(20)       |           |          |
 hire_date      | timestamp without time zone |           | not null |
 job_id         | character varying(10)       |           | not null |
 salary         | numeric(8,2)                |           |          |
 commission_pct | numeric(2,2)                |           |          |
 manager_id     | integer                     |           |          |
 department_id  | integer                     |           |          |
Indexes:
    "employees_pkey" PRIMARY KEY, btree (employee_id)
    "emp_department_ix" btree (department_id)
    "emp_email_uk" UNIQUE CONSTRAINT, btree (email)
    "emp_job_ix" btree (job_id)
    "emp_manager_ix" btree (manager_id)
    "emp_name_ix" btree (last_name, first_name)
Check constraints:
    "emp_salary_min" CHECK (salary > 0::numeric)
Foreign-key constraints:
    "employees_department_id_fkey" FOREIGN KEY (department_id) REFERENCES departments(department_id)
    "employees_job_id_fkey" FOREIGN KEY (job_id) REFERENCES jobs(job_id)
    "employees_manager_id_fkey" FOREIGN KEY (manager_id) REFERENCES employees(employee_id)
Referenced by:
    TABLE "departments" CONSTRAINT "dept_mgr_fk" FOREIGN KEY (manager_id) REFERENCES employees(employee_id)
    TABLE "employees" CONSTRAINT "employees_manager_id_fkey" FOREIGN KEY (manager_id) REFERENCES employees(employee_id)
    TABLE "job_history" CONSTRAINT "job_history_employee_id_fkey" FOREIGN KEY (employee_id) REFERENCES employees(employee_id)
```

### 2. Restricting and Sorting Data
#### Objectives
After completing this lesson, you should be able to do the following:
- Limit the rows that are retrieved by a query
- Sort the rows that are retrieved by a query
- Use ampersand substitution to restrict and sort output at run time

#### Limiting Rows Using a Selection

```sh
 employee_id |  last_name  |   job_id   | department_id
-------------+-------------+------------+---------------
         100 | King        | AD_PRES    |            90
         101 | Kochhar     | AD_VP      |            90
         102 | De Haan     | AD_VP      |            90
         103 | Hunold      | IT_PROG    |            60
         104 | Ernst       | IT_PROG    |            60
         105 | Austin      | IT_PROG    |            60
-- More  --
```

“retrieve all employees in department 90”
















| **Operator**          | **Meaning**                                 |
| --------------------- | ------------------------------------------- |
| `=`                   | Equal to                                    |
| `>`                   | Greater than                                |
| `>=`                  | Greater than or equal to                    |
| `<`                   | Less than                                   |
| `<=`                  | Less than or equal to                       |
| `<>`                  | Not equal to                                |
| `BETWEEN ... AND ...` | Between two values (inclusive)              |
| `IN (set)`            | Match any value from a list                 |
| `LIKE`                | Match a character pattern (wildcards `% _`) |
| `IS NULL`             | Checks if a value is `NULL`                 |


![alt text](/images/image.png)


