
---

# SQL Lab Summary — Creating, Modifying, and Dropping Databases and Tables

## 1. Listing Existing Databases

```sql
SHOW DATABASES;
```
<img width="312" height="176" alt="image" src="https://github.com/user-attachments/assets/0cd8ff64-93a0-42a6-a9e6-17a82948be11" />

This command displays all databases available in the MariaDB instance.

---

## 2. Creating the `world` Database

```sql
CREATE DATABASE world;
```
 <img width="382" height="48" alt="image" src="https://github.com/user-attachments/assets/d0a0cd29-2005-49b2-90e0-318a6f3352a7" />

Verify creation:

```sql
SHOW DATABASES;
```
<img width="325" height="191" alt="image" src="https://github.com/user-attachments/assets/077760b5-cf83-4f51-b901-b9fcd03e6d0c" />

---

## 3. Creating the `country` Table

```sql
CREATE TABLE world.country (
  `Code` CHAR(3) NOT NULL DEFAULT '',
  `Name` CHAR(52) NOT NULL DEFAULT '',
  `Conitinent` enum('Asia','Europe','North America','Africa','Oceania','Antarctica','South  America') NOT NULL DEFAULT 'Asia',
  `Region` CHAR(26) NOT NULL DEFAULT '',
  `SurfaceArea` FLOAT(10,2) NOT NULL DEFAULT '0.00',
  `IndepYear` SMALLINT(6) DEFAULT NULL,
  `Population` INT(11) NOT NULL DEFAULT '0',
  `LifeExpectancy` FLOAT(3,1) DEFAULT NULL,
  `GNP` FLOAT(10,2) DEFAULT NULL,
  `GNPOld` FLOAT(10,2) DEFAULT NULL,
  `LocalName` CHAR(45) NOT NULL DEFAULT '',
  `GovernmentForm` CHAR(45) NOT NULL DEFAULT '',
  `HeadOfState` CHAR(60) DEFAULT NULL,
  `Capital` INT(11) DEFAULT NULL,
  `Code2` CHAR(2) NOT NULL DEFAULT '',
  PRIMARY KEY (`Code`)
);
```
<img width="932" height="360" alt="image" src="https://github.com/user-attachments/assets/60950e8b-e057-48a4-b0da-42b3b849daa4" />

Switch to the database and verify:

```sql
USE world;
SHOW TABLES;
```

List table columns:

```sql
SHOW COLUMNS FROM world.country;
```
<img width="941" height="691" alt="image" src="https://github.com/user-attachments/assets/daf433a2-2318-4ee9-be49-aafdfa44fc71" />

---

## 4. Correcting a Misspelled Column

The table contains a typo: `Conitinent`.

Rename it:

```sql
ALTER TABLE world.country 
RENAME COLUMN Conitinent TO Continent;
```

Verify:

```sql
SHOW COLUMNS FROM world.country;
```
<img width="818" height="61" alt="image" src="https://github.com/user-attachments/assets/fd1f20a3-7278-40c7-85a9-7196a3aff30c" />

---

## 5. Challenge 1 — Creating the `city` Table

```sql
CREATE TABLE city (
  Name CHAR(50),
  Region CHAR(50)
);
```

Verify:

```sql
SHOW TABLES;
DESCRIBE city;
```
<img width="624" height="486" alt="image" src="https://github.com/user-attachments/assets/6a505262-5a22-4537-9ffe-b82b1dfd6a32" />

---

## 6. Challenge 2 — Dropping Tables and Database

Drop the tables:

```sql
DROP TABLE world.city;
DROP TABLE world.country;
```

Verify:

```sql
SHOW TABLES;
```

Drop the database:

```sql
DROP DATABASE world;
```

Verify deletion:

```sql
SHOW DATABASES;
```
<img width="414" height="423" alt="image" src="https://github.com/user-attachments/assets/8e04f3e8-2412-4ea3-a282-4c54b583527c" />

---

## Summary

This lab covered the full lifecycle of SQL schema management:

- Listing databases  
- Creating a database  
- Creating tables with structured schemas  
- Inspecting table metadata  
- Altering table structure  
- Dropping tables  
- Dropping an entire database  

These operations form the foundation for working with relational databases in AWS environments such as RDS and Aurora.

---


