
---

#  Lab Summary — Working with MariaDB on the Command Host

##  **Objective**
This lab focused on connecting to a pre‑configured Command Host, accessing a local MariaDB instance, and practicing SQL operations including data exploration, aggregation, string manipulation, filtering, and deduplication. The lab concluded with a challenge requiring conditional filtering and string splitting.

---

##  **What I Did**
- Connected to the Command Host using **AWS Systems Manager Session Manager**.  
  *(Screenshot placeholder: Connection to Command Host)*

- Switched to the correct working directory and authenticated into MariaDB using the provided root credentials.  
  *(Screenshot placeholder: Successful MariaDB login)*

<img width="769" height="385" alt="Screenshot 2026-04-24 173550" src="https://github.com/user-attachments/assets/8fd5513a-c3d6-44cf-adf9-05db60a74bc9" />

- Explored the `world` sample database and inspected the `country` table.  
  *(Screenshot placeholder: SHOW DATABASES and SELECT * FROM world.country)*


- Practiced SQL operations:
  - Aggregate functions: `SUM()`, `AVG()`, `MAX()`, `MIN()`, `COUNT()`
  - String manipulation: `substring_index()`, `TRIM()`, `LENGTH()`
  - Filtering with `WHERE` and pattern matching
  - Removing duplicates using `DISTINCT()`

---
<img width="909" height="233" alt="Screenshot 2026-04-24 171958" src="https://github.com/user-attachments/assets/cf446e70-e4d4-4a26-8fd7-d762f8ea5d2d" />

---
<img width="1357" height="251" alt="image" src="https://github.com/user-attachments/assets/4fca8c52-f0ca-43a7-9ee9-5144bb6e7c2d" />

---
<img width="678" height="215" alt="image" src="https://github.com/user-attachments/assets/82e12dca-6b41-4eae-8715-ca7bcf251f51" />

---
<img width="633" height="277" alt="image" src="https://github.com/user-attachments/assets/44c14b5a-469e-4e5a-b6c3-38f85955e9bf" />
---
<img width="452" height="244" alt="image" src="https://github.com/user-attachments/assets/68fb3bcd-f989-432c-be3d-cabb6e0b2c39" />
---
<img width="459" height="202" alt="image" src="https://github.com/user-attachments/assets/1e74afd5-fd33-40c6-8d1f-136de7effbe8" />


---

##  **Key Learnings**
- How to connect to a database client on a managed EC2 instance using Session Manager.
- How to inspect database schemas and explore table contents.
- How to summarize data using aggregate functions.
- How to extract and manipulate string fragments using `substring_index()`.
- How to filter rows using string patterns and character length.
- How to remove duplicate values using `DISTINCT()`.
- How to troubleshoot mismatches between lab instructions and actual dataset values.

---

##  **Challenge Note — Mismatch in Lab Instructions**
The challenge instructions referenced the region:

```
Micronesian/Caribbean
```

However, this value **does not exist** in the dataset.  
To verify the correct region name, the following query was used:

```sql
SELECT DISTINCT Region
FROM world.country
WHERE Region LIKE '%/%';
```

**Actual result:**

```
+----------------------+
| Region               |
+----------------------+
| Micronesia/Caribbean |
+----------------------+
```

*(Screenshot placeholder: Verification of region name)*

This confirmed the correct spelling is **Micronesia/Caribbean**, not *Micronesian/Caribbean*.

---

##  **Challenge Solution**

### Final Query

```sql
SELECT 
    Name,
    Region,
    substring_index(Region, '/', 1) AS `Region Name 1`,
    substring_index(Region, '/', -1) AS `Region Name 2`
FROM world.country
WHERE Region = 'Micronesia/Caribbean';
```

*(Screenshot placeholder: Challenge query and output)*
<img width="917" height="270" alt="image" src="https://github.com/user-attachments/assets/625d220d-e8ae-491a-aff5-26b35091fd97" />

This query returns all countries in the `Micronesia/Caribbean` region and splits the region into two separate columns as required.

---

