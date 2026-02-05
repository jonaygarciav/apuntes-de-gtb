# Motores de Almacenamiento (Engines)

* Introducción
* Comando `SHOW ENGINES`
* InnoDB
* MyISAM

## Introducción

Los __engines__ o _motores de almacenamiento_ son componentes de MySQL que manejan las operaciones SQL para diferentes tipos de tablas. `InnoDB` es el motor de almacenamiento predeterminado y de propósito general, y Oracle recomienda usarlo para las tablas excepto en casos de uso especializados.

## Comando SHOW ENGINES

Para determinar qué motores de almacenamiento soporta nuestro servidor MySQL se utiliza el comando `SHOW ENGINES`.

```sql
-- Opción 1:
mysql> SHOW ENGINES;
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| Engine             | Support | Comment                                                        | Transactions | XA   | Savepoints |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| ndbcluster         | NO      | Clustered, fault-tolerant tables                               | NULL         | NULL | NULL       |
| MEMORY             | YES     | Hash based, stored in memory, useful for temporary tables      | NO           | NO   | NO         |
| InnoDB             | DEFAULT | Supports transactions, row-level locking, and foreign keys     | YES          | YES  | YES        |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                                             | NO           | NO   | NO         |
| MyISAM             | YES     | MyISAM storage engine                                          | NO           | NO   | NO         |
| FEDERATED          | NO      | Federated MySQL storage engine                                 | NULL         | NULL | NULL       |
| ndbinfo            | NO      | MySQL Cluster system information storage engine                | NULL         | NULL | NULL       |
| MRG_MYISAM         | YES     | Collection of identical MyISAM tables                          | NO           | NO   | NO         |
| BLACKHOLE          | YES     | /dev/null storage engine (anything you write to it disappears) | NO           | NO   | NO         |
| CSV                | YES     | CSV storage engine                                             | NO           | NO   | NO         |
| ARCHIVE            | YES     | Archive storage engine                                         | NO           | NO   | NO         |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
11 rows in set (0,00 sec)

-- Opción 2:
mysql> SELECT * FROM INFORMATION_SCHEMA.ENGINES;
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| ENGINE             | SUPPORT | COMMENT                                                        | TRANSACTIONS | XA   | SAVEPOINTS |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| ndbcluster         | NO      | Clustered, fault-tolerant tables                               | NULL         | NULL | NULL       |
| MEMORY             | YES     | Hash based, stored in memory, useful for temporary tables      | NO           | NO   | NO         |
| InnoDB             | DEFAULT | Supports transactions, row-level locking, and foreign keys     | YES          | YES  | YES        |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                                             | NO           | NO   | NO         |
| MyISAM             | YES     | MyISAM storage engine                                          | NO           | NO   | NO         |
| FEDERATED          | NO      | Federated MySQL storage engine                                 | NULL         | NULL | NULL       |
| ndbinfo            | NO      | MySQL Cluster system information storage engine                | NULL         | NULL | NULL       |
| MRG_MYISAM         | YES     | Collection of identical MyISAM tables                          | NO           | NO   | NO         |
| BLACKHOLE          | YES     | /dev/null storage engine (anything you write to it disappears) | NO           | NO   | NO         |
| CSV                | YES     | CSV storage engine                                             | NO           | NO   | NO         |
| ARCHIVE            | YES     | Archive storage engine                                         | NO           | NO   | NO         |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
```

* __Engine__: el nombre del motor de almacenamiento disponible en MySQL. Cada motor tiene características y usos específicos.
* __Support__: indica si el motor está soportado en el servidor actual: `YES` (disponible), `NO` (no disponible), o `DEFAULT` (motor predeterminado).
* __Comment__: una breve descripción del motor, explicando su propósito o características principales.
* __Transactions__: indica si el motor soporta transacciones ACID (como commit, rollback y consistencia de datos). `YES` significa que las soporta.
* __XA__: muestra si el motor soporta transacciones distribuidas XA, que permiten coordinar operaciones entre múltiples bases de datos.
* __Savepoints__: indica si el motor soporta puntos de guardado (savepoints) dentro de transacciones, lo que permite volver a un estado anterior dentro de la misma transacción.

A continuación, una tabla comparativa de los principales `engines` de MySQL:

