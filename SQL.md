[Source](https://youtu.be/HXV3zeQKqGY)

## Tables and Relations

Let us start with a table:

| student_id | name | major |
| --- | --- | --- |
| 1 | Jack | Biology |
| 2 | Kate | English |
| 3 | Jack | Biology |

Columns define single attributes like the name, major and so on. Rows represent a singly entry which is a collection of attributes. A ***primary key*** is a special attribute that identifies each row in the table. So for example, we might end up having two Jacks who studies English, so without the primary key we might get confused and we will not be able to differentiate between each entry.

Note that the primary key does not always have to be a number, it might be the email of that student or any other attribute that is unique to each student.

Note that primary keys have two shapes, surrogate and natural, a ***surrogate key*** does not have any mapping to anything in the real world unlike ***natural keys.*** An example on a surrogate key is employee id, while an example on a natural key is the social security number.

A ***foreign key*** is a key that connects us to another table, and it gets stored for each row in a table:

| emp_id | name | salary | branch_id |
| --- | --- | --- | --- |
| 101 | Jan Lavinson | 110000 | 1 |
| 102 | Michael Scott | 75000 | 2 |

Note that a foreign key stores the primary key of another entry in another table.

The branch table might look something like this:

| branch_id | branch_name | mgr_id |
| --- | --- | --- |
| 2 | Scranton | 101 |
| 3 | Stamford | 102 |

Also, note that an entry can have more than one foreign key, so for example, here super_id refers to the supervisor’s id:

| emp_id | name | salary | branch_id | super_id |
| --- | --- | --- | --- | --- |
| 101 | Jan Lavinson | 110000 | 1 | null |
| 102 | Michael Scott | 75000 | 2 | 101 |

We also have another type of keys called a ***composite key***, which is a key that needs more than one key (foreign keys) to uniquely identify a single row in a table:

| branch_id | supplier_name | supply_type |
| --- | --- | --- |
| 2 | Hammer Mill | Paper |
| 3 | Uni-ball | Custom Forms |

Note that the reason that we sometimes need a composite key is that either of the keys in the composite key does not uniquely identify each row in the new table.

---

## SQL

A language that is used for interacting with a relational database management systems (RDBMS). SQL allows us to get RDBMS to do things like doing CRUD operations, managing databases, designing and creating database tables as well as performing administrative tasks.

Not all RDBMS follow the SQL standard, but the concepts are all the same but the implementation may vary.

SQL is a hybrid language combined together, it is a data query language, a data definition language (schemas), a data control language (access, permissions) and a data manipulation language (CRUD). 

A ***query*** is a set of instructions given to the RDBMS in SQL that tells RDBMS what information you want, so as an example:

```sql
SELECT employee.name, employee.age 
FROM employee
WHERE employee.salary > 50000
```

---

## Creating Tables

Head to MySQL command line client and create a database by typing:

`create database <db-name>;`

You can keep typing commands in this terminal, but I will be using PopSQL as per the instructor 😛.

The first thing we will have to do after connecting our database is to define the table’s layout or schema. But before, note that we have various datatypes, including:

- INT : whole numbers
- DECIMAL(M,N) : decimal numbers
- VARCHAR(l) : string of text of length 1
- BLOB: Binary large object like an image
- DATE: “YYYY-MM-DD”
- TIMESTAMP: “YYYY-MM-DD HH:MM:SS” mainly used for recording

Note that any command we write in SQL should end up with a semicolon “;”.

To start, we will create a table that stores a student_id, name and major:

```sql
CREATE TABLE student (
	student_id INT PRIMARY KEY,
	name VARCHAR(20),
	major VARCHAR(20)
);
```

Note that you can just type the commands in small letter too. 

Now click on the query and click run at the top, and you will see a status message showing success.

we can also do thins instead to define our primary key:

```sql
student_id INT,
...
PRIMARY KEY(student_id)
```

The command `DESCRIBE student;` will show you a summary of the table including the keys, and types and so on.

If you want to drop the table we just created user the command DROP:

`DROP TABLE student;`

You can also modify a table after it is created like adding a column using the command ALTER:

`ALTER TABLE student ADD gpa DECIMAL(3,2);` and what this does is that it adds a gpa column to our table with type decimal, which should be a number, and two numbers of that number must come after the comma.

To drop a column, say here the gpa column, we can type the following:

`ALTER TABLE student DROP COLUMN gpa;`

---

## Inserting Data

Note that here the order matters, and it should be as you defined your schema:

```sql
INSERT INTO student VALUES(1, 'Jack', 'Biology');
```

Once you run this query it will show you in the status message that one row has been affected which is reasonable since you added an entry to the table.

If you type the query `SELECT * FROM student;` it will show you a table containing the only row we have added, and if you add another row it will look like this:

| student_id | name | major |
| --- | --- | --- |
| 1 | Jack | Biology |
| 2 | Kate | English |

Assuming that one of the student does not know their major for some reason, so what we could do is to specify or tell what attributes we want to add like this:

```sql
INSERT INTO student(student_id, name) VALUES(3, 'Claire');
```

And if you make a query to get the data stored in the table, you will get a NULL in place of the attribute that was not added.

Note that you CAN’T add the same entry again (if it has the same primary key).

---

## Constraints

Assuming that we have a table that looks like this:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/385cba77-e464-4d83-95eb-1edbd960453e/Untitled.png)

