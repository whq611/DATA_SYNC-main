# Data-migration
Data is migrated from MySQL and MongoDB to Postgresql

# #few:
This program mainly carries out the parallel migration of data from MySQL to PostgreSQL and from MongoDB to PostgreSQL. The last update time (last modification time) of data is used as the time node, and the data in a certain range can be selected for migration.

Principle # # :
# # # MySQL -- -- -- -- -- -- - > PostgreSQL
1. Basic fields: Perform one-to-one mapping and synchronize related data. Note that the source table ID cannot be directly synchronized to the PostgresQL ID field.
2. Foreign key field: ODOO postgresQL foreign key field stores primary key (self-increment ID) of primary table, direct synchronization association cannot be associated, this program uses two fields (a field, B field) to add association.
Select origin_ID from origin_ID where origin_ID = a from origin_id where origin_ID = a from origin_id where origin_ID = a Origin_id indicates that the source table is associated with the primary table field corresponding to the primary key field of the target table. Because the original table is unique, the queried ID is stored in field B (the new foreign key field of the target table) to ensure that the association relationship is established.

# # # mongo -- -- -- -- -- -- - > PostgreSQL
One-to-one field mapping synchronization is adopted. Since documents are the basic unit of data in MongoDB, similar to (but more complex than) rows in a relational database, documents are formed by placing multiple keys and their associated values together in an orderly manner.
A document contains a set of fields, each of which is a key/value. The key must be a string. Value can contain the following types:
* Basic types, e.g. String, int, float, timestamp, binary, etc.
* A document.
* Array type.
Key is the name of a field in PostgresQL, and value is the type of a field. Postgresql does not support document and array types. Therefore, postgresQL uses text instead. In addition, if the default _id is used in MongoDB, it is an ObjectId field. Postgresql does not support this field. The char type is used for conversion.
< font color = red > this procedure has not been added mongo main foreign key associated synchronization, the same kind of different patterns of documents on the same collection. </font>

##
1. The database information to fill in, fill out the need to synchronize of information related to the source database and target database related information, such as IP address, port number, login user name, password, etc., specific information is as follows:
* Database name: Enter the database name, such as test;
* Database host (IP) : Enter the database connection IP, such as localhost;
* Port number: Enter the database connection port number, for example, 3306.
* Database login user name: Enter the database connection login user name, such as root;
* Database login password: Enter the database connection password, such as 123456;
* Effective status: effective or not;
* Database description: Add a description for the database;
* Database type: select the database type from the drop-down list box, such as MySQL.

2. The synchronous table information to fill in, fill in the source table needs to be synchronized (set) as well as information on the target table, such as table name, specific information is as follows:
* Database: drop down box, select the database filled in the previous step;
* Sync table name: Enter the name of the table to be synchronized, such as student;
* Effective status: effective or not;
* Mapping status: If the table is a target table and a configuration table, the mapping status can be set to True to facilitate quick query during primary and foreign key association.
* table description: add a description of the table, such as target students table;
* Database description: automatically bring out.
< font color = red > note: the table and database is a one-to-one relationship, namely, select the source database fill in information of the source table, fill in information of the target table target database. </font>

3. Fill in the mapping relation of the synchronization table, select the synchronization table and target table filled in the previous step, and then select the synchronization sequence, start and end synchronization time, effective status, and group information as follows:
* Source synchronization table: Select the table filled in the previous step (source table);
* Source synchronization table description: automatically bring out;
* Target synchronization table: Select the table filled in the previous step (target table);
* Target synchronization table description: automatically bring out;
* Synchronization sequence: Fill in the synchronization sequence. Refer to basic table (1-99), master table (100-999), and slave table (1000-9999).
* Start synchronization time: Select the start node in the data synchronization time range.
* End synchronization time: Select the end node in the data synchronization time range.
* Effective status: effective or not;
* Grouping: Synchronization data is grouped into four groups, corresponding to four scheduled tasks, which can be grouped according to different categories.

4. Fill in the field mapping relationship to be synchronized, and fill in the name and type of the related field, whether it is unique, whether it takes effect, and whether it is a time node field (last updated (modified) time, etc., as follows:
* Synchronization table mapping: Select the synchronization table mapping in the previous step from the drop-down list.
* the source synchronous table name: automatic out;
* Source synchronization table field name: Enter the source table field to be synchronized (single field), for example, name.
* Source synchronization table field type: Enter the field type of the source synchronization table field, such as VARCHAR.
* Source field Description: Fill in the field description of the source synchronization table, such as the name of the student in the source table;
* Target sync table name: automatically bring out;
* Target synchronization table field name: Fill in the target table field corresponding to the source table field (single field), such as name;
* Target synchronization table field type: Enter the field type of the target synchronization table, such as VARCHAR.
* Target field description: Add the description of the target field, such as the name of the student in the target table;
* a unique identifier: a unique identifier fields, namely whether the field source table primary key;
* Effective status: effective or not;
* Time node: time node field, such as whether the last updated (modified) time field.

Mysql -→ postgresQL; mysql-→ postgresQL; mysql-→ postgresQL;
* Synchronization table mapping: drop down box, select synchronization table mapping data;
* Target table associated with primary table: drop down box, select the primary table associated with the foreign key field of the target table, such as teacher;
* the main table primary key: fill in the target table foreign key reference (associated) field, such as id;
* Primary key of the source table: Enter the corresponding field of the primary key field of the source table in the target primary table, for example, origin_id.
* Target table: drop down box to select synchronized target table, such as student;
* Foreign key: Fill in the foreign key field of the target table, such as Teacher_id;
* Source table foreign key: Enter the foreign key field of the source table in the target table, for example, origin_TEACHer_id.
* Effective status: Whether it takes effect.

6. Primary foreign key association task table, the program automatically add data, we can view the relevant information, do not allow to create, edit.
This section mainly displays information about the primary foreign key compensation task. That is, if the data in the secondary table has been synchronized but the data in the primary table has not been synchronized and the primary foreign key cannot be associated, the related information is saved in this section. After the primary table data is synchronized, a scheduled task is used to scan the table and associate the related primary foreign keys.