| **Feature**                           | **MyISAM** | **Memory** | **InnoDB** | **Archive** | **NDB** |
|---------------------------------------|------------|------------|------------|-------------|---------|
| B-tree indexes                        | Yes        | Yes        | Yes        | No          | No      |
| Backup/point-in-time recovery         | Yes        | Yes        | Yes        | Yes         | Yes     |
| Cluster database support              | No         | No         | No         | No          | Yes     |
| Clustered indexes                     | No         | No         | Yes        | No          | No      |
| Compressed data                       | Yes        | No         | Yes        | Yes         | No      |
| Data caches                           | No         | N/A        | Yes        | No          | Yes     |
| Encrypted data                        | Yes        | Yes        | Yes        | Yes         | Yes     |
| Foreign key support                   | No         | No         | Yes        | No          | Yes     |
| Full-text search indexes              | Yes        | No         | Yes        | No          | No      |
| Geospatial data type support          | Yes        | No         | Yes        | Yes         | Yes     |
| Geospatial indexing support           | Yes        | No         | Yes        | No          | No      |
| Hash indexes                          | No         | Yes        | No         | No          | Yes     |
| Index caches                          | Yes        | N/A        | Yes        | No          | Yes     |
| Locking granularity                   | Table      | Table      | Row        | Row         | Row     |
| MVCC                                  | No         | No         | Yes        | No          | No      |
| Replication support                   | Yes        | Limited    | Yes        | Yes         | Yes     |
| Storage limits                        | 256TB      | RAM        | 64TB       | None        | 384EB   |
| T-tree indexes                        | No         | No         | No         | No          | Yes     |
| Transactions                          | No         | No         | Yes        | No          | Yes     |
| Update statistics for data dictionary | Yes        | Yes        | Yes        | Yes         | Yes     |


## InnoDB

`InnoDB` es un motor de almacenamiento de propósito general que equilibra alta confiabilidad y alto rendimiento. En MySQL 8.0, InnoDB es el motor de almacenamiento predeterminado. A menos que configuremos un motor de almacenamiento diferente, al emitir una instrucción `CREATE TABLE` sin una cláusula `ENGINE`, se creará una tabla `InnoDB`.

Ventajas Clave de InnoDB: 

* __Modelo ACID__: las operaciones DML (Data Manipulation Language) siguen el modelo ACID, lo que incluye transacciones con capacidades de commit, rollback y recuperación tras fallos para proteger los datos del usuario.
* __Bloqueo a nivel de gila y lecturas consistentes__: aumenta la concurrencia de múltiples usuarios y el rendimiento.
* __Índices agrupados (clustered indexes)__: las tablas InnoDB organizan los datos en disco para optimizar las consultas basadas en claves primarias. Cada tabla tiene un índice agrupado que organiza los datos para minimizar las operaciones de I/O en búsquedas de claves primarias.
* __Integridad de datos con claves foráneas__: InnoDB soporta restricciones de claves foráneas, verificando las operaciones de inserción, actualización y eliminación para evitar inconsistencias entre tablas relacionadas.

Características del motor de almacenamiento `InnoDB`:

| **Característica**                    | **Soporte**                                                                              |
|---------------------------------------|------------------------------------------------------------------------------------------|
| B-tree indexes                        | Sí                                                                                       |
| Backup/point-in-time recovery         | Sí (Implementado en el servidor, no en el motor de almacenamiento.)                      |
| Cluster database support              | No                                                                                       |
| Clustered indexes                     | Sí                                                                                       |
| Compressed data                       | Sí                                                                                       |
| Data caches                           | Sí                                                                                       |
| Encrypted data                        | Sí (Implementado en el servidor; soporte para cifrado en reposo desde MySQL 5.7.)        |
| Foreign key support                   | Sí                                                                                       |
| Full-text search indexes              | Sí (Disponible desde MySQL 5.6.)                                                         |
| Geospatial data type support          | Sí                                                                                       |
| Geospatial indexing support           | Sí (Disponible desde MySQL 5.7.)                                                         |
| Hash indexes                          | No (InnoDB utiliza índices hash internamente para su funcionalidad Adaptive Hash Index.) |
| Index caches                          | Sí                                                                                       |
| Locking granularity                   | Fila                                                                                     |
| MVCC                                  | Sí                                                                                       |
| Replication support                   | Sí (Implementado en el servidor, no en el motor de almacenamiento.)                      |
| Storage limits                        | 64TB                                                                                     |
| T-tree indexes                        | No                                                                                       |
| Transactions                          | Sí                                                                                       |
| Update statistics for data dictionary | Sí                                                                                       |

Casos de uso principales de InnoDB:
* __Aplicaciones que requieren transacciones ACID__:
    * _A (Atomicidad)_: si una operación necesita varios pasos para ser completada se ejecutan todos los pasos o ninguno. Configurar Autocommit, usar el comando COMMIT y ROLLBACK, los datos operativos de las tablas INFORMATION_SCHEMA.
    * _C (Consistencia)_: consiste en mantener la integridad de los datos de la base de datos. Usando el buffer de doble escritura de InnoDB y usando la recuperación de bloqueo deInnoDB.
    * _I (Aislamiento)_: una operación no puede afectar a otras operaciones. Configurando el Autocommit, establecer SET ISOLATION LEVEL sentencia.
    * _D (Durabilidad o persistencia)_: es la propiedad que asegura que una vez realizada la operación, ésta persistirá. Para conseguir esta carecterísitca es necesario tener sistemas de backup, unidades SAI por si hay apagones de electricidad
