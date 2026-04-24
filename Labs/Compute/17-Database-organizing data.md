
---

#  Lab Summary — Using `GROUP BY` and `OVER()` Clauses

This lab introduced how to work with aggregated data and window functions in SQL using the **world** database. The goal was to understand when to use `GROUP BY` versus `OVER()`, and how functions like `SUM()` and `RANK()` behave in each context.

---

##  Objectives

By completing this lab, I learned how to:

- Use **`GROUP BY`** with aggregate functions like `SUM()`
- Use **`OVER()`** with window functions such as `RANK()`
- Compare grouped aggregates vs. windowed aggregates
- Generate running totals and rankings without collapsing rows

The lab environment provided a pre‑created **world** database with three tables:
- `city`
- `country`
- `countrylanguage`

---

##  What I Did in the Lab

### 1 Checked available databases  
```sql
SHOW DATABASES;
```

<img width="334" height="183" alt="Screenshot 2026-04-24 202845" src="https://github.com/user-attachments/assets/9c9b05d5-b9d2-431c-8ce3-35bed8939683" />

### 2 Explored the `country` table  
```sql
SELECT * FROM world.country;
```

<img width="1350" height="397" alt="image" src="https://github.com/user-attachments/assets/a5d090f3-9d92-4a35-9618-2d30a2d3fdf1" />

<img width="1348" height="225" alt="image" src="https://github.com/user-attachments/assets/baf21cfd-61b9-44c7-bb5e-74343f70d459" />

### 3 Filtered records by region  
```sql
SELECT Region, Name, Population
FROM world.country
WHERE Region = 'Australia and New Zealand'
ORDER BY Population DESC;
```

<img width="1288" height="202" alt="image" src="https://github.com/user-attachments/assets/cfaf623f-3544-498b-9965-cf83794e0b9d" />

### 4 Used `GROUP BY` to aggregate population  
```sql
SELECT Region, SUM(Population)
FROM world.country
WHERE Region = 'Australia and New Zealand'
GROUP BY Region
ORDER BY SUM(Population) DESC;
```

<img width="1343" height="148" alt="image" src="https://github.com/user-attachments/assets/f9201eec-e6e6-48c4-8e7d-909742404a8a" />

### 5 Used a window function to compute a running total  
```sql
SELECT Region, Name, Population,
       SUM(Population) OVER (PARTITION BY Region ORDER BY Population) AS 'Running Total'
FROM world.country
WHERE Region = 'Australia and New Zealand';
```

<img width="1352" height="229" alt="image" src="https://github.com/user-attachments/assets/4157f7ce-813a-47c2-b2c3-096a2288811c" />

### 6 Combined windowed `SUM()` with `RANK()`  
```sql
SELECT Region, Name, Population,
       SUM(Population) OVER (PARTITION BY Region ORDER BY Population) AS 'Running Total',
       RANK() OVER (PARTITION BY Region ORDER BY Population) AS 'Ranked'
FROM world.country
WHERE Region = 'Australia and New Zealand';
```

<img width="1349" height="222" alt="image" src="https://github.com/user-attachments/assets/c0a48991-1145-4408-a300-b92e2fb8c74c" />

---

##  Challenge — Ranking Countries by Population

**Task:**  
Rank all countries **within each region** from largest to smallest population.

**Correct reasoning:**  
- ❌ `GROUP BY` collapses rows → cannot rank individual countries  
- ❌ `SUM()` is for totals → not needed  
- ✅ `OVER()` keeps all rows  
- ✅ `RANK()` assigns ranking within each region  

**Final Query (Correct Solution):**

```sql
SELECT 
    Region,
    Name,
    Population,
    RANK() OVER (PARTITION BY Region ORDER BY Population DESC) AS Population_Rank
FROM world.country;
```

<img width="985" height="429" alt="image" src="https://github.com/user-attachments/assets/e2f991e8-935a-4bbf-bbb3-c15303a8e7a6" />

**Result:**  
239 rows — one per country — each ranked correctly inside its region.


---
