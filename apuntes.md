# SQL y Sublenguajes SQL
SQL (Structured Query Language) es un lenguaje declarativo utilizado en programación, diseñado para administrar, y recuperar información de sistemas de gestión de bases de datos relacionales.

Existen 6 tipos de sublenguajes SQL:
- DDL (Data Definition Language) → CREATE, ALTER, DROP
- DML (Data Manipulation Language) → INSERT, UPDATE, DELETE
- DCL (Data Control Language) → GRANT, REVOKE, AUDIT, COMMENT
- TCL (Transition Control Language) → COMMIT, ROLLBACK, SAVEPOINT
- DQL (Data Query Language) → SELECT
- SCL (Session Control Language) → ALTER SESSION, SET ROLL

---
# DQL y el SELECT
```sql
SELECT column_list 
FROM table-name
[WHERE Clause]
[GROUP BY clause]
[HAVING clause]
[ORDER BY clause];
```
El código de la consulta debe acabar con un punto y coma ( ; )
Podemos usar operadores para unir clausulas (AND, OR)

# Contenido básico 
El comando SELECT lo utilizamos para filtrar las columnas que queremos sacar en la consulta y el FROM para elegir la tabla de donde queremos sacar los datos.
```sql
SELECT population 
FROM world;
```
Con una consulta como esta, obtendríamos la columna 'population' de la tabla 'world'

Para filtrar los resultados por tuplas usaremos el comando WHERE
```sql
SELECT population 
FROM world
WHERE name = 'Germany';
```
Las cadenas de caracteres van entre comillas simples
Dentro del WHERE podemos usar los comparadores para comprobar los datos. Estos comparadores son los siguientes:

| Símbolo | Significado      |
| ------- | -----------------|
| =       | Igual a          | 
| >       | Mayor que        | 
| <       | Menor que        | 
| >=      | Mayor o igual que|
| <=      | Menor o igual que|
| <>      | No es igual a    |