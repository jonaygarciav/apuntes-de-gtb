# Elección de Imágenes de Docker Hub

* Introducción
* Origen y Mantenimiento
* Repositorios en Docker Hub
* Características de la imagen
* Actualizaciones
* Elección entre las dos
* Descargar imagen de Docker Hub

## Introducción

Existen dos imágenes Docker que podemos utilizar: `mysql` y `mysql/mysql-server`. Su diferencia se encuentra en su procedencia, mantenimiento y algunas configuraciones predeterminadas.

> __Nota__: la imagen `mysql/mysql-server` ha quedado deprecada a la versión 8.0

## Origen y Mantenimiento

`mysql:8.4.7`:
* __Origen__: es parte del repositorio oficial de imágenes de Docker Hub, conocido como "Official Images".
* __Mantenimiento__: es mantenida directamente por el equipo de Docker en colaboración con la comunidad de MySQL.
* __Verificación__: tiene revisiones regulares para cumplir con las pautas de seguridad de Docker Hub.

# Repositorios en Docker Hub

`mysql:8.4.7`: su repositorio oficial es [https://hub.docker.com/_/mysql](https://hub.docker.com/_/mysql)

## Características de la imagen

`mysql:8.4.7`:
* Es más ligera.
* Generalmente basada en Debian o Alpine, dependiendo de la etiqueta utilizada (puedes verificar con docker inspect).
* Está diseñada para ser una implementación más general, adecuada para una amplia variedad de aplicaciones y entornos.
* Tiene un soporte más amplio de la comunidad Docker, ya que es parte de las imágenes oficiales.

## Actualizaciones

`mysql:8.4.7`:
* Se actualiza siguiendo las versiones principales MySQL pensando en la estabilidad.

## Elección entre las dos

Usa `mysql:8.4.7` si:
* Necesitas una solución estándar ampliamente utilizada.
* Quieres una imagen liviana y compatible con la mayoría de los entornos.
* Prefieres aprovechar la comunidad Docker para soporte y documentación.

# Descargar imagen de Docker Hub

`mysql:8.4.7`:

```bash
$ docker pull mysql:8.4.7
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


* `docker pull`: descarga una imagen de Docker desde el _Docker Hub_ o de otro registro de Docker configurado.
* `mysql:8.4.7`: especifica la imagen _mysql_ con la etiqueta _4.7_. La etiqueta indica la versión específica de MySQL que se descargará.
