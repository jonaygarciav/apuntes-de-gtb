# Comando INSERT

* [Introducción](#introducción)
* [Insertar un registro](#insertar-un-registro)
* [Insertar varios registros](#insertar-varios-registros)
* [Insertar indicando columnas](#insertar-indicando-columnas)
* [Insertar todos los valores](#insertar-todos-los-valores)
* [Uso de INSERT con valores NULL](#uso-de-insert-con-valores-null)
* [Uso de INSERT con funciones](#uso-de-insert-con-funciones)

## Introducción

El comando `INSERT` se utiliza para añadir nuevos registros a una tabla en una base de datos.

Permite:

* Insertar uno o varios registros
* Especificar todas o algunas columnas
* Insertar datos desde otra tabla

La sintaxis básica es la siguiente:

```
INSERT INTO nombre_tabla (columna1, columna2, ...)
VALUES (valor1, valor2, ...);
```

* `INSERT INTO nombre_tabla`: especifica la tabla donde se insertan los datos
* `(columna1, columna2, ...)`: columnas donde se insertarán los valores
* `VALUES (valor1, valor2, ...)`: valores correspondientes a las columnas

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

## Insertar un registro

Si queremos añadir un nuevo alumno:

```
mysql> INSERT INTO alumnos (nombre, edad, ciudad)
    -> VALUES ('Carlos', 23, 'Bilbao');
Query OK, 1 row affected (0.00 sec)

mysql> SELECT * FROM alumnos;
+----+---------+------+----------+
| id | nombre  | edad | ciudad   |
+----+---------+------+----------+
|  1 | Ana     |   20 | Madrid   |
|  2 | Luis    |   22 | Sevilla  |
|  3 | Marta   |   21 | Valencia |
|  4 | Carlos  |   23 | Bilbao   |
+----+---------+------+----------+
```

## Insertar varios registros

Podemos insertar varios registros en una sola sentencia:

```
mysql> INSERT INTO alumnos (nombre, edad, ciudad)
    -> VALUES 
    -> ('Laura', 19, 'Granada'),
    -> ('Pedro', 24, 'Zaragoza');
Query OK, 2 rows affected (0.00 sec)

mysql> SELECT * FROM alumnos;
+----+---------+------+----------+
| id | nombre  | edad | ciudad   |
+----+---------+------+----------+
|  1 | Ana     |   20 | Madrid   |
|  2 | Luis    |   22 | Sevilla  |
|  3 | Marta   |   21 | Valencia |
|  4 | Carlos  |   23 | Bilbao   |
|  5 | Laura   |   19 | Granada  |
|  6 | Pedro   |   24 | Zaragoza |
+----+---------+------+----------+
```

## Insertar indicando columnas

Es recomendable indicar siempre las columnas en el comando `INSERT`:

```
mysql> INSERT INTO alumnos (nombre, ciudad)
    -> VALUES ('Sofía', 'Madrid');
```
En este caso:

* `edad` será NULL o valor por defecto

## Insertar todos los valores

Si insertamos todos los campos, podemos omitir las columnas:

```
mysql> INSERT INTO alumnos
    -> VALUES (7, 'Javier', 25, 'Valencia');
```

> __Nota__: hay que respetar el orden de las columnas de la tabla.

## Uso de INSERT con valores NULL

Es útil cuando no conocemos algún dato.

```
mysql> INSERT INTO alumnos (nombre, edad, ciudad)
    -> VALUES ('Lucía', NULL, 'Sevilla');
```

## Uso de INSERT con funciones

Podemos usar el comando `INSERT` con funciones de MySQL:

```
mysql> INSERT INTO alumnos (nombre, edad, ciudad)
    -> VALUES ('Mario', 20 + 2, UPPER('madrid'));
```

Resultado:
* `edad`: 22
* `ciudad`: MADRID