Note that we can use NOT NULL to explicitly state that we do not want a specify attribute to be NULL, as we also can specify that we want an attribute to be unique, which means that any entry with matching value of that attribute will get rejected.

```sql
CREATE TABLE student (
	student_id INT,
	name VARCHAR(20) NOT NULL,
	major VARCHAR(20) UNIQUE,
	PRIMARY KEY(student_id)
);
```

Violating any of these constraints will yield an error. A primary key is not null and unique by default.
If you want to give an attribute a default value if not entered, you can do so as follows:

```sql
...
major VARCHAR(20) DEFAULT 'undecided',
...
```

Note that so far we have been incrementing the student ids manually when adding them, but we can make SQL do that for us by using AUTO_INCREMENT: `student_id INT AUTO_INCREMENT` so now we can just ignore adding student id.

---

## Update and Delete

Starting with the table we had before, to update the major for example, where the major is ‘Bio” you can do the following:

```sql
UPDATE student
SET major = 'Bio'
WHERE major = 'Biology'
```

So we start by selecting or setting the attribute that we want to choose based on, then we set the attribute we want.

We can select other types of attributes, as we also can select other types of comparison operators like ≤ ≥, <, > and so on.

Criteria can get a bit more complex based on what you want:

```sql
UPDATE student
SET major = 'Biochemistry'
WHERE major = 'Bio' OR major = 'Chemistry';
```

You can also set more than one attribute as well:

```sql
UPDATE student
SET name = 'Tom', major = 'undecided'
WHERE student_id = 1;
```

If you omit the WHERE keyword, then the update will affect every single entry in the table.

The same applies to DELETE, if you just type something like the following, it will just delete all the rows from the selected table: 

`DELETE FROM student;`

But to delete a specific row:

```sql
DELETE FROM student
WHERE student_id = 5;
```

```sql
DELETE FROM student
WHERE name = 'Tom' AND major = 'undecided';
```

---

## Basic Queries

running some command like this will only return the selected attribute for each entry in the table:

```sql
SELECT name
FROM student;
```

This will only return the name of each student in the table.

To select more than one attribute, you can just separate between them with a comma:

```sql
SELECT name, major
FROM student;
```

Note that you can use the dot notation to specify the attribute as well as the table that you want to retrieve from as follows, and you can also order the returned data or table with ORDER BY:

