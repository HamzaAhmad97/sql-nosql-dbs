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
