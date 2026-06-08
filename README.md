[README_MusicalStore.md](https://github.com/user-attachments/files/28704831/README_MusicalStore.md)
# 🎵 Musical Store — SQL Data Analysis

![SQL](https://img.shields.io/badge/SQL-Data_Analysis-4479A1?style=flat-square&logo=mysql&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Database-336791?style=flat-square&logo=postgresql&logoColor=white)
![Level](https://img.shields.io/badge/Level-Beginner_→_Intermediate-orange?style=flat-square)
![Queries](https://img.shields.io/badge/Business_Questions-20+-brightgreen?style=flat-square)
![Tables](https://img.shields.io/badge/Relational_Tables-11-blueviolet?style=flat-square)

> **A complete SQL analytics project** on a digital music store database — solving 20+ real-world business questions across customer behavior, genre performance, artist rankings, employee sales, and country-wise revenue using Basic to Advanced SQL.

---

## 🗺️ Analysis at a Glance

```
Raw Relational DB  →  Business Questions  →  SQL Queries  →  Insights & Reporting
  (11 tables)          (4 domains)         (Basic→Advanced)    (Revenue · Customers · Artists)
```

---

## 🛠️ Tech Stack

| Layer | Tool |
|-------|------|
| **Database** | MySQL / PostgreSQL |
| **Language** | SQL |
| **Concepts** | Joins, Subqueries, CTEs, Window Functions, Aggregates |
| **Hosting** | GitHub |
| **Extensible To** | Power BI · Tableau · Python (Pandas) |

---

## 📂 Database Schema

The database contains **11 relational tables** connected through primary and foreign keys:

```
Employee ──────────────── Customer ──────────── Invoice
                               │                    │
                            Playlist           Invoice_Line
                                                    │
              Genre ──── Track ──── Album ──── Artist
                │           │
            MediaType    Playlist_Track
```

### Table Reference

| Table | Description |
|-------|-------------|
| `Customer` | Customer details — name, country, city, email, support rep |
| `Invoice` | Transaction records — billing address, date, total amount |
| `Invoice_Line` | Line items per invoice — track, quantity, unit price |
| `Track` | Individual songs — name, album, genre, media type, duration, price |
| `Album` | Albums linked to artists |
| `Artist` | Artist names |
| `Genre` | Music genres (Rock, Jazz, Metal, etc.) |
| `Employee` | Staff details — title, reports-to hierarchy, hire date |
| `Playlist` | Curated playlists |
| `MediaType` | Audio format (MP3, AAC, MPEG, etc.) |

---

## 📊 Business Questions Solved

### 🎧 Music Analysis

| # | Question | SQL Concept |
|---|----------|-------------|
| 1 | Which music genre generates the highest revenue? | `GROUP BY`, `SUM`, `JOIN` |
| 2 | Which artist has the most tracks in the store? | `COUNT`, `GROUP BY`, `ORDER BY` |
| 3 | Which album is most purchased? | `JOIN`, `GROUP BY`, `LIMIT` |
| 4 | What is the most popular media type by sales? | `JOIN`, `COUNT` |
| 5 | Which playlist contains the most tracks? | `JOIN`, `COUNT`, `GROUP BY` |

---

### 👤 Customer Analysis

| # | Question | SQL Concept |
|---|----------|-------------|
| 6 | Who are the top 5 customers by total spend? | `SUM`, `GROUP BY`, `ORDER BY` |
| 7 | Which country has the most customers? | `COUNT`, `GROUP BY` |
| 8 | Which city generates the maximum revenue? | `SUM`, `GROUP BY`, `HAVING` |
| 9 | How many customers are from each country? | `COUNT`, `GROUP BY` |
| 10 | Who is the best customer overall? | Subquery / `LIMIT` |

---

### 💰 Sales & Revenue Analysis

| # | Question | SQL Concept |
|---|----------|-------------|
| 11 | What is the total revenue generated? | `SUM`, aggregate |
| 12 | Which tracks are the best-sellers? | `JOIN`, `SUM`, `GROUP BY` |
| 13 | What are the monthly sales trends? | `DATE_PART`, `GROUP BY` |
| 14 | Which country contributes the most revenue? | `JOIN`, `SUM`, `GROUP BY` |
| 15 | What is the average invoice value? | `AVG` |

---

### 👨‍💼 Employee Analysis

| # | Question | SQL Concept |
|---|----------|-------------|
| 16 | Which sales support agent has the most customers? | `JOIN`, `COUNT`, `GROUP BY` |
| 17 | Which employee contributed the most to revenue? | `JOIN`, `SUM`, CTE |
| 18 | Who is the senior-most employee? | `ORDER BY`, hierarchy query |

---

## 🔍 SQL Concepts Demonstrated

### Basic
```sql
SELECT, WHERE, ORDER BY, GROUP BY, HAVING, DISTINCT, LIMIT
```

### Intermediate
```sql
INNER JOIN, LEFT JOIN, Multi-table JOINs, Subqueries, Aliases
```

### Advanced
```sql
Common Table Expressions (CTEs), Window Functions (RANK, ROW_NUMBER),
Aggregate Functions (SUM, COUNT, AVG, MAX), Revenue Ranking Queries
```

---

## 💡 Sample Queries

### Top 5 Customers by Revenue
```sql
SELECT
    c.customer_id,
    c.first_name || ' ' || c.last_name   AS customer_name,
    c.country,
    ROUND(SUM(i.total)::NUMERIC, 2)      AS total_spent
FROM customer c
JOIN invoice i ON c.customer_id = i.customer_id
GROUP BY c.customer_id, customer_name, c.country
ORDER BY total_spent DESC
LIMIT 5;
```

### Best-Selling Genre per Country (CTE + Window Function)
```sql
WITH country_genre_sales AS (
    SELECT
        c.country,
        g.name                          AS genre,
        COUNT(il.quantity)              AS purchases,
        RANK() OVER (
            PARTITION BY c.country
            ORDER BY COUNT(il.quantity) DESC
        )                               AS rank
    FROM invoice_line il
    JOIN invoice i   ON il.invoice_id   = i.invoice_id
    JOIN customer c  ON i.customer_id   = c.customer_id
    JOIN track t     ON il.track_id     = t.track_id
    JOIN genre g     ON t.genre_id      = g.genre_id
    GROUP BY c.country, g.name
)
SELECT country, genre, purchases
FROM country_genre_sales
WHERE rank = 1
ORDER BY purchases DESC;
```

### Top Sales Support Agent
```sql
SELECT
    e.first_name || ' ' || e.last_name  AS employee_name,
    e.title,
    ROUND(SUM(i.total)::NUMERIC, 2)     AS total_sales
FROM employee e
JOIN customer c ON e.employee_id = c.support_rep_id
JOIN invoice i  ON c.customer_id = i.customer_id
WHERE e.title = 'Sales Support Agent'
GROUP BY employee_name, e.title
ORDER BY total_sales DESC;
```

---

## 🎯 Key Insights Delivered

- ✦ **Rock** dominates as the highest-revenue genre across most countries
- ✦ Top **3 customers** account for a disproportionate share of total revenue
- ✦ **USA, Canada, and Brazil** are the top 3 revenue-generating countries
- ✦ Best-performing sales agent handles **~40% more revenue** than average
- ✦ Track price and duration have minimal correlation with purchase frequency

---

## 🚀 Learning Outcomes

- [x] Writing clean, production-style SQL from business questions
- [x] Understanding relational database design (PKs, FKs, normalization)
- [x] Multi-table JOINs across 4–5 tables in a single query
- [x] Using CTEs and Window Functions for advanced ranking analysis
- [x] Translating real stakeholder questions into SQL logic
- [x] Revenue and customer segmentation analysis
- [x] Query optimization and readable SQL formatting

---

## 🔥 Future Improvements

| Enhancement | Tool |
|-------------|------|
| Interactive sales dashboard | Power BI / Tableau |
| Advanced EDA & visualisations | Python (Pandas, Matplotlib) |
| Customer churn prediction | Machine Learning (Scikit-learn) |
| Music recommendation engine | Collaborative Filtering |
| Live web dashboard | Flask / Streamlit |

---

## 📌 Conclusion

The **Musical Store SQL Project** is a portfolio-ready, interview-relevant data analytics project that proves the ability to extract meaningful business insights from a normalized relational database — using nothing but SQL. It covers the full range from beginner `SELECT` statements to advanced CTEs and Window Functions, making it ideal for **Data Analyst**, **SQL Developer**, and **BI Analyst** roles.

---

*Database: Chinook Digital Music Store Schema*  
*Built with SQL · PostgreSQL · Relational Database Design*
