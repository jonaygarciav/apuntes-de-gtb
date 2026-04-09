# Comando DELETE

* [Introducción](#introducción)
* [Eliminar un registro](#eliminar-un-registro)
* [Eliminar varios registros](#eliminar-varios-registros)
* [Eliminar todos los registros](#eliminar-todos-los-registros)
* [Usar condiciones (WHERE)](#usar-condiciones-where)
* [Uso de IN](#uso-de-in)
* [Uso de BETWEEN](#uso-de-between)
* [DELETE con subconsulta](#delete-con-subconsulta)
* [DELETE con JOIN (avanzado)](#delete-con-join-avanzado)

## Introducción

El comando DELETE se utiliza para eliminar registros de una tabla. Permite:

* Borrar una o varias filas
* Aplicar condiciones con WHERE

> __Nota__: si no usas la cláusula `WHERE`, se eliminan TODOS los registros.

La sintaxis del comando `DELETE` es la siguiente:

```
DELETE FROM nombre_tabla
WHERE condicion;
```

A continuación se muestra una tabla de ejemplo:

```
mysql> SELECT * FROM alumnos;
+----+--------+------+----------+
| id | nombre | edad | ciudad   |
+----+--------+------+----------+
|  1 | Ana    |   22 | Bilbao   |
|  2 | Luis   |   24 | Barcelona|
|  3 | Marta  |   22 | Bilbao   |
+----+--------+------+----------+
3 rows in set (0.00 sec)
```

## Eliminar un registro

Permite eliminar una única fila de la tabla que cumpla una condición concreta. En este caso, se elimina el alumno cuyo nombre es `Ana`. Es importante usar WHERE para evitar borrar más datos de los necesarios.

```
mysql> DELETE FROM alumnos
    -> WHERE nombre = 'Ana';
Query OK, 1 row affected (0.00 sec)
mysql> SELECT * FROM alumnos;
+----+--------+------+-----------+
| id | nombre | edad | ciudad    |
+----+--------+------+-----------+
|  2 | Luis   |   24 | Barcelona |
|  3 | Marta  |   22 | Bilbao    |
+----+--------+------+-----------+
2 rows in set (0.00 sec)
```

## Eliminar varios registros

Permite eliminar varias filas a la vez cuando cumplen una misma condición. En este ejemplo se eliminan todos los alumnos cuya ciudad es `Bilbao`.

```
mysql> DELETE FROM alumnos
    -> WHERE ciudad = 'Bilbao';
Query OK, 2 rows affected (0.00 sec)
mysql> SELECT * FROM alumnos;
+----+--------+------+-----------+
| id | nombre | edad | ciudad    |
+----+--------+------+-----------+
|  2 | Luis   |   24 | Barcelona |
+----+--------+------+-----------+
1 row in set (0.00 sec)
```

## Eliminar todos los registros

Si no se usa `WHERE`, se eliminan todos los registros de la tabla. Este comando deja la tabla vacía, pero no elimina su estructura.

```
mysql> DELETE FROM alumnos;
Query OK, 1 row affected (0.00 sec)
mysql> SELECT * FROM alumnos;
Empty set (0.00 sec)
```

> __Nota__: no elimina la tabla, solo los datos.

## Usar condiciones (WHERE)

La cláusula `WHERE` permite especificar qué registros se deben eliminar. En este caso, se eliminan los alumnos cuya edad es mayor de 22 años.

```
mysql> DELETE FROM alumnos
    -> WHERE edad > 22;
Query OK, 1 row affected (0.00 sec)
```

## Uso de IN

Permite eliminar registros que coincidan con varios valores posibles en una misma columna. Es equivalente a usar varios OR, pero más limpio y legible.

```
mysql> DELETE FROM alumnos
    -> WHERE nombre IN ('Ana', 'Marta');
Query OK, 2 rows affected (0.00 sec)
```

## Uso de BETWEEN

Permite eliminar registros cuyos valores estén dentro de un rango. En este caso, se eliminan los alumnos cuya edad está entre 20 y 22 años (ambos inclusive).

```
mysql> DELETE FROM alumnos
    -> WHERE edad BETWEEN 20 AND 22;
Query OK, 2 rows affected (0.00 sec)
```

## DELETE con subconsulta

#TODO

## DELETE con JOIN (avanzado)

#TODO