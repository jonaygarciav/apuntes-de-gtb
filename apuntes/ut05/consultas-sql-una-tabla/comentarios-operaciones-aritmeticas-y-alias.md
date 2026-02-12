# Comentarios, Operaciones Aritméticas y Alias

* [Introducción](#introducción)
* [Sintaxis de la instrucción SELECT](#sintaxis-de-la-instrucción-select)
* [Cláusula SELECT](#cláusula-select)
  * [Cómo obtener los datos de todas las columnas de una tabla](#cómo-obtener-los-datos-de-todas-las-columnas-de-una-tabla)
  * [Cómo obtener los datos de algunas columnas de una tabla](#cómo-obtener-los-datos-de-algunas-columnas-de-una-tabla)
* [Comentarios](#comentarios)
  * [Cómo realizar comentarios en sentencias SQL](#cómo-realizar-comentarios-en-sentencias-sql)
* [Realizar operaciones aritméticas](#realizar-operaciones-aritméticas)
  * [Cómo realizar alias de columnas con AS](#cómo-realizar-alias-de-columnas-con-as)

## Introducción

__DML__ (_Data Manipulation Language_ o _Lenguaje de Manipulación de Datos_ es la parte de SQL dedicada a la manipulación de los datos. Las sentencias DML son las siguientes:

* `SELECT`: se utiliza para realizar consultas y extraer información de la base de datos.
* `INSERT`: se utiliza para insertar registros en las tablas de la base de datos.
* `UPDATE`: se utiliza para actualizar los registros de una tabla.
* `DELETE`: se utiliza para eliminar registros de una tabla.

![][01]

En este tema nos vamos a centrar en el uso de la sentencia `SELECT`.

## Sintaxis de la instrucción SELECT

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

![][02]

Hay que tener en cuenta que el resultado de una consulta siempre será una tabla de datos, que puede tener una o varias columnas y ninguna, una o varias filas.

El hecho de que el resultado de una consulta sea una tabla es muy interesante porque nos permite realizar cosas como almacenar los resultados como una nueva en la base de datos. También será posible combinar el resultado de dos o más consultas para crear una tabla mayor con la unión de los dos resultados. Además, los resultados de una consulta también pueden consultados por otras nuevas consultas.

## Cláusula SELECT

Nos permite indicar cuáles serán las columnas que tendrá la tabla de resultados de la consulta que estamos realizando. Las opciones que podemos indicar son las siguientes:

* El __nombre de una columna__ de la tabla sobre la que estamos realizando la consulta. Será una columna de la tabla que aparece en la cláusula `FROM`.
* Una __constante__ que aparecerá en todas las filas de la tabla resultado.
* Una __expresión__ que nos permite calcular nuevos valores.

### Cómo obtener los datos de todas las columnas de una tabla

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
  es_repetidor BOOLEAN NOT NULL,
  telefono VARCHAR(9)
);

INSERT INTO alumno (nombre, apellido1, apellido2, fecha_nacimiento, es_repetidor, telefono) VALUES ('María', 'Sánchez', 'Pérez', '1990-12-01', FALSE, NULL);
INSERT INTO alumno (nombre, apellido1, apellido2, fecha_nacimiento, es_repetidor, telefono) VALUES ('Juan', 'Sáez', 'Vega', '1998-04-02', FALSE, '618253876');
INSERT INTO alumno (nombre, apellido1, apellido2, fecha_nacimiento, es_repetidor, telefono) VALUES ('Pepe', 'Ramírez', 'Gea', '1988-01-03', FALSE, NULL);
INSERT INTO alumno (nombre, apellido1, apellido2, fecha_nacimiento, es_repetidor, telefono) VALUES ('Lucía', 'Sánchez', 'Ortega', '1993-06-13', TRUE, '678516294');
INSERT INTO alumno (nombre, apellido1, apellido2, fecha_nacimiento, es_repetidor, telefono) VALUES ('Paco', 'Martínez', 'López', '1995-11-24', FALSE, '692735409');
INSERT INTO alumno (nombre, apellido1, apellido2, fecha_nacimiento, es_repetidor, telefono) VALUES ('Irene', 'Gutiérrez', 'Sánchez', '1991-03-28', TRUE, NULL);
INSERT INTO alumno (nombre, apellido1, apellido2, fecha_nacimiento, es_repetidor, telefono) VALUES ('Cristina', 'Fernández', 'Ramírez', '1996-09-17', FALSE, '628349590');
INSERT INTO alumno (nombre, apellido1, apellido2, fecha_nacimiento, es_repetidor, telefono) VALUES ('Antonio', 'Carretero', 'Ortega', '1994-05-20', TRUE, '612345633');
INSERT INTO alumno (nombre, apellido1, apellido2, fecha_nacimiento, es_repetidor, telefono) VALUES ('Manuel', 'Domínguez', 'Hernández', '1999-07-08', FALSE, NULL);
INSERT INTO alumno (nombre, apellido1, apellido2, fecha_nacimiento, es_repetidor, telefono) VALUES ('Daniel', 'Moreno', 'Ruiz', '1998-02-03', FALSE, NULL);
```

Supongamos que tenemos una tabla llamada __alumno__ con la siguiente información de los alumnos matriculados en un determinado curso:

| id  | nombre    | apellido1  | apellido2  | fecha_nacimiento | es_repetidor | telefono  |
|-----|-----------|------------|------------|------------------|--------------|-----------|
| 1   | María     | Sánchez    | Pérez      | 1990/12/01       | FALSE        | 618253876 |
| 2   | Juan      | Sáez       | Vega       | 1998/04/02       | FALSE        | 618253876 |
| 3   | Pepe      | Ramírez    | Gea        | 1988/01/03       | FALSE        | NULL      |
| 4   | Lucía     | Sánchez    | Ortega     | 1993/06/13       | TRUE         | 678516294 |
| 5   | Paco      | Martínez   | López      | 1995/11/24       | FALSE        | 692735409 |
| 6   | Irene     | Gutiérrez  | Sánchez    | 1991/03/23       | TRUE         | NULL      |
| 7   | Cristina  | Fernández  | Ramírez    | 1996/09/17       | FALSE        | 628349590 |
| 8   | Antonio   | Carretero  | Ortega     | 1994/05/20       | TRUE         | 612345633 |
| 9   | Manuel    | Domínguez  | Hernández  | 1997/08/07       | FALSE        | NULL      |
| 10  | Daniel    | Moreno     | Ruiz       | 1998/02/03       | FALSE        | NULL      |


Vamos a ver qué consultas sería necesario realizar para obtener los siguientes datos.

Cómo obtener  todos los datos de todos los alumnos matriculados en el curso:

```sql
SELECT *
FROM alumno;

+----+----------+------------+------------+------------------+--------------+-----------+
| id | nombre   | apellido1  | apellido2  | fecha_nacimiento | es_repetidor | teléfono  |
+----+----------+------------+------------+------------------+--------------+-----------+
|  1 | María    | Sánchez    | Pérez      | 1990-12-01       | FALSE        | NULL      |
|  2 | Juan     | Sáez       | Vega       | 1998-04-02       | FALSE        | 618253876 |
|  3 | Pepe     | Ramírez    | Gea        | 1988-01-03       | FALSE        | NULL      |
|  4 | Lucía    | Sánchez    | Ortega     | 1993-06-13       | TRUE         | 678516294 |
|  5 | Paco     | Martínez   | López      | 1995-11-24       | FALSE        | 692735409 |
|  6 | Irene    | Gutiérrez  | Sánchez    | 1991-03-28       | TRUE         | NULL      |
|  7 | Cristina | Fernández  | Ramírez    | 1996-09-17       | FALSE        | 628349590 |
|  8 | Antonio  | Carretero  | Ortega     | 1994-05-20       | TRUE         | 612345633 |
|  9 | Manuel   | Domínguez  | Hernández  | 1999-07-08       | FALSE        | NULL      |
| 10 | Daniel   | Moreno     | Ruiz       | 1998-02-03       | FALSE        | NULL      |
+----+----------+------------+------------+------------------+--------------+-----------+

10 rows in set (0,00 sec)
```

El carácter `*` es un comodín que utilizamos para indicar que queremos seleccionar todas las columnas de la tabla. La consulta anterior devolverá todos los datos de la tabla.

Otra consideración que también debemos tener en cuenta es que una consulta SQL se puede escribir en una o varias líneas. Por ejemplo, la siguiente sentencia tendría el mismo resultado que la anterior:

```sql
SELECT * FROM alumno;
```

### Cómo obtener los datos de algunas columnas de una tabla

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

## Comentarios

### Cómo realizar comentarios en sentencias SQL

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

## Realizar operaciones aritméticas

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

```

Supongamos que tenemos una tabla llamada oficinas que contiene información sobre las ventas reales (campo `ventas`) que ha generado y el valor de ventas esperado (campo `objetivo`), y nos gustaría conocer si las oficinas han conseguido el objetivo propuesto, y si están por debajo o por encima del valor de ventas esperado.

![][03]

```sql
DROP DATABASE IF EXISTS empresa;
CREATE DATABASE empresa CHARACTER SET utf8mb4;
USE empresa;

CREATE TABLE oficinas (
  oficina INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  ciudad VARCHAR(50) NOT NULL,
  region VARCHAR(50) NOT NULL,
  director INT UNSIGNED,
  objetivo DECIMAL(9,2) NOT NULL,
  ventas DECIMAL(9,2) NOT NULL
);

INSERT INTO oficinas (ciudad, region, director, objetivo, ventas) VALUES ('New York', 'Eastern', 106, 575000, 692637);
INSERT INTO oficinas (ciudad, region, director, objetivo, ventas) VALUES ('Chicago', 'Eastern', 104, 800000, 735042);
INSERT INTO oficinas (ciudad, region, director, objetivo, ventas) VALUES ('Atlanta', 'Eastern', NULL, 350000, 367911);
INSERT INTO oficinas (ciudad, region, director, objetivo, ventas) VALUES ('Los Angeles', 'Western', 108, 725000, 835915);
INSERT INTO oficinas (ciudad, region, director, objetivo, ventas) VALUES ('Denver', 'Western', 108, 300000, 186042);
```

En este caso podríamos realizar la siguiente consulta:

```sql
-- comparación entre las ventas reales y las metas de ventas para cada oficina.
SELECT ciudad, region, ventas - objetivo
FROM oficinas;

+-------------+---------+--------------------+
| ciudad      | region  | ventas-objetivo    |
+-------------+---------+--------------------+
| New York    | Eastern |        117637.00   |
| Chicago     | Eastern |        -64958.00   |
| Atlanta     | Eastern |         17911.00   |
| Los Angeles | Western |        110915.00   |
| Denver      | Western |       -113958.00   |
+-------------+---------+--------------------+
5 rows in set (0,00 sec)
```

### Cómo realizar alias de columnas con AS

Con la palabra reservada `AS` podemos crear alias para las columnas. Esto puede ser útil cuando estamos calculando nuevas columnas a partir de valores de las columnas actuales.

En el ejemplo anterior de la tabla `ventas` en la BBDD `tienda`, podríamos modificar el nombre del campo del resultado de la consulta a `precio_total` mediante un alias:

```sql
SELECT id, cantidad_comprada, precio_por_elemento, cantidad_comprada * precio_por_elemento AS 'precio_total'
FROM ventas;
```

Si el nuevo nombre que estamos creando para el alias contiene espacios en blanco es necesario usar comillas simples.

Al crear este alias obtendremos el siguiente resultado:

```sql
+----+-------------------+---------------------+--------------+
| id | cantidad_comprada | precio_por_elemento | precio_total |
+----+-------------------+---------------------+--------------+
|  1 |                 2 |                1.50 |         3.00 |
|  2 |                 5 |                1.75 |         8.75 |
|  3 |                 7 |                2.00 |        14.00 |
|  4 |                 9 |                3.50 |        31.50 |
|  5 |                 6 |                9.99 |        59.94 |
+----+-------------------+---------------------+--------------+
5 rows in set (0,00 sec)
```

[01]: ../../img/ut05/consultas-sql-una-tabla/01.png "01"
[02]: ../../img/ut05/consultas-sql-una-tabla/02.png "02"
[03]: ../../img/ut05/consultas-sql-una-tabla/03.png "03"