```sql
SELECT student.name, student.major
FROM student
ORDER BY name;
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e606697b-720e-48a5-9dc2-e10a234065b1/Untitled.png)

Note that you can also specify if you want to get the results in descending or ascending order by adding **DESC** or **ASC** after the attribute you want to order by.

`ORDER BY student_id ASC;`

You can also order by more than one attribute and the result table will get ordered based on them starting with the first one and then based on the second attribute just like a pivot table:

`ORDER BY major, name DESC;`

You can also specify the number of rows you want to get in the result table by using the LIMIT command and adding the number of rows you want:

```sql
SELECT * 
FROM student
LIMIT 2;
```

And you can also order the result as well at the same time.

You can also select based on a specific criteria just like in deleting and updating using WHERE, and you can also choose which attributes or columns you want to get back:

```sql
SELECT *
FROM student
WHERE major = 'Chemistry';
```

or `SELECT name, major`

You can use  - -, <, >, ≤, ≥, =, <>, AND, OR

<> is not equal

You can specify if an attribute is among a set of values using IN:

```sql
SELECT * 
FROM student
WHERE name IN ('Claire', 'Kate', 'Mike');
```

You can also combine more than one selection criteria:

`WHERE major IN (’Biology’, ‘Chemistry’) AND student_id > 2;`

---

## Company Database Into

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d6992fcf-d308-43f1-8cd7-25b31c0b18a0/Untitled.png)

You can find the SQL code for this database [here](https://www.mikedane.com/databases/sql/creating-company-database/). But there are a couple of things that we should take into consideration:

- When the foreign key points to another table and this table does not yet exist, we should just specify the datatype of that table’s primary key, so for example, if you look on when we defined the employee table, the branch table did not exist yet so we sat the branch_id to INT:
    - `branch_id INT`
- This is how you define a foreign key in a table:
    - `FOREIGN KEY(mgr_id) REFERENCES employee(emp_id) ON DELETE SET NULL`
- After the other table is created, we can then alter or update it to have our foreign key pointing to that table.
- Since a composite key is a combination of more than one foreign key, this is how we define it since the foreign keys together form the primary key to our table:

```sql
PRIMARY KEY(emp_id, client_id),
FOREIGN KEY(emp_id) REFERENCES employee(emp_id) ON DELETE CASCADE,
FOREIGN KEY(client_id) REFERENCES client(client_id) ON DELETE CASCADE
```

- When we start adding data, if the data we are referencing via the foreign key is still not added yet, we should set it to Null and then we alter the table after we update the other table.

---

## More Basic Queries

You can select columns but named differently, so for example, you might want to select the first name as the forename and the last name as the surname, and you can do that using the AS keyword:

```sql
SELECT first_name AS forename, last_name AS surname
FROM employee;
```

The resulting table will have its columns named as we specified.

You can select all distinct values of an attribute, for example, we have male (M) or female (F) as the only values for the sec attribute, we can do that using the DISTINCT keyword:

```sql
SELECT DISTINCT sex
FROM employee;
```

This also applies to foreign keys too.

---

## Functions

For example, to get the number of employees in the employee tables, we can use the built in function COUNT:

```sql
SELECT COUNT(emp_id)
FROM employee;
```

This will give us 9.

If you for example put super_id instead of emp_id it will return 8 since one of them has the super_id as NULL which is David Wallace. So it actually counts the entries that has a value corresponding to that column or attribute.

Example: count the number of female employees who were born after 1970:

```sql
SELECT COUNT(emp_id) 
FROM employee
WHERE sec = 'F' AND birth_date > '1970-01-01';
```

Example: find the average of all employee’s salaries:

```sql
SELECT AVG(salary)
FROM employee
```

To find the avg for only male employees we can add a condition:

`WHERE sex  = ‘M’`

Example: find the sum of all employee’s salaries:

```sql
SELECT SUM(salary)
FROM employee;
```

Example: find out how many males and females there are:

```sql
SELECT COUNT(sex), sex
FROM employee
GROUP BY sex;
```

The result will look like this:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4eb3df65-d3f8-46d9-9dfd-3f3a35508661/Untitled.png)

Example: find the total sales of each salesman:

```sql
SELECT SUM(total_sales), emp_id 
FROM works_with
GROUP BY emp_id;
```

---

## Wildcards

A way for defining different patterns that we want to match a specific piece of data to, and we use LIKE.

Note that we have % = which matches any number of characters, and _ = for only one character.

Example: find any client who are an LLC

```sql
SELECT *
FROM client 
WHERE client_name LIKE '%LLC';
```

Note that this is similar to regular expressions, but more simplified.

If you add another % at the end, this means that this word should be included within that value.

Example: find any employee born in October:

```sql
SELECT *
FROM employee 
WHERE birth_date LIKE '____-10%';
```

The under score will match with any single character until we reach the month which should be 10 or October.

---
