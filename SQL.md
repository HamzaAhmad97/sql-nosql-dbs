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
