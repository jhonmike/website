# INSERT

```sql
INSERT INTO table_name VALUES (value1,value2,value3);

OR

INSERT INTO table_name (column1,column2,column3) VALUES (value1,value2,value3);
```

# UPDATE

```sql
UPDATE table_name SET column1=value1,column2=value2 WHERE some_column=some_value;
```

# DELETE

```sql
DELETE FROM table_name WHERE some_column=some_value;
```

# SELECT

```sql
SELECT * FROM table_name WHERE some_column=some_value ORDER BY column_name ASC, column_name DESC;
```

# JOIN

```sql
SELECT * FROM Orders INNER JOIN Customers ON Orders.customerId=Customers.id; 
```

# CREATE TABLE

```sql
CREATE TABLE table_name (
    id INT AUTO_INCREMENT NOT NULL,
    sequence INT(10) NOT NULL,
    account_id INT DEFAULT NULL,
    revenue_id INT DEFAULT NULL,
    created DATETIME NOT NULL,
    updated DATETIME DEFAULT NULL,
    INDEX IDX_921775439B6B5FBA (account_id),
    INDEX IDX_92177543224718EB (revenue_id),
    PRIMARY KEY(id)
);
```

# ALTER TABLE

```sql
ALTER TABLE ApplicationClient ADD email_accountant varchar(100);
ALTER TABLE CoreZipCode MODIFY COLUMN zipCode varchar(8);
ALTER TABLE ApplicationClient CHANGE columnOldName columnNewName varchar(10);
```

# Show info table

```sql
DESCRIBE ApplicationClient;
```
