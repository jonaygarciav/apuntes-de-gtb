# Comandos Base de Datos

* [Sentencias SQL](#sentencias-sql)
  * [Data Definition Language (DDL)](#data-definition-language-ddl)
  * [Data Manipulation Language (DML)](#data-manipulation-language-dml)
  * [Data Control Language (DCL)](#data-control-language-dcl)
  * [Transaction Control Language (TCL)](#transaction-control-language-tcl)
* [Comando USE](#comando-use)
* [Comando SHOW](#comando-show)
* [Operador LIKE](#operador-like)
* [Comando SOURCE](#comando-source)

## Sentencias SQL

__SQL__ (Structured Query Language) es un lenguaje estándar para interactuar con bases de datos relacionales. Fue desarrollado en la década de 1970 en los laboratorios de _IBM_ por _Donald D. Chamberlin_ y _Raymond F. Boyce_, como parte del sistema _System R_.

La primera versión oficial de _SQL_ fue introducida en 1986 llamada _SQL-86_, cuando el lenguaje fue estandarizado por _ANSI_ (_American National Standards Institute_).

Las _sentencias SQL_ se agrupan en varios tipos según su propósito: __DDL__, __DML__, __DCL__, y __TCL__.

| **Categoría** | **Propósito**                                  | **Ejemplos**                                          |
|---------------|------------------------------------------------|-------------------------------------------------------|
| **DDL**       | Definir o modificar estructuras de datos.      | `CREATE`, `ALTER`, `DROP`, `TRUNCATE`                 |
| **DML**       | Manipular datos dentro de las tablas.          | `INSERT`, `UPDATE`, `DELETE`, `SELECT`                |
| **DCL**       | Gestionar permisos y acceso.                   | `GRANT`, `REVOKE`                                     |
| **TCL**       | Gestionar transacciones en la base de datos.   | `COMMIT`, `ROLLBACK`, `SAVEPOINT`, `SET TRANSACTION`  |

### Data Definition Language (DDL)

Las sentencias __DDL__ (_Data Definition Language_) se utilizan para definir y modificar la estructura de bases de datos, tablas, índices, vistas, etc.

| **Comando** | **Descripción**                                                                        | **Ejemplo**                                               |
|-------------|----------------------------------------------------------------------------------------|-----------------------------------------------------------|
| `CREATE`    | Crea una nueva base de datos, tabla, índice o vista.                                   | `CREATE TABLE empleados (id INT, nombre VARCHAR(50));`    |
| `ALTER`     | Modifica la estructura de una tabla existente (añadir, cambiar o eliminar columnas).   | `ALTER TABLE empleados ADD columna_sueldo FLOAT;`         |
| `DROP`      | Elimina una base de datos, tabla, índice o vista existente.                            | `DROP TABLE empleados;`                                   |
| `TRUNCATE`  | Elimina todos los registros de una tabla pero conserva su estructura.                  | `TRUNCATE TABLE empleados;`                               |

Ejemplos de Operaciones DDL con bases de datos:

```sql
-- Crear una base de datos
CREATE DATABASE empresa;

-- Seleccionar una base de datos
USE empresa;

-- Eliminar una base de datos
DROP DATABASE empresa;

-- Crear una tabla dentro de la base de datos seleccionada
CREATE TABLE empleados (
    id INT PRIMARY KEY,
    nombre VARCHAR(50),
    salario FLOAT
);

-- Modificar la estructura de una tabla
ALTER TABLE empleados ADD fecha_contratacion DATE;

-- Eliminar una tabla
DROP TABLE empleados;

-- Eliminar todos los datos de una tabla, pero conservar su estructura
TRUNCATE TABLE empleados;
```

Las sentencias _DDL_:
* Afectan a la estructura o el esquema de la base de datos.
* Los cambios realizados por sentencias _DDL_ son permanentes y no pueden deshacerse.

### Data Manipulation Language (DML)

Las sentencias __DML__ (_Data Definition Language_) se utilizan para manipular de datos dentro de los objetos definidos por _DDL_, como _tablas_ o _vistas_.

| **Comando** | **Descripción**                                          | **Ejemplo**                                                |
|-------------|----------------------------------------------------------|------------------------------------------------------------|
| `INSERT`    | Inserta uno o más registros en una tabla.                | `INSERT INTO empleados (id, nombre) VALUES (1, 'Juan');`   |
| `UPDATE`    | Actualiza uno o más registros existentes en una tabla.   | `UPDATE empleados SET nombre = 'Carlos' WHERE id = 1;`     |
| `DELETE`    | Elimina uno o más registros de una tabla.                | `DELETE FROM empleados WHERE id = 1;`                      |
| `SELECT`    | Recupera datos de una tabla o combinación de tablas.     | `SELECT * FROM empleados;`                                 |

Ejemplos de operaciones _DML_ con bases de datos:

```sql
-- Insertar datos en una tabla
INSERT INTO empleados (id, nombre, salario, fecha_contratacion)
VALUES (1, 'Juan', 2500.00, '2022-01-15');

-- Actualizar datos en una tabla
UPDATE empleados
SET salario = 2700.00
WHERE id = 1;

-- Eliminar datos de una tabla
DELETE FROM empleados
WHERE id = 1;

-- Consultar datos (todos los campos) de una tabla
SELECT * FROM empleados;

-- Consultar datos (solamente algunos campos) de una tabla
SELECT nombre,salario FROM empleados;
--
```

Las sentencias _DML_:
* Afectan a los datos almacenados en las tablas.
* Los cambios realizados por sentencias _DML_ pueden deshacerse si se usan con transacciones (_COMMIT_ o _ROLLBACK_).

### Data Control Language (DCL)

Las sentencias __DCL__ (_Data Control Language_) se utilizan para controlar los permisos y el acceso a la base de datos.

| **Comando**  | **Descripción**                                                                     | **Ejemplo**                                       |
|--------------|-------------------------------------------------------------------------------------|---------------------------------------------------|
| `GRANT`      | Concede permisos a usuarios para realizar acciones específicas en la base de datos. | `GRANT SELECT, INSERT ON empleados TO usuario;`   |
| `REVOKE`     | Revoca permisos previamente otorgados a un usuario.                                 | `REVOKE SELECT ON empleados FROM usuario;`        |


Ejemplos de operaciones _DML_ con bases de datos:

```sql
-- Conceder permisos a un usuario
GRANT SELECT, INSERT ON empleados TO <usuario>;

-- Revocar permisos a un usuario
REVOKE SELECT ON empleados FROM <usuario>;
```

Las sentencias _DTL_:
* Se utilizan principalmente para gestionar la seguridad de la base de datos.
* Permite controlar quién puede acceder y qué operaciones pueden realizar los usuarios.

### Transaction Control Language (TCL)

Las sentencias __TCL__ (_Transaction Control Language_) gestionan las transacciones en la base de datos, asegurando consistencia e integridad de los datos.

| **Comando**       | **Descripción**                                                                    | **Ejemplo**                                         |
|-------------------|------------------------------------------------------------------------------------|-----------------------------------------------------|
| `COMMIT`          | Guarda todos los cambios realizados en la base de datos dentro de una transacción. | `COMMIT;`                                           |
| `ROLLBACK`        | Deshace todos los cambios realizados dentro de una transacción activa.             | `ROLLBACK;`                                         |
| `SAVEPOINT`       | Marca un punto intermedio dentro de una transacción.                               | `SAVEPOINT punto_intermedio;`                       |
| `SET TRANSACTION` | Define las propiedades de una transacción, como su nivel de aislamiento.           | `SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;`     |

Ejemplos de operaciones _TCL_ con bases de datos:

```sql
-- Iniciar y confirmar una transacción
BEGIN TRANSACTION;
INSERT INTO empleados (id, nombre, salario) VALUES (3, 'Carlos', 2800.00);
COMMIT;

-- Deshacer cambios realizados dentro de una transacción
BEGIN TRANSACTION;
INSERT INTO empleados (id, nombre, salario) VALUES (4, 'Luis', 2900.00);
ROLLBACK;

-- Usar puntos intermedios dentro de una transacción
BEGIN TRANSACTION;
INSERT INTO empleados (id, nombre, salario) VALUES (5, 'Marta', 3000.00);
SAVEPOINT punto_intermedio;
UPDATE empleados SET salario = 3100.00 WHERE id = 5;
ROLLBACK TO punto_intermedio;
COMMIT;
```

Las sentencias _DTL_:
* Aseguran la consistencia e integridad de los datos.
* Permiten deshacer operaciones no deseadas (_ROLLBACK_) o guardar cambios realizados (_COMMIT_).

## Comando USE

El comando `USE` se utiliza para seleccionar una base de datos específica dentro de un servidor o sistema gestor de bases de datos (DBMS). Después de ejecutar este comando, todas las sentencias subsiguientes (como SELECT, INSERT, UPDATE, etc.) se aplicarán a la base de datos seleccionada, a menos que se especifique otra base de datos explícitamente.

La sintaxis del comando es la siguiente:

```sql
USE nombre-base-de-datos;
```

* __nombre-base-de-datos__: es el nombre de la base de datos que deseas seleccionar.

Ejemplos de uso:

```sql
-- Seleccionar una base de datos
USE empresa; -- Después de ejecutar esta sentencia, cualquier operación (SELECT, INSERT, etc.) afectará a la base de datos empresa.

-- Crear una base de datos
CREATE DATABASE tienda;

-- Seleccionar la base de datos creada
USE tienda;
```

El comando `USE`:
* Selecciona una base de datos activa.
* Es útil para cambiar el contexto dentro de una sesión SQL.

En `MySQL`, el comando `USE` no pertenece estrictamente a ninguna de las categorías estándar de sentencias _SQL_ (_DDL_, _DML_, _DCL_, _TCL_). Sin embargo, en el contexto de _MySQL_, se agrupa como _comando de administración del servidor_ o _comando de sesión y contexto_.

## Comando SHOW

El comando `SHOW` se utiliza para mostrar información sobre las bases de datos, tablas, usuarios, variables del sistema, y configuraciones del DBMS. Es una herramienta poderosa para inspeccionar el estado de un sistema de base de datos.

| **Opción**       | **Descripción**                                                                   |
|-------------------|----------------------------------------------------------------------------------|
| `DATABASES`       | Lista todas las bases de datos disponibles en el servidor.                       |
| `TABLES`          | Muestra todas las tablas de la base de datos actualmente seleccionada.           |
| `COLUMNS`         | Muestra las columnas de una tabla específica.                                    |
| `INDEX`           | Muestra los índices definidos en una tabla específica.                           |
| `VARIABLES`       | Lista las variables de configuración del servidor y sus valores actuales.        |
| `STATUS`          | Muestra estadísticas generales del estado del servidor.                          |
| `GRANTS`          | Muestra los permisos de un usuario específico.                                   |
| `CREATE DATABASE` | Muestra la instrucción SQL utilizada para crear una base de datos.               |
| `CREATE TABLE`    | Muestra la instrucción SQL utilizada para crear una tabla.                       |

Ejemplos de uso:

```sql
-- Mostrar todas las bases de datos disponibles
SHOW DATABASES;

+--------------------+
| Database           |
+--------------------+
| empresa            |
| tienda             |
| information_schema |
+--------------------+

-- Mostrar todas las tablas en la base de datos actual
USE tienda;
SHOW TABLES;

+-------------------+
| Tables_in_tienda  |
+-------------------+
| productos         |
| clientes          |
+-------------------+

-- Mostrar las columnas de una tabla
SHOW COLUMNS FROM productos;

+----------+--------------+------+-----+---------+-------+
| Field    | Type         | Null | Key | Default | Extra |
+----------+--------------+------+-----+---------+-------+
| id       | int          | NO   | PRI | NULL    |       |
| nombre   | varchar(50)  | YES  |     | NULL    |       |
| precio   | decimal(10,2)| YES  |     | NULL    |       |
+----------+--------------+------+-----+---------+-------+

-- Mostrar los índices de una tabla
SHOW INDEX FROM productos;

+-------+------------+----------+--------------+-------------+
| Table | Non_unique | Key_name | Seq_in_index | Column_name |
+-------+------------+----------+--------------+-------------+
| productos | 0       | PRIMARY  | 1            | id          |
+-----------+----------+----------+--------------+-------------+

-- Mostrar las variables del sistema
SHOW VARIABLES;

+---------------------------------+-----------------------------+
| Variable_name                   | Value                       |
+---------------------------------+-----------------------------+
| auto_increment_increment        | 1                           |
| character_set_client            | utf8mb4                     |
| max_connections                 | 151                         |
| sql_mode                        | STRICT_TRANS_TABLES         |
+---------------------------------+-----------------------------+

-- Mostrar el estado del servidor
SHOW STATUS;

+-------------------------+---------+
| Variable_name           | Value   |
+-------------------------+---------+
| Connections             | 10      |
| Uptime                  | 3400    |
| Questions               | 220     |
+-------------------------+---------+

-- Mostrar permisos de un usuario
SHOW GRANTS FOR 'usuario'@'localhost';

+------------------------------------------------------------+
| Grants for usuario@localhost                               |
+------------------------------------------------------------+
| GRANT SELECT, INSERT ON *.* TO 'usuario'@'localhost'       |
+------------------------------------------------------------+

-- Mostrar el comando SQL para crear una BBDD
SHOW CREATE DATABASE tienda;

+----------+-------------------------------------------------+
| Database | Create Database                                 |
+----------+-------------------------------------------------+
| tienda   | CREATE DATABASE `tienda` /*!40100 DEFAULT ... */|
+----------+-------------------------------------------------+

-- Mostrar el comando SQL para crear una tabla
SHOW CREATE TABLE productos;

+-----------+-----------------------------------------------+
| Table     | Create Table                                  |
+-----------+-----------------------------------------------+
| productos | CREATE TABLE `productos` (                    |
|           |   `id` int NOT NULL,                          |
|           |   `nombre` varchar(50) DEFAULT NULL,          |
|           |   `precio` decimal(10,2) DEFAULT NULL,        |
|           |   PRIMARY KEY (`id`)                          |
|           | ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4       |
+-----------+-----------------------------------------------+
```

El comando `SHOW`:
* Proporciona información del sistema de bases de datos (_bases_, _tablas_, _columnas_, _índices_, _variables_, etc.).
* Muy útil para administrar y depurar bases de datos.

En `MySQL`, el comando `SHOW` no pertenece estrictamente a ninguna de las categorías estándar de sentencias _SQL_ (_DDL_, _DML_, _DCL_, _TCL_). Sin embargo, en el contexto de _MySQL_, se agrupa como _comando de administración del servidor_ o _comando de sesión y contexto_.

## Operador LIKE

El operador `LIKE` se utiliza en SQL para realizar búsquedas de texto basadas en patrones. Es especialmente útil para encontrar datos cuando no se conoce el valor exacto o cuando se quiere buscar coincidencias parciales.

A menudo se usa con la cláusula `WHERE` para filtrar filas de una tabla, pero también puede usarse con comandos como `SHOW VARIABLES` en MySQL.

Junto con el operador `LIKE` se pueden utilizar los comodines `%` y `_`:
* `%`: representa cualquier secuencia de caracteres (incluido cero caracteres). Útil para buscar valores que comiencen con un texto, que terminen con un texto o que contengan un texto en cualquier posición.
* `_`: representa exactamente un cáracter en una posición específica. Útil cuando se buscan valores con una longitud fija o valores con un carácter desconocido en una posición específica.

Ejemplos de uso:
* `max%`: todos los valores que comiencen por _max_.
* `%max%`: todos los valores que contengan _max_.
* `%max`: todos los valores que acaben en _max_.
* `_hon`: todos los valores que tengan 4 letras y acaben en _hon_.
* `m_x`: todos los valores que tengan 3 letras, comiencen con una _m_ y acaben con una _x_.
* `Jho_`: todos los valores que tengan 4 letras y empiecen en _Jho_.

Ejemplo de uso para filtrar variables:

```sql
-- Muestra la variable max_allowed_packet
SHOW VARIABLES LIKE 'max_allowed_packet';

+-------------------+---------+
| Variable_name     | Value   |
+-------------------+---------+
| max_allowed_packet| 4194304 |
+-------------------+---------+

-- Muestra variables que contengan la palabra size
SHOW VARIABLES LIKE '%size%';

+-----------------------------+-------------+
| Variable_name               | Value       |
+-----------------------------+-------------+
| binlog_cache_size           | 32768       |
| innodb_buffer_pool_size     | 134217728   |
| key_buffer_size             | 8388608     |
| query_cache_size            | 1048576     |
+-----------------------------+-------------+
```

## Comando SOURCE

El comando `SOURCE` en _MySQL_ se utiliza para ejecutar un archivo de script SQL desde la línea de comandos o desde el cliente MySQL. Este comando es especialmente útil para automatizar tareas, cargar grandes conjuntos de datos, o aplicar configuraciones predefinidas a una base de datos.

Uso del comando `SOURCE`
* Ejecuta un archivo que contiene comandos SQL de manera secuencial.
* Cargar y ejecutar scripts SQL almacenados en un archivo.
* Migrar bases de datos mediante la restauración de un archivo SQL generado previamente.
* Automatizar configuraciones iniciales de bases de datos.

Sintaxis del comando `SOURCE`:

```sql
SOURCE <archivo>;
```

* `<archivo>`: archivo que contiene las sentencias SQL.

Uso del comando `SOURCE` con _rutas absolutas_:

```sql
SOURCE /home/usuario/scripts/seed.sql;
```

Uso del comando `SOURCE` con _rutas relativas_:

```sql
SOURCE seed.sql;
```

Los comandos se ejecutan en el orden en que aparecen en el archivo de manera secuencial, ignorando los comentarios, es decir, aquellas líneas que comienzan con `--` o están delimitadas por `/* ... */`

Ejemplo de uso del comando `SOURCE`:

Archivo `seed.sql`:

```sql
CREATE DATABASE empresa;
USE Empresa;

CREATE TABLE empleados (
    id INT PRIMARY KEY,
    nombre VARCHAR(50),
    salario DECIMAL(10, 2)
);

INSERT INTO empleados (id, nombre, salario) VALUES (1, 'Juan', 2500.00);
INSERT INTO empleados (id, nombre, salario) VALUES (2, 'Ana', 3000.00);

SELECT * FROM empleados;
```

Comando para ejecutar el archivo `seed.sql` utilizando _rutas absolutas_:

```sql
SOURCE /home/alumno/seed.sql;
```

Comando para ejecutar el archivo `seed.sql` utilizando _rutas relativas_:

```sql
SOURCE seed.sql;
```

Resultado:

```sql
+----+-------+---------+
| id | nombre| salario |
+----+-------+---------+
| 1  | Juan  | 2500.00 |
| 2  | Ana   | 3000.00 |
+----+-------+---------+
```

Errores comunes al utilizar el comando `SOURCE`:
* `ERROR: Failed to open file 'ruta_del_archivo.sql', error: 2`: el archivo no existe o la ruta es incorrecta. Hay que revisar la ruta del archivo y los permisos del fichero.
* `ERROR 1064 (42000): You have an error in your SQL syntax;`: el archivo contiene un error de sintaxis. Hay que revisar la sintaxis del fichero.
