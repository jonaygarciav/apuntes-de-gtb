# Fechas

* [Introducción](#introducción)
* [Tipos de datos de fecha y hora en MySQL](#tipos-de-datos-de-fecha-y-hora-en-mysql)
* [Funciones para trabajar con fechas](#funciones-para-trabajar-con-fechas)
* [Extraer partes de una fecha](#extraer-partes-de-una-fecha)
* [Calcular diferencias entre fechas](#calcular-diferencias-entre-fechas)
  * [DATEDIFF()](#datediff)
  * [TIMESTAMPDIFF()](#timestampdiff)
* [Sumar o restar tiempo a una fecha](#sumar-o-restar-tiempo-a-una-fecha)
  * [INTERVAL](#interval)
  * [DATE\_ADD()](#date_add)
  * [DATE\_SUB()](#date_sub)
* [Filtrar por fechas (uso con WHERE)](#filtrar-por-fechas-uso-con-where)
  * [Comparaciones simples](#comparaciones-simples)
  * [Uso de BETWEEN con fechas](#uso-de-between-con-fechas)
  * [Filtrar por año concreto](#filtrar-por-año-concreto)
  * [Filtrar por mes](#filtrar-por-mes)
  * [Comparaciones con fecha actual](#comparaciones-con-fecha-actual)
* [Formatear fechas](#formatear-fechas)
  * [DATE\_FORMAT()](#date_format)

## Introducción

En las bases de datos es muy habitual trabajar con fechas y horas:

* Fecha de nacimiento
* Fecha de pedido
* Fecha de matrícula
* Fecha y hora de inicio de sesión
* Fecha de entrega

MySQL dispone de varios tipos de datos específicos para fechas y horas, así como funciones que nos permiten:

* Extraer partes de una fecha (año, mes, día…)
* Calcular diferencias entre fechas
* Sumar o restar intervalos
* Formatear fechas
* Obtener la fecha y hora actuales

Trabajar correctamente con fechas es fundamental para poder realizar consultas avanzadas y análisis temporales.

## Tipos de datos de fecha y hora en MySQL

MySQL dispone de los siguientes tipos principales:

| Tipo      | Descripción                    |
| --------- | ------------------------------ |
| DATE      | Solamente fecha (YYYY-MM-DD)   |
| TIME      | Solamente hora (HH:MM:SS)      |
| DATETIME  | Fecha y hora                   |
| TIMESTAMP | Fecha y hora con zona horaria  |
| YEAR      | Solamente año                  |

MySQL utiliza el formato `YYYY-MM-DD` para almacenar las fechas, por ejemplo. `2026-03-19`.

Para fecha y hora se utiliza el formato `YYYY-MM-DD HH:MM:SS`, por ejemplo, `2026-03-19 18:30:00`

## Funciones para trabajar con fechas

Podemos obtener la fecha y hora actual con las siguientes funciones:

| Función   | Descripción                     |
| --------- | ------------------------------- |
| CURDATE() | Devuelve la fecha actual        |
| CURTIME() | Devuelve la hora actual         |
| NOW()     | Defuelve la fecha y hora actual |

> __Nota__: la fecha y hora que devuelven estas funciones son la fecha y hora del OS donde se ejecuta el SGBD de MySQL.

Por ejemplo, para obtener la fecha actual con la función `CURDATE()`:

```
SELECT CURDATE();
+------------+
| CURDATE()  |
+------------+
| 2026-03-19 |
+------------+
1 row in set (0,00 sec)
```

Por ejemplo, para obtener la fecha y hora actual con la función `NOW()`

```
SELECT NOW();
+---------------------+
| NOW()               |
+---------------------+
| 2026-03-19 18:42:35 |
+---------------------+
1 row in set (0,00 sec)
```

## Extraer partes de una fecha

MySQL cuenta también con funciones que nos permiten obtener partes concretas de una fecha o de una hora.

| Función       | Descripción |
| ------------- | ----------- |
| YEAR(fecha)   | Año         |
| MONTH(fecha)  | Mes         |
| DAY(fecha)    | Día         |
| HOUR(fecha)   | Hora        |
| MINUTE(fecha) | Minutos     |
| SECOND(fecha) | Segundos    |

Por ejemplo, para seleccionar el año de nacimiento de un alumno con la función `YEAR(año)`:

```
SELECT nombre, YEAR(fecha_nacimiento) AS año
FROM alumno;
+--------+------+
| nombre | año  |
+--------+------+
| María  | 1990 |
| Juan   | 1998 |
| Pepe   | 1988 |
| Lucía  | 1993 |
| Paco   | 1995 |
| Irene  | 1991 |
| Cristina | 1996 |
| Antonio  | 1994 |
| Manuel   | 1999 |
| Daniel   | 1998 |
+--------+------+
10 rows in set (0,00 sec)
```

## Calcular diferencias entre fechas

### DATEDIFF()

La función `DATEDIFF()` devuelve el número de días entre dos fechas.

Sintaxis:

```
DATEDIFF(fecha1, fecha2)
```

Por ejemplo, para calcular el número de días que han pasado entre la fecha de nacimiento de un alumno y el día de hoy:

```
SELECT nombre,
DATEDIFF(CURDATE(), fecha_nacimiento) AS dias_vividos
FROM alumno;
+--------+---------------+
| nombre | dias_vividos  |
+--------+---------------+
| María  | 12853         |
| Juan   | 10216         |
| Pepe   | 13941         |
| Lucía  | 11990         |
| Paco   | 11110         |
| Irene  | 12774         |
| Cristina | 10800       |
| Antonio  | 11600       |
| Manuel   | 9900        |
| Daniel   | 10240       |
+--------+---------------+
10 rows in set (0,00 sec)
```

### TIMESTAMPDIFF()

La función `TIMESTAMPDIFF()` permite calcular la diferencia en años, meses, días, etc., entre dos fechas

Sintaxis:

```
TIMESTAMPDIFF(unidad, fecha1, fecha2)
```

Unidades posibles: `YEAR`, `MONTH`, `DAY`, `HOUR`, `MINUTE`

Por ejemplo, para calcular la edad de un alumno en años utilizando la fecha de naciemiento y la fecha actual:

```
SELECT nombre,
TIMESTAMPDIFF(YEAR, fecha_nacimiento, CURDATE()) AS edad
FROM alumno;
+--------+------+
| nombre | edad |
+--------+------+
| María  | 35   |
| Juan   | 27   |
| Pepe   | 38   |
| Lucía  | 32   |
| Paco   | 30   |
| Irene  | 34   |
| Cristina | 29 |
| Antonio  | 31 |
| Manuel   | 26 |
| Daniel   | 28 |
+--------+------+
10 rows in set (0,00 sec)
```

## Sumar o restar tiempo a una fecha

### INTERVAL

`INTERVAL` indica que vamos a trabajar con una _cantidad de tiempo_.

Sintaxis:

```
INTERVAL cantidad unidad
```

Donde:
* `cantidad`: número entero
* `unidad`: tipo de tiempo que queremos sumar o restar: `YEAR`, `MONTH`, `DAY`, `HOUR`, `MINUTE`, `SECOND`

Por ejemplo para indicar un intervalo de tiempo de 10 días:

```
INTERVAL 10 DAY
```

### DATE_ADD()

La función `DATE_ADD()` permite añadir una cantidad de tiempo a una fecha.

Sintaxis:

```
DATE_ADD(fecha, INTERVAL cantidad unidad)
```

Por ejemplo, para añadir un intefvalo de 10 días a la fecha `2026-03-19`:

```
SELECT DATE_ADD('2026-03-19', INTERVAL 10 DAY);
+-------------------------------------------+
| DATE_ADD('2026-03-19', INTERVAL 10 DAY)  |
+-------------------------------------------+
| 2026-03-29                                |
+-------------------------------------------+
1 row in set (0,00 sec)
```

### DATE_SUB()

La función `DATE_SUB())` permite restar una cantidad de tiempo a una fecha.

Sintaxis:

```
DATE_SUB(fecha, INTERVAL cantidad unidad)
```

Por ejemplo, para restar un mes a la fecha actual (19/03/2026):

```
SELECT DATE_SUB(CURDATE(), INTERVAL 1 MONTH);
+-------------------------------------------+
| DATE_SUB(CURDATE(), INTERVAL 1 MONTH)    |
+-------------------------------------------+
| 2026-02-19                                |
+-------------------------------------------+
1 row in set (0,00 sec)
```

## Filtrar por fechas (uso con WHERE)

Podemos utilizar operadores de comparación con fechas exactamente igual que con números.

### Comparaciones simples

Alumnos nacidos después de 1995:

```
SELECT *
FROM alumno
WHERE fecha_nacimiento > '1995-01-01';
+----+----------+------------+------------+------------------+--------------+-----------+
| id | nombre   | apellido1  | apellido2  | fecha_nacimiento | es_repetidor | telefono  |
+----+----------+------------+------------+------------------+--------------+-----------+
|  2 | Juan     | Sáez       | Vega       | 1998-04-02       | FALSE        | 618253876 |
|  5 | Paco     | Martínez   | López      | 1995-11-24       | FALSE        | 692735409 |
|  7 | Cristina | Fernández  | Ramírez    | 1996-09-17       | FALSE        | 628349590 |
|  9 | Manuel   | Domínguez  | Hernández  | 1999-07-08       | FALSE        | NULL      |
| 10 | Daniel   | Moreno     | Ruiz       | 1998-02-03       | FALSE        | NULL      |
+----+----------+------------+------------+------------------+--------------+-----------+
5 rows in set (0,00 sec)
```

### Uso de BETWEEN con fechas

Alumnos nacidos entre 1990 y 1994:

```
SELECT *
FROM alumno
WHERE fecha_nacimiento BETWEEN '1990-01-01' AND '1994-12-31';
+----+----------+------------+------------+------------------+--------------+-----------+
| id | nombre   | apellido1  | apellido2  | fecha_nacimiento | es_repetidor | telefono  |
+----+----------+------------+------------+------------------+--------------+-----------+
|  1 | María    | Sánchez    | Pérez      | 1990-12-01       | FALSE        | NULL      |
|  4 | Lucía    | Sánchez    | Ortega     | 1993-06-13       | TRUE         | 678516294 |
|  6 | Irene    | Gutiérrez  | Sánchez    | 1991-03-28       | TRUE         | NULL      |
|  8 | Antonio  | Carretero  | Ortega     | 1994-05-20       | TRUE         | 612345633 |
+----+----------+------------+------------+------------------+--------------+-----------+
4 rows in set (0,00 sec)
```

### Filtrar por año concreto

Seleccionar aquellos alumnos que hayan nacido en 1998:

```
SELECT *
FROM alumno
WHERE YEAR(fecha_nacimiento) = 1998;
+----+--------+------------+------------+------------------+--------------+-----------+
| id | nombre | apellido1  | apellido2  | fecha_nacimiento | es_repetidor | telefono  |
+----+--------+------------+------------+------------------+--------------+-----------+
|  2 | Juan   | Sáez       | Vega       | 1998-04-02       | FALSE        | 618253876 |
| 10 | Daniel | Moreno     | Ruiz       | 1998-02-03       | FALSE        | NULL      |
+----+--------+------------+------------+------------------+--------------+-----------+
2 rows in set (0,00 sec)
```

### Filtrar por mes

Seleccionar aquellos alumnos que hayan nacido el mes de junio:

```
SELECT *
FROM alumno
WHERE MONTH(fecha_nacimiento) = 6;
+----+--------+------------+------------+------------------+--------------+-----------+
| id | nombre | apellido1  | apellido2  | fecha_nacimiento | es_repetidor | telefono  |
+----+--------+------------+------------+------------------+--------------+-----------+
|  4 | Lucía  | Sánchez    | Ortega     | 1993-06-13       | TRUE         | 678516294 |
+----+--------+------------+------------+------------------+--------------+-----------+
1 row in set (0,00 sec)
```

### Comparaciones con fecha actual

Seleccionar  aquellos alumnos mayores de edad:

```
SELECT *
FROM alumno
WHERE TIMESTAMPDIFF(YEAR, fecha_nacimiento, CURDATE()) >= 18;
+----+--------+------------+------------+------------------+--------------+-----------+
| id | nombre | apellido1  | apellido2  | fecha_nacimiento | es_repetidor | telefono  |
+----+--------+------------+------------+------------------+--------------+-----------+
|  1 | María  | Sánchez    | Pérez      | 1990-12-01       | FALSE        | NULL      |
|  2 | Juan   | Sáez       | Vega       | 1998-04-02       | FALSE        | 618253876 |
|  4 | Lucía  | Sánchez    | Ortega     | 1993-06-13       | TRUE         | 678516294 |
|  5 | Paco   | Martínez   | López      | 1995-11-24       | FALSE        | 692735409 |
|  6 | Irene  | Gutiérrez  | Sánchez    | 1991-03-28       | TRUE         | NULL      |
|  7 | Cristina | Fernández | Ramírez   | 1996-09-17       | FALSE        | 628349590 |
|  8 | Antonio  | Carretero | Ortega    | 1994-05-20       | TRUE         | 612345633 |
|  9 | Manuel   | Domínguez | Hernández | 1999-07-08       | FALSE        | NULL      |
| 10 | Daniel   | Moreno    | Ruiz      | 1998-02-03       | FALSE        | NULL      |
+----+--------+------------+------------+------------------+--------------+-----------+
```

## Formatear fechas

### DATE_FORMAT()

La función `DATE_FORMAT()` permite mostrar fechas en formato personalizado.

Sintaxis:

```
DATE_FORMAT(fecha, formato)
```

Por ejemplo, mostar la fecha de naciemiento de un alumno en formato `día/mes/año`:

```
SELECT nombre,
DATE_FORMAT(fecha_nacimiento, '%d/%m/%Y') AS fecha_formateada
FROM alumno;
+--------+------------------+
| nombre | fecha_formateada |
+--------+------------------+
| María  | 01/12/1990       |
| Juan   | 02/04/1998       |
| Pepe   | 03/01/1988       |
| Lucía  | 13/06/1993       |
| Paco   | 24/11/1995       |
| Irene  | 28/03/1991       |
| Cristina | 17/09/1996     |
| Antonio  | 20/05/1994     |
| Manuel   | 08/07/1999     |
| Daniel   | 03/02/1998     |
+--------+------------------+
10 rows in set (0,00 sec)
```

Algunos códigos comunes:

| Código | Significado |
| ------ | ----------- |
| %d     | Día         |
| %m     | Mes         |
| %Y     | Año         |
| %H     | Hora        |
| %i	 | Minuto      |
