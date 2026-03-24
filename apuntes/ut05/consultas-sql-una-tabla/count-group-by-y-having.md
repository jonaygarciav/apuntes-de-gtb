# Función COUNT(), Cláusula GROUP BY y Cláusula HAVING

* [Introducción](#introducción)
* [Función COUNT()](#función-count)
* [Cláusula GROUP BY](#cláusula-group-by)
* [Cláusula HAVING](#cláusula-having)

## Introducción

Cuando trabajamos con bases de datos, muchas veces no solamente queremos ver datos individuales, sino resúmenes o estadísticas, como por ejemplo:

* Cuántos alumnos hay
* Cuántos alumnos hay por grupo
* Cuántos pedidos ha hecho cada cliente
* Qué grupos tienen más de 5 alumnos

Para este tipo de consultas usamos:

* Funciones de agregación: mediante la función `COUNT()`
* Agrupaciones: mediante la cláusla `GROUP BY`
* Filtros sobre agrupaciones: mediante la cláusla `HAVING`
* Función COUNT()

## Función COUNT()

La función `COUNT()` se utiliza para contar filas.

Sintaxis:

```
COUNT(*)

COUNT(columna)
```

Diferencias importantes:

| Forma          | Qué hace                                           |
| -------------- | -------------------------------------------------- |
| COUNT(*)       | Cuenta todas las filas                             |
| COUNT(columna) | Cuenta todas las filas donde la columna NO es NULL |

Por ejemplo, si queremos contar el número total de alumnos de nuestra tabla usamos `COUNT(*)`:

```
SELECT COUNT(*) AS total_alumnos
FROM alumno;
+----------------+
| total_alumnos  |
+----------------+
| 10             |
+----------------+
1 row in set (0,00 sec)
```

Por ejemplo, si queremos contar el número de valores __NO NULOS__ que hay en nuestra tabla, en este caso queremos contar el número de alumnos que tienen un teléfono, es decir, que en el campo `telefono` no tenga el valor `NULL`, para esto usamos `COUNT(telefono)`:

```
SELECT COUNT(telefono) AS alumnos_con_telefono
FROM alumno;
+------------------------+
| alumnos_con_telefono   |
+------------------------+
| 6                      |
+------------------------+
1 row in set (0,00 sec)
```

## Cláusula GROUP BY

La cláusula `GROUP BY` permite agrupar filas que tienen el mismo valor en una o varias columnas. Se usa normalmente junto con funciones como: `COUNT()`, `SUM()`, `AVG()`, `MAX()`, `MIN()`.

Por ejemplo, si queremos contar el número de alumnos en función de si es repetidor o no:

```
SELECT es_repetidor, COUNT(*) AS total
FROM alumno
GROUP BY es_repetidor;
+--------------+-------+
| es_repetidor | total |
+--------------+-------+
| 0            | 6     |
| 1            | 4     |
+--------------+-------+
2 rows in set (0,00 sec)
```

En este caso:

* Tenemos 6 alumnos que no repiten
* Tenmos 4 alumnos repetidores

Por ejemplo, si queremos contar alumnos por año de nacimiento:

```
SELECT YEAR(fecha_nacimiento) AS año, COUNT(*) AS total
FROM alumno
GROUP BY año;
+------+-------+
| año  | total |
+------+-------+
| 1988 | 1     |
| 1990 | 1     |
| 1991 | 1     |
| 1993 | 1     |
| 1994 | 1     |
| 1995 | 1     |
| 1996 | 1     |
| 1998 | 2     |
| 1999 | 1     |
+------+-------+
```

## Cláusula HAVING

La cláusula `HAVING` es una cláusula de SQL que se utiliza para filtrar resultados después de haber agrupado los datos con `GROUP BY`, es decir, permite aplicar condiciones sobre grupos de datos, no sobre filas individuales: la cláusula `HAVING` sirve para filtrar grupos creados con `GROUP BY`.

Diferencias entre la cláusula `WHERE` y la cláusula `HAVING`:

| Cláusula  | Cuándo actúa         | Sobre qué actúa    |
| --------- | -------------------- | ------------------ |
| WHERE     | Antes del GROUP BY   | Filas individuales |
| HAVING    | Después del GROUP BY | Grupos             |

Por ejemplo, si tenemos una tabla de alumnos donde queremos contar cuántos alumnos hay por año de nacimiento.

```
SELECT YEAR(fecha_nacimiento) AS año, COUNT(*) AS total
FROM alumno
GROUP BY año;
+------+-------+
| año  | total |
+------+-------+
| 1990 | 1     |
| 1991 | 1     |
| 1993 | 1     |
| 1994 | 1     |
| 1995 | 1     |
| 1996 | 1     |
| 1998 | 2     |
| 1999 | 1     |
+------+-------+
```

Ahora vamos a usar la cláusula `HAVING` para aplicar una condición sobre el grupo de datos de la consulta anterior, si queremos mostrar solamente los años donde hay más de un alumno:

```
SELECT YEAR(fecha_nacimiento) AS año, COUNT(*) AS total
FROM alumno
GROUP BY año
HAVING total > 1;
+------+-------+
| año  | total |
+------+-------+
| 1998 | 2     |
+------+-------+
```

Otro ejemplo, si queremos contar cuántos alumnos hay en cada grupo (repetidores y no repetidores) y luego mostrar solamente los grupos que tienen 3 o más alumnos:

```
SELECT es_repetidor, COUNT(*) AS total
FROM alumno
GROUP BY es_repetidor
HAVING total >= 3;
+--------------+-------+
| es_repetidor | total |
+--------------+-------+
| 0            | 6     |
| 1            | 4     |
+--------------+-------+
```
