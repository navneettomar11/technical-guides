# Database
Database is nothing but an organized form of data for easy access, storing, retrieval and managing of data. This is also known as structured form of data which can be accessed in many ways.

# Tables and Fields
A table is a set of data that are organied in a model with columns and rows. Columns can be categorized as vertical, and rows are horizontal. A table has specified number of column called fields but can have any number of rows which is called record.

# Primary Key
A primary key is combination of fields which uniquely specify a row. This is a special kind of unique key, and it has implicit NOT NULL constraint. It means, Primary key values cannot be NULL.
A primary is a single column value used to identify a database record uniquely.
It has following attributes
- A primary key values cannot be NULL
- A primary key value must be unique
- The primary key values should rarely be changed
- The primary key must be given a value when a new record is inserted.

# Composite Key
A composite key is a primary  composed of multiple columns used to identify a record uniquely.

# Unique Key
A Unique key constraint uniquely identified each record in the database. This provides uniqueness for the column or set of columns.
A primary key constraint has automatic unique constraint defined on it. But not, in the case of Unique key.
There can be many unique constraint defined per table, but only one Primary key constraint defined per table.

# Foreign key
A foreign key is one table which can be related to the primary key of another table. Relationship needs to be created between two table bt referencing foreign key with the primary key of another table.

# Join
This is a keyword used to query data from more tables based on the relationship between the fields of the tables. Keys play a major role when JOINs are used.
## Type of join
There are various types of join which can be used to retrieve data and it depends on the relationship between tables.
- **Inner Join**: Inner join return rows when there is at least one match of rows between the tables.
- **Right Join**: Right join return row which are common between the tables and all rows of right hand side table. Simply, it returns all the rows from the right hand side table even though there are no matches in the left hand side table.
- **Left Join**: Left join return rows which are common between the tables and all rows of Left hand side table. Simply, it return all the rows from Left hand side table even though there are no matches in the right hand side table.
- **Full Join**: Full join return rows where there are matching rows in any one of the tables. This means, it returns all the rows from the left hand side table and all the rows from the right hand side table.

# Normalization
Normalization is the process of minimizing redundancy and dependency by organizing fields and table of a database. The main aim of Normalization is to add, delete, or modify field that can be made in a single table.
Normalization is a databaser design technique which organize tables in a manner that reduces redundancy and dependency of data.
It divides larger table to smaller tables and links them using relationships.

The normal forms can be divided into 5 forms
- **First Normal Form (1NF)**: This should remove all the duplicate columns from the table. Creation of tables for the related data and identification of unique columns.
- **Second Normal Form (2NF)**: Meeting all requirments of the first normal form. Placing the subsets of data in separate tables and Creation of relationship between the tables using primary keys.
- **Third Normal Form (3NF)**: This should meet all requirements of 2NF. Removing the columns which are dependent on primary key constraints.
- **Boyce-Codd Normal Form (BCNF)**: Even when a database is in 3rd Normal Form, still there would be anomalies resulted if it has more than one Candidate Key.
- **Fourth Normal Form(4NF)**: Meeting all the requirments of third normal form and it should not have multi-valued dependencies.
- **Fifth Normal Form(5NF)**: A table is in 5th Normal Formal if it is in 4NF and it cannot be decomposed into any number of smallest tables without loss of data.

## Database Normalization Examples -
Assume a video library maintains a database of movies rented out. Without any normalization, all information is stored in one table as shown below:

|Full Names|Physical Address|Movies Rented|Salutation|Category|
|----------|----------------|-------------|----------|--------|
|Janet jones|First stree plot no: 4| Pirates of the Caribbean, Clash of titans|Ms.|Action, Action|
|Robert Phil|3rd street 34| Forgetting Sarah Marshal, Daddy's Little Girls|Mr.|Romance, Romance|
|Robert Phill|5th Avenue| Clash of titans|Mr.|Action|

Here you see Movies related column has multiple values.

Now let's movie into 1st Normal forms
**1NF (First Normal Form) Rules
- Each table cell should contain a single value.
- Each record need to be unique.
The above table in 1NF -

