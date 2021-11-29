# Create a database(internship) and a few tables in the database.

To create a database in Postgresql, we need to execute the CREATE DATABASE statement followed by our database name. In our case it will be:
- `CREATE DATABASE internship;`<br/>
  ![create database](https://github.com/LF-DevOps-Intern/6_1_k8s-roubusgauli-rikeshkarma/blob/master/Assignment%202/Qno4/snapshots/create%20database.png)
- We can use the _“\l”_ command to view the databases present.<br/>
  ![view the database](https://github.com/LF-DevOps-Intern/6_1_k8s-roubusgauli-rikeshkarma/blob/master/Assignment%202/Qno4/snapshots/view%20the%20database.png)
- And to use _“\c internship”_ to connect to the database internship.<br/>
  ![connect to internship database](https://github.com/LF-DevOps-Intern/6_1_k8s-roubusgauli-rikeshkarma/blob/master/Assignment%202/Qno4/snapshots/connect%20to%20internship%20database.png)
- Now to create we need to execute the CREATE TABLE statement i.e
  ```
  CREATE TABLE table_name (
    column_name TYPE [column_constraint],
    [table_constraint,]
  );
  ```
- This is the simplified basic syntax for the command.
- In our case let’s create two tables named “manufacturer” and ”supplies” using the following commands:
  - For manufacturer:
  ```
  CREATE TABLE manufacturer (
    id INT PRIMARY KEY,
    name VARCHAR,
    products VARCHAR,
    quality_level int
  );
  ```
  ![table manufacturer created](https://github.com/LF-DevOps-Intern/6_1_k8s-roubusgauli-rikeshkarma/blob/master/Assignment%202/Qno4/snapshots/table%20manufacturer%20created.png)
  - For supplies:
  ```
  CREATE TABLE supplies (
    id INT PRIMARY KEY,
    name VARCHAR,
    description VARCHAR,
    manufacturer VARCHAR,
    color VARCHAR
  );
  ```
  ![table supplies created](https://github.com/LF-DevOps-Intern/6_1_k8s-roubusgauli-rikeshkarma/blob/master/Assignment%202/Qno4/snapshots/table%20supplies%20created.png)
  
- We can use the _“\d”_ command to list the tables in our connected database.<br/>
  ![list the tables](https://github.com/LF-DevOps-Intern/6_1_k8s-roubusgauli-rikeshkarma/blob/master/Assignment%202/Qno4/snapshots/list%20the%20tables.png)
- Also we can use “\d table_name” to also view the columns and its values of the table.
    - \d manufacturers<br/>
    ![\d manufacturers](https://github.com/LF-DevOps-Intern/6_1_k8s-roubusgauli-rikeshkarma/blob/master/Assignment%202/Qno4/snapshots/%5Cd%20manufacturers.png)
    - \d supplies<br/>
    ![\d suppliers](https://github.com/LF-DevOps-Intern/6_1_k8s-roubusgauli-rikeshkarma/blob/master/Assignment%202/Qno4/snapshots/%5Cd%20supplies.png)

We have successfully created a database(internship) and a few tables in the internship database.