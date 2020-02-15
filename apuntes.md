# SQL y Sublenguajes SQL
SQL (Structured Query Language) es un lenguaje de dominio específico utilizado en programación, diseñado para administrar, y recuperar información de sistemas de gestión de bases de datos relacionales.

Existen 6 tipos de sublenguajes SQL:
- DDL (Data Definition Language) → CREATE, ALTER, DROP
- DML (Data Manipulation Language) → INSERT, UPDATE, DELETE
- DCL (Data Control Language) → GRANT, REVOKE, AUDIT, COMMENT
- TCL (Transition Control Language) → COMMIT, ROLLBACK, SAVEPOINT
- DQL (Data Query Language) → SELECT
- SCL (Session Control Language) → ALTER SESSION, SET ROLL

---
# DQL y la clausula SELECT
```sql
SELECT column_list 
FROM table-name
[WHERE Clause]
[GROUP BY clause]
[HAVING clause]
[ORDER BY clause];
```
