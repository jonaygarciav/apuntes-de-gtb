# SELECT EN DOS TABLAS

* [Introducción](#introducción)
* [Consulta básica con dos tablas](#consulta-básica-con-dos-tablas)
* [Filtrar resultados con WHERE](#filtrar-resultados-con-where)
* [Mostrar todas las columnas](#mostrar-todas-las-columnas)
* [Usar varias condiciones](#usar-varias-condiciones)
* [Comparaciones en condiciones](#comparaciones-en-condiciones)
* [Añadir alias en los nombres de las columnas (AS)](#añadir-alias-en-los-nombres-de-las-columnas-as)

## Introducción

Cuando trabajamos con bases de datos, muchas veces necesitamos obtener información que está repartida en varias tablas:

* Varias tablas en `FROM`
* Una condición en `WHERE` que las relacione

Esto funciona porque existe una clave foránea (`FOREIGN KEY`) entre las tablas.

Tablas de profesores:

```
CREATE TABLE profesores (
    id INT PRIMARY KEY,
    nombre VARCHAR(50)
);
```

Tabla alumnos:

```
CREATE TABLE alumnos (
    id INT PRIMARY KEY,
    nombre VARCHAR(50),
    profesor_id INT,
    FOREIGN KEY (profesor_id) REFERENCES profesores(id)
);
```

Datos de ejemplo:

```
INSERT INTO profesores VALUES
(1, 'Juan'),
(2, 'María'),
(3, 'Pedro');

INSERT INTO alumnos VALUES
(1, 'Ana', 1),
(2, 'Luis', 2),
(3, 'Marta', 1),
(4, 'Carlos', 3);
```

## Consulta básica con dos tablas

Para consultar datos de dos tablas:

* Se escriben ambas en FROM
* Se relacionan en WHERE


```
SELECT alumnos.nombre, profesores.nombre
FROM alumnos, profesores
WHERE alumnos.profesor_id = profesores.id;

+---------+-----------+
| nombre  | nombre    |
+---------+-----------+
| Ana     | Juan      |
| Marta   | Juan      |
| Luis    | María     |
| Carlos  | Pedro     |
+---------+-----------+
```

Cada alumno aparece con su profesor correspondiente. Se recomienda el uso de `alias` para abreviar el nombre de las tablas:

```
SELECT a.nombre, p.nombre
FROM alumnos a, profesores p
WHERE a.profesor_id = p.id;

+---------+-----------+
| nombre  | nombre    |
+---------+-----------+
| Ana     | Juan      |
| Marta   | Juan      |
| Luis    | María     |
| Carlos  | Pedro     |
+---------+-----------+
```

## Filtrar resultados con WHERE

Podemos añadir condiciones adicionales con `AND` para filtrar resultados.

```
SELECT a.nombre, p.nombre
FROM alumnos a, profesores p
WHERE a.profesor_id = p.id
AND p.nombre = 'Juan';

+---------+--------+
| nombre  | nombre |
+---------+--------+
| Ana     | Juan   |
| Marta   | Juan   |
+---------+--------+
```

Solo aparecen alumnos cuyo profesor es Juan

## Mostrar todas las columnas

Podemos usar `SELECT *` para ver todas las columnas de ambas tablas.

```
SELECT *
FROM alumnos a, profesores p
WHERE a.profesor_id = p.id;

+----+--------+--------------+----+--------+
| id | nombre | profesor_id  | id | nombre |
+----+--------+--------------+----+--------+
|  1 | Ana    |            1 |  1 | Juan   |
|  3 | Marta  |            1 |  1 | Juan   |
|  2 | Luis   |            2 |  2 | María  |
|  4 | Carlos |            3 |  3 | Pedro  |
+----+--------+--------------+----+--------+
```

Aparecen columnas repetidas (id, nombre)

Si no relacionamos las tablas, MySQL hace un producto cartesiano.

Combina todas las filas de una tabla con todas las de la otra:

```
SELECT *
FROM alumnos, profesores;

+----+--------+--------------+----+--------+
| id | nombre | profesor_id  | id | nombre |
+----+--------+--------------+----+--------+
|  1 | Ana    |            1 |  1 | Juan   |
|  1 | Ana    |            1 |  2 | María  |
|  1 | Ana    |            1 |  3 | Pedro  |
|  2 | Luis   |            2 |  1 | Juan   |
|  2 | Luis   |            2 |  2 | María  |
|  2 | Luis   |            2 |  3 | Pedro  |
|  3 | Marta  |            1 |  1 | Juan   |
|  3 | Marta  |            1 |  2 | María  |
|  3 | Marta  |            1 |  3 | Pedro  |
|  4 | Carlos |            3 |  1 | Juan   |
|  4 | Carlos |            3 |  2 | María  |
|  4 | Carlos |            3 |  3 | Pedro  |
+----+--------+--------------+----+--------+
```

## Usar varias condiciones

Podemos añadir más condiciones para refinar la consulta.

```
SELECT a.nombre, p.nombre
FROM alumnos a, profesores p
WHERE a.profesor_id = p.id
AND a.nombre = 'Ana';

+--------+--------+
| nombre | nombre |
+--------+--------+
| Ana    | Juan   |
+--------+--------+
```

## Comparaciones en condiciones

Se pueden usar operadores:

* `=`
* `>`
* `<`
* `>=`
* `<=`

```
SELECT a.nombre, p.nombre
FROM alumnos a, profesores p
WHERE a.profesor_id = p.id
AND p.id > 1;

+---------+--------+
| nombre  | nombre |
+---------+--------+
| Luis    | María  |
| Carlos  | Pedro  |
+---------+--------+
```

## Añadir alias en los nombres de las columnas (AS)

Cuando seleccionamos columnas de dos tablas, es muy común que tengan el mismo nombre (por ejemplo, nombre).

Esto provoca que en el resultado aparezcan columnas duplicadas con el mismo nombre, lo que puede generar confusión.

Para solucionar esto, usamos alias en las columnas con la palabra clave AS.

Sintaxis:

```
SELECT columna AS alias
FROM tabla;
```

Ejemplo con dos tablas:

```
SELECT a.nombre AS alumno, p.nombre AS profesor
FROM alumnos a, profesores p
WHERE a.profesor_id = p.id;

+---------+-----------+
| alumno  | profesor  |
+---------+-----------+
| Ana     | Juan      |
| Marta   | Juan      |
| Luis    | María     |
| Carlos  | Pedro     |
+---------+-----------+
```

* `a.nombre AS alumno`: renombra la columna como alumno
* `p.nombre AS profesor`: renombra la columna como profesor

El resultado ahora es mucho más claro:

```
SELECT a.nombre alumno, p.nombre profesor
FROM alumnos a, profesores p
WHERE a.profesor_id = p.id;

+---------+-----------+
| alumno  | profesor  |
+---------+-----------+
| Ana     | Juan      |
| Marta   | Juan      |
| Luis    | María     |
| Carlos  | Pedro     |
+---------+-----------+
```

Cuándo usar alias:

* Cuando hay columnas con el mismo nombre
* Para mejorar la legibilidad
* En resultados que se mostrarán al usuario
* En exámenes (muy recomendable)
