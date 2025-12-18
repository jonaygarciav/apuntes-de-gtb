# Elección de Imágenes de Docker Hub

* Introducción
* Origen y Mantenimiento
* Repositorios en Docker Hub
* Características de la imagen
* Actualizaciones
* Elección entre las dos
* Descargar imagen de Docker Hub

## Introducción

Existen dos imágenes Docker que podemos utilizar: `mysql:8.4.7` y `mysql/mysql-server:8.4.7`. Su diferencia se encuentra en su procedencia, mantenimiento y algunas configuraciones predeterminadas.

## Origen y Mantenimiento

`mysql:8.4.7`:
* __Origen__: es parte del repositorio oficial de imágenes de Docker Hub, conocido como "Official Images".
* __Mantenimiento__: es mantenida directamente por el equipo de Docker en colaboración con la comunidad de MySQL.
* __Verificación__: tiene revisiones regulares para cumplir con las pautas de seguridad de Docker Hub.

`mysql/mysql-server:8.4.7`:
* __Origen__: proviene directamente del equipo de desarrollo de MySQL en Oracle y está alojada en un repositorio específico dentro de Docker Hub.
* __Mantenimiento__: es mantenida exclusivamente por Oracle y sigue las actualizaciones del servidor MySQL publicadas por Oracle.

# Repositorios en Docker Hub

`mysql:8.4.7`: su repositorio oficial es [https://hub.docker.com/_/mysql](https://hub.docker.com/_/mysql)

`mysql/mysql-server:8.4.7`: su repositorio oficial está gestionado por Oracle y es [https://hub.docker.com/r/mysql/mysql-server](https://hub.docker.com/r/mysql/mysql-server)

## Características de la imagen

`mysql:8.4.7`:
* Es más ligera.
* Generalmente basada en Debian o Alpine, dependiendo de la etiqueta utilizada (puedes verificar con docker inspect).
* Está diseñada para ser una implementación más general, adecuada para una amplia variedad de aplicaciones y entornos.
* Tiene un soporte más amplio de la comunidad Docker, ya que es parte de las imágenes oficiales.

`mysql/mysql-server:8.4.7`:
* Puede incluir herramientas adicionales o configuraciones específicas de Oracle, lo que puede hacerla más pesada.
* Basada en una imagen mínima seleccionada por Oracle (generalmente Oracle Linux o Debian).
* Incluye configuraciones más alineadas con las prácticas recomendadas por Oracle.
* Tiene soporte directo del equipo de MySQL en Oracle, lo que puede ser preferible si ya trabajas en un entorno empresarial que utiliza productos Oracle.

## Actualizaciones

`mysql:8.4.7`:
* Se actualiza siguiendo las versiones principales MySQL pensando en la estabilidad.

`mysql/mysql-server:8.4.7`:
* Puede recibir actualizaciones más rápidas para alinearse con los lanzamientos de Oracle MySQL, lo que es ventajoso para usar las últimas características.

## Elección entre las dos

Usa `mysql:8.4.7` si:
* Necesitas una solución estándar ampliamente utilizada.
* Quieres una imagen liviana y compatible con la mayoría de los entornos.
* Prefieres aprovechar la comunidad Docker para soporte y documentación.

Usa `mysql/mysql-server:8.4.7` si:
* Quieres una imagen proporcionada directamente por Oracle para entornos empresariales.
* Necesitas configuraciones avanzadas específicas de Oracle MySQL.
* Ya estás utilizando otros productos de Oracle y quieres alinearte con su ecosistema.

# Descargar imagen de Docker Hub

`mysql:8.4.7`:

```bash
$ docker pull mysql:8.4,7
8.4.7: Pulling from library/mysql
2c0a233485c3: Pull complete
b746eccf8a0b: Pull complete
570d30cf82c5: Pull complete
c7d84c48f09d: Pull complete
e9ecf1ccdd2a: Pull complete
6331406986f7: Pull complete
f93598758d10: Pull complete
6c136cb242f2: Pull complete
d255d476cd34: Pull complete
dbfe60d9fe24: Pull complete
9cb9659be67b: Pull complete
Digest: sha256:d58ac93387f644e4e040c636b8f50494e78e5afc27ca0a87348b2f577da2b7ff
Status: Downloaded newer image for mysql:8.4.7
docker.io/library/mysql:8.4.7
```

```bash
$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
...
mysql         8.4.7       6c55ddbef969   7 weeks ago     591MB
```

`mysql/mysql-server:8.4.7`:

```bash
$ docker pull mysql/mysql-server:8.4.7
8.4.7: Pulling from mysql/mysql-server
6a4a3ef82cdc: Pull complete
5518b09b1089: Pull complete
b6b576315b62: Pull complete
349b52643cc3: Pull complete
abe8d2406c31: Pull complete
c7668948e14a: Pull complete
c7e93886e496: Pull complete
Digest: sha256:d6c8301b7834c5b9c2b733b10b7e630f441af7bc917c74dba379f24eeeb6a313
Status: Downloaded newer image for mysql/mysql-server:8.4.7
docker.io/mysql/mysql-server:8.4.7
```

```bash
$ docker images
REPOSITORY           TAG       IMAGE ID       CREATED         SIZE
...
mysql/mysql-server   8.4.7       1d9c2219ff69   22 months ago   496MB
```

* `docker pull`: descarga una imagen de Docker desde el _Docker Hub_ o de otro registro de Docker configurado.
* `mysql:8.4.7`: especifica la imagen _mysql_ con la etiqueta _4.7_. La etiqueta indica la versión específica de MySQL que se descargará.
* `mysql/mysql-server:8.4.7`: especifica la imagen _mysql/mysql-server_ con la etiqueta _8.4.7_. La etiqueta indica la versión específica de MySQL que se descargará.
