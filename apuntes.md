# 📖 SQL y Sublenguajes SQL 📖


## Índice
- [SQL y Sublenguajes](#sql-y-sublenguajes-sql)
- [DQL](#dql)
- [El SELECT](#comando-select)
- [WHERE y sus atributos](#where-y-sus-atributos)
- [ORDER BY](#order-by)
- [COUNT, SUM y AVG](#count-sum-y-avg)
- [GROUP BY](#group-by)
- [HAVING](#having)
- [Subconsultas](#subconsultas)
- [JOIN](#join)
- [LEFT JOIN y RIGHT JOIN](#otros-tipos-de-join)
- -
- [DDL](#ddl)
- [CREATE](#create)
- [ALTER](#alter)
- [DROP](#drop)
- [Restricciones](#restricciones)
- -
- [DML](#dml)
- [INSERT](#insert)
- [DELETE](#delete)
- [UPDATE](#update)











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
# DQL
Las consultas en DQL siguen la siguiente syntax:
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
  
---

# Comando SELECT 
El comando SELECT lo utilizamos para filtrar las columnas que queremos sacar en la consulta y el FROM para elegir la tabla de donde queremos sacar los datos.
```sql
SELECT population 
FROM world;
```
Con una consulta como esta, obtendríamos la columna 'population' de la tabla 'world'
 
  
---

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
Podemos hacer más de una subconsulta:
```sql
SELECT name,population FROM world
WHERE population BETWEEN
(SELECT population+1 FROM world WHERE name='Canada')
AND
(SELECT population-1 FROM world WHERE name='Poland');
```
---

## JOIN
Cuando necesitamos acceder a otra tabla y no queremos realizar una subconsulta existe la función JOIN, que nos sirve para enlazar tablas a la hora de seleccionar dónde vamos a hacer la consulta (es decir en el FROM).

Para utilizar el JOIN simplemente lo utilizaremos para unir todas las tablas que vamos a necesitar en la consulta, y para combinarlas utilizaremos 2 columnas con tuplas similares, especificadas con un ON. Con la siguiente syntax
```sql
SELECT column_names(de las tablas que sean)
FROM table1 JOIN table2 ON column_table1 = column_table2;
```
Con esto, las tuplas de la **column_table1** que coincidan con las tuplas de la **column_table2** unirán las 2 tablas. 

Un ejemplo de un ejercicio con JOIN:
```sql
SELECT goal.player
FROM game JOIN goal ON game.id = goal.matchid
WHERE teamid='GER';
```
- En esta consulta se unen las tablas **game** y **goal**, de forma que coincidirán en las columnas del ID del partido. 
- Para diferenciar a que tabla pertenece cada columna las definiremos con el formato *table.column*
- En el ON se pueden incluir predicados

### Art Garfunkel 😨
 También podemos hacer JOIN de una tabla con ella misma, para ello necesitamos usar el comando alias [AS] con esto podemos operar con la misma tabla sin que haya redundancia de datos al nombrarla, ya que la nombraremos distinto según para que la vayamos a consultar.
 
 Sabiendo esto podremos hacer este ejercicio, el cual nos pide acceder a la tabla 'actores' de dos maneras distintas
 
```sql
SELECT DISTINCT a2.name
FROM actor AS a1 JOIN casting AS c1 ON c1.actorid = a1.id
                 JOIN casting AS c2 ON c1.movieid = c2.movieid
                 JOIN actor AS a2 ON c2.actorid = a2.id
WHERE a1.name = 'Art Garfunkel'
  AND a1.name <> a2.name;
```
Además utilizamos el comando DISTINCT que nos sirve para que no nos devuelva datos duplicados.


### Otros tipos de JOIN
Existen también los LEFT y RIGHT JOIN que se diferencian en que muestran todos los valores del lado que se le indique aunque tengan **nulos** al otro lado.
```sql
SELECT teacher.name, COALESCE(dept.name,'None')
FROM teacher LEFT JOIN dept ON teacher.dept = dept.id;
```
También tenemos, como vemos en el ejemplo, la función COALESCE, que sustituye los valores nulos por el valor que le introducimos.


---
# DDL
Las consultas en DDL sirven para organizar las estructuras de las bases de datos

## CREATE
```sql
CREATE DATABASE <name>
	[IF NOT EXIST] <nameDB>
    [CHARACTER SET <CharsetName>];
```
```sql
CREATE SCHEMA <name>;
```
```sql
CREATE TABLE <table_name> (
		<atributo1> <dominio1>,
		<atributo2> <dominio2> [NOT NULL] [(*)] [DEFAULT <x>],
		…
		<atributoN> <dominioN>
		[CONSTRAINT <restriction_name> PRIMARY KEY (<atributos>)],
		[CONSTRAINT <restriction_name> FOREIGN KEY (<atributos>)],
		[CONSTRAINT <restriction_name> UNIQUE (<atributos>)],
		...
);

```
Podemos crear tablas, bases de datos, o esquemas.

## ALTER
Con ALTER modificamos la estructura de una tabla, añadiendo o eliminando columnas.

```sql
ALTER TABLE <nome_da_taboa>

	columnas	|		restriccion
	/	\			/	\
engadir	     eliminar		 engadir	 eliminar
ADD		DROP		ADD		DROP

ADD [COLUMN] <atributo1> <dominio1> [NOT NULL] [DEFAULT <x>]
DROP [COLUMN] <atributo1> [CASCADE | RESTRICT]
ADD [CONSTRAINT <nome>] ...
DROP [CONSTRAINT <nome>] …

```

## DROP
Con DROP podemos eliminar bases de datos o tablas
```sql
DROP SCHEMA
[IF EXISTS] <database_name>
[CASCADE|RESTRICT];
```
```sql
DROP TABLE
[IF EXISTS] <table_name>
[CASCADE|RESTRICT];
```
Es importante especificar el CASCADE o RESTRICT

- CASCADE elimina sin importar si hay o no contenido de datos
- RESTRICT protege de ser eliminado si hay datos

## Restricciones

Existen bastantes tipos de restricciones, que se indican de varias formas:

### PRIMARY KEY
Define la clave primaria de la tabla (puede ponerse como una restricción separada o en la misma línea en la declaración del atributo)
```sql
[CONSTRAINT <restriction_name>]
PRIMARY KEY (<atributo>);
```
### FOREIGN KEY
Define la clave foránea y la tabla y atributo al que referencia.
```sql
CONSTRAINT <reference_name> FOREIGN KEY (b1, b2) REFERENCES <table_name> (c1, c2)
);
```
Existen varias propiedades que se le pueden añadir a la FOREIGN KEY: 
- ON (DELETE) determina que hacer por defecto en un DELETE
- NO ACTION (opción por defecto) los datos no se modifican entre tablas
- SET NULL establece NULL 
- SET DEFAULT toma los valores por defecto

### NOT NULL
Evita que un dato sea nulo

### UNIQUE
Protege la duplicación de datos

### CHECK
Limita los valores en una tabla

ejemplo:
```sql
CONSTRAINT check_soldo
CHECK soldo >= (
		SELECT soldo
		FROM empregado
		WHERE dept=’A’
);
```

---

# DML
Sublenguaje sql que utilizamos para introducir o modificar datos en una base de datos o en las tablas.

## INSERT 
Inserta valores en una base de datos
```sql
INSERT INTO <table_name>
[(<atribute1>,<atribute2>,...)]
VALUES 
      (<value1A>,value2A,...),
      (<value1B>,<value2B>,...);
```

## DELETE
Elimina datos dentro de una tabla
```sql
DELETE FROM <table_name>
[WHERE <predicado>];
```

## UPDATE
Modifica los valores de los registros
```sql
UPDATE <table_name>
SET <atribute1> = <value1>
    <atribute2> = <value2>
[WHERE <predicado>];
```