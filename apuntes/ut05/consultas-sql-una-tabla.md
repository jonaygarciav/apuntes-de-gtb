# Consultas SQL de una sola Tabla

* [Introducción](#introducción)
* [Consultas básicas sobre una tabla](#consultas-básicas-sobre-una-tabla)
  * [Sintaxis de la instrucción SELECT](#sintaxis-de-la-instrucción-select)
  * [Cláusula SELECT](#cláusula-select)
    * [Cómo obtener los datos de todas las columnas de una tabla](#cómo-obtener-los-datos-de-todas-las-columnas-de-una-tabla)
    * [Cómo obtener los datos de algunas columnas de una tabla](#cómo-obtener-los-datos-de-algunas-columnas-de-una-tabla)
    * [Cómo realizar comentarios en sentencias SQL](#cómo-realizar-comentarios-en-sentencias-sql)
    * [Cómo obtener columnas calculadas](#cómo-obtener-columnas-calculadas)
    * [Cómo realizar alias de columnas con AS](#cómo-realizar-alias-de-columnas-con-as)
    * [Cómo utilizar funciones de MySQL en la cláusula SELECT](#cómo-utilizar-funciones-de-mysql-en-la-cláusula-select)
  * [Modificadores ALL, DISTINCT y DISTINCTROW](#modificadores-all-distinct-y-distinctrow)
  * [Cláusula ORDER BY](#cláusula-order-by)
    * [Cómo ordenar de forma ascendente](#cómo-ordenar-de-forma-ascendente)
    * [Cómo ordenar de forma descendente](#cómo-ordenar-de-forma-descendente)
    * [Cómo ordenar utilizando múltiples columnas](#cómo-ordenar-utilizando-múltiples-columnas)
  * [Cláusula LIMIT](#cláusula-limit)
  * [Cláusula WHERE](#cláusula-where)
    * [Operadores disponibles en MySQL](#operadores-disponibles-en-mysql)
    * [Operador BETWEEN](#operador-between)
    * [Operador IN](#operador-in)
    * [Operador LIKE](#operador-like)
    * [Expresiones regulares utilizando los operadores REGEXP y RLIKE](#expresiones-regulares-utilizando-los-operadores-regexp-y-rlike)
    * [Operadores IS e IS NOT](#operadores-is-e-is-not)
  * [Funciones disponibles en MySQL](#funciones-disponibles-en-mysql)
    * [Funciones con cadenas](#funciones-con-cadenas)
    * [Funciones de fecha y hora](#funciones-de-fecha-y-hora)
    * [Funciones matemáticas](#funciones-matemáticas)
    * [Funciones de control de flujo](#funciones-de-control-de-flujo)
* [Errores comunes](#errores-comunes)
  * [Error al comprobar si una columna es NULL](#error-al-comprobar-si-una-columna-es-null)
  * [Error al comparar cadenas con patrones utilizando el operador =](#error-al-comparar-cadenas-con-patrones-utilizando-el-operador-)
  * [Error al comparar cadenas con patrones utilizando el operador !=](#error-al-comparar-cadenas-con-patrones-utilizando-el-operador--1)
  * [Error al comparar un rango de valores con AND](#error-al-comparar-un-rango-de-valores-con-and)
  * [Errores de conversión de tipos en la evaluación de expresiones](#errores-de-conversión-de-tipos-en-la-evaluación-de-expresiones)
  * [Error al utilizar los operadores de suma y resta entre datos de tipo DATE, DATETIME y TIMESTAMP](#error-al-utilizar-los-operadores-de-suma-y-resta-entre-datos-de-tipo-date-datetime-y-timestamp)

## Introducción

__DML__ (_Data Manipulation Language_ o _Lenguaje de Manipulación de Datos_ es la parte de SQL dedicada a la manipulación de los datos. Las sentencias DML son las siguientes:

* `SELECT`: se utiliza para realizar consultas y extraer información de la base de datos.
* `INSERT`: se utiliza para insertar registros en las tablas de la base de datos.
* `UPDATE`: se utiliza para actualizar los registros de una tabla.
* `DELETE`: se utiliza para eliminar registros de una tabla.

![][01]

En este tema nos vamos a centrar en el uso de la sentencia `SELECT`.

## Consultas básicas sobre una tabla

### Sintaxis de la instrucción SELECT

Según la documentación oficial de MySQL ésta sería la sintaxis para realizar una consulta con la sentencia `SELECT` en MySQL:

```sql
SELECT
    [ALL | DISTINCT | DISTINCTROW ]
      [HIGH_PRIORITY]
      [STRAIGHT_JOIN]
      [SQL_SMALL_RESULT] [SQL_BIG_RESULT] [SQL_BUFFER_RESULT]
      [SQL_CACHE | SQL_NO_CACHE] [SQL_CALC_FOUND_ROWS]
    select_expr [, select_expr ...]
    [FROM table_references]
      [PARTITION partition_list]
    [WHERE where_condition]
    [GROUP BY {col_name | expr | position}
      [ASC | DESC], ... [WITH ROLLUP]]
    [HAVING having_condition]
    [ORDER BY {col_name | expr | position}
      [ASC | DESC], ...]
    [LIMIT {[offset,] row_COUNT | row_COUNT OFFSET offset}]
    [PROCEDURE procedure_name(argument_list)]
    [INTO OUTFILE 'file_name'
        [CHARACTER SET charset_name]
        export_options
      | INTO DUMPFILE 'file_name'
      | INTO var_name [, var_name]]
    [FOR UPDATE | LOCK IN SHARE MODE]]
```

Para empezar con consultas sencillas podemos simplificar la definición anterior y quedarnos con la siguiente:

```sql
SELECT [DISTINCT] select_expr [, select_expr ...]
[FROM table_references]
[WHERE where_condition]
[GROUP BY {col_name | expr | position} [ASC | DESC], ... [WITH ROLLUP]]
[HAVING having_condition]
[ORDER BY {col_name | expr | position} [ASC | DESC], ...]
[LIMIT {[offset,] row_COUNT | row_COUNT OFFSET offset}]
```

__Nota__: es muy importante conocer en qué orden se ejecuta cada una de las cláusulas que forman la sentencia `SELECT`. El orden de ejecución es el siguiente:

1. Cláusula `FROM`.
2. Cláusula `WHERE` (Es opcional, puede ser que no aparezca).
3. Cláusula `GROUP BY` (Es opcional, puede ser que no aparezca).
4. Cláusula `HAVING` (Es opcional, puede ser que no aparezca).
5. Cláusula `SELECT`.
6. Cláusula `ORDER BY` (Es opcional, puede ser que no aparezca).
7. Cláusula `LIMIT` (Es opcional, puede ser que no aparezca).

![][02]

Hay que tener en cuenta que el resultado de una consulta siempre será una tabla de datos, que puede tener una o varias columnas y ninguna, una o varias filas.

El hecho de que el resultado de una consulta sea una tabla es muy interesante porque nos permite realizar cosas como almacenar los resultados como una nueva en la base de datos. También será posible combinar el resultado de dos o más consultas para crear una tabla mayor con la unión de los dos resultados. Además, los resultados de una consulta también pueden consultados por otras nuevas consultas.

### Cláusula SELECT

Nos permite indicar cuáles serán las columnas que tendrá la tabla de resultados de la consulta que estamos realizando. Las opciones que podemos indicar son las siguientes:

* El __nombre de una columna__ de la tabla sobre la que estamos realizando la consulta. Será una columna de la tabla que aparece en la cláusula `FROM`.
* Una __constante__ que aparecerá en todas las filas de la tabla resultado.
* Una __expresión__ que nos permite calcular nuevos valores.

#### Cómo obtener los datos de todas las columnas de una tabla

Vamos a utilizar la siguiente base de datos de ejemplo para MySQL:

```sql
DROP DATABASE IF EXISTS instituto;
CREATE DATABASE instituto CHARACTER SET utf8mb4;
USE instituto;

CREATE TABLE alumno (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  apellido1 VARCHAR(100) NOT NULL,
  apellido2 VARCHAR(100),
  fecha_nacimiento DATE NOT NULL,
  es_repetidor ENUM('sí', 'no') NOT NULL,
  teléfono VARCHAR(9)
);

INSERT INTO alumno VALUES(1, 'María', 'Sánchez', 'Pérez', '1990-12-01', 'no', NULL);
INSERT INTO alumno VALUES(2, 'Juan', 'Sáez', 'Vega', '1998-04-02', 'no', 618253876);
INSERT INTO alumno VALUES(3, 'Pepe', 'Ramírez', 'Gea', '1988-01-03', 'no', NULL);
INSERT INTO alumno VALUES(4, 'Lucía', 'Sánchez', 'Ortega', '1993-06-13', 'sí', 678516294);
INSERT INTO alumno VALUES(5, 'Paco', 'Martínez', 'López', '1995-11-24', 'no', 692735409);
INSERT INTO alumno VALUES(6, 'Irene', 'Gutiérrez', 'Sánchez', '1991-03-28', 'sí', NULL);
INSERT INTO alumno VALUES(7, 'Cristina', 'Fernández', 'Ramírez', '1996-09-17', 'no', 628349590);
INSERT INTO alumno VALUES(8, 'Antonio', 'Carretero', 'Ortega', '1994-05-20', 'sí', 612345633);
INSERT INTO alumno VALUES(9, 'Manuel', 'Domínguez', 'Hernández', '1999-07-08', 'no', NULL);
INSERT INTO alumno VALUES(10, 'Daniel', 'Moreno', 'Ruiz', '1998-02-03', 'no', NULL);
```

Supongamos que tenemos una tabla llamada __alumno__ con la siguiente información de los alumnos matriculados en un determinado curso:

| id  | nombre  | apellido1  | apellido2  | fecha_nacimiento | es_repetidor | teléfono   |
| --- | ------- | ---------- | ---------- | ---------------- | ------------ | ---------- |
| 1   | María   | Sánchez    | Pérez      | 1990/12/01       | no           | 618253876  |
| 2   | Juan    | Sáez       | Vega       | 1998/04/02       | no           | 618253876  |
| 3   | Pepe    | Ramírez    | Gea        | 1988/01/03       | no           | NULL       |
| 4   | Lucía   | Sánchez    | Ortega     | 1993/06/13       | sí           | 678516294  |
| 5   | Paco    | Martínez   | López      | 1995/11/24       | no           | 692735409  |
| 6   | Irene   | Gutiérrez  | Sánchez    | 1991/03/23       | sí           | NULL       |
| 7   | Cristina| Fernández  | Ramírez    | 1996/09/17       | no           | 628349590  |
| 8   | Antonio | Carretero  | Ortega     | 1994/05/20       | sí           | 612345633  |
| 9   | Manuel  | Domínguez  | Hernández  | 1997/08/07       | no           | NULL       |
| 10  | Daniel  | Moreno     | Ruiz       | 1998/02/03       | no           | NULL       |

Vamos a ver qué consultas sería necesario realizar para obtener los siguientes datos.

Cómo obtener  todos los datos de todos los alumnos matriculados en el curso:

```sql
SELECT *
FROM alumno;

+----+----------+------------+------------+------------------+--------------+-----------+
| id | nombre   | apellido1  | apellido2  | fecha_nacimiento | es_repetidor | teléfono  |
+----+----------+------------+------------+------------------+--------------+-----------+
|  1 | María    | Sánchez    | Pérez      | 1990-12-01       | no           | NULL      |
|  2 | Juan     | Sáez       | Vega       | 1998-04-02       | no           | 618253876 |
|  3 | Pepe     | Ramírez    | Gea        | 1988-01-03       | no           | NULL      |
|  4 | Lucía    | Sánchez    | Ortega     | 1993-06-13       | sí           | 678516294 |
|  5 | Paco     | Martínez   | López      | 1995-11-24       | no           | 692735409 |
|  6 | Irene    | Gutiérrez  | Sánchez    | 1991-03-28       | sí           | NULL      |
|  7 | Cristina | Fernández  | Ramírez    | 1996-09-17       | no           | 628349590 |
|  8 | Antonio  | Carretero  | Ortega     | 1994-05-20       | sí           | 612345633 |
|  9 | Manuel   | Domínguez  | Hernández  | 1999-07-08       | no           | NULL      |
| 10 | Daniel   | Moreno     | Ruiz       | 1998-02-03       | no           | NULL      |
+----+----------+------------+------------+------------------+--------------+-----------+
10 rows in set (0,00 sec)
```

El carácter `*` es un comodín que utilizamos para indicar que queremos seleccionar todas las columnas de la tabla. La consulta anterior devolverá todos los datos de la tabla.

Otra consideración que también debemos tener en cuenta es que una consulta SQL se puede escribir en una o varias líneas. Por ejemplo, la siguiente sentencia tendría el mismo resultado que la anterior:

```sql
SELECT * FROM alumno;
```

#### Cómo obtener los datos de algunas columnas de una tabla

Obtener el nombre de todos los alumnos:

```sql
-- Obtener el nombre de todos los alumnos.
SELECT nombre
FROM alumno;

+----------+
| nombre   |
+----------+
| María    |
| Juan     |
| Pepe     |
| Lucía    |
| Paco     |
| Irene    |
| Cristina |
| Antonio  |
| Manuel   |
| Daniel   |
+----------+
10 rows in set (0,00 sec)
```

Obtener el nombre y los apellidos de todos los alumnos:

```sql
-- Obtener el nombre y los apellidos de todos los alumnos.
SELECT nombre, apellido1, apellido2
FROM alumno;

+----------+------------+------------+
| nombre   | apellido1  | apellido2  |
+----------+------------+------------+
| María    | Sánchez    | Pérez      |
| Juan     | Sáez       | Vega       |
| Pepe     | Ramírez    | Gea        |
| Lucía    | Sánchez    | Ortega     |
| Paco     | Martínez   | López      |
| Irene    | Gutiérrez  | Sánchez    |
| Cristina | Fernández  | Ramírez    |
| Antonio  | Carretero  | Ortega     |
| Manuel   | Domínguez  | Hernández  |
| Daniel   | Moreno     | Ruiz       |
+----------+------------+------------+
10 rows in set (0,00 sec)
```

Ten en cuenta que el resultado de la consulta SQL mostrará las columnas que haya solicitado, siguiendo el orden en el que se hayan indicado. Por lo tanto la siguiente consulta:

```sql
-- Obtener los apellidos y el nombre de todos los alumnos.
SELECT apellido1, apellido2, nombre
FROM alumno;
```

Devolverá lo siguiente:

```sql
+------------+------------+----------+
| apellido1  | apellido2  | nombre   |
+------------+------------+----------+
| Sánchez    | Pérez      | María    |
| Sáez       | Vega       | Juan     |
| Ramírez    | Gea        | Pepe     |
| Sánchez    | Ortega     | Lucía    |
| Martínez   | López      | Paco     |
| Gutiérrez  | Sánchez    | Irene    |
| Fernández  | Ramírez    | Cristina |
| Carretero  | Ortega     | Antonio  |
| Domínguez  | Hernández  | Manuel   |
| Moreno     | Ruiz       | Daniel   |
+------------+------------+----------+
10 rows in set (0,00 sec)
```

#### Cómo realizar comentarios en sentencias SQL

Para comentarios de una línea se utilizan los caracteres `--` o `#`:

```sql
-- Esto es un comentario
SELECT nombre, apellido1, apellido2 -- Esto es otro comentario
FROM alumno;
```

```sql
# Esto es un comentario
SELECT nombre, apellido1, apellido2 # Esto es otro comentario
FROM alumno;
```

Para comentarios de múltiples líneas se utilizan los caracteres `/*` para comenzar el comentario y `*/` para cerrar el comentario:

```sql
/* Esto es un comentario
   de varias líneas */
SELECT nombre, apellido1, apellido2 /* Esto es otro comentario */
FROM alumno;
```

#### Cómo obtener columnas calculadas

Vamos a utilizar la siguiente base de datos de ejemplo para MySQL:

```sql
DROP DATABASE IF EXISTS tienda;
CREATE DATABASE tienda CHARACTER SET utf8mb4;
USE tienda;

CREATE TABLE ventas (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  cantidad_comprada INT UNSIGNED NOT NULL,
  precio_por_elemento DECIMAL(7,2) NOT NULL 
);

INSERT INTO ventas VALUES (1, 2, 1.50);
INSERT INTO ventas VALUES (2, 5, 1.75);
INSERT INTO ventas VALUES (3, 7, 2.00);
INSERT INTO ventas VALUES (4, 9, 3.50);
INSERT INTO ventas VALUES (5, 6, 9.99);
```

Es posible realizar cálculos aritméticos entre columnas para calcular nuevos valores. Por ejemplo, supongamos que tenemos la siguiente tabla con información sobre ventas.

```sql
| id  | cantidad_comprada | precio_por_elemento |
| --- | ----------------- | ------------------- |
| 1   | 2                 | 1.50                |
| 2   | 5                 | 1.75                |
| 3   | 7                 | 2.00                |
| 4   | 9                 | 3.50                |
| 5   | 6                 | 9.99                |
```

Y queremos calcular una nueva columna con el precio total de la venta, que sería equivalente a multiplicar el valor de la column _cantidad\_comprada_ por _precio\_por\_elemento_.

Para obtener esta nueva columna podríamos realizar la siguiente consulta:

```sql
SELECT id, cantidad_comprada, precio_por_elemento, cantidad_comprada * precio_por_elemento
FROM ventas;
```

Y el resultado sería el siguiente:

```sql
+----+-------------------+---------------------+-----------------------------------------+
| id | cantidad_comprada | precio_por_elemento | cantidad_comprada * precio_por_elemento |
+----+-------------------+---------------------+-----------------------------------------+
|  1 |                 2 |                1.50 |                                    3.00 |
|  2 |                 5 |                1.75 |                                    8.75 |
|  3 |                 7 |                2.00 |                                   14.00 |
|  4 |                 9 |                3.50 |                                   31.50 |
|  5 |                 6 |                9.99 |                                   59.94 |
+----+-------------------+---------------------+-----------------------------------------+
5 rows in set (0,00 sec)
```

Para este ejemplo, vamos a utilizar la siguiente base de datos de ejemplo para MySQL:

```sql
DROP DATABASE IF EXISTS company;
CREATE DATABASE company CHARACTER SET utf8mb4;
USE company;

CREATE TABLE offices (
  office INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  city VARCHAR(50) NOT NULL,
  region VARCHAR(50) NOT NULL,
  manager INT UNSIGNED,
  target DECIMAL(9,2) NOT NULL,
  sales DECIMAL(9,2) NOT NULL
);

INSERT INTO offices VALUES (11, 'New York', 'Eastern', 106,  575000, 692637);
INSERT INTO offices VALUES (12, 'Chicago', 'Eastern', 104, 800000, 735042);
INSERT INTO offices VALUES (13, 'Atlanta', 'Eastern', NULL, 350000, 367911);
INSERT INTO offices VALUES (21, 'Los Angeles', 'Western', 108, 725000, 835915);
INSERT INTO offices VALUES (22, 'Denver', 'Western', 108, 300000, 186042);
```

Supongamos que tenemos una tabla llamada oficinas que contiene información sobre las ventas reales que ha generado y el valor de ventas esperado, y nos gustaría conocer si las oficinas han conseguido el objetivo propuesto, y si están por debajo o por encima del valor de ventas esperado.

![][03]

```sql
-- Crear la base de datos
CREATE DATABASE office_sales;

-- Usar la base de datos
USE office_sales;

-- Crear la tabla offices
CREATE TABLE offices (
    office INT PRIMARY KEY,
    city VARCHAR(50),
    region VARCHAR(50),
    mgr INT,
    target DECIMAL(10, 2),
    sales DECIMAL(10, 2)
);

-- Insertar los datos en la tabla offices
INSERT INTO offices (office, city, region, mgr, target, sales) VALUES
(22, 'Denver', 'Western', 108, 300000.00, 186042.00),
(21, 'Los Angeles', 'Western', 108, 725000.00, 835915.00),
(12, 'New York', 'Eastern', 106, 575000.00, 692637.00),
(13, 'Chicago', 'Eastern', 104, 800000.00, 735042.00),
(14, 'Atlanta', 'Eastern', NULL, 350000.00, 367911.00);
```

En este caso podríamos realizar la siguiente consulta:

```sql
-- comparación entre las ventas reales y las metas de ventas para cada oficina.
SELECT city, region,  sales - target
FROM offices;

+-------------+---------+--------------+
| city        | region  | sales-target |
+-------------+---------+--------------+
| New York    | Eastern |    117637.00 |
| Chicago     | Eastern |    -64958.00 |
| Atlanta     | Eastern |     17911.00 |
| Los Angeles | Western |    110915.00 |
| Denver      | Western |   -113958.00 |
+-------------+---------+--------------+
5 rows in set (0,00 sec)
```

#### Cómo realizar alias de columnas con AS

Con la palabra reservada AS podemos crear alias para las columnas. Esto puede ser útil cuando estamos calculando nuevas columnas a partir de valores de las columnas actuales. En el ejemplo anterior de la tabla que contiene información sobre las ventas, podríamos crear el siguiente alias:

```sql
SELECT id, cantidad_comprada, precio_por_elemento, cantidad_comprada * precio_por_elemento AS 'precio total'
FROM ventas;
```

Si el nuevo nombre que estamos creando para el alias contiene espacios en blanco es necesario usar comillas simples.

Al crear este alias obtendremos el siguiente resultado:

```sql
+----+-------------------+---------------------+--------------+
| id | cantidad_comprada | precio_por_elemento | precio total |
+----+-------------------+---------------------+--------------+
|  1 |                 2 |                1.50 |         3.00 |
|  2 |                 5 |                1.75 |         8.75 |
|  3 |                 7 |                2.00 |        14.00 |
|  4 |                 9 |                3.50 |        31.50 |
|  5 |                 6 |                9.99 |        59.94 |
+----+-------------------+---------------------+--------------+
5 rows in set (0,00 sec)
```

#### Cómo utilizar funciones de MySQL en la cláusula SELECT

Es posible hacer uso de funciones específicas de MySQL en la cláusula `SELECT`. MySQL nos ofrece funciones matemáticas, funciones para trabajar con cadenas y funciones para trabajar con fechas y horas. Algunos ejemplos de las funciones de MySQL que utilizaremos a lo largo del curso son las siguientes.

Funciones con cadenas

| Función    | Descripción                                  |
| ---------- | -------------------------------------------- |
| CONCAT     | Concatena cadenas                            |
| CONCAT_WS  | Concatena cadenas con un separador           |
| LOWER      | Devuelve una cadena en minúscula             |
| UPPER      | Devuelve una cadena en mayúscula             |
| SUBSTR     | Devuelve una subcadena                       |

Funciones matemáticas:

| Función     | Descripción                        |
| ----------- | ---------------------------------- |
| ABS()       | Devuelve el valor absoluto         |
| POW(x,y)    | Devuelve el valor de x elevado a y |
| SQRT()      | Devuelve la raíz cuadrada          |
| PI()        | Devuelve el valor del número PI    |
| ROUND()     | Redondea un valor numérico         |
| TRUNCATE()  | Trunca un valor numérico           |

Funciones de fecha y hora:

| Función   | Descripción                        |
| --------- | ---------------------------------- |
| NOW()     | Devuelve la fecha y la hora actual |
| CURTIME() | Devuelve la hora actual            |

En la (documentación oficial)[https://dev.mysql.com/doc/refman/8.4/en/functions.html] puede encontrar la referencia completa de todas las funciones y operadores disponibles en MySQL

Ejemplo de cómo obtener el nombre y los apellidos de todos los alumnos en una única columna.

```sql
SELECT CONCAT(nombre, apellido1, apellido2) AS nombre_completo
FROM alumno;

+----------------------------+
| nombre_completo            |
+----------------------------+
| MaríaSánchezPérez          |
| JuanSáezVega               |
| PepeRamírezGea             |
| LucíaSánchezOrtega         |
| PacoMartínezLópez          |
| IreneGutiérrezSánchez      |
| CristinaFernándezRamírez   |
| AntonioCarreteroOrtega     |
| ManuelDomínguezHernández   |
| DanielMorenoRuiz           |
+----------------------------+
10 rows in set (0,00 sec)
```

En este caso estamos haciendo uso de la función `CONCAT` de _MySQL_ y la palabra reservada AS para crear un alias de la columna y renombrarla como nombre_completo.

La función `CONCAT` de MySQL no añade ningún espacio entre las columnas, por eso los valores de las tres columnas aparecen como una sola cadena sin espacios entre ellas. Para resolver este problema podemos hacer uso de la función `CONCAT_WS` que nos permite definir un carácter separador entre cada columna. En el siguiente ejemplo haremos uso de la función CONCAT_WS y usaremos un espacio en blanco como separador.

```sql
SELECT CONCAT_WS(' ', nombre, apellido1, apellido2) AS nombre_completo
FROM alumno;

+------------------------------+
| nombre_completo              |
+------------------------------+
| María Sánchez Pérez          |
| Juan Sáez Vega               |
| Pepe Ramírez Gea             |
| Lucía Sánchez Ortega         |
| Paco Martínez López          |
| Irene Gutiérrez Sánchez      |
| Cristina Fernández Ramírez   |
| Antonio Carretero Ortega     |
| Manuel Domínguez Hernández   |
| Daniel Moreno Ruiz           |
+------------------------------+
10 rows in set (0,00 sec)
```

> __Nota__: la función `CONCAT` devolverá NULL cuando alguna de las cadenas que está concatenando es igual `NULL`, mientras que la función CONCAT_WS omitirá todas las cadenas que sean igual a `NULL` y realizará la concatenación con el resto de cadenas.

Obtener el nombre y los apellidos de todos los alumnos en una única columna en minúscula:

```sql
SELECT LOWER(CONCAT(nombre, ' ', apellido1, ' ', apellido2)) AS nombre_completo FROM alumno;

+------------------------------+
| nombre_completo              |
+------------------------------+
| maría sánchez pérez          |
| juan sáez vega               |
| pepe ramírez gea             |
| lucía sánchez ortega         |
| paco martínez lópez          |
| irene gutiérrez sánchez      |
| cristina fernández ramírez   |
| antonio carretero ortega     |
| manuel domínguez hernández   |
| daniel moreno ruiz           |
+------------------------------+
```

Obtener el nombre y los apellidos de todos los alumnos en una única columna en mayúscula:

```sql
SELECT UPPER(CONCAT(nombre, ' ', apellido1, ' ', apellido2)) AS nombre_completo FROM alumno;

+------------------------------+
| nombre_completo              |
+------------------------------+
| MARÍA SÁNCHEZ PÉREZ          |
| JUAN SÁEZ VEGA               |
| PEPE RAMÍREZ GEA             |
| LUCÍA SÁNCHEZ ORTEGA         |
| PACO MARTÍNEZ LÓPEZ          |
| IRENE GUTIÉRREZ SÁNCHEZ      |
| CRISTINA FERNÁNDEZ RAMÍREZ   |
| ANTONIO CARRETERO ORTEGA     |
| MANUEL DOMÍNGUEZ HERNÁNDEZ   |
| DANIEL MORENO RUIZ           |
+------------------------------+
10 rows in set (0,00 sec)
```

Obtener el nombre y los apellidos de todos los alumnos en una única columna. Cuando el segundo apellido de un alumno sea `NULL` se devolverá el nombre y el primer apellido concatenados en mayúscula, y cuando no lo sea, se devolverá el nombre completo concatenado tal y como aparece en la tabla:

```sql
SELECT 
    CASE
        WHEN apellido2 IS NULL THEN UPPER(CONCAT(nombre, ' ', apellido1))
        ELSE CONCAT(nombre, ' ', apellido1, ' ', apellido2)
    END AS nombre_completo
FROM alumnos;

+------------------------------+
| nombre_completo              |
+------------------------------+
| María Sánchez Pérez          |
| Juan Sáez Vega               |
| Pepe Ramírez Gea             |
| Lucía Sánchez Ortega         |
| Paco Martínez López          |
| Irene Gutiérrez Sánchez      |
| Cristina Fernández Ramírez   |
| Antonio Carretero Ortega     |
| Manuel Domínguez Hernández   |
| Daniel Moreno Ruiz           |
+------------------------------+
10 rows in set (0,00 sec)
```

### Modificadores ALL, DISTINCT y DISTINCTROW

Los modificadores `ALL` y `DISTINCT` indican si se deben incluir o no filas repetidas en el resultado de la consulta.

* `ALL` indica que se deben incluir todas las filas, incluidas las repetidas. Es la opción por defecto, por lo tanto no es necesario indicarla.
* `DISTINCT` elimina las filas repetidas en el resultado de la consulta.
* `DISTINCTROW` es un sinónimo de `DISTINCT`.

En el siguiente ejemplo vamos a ver la diferencia que existe entre utilizar `DISTINCT` y no utilizarlo. La siguiente consulta mostrará todas las filas que existen en la columna apellido1 de la tabla alumno.

```sql
SELECT apellido1
FROM alumno;

+------------+
| apellido1  |
+------------+
| Sánchez    |
| Sáez       |
| Ramírez    |
| Sánchez    |
| Martínez   |
| Gutiérrez  |
| Fernández  |
| Carretero  |
| Domínguez  |
| Moreno     |
+------------+
10 rows in set (0,00 sec)
```

Si en la consulta anterior utilizamos `DISTINCT` se eliminarán todos los valores repetidos que existan.

```sql
SELECT DISTINCT apellido1
FROM alumno;

+------------+
| apellido1  |
+------------+
| Sánchez    |
| Sáez       |
| Ramírez    |
| Martínez   |
| Gutiérrez  |
| Fernández  |
| Carretero  |
| Domínguez  |
| Moreno     |
+------------+
9 rows in set (0,01 sec)
```

Si en la cláusula `SELECT` utilizamos `DISTINCT` con más de una columna, la consulta seguirá eliminando todas las filas repetidas que existan. Por ejemplo, si tenemos las columnas _apellido1_, _apellido2_ y _nombre_, se eliminarán todas las filas que tengan los mismos valores en las tres columnas.

```sql
SELECT DISTINCT apellido1, apellido2, nombre
FROM alumno;

+------------+------------+----------+
| apellido1  | apellido2  | nombre   |
+------------+------------+----------+
| Sánchez    | Pérez      | María    |
| Sáez       | Vega       | Juan     |
| Ramírez    | Gea        | Pepe     |
| Sánchez    | Ortega     | Lucía    |
| Martínez   | López      | Paco     |
| Gutiérrez  | Sánchez    | Irene    |
| Fernández  | Ramírez    | Cristina |
| Carretero  | Ortega     | Antonio  |
| Domínguez  | Hernández  | Manuel   |
| Moreno     | Ruiz       | Daniel   |
+------------+------------+----------+
10 rows in set (0,00 sec)
```

> __Nota__: en este ejemplo no se ha eliminado ninguna fila porque no existen alumnos que tengan los mismos apellidos y el mismo nombre.

### Cláusula ORDER BY

`ORDER BY` permite ordenar las filas que se incluyen en el resultado de la consulta. La sintaxis de MySQL es la siguiente:

```sql
[ORDER BY {col_name | expr | position} [ASC | DESC], ...]
```

Esta cláusula nos permite ordenar el resultado de forma ascendente `ASC` o descendente `DESC`, además de permitirnos ordenar por varias columnas estableciendo diferentes niveles de ordenación.

#### Cómo ordenar de forma ascendente

Cómo obtener el nombre y los apellidos de todos los alumnos, ordenados por su primer apellido de forma ascendente:

```sql
SELECT apellido1, apellido2, nombre
FROM alumno
ORDER BY apellido1;

+------------+------------+----------+
| apellido1  | apellido2  | nombre   |
+------------+------------+----------+
| Carretero  | Ortega     | Antonio  |
| Domínguez  | Hernández  | Manuel   |
| Fernández  | Ramírez    | Cristina |
| Gutiérrez  | Sánchez    | Irene    |
| Martínez   | López      | Paco     |
| Moreno     | Ruiz       | Daniel   |
| Ramírez    | Gea        | Pepe     |
| Sáez       | Vega       | Juan     |
| Sánchez    | Pérez      | María    |
| Sánchez    | Ortega     | Lucía    |
+------------+------------+----------+
10 rows in set (0,00 sec)
```

Si no indicamos nada en la cláusula `ORDER BY` se ordenará por defecto de forma ascendente.

La consulta anterior es equivalente a esta otra.

```sql
SELECT apellido1, apellido2, nombre
FROM alumno
ORDER BY apellido1 ASC;
```

El resultado de ambas consultas será:

```
+------------+------------+----------+
| apellido1  | apellido2  | nombre   |
+------------+------------+----------+
| Carretero  | Ortega     | Antonio  |
| Domínguez  | Hernández  | Manuel   |
| Fernández  | Ramírez    | Cristina |
| Gutiérrez  | Sánchez    | Irene    |
| Martínez   | López      | Paco     |
| Moreno     | Ruiz       | Daniel   |
| Ramírez    | Gea        | Pepe     |
| Sáez       | Vega       | Juan     |
| Sánchez    | Pérez      | María    |
| Sánchez    | Ortega     | Lucía    |
+------------+------------+----------+
10 rows in set (0,00 sec)
```

Las filas están ordenadas correctamente por el primer apellido, pero todavía hay que resolver cómo ordenar el listado cuando existen varias filas donde coincide el valor del primer apellido. En este caso tenemos dos filas donde el primer apellido es _Sánchez_:

```
nombre	apellido1	apellido2
…	…	…
Sánchez	Pérez	María
Sánchez	Ortega	Lucía
```

#### Cómo ordenar de forma descendente

Cómo obtener el nombre y los apellidos de todos los alumnos, ordenados por su primer apellido de forma ascendente:

```sql
SELECT apellido1, apellido2, nombre
FROM alumno
ORDER BY apellido1 DESC;

+------------+------------+----------+
| apellido1  | apellido2  | nombre   |
+------------+------------+----------+
| Sánchez    | Pérez      | María    |
| Sánchez    | Ortega     | Lucía    |
| Sáez       | Vega       | Juan     |
| Ramírez    | Gea        | Pepe     |
| Moreno     | Ruiz       | Daniel   |
| Martínez   | López      | Paco     |
| Gutiérrez  | Sánchez    | Irene    |
| Fernández  | Ramírez    | Cristina |
| Domínguez  | Hernández  | Manuel   |
| Carretero  | Ortega     | Antonio  |
+------------+------------+----------+
10 rows in set (0,00 sec)
```

#### Cómo ordenar utilizando múltiples columnas

En este ejemplo vamos obtener un listado de todos los alumnos ordenados por el primer apellido, segundo apellido y nombre, de forma ascendente.

```sql
SELECT apellido1, apellido2, nombre
FROM alumno
ORDER BY apellido1, apellido2, nombre;

+------------+------------+----------+
| apellido1  | apellido2  | nombre   |
+------------+------------+----------+
| Carretero  | Ortega     | Antonio  |
| Domínguez  | Hernández  | Manuel   |
| Fernández  | Ramírez    | Cristina |
| Gutiérrez  | Sánchez    | Irene    |
| Martínez   | López      | Paco     |
| Moreno     | Ruiz       | Daniel   |
| Ramírez    | Gea        | Pepe     |
| Sáez       | Vega       | Juan     |
| Sánchez    | Ortega     | Lucía    |
| Sánchez    | Pérez      | María    |
+------------+------------+----------+
10 rows in set (0,00 sec)
```

En lugar de indicar el nombre de las columnas en la cláusula `ORDER BY` podemos indicar sobre la posición donde aparecen en la cláusula `SELECT`, de modo que la consulta anterior sería equivalente a la siguiente:

```sql
SELECT apellido1, apellido2, nombre
FROM alumno
ORDER BY 1, 2, 3;

+------------+------------+----------+
| apellido1  | apellido2  | nombre   |
+------------+------------+----------+
| Carretero  | Ortega     | Antonio  |
| Domínguez  | Hernández  | Manuel   |
| Fernández  | Ramírez    | Cristina |
| Gutiérrez  | Sánchez    | Irene    |
| Martínez   | López      | Paco     |
| Moreno     | Ruiz       | Daniel   |
| Ramírez    | Gea        | Pepe     |
| Sáez       | Vega       | Juan     |
| Sánchez    | Ortega     | Lucía    |
| Sánchez    | Pérez      | María    |
+------------+------------+----------+
10 rows in set (0,00 sec)
```

### Cláusula LIMIT

`LIMIT` permite limitar el número de filas que se incluyen en el resultado de la consulta. La sintaxis de MySQL es la siguiente

```sql
[LIMIT {[offset,] row_COUNT | row_COUNT OFFSET offset}]
```

donde `row_COUNT` es el número de filas que queremos obtener y offset el número de filas que nos saltamos antes de empezar a contar. Es decir, la primera fila que se obtiene como resultado es la que está situada en la posición offset + 1.

Teniendo en cuenta la siguiente Base de Datos:

```sql
DROP DATABASE IF EXISTS google;
CREATE DATABASE google CHARACTER SET utf8mb4;
USE google;

CREATE TABLE resultado (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  descripcion VARCHAR(200) NOT NULL,
  url VARCHAR(512) NOT NULL
);

INSERT INTO resultado VALUES (1, 'Resultado 1', 'Descripción 1', 'https://www.google.com');
INSERT INTO resultado VALUES (2, 'Resultado 2', 'Descripción 2', 'https://www.example.com');
INSERT INTO resultado VALUES (3, 'Resultado 3', 'Descripción 3', 'https://www.wikipedia.org');
INSERT INTO resultado VALUES (4, 'Resultado 4', 'Descripción 4', 'https://www.github.com');
INSERT INTO resultado VALUES (5, 'Resultado 5', 'Descripción 5', 'https://www.stackoverflow.com');
INSERT INTO resultado VALUES (6, 'Resultado 6', 'Descripción 6', 'https://www.reddit.com');
INSERT INTO resultado VALUES (7, 'Resultado 7', 'Descripción 7', 'https://www.medium.com');
INSERT INTO resultado VALUES (8, 'Resultado 8', 'Descripción 8', 'https://www.linkedin.com');
INSERT INTO resultado VALUES (9, 'Resultado 9', 'Descripción 9', 'https://www.twitter.com');
INSERT INTO resultado VALUES (10, 'Resultado 10', 'Descripción 10', 'https://www.facebook.com');
INSERT INTO resultado VALUES (11, 'Resultado 11', 'Descripción 11', 'https://www.instagram.com');
INSERT INTO resultado VALUES (12, 'Resultado 12', 'Descripción 12', 'https://www.youtube.com');
INSERT INTO resultado VALUES (13, 'Resultado 13', 'Descripción 13', 'https://www.pinterest.com');
INSERT INTO resultado VALUES (14, 'Resultado 14', 'Descripción 14', 'https://www.quora.com');
INSERT INTO resultado VALUES (15, 'Resultado 15', 'Descripción 15', 'https://www.tumblr.com');
```

Supongamos que queremos mostrar en una página web las filas que están almacenadas en la tabla resultados, y queremos mostrar la información en diferentes páginas, donde cada una de las páginas muestra solamente 5 resultados. ¿Qué consulta SQL necesitamos realizar para incluir los primeros 5 resultados de la primera página?

```sql
SELECT * FROM resultado
LIMIT 5 OFFSET 0;

+----+-------------+----------------+-------------------------------+
| id | nombre      | descripcion    | url                           |
+----+-------------+----------------+-------------------------------+
|  1 | Resultado 1 | Descripción 1  | https://www.google.com        |
|  2 | Resultado 2 | Descripción 2  | https://www.example.com       |
|  3 | Resultado 3 | Descripción 3  | https://www.wikipedia.org     |
|  4 | Resultado 4 | Descripción 4  | https://www.github.com        |
|  5 | Resultado 5 | Descripción 5  | https://www.stackoverflow.com |
+----+-------------+----------------+-------------------------------+
5 rows in set (0,00 sec)
```

¿Qué consulta necesitaríamos para mostrar resultados de la segunda página?

```sql
SELECT * FROM resultado
LIMIT 5 OFFSET 5;

+----+--------------+-----------------+--------------------------+
| id | nombre       | descripcion     | url                      |
+----+--------------+-----------------+--------------------------+
|  6 | Resultado 6  | Descripción 6   | https://www.reddit.com   |
|  7 | Resultado 7  | Descripción 7   | https://www.medium.com   |
|  8 | Resultado 8  | Descripción 8   | https://www.linkedin.com |
|  9 | Resultado 9  | Descripción 9   | https://www.twitter.com  |
| 10 | Resultado 10 | Descripción 10  | https://www.facebook.com |
+----+--------------+-----------------+--------------------------+
5 rows in set (0,00 sec)
```

¿Y los resultados de la tercera página?

```sql
SELECT * FROM resultado
LIMIT 5 OFFSET 10;

+----+--------------+-----------------+---------------------------+
| id | nombre       | descripcion     | url                       |
+----+--------------+-----------------+---------------------------+
| 11 | Resultado 11 | Descripción 11  | https://www.instagram.com |
| 12 | Resultado 12 | Descripción 12  | https://www.youtube.com   |
| 13 | Resultado 13 | Descripción 13  | https://www.pinterest.com |
| 14 | Resultado 14 | Descripción 14  | https://www.quora.com     |
| 15 | Resultado 15 | Descripción 15  | https://www.tumblr.com    |
+----+--------------+-----------------+---------------------------+
5 rows in set (0,00 sec)
```

### Cláusula WHERE

La cláusula `WHERE` nos permite añadir filtros a nuestras consultas para seleccionar sólo aquellas filas que cumplen una determinada condición. Estas condiciones se denominan predicados y el resultado de estas condiciones puede ser verdadero, falso o desconocido.

Una condición tendrá un resultado desconocido cuando alguno de los valores utilizados tiene el valor `NULL`.

Podemos diferenciar cinco tipos de condiciones que pueden aparecer en la cláusula `WHERE`:

* Condiciones para comparar valores o expresiones.
* Condiciones para comprobar si un valor está dentro de un rango de valores.
* Condiciones para comprobar si un valor está dentro de un conjunto de valores.
* Condiciones para comparar cadenas con patrones.
* Condiciones para comprobar si una columna tiene valores a `NULL`.

Los operandos usados en las condiciones pueden ser nombres de columnas, constantes o expresiones. Los operadores que podemos usar en las condiciones pueden ser aritméticos, de comparación, lógicos, etc.

#### Operadores disponibles en MySQL

A continuación se muestran los operadores más utilizados en MySQL para realizar las consultas.

Operadores aritméticos:

| Operador | Descripción    |
| -------- | -------------- |
| +        | Suma           |
| -        | Resta          |
| *        | Multiplicación |
| /        | División       |
| %        | Módulo         |

Operadores de comparación:

| Operador | Descripción      |
| -------- | ---------------- |
| <        | Menor que        |
| <=       | Menor o igual    |
| >        | Mayor que        |
| >=       | Mayor o igual    |
| <>       | Distinto         |
| !=       | Distinto         |
| =        | Igual que        |

Operadores lógicos:

| Operador | Descripción       |
| -------- | ----------------- |
| AND      | Y lógica          |
| &&       | Y lógica          |
| OR       | O lógica          |
| ||       | O lógica          |
| NOT      | Negación lógica   |
| !        | Negación lógica   |


Vamos a continuar con el ejemplo de la tabla alumno que almacena la información de los alumnos matriculados en un determinado curso.


Cómo obtener el nombre de todos los alumnos que su primer apellido sea Martínez:

```
SELECT nombre
FROM alumno
WHERE apellido1 = 'Martínez';

+--------+
| nombre |
+--------+
| Paco   |
+--------+
1 row in set (0,00 sec)
```

Cómo obtener todos los datos del alumno que tiene un id igual a 9:

```sql
SELECT *
FROM alumno
WHERE id = 9;

+----+--------+------------+------------+------------------+--------------+-----------+
| id | nombre | apellido1  | apellido2  | fecha_nacimiento | es_repetidor | teléfono  |
+----+--------+------------+------------+------------------+--------------+-----------+
|  9 | Manuel | Domínguez  | Hernández  | 1999-07-08       | no           | NULL      |
+----+--------+------------+------------+------------------+--------------+-----------+
1 row in set (0,00 sec)
```

Obtener el nombre y la fecha de nacimiento de todos los alumnos nacieron después del 1 de enero de 1997:

```sql
SELECT nombre, fecha_nacimiento
FROM alumno
WHERE fecha_nacimiento >= '1997/01/01';

+--------+------------------+
| nombre | fecha_nacimiento |
+--------+------------------+
| Juan   | 1998-04-02       |
| Manuel | 1999-07-08       |
| Daniel | 1998-02-03       |
+--------+------------------+
3 rows in set, 1 warning (0,00 sec)
```

__Ejercicios__: realice las siguientes consultas teniendo en cuenta la base de datos instituto.

1. Devuelve los datos del alumno cuyo id es igual a 1.

2. Devuelve los datos del alumno cuyo teléfono es igual a 692735409.

3. Devuelve un listado de todos los alumnos que son repetidores.

4. Devuelve un listado de todos los alumnos que no son repetidores.

5. Devuelve el listado de los alumnos que han nacido antes del 1 de enero de 1993.

6. Devuelve el listado de los alumnos que han nacido después del 1 de enero de 1994.

7. Devuelve el listado de los alumnos que han nacido después del 1 de enero de 1994 y no son repetidores.

8. Devuelve el listado de todos los alumnos que nacieron en 1998.

9. Devuelve el listado de todos los alumnos que no nacieron en 1998.

#### Operador BETWEEN

Sintaxis:

```sql
expresión [NOT] BETWEEN cota_inferior AND cota_superior
```

Se utiliza para comprobar si un valor está dentro de un rango de valores. Por ejemplo, si queremos obtener los pedidos que se han realizado durante el mes de enero de 2018 podemos realizar la siguiente consulta:

```sql
SELECT *
FROM pedido
WHERE fecha_pedido BETWEEN '2018-01-01' AND '2018-01-31';
```

__Ejercicios__: realice las siguientes consultas teniendo en cuenta la base de datos instituto:

1. Devuelve los datos de los alumnos que hayan nacido entre el 1 de enero de 1998 y el 31 de mayo de 1998.

2. Devuelve los datos de los alumnos que no hayan nacido entre el 1 de enero de 1998 y el 31 de mayo de 1998.

#### Operador IN

Este operador nos permite comprobar si el valor de una determinada columna está incluido en una lista de valores.

Cómo obtener todos los datos de los alumnos que tengan como primer apellido Sánchez, Martínez o Domínguez.

```sql
SELECT *
FROM alumno
WHERE apellido1 IN ("Sánchez", "Martínez", "Domínguez");

+----+--------+------------+------------+------------------+--------------+-----------+
| id | nombre | apellido1  | apellido2  | fecha_nacimiento | es_repetidor | teléfono  |
+----+--------+------------+------------+------------------+--------------+-----------+
|  1 | María  | Sánchez    | Pérez      | 1990-12-01       | no           | NULL      |
|  4 | Lucía  | Sánchez    | Ortega     | 1993-06-13       | sí           | 678516294 |
|  5 | Paco   | Martínez   | López      | 1995-11-24       | no           | 692735409 |
|  9 | Manuel | Domínguez  | Hernández  | 1999-07-08       | no           | NULL      |
+----+--------+------------+------------+------------------+--------------+-----------+
4 rows in set (0,00 sec)
```

Cómo obtener todos los datos de los alumnos que no tengan como primer apellido Sánchez, Martínez o Domínguez.

```sql
SELECT *
FROM alumno
WHERE apellido1 NOT IN ("Sánchez", "Martínez", "Domínguez");

+----+----------+------------+-----------+------------------+--------------+-----------+
| id | nombre   | apellido1  | apellido2 | fecha_nacimiento | es_repetidor | teléfono  |
+----+----------+------------+-----------+------------------+--------------+-----------+
|  2 | Juan     | Sáez       | Vega      | 1998-04-02       | no           | 618253876 |
|  3 | Pepe     | Ramírez    | Gea       | 1988-01-03       | no           | NULL      |
|  6 | Irene    | Gutiérrez  | Sánchez   | 1991-03-28       | sí           | NULL      |
|  7 | Cristina | Fernández  | Ramírez   | 1996-09-17       | no           | 628349590 |
|  8 | Antonio  | Carretero  | Ortega    | 1994-05-20       | sí           | 612345633 |
| 10 | Daniel   | Moreno     | Ruiz      | 1998-02-03       | no           | NULL      |
+----+----------+------------+-----------+------------------+--------------+-----------+
```

#### Operador LIKE

Sintaxis:

```sql
columna [NOT] LIKE patrón
```

Se utiliza para comparar si una cadena de caracteres coincide con un patrón. En el patrón podemos utilizar cualquier carácter alfanumérico, pero hay dos caracteres que tienen un significado especial, el símbolo del porcentaje `%` y el carácter de subrayado `_`.

* `%`: este carácter equivale a cualquier conjunto de caracteres.
* `_`: este carácter equivale a cualquier carácter.

Cómo obtener un listado de todos los alumnos que su primer apellido empiece por la letra `S`:

```sql
SELECT *
FROM alumno
WHERE apellido1 LIKE 'S%';

+----+--------+-----------+-----------+------------------+--------------+-----------+
| id | nombre | apellido1 | apellido2 | fecha_nacimiento | es_repetidor | teléfono  |
+----+--------+-----------+-----------+------------------+--------------+-----------+
|  1 | María  | Sánchez   | Pérez     | 1990-12-01       | no           | NULL      |
|  2 | Juan   | Sáez      | Vega      | 1998-04-02       | no           | 618253876 |
|  4 | Lucía  | Sánchez   | Ortega    | 1993-06-13       | sí           | 678516294 |
+----+--------+-----------+-----------+------------------+--------------+-----------+
3 rows in set (0,01 sec)
```

Cómo obtener un listado de todos los alumnos que su primer apellido termine por la letra `z`:

```sql
SELECT *
FROM alumno
WHERE apellido1 LIKE '%z';

+----+----------+------------+------------+------------------+--------------+-----------+
| id | nombre   | apellido1  | apellido2  | fecha_nacimiento | es_repetidor | teléfono  |
+----+----------+------------+------------+------------------+--------------+-----------+
|  1 | María    | Sánchez    | Pérez      | 1990-12-01       | no           | NULL      |
|  2 | Juan     | Sáez       | Vega       | 1998-04-02       | no           | 618253876 |
|  3 | Pepe     | Ramírez    | Gea        | 1988-01-03       | no           | NULL      |
|  4 | Lucía    | Sánchez    | Ortega     | 1993-06-13       | sí           | 678516294 |
|  5 | Paco     | Martínez   | López      | 1995-11-24       | no           | 692735409 |
|  6 | Irene    | Gutiérrez  | Sánchez    | 1991-03-28       | sí           | NULL      |
|  7 | Cristina | Fernández  | Ramírez    | 1996-09-17       | no           | 628349590 |
|  9 | Manuel   | Domínguez  | Hernández  | 1999-07-08       | no           | NULL      |
+----+----------+------------+------------+------------------+--------------+-----------+
8 rows in set (0,00 sec)
```

Cómo obtener un listado de todos los alumnos que su nombre tenga el carácter `a`:

```sql
SELECT *
FROM alumno
WHERE nombre LIKE '%a%';

+----+----------+------------+------------+------------------+--------------+-----------+
| id | nombre   | apellido1  | apellido2  | fecha_nacimiento | es_repetidor | teléfono  |
+----+----------+------------+------------+------------------+--------------+-----------+
|  1 | María    | Sánchez    | Pérez      | 1990-12-01       | no           | NULL      |
|  2 | Juan     | Sáez       | Vega       | 1998-04-02       | no           | 618253876 |
|  4 | Lucía    | Sánchez    | Ortega     | 1993-06-13       | sí           | 678516294 |
|  5 | Paco     | Martínez   | López      | 1995-11-24       | no           | 692735409 |
|  7 | Cristina | Fernández  | Ramírez    | 1996-09-17       | no           | 628349590 |
|  8 | Antonio  | Carretero  | Ortega     | 1994-05-20       | sí           | 612345633 |
|  9 | Manuel   | Domínguez  | Hernández  | 1999-07-08       | no           | NULL      |
| 10 | Daniel   | Moreno     | Ruiz       | 1998-02-03       | no           | NULL      |
+----+----------+------------+------------+------------------+--------------+-----------+
8 rows in set (0,00 sec)
```

Cómo obtener un listado de todos los alumnos que tengan un nombre de cuatro caracteres.

```sql
SELECT *
FROM alumno
WHERE nombre LIKE '____';
```

#### Expresiones regulares utilizando los operadores REGEXP y RLIKE

Los operadores `REGEXP` y `RLIKE` son equivalentes, los dos nos permiten realizar búsquedas mucho más potentes haciendo uso de expresiones regulares.

Una expresión regular es una forma de representar un patrón de búsqueda más complejo.

Algunos de los patrones que podemos utilizar en las expresiones regulares son los siguientes.

| Patrón   | Descripción                                     | Ejemplo                                   |
| -------- | ------------------------------------------------| ----------------------------------------- |
| .        | Equivale a cualquier carácter                   | a.b coincide con: “acb”, “a3b”           |
| ^        | Inicio de línea                                 | ^Hola coincide con: “Hola mundo”         |
| $        | Fin de línea                                    | mundo$ coincide con: “Hola mundo”        |
| *        | Cero o más repeticiones del carácter anterior   | ab*c coincide con: “ac”, “abc”, “abbc”   |
| +        | Una o más repeticiones del carácter anterior    | ab+c coincide con: “abc”, “abbc”, pero no “ac” |
| ?        | Cero o una repetición del carácter anterior     | ab?c coincide con: “ac” o “abc”          |
| {n}      | Exactamente n repeticiones                      | a{3} coincide con: “aaa”                 |
| {n,}     | Al menos n repeticiones                         | a{2,} coincide con: “aa”, “aaa”, “aaaa”  |
| {n,m}    | Entre n y m repeticiones                        | a{2,4} coincide con: “aa”, “aaa”, “aaaa”|
| [abc]    | Cualquier carácter dentro de los corchetes      | [aeiou] coincide con cualquier vocal     |
| [^abc]   | Cualquier carácter excepto los especificados    | [^0-9] excluye dígitos                   |
| [a-z]    | Rango de caracteres                             | [A-Z] coincide con cualquier letra mayúscula |
| |        | Equivale al operador OR                         | abc|def coincide con: “abc” o “def”      |

Cómo obtener un listado de todos los alumnos que su primer apellido empiece por la letra S.

```sql
SELECT *
FROM alumno
WHERE apellido1 REGEXP '^S';

+----+--------+-----------+-----------+------------------+--------------+-----------+
| id | nombre | apellido1 | apellido2 | fecha_nacimiento | es_repetidor | teléfono  |
+----+--------+-----------+-----------+------------------+--------------+-----------+
|  1 | María  | Sánchez   | Pérez     | 1990-12-01       | no           | NULL      |
|  2 | Juan   | Sáez      | Vega      | 1998-04-02       | no           | 618253876 |
|  4 | Lucía  | Sánchez   | Ortega    | 1993-06-13       | sí           | 678516294 |
+----+--------+-----------+-----------+------------------+--------------+-----------+
3 rows in set (0,02 sec)
```

Cómo obtener un listado de todos los alumnos que su primer apellido termine por la letra z.

```sql
SELECT *
FROM alumno
WHERE apellido1 REGEXP 'z$';

+----+----------+------------+------------+------------------+--------------+-----------+
| id | nombre   | apellido1  | apellido2  | fecha_nacimiento | es_repetidor | teléfono  |
+----+----------+------------+------------+------------------+--------------+-----------+
|  1 | María    | Sánchez    | Pérez      | 1990-12-01       | no           | NULL      |
|  2 | Juan     | Sáez       | Vega       | 1998-04-02       | no           | 618253876 |
|  3 | Pepe     | Ramírez    | Gea        | 1988-01-03       | no           | NULL      |
|  4 | Lucía    | Sánchez    | Ortega     | 1993-06-13       | sí           | 678516294 |
|  5 | Paco     | Martínez   | López      | 1995-11-24       | no           | 692735409 |
|  6 | Irene    | Gutiérrez  | Sánchez    | 1991-03-28       | sí           | NULL      |
|  7 | Cristina | Fernández  | Ramírez    | 1996-09-17       | no           | 628349590 |
|  9 | Manuel   | Domínguez  | Hernández  | 1999-07-08       | no           | NULL      |
+----+----------+------------+------------+------------------+--------------+-----------+
8 rows in set (0,00 sec)
```

Cómo obtener un listado de todos los alumnos que tienen una dirección de email válida, que se corresponde con el siguiente patrón.

```sql
SELECT * 
FROM alumno 
WHERE email REGEXP '^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$';
```

#### Operadores IS e IS NOT

Estos operadores nos permiten comprobar si el valor de una determinada columna es `NULL` o no lo es.

Cómo obtener la lista de alumnos que tienen un valor NULL en la columna teléfono:

```sql
SELECT *
FROM alumno
WHERE teléfono IS NULL;

+----+--------+------------+------------+------------------+--------------+-----------+
| id | nombre | apellido1  | apellido2  | fecha_nacimiento | es_repetidor | teléfono  |
+----+--------+------------+------------+------------------+--------------+-----------+
|  1 | María  | Sánchez    | Pérez      | 1990-12-01       | no           | NULL      |
|  3 | Pepe   | Ramírez    | Gea        | 1988-01-03       | no           | NULL      |
|  6 | Irene  | Gutiérrez  | Sánchez    | 1991-03-28       | sí           | NULL      |
|  9 | Manuel | Domínguez  | Hernández  | 1999-07-08       | no           | NULL      |
| 10 | Daniel | Moreno     | Ruiz       | 1998-02-03       | no           | NULL      |
+----+--------+------------+------------+------------------+--------------+-----------+
5 rows in set (0,00 sec)
```

Cómo obtener la lista de alumnos que no tienen un valor NULL en la columna teléfono:

```sql
SELECT *
FROM alumno
WHERE teléfono IS NOT NULL;

+----+----------+------------+-----------+------------------+--------------+-----------+
| id | nombre   | apellido1  | apellido2 | fecha_nacimiento | es_repetidor | teléfono  |
+----+----------+------------+-----------+------------------+--------------+-----------+
|  2 | Juan     | Sáez       | Vega      | 1998-04-02       | no           | 618253876 |
|  4 | Lucía    | Sánchez    | Ortega    | 1993-06-13       | sí           | 678516294 |
|  5 | Paco     | Martínez   | López     | 1995-11-24       | no           | 692735409 |
|  7 | Cristina | Fernández  | Ramírez   | 1996-09-17       | no           | 628349590 |
|  8 | Antonio  | Carretero  | Ortega    | 1994-05-20       | sí           | 612345633 |
+----+----------+------------+-----------+------------------+--------------+-----------+
5 rows in set (0,00 sec)
```

### Funciones disponibles en MySQL

A continuación se muestran algunas funciones disponibles en MySQL que pueden ser utilizadas para realizar consultas.

Las funciones se pueden utilizar en las cláusulas `SELECT`, `WHERE` y `ORDER BY`.

#### Funciones con cadenas

| Función     | Descripción                                              |
| ----------- | -------------------------------------------------------- |
| CONCAT      | Concatena cadenas                                        |
| CONCAT_WS   | Concatena cadenas con un separador                       |
| LOWER       | Devuelve una cadena en minúscula                         |
| UPPER       | Devuelve una cadena en mayúscula                         |
| SUBSTR      | Devuelve una subcadena                                   |
| LEFT        | Devuelve los caracteres de una cadena, empezando por la izquierda |
| RIGHT       | Devuelve los caracteres de una cadena, empezando por la derecha |
| CHAR_LENGTH | Devuelve el número de caracteres que tiene una cadena    |
| LENGTH      | Devuelve el número de bytes que ocupa una cadena         |
| REVERSE     | Devuelve una cadena invirtiendo el orden de sus caracteres |
| LTRIM       | Elimina los espacios en blanco que existan al inicio de una cadena |
| RTRIM       | Elimina los espacios en blanco que existan al final de una cadena |
| TRIM        | Elimina los espacios en blanco que existan al inicio y al final de una cadena |
| REPLACE     | Permite reemplazar un carácter dentro de una cadena     |

Ejercicios:

1. Devuelve un listado con dos columnas, donde aparezca en la primera columna el nombre de los alumnos y en la segunda, el nombre con todos los caracteres invertidos.

2. Devuelve un listado con dos columnas, donde aparezca en la primera columna el nombre y los apellidos de los alumnos y en la segunda, el nombre y los apellidos con todos los caracteres invertidos.

3. Devuelve un listado con dos columnas, donde aparezca en la primera columna el nombre y los apellidos de los alumnos en mayúscula y en la segunda, el nombre y los apellidos con todos los caracteres invertidos en minúscula.

4. Devuelve un listado con tres columnas, donde aparezca en la primera columna el nombre y los apellidos de los alumnos, en la segunda, el número de caracteres que tiene en total el nombre y los apellidos y en la tercera el número de bytes que ocupa en total.

5. Devuelve un listado con dos columnas, donde aparezca en la primera columna el nombre y los dos apellidos de los alumnos. En la segunda columna se mostrará una dirección de correo electrónico que vamos a calcular para cada alumno. La dirección de correo estará formada por el nombre y el primer apellido, separados por el carácter . y seguidos por el dominio @iescelia.org. Tenga en cuenta que la dirección de correo electrónico debe estar en minúscula. Utilice un alias apropiado para cada columna.

6. Devuelve un listado con tres columnas, donde aparezca en la primera columna el nombre y los dos apellidos de los alumnos. En la segunda columna se mostrará una dirección de correo electrónico que vamos a calcular para cada alumno. La dirección de correo estará formada por el nombre y el primer apellido, separados por el carácter . y seguidos por el dominio @iescelia.org. Tenga en cuenta que la dirección de correo electrónico debe estar en minúscula. La tercera columna será una contraseña que vamos a generar formada por los caracteres invertidos del segundo apellido, seguidos de los cuatro caracteres del año de la fecha de nacimiento. Utilice un alias apropiado para cada columna.

#### Funciones de fecha y hora

| Función     | Descripción                                       |
| ----------- | ------------------------------------------------- |
| NOW()       | Devuelve la fecha y la hora actual               |
| CURTIME()   | Devuelve la hora actual                           |
| ADDDATE     | Suma un número de días a una fecha y calcula la nueva fecha |
| DATE_FORMAT | Nos permite formatear fechas                     |
| DATEDIFF    | Calcula el número de días que hay entre dos fechas |
| YEAR        | Devuelve el año de una fecha                     |
| MONTH       | Devuelve el mes de una fecha                     |
| MONTHNAME   | Devuelve el nombre del mes de una fecha          |
| DAY         | Devuelve el día de una fecha                     |
| DAYNAME     | Devuelve el nombre del día de una fecha          |
| HOUR        | Devuelve las horas de un valor de tipo DATETIME  |
| MINUTE      | Devuelve los minutos de un valor de tipo DATETIME|
| SECOND      | Devuelve los segundos de un valor de tipo DATETIME |

Configuración regional en MySQL Server (locale)

> __Nota__: tenga en cuenta que para que los nombres de los meses y las abreviaciones aparezcan en español deberá configurar la variable del sistema `lc_time_names`. Esta variable afecta al resultado de las funciones `DATE_FORMAT`, `DAYNAME` y `MONTHNAME`.

En MySQL las variables se pueden definir como variables globales o variables de sesión. La diferencia que existe entre ellas es que una variable de sesión pierde su contenido cuando cerramos la sesión con el servidor, mientras que una variable global mantiene su valor hasta que se realiza un reinicio del servicio o se modifica por otro valor. Las variables globales sólo pueden ser configuradas por usuarios con privilegios de administración.

Para configurar la variable lc_time_names como una variable global, con la configuración regional de España tendrá que utilizar la palabra reservada `GLOBAL`, como se indica en el siguiente ejemplo.

```sql
SET GLOBAL lc_time_names = 'es_ES';
```

Para realizar la configuración como una variable de sesión tendría que ejecutar:

```sql
SET lc_time_names = 'es_ES';
```

Una vez hecho esto podrá consultar sus valores haciendo:

```sql
SELECT @@GLOBAL.lc_time_names, @@SESSION.lc_time_names;
```

Para consultar el valor de todas las variables de sesión del sistema podemos utilizar la sentencia:

```sql
SHOW VARIABLES;
```

O con el modificador `SESSION`:

```sql
SHOW SESSION VARIABLES;
```

Para consultar el valor de todas las variables globales del sistema podemos utilizar la sentencia:

```sql
SHOW GLOBAL VARIABLES;
```

__Configuración de la zona horaria en MySQL Server (time zone)__

> __Nota__: también será necesario configurar nuestra zona horaria para que las funciones relacionadas con la hora devuelvan los valores de nuestra zona horaria. En este caso tendrá que configurar la variable del sistema time_zone, como variable global o como variable de sesión. A continuación, se muestra un ejemplo de cómo habría que configurar las variables para la zona horaria de Madrid.

```sql
SET GLOBAL time_zone = 'Europe/Madrid';
SET time_zone = 'Europe/Madrid';
```

Podemos comprobar el estado de las variables haciendo:

```sql
SELECT @@GLOBAL.time_zone, @@SESSION.time_zone;
```

Cómo obtener la fecha y hora actual:

```sql
SELECT NOW();
```

Cómo obtener la hora actual:

```sql
SELECT CURTIME();
```

Ejercicios:

1. Devuelva un listado con cuatro columnas, donde aparezca en la primera columna la fecha de nacimiento completa de los alumnos, en la segunda columna el día, en la tercera el mes y en la cuarta el año. Utilice las funciones DAY, MONTH y YEAR.
2. Devuelva un listado con tres columnas, donde aparezca en la primera columna la fecha de nacimiento de los alumnos, en la segunda el nombre del día de la semana de la fecha de nacimiento y en la tercera el nombre del mes de la fecha de nacimiento.
    * Resuelva la consulta utilizando las funciones DAYNAME y MONTHNAME.
    * Resuelva la consulta utilizando la función DATE_FORMAT.
3. Devuelva un listado con dos columnas, donde aparezca en la primera columna la fecha de nacimiento de los alumnos y en la segunda columna el número de días que han pasado desde la fecha actual hasta la fecha de nacimiento. Utilice las funciones DATEDIFF y NOW. Consulte la documentación oficial de MySQL para DATEDIFF.
4. Devuelva un listado con dos columnas, donde aparezca en la primera columna la fecha de nacimiento de los alumnos y en la segunda columna la edad de cada alumno/a. La edad (aproximada) la podemos calcular realizando las siguientes operaciones:
5. Calcula el número de días que han pasado desde la fecha actual hasta la fecha de nacimiento. Utilice las funciones DATEDIFF y NOW.
6. Divida entre 365.25 el resultado que ha obtenido en el paso anterior. (El 0.25 es para compensar los años bisiestos que han podido existir entre la fecha de nacimiento y la fecha actual).
7. Trunca las cifras decimales del número obtenido.

#### Funciones matemáticas

| Función   | Descripción                                         |
| --------- | --------------------------------------------------- |
| ABS()     | Devuelve el valor absoluto                          |
| POW(x,y)  | Devuelve el valor de x elevado a y                  |
| SQRT      | Devuelve la raíz cuadrada                           |
| PI()      | Devuelve el valor del número PI                    |
| ROUND     | Redondea un valor numérico                          |
| TRUNCATE  | Trunca un valor numérico                            |
| CEIL      | Devuelve el entero inmediatamente superior o igual |
| FLOOR     | Devuelve el entero inmediatamente inferior o igual |
| MOD       | Devuelve el resto de una división                  |

Cómo obtener el el valor absoluto de un número:

```sql
SELECT ABS(-25);

25
```

Cómo obtener el valor de `x` elevado a `y`:

```sql
SELECT POW(2, 10);

1024
```

Cómo obtener la raíz cuadrada de un número:

```sql
SELECT SQRT(1024);

32
```

Cómo obtener el valor del número PI:

```sql
SELECT PI();

3.141593
```

Cómo obtener el redondeo de un valor numérico:

```sql
SELECT ROUND(37.6234);

38
```

Cómo truncar un valor numérico:

```sql
SELECT TRUNCATE(37.6234, 0);

37
```

Cómo obtener el entero inmediatamente superior o igual al parámetro de entrada:

```sql
SELECT CEIL(4.3);

5
```

Cómo obtener  el entero inmediatamente inferior o igual al parámetro de entrada:

```sql
SELECT FLOOR(4.3);

4
```

#### Funciones de control de flujo

| Función  | Descripción                                             |
| -------- | ------------------------------------------------------- |
| CASE     | Operador para realizar condicionales múltiples         |
| IF()     | Permite realizar condiciones de tipo IF/ELSE            |
| IFNULL() | Devuelve el primer argumento si no es NULL, en caso contrario devuelve el segundo argumento |
| NULLIF() | Devuelve NULL si los dos argumentos son iguales, en caso contrario devuelve el primer argumento |

Ejemplo del operador `CASE`:

Existen dos formas de utilizar el operador CASE:

Sintaxis 1:

```sql
CASE value 
  WHEN compare_value THEN result 
  [WHEN compare_value THEN result ...] 
  [ELSE result] 
END
```

En este caso, junto al operador CASE se utiliza un valor, el nombre de una columna o una expresión y se compara con una serie de valores. Cuando se encuentra la primera coincidencia, se devuelve el resultado asociado y no se siguen evaluando el resto de comapraciones.

```sql
SELECT 
  nombre, apellido1, apellido2,
  CASE es_repetidor
    WHEN 'sí' THEN 'Repite'
    WHEN 'no' THEN 'No repite'
  END
FROM alumno;
```

Sintaxis 2:

```sql
CASE 
  WHEN condition THEN result 
  [WHEN condition THEN result ...] 
  [ELSE result] 
END
```

En este caso se utiliza una condición en cada comparación y cuando se encuentra la primera condición que sea cierta, se devuelve el resultado asociado y no se siguen evaluando el resto de condiciones.

```sql
SELECT
  producto, cantidad,
  CASE
    WHEN cantidad > 100 THEN 'Stock suficiente'
    WHEN cantidad > 50 THEN 'Stock moderado'
    ELSE 'Stock bajo'
  END AS estado
FROM
  producto;
```

Ejemplo de la función `IF`:

La sintaxis de la función `IF` es la siguiente:

```
IF(expr1,expr2,expr3)
```

Si la expresión expr1 es `TRUE` entonces duvuelve el valor de expr2, en caso contrario devuelve el valor de expr3.

```sql
SELECT 
  nombre, apellido1, apellido2, IF(es_repetidor = 'sí', 'Repite', 'No repite')
FROM alumno;
```

Ejemplo de la función `IFNULL`:

La sintaxis de la función `IFNULL` es la siguiente:

```sql
IFNULL(expr1,expr2)
```

Si la expresión expr1 no es `NULL` entonces duvuelve el valor de expr1, en caso contrario devuelve el valor de expr2.

```sql
SELECT 
  nombre, apellido1, apellido2, IFNULL(telefono, ' Teléfono no disponible')
FROM alumno;
```

Ejemplo de la función `NULLIF`:

La sintaxis de la función `NULLIF` es la siguiente:

```sql
NULLIF(expr1,expr2)
```

Esta función devuelve `NULL` si expr1 es igual a expr2, en caso contrario devuelve el valor de expr1.

## Errores comunes

### Error al comprobar si una columna es NULL

Cómo obtener la lista de alumnos que tienen un valor NULL en la columna teléfono.

```sql
-- Consulta incorrecta.
SELECT *
FROM alumno
WHERE teléfono = NULL;

-- Consulta correcta.
SELECT *
FROM alumno
WHERE teléfono IS NULL;
```

### Error al comparar cadenas con patrones utilizando el operador =

Cómo obtener un listado de los alumnos cuyo primer apellido empieza por `S`.

```sql
-- Consulta incorrecta.
SELECT *
FROM alumno
WHERE apellido1 = 'S%';

-- Consulta correcta.
SELECT *
FROM alumno
WHERE apellido1 LIKE 'S%';
```

### Error al comparar cadenas con patrones utilizando el operador !=

Cómo obtener un listado de los alumnos cuyo primer apellido no empieza por `S`.

```sql
-- Consulta incorrecta.
SELECT *
FROM alumno
WHERE apellido1 != 'S%';

-- Consulta correcta.
SELECT *
FROM alumno
WHERE apellido1 NOT LIKE 'S%';
```

### Error al comparar un rango de valores con AND

Cuando queremos comparar si un valor está dentro de un rango tenemos que utilizar dos condiciones unidas con la operación lógica AND. En el siguiente ejemplo se muestra una consulta incorrecta donde la segunda condición siempre será verdadera porque no estamos comparando con ningún valor, estamos poniendo un valor constante que al ser distinto de 0 siempre nos dará un valor verdadero como resultado de la comparación.

```sql
-- Consulta incorrecta
SELECT *
FROM alumno
WHERE fecha_nacimiento >= '1999/01/01' AND '1999/12/31'

-- Consulta correcta
SELECT *
FROM alumno
WHERE fecha_nacimiento >= '1999/01/01' AND fecha_nacimiento <= '1999/12/31'
```

### Errores de conversión de tipos en la evaluación de expresiones

Cuando se utiliza un operador con operandos de diferentes tipos de dato, MySQL realiza una conversión automática de tipo para que los operandos sean compatibles. Por ejemplo, convierte automáticamente cadenas en números según sea necesario y viceversa:

```sql
SELECT 1 + '1';
 -> 2

SELECT CONCAT(1, '1');
 -> '11'
```

Las cadenas las convierte a datos de tipo entero de la mejor forma posible y luego realiza la resta. En este caso la cadena '2021-01-31' la convierte en el número entero 2021 y la cadena '2021-02-01' en el número entero 2021, por este motivo el resultado de la resta es 0:

```sql
SELECT '2021-01-31' - '2021-02-01';
 -> 0
```

En este ejemplo estamos convirtiendo las cadenas a un valor de tipo DATE y luego se realiza la resta. En este caso, los datos de tipo DATE los convierte a datos de tipo entero pero de una forma más precisa que la conversión anterior, ya que la cadena '2008-08-31' la convierte en el número entero 20080831 y la cadena '2008-09-01' en el número entero 20080901, por este motivo el resultado de la resta es -70.

```sql
SELECT CAST('2021-01-31' AS DATE) - CAST('2021-02-01' AS DATE);
 -> -70
```

En este ejemplo resta entre dos valores de tipo INT

```sql
SELECT 20210131 - 20210201;
 -> -70
```

### Error al utilizar los operadores de suma y resta entre datos de tipo DATE, DATETIME y TIMESTAMP

Para poder realizar operaciones de suma y resta entre datos de tipo `DATE`, `DATETIME` y `TIMESTAMP` es necesario hacer uso de las funciones específicas de fecha y hora disponibles en MySQL. Si tratamos de realizar una operación de suma o resta entre valores de tipo `DATE`, `DATETIME` y `TIMESTAMP` podemos obtener un resultado incorrecto en algunas situaciones, ya que estos valores se convierten a un dato de tipo numérico y posteriormente se realiza la operación entre ellos.

```sql
-- Consulta incorrecta
SELECT fecha_esperada, fecha_entrega,  fecha_entrega - fecha_esperada
FROM pedido;

fecha_esperada	fecha_entrega	fecha_entrega - fecha_esperada
2021-01-31	 2021-02-01	-70
```

En este ejemplo estamos realizando una resta entre dos datos de tipo DATE utilizando el operador `-`. Lo que ocurre en este caso, es que antes de realizar la operación de resta, MySQL convierte los datos de tipo `DATE` a datos de tipo entero. De forma que la fecha 2008-08-31 la convierte en el número entero 20080831 y la fecha 2008-09-01 en el número entero 20080901. Por este motivo el resultado que obtenemos a realizar la resta es -70.

```sql
--Consulta correcta
SELECT fecha_esperada, fecha_entrega,  DATEDIFF(fecha_entrega, fecha_esperada)
FROM pedido;

fecha_esperada	fecha_entrega	DATEDIFF(fecha_entrega, fecha_esperada)
2021-01-31	 2021-02-01	-1
```

En este caso se está utilizando la función `DATEDIFF` en lugar del operador de resta `-`p. Al ser una función específica para trabajar con este tipo de dato obtenemos el resultado esperado.

[01]: ../img/ut05/consultas-sql-una-tabla/01.png "01"
[02]: ../img/ut05/consultas-sql-una-tabla/02.png "02"
[03]: ../img/ut05/consultas-sql-una-tabla/03.png "03"
