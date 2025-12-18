# Desplegar un contenedor de MySQL 8.0 en Docker cargando un fichero SQL cuando arranca por primera vez el contenedor

Vamos a crear un fichero llamado `seed.sql` el cual queremos que se cargue cuando ejecutemos por primera vez el contenedor.

```bash
$ cat seed.sql
CREATE DATABASE IF NOT EXISTS testdb;
USE testdb;

CREATE TABLE IF NOT EXISTS alumnos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50),
    apellidos VARCHAR(50),
    email VARCHAR(100),
    telefono VARCHAR(15)
);

INSERT INTO alumnos (nombre, apellidos, email, telefono) VALUES
    ('Juan', 'Perez Dominguez', 'juan.perez@gmail.com', '1234567890'),
    ('Maria', 'Lopez Cano', 'maria.lopez@gmail.com', '2345678901'),
    ('Carlos', 'Ramirez Nuñez', 'carlos.ramirez@gmail.com', '3456789012'),
    ('Ana', 'Morales Rios', 'ana.morales@gmail.com', '4567890123'),
    ('Pedro', 'Rodriguez Marin', 'pedro.rodriguez@gmail.com', '5678901234'),
    ('Sofia', 'Mendez Santos', 'sofia.mendez@gmail.com', '6789012345'),
    ('Luis', 'Fernandez Ruiz', 'luis.fernandez@gmail.com', '7890123456'),
    ('Laura', 'Castillo Fuentes', 'laura.castillo@gmail.com', '8901234567'),
    ('David', 'Torres Molina', 'david.torres@gmail.com', '9012345678'),
    ('Marta', 'Ramos Pardo', 'marta.ramos@gmail.com', '0123456789');

$ ls -l
total 24
-rw-rw-r-- 1 alumno alumno  1020 nov 28 09:36 seed.sql
```

Para que cargue este fichero, es necesario compartir el fichero `seed.sql` dentro del directorio `/docker-entrypoint-initdb.d` del contenedor:
* `-v ./seed.sql:/docker-entrypoint-initdb.d/seed.sql`: copia el fichero `seed.sql` dentro del directorio `/docker-entrypoint-initdb.d` del contenedor.

Crear y ejecutar el contenedor MySQL que cargue el fichero SQL:

```bash
$ docker run -d --name mysql-8.0-srv5 -v ./seed.sql:/docker-entrypoint-initdb.d/seed.sql -e MYSQL_ROOT_PASSWORD=mysql8 mysql/mysql-server:8.0
18a841a174512de7b640cf313cd2da096430a7ca9c60f0b530c37bf122e657cf
```

Nos conectamos al contenedor:

```bash
$ docker exec -it mysql-8.0-srv5 mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 8.0.32 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> exit
Bye
```

Comprobamos que la carga del fichero SQL se ha realizado correctamente:

```sql
-- Mostrar las bases de datos creadas
mysql> SHOW databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| testdb             |
+--------------------+
5 rows in set (0,01 sec)

-- Conectar a la base de datos testdb
mysql> USE testdb;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed

-- Mostrar todas las tablas de la base de datos testdb
mysql> SHOW tables;
+------------------+
| Tables_in_testdb |
+------------------+
| alumnos          |
+------------------+
1 row in set (0,00 sec)

-- Mostrar la configuración de la tabla alumnos
mysql> DESCRIBE alumnos;
+-----------+--------------+------+-----+---------+----------------+
| Field     | Type         | Null | Key | Default | Extra          |
+-----------+--------------+------+-----+---------+----------------+
| id        | int          | NO   | PRI | NULL    | auto_increment |
| nombre    | varchar(50)  | YES  |     | NULL    |                |
| apellidos | varchar(50)  | YES  |     | NULL    |                |
| email     | varchar(100) | YES  |     | NULL    |                |
| telefono  | varchar(15)  | YES  |     | NULL    |                |
+-----------+--------------+------+-----+---------+----------------+
5 rows in set (0,00 sec)

-- Mostrar todos los registros de la tabla alumnos
mysql> SELECT * FROM alumnos;
+----+--------+------------------+---------------------------+------------+
| id | nombre | apellidos        | email                     | telefono   |
+----+--------+------------------+---------------------------+------------+
|  1 | Juan   | Perez Dominguez  | juan.perez@gmail.com      | 1234567890 |
|  2 | Maria  | Lopez Cano       | maria.lopez@gmail.com     | 2345678901 |
|  3 | Carlos | Ramirez Nuñez    | carlos.ramirez@gmail.com  | 3456789012 |
|  4 | Ana    | Morales Rios     | ana.morales@gmail.com     | 4567890123 |
|  5 | Pedro  | Rodriguez Marin  | pedro.rodriguez@gmail.com | 5678901234 |
|  6 | Sofia  | Mendez Santos    | sofia.mendez@gmail.com    | 6789012345 |
|  7 | Luis   | Fernandez Ruiz   | luis.fernandez@gmail.com  | 7890123456 |
|  8 | Laura  | Castillo Fuentes | laura.castillo@gmail.com  | 8901234567 |
|  9 | David  | Torres Molina    | david.torres@gmail.com    | 9012345678 |
| 10 | Marta  | Ramos Pardo      | marta.ramos@gmail.com     | 0123456789 |
+----+--------+------------------+---------------------------+------------+
10 rows in set (0,00 sec)
```

### Eliminar el contenedor (Opcional)

Eliminar el contenedor si ya no lo vamos a utilizar. Para ello, detenemos primero el contenedor de MySQL:

```bash
$ docker stop mysql-8.0-srv5
mysql-8.0-srv5
```

Este comando detiene el contenedor `mysql-8.0-srv5` sin eliminarlo. Esto es útil cuando quieres detener temporalmente el servicio sin eliminar el contenedor.

Eliminar el contenedor de MySQL si ya no lo necesitamos:

```bash
$ docker rm mysql-8.0-srv5
mysql-8.0-srv5
```

Este comando elimina el contenedor `mysql-8.0-srv5` del sistema. El contenedor debe estar detenido antes de poder eliminarlo.

Verificar los contenedores activos:

```bash
$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

Este comando muestra nuevamente la lista de contenedores activos. Después de eliminar el contenedor _mysql-8.0-srv5_, éste último no debería de estar en la lista.
