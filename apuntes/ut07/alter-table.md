# Comando CREATE TABLE

* [Introducción](#introducción)
* [ADD COLUMN](#add-column)
* [MODIFY COLUMN](#modify-column)
* [CHANGE COLUMN](#change-column)
* [DROP COLUMN](#drop-column)
* [ADD PRIMARY KEY](#add-primary-key)
* [DROP PRIMARY KEY](#drop-primary-key)
* [ADD UNIQUE](#add-unique)
* [ADD FOREIGN KEY](#add-foreign-key)
* [DROP FOREIGN KEY](#drop-foreign-key)
* [RENAME TABLE](#rename-table)

## Introducción

El comando `ALTER TABLE` se utiliza para modificar una tabla ya existente en una base de datos.

Mientras que `CREATE TABLE` sirve para crear la estructura inicial, `ALTER TABLE` permite hacer cambios posteriores como:

* Añadir o eliminar columnas
* Modificar tipos de datos
* Añadir o eliminar restricciones
* Renombrar columnas o tablas

Sintaxis básica:

```
ALTER TABLE nombre_tabla
accion;
```

Ejemplo:

```
mysql> ALTER TABLE alumnos
    -> ADD edad INT;
Query OK, 0 rows affected (0.02 sec)
```

## ADD COLUMN

Se utiliza para añadir una nueva columna a una tabla existente.

Sintaxis:

```
ALTER TABLE nombre_tabla
ADD nombre_columna tipo_dato;
```

Ejemplo:

```
mysql> ALTER TABLE alumnos
    -> ADD email VARCHAR(100);
```

Ahora la tabla tendrá una nueva columna `email`.

## MODIFY COLUMN

Se utiliza para modificar el tipo o las restricciones de un campo de una tabla ya existente.

Sintaxis:

```
ALTER TABLE nombre_tabla
MODIFY nombre_columna nuevo_tipo_dato;
```

Ejemplo:

```
mysql> ALTER TABLE alumnos
    -> MODIFY nombre VARCHAR(100);
```

Se cambia el tamaño del campo `nombre` de 50 a 100 caracteres.

## CHANGE COLUMN

Permite cambiar el nombre de una columna y también su tipo de dato.

Sintaxis:

```
ALTER TABLE nombre_tabla
CHANGE nombre_antiguo nombre_nuevo tipo_dato;
```

Ejemplo:

```
mysql> ALTER TABLE alumnos
    -> CHANGE nombre nombre_completo VARCHAR(100);
```

Se renombra la columna nombre a nombre_completo.

## DROP COLUMN

Se utiliza para eliminar una columna de la tabla.

Sintaxis:

```
ALTER TABLE nombre_tabla
DROP COLUMN nombre_columna;
```

Ejemplo:

```
mysql> ALTER TABLE alumnos
    -> DROP COLUMN edad;
```

> __Nota__: se perderán todos los datos almacenados en esa columna.

## ADD PRIMARY KEY

Permite añadir una __clave primaria__ a una tabla que no la tenía.

Sintaxis:

```
ALTER TABLE nombre_tabla
ADD PRIMARY KEY (columna);
```

Ejemplo:

```
mysql> ALTER TABLE alumnos
    -> ADD PRIMARY KEY (id);
```

## DROP PRIMARY KEY

Permite eliminar la __clave primaria__ de una tabla.

Sintaxis:

```
ALTER TABLE nombre_tabla
DROP PRIMARY KEY;
```

Ejemplo:

```
mysql> ALTER TABLE alumnos
    -> DROP PRIMARY KEY;
```

## ADD UNIQUE

Permite añadir una restricción `UNIQUE` a una columna.

Sintaxis:

```
ALTER TABLE nombre_tabla
ADD UNIQUE (columna);
```

Ejemplo:

```
mysql> ALTER TABLE usuarios
    -> ADD UNIQUE (email);
```

## ADD FOREIGN KEY

Permite crear una clave foránea en una tabla ya existente.

Sintaxis:

```
ALTER TABLE nombre_tabla
ADD FOREIGN KEY (columna)
REFERENCES otra_tabla(columna);
```

Ejemplo:

```
mysql> ALTER TABLE alumnos
    -> ADD profesor_id INT,
    -> ADD FOREIGN KEY (profesor_id) REFERENCES profesores(id);
```

Explicación:

* Se añade la columna profesor_id
* Se establece la relación con la tabla profesores

## DROP FOREIGN KEY

Permite eliminar una clave foránea de una tabla.

> __Nota__: necesitas conocer el nombre de la restricción.

Sintaxis:

```
ALTER TABLE nombre_tabla
DROP FOREIGN KEY nombre_constraint;
```

Ejemplo:

```
mysql> ALTER TABLE alumnos
    -> DROP FOREIGN KEY alumnos_ibfk_1;
```

## RENAME TABLE

Permite cambiar el nombre de una tabla.

Sintaxis:

```
ALTER TABLE nombre_antiguo
RENAME nombre_nuevo;
```

Ejemplo:

```
mysql> ALTER TABLE alumnos
    -> RENAME estudiantes;
```
