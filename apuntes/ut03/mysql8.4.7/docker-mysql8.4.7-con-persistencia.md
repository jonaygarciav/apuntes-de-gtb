# Desplegar un contenedor de MySQL 8.4.7 en Docker con Persistencia

Crear un volumen de Docker para persistencia:

```bash
$ docker volume create mysql-8.4.7-srv2-data
mysql-8.4.7-data-srv2
```
Este comando crea un volumen de Docker llamado _mysql-8.4.7-srv2-data_, que permite almacenar datos de forma persistente. Esto significa que los datos almacenados en este volumen persistirán incluso si el contenedor se elimina o se detiene.

```bash
$ docker volume ls
DRIVER    VOLUME NAME
local     mysql-8.4.7-srv2-data
```
Inspeccionar el volumen:

```bash
$ docker volume inspect mysql-8.4.7-srv2-data
[
    {
        "CreatedAt": "2024-10-30T06:59:56Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/mysql-8.4.7-srv2-data/_data",
        "Name": "mysql-8.4.7-srv2-data",
        "Options": null,
        "Scope": "local"
    }
]
```

`docker volume inspect` proporciona información detallada sobre el volumen, incluyendo:
* __Mountpoint__: ruta en el sistema de archivos donde se almacena el volumen.
* __Driver__: tipo de controlador de almacenamiento, en este caso local

Crear y ejecutar un contenedor MySQL usando el volumen:f

```bash
$ docker run -d --name mysql-8.4.7-srv2 -v mysql-8.4.7-srv2-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=mysql8  mysql:8.4.7
622c21a3e55f1eac0056c808e2b9280299b7b00377e46b3f974cd108cfec4fc5
```

Este comando ejecuta un contenedor MySQL en segundo plano:

* `-v mysql-8.4.7-svr1-data:/var/lib/mysql`: monta el volumen _mysql-8.4.7-data_ en _/var/lib/mysql_, la ruta donde MySQL almacena sus datos.
* `-e MYSQL_ROOT_PASSWORD=mysql8`: establece la contraseña para el usuario root de MySQL como _mysql8_.
* `mysql:8.4.7`: especifica la imagen de MySQL, en este caso la versión _8.4.7_.

Verificar el contenedor en ejecución:

```bash
$ docker ps
CONTAINER ID   IMAGE                    COMMAND                  CREATED          STATUS                    PORTS                                                            NAMES
622c21a3e55f   mysql:8.4.7   "/entrypoint.sh mysq…"   45 seconds ago   Up 44 seconds (healthy)   3306/tcp, 33060-33061/tcp   mysql-8.4.7-srv2
```

Este comando muestra los contenedores en ejecución. Deberías ver _mysql-8.4.7-srv2_ en la lista, indicando que el contenedor MySQL está corriendo con la configuración especificada.

Inspeccionar el contenedor para confirmar el volumen:

```bash
$ docker inspect mysql-8.4.7-srv2 | grep mounts -i -A10
        "Mounts": [
            {
                "Type": "volume",
                "Name": "mysql-8.4.7-data",
                "Source": "/var/lib/docker/volumes/mysql-8.4.7-svr1-data/_data",
                "Destination": "/var/lib/mysql",
                "Driver": "local",
                "Mode": "z",
                "RW": true,
                "Propagation": ""
            }
```

Este comando inspecciona el contenedor y muestra información sobre los volúmenes montados. Busca específicamente la sección _Mounts_ para confirmar que el volumen _mysql-8.4.7-data_ está montado en _/var/lib/mysql_.

Acceder a MySQL en el contenedor:

```bash
$ docker exec -it mysql-8.4.7-srv2 mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 12
Server version: 8.4.7.32 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> exit
Bye
```

`docker exec` ejecuta un comando en el contenedor. Este comando accede al cliente MySQL en el contenedor, utilizando el usuario root y la contraseña definida _mysql8_.

Crear y verificar una Base de Datos en MySQL, para ello, dentro de la consola de MySQL, creamos una base de datos y verificamos su persistencia:

```bash
$ docker exec -it mysql-8.4.7-srv2 mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 21
Server version: 8.4.7.32 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> CREATE DATABASE pruebas;
Query OK, 1 row affected (0.03 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| pruebas            |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

mysql> exit
Bye
```

* `CREATE DATABASE pruebas;`: crea una nueva base de datos llamada _pruebas_.
* `SHOW DATABASES;`: muestra todas las bases de datos disponibles, incluyendo la recién creada.

Detener el contendor:

```bash
$ docker stop mysql-8.4.7-srv2
mysql-8.4.7-srv2
```

Este comando detiene el contenedor MySQL _mysql-8.4.7-srv2_. Detener el contenedor no elimina el volumen, por lo que los datos permanecen intactos.

Eliminar el contenedor:

```bash
$ docker rm mysql-8.4.7-srv2
mysql-8.4.7-srv2
```

`docker rm` elimina el contenedor. Como los datos están fuera del contenedor en un volumen separado, eliminar el contenedor no afecta la persistencia del volumen.

Verificar los contenedores activos:

```bash
$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

Crear un nuevo contenedor utilizando el mismo volumen que utilizaba el anterior contenedor creado, es decir, _docker rm mysql-8.4.7-srv2-data_:

```bash
$ docker run -d --name mysql-8.4.7-srv2 -v mysql-8.4.7-srv2-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=mysql8  mysql:8.4.7
28a24a02cfdd9d3cb287234b2fcd74ce7da7e83caca07f1a709571d9d422aa9c
```

Este comando crea y ejecuta un nuevo contenedor MySQL, montando el mismo volumen _mysql-8.4.7-data_ que usaba el contenedor anterior. Al hacerlo, el nuevo contenedor accede a los datos almacenados previamente.

```bash
$ docker ps
CONTAINER ID   IMAGE                    COMMAND                  CREATED          STATUS                    PORTS                                                            NAMES
28a24a02cfdd   mysql:8.4.7   "/entrypoint.sh mysq…"   59 seconds ago   Up 58 seconds (healthy)   3306/tcp, 33060-33061/tcp   mysql-8.4.7-srv2
```

Verificar la persistencia de los datos:

```bash
$ docker exec -it mysql-8.4.7-srv2 mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.4.7.32 MySQL Community Server - GPL

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
| pruebas            |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

mysql>
```

Al acceder nuevamente a la consola de MySQL y ejecutar `SHOW DATABASES;`, vemos la base de datos `pruebas` creada anteriormente, confirmando que el volumen ha mantenido la persistencia de los datos incluso después de eliminar el contenedor, gracias al volumen `mysql-8.4.7-data`.

### Eliminar el contenedor (Opcional)

Eliminar el contenedor si ya no lo vamos a utilizar. Para ello, detenemos primero el contenedor de MySQL:

```bash
$ docker stop mysql-8.4.7-srv2
mysql-8.4.7-srv2
```

Este comando detiene el contenedor `mysql-8.4.7-srv2` sin eliminarlo. Esto es útil cuando quieres detener temporalmente el servicio sin eliminar el contenedor.

Eliminar el contenedor de MySQL si ya no lo necesitamos:

```bash
$ docker rm mysql-8.4.7-srv2
mysql-8.4.7-srv2
```

Este comando elimina el contenedor `mysql-8.4.7-srv2` del sistema. El contenedor debe estar detenido antes de poder eliminarlo.

Verificar los contenedores activos:

```bash
$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

Este comando muestra nuevamente la lista de contenedores activos. Después de eliminar el contenedor mysql-8.4.7-srv2, éste último no debería de estar en la lista.