* __Bases de datos con altos niveles de concurrencia__: gracias al bloqueo a nivel de fila y a la concurrencia controlada mediante MVCC, InnoDB ofrece un rendimiento superior en entornos con muchos usuarios realizando consultas y transacciones simultáneamente.
* __Tablas con relaciones complejas__: soporta claves foráneas para garantizar la integridad referencial entre tablas relacionadas. Es útil en aplicaciones que necesitan asegurar consistencia entre registros relacionados, como sistemas de gestión de inventarios o ERPs.
* __Consultas basadas en claves primarias__: con índices agrupados (clustered indexes), InnoDB organiza los datos en disco para optimizar consultas basadas en claves primarias, reduciendo el número de operaciones de I/O necesarias.
* __Recuperación ante Fallos__: InnoDB es ideal para aplicaciones que necesitan proteger los datos contra fallos del sistema, ya que incluye recuperación automática después de un fallo utilizando su registro de transacciones.

Limitaciones de InnoDB: 
* __Rendimiento inferior en lecturas masivas__: en sistemas de solo lectura o con consultas predominantemente de lectura, InnoDB puede ser más lento que MyISAM debido al uso de bloqueos y sobrecarga de transacciones.
* __Consumo de memoria y espacio__: InnoDB requiere más memoria para su buffer pool, utilizado para cachear datos e índices. Además, las tablas InnoDB ocupan más espacio en disco que otros motores como MyISAM debido a su estructura transaccional.
* __Limitación de claves foráneas en ciertas condiciones__: no permite claves foráneas si las tablas involucradas no están en el mismo motor. Además, las restricciones de claves foráneas pueden agregar complejidad al diseño y mantenimiento.
* __Sin compresión avanzada (por defecto)__: aunque InnoDB soporta compresión, requiere configuraciones adicionales y puede no ser tan eficiente como otras soluciones especializadas para almacenamiento comprimido.
* __Rendimiento dependiente de la configuración__: InnoDB requiere una configuración adecuada (como innodb_buffer_pool_size) para obtener un rendimiento óptimo. Configuraciones por defecto pueden ser insuficientes para cargas de trabajo intensivas.
* __Menor Velocidad en Full-Text Search__: aunque soporta índices de texto completo, pueden no ser tan rápidos o eficientes como las implementaciones especializadas.
* __Sin soporte para ciertas funcionalidades__: no soporta particionamiento basado en sub-tablas como otros motores especializados.

## MyISAM

`MyISAM` está basado en el motor de almacenamiento más antiguo ISAM (ya no disponible) pero cuenta con muchas extensiones útiles.

Características del motor de almacenamiento MyISAM:

| **Característica**                    | **Soporte**                                                                    |
|---------------------------------------|--------------------------------------------------------------------------------|
| B-tree indexes                        | Sí                                                                             |
| Backup/point-in-time recovery         | Sí (Implementado en el servidor, no en el motor de almacenamiento.)            |
| Cluster database support              | No                                                                             |
| Clustered indexes                     | No                                                                             |
| Compressed data                       | Sí (Solo para tablas con formato de fila comprimido, que son de solo lectura.) |
| Data caches                           | No                                                                             |
| Encrypted data                        | Sí (Implementado en el servidor mediante funciones de cifrado.)                |
| Foreign key support                   | No                                                                             |
| Full-text search indexes              | Sí                                                                             |
| Geospatial data type support          | Sí                                                                             |
| Geospatial indexing support           | Sí                                                                             |
| Hash indexes                          | No                                                                             |
| Index caches                          | Sí                                                                             |
| Locking granularity                   | Tabla                                                                          |
| MVCC                                  | No                                                                             |
| Replication support                   | Sí (Implementado en el servidor, no en el motor de almacenamiento.)            |
| Storage limits                        | 256TB                                                                          |
| T-tree indexes                        | No                                                                             |
| Transactions                          | No                                                                             |
| Update statistics for data dictionary | Sí                                                                             |

Características adicionales de MyISAM:
* __Estructura de archivos__: cada tabla MyISAM se almacena en disco en dos archivos: .MYD (datos) y .MYI (índices). La definición de la tabla se guarda en el diccionario de datos de MySQL.
* __Capacidades de Índices__: soporta índices BLOB y TEXT. Soporta valores NULL en columnas indexadas, ocupando entre 0 y 1 byte por clave.
* __Límites de Almacenamiento__: Tamaño máximo de archivo: 256TB en sistemas que lo soporten.
* __Máximo de índices por tabla__: 64.
* __Máximo de columnas por índice__: 16.
* __Longitud máxima de clave__: 1000 bytes (ajustable con recompilación del código fuente).
* __Auto_increment y Gestión de espacios libres__: admite una columna AUTO_INCREMENT por tabla.

Casos de uso principales:
* Adecuado para tablas de solo lectura o lectura intensiva.
* Útil para aplicaciones web, almacenamiento de logs o análisis de datos.

Limitaciones:
* No soporta transacciones.
* No admite claves foráneas.
* No tiene soporte para particionamiento.
