# Cláusula Where

* [Introducción](#introducción)
* [Operadores disponibles en MySQL](#operadores-disponibles-en-mysql)
* [Operador BETWEEN](#operador-between)
* [Operador IN](#operador-in)
* [Operador LIKE](#operador-like)
* [Ejemplos de uso de la cláusula WHERE](#ejemplos-de-uso-de-la-cláusula-where)
  * [Filtrado por comparaciones simples](#filtrado-por-comparaciones-simples)
  * [Uso de operadores lógicos (AND, OR, NOT)](#uso-de-operadores-lógicos-and-or-not)
  * [Comprobación de rangos y conjuntos (BETWEEN e IN)](#comprobación-de-rangos-y-conjuntos-between-e-in)
  * [Búsqueda de patrones de texto (LIKE)](#búsqueda-de-patrones-de-texto-like)
  * [Tratamiento de valores nulos (IS NULL)](#tratamiento-de-valores-nulos-is-null)
  * [Operaciones en la cláusula WHERE](#operaciones-en-la-cláusula-where)

## Introducción

La cláusula `WHERE` nos permite añadir filtros a nuestras consultas para seleccionar sólo aquellas filas que cumplen una determinada condición. Estas condiciones se denominan predicados y el resultado de estas condiciones puede ser verdadero, falso o desconocido.

Una condición tendrá un resultado desconocido cuando alguno de los valores utilizados tiene el valor `NULL`.

Podemos diferenciar cinco tipos de condiciones que pueden aparecer en la cláusula `WHERE`:

* Condiciones para comparar valores o expresiones.
* Condiciones para comprobar si un valor está dentro de un rango de valores.
* Condiciones para comprobar si un valor está dentro de un conjunto de valores.
* Condiciones para comparar cadenas con patrones.
* Condiciones para comprobar si una columna tiene valores a `NULL`.

Los operandos usados en las condiciones pueden ser nombres de columnas, constantes o expresiones. Los operadores que podemos usar en las condiciones pueden ser aritméticos, de comparación, lógicos, etc.

## Operadores disponibles en MySQL

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


## Operador BETWEEN

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

## Operador IN

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

## Operador LIKE

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

## Ejemplos de uso de la cláusula WHERE

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


### Filtrado por comparaciones simples

Utilizamos operadores estándar para buscar coincidencias exactas o rangos numéricos y de fechas. 

Alumnos cuyo primer apellido es `Sánchez`:

```
SELECT nombre, apellido1 FROM alumno WHERE apellido1 = 'Sánchez';
```

Alumnos que `NO` son repetidores (uso de BOOLEAN):

```
SELECT nombre, apellido1 FROM alumno WHERE es_repetidor = FALSE;
-- También es válido: WHERE es_repetidor IS FALSE o WHERE es_repetidor = 0
```

Alumnos nacidos antes de 1994:

```
SELECT nombre, fecha_nacimiento FROM alumno WHERE fecha_nacimiento < '1994-01-01';
```


### Uso de operadores lógicos (AND, OR, NOT)

Permiten combinar criterios para consultas más complejas.

Alumnos que son repetidores Y nacieron después de 1992:

```
SELECT * FROM alumno WHERE es_repetidor = TRUE AND fecha_nacimiento > '1992-12-31';
```

Alumnos que se llaman 'María' o 'Juan':

```
SELECT * FROM alumno WHERE nombre = 'María' OR nombre = 'Juan';
```

### Comprobación de rangos y conjuntos (BETWEEN e IN)

Ideal para simplificar el código cuando trabajamos con listas de valores o intervalos.

Alumnos con ID entre 3 y 7:

```
SELECT * FROM alumno WHERE id BETWEEN 3 AND 7;
```

Alumnos cuyo nombre es 'Pepe', 'Lucía' o 'Irene':

```
SELECT * FROM alumno WHERE nombre IN ('Pepe', 'Lucía', 'Irene');
```

### Búsqueda de patrones de texto (LIKE)

Útil para búsquedas parciales. El símbolo % representa cualquier cantidad de caracteres.

Alumnos cuyo apellido2 termina en 'ez': (Pérez, Martínez, Hernández...)

```
SELECT nombre, apellido2 FROM alumno WHERE apellido2 LIKE '%ez';
```

Alumnos cuyo teléfono empieza por '67':

```
SELECT nombre, telefono FROM alumno WHERE telefono LIKE '67%';
```

### Tratamiento de valores nulos (IS NULL)

Recuerda que para valores NULL no se usa el signo =, sino el operador IS.

Alumnos que no tienen teléfono registrado:

```
SELECT nombre, apellido1 FROM alumno WHERE telefono IS NULL;
```

Alumnos que sí tienen un segundo apellido registrado:

```
SELECT nombre, apellido1, apellido2 FROM alumno WHERE apellido2 IS NOT NULL;
```

### Operaciones en la cláusula WHERE

Podemos realizar cálculos o transformaciones antes de filtrar.

Alumnos cuyo ID es un número par:

```
SELECT id, nombre FROM alumno WHERE (id % 2) = 0;
```