# 📖 SQL y Sublenguajes SQL 📖
__SQL__ (Structured Query Language) es un lenguaje declarativo utilizado en programación, diseñado para administrar, y recuperar información de sistemas de gestión de bases de datos relacionales.

Existen 6 tipos de sublenguajes SQL:
- DDL (Data Definition Language) → CREATE, ALTER, DROP
- DML (Data Manipulation Language) → INSERT, UPDATE, DELETE
- DCL (Data Control Language) → GRANT, REVOKE, AUDIT, COMMENT
- TCL (Transition Control Language) → COMMIT, ROLLBACK, SAVEPOINT
- DQL (Data Query Language) → SELECT
- SCL (Session Control Language) → ALTER SESSION, SET ROLL

---
# DQL y el SELECT
Las consultas con el SELECT siguen la siguiente syntax:
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
  

# Comando SELECT 
El comando SELECT lo utilizamos para filtrar las columnas que queremos sacar en la consulta y el FROM para elegir la tabla de donde queremos sacar los datos.
```sql
SELECT population 
FROM world;
```
Con una consulta como esta, obtendríamos la columna 'population' de la tabla 'world'
 
  

## WHERE y sus atributos
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

```sql
SELECT name 
FROM world
WHERE population > 200000000;
```
También existe **LIKE** que compara valores con el uso de comodines. Los comodines se utilizan para sustituir caracteres sin definirlos, existen los siguientes
- __%__ que sustituye de 0 a más caracteres
- **\_** que sustituye a un carácter único
```sql
SELECT name 
FROM world
WHERE name LIKE '%United%';
```
Este código filtraría todas las tuplas que contengan la palabra 'United'
  

Además de los comparadores podemos usar clausulas como el BETWEEN para devolver valores dentro de un rango, para ello jugaremos con el operador AND:
```sql
SELECT name 
FROM world
WHERE population BETWEEN 200000000 AND 300000000;
```
  

Si queremos filtrar las tuplas de una columna con valores distintos entre si (ya sean cadenas de caracteres o números salteados) podemos utilizar el operador IN, para comparar valores a partir de una lista sin necesidad de usar varias veces el comparador '='

```sql
/* 
SELECT population 
FROM world
WHERE name = 'France'
   OR name = 'Germany'
   OR name = 'Italy';
*/
SELECT name, population 
FROM world
WHERE name IN ('France','Germany','Italy');
```
Como podemos ver en este ejemplo, podemos consultar mas de una columna separando los nombres de las columnas con __comas__ en el SELECT, además podemos hacer __comentarios__ que no influirán en nuestro código, para ello los señalizamos con /* y */ tal y como en el ejemplo.

También hemos usado OR así que explicaremos su funcionamiento junto con el de AND y NOT
- AND requiere que dos o más condiciones sean correctas
- OR requiere que una de las condiciones sea correcta
- NOT niega la condición dada

```sql
SELECT name, population, area
FROM world
WHERE   (population>250000000 or area>3000000)
AND NOT (population>250000000 and area>3000000);
```

Existen funciones como **LENGTH** que sirven para registrar el número de caracteres de una cadena para operar con ello, o **LEFT** que sirve para coger los primeros caracteres de una cadena con el formato
```sql
LENGTH(name)
```
```sql
LEFT(name, 1)
```

---

### ORDER BY 

Para ordenar los resultados de nuestra consulta podemos ordenarlos según la columna que nosotros le digamos con un **ORDER BY**. Se coloca después del WHERE con la siguiente syntax
```sql
SELECT column_name
FROM table_name
WHERE condition
ORDER BY column_name;
```
Por defecto el orden será ascendente pero puede especificarse usando **ASC** o **DESC**
```sql
SELECT company_name, city
FROM dept
ORDER BY company_name DESC;
```
---

### COUNT, SUM y AVG
COUNT, SUM y AVG son funciones que agregan valores según los datos que reciben
- **COUNT**: devuelve un número resultado de contar los valores introducidos
- **SUM**: devuelve la suma de los valores introducidos
- **AVG**: devuelve la media de los valores introducidos

La syntax sería así:
```sql
SELECT [COUNT|SUM|AVG](column_name)
FROM table_name;
```
En caso de que tengamos más columnas en el SELECT además de las que tenemos en las funciones de agregado, debemos agrupar las demás con el comando **GROUP BY** 
También podemos usarlos para filtrar resultados añadiéndolos a la cláusula **HAVING**

---
### GROUP BY
Cuando queremos señalar sobre que tuplas vamos a hacer las funciones de agregado lo hacemos con un GROUP BY
La syntax sería así
```sql
SELECT column_name
FROM table_name
WHERE condition
GROUP BY column_name
ORDER BY column_name;
```
Con un ejemplo sería así:
```sql
SELECT COUNT(id), country 
FROM customer
GROUP BY country;
```
---
### HAVING
La cláusula HAVING filtra resultados en agrupaciones, a diferencia del WHERE que lo hace en las tuplas de forma individual. Igualmente un WHERE y un HAVING pueden ir en la misma consulta. 
La syntax sería así:
```sql
SELECT column_name
FROM table_name
WHERE condition
GROUP BY column_name
HAVING condition
ORDER BY column_name;
```
Aplicándolo en el ejemplo del GROUP BY podríamos aplicarlo así
```sql
SELECT COUNT(id), country 
FROM customer
GROUP BY country
HAVING COUNT(id) > 10
ORDER BY COUNT(id) DESC;
```
---

## Subconsultas

Habrá situaciones en las que se necesitarán datos de otras tablas a las que no podremos acceder directamente con nuestra consulta, para ello realizaremos subconsultas que trabajarán independientemente de la consulta a la que irán anidada (lo que pasa en Las Vegas, se queda en Las Vegas 🎰) .
Podremos colocar las subconsultas en el SELECT y en el WHERE pero en principio las usaremos en el WHERE. Irán siempre entre paréntesis.

Un ejemplo de un caso práctico sería el siguiente:
```sql
SELECT name FROM world
WHERE population > (SELECT population FROM world
                    WHERE name='Romania'
                    );
```






