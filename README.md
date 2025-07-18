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
<pre>
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
</pre>

```sql
SELECT department_id, location_id 
FROM   departments;
```

<pre>
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
</pre>

 #### Concatenation Operator
 A concatenation operator:
 - Links columns or character strings to other columns
 - Is represented by two vertical bars (||)
 - Creates a resultant column that is a character expression

```sql
SELECT last_name||job_id AS "Employees" 
FROM employees;
```

<pre>
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
</pre>

#### Literal Character Strings
- A literal is a character, a number, or a date that is included in the SELECT statement.
- Date and character literal values must be enclosed within single quotation marks.
- Each character string is output once for each row returned.

#### Using Literal Character Strings

```sql
SELECT last_name ||' is a '||job_id AS "Employee Details"
FROM   employees;
```
<pre>
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
</pre>

#### Duplicate Rows
The default display of queries is all rows, including duplicate rows.

```sql
SELECT department_id 
FROM   employees;
```
<pre>
 department_id
---------------
            90
            90
            90
            60
            60
            60
-- More  --
</pre>

```sql
SELECT DISTINCT department_id 
FROM   employees;
```
<pre>
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
</pre>


#### Displaying the Table Structure
- Use the `\d` command to display the structure of a table.
- Or, select the table in the Connections tree and use the Columns tab to view the table structure.

```sql
\d employees
```

<pre>
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
</pre>

### 2. Restricting and Sorting Data
#### Objectives
After completing this lesson, you should be able to do the following:
- Limit the rows that are retrieved by a query
- Sort the rows that are retrieved by a query
- Use ampersand substitution to restrict and sort output at run time

#### Limiting Rows Using a Selection

> retrieve all employees
<pre>
 employee_id |  last_name  |   job_id   | department_id
-------------+-------------+------------+---------------
         100 | King        | AD_PRES    |            90
         101 | Kochhar     | AD_VP      |            90
         102 | De Haan     | AD_VP      |            90
         103 | Hunold      | IT_PROG    |            60
         104 | Ernst       | IT_PROG    |            60
         105 | Austin      | IT_PROG    |            60
-- More  --
</pre>

> retrieve all employees in department 90

<pre>
 employee_id | last_name | job_id  | department_id
-------------+-----------+---------+---------------
         100 | King      | AD_PRES |            90
         101 | Kochhar   | AD_VP   |            90
         102 | De Haan   | AD_VP   |            90
(3 rows)
</pre>

#### Limiting the Rows That Are Selected
- Restrict the rows that are returned by using the `WHERE` clause:

```sh
SELECT *|{[DISTINCT] column|expression [alias],...} 
FROM   table
[WHERE condition(s)];
```
- The `WHERE` clause follows the `FROM` clause.

#### Using the `WHERE` Clause

```sh
SELECT employee_id, last_name, job_id, department_id 
FROM   employees
WHERE  department_id = 90 ;
```

#### Character Strings and Dates
- Character strings and date values are enclosed with single quotation marks.
- Character values are case-sensitive and date values are format-sensitive.
- The default date display format is `DD-MON-RR`.

```sh
SELECT last_name, job_id, department_id 
FROM   employees
WHERE  last_name = 'Whalen' ;
```

```sh
SELECT last_name 
FROM   employees
WHERE  hire_date = '17-FEB-96' ;
```

#### Comparison Operators
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

#### Using Comparison Operators
```sh
SELECT last_name, salary 
FROM   employees
WHERE  salary <= 3000 ;
```

<pre>
  last_name  | salary
-------------+---------
 Baida       | 2900.00
 Tobias      | 2800.00
 Himuro      | 2600.00
 Colmenares  | 2500.00
 Mikkilineni | 2700.00
 Landry      | 2400.00
 Markle      | 2200.00
 Atkinson    | 2800.00
 Marlow      | 2500.00
 Olson       | 2100.00
 Rogers      | 2900.00
 Gee         | 2400.00
 Philtanker  | 2200.00
 Seo         | 2700.00
 Patel       | 2500.00
 Matos       | 2600.00
 Vargas      | 2500.00
 Sullivan    | 2500.00
 Geoni       | 2800.00
 Cabrio      | 3000.00
 Gates       | 2900.00
 Perkins     | 2500.00
 Jones       | 2800.00
 Feeney      | 3000.00
 OConnell    | 2600.00
 Grant       | 2600.00
(26 rows)
</pre>

#### Range Conditions Using the `BETWEEN` Operator

```sh
SELECT last_name, salary
FROM   employees
WHERE  salary BETWEEN 2500 AND 3500 ;
```

<pre>
  last_name  | salary
-------------+---------
 Khoo        | 3100.00
 Baida       | 2900.00
 Tobias      | 2800.00
 Himuro      | 2600.00
 Colmenares  | 2500.00
 Nayer       | 3200.00
 Mikkilineni | 2700.00
 Bissot      | 3300.00
 Atkinson    | 2800.00
 Marlow      | 2500.00
 Mallin      | 3300.00
 Rogers      | 2900.00
 Stiles      | 3200.00
 Seo         | 2700.00
 Patel       | 2500.00
 Rajs        | 3500.00
 Davies      | 3100.00
 Matos       | 2600.00
 Vargas      | 2500.00
 Taylor      | 3200.00
 Fleaur      | 3100.00
 Sullivan    | 2500.00
 Geoni       | 2800.00
 Dellinger   | 3400.00
 Cabrio      | 3000.00
 Gates       | 2900.00
 Perkins     | 2500.00
 McCain      | 3200.00
 Jones       | 2800.00
 Walsh       | 3100.00
 Feeney      | 3000.00
 OConnell    | 2600.00
 Grant       | 2600.00
(33 rows)
</pre>

#### Membership Condition Using the `IN` Operator
Use the `IN` operator to test for values in a list:

```sh
SELECT employee_id, last_name, salary, manager_id 
FROM   employees
WHERE  manager_id IN (100, 101, 201) ;
```

<pre>
 employee_id | last_name |  salary  | manager_id
-------------+-----------+----------+------------
         101 | Kochhar   | 17000.00 |        100
         102 | De Haan   | 17000.00 |        100
         108 | Greenberg | 12000.00 |        101
         114 | Raphaely  | 11000.00 |        100
         120 | Weiss     |  8000.00 |        100
         121 | Fripp     |  8200.00 |        100
         122 | Kaufling  |  7900.00 |        100
         123 | Vollman   |  6500.00 |        100
         124 | Mourgos   |  5800.00 |        100
         145 | Russell   | 14000.00 |        100
         146 | Partners  | 13500.00 |        100
         147 | Errazuriz | 12000.00 |        100
         148 | Cambrault | 11000.00 |        100
         149 | Zlotkey   | 10500.00 |        100
         200 | Whalen    |  4400.00 |        101
         201 | Hartstein | 13000.00 |        100
         202 | Fay       |  6000.00 |        201
         203 | Mavris    |  6500.00 |        101
         204 | Baer      | 10000.00 |        101
         205 | Higgins   | 12000.00 |        101
(20 rows)
</pre>

#### Pattern Matching Using the `LIKE` Operator
- Use the `LIKE` operator to perform wildcard searches of valid search string values.
- Search conditions can contain either literal characters or numbers:
    - `%` denotes zero or many characters. 
    - `_` denotes one character.

```sh
SELECT first_name
FROM employees
WHERE first_name LIKE 'S%' ;
```

<pre>
 first_name
------------
 Steven
 Shelli
 Sigal
 Shanta
 Steven
 Stephen
 Sarath
 Sundar
 Sundita
 Sarah
 Samuel
 Susan
 Shelley
(13 rows)
</pre>

#### Combining Wildcard Characters
• You can combine the two wildcard characters (`%, _`) with literal characters for pattern matching:

```sh
SELECT last_name
FROM   employees
WHERE  last_name LIKE '_o%' ;
```

<pre>
 last_name
------------
 Kochhar
 Lorentz
 Popp
 Tobias
 Colmenares
 Vollman
 Mourgos
 Rogers
 Doran
 Fox
 Johnson
 Jones
(12 rows)
</pre>

- You can use the ESCAPE identifier to search for the actual `%` and `_` symbols.