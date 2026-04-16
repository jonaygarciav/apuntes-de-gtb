# Comando CREATE TABLE

* [Introducción](#introducción)
* [PRIMARY KEY](#primary-key)
* [AUTO\_INCREMENT](#auto_increment)
* [NOT NULL](#not-null)
* [UNIQUE](#unique)
* [DEFAULT](#default)
* [FOREIGN KEY](#foreign-key)
* [Relación uno a uno (1:1)](#relación-uno-a-uno-11)
* [Relación uno a muchos (1:N)](#relación-uno-a-muchos-1n)
* [Relación muchos a muchos (N:M)](#relación-muchos-a-muchos-nm)

## Introducción

El comando `CREATE TABLE` se utiliza para crear tablas dentro de una base de datos. Una tabla es la estructura donde se almacenan los datos organizados en filas y columnas. Cuando creamos una tabla, estamos definiendo:

* Qué información vamos a guardar (columnas)
* Qué tipo de datos tendrá cada columna
* Qué restricciones se aplican (claves, valores obligatorios, etc.)

Es importante diseñar bien las tablas desde el principio, ya que una mala estructura puede provocar problemas más adelante, como datos duplicados, inconsistencias o dificultad para hacer consultas.

Sintaxis básica:

```
CREATE TABLE nombre_tabla (
    columna1 tipo_dato,
    columna2 tipo_dato,
    ...
);
```

En esta sintaxis:

* `nombre_tabla` es el nombre de la tabla.
* Cada columna tiene un nombre y un tipo de dato.
* Las columnas se separan por comas.

Ejemplo básico:

```
mysql> CREATE TABLE alumnos (
    -> id INT,
    -> nombre VARCHAR(50),
    -> edad INT
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql> DESCRIBE alumnos;
+--------+-------------+------+-----+---------+-------+
| Field  | Type        | Null | Key | Default | Extra |
+--------+-------------+------+-----+---------+-------+
| id     | int         | YES  |     | NULL    |       |
| nombre | varchar(50) | YES  |     | NULL    |       |
| edad   | int         | YES  |     | NULL    |       |
+--------+-------------+------+-----+---------+-------+
3 rows in set (0.00 sec)
```

Explicación:

En este ejemplo se crea una tabla llamada alumnos con tres columnas:

* `id`: número entero.
* `nombre`: texto de hasta 50 caracteres.
* `edad`: número entero.

En este caso no hay restricciones, por lo que:

* Se permiten valores nulos.
* No hay clave primaria.
* Se podrían repetir registros.

## PRIMARY KEY

La `primary key` o `clave primaria` sirve para identificar de forma única cada registro de la tabla. Esto significa que:

* No puede haber valores repetidos.
* No puede haber valores nulos.

Es una de las restricciones más importantes en una base de datos, ya que permite distinguir cada fila del resto.

Ejemplo:

```
mysql> CREATE TABLE profesores (
    -> id INT PRIMARY KEY,
    -> nombre VARCHAR(50)
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql> DESCRIBE profesores;
+--------+-------------+------+-----+---------+-------+
| Field  | Type        | Null | Key | Default | Extra |
+--------+-------------+------+-----+---------+-------+
| id     | int         | NO   | PRI | NULL    |       |
| nombre | varchar(50) | YES  |     | NULL    |       |
+--------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```

* `Campo Key; Valor PRI`: indica clave primaria.
* `Campo Null; Valor NO`: no permite valores nulos.

## AUTO_INCREMENT

La opción `AUTO_INCREMENT` permite que MySQL genere automáticamente el valor de una columna numérica. Se usa normalmente junto con `PRIMARY KEY`, para que cada nuevo registro tenga un identificador único sin tener que escribirlo manualmente.

Cada vez que se inserta un nuevo registro el valor aumenta automáticamente (1, 2, 3, ...)

Ejemplo:

```
mysql> CREATE TABLE departamentos (
    -> id INT PRIMARY KEY AUTO_INCREMENT,
    -> nombre VARCHAR(50)
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql> DESCRIBE departamentos;
+--------+-------------+------+-----+---------+----------------+
| Field  | Type        | Null | Key | Default | Extra          |
+--------+-------------+------+-----+---------+----------------+
| id     | int         | NO   | PRI | NULL    | auto_increment |
| nombre | varchar(50) | YES  |     | NULL    |                |
+--------+-------------+------+-----+---------+----------------+
2 rows in set (0.00 sec)
```

* Aparece el valor `auto_increment` en la columna `Extra`.

## NOT NULL

La restricción `NOT NULL` obliga a que una columna tenga siempre un valor. Es decir, no se permite guardar NULL en esa columna. Se utiliza cuando un dato es obligatorio. Por ejemplo:

* El nombre de una persona.
* El título de un libro.

Esto ayuda a evitar registros incompletos o incorrectos.

Ejemplo:

```
mysql> CREATE TABLE asignaturas (
    -> id INT PRIMARY KEY AUTO_INCREMENT,
    -> nombre VARCHAR(50) NOT NULL
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql> DESCRIBE asignaturas;
+--------+-------------+------+-----+---------+----------------+
| Field  | Type        | Null | Key | Default | Extra          |
+--------+-------------+------+-----+---------+----------------+
| id     | int         | NO   | PRI | NULL    | auto_increment |
| nombre | varchar(50) | NO   |     | NULL    |                |
+--------+-------------+------+-----+---------+----------------+
2 rows in set (0.00 sec)
```

* Columna Null; Valor = NO: en el campo nombre.

## UNIQUE

La restricción `UNIQUE` impide que se repitan valores en una columna. A diferencia de `PRIMARY KEY`:

* Sí permite normalmente un valor NULL.
* Pero no permite duplicados en valores reales.

Se usa cuando un dato debe ser único, por ejemplo: email o DNI.

```
mysql> CREATE TABLE usuarios (
    -> id INT PRIMARY KEY AUTO_INCREMENT,
    -> email VARCHAR(100) UNIQUE
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql> DESCRIBE usuarios;
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int          | NO   | PRI | NULL    | auto_increment |
| email | varchar(100) | YES  | UNI | NULL    |                |
+-------+--------------+------+-----+---------+----------------+
2 rows in set (0.00 sec)
```

* `Columna Key; valor UNI`

## DEFAULT

La restricción `DEFAULT` permite asignar un valor por defecto a una columna. Si al insertar un registro no se indica valor para esa columna MySQL usará automáticamente el valor por defecto.

Esto es útil para:

* evitar valores nulos
* simplificar inserciones

Ejemplo:

```
mysql> CREATE TABLE personas (
    -> id INT PRIMARY KEY AUTO_INCREMENT,
    -> nombre VARCHAR(50),
    -> ciudad VARCHAR(50) DEFAULT 'Sevilla'
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql> DESCRIBE personas;
+--------+-------------+------+-----+----------+----------------+
| Field  | Type        | Null | Key | Default  | Extra          |
+--------+-------------+------+-----+----------+----------------+
| id     | int         | NO   | PRI | NULL     | auto_increment |
| nombre | varchar(50) | YES  |     | NULL     |                |
| ciudad | varchar(50) | YES  |     | Sevilla  |                |
+--------+-------------+------+-----+----------+----------------+
3 rows in set (0.00 sec)
```

## FOREIGN KEY

Una `FOREIGN KEY` o `clave foránea` sirve para relacionar tablas entre sí. Permite que una columna de una tabla haga referencia a la clave primaria de otra. Esto garantiza que los datos estén relacionados correctamente.

Las distintas relaciones que podemos encontrar entre tablas son:

* Uno a uno (1:1).
* 1 a muchos (1:N).
* Muchos a muchos (N:M)

Ejemplo básico de una relación:

```
mysql> CREATE TABLE profesores (
    -> id INT PRIMARY KEY AUTO_INCREMENT,
    -> nombre VARCHAR(50)
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql> CREATE TABLE alumnos (
    -> id INT PRIMARY KEY AUTO_INCREMENT,
    -> nombre VARCHAR(50),
    -> profesor_id INT,
    -> FOREIGN KEY (profesor_id) REFERENCES profesores(id)
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql> DESCRIBE profesores;
+--------+-------------+------+-----+---------+----------------+
| Field  | Type        | Null | Key | Default | Extra          |
+--------+-------------+------+-----+---------+----------------+
| id     | int         | NO   | PRI | NULL    | auto_increment |
| nombre | varchar(50) | YES  |     | NULL    |                |
+--------+-------------+------+-----+---------+----------------+
2 rows in set (0.00 sec)

mysql> DESCRIBE alumnos;
+-------------+-------------+------+-----+---------+----------------+
| Field       | Type        | Null | Key | Default | Extra          |
+-------------+-------------+------+-----+---------+----------------+
| id          | int         | NO   | PRI | NULL    | auto_increment |
| nombre      | varchar(50) | YES  |     | NULL    |                |
| profesor_id | int         | YES  | MUL | NULL    |                |
+-------------+-------------+------+-----+---------+----------------+
3 rows in set (0.00 sec)
```

* `Columna Key; Valor MUL`: indica índice en clave foránea

## Relación uno a uno (1:1)

En una relación `1:1`, cada registro de una tabla está relacionado con uno y solo uno de la otra.

Para conseguir esto:

* `FOREIGN KEY (usuario_id) REFERENCES usuarios(id)`: Usamos `FOREIGN KEY` para crear la relación entre tablas.
* `usuario_id INT UNIQUE;`: Añadimos `UNIQUE`, de lo contrario, estaríamos creando una relación uno a muchos (1:N).

Ejemplo:

```
mysql> CREATE TABLE usuarios (
    -> id INT PRIMARY KEY AUTO_INCREMENT,
    -> nombre VARCHAR(50)
    -> );

mysql> CREATE TABLE perfiles (
    -> id INT PRIMARY KEY AUTO_INCREMENT,
    -> usuario_id INT UNIQUE,
    -> FOREIGN KEY (usuario_id) REFERENCES usuarios(id)
    -> );

mysql> DESCRIBE usuarios;
+--------+-------------+------+-----+---------+----------------+
| Field  | Type        | Null | Key | Default | Extra          |
+--------+-------------+------+-----+---------+----------------+
| id     | int         | NO   | PRI | NULL    | auto_increment |
| nombre | varchar(50) | YES  |     | NULL    |                |
+--------+-------------+------+-----+---------+----------------+
2 rows in set (0.00 sec)

mysql> DESCRIBE perfiles;
+------------+------+------+-----+---------+----------------+
| Field      | Type | Null | Key | Default | Extra          |
+------------+------+------+-----+---------+----------------+
| id         | int  | NO   | PRI | NULL    | auto_increment |
| usuario_id | int  | YES  | UNI | NULL    |                |
+------------+------+------+-----+---------+----------------+
```

## Relación uno a muchos (1:N)

En una relación `1:N`, cada registro de una tabla puede estar relacionado con varios registros de la otra, pero cada uno de esos registros secundarios solo pertenece a uno de la tabla principal.

Para conseguir esto:

* `FOREIGN KEY (usuario_id) REFERENCES usuarios(id)`: Usamos `FOREIGN KEY` para crear la relación entre tablas.
* `usuario_id INT;`: sin `UNIQUE` como hacíamos con la relación uno a uno (1:1).

```
mysql> CREATE TABLE profesores (
    -> id INT PRIMARY KEY AUTO_INCREMENT,
    -> nombre VARCHAR(50)
    -> );

mysql> CREATE TABLE alumnos (
    -> id INT PRIMARY KEY AUTO_INCREMENT,
    -> nombre VARCHAR(50),
    -> profesor_id INT,
    -> FOREIGN KEY (profesor_id) REFERENCES profesores(id)
    -> );

mysql> DESCRIBE profesores;
+--------+-------------+------+-----+---------+----------------+
| Field  | Type        | Null | Key | Default | Extra          |
+--------+-------------+------+-----+---------+----------------+
| id     | int         | NO   | PRI | NULL    | auto_increment |
| nombre | varchar(50) | YES  |     | NULL    |                |
+--------+-------------+------+-----+---------+----------------+
2 rows in set (0.00 sec)

mysql> DESCRIBE alumnos;
+-------------+-------------+------+-----+---------+----------------+
| Field       | Type        | Null | Key | Default | Extra          |
+-------------+-------------+------+-----+---------+----------------+
| id          | int         | NO   | PRI | NULL    | auto_increment |
| nombre      | varchar(50) | YES  |     | NULL    |                |
| profesor_id | int         | YES  | MUL | NULL    |                |
+-------------+-------------+------+-----+---------+----------------+
```

## Relación muchos a muchos (N:M)

En una relación `N:M`, cada registro de una tabla puede estar relacionado con varios registros de la otra, y a su vez, cada uno de esos registros también puede estar relacionado con varios de la primera.

Para conseguir esto:

* Creamos una tabla intermedia con dos columnas:
  * una clave foránea hacia la primera tabla
  * otra clave foránea hacia la segunda tabla
* Usamos dos FOREIGN KEY para crear las relaciones:
```
FOREIGN KEY (alumno_id) REFERENCES alumnos(id)
FOREIGN KEY (curso_id) REFERENCES cursos(id)
```
* Definimos una clave primaria compuesta:
```
PRIMARY KEY (alumno_id, curso_id)
```

> __Nota__: esto evita duplicados en la relación

```
mysql> CREATE TABLE alumnos (
    -> id INT PRIMARY KEY AUTO_INCREMENT,
    -> nombre VARCHAR(50)
    -> );

mysql> CREATE TABLE cursos (
    -> id INT PRIMARY KEY AUTO_INCREMENT,
    -> nombre VARCHAR(50)
    -> );

mysql> CREATE TABLE alumnos_cursos (
    -> alumno_id INT,
    -> curso_id INT,
    -> PRIMARY KEY (alumno_id, curso_id),
    -> FOREIGN KEY (alumno_id) REFERENCES alumnos(id),
    -> FOREIGN KEY (curso_id) REFERENCES cursos(id)
    -> );

mysql> DESCRIBE alumnos_cursos;
+-----------+------+------+-----+---------+-------+
| Field     | Type | Null | Key | Default | Extra |
+-----------+------+------+-----+---------+-------+
| alumno_id | int  | NO   | PRI | NULL    |       |
| curso_id  | int  | NO   | PRI | NULL    |       |
+-----------+------+------+-----+---------+-------+
```