|Full Names|Physical Address|Movies Rented|Salutation|
|----------|----------------|-------------|----------|--------|
|Janet jones|First stree plot no: 4| Pirates of the Caribbean|Ms.|
|Janet jones|First stree plot no: 4| Clash of titans|Ms.|
|Robert Phil|3rd street 34| Forgetting Sarah Marshal|Mr.|
|Robert Phil|3rd street 34| Daddy's Little Girls|Mr.|
|Robert Phill|5th Avenue| Clash of titans|Mr.|Action|

Let's move into second normal form 2NF

**2NF (Second Normal Form) Rules**
- Rule1 - Be in 1NF
- Rule2 - Single Column Primary key

It is clear that we can't move forward to make our simplest database in 2nd Normalization form

|Membership ID|Full Names | Physical Address | Salutation|
|-------------|-----------|------------------|-----------|
|1|Janet Jones| First Street Plot no: 4| Ms.|
|2|Robert Phil| 3rd Street 34| Mr.|
|3|Robert Phil| 5th Avenue| Mr.|

|Membership ID| Movies Rented|
|-------------|--------------|
|1|Pirates of the Caribbean|
|1|Clash of the titans|
|2| Forgetting Sarah Marshal|
|2| Daddy's little girls|
|3|Clash of titans|

We have divided our 1NF table into two tables. 
We have introduced a new column called Membership_id which is the primary key for table. Record can be uniquely identified using membership id.

### Transitive functional dependencies
A transitive functional dependency is when changing a non-key column, might cause any of the other non-key columns to change.
Changing the non-key column Full Name may change Salutation.

Let's move into 3NF
**3NF (Thirst Normal Form) Rules**
- Rule 1 - Be in 2NF
- Rule 2 - Has no transitive functional dependencies.
To move our 2NF table into 3NF, we again need to again divide our table.

|Membership ID|Full Names | Physical Address | Salutation ID|
|-------------|-----------|------------------|-----------|
|1|Janet Jones| First Street Plot no: 4| 2|
|2|Robert Phil| 3rd Street 34| 1|
|3|Robert Phil| 5th Avenue| 1|

|Membership ID| Movies Rented|
|-------------|--------------|
|1|Pirates of the Caribbean|
|1|Clash of the titans|
|2| Forgetting Sarah Marshal|
|2| Daddy's little girls|
|3|Clash of titans|

|Salutation ID| Salutation|
|-------------|-----------|
|1|Mr.|
|2|Ms.|
|3|Mrs.|
|4|Dr.|

We have again divided our tables and created a new table which store Salutations.

There are no transitive functional dependencies, and hence our table is in 3NF.

# View
A view is a virtual table which consists of a subset of data contained in a table. View are not virtually present and it takes less space to store. View can have data of one or more tables combined, and its depending on the relationship.

# Index
An index is performance tuning method of allowing faster retrieval of records from the table. An index creates an entry for each value and it will be faster to retrieve data.
Indexing is a way to optimize the performance of a database by minimizing the number of disk accesses required when a query is processed. It is a data structure technique which is used to quickly locate and access the data in a database.
Index are created using a few database columns.
- The first column is the **Search key** that contains a copy of the primary key or candidate key of the table. These values are stored in sorted order so that the corresponding data can be accessed quickly.
- The second column is the **Data Reference** or **Pointer** which contains a set of pointer holding the address of the disk block where the particular key value can be found.

In general, there are two types of the file organization mechanism which are followed by the indexing methods to store the data:
1. **Sequentical File Organization or Order Index File**: In this, the indicies are based on a sorted ordering of the values. These are generally fast and a more traditional type of storing mechanism. These Ordered or Sequential file organization might store the data in a dense or sparse format.
2. **Hash File organization**: Indices are based on the values being distributed uniformly across a range of buckets. The buckets to which a value is assigned is determined by a function called a hash function.

There are primarly three methods of indexing:
- **Unique Index**: This indexing does not allow the field to have duplicate values if the column is unique indexed. Unique index can be applied automatically when primary key is defined.
- **Clustered Index**: This type of index reorders the physical order of the table and search based on the key values. Each table can have only one clustered index.
When more than two records are stored in the same file these types of storing known as cluster indexing. By using the cluster indexing we can reduce the cost of searching reason being multiple records related to the same thing are stored at one place and it also give the frequent joing of more than two tables.
- **NonClustered Index**: NonClustered Index does not alter the physical order of the table and maintains logical order of data.Each table can have 999 nonclustered indexes.