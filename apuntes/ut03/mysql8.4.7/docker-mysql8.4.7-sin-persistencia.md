# Desplegar un contenedor de MySQL 8.4.7 en Docker sin Persistencia

Crear y ejecutar un contenedor de MySQL con Docker:

```bash
$ docker run -d --name mysql-8.4.7-srv1 -e MYSQL_ROOT_PASSWORD=mysql8 mysql:8.4.7
Unable to find image 'mysql:8.4.7' locally
8.4.7: Pulling from mysql
6a4a3ef82cdc: Pull complete
5518b09b1089: Pull complete
b6b576315b62: Pull complete
349b52643cc3: Pull complete
abe8d2406c31: Pull complete
c7668948e14a: Pull complete
c7e93886e496: Pull complete
Digest: sha256:d6c8301b7834c5b9c2b733b10b7e630f441af7bc917c74dba379f24eeeb6a313
Status: Downloaded newer image for mysql:8.4.7
87f99c3cb1e53369dc064fd7d31afe7f7e7142983ac6743dc3c2bfe84b14bbf5
```

* `docker run -d`: crea y ejecuta el contenedor en segundo plano (modo detached).
* `--name mysql-8.4.7-srv1`: asigna el nombre _mysql-8.4.7-srv1_ al contenedor, para facilitar su administración.
* `-e MYSQL_ROOT_PASSWORD=mysql8`: configura la contraseña del usuario root de MySQL en el contenedor como mysql8.
* `mysql:8.4.7`: especifica la imagen de MySQL que Docker utilizará, en este caso, la versión _8.4.7_.

Si la imagen no está disponible localmente, Docker la descargará y luego ejecutará el contenedor, mostrando su _identificador_ (_ID_).

Verificar las imágenes de Docker disponibles:

```bash
$ docker images
REPOSITORY           TAG       IMAGE ID       CREATED         SIZE
mysql                8.4.7     1d9c2219ff69   21 months ago   496MB
```

Este comando lista todas las imágenes de Docker almacenadas en el sistema local. Cada imagen muestra:

* __REPOSITORY__: nombre del repositorio de la imagen.
* __TAG__: versión de la imagen.
* __IMAGE ID__: identificador único de la imagen.
* __CREATED__: cuándo fue creada la imagen.
* __SIZE__: tamaño de la imagen.

Debe aparecer la imagen `mysql` con la versión `8.4.7`.

Verificar los contenedores en ejecución:

```bash
$ docker ps
CONTAINER ID   IMAGE                    COMMAND                  CREATED          STATUS                    PORTS                       NAMES
87f99c3cb1e5   mysql:8.4.7              "/entrypoint.sh mysq…"   35 seconds ago   Up 34 seconds (healthy)   3306/tcp, 33060-33061/tcp   mysql-8.4.7-srv1
```

Este comando muestra todos los contenedores en ejecución y su estado. Muestra información como:

* __CONTAINER ID__: identificador del contenedor.
* __IMAGE__: imagen de la cual fue creado el contenedor.
* __COMMAND__: comando principal que el contenedor está ejecutando.
* __STATUS__: estado actual del contenedor.
* __PORTS__: puertos expuestos por el contenedor.
* __NAMES__: nombre asignado al contenedor.

El contenedor `mysql-8.4.7-srv1` debe estar en ejecución y en estado Up.

Acceder a la consola MySQL dentro del contenedor:

```bash
$ docker exec -it mysql-8.4.7-srv1 mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 8.4.7.32 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> exit
Bye
```

* `docker exec -it`: ejecuta un comando dentro del contenedor en modo interactivo.
* `mysql-8.4.7-srv1`: nombre del contenedor en el cual se ejecutará el comando.
* `mysql -u root -p`: inicia la consola de MySQL como usuario root, solicitando la contraseña configurada (_mysql8_ en este caso).

Esto nos permite acceder a la consola de MySQL para ejecutar comandos SQL.

Acceder al sistema operativo del contenedor:

```bash
$ docker exec -it mysql-8.4.7-srv1 bash
bash-4.4# mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 14
Server version: 8.4.7.32 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> exit
Bye

bash-4.4# exit
exit
```

Este comando abre una sesión de terminal (bash) dentro del contenedor, lo que permite ejecutar comandos directamente en el sistema operativo del contenedor, es decir, muestra un prompt bash dentro del contenedor con el que podemos interactuar con el sistema, ejecutar comandos de administración de MySQL o explorar el sistema de archivos del contenedor.

### Eliminar el contenedor (Opcional)

Eliminar el contenedor si ya no lo vamos a utilizar. Para ello, detenemos primero el contenedor de MySQL:

```bash
$ docker stop mysql-8.4.7-srv1
mysql-8.4.7-srv1
```

Este comando detiene el contenedor `mysql-8.4.7-srv1` sin eliminarlo. Esto es útil cuando quieres detener temporalmente el servicio sin eliminar el contenedor.

Eliminar el contenedor de MySQL si ya no lo necesitamos:

```bash
$ docker rm mysql-8.4.7-srv1
mysql-8.4.7-srv1
```

Este comando elimina el contenedor `mysql-8.4.7-srv1` del sistema. El contenedor debe estar detenido antes de poder eliminarlo.

Verificar los contenedores activos:

```bash
$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

Este comando muestra nuevamente la lista de contenedores activos. Después de eliminar el contenedor _mysql-8.4.7-srv1_, éste último no debería de estar en la lista.
