# Desplegar un contenedor de MySQL 8.0 en Docker compartiendo una carpeta entre el Host y el Contenedor

Comprobar el directorio actual y crear la carpeta _scripts_:

```bash
$ pwd
/home/alumno
```

```bash
$ mkdir scripts
```

Verificar la nueva carpeta:

```bash
$ ls -l
total 24
drwxrwxr-x 2 alumno alumno  4096 oct 30 07:14 scripts
```
Crear y ejecutar el contenedor MySQL con un volumen de enlace:

```bash
$ docker run -d --name mysql-8.0-srv3 -v ./scripts:/scripts -e MYSQL_ROOT_PASSWORD=mysql8 mysql/mysql-server:8.0
18a841a174512de7b640cf313cd2da096430a7ca9c60f0b530c37bf122e657cf
```

* `-v ./scripts:/scripts`: crea un volumen de enlace entre el directorio _scripts_ en el sistema host y _/scripts_ dentro del contenedor.
* `-e MYSQL_ROOT_PASSWORD=mysql8`: configura la contraseña de root para MySQL.

Verificar el contenedor en ejecución:

```bash
$ docker ps
CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS                   PORTS                  NAMES
18a841a17451   mysql/mysql-server:8.0   "/entrypoint.sh mysq…"   2 minutes ago   Up 2 minutes (healthy)   [::]:33306->3306/tcp   mysql-8.0-srv3
```

El volumen de enlace permite que cualquier archivo creado en _/scripts_ dentro del contenedor esté disponible en la carpeta _scripts_ en el sistema host y viceversa, facilitando el acceso a scripts o archivos de configuración externos.

Inspeccionar el montaje del volumen en el contenedor:

```bash
$ docker  inspect mysql-8.0-srv3 | grep mounts -i -A10
        "Mounts": [
            {
                "Type": "bind",
                "Source": "/home/alumno/scripts",
                "Destination": "/scripts",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ],
        "Config": {
```

* __Mounts__: esta sección muestra la configuración de montaje del volumen. Verifica que el volumen de enlace _./scripts_ en el host esté montado en _/scripts_ en el contenedor.

Verificar la estructura de archivos en el contenedor:

```bash
$ docker exec mysql-8.0-srv3 ls -l /
total 80
lrwxrwxrwx   1 root root    7 Oct  9  2021 bin -> usr/bin
dr-xr-xr-x   2 root root 4096 Oct  9  2021 boot
drwxr-xr-x   5 root root  340 Oct 30 07:15 dev
drwxr-xr-x   2 root root 4096 Jan 18  2023 docker-entrypoint-initdb.d
-rwxr-xr-x   1 root root 8544 Jan 18  2023 entrypoint.sh
drwxr-xr-x   1 root root 4096 Oct 30 07:15 etc
-rwxr-xr-x   1 root root 1098 Jan 18  2023 healthcheck.sh
drwxr-xr-x   2 root root 4096 Oct  9  2021 home
lrwxrwxrwx   1 root root    7 Oct  9  2021 lib -> usr/lib
lrwxrwxrwx   1 root root    9 Oct  9  2021 lib64 -> usr/lib64
drwxr-xr-x   2 root root 4096 Oct  9  2021 media
drwxr-xr-x   2 root root 4096 Oct  9  2021 mnt
drwxr-xr-x   2 root root 4096 Oct  9  2021 opt
dr-xr-xr-x 191 root root    0 Oct 30 07:15 proc
dr-xr-x---   2 root root 4096 Oct  9  2021 root
drwxr-xr-x   1 root root 4096 Jan 18  2023 run
lrwxrwxrwx   1 root root    8 Oct  9  2021 sbin -> usr/sbin
drwxrwxr-x   2 1000 1000 4096 Oct 30 07:14 scripts
drwxr-xr-x   2 root root 4096 Oct  9  2021 srv
dr-xr-xr-x  13 root root    0 Oct 30 07:15 sys
drwxrwxrwt   1 root root 4096 Oct 30 07:15 tmp
drwxr-xr-x   1 root root 4096 Jan 16  2023 usr
drwxr-xr-x   1 root root 4096 Jan 16  2023 var
```

Este comando lista los archivos y directorios en el contenedor, y deberías ver la carpeta _/scripts_ montada. Los permisos de usuario indican que el volumen se montó correctamente desde el sistema host.

### Eliminar el contenedor y el volumen (Opcional)

Eliminar el contenedor si ya no lo vamos a utilizar. Para ello, detenemos primero el contenedor de MySQL:

```bash
$ docker stop mysql-8.0-srv3
mysql-8.0-srv3
```

Este comando detiene el contenedor `mysql-8.0-srv3` sin eliminarlo. Esto es útil cuando quieres detener temporalmente el servicio sin eliminar el contenedor.

Eliminar el contenedor de MySQL si ya no lo necesitamos:

```bash
$ docker rm mysql-8.0-srv3
mysql-8.0-srv3
```

Este comando elimina el contenedor `mysql-8.0-srv3` del sistema. El contenedor debe estar detenido antes de poder eliminarlo.

Verificar los contenedores activos:

```bash
$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

Este comando muestra nuevamente la lista de contenedores activos. Después de eliminar el contenedor mysql-8.0-srv3, éste último no debería de estar en la lista.
