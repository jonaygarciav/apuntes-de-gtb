# Desplegar un contenedor de MySQL 8.0 en Docker con acceso externo al Servicio del Contenedor

Crear y ejecutar el contenedor MySQL con acceso externo:

```bash
$ docker run -d --name mysql-8.0-srv4 -p 33306:3306 -e MYSQL_ROOT_PASSWORD=mysql8 mysql/mysql-server:8.0
39a5d23773d23bcafbdd360bece4cff5a00d9e58d87c64a2cc5069650c494caa
```

* `-p 33306:3306`: mapea el puerto 3306 del contenedor al puerto 33306 en la máquina host, permitiendo que otras máquinas accedan a MySQL a través del puerto 33306.
* `-e MYSQL_ROOT_PASSWORD=mysql8`: define la contraseña del usuario root de MySQL.

Verificar el contenedor en ejecución:

```bash
$ docker ps
CONTAINER ID   IMAGE                    COMMAND                  CREATED          STATUS                    PORTS                                                            NAMES
39a5d23773d2   mysql/mysql-server:8.0   "/entrypoint.sh mysq…"   53 seconds ago   Up 52 seconds (healthy)   33060-33061/tcp, 0.0.0.0:33306->3306/tcp, [::]:33306->3306/tcp   mysql-8.0-srv4
```

Este comando muestra los contenedores en ejecución. Deberías ver que _mysql-8.0-srv4_ está activo y que el puerto 33306 está mapeado correctamente al puerto 3306 del contenedor.

Acceso al contenedor y configuración de permisos para acceso:

```bash
$ docker exec -it mysql-8.0-srv4 mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.32 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.01 sec)

mysql> use mysql
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+------------------------------------------------------+
| Tables_in_mysql                                      |
+------------------------------------------------------+
| columns_priv                                         |
| component                                            |
| db                                                   |
| default_roles                                        |
| engine_cost                                          |
| func                                                 |
| general_log                                          |
| global_grants                                        |
| gtid_executed                                        |
| help_category                                        |
| help_keyword                                         |
| help_relation                                        |
| help_topic                                           |
| innodb_index_stats                                   |
| innodb_table_stats                                   |
| ndb_binlog_index                                     |
| password_history                                     |
| plugin                                               |
| procs_priv                                           |
| proxies_priv                                         |
| replication_asynchronous_connection_failover         |
| replication_asynchronous_connection_failover_managed |
| replication_group_configuration_version              |
| replication_group_member_actions                     |
| role_edges                                           |
| server_cost                                          |
| servers                                              |
| slave_master_info                                    |
| slave_relay_log_info                                 |
| slave_worker_info                                    |
| slow_log                                             |
| tables_priv                                          |
| time_zone                                            |
| time_zone_leap_second                                |
| time_zone_name                                       |
| time_zone_transition                                 |
| time_zone_transition_type                            |
| user                                                 |
+------------------------------------------------------+
38 rows in set (0.01 sec)

mysql> select host,user from user;
+-----------+------------------+
| host      | user             |
+-----------+------------------+
| localhost | healthchecker    |
| localhost | mysql.infoschema |
| localhost | mysql.session    |
| localhost | mysql.sys        |
| localhost | root             |
+-----------+------------------+
5 rows in set (0.00 sec)

mysql> CREATE USER 'admin'@'%' IDENTIFIED WITH mysql_native_password BY 'mysql8';
Query OK, 0 rows affected (0.03 sec)

mysql> GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' WITH GRANT OPTION;
Query OK, 0 rows affected (0.03 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.02 sec)

mysql> select host,user from user;
+-----------+------------------+
| host      | user             |
+-----------+------------------+
| %         | admin            |
| localhost | healthchecker    |
| localhost | mysql.infoschema |
| localhost | mysql.session    |
| localhost | mysql.sys        |
| localhost | root             |
+-----------+------------------+
6 rows in set (0.00 sec)

mysql> exit
Bye
```

Creamos el usuario `admin` en base de datos el cual puede conectarse desde cualquier ip:

* `use mysql`: conecta a la base de datos llamada _mysql_, que es donde se encuentra la tabla _user_ con todos los usuarios.
* `show tables`: muestra todas las tablas de la base de datos _mysql_, se puede ver al final la tabla _user_.
* `SELECT host,user FROM user;`: devuelve los campos _host_ y _user_ de la tabla _user_. Se puede ver que el usuario _root_ solamente tiene acceso desde _localhost_.
* `CREATE USER 'admin'@'%' IDENTIFIED WITH mysql_native_password BY 'mysql8'`: crea un usuario _admin_ que puede conectarse desde cualquier dirección IP (%) utilizando el password _mysql8_.
* `GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;`: otorga todos los permisos al usuario _admin_ para que pueda realizar cualquier acción en todas las bases de datos.
* `FLUSH PRIVILEGES`: recarga los privilegios para que los cambios surtan efecto de inmediato.

> __Nota__: el carácter '%' es un comodín para direcciones IP o cualquier host. Cuando configuramos permisos para un usuario en MySQL, _%_ se usa como un comodín que representa cualquier dirección IP o cualquier host. Esto permite que el usuario se conecte desde cualquier lugar.

Conectarse a MySQL externamente desde el Host (Máquina Virtual):

```bash
$ mysql -h 127.0.0.1 -P 33306 -u admin -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 17
Server version: 8.0.32 MySQL Community Server - GPL

Copyright (c) 2000, 2024, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> exit
Bye
```

* `-h 127.0.0.1`: especifica 127.0.0.1 como el host, que corresponde al localhost de la máquina.
* `-P 33306`: usa el puerto 33306 (mapeado al puerto 3306 en el contenedor).
* `-u admin -p`: se conecta con el usuario admin y solicita la contraseña.

Esto prueba el acceso al servicio MySQL en el contenedor desde el host, a través del puerto mapeado.

### Eliminar el contenedor (Opcional)

Eliminar el contenedor si ya no lo vamos a utilizar. Para ello, detenemos primero el contenedor de MySQL:

```bash
$ docker stop mysql-8.0-srv4
mysql-8.0-srv4
```

Este comando detiene el contenedor `mysql-8.0-srv4` sin eliminarlo. Esto es útil cuando quieres detener temporalmente el servicio sin eliminar el contenedor.

Eliminar el contenedor de MySQL si ya no lo necesitamos:

```bash
$ docker rm mysql-8.0-srv4
mysql-8.0-srv4
```

Este comando elimina el contenedor `mysql-8.0-srv4` del sistema. El contenedor debe estar detenido antes de poder eliminarlo.

Verificar los contenedores activos:

```bash
$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

Este comando muestra nuevamente la lista de contenedores activos. Después de eliminar el contenedor mysql-8.0-srv4, éste último no debería de estar en la lista.
