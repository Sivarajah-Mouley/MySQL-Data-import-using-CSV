# MySQL-Data-import-using-CSV
## 1. Create a Database

If a database doesn't exist, create one:

```sql
CREATE DATABASE IF NOT EXISTS your_database_name;
USE your_database_name;
```
- Change ```your_database_name``` as your one.
- Select the database by double clicking inside the Schemas.
## 2. Create Table: `movie_data`
- Here, i am using a data set with movies as an example.
 Create a table to store movie data:

```sql
CREATE TABLE movie_data (
    Title VARCHAR(255),
    Release_date DATE,
    URL VARCHAR(255),
    Genre VARCHAR(100),
    Director_1 VARCHAR(100),
    Director_2 VARCHAR(100),
    Cast_1 VARCHAR(100),
    Cast_2 VARCHAR(100),
    Cast_3 VARCHAR(100),
    Cast_4 VARCHAR(100),
    Cast_5 VARCHAR(100),
    Budget DECIMAL(15, 2),
    Revenue DECIMAL(15, 2)
);
```
- Make sure the table headers are short and do not contain any special characters.
- Change the attributes and data types as in your project.

## 3. Enable `local_infile`

To import data, ensure `local_infile` is enabled:

- Modify MySQL configuration file (`my.ini` or `my.cnf`) by adding `local_infile=1` under `[mysqld]` section.
- Restart MySQL Server to apply changes.

### On MySQL Workbench (Client Side):

Before using `LOAD DATA LOCAL INFILE` command, execute this query to enable it for the current session:

```sql
SET GLOBAL local_infile = 'ON';
```
- This command allows the client (Workbench) to use `LOAD DATA LOCAL INFILE`.
  
## 4. Import Data
- Before import `.CSV` data make sure your file headers are same as the table headers.
- Make sure the data types like currency  do not have any "," or "$" sybmbols.
- Dates should be in  YYYY-MM-DD format.
  
Choose the suitable import option:

### Option 1: Copy to Secure File Priv Path (Not Recommended for Production)

- To find path allowed to import files:

```sql
SHOW VARIABLES LIKE "secure_file_priv"; 
```
- This is to find where the files should be stored to get imported here.
- Copy the file to the `secure_file_priv` path.
- Locate the file path as the path shown in secure_file_priv and change it code.
- Import file with below code :

```sql
LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0.34/Uploads/Movie_data1.csv'
INTO TABLE movie_data
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 LINES;  
```

### Option 2: Less Secure Method

Import directly from the file's path:

```sql
LOAD DATA LOCAL INFILE 'C:/Users/hi/Downloads/Movie_data1.csv'
INTO TABLE movie_data
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 LINES;
```
- Change the path as your actual path of target file.
- Change the table name in below code as yours.
  
### Option 3: Import Using MySQL Workbench Wizard

1. Open MySQL Workbench and connect to your MySQL server.
2. Click on Server > Data Import: Use the built-in wizard for importing data.
3. Choose Import Source: Select the CSV file as your source.
4. Select Target Table: Choose the table you created earlier (`movie_data` in this example).
5. Specify CSV Options: Define the options such as delimiter, character set, and other settings matching your CSV file.
6. Run the Import: Follow the wizard steps to execute the import.

Notes:
- Always back up your database before making significant changes.

