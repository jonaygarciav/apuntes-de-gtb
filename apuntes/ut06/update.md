# Comando UPDATE

* [Introducción](#introducción)
* [Actualizar una columna](#actualizar-una-columna)
* [Actualizar varias columnas](#actualizar-varias-columnas)
* [Actualizar todos los registros (sin WHERE)](#actualizar-todos-los-registros-sin-where)
* [Uso UPDATE con expresiones](#uso-update-con-expresiones)
* [Uso de UPDATE con la cláusula WHERE](#uso-de-update-con-la-cláusula-where)
* [Uso de UPDATE con IN](#uso-de-update-con-in)
* [Uso de UPDATE con subconsulta](#uso-de-update-con-subconsulta)

## Introducción

El comando `UPDATE` se utiliza para modificar datos existentes en una tabla de una base de datos.

Permite:

* Cambiar valores de una o varias columnas
* Modificar uno o varios registros
* Aplicar condiciones con la cláusula `WHERE`

> __Nota__: si no usamos `WHERE`, se actualizarán TODOS los registros de la tabla.

La sintáxis del comando es la siguiente:

```
UPDATE nombre_tabla
SET columna1 = valor1, columna2 = valor2, ...
WHERE condicion;
```

* `UPDATE nombre_tabla`: especifica el nombre de la tabla a modificar
* `SET columna1 = valor1, columna2 = valor2, ...`: especifica las columnas a modificar y sus nuevos valores
* `WHERE condicion`: filtra los registros que se van a modificar

Usaremos la siguiente tabla de ejemplo:

```
mysql> SELECT * FROM alumnos;
+----+--------+------+----------+
| id | nombre | edad | ciudad   |
+----+--------+------+----------+
|  1 | Ana    |   20 | Madrid   |
|  2 | Luis   |   22 | Sevilla  |
|  3 | Marta  |   21 | Valencia |
+----+--------+------+----------+
3 rows in set (0.00 sec)
```

## Actualizar una columna

Partiendo de la tabla `alumnos`, si queremos modificar la edad de `Ana`:

```
mysql> UPDATE alumnos
    -> SET edad = 21
    -> WHERE nombre = 'Ana';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> SELECT * FROM alumnos;
+----+--------+------+----------+
| id | nombre | edad | ciudad   |
+----+--------+------+----------+
|  1 | Ana    |   21 | Madrid   |
|  2 | Luis   |   22 | Sevilla  |
|  3 | Marta  |   21 | Valencia |
+----+--------+------+----------+
3 rows in set (0.00 sec)
```

## Actualizar varias columnas

Partiendo de la tabla `alumnos`, si queremos modificar la edad y la ciudad de `Luis`:

```
mysql> UPDATE alumnos
    -> SET edad = 23, ciudad = 'Granada'
    -> WHERE id = 2;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> SELECT * FROM alumnos;
+----+--------+------+----------+
| id | nombre | edad | ciudad   |
+----+--------+------+----------+
|  1 | Ana    |   21 | Madrid   |
|  2 | Luis   |   23 | Granada  |
|  3 | Marta  |   21 | Valencia |
+----+--------+------+----------+
3 rows in set (0.00 sec)
```

## Actualizar todos los registros (sin WHERE)

Partiendo de la tabla `alumnos`, si queremos modificar la ciudad de todos y cada uno de los registros que hay en la tabla a `Sevilla`:

```
mysql> UPDATE alumnos
    -> SET ciudad = 'Sevilla';
Query OK, 3 rows affected (0.00 sec)
Rows matched: 3  Changed: 3  Warnings: 0

mysql> SELECT * FROM alumnos;
+----+--------+------+----------+
| id | nombre | edad | ciudad   |
+----+--------+------+----------+
|  1 | Ana    |   21 | Sevilla  |
|  2 | Luis   |   23 | Sevilla  |
|  3 | Marta  |   21 | Sevilla  |
+----+--------+------+----------+
3 rows in set (0.00 sec)
```

## Uso UPDATE con expresiones

Partiendo de la tabla `alumnos`, si queremos incrementar la edad de todos y cada uno de los registros que hay en la tabla en `1`:

```
mysql> UPDATE alumnos
    -> SET edad = edad + 1;
Query OK, 3 rows affected (0.00 sec)
Rows matched: 3  Changed: 3  Warnings: 0

mysql> SELECT * FROM alumnos;
+----+--------+------+----------+
| id | nombre | edad | ciudad   |
+----+--------+------+----------+
|  1 | Ana    |   22 | Sevilla  |
|  2 | Luis   |   24 | Sevilla  |
|  3 | Marta  |   22 | Sevilla  |
+----+--------+------+----------+
3 rows in set (0.00 sec)
```

> __Nota__: se pueden usar operaciones matemáticas directamente.

##  Uso de UPDATE con la cláusula WHERE

Partiendo de la tabla `alumnos`, si queremos modificar la ciudad a `Barcelona` a aquellas personas cuyos alumnos tienen más de 22 años:

```
mysql> UPDATE alumnos
    -> SET ciudad = 'Barcelona'
    -> WHERE edad > 22;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> SELECT * FROM alumnos;
+----+--------+------+-----------+
| id | nombre | edad | ciudad    |
+----+--------+------+-----------+
|  1 | Ana    |   22 | Sevilla   |
|  2 | Luis   |   24 | Barcelona |
|  3 | Marta  |   22 | Sevilla   |
+----+--------+------+-----------+
3 rows in set (0.00 sec)
```

## Uso de UPDATE con IN

Partiendo de la tabla `alumnos`, si queremos modificar la ciudad a `Bilbao` de aquellas personas que se llaman `Ana` o `Marta`:

```
mysql> UPDATE alumnos
    -> SET ciudad = 'Bilbao'
    -> WHERE nombre IN ('Ana', 'Marta');
Query OK, 2 rows affected (0.00 sec)
Rows matched: 2  Changed: 2  Warnings: 0

mysql> SELECT * FROM alumnos;
+----+--------+------+-----------+
| id | nombre | edad | ciudad    |
+----+--------+------+-----------+
|  1 | Ana    |   22 | Bilbao    |
|  2 | Luis   |   24 | Barcelona |
|  3 | Marta  |   22 | Bilbao    |
+----+--------+------+-----------+
3 rows in set (0.00 sec)
```

## Uso de UPDATE con subconsulta

#TODO
