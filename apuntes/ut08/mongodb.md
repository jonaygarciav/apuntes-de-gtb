# MongoDB

* [Introducción](#introducción)
* [MongoDB](#mongodb-1)
  * [Instalación](#instalación)
  * [Elementos de MongoDB](#elementos-de-mongodb)
  * [Creación de BBDD](#creación-de-bbdd)
  * [Creación de colecciones y documentos](#creación-de-colecciones-y-documentos)


## Introducción

El gestor de base de datos MongoDB se lo puede asociar a un conjunto de gestores de bases de datos que no tienen como lenguaje principal el SQL para su manipulación.

Los gestores de bases de datos NoSQL no requieren estructuras fijas como tablas, normalmente no soportan operaciones join y presentan como gran ventaja que pueden escalar en forma sencilla.

Ventajas:

* Permiten escalar en forma sencilla.
* Tienen un lenguaje adaptada al modelo de datos que implementa el gestor de base de datos.
* Permiten administrar grandes cantidades de datos no estructurados.

Desventajas:

* No hay al momento gran cantidad de desarrolladores que conozcan este tipo de gestores de bases de datos, a diferencia a lo que ocurre con los gestores clásicos como Oracle, SQL Server, MySQL, etc.
* La compatibilidad entre los distintos gestores de bases de datos NoSQL es nula.
* Los gestores NoSQL son una tecnología relativamente nueva por lo que le falta alguna madurez a algunos de ellos.
* La cantidad de herramientas para administrarlos por el momento es muy limitado.

Los gestores NoSQL más destacados:

* MongoDB
* Cassandra
* redis
* CouchDB
* Amazon SimpleDB
* IBM Informix
* Elasticsearch
* Hadoop
* Cloud Datastore (Google)

Estos y muchos otros gestores de bases de datos son alternativas a los gestores de bases de datos relacionales tradicionales (SQL Server, MySQL, Oracle etc.) y pretenden superarlos en velocidad de acceso a los datos, manejo de grandes volúmenes de datos y la posibilidad de tener sistemas distribuidos.

Por el momento no hay un claro ganador en este movimiento de los gestores de bases de datos NoSQL, pero podemos ver que grandes empresas como Google, Amazon, Ibm etc. están invirtiendo en este sector de la tecnología.

## MongoDB

El sitio oficial de MongoDB define a su producto: "MongoDB es una base de datos de documentos que ofrece una gran escalabilidad y flexibilidad, y un modelo de consultas e indexación avanzado."

Sus características son:

* Almacena datos en documentos JSON,es decir, cada documento puede contener diferentes campos y las estructuras de datos se pueden ir modificando.
* Es una base de datos distribuida, por lo que es fácil de usar y proporciona una elevada disponibilidad, escalabilidad horizontal y distribución geográfica.
* Es una base de datos de código abierto y de uso gratuito.

Las versiones LTS (Long Term Support) de MongoDB siguen un ciclo bastante claro. Tienen un soporte aproximado de 3 años desde su lanzamiento:

Se divide en dos fases:

1. Soporte activo (≈ 1 año)
    * Nuevas funcionalidades menores
    * Mejoras de rendimiento
    * Corrección de errores (bugs)
    * Actualizaciones de seguridad

2. Soporte de mantenimiento / crítico (≈ 2 años)
    * Solamente parches críticos y de seguridad
    * No se añaden nuevas funcionalidades

Versiones `LTS` de MongoDB:

| Año        | Versión MongoDB | Tipo            | Fin de ciclo de vida (EOL) |
|------------|----------------|-----------------|----------------------------|
| 2024       | 8.0            | LTS             | ~ 2027                     |
| 2023       | 7.0            | LTS             | ~ 2026                     |
| 2022       | 6.0            | LTS             | ~ 2025                     |
| 2021-2022  | 5.0            | LTS             | ~ 2024                     |
| 2020       | 4.4            | LTS             | ~ 2023                     |
| 2019       | 4.2            | LTS             | ~ 2022                     |
| 2018       | 4.0            | LTS             | ~ 2021                     |
| 2017       | 3.6            | LTS             | ~ 2020                     |
| 2016       | 3.4            | LTS             | ~ 2019                     |
| 2015 (T)   | 3.2            | LTS             | ~ 2018                     |
| 2015 (E)   | 3.0            | LTS             | ~ 2018                     |

### Instalación

MongoDB es multiplataforma y se puede instalar en sistemas Windows, Linux o mac OS. En la documentación oficial tienen una guía de instalación para cada Sistema Operativo:

[https://www.mongodb.com/es/docs/v8.0/administration/install-community](https://www.mongodb.com/es/docs/v8.0/administration/install-community)

En este caso instalaremos la versión de MongoDB 8.2 para Ubuntu 24.04:

```
$ curl -fsSL https://pgp.mongodb.com/server-8.0.asc | \
sudo gpg -o /usr/share/keyrings/mongodb-server-8.0.gpg \
--dearmor

$ echo "deb [ arch=amd64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.org/apt/ubuntu noble/mongodb-org/8.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-8.2.list

$ sudo apt-get update
$ apt-get install -y mongodb-org
```

```
$ sudo systemctl start mongod

$ systemctl status mongod
● mongod.service - MongoDB Database Server
     Loaded: loaded (/usr/lib/systemd/system/mongod.service; disabled; preset: enabled)
     Active: active (running) since Tue 2026-05-05 10:09:34 UTC; 2s ago
       Docs: https://docs.mongodb.org/manual
   Main PID: 3617 (mongod)
     Memory: 99.5M (peak: 99.6M)
        CPU: 175ms
     CGroup: /system.slice/mongod.service
             └─3617 /usr/bin/mongod --config /etc/mongod.conf

may 05 10:09:34 ubuntu systemd[1]: Started mongod.service - MongoDB Database Server.
may 05 10:09:34 ubuntu mongod[3617]: {"t":{"$date":"2026-05-05T10:09:34.286+00:00"},"s":"I",  "c":"-",        "id":8991200, "ctx":"main","msg":"Shuffling initializers","attr":{"seed":774886971}}
may 05 10:09:34 ubuntu mongod[3617]: {"t":{"$date":"2026-05-05T10:09:34.288+00:00"},"s":"I",  "c":"CONTROL",  "id":7484500, "ctx":"main","msg":"Environment variable MONGODB_CONFIG_OVERRIDE_NOFORK ==
```

Para acceder por terminal a través del cliente:

```
$ mongosh
Current Mongosh Log ID: 69f9c229a490d5816e9df8a2
Connecting to:          mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.8.3
Using MongoDB:          8.2.7
Using Mongosh:          2.8.3

For mongosh info see: https://www.mongodb.com/docs/mongodb-shell/


To help improve our products, anonymous usage data is collected and sent to MongoDB periodically (https://www.mongodb.com/legal/privacy-policy).
You can opt-out by running the disableTelemetry() command.

------
   The server generated these startup warnings when booting
   2026-05-05T10:09:34.478+00:00: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
   2026-05-05T10:09:34.567+00:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
   2026-05-05T10:09:34.567+00:00: For customers running the current memory allocator, we suggest changing the contents of the following sysfsFile
   2026-05-05T10:09:34.567+00:00: For customers running the current memory allocator, we suggest changing the contents of the following sysfsFile
   2026-05-05T10:09:34.567+00:00: We suggest setting the contents of sysfsFile to 0.
   2026-05-05T10:09:34.567+00:00: We suggest setting swappiness to 0 or 1, as swapping can cause performance problems.
------

test>
```

### Elementos de MongoDB

Los elementos que podemos encontrar en MongoDB son: `Bases de datos`, `Colecciones` y `Documentos`:

![][01]

Una `Base de datos` (Database) en MongoDB es el contenedor principal donde se almacenan los datos:

* Agrupa varias colecciones relacionadas
* Es equivalente a una base de datos en otros sistemas (como MySQL)
* Sirve para organizar la información de una aplicación

Ejemplo:

* Una aplicación puede tener una base de datos llamada tienda

Una `Colección` (Collection) es un conjunto de documentos dentro de una base de datos:

* Es equivalente a una tabla en bases de datos relacionales
* No requiere un esquema fijo (los documentos pueden tener estructuras diferentes)
* Agrupa datos del mismo tipo

Ejemplo:

* Dentro de tienda puedes tener: _clientes_, _productos_, _pedidos_.

Un `Documento` (Document) es la unidad básica de datos en MongoDB:

* Se almacena en formato JSON/BSON
* Contiene pares clave-valor
* Es equivalente a una fila (registro) en bases de datos relacionales

Ejemplo de documento:

```
{
  codigo: 1,  
  nombre: 'El aleph',
  autor: 'Borges',
  editoriales: ['Planeta','Siglo XXI']
}
```

El documento anterior representa un libro donde se almacenan su código, nombre, autor y editoriales que lo suministran.


La diferencia entre un Sistema Gestor de Base de Datos y MongoDB es la siguiente:

![][02]

### Creación de BBDD 

Para crear una BBDD usaremos el comando `use` para crear una Base de Datos llamada `biblioteca`:

```
> use biblioteca
switched to db biblioteca
```

Mediante el comando `use` activamos una base de datos existente o creamos una nueva (en nuestro caso la llamamos biblioteca), queda luego activa la base de datos `biblioteca`.

Para ver las Bases de Datos ya creadas:

```
test> use biblioteca
switched to db biblioteca

biblioteca> show dbs
admin   40.00 KiB
config  12.00 KiB
local   40.00 KiB
```

> __Nota__: la base de datos `biblioteca` no aparece porque no tiene datos todavía.

### Creación de colecciones y documentos

Procedemos ahora a crear la colección `libros` e insertar el primer documento, la colección se crea en el momento que insertamos el primer documento, en la shell de Mongo tenemos que ingresar:

```
db.libros.insertOne(
  {
    codigo: 1,  
    nombre: 'El aleph',
    autor: 'Borges',
    editoriales: ['Planeta','Siglo XXI']
  }
)
```

de la siguiente manera:

```
biblioteca> db.libros.insertOne(
|   {
|     codigo: 1,
|     nombre: 'El aleph',
|     autor: 'Borges',
|     editoriales: ['Planeta','Siglo XXI']
|   }
| )
{
  acknowledged: true,
  insertedId: ObjectId('69f9c361d20ac2b4799df8a3')
}

biblioteca> show dbs
admin       40.00 KiB
biblioteca   8.00 KiB
config      60.00 KiB
local       40.00 KiB
```

> __Nota__: ahora sí aparece la Base de Datos `biblioteca` ya que tiene una `colección` llamada `libros`.

Después de crear la colección `libros` procedamos a insertar el segundo documento:

```
db.libros.insertOne(
  {
    codigo: 2,
    nombre: 'Martin Fierro',
    autor: 'Jose Hernandez',
    editoriales: ['Planeta']
  }
)
```

de la siguiente manera:

```
biblioteca> db.libros.insertOne(
|   {
|     codigo: 2,
|     nombre: 'Martin Fierro',
|     autor: 'Jose Hernandez',
|     editoriales: ['Planeta']
|   }
| )
{
  acknowledged: true,
  insertedId: ObjectId('69f9c458d20ac2b4799df8a4')
}
```

Hay que tener en cuenta que mediante el objeto `db` accedemos a la base de datos activa (la misma la activamos con el comando `use biblioteca`), seguidamente disponemos el nombre de la colección `libros` y finalmente el nombre del método `insertOne` al que le pasamos un objeto en formato JSON.

Ahora nuestra colección `libros` tiene dos documentos, si queremos mostrar los datos almacenados en la colección `libros` podemos llamar al método `find()`:

```
biblioteca> db.libros.find()
[
  {
    _id: ObjectId('69f9c361d20ac2b4799df8a3'),
    codigo: 1,
    nombre: 'El aleph',
    autor: 'Borges',
    editoriales: [ 'Planeta', 'Siglo XXI' ]
  },
  {
    _id: ObjectId('69f9c458d20ac2b4799df8a4'),
    codigo: 2,
    nombre: 'Martin Fierro',
    autor: 'Jose Hernandez',
    editoriales: [ 'Planeta' ]
  }
]
```

Los datos que vemos coinciden con los ingresados al llamar al método insertOne, con la salvedad que se ha agregado un campo llamado `_id` en forma automática.

Todos los documentos requiere una clave principal almacenada en el campo `_id`. Podemos indicar nosotros el valor a almacenar en el campo `_id`, pero si no lo hacemos se crea en forma automática.

> __Nota__: cada vez que iniciamos MongoDB shell se activa por defecto la base de datos `test` mediante el comando `use` debemos activar la base de datos que necesitamos trabajar. Para saber en todo momento que base de datos se encuentra activa debemos escribir la variable `db`:

```
biblioteca> db
biblioteca
```

[01]: ../img/ut08/01.png "01"
[02]: ../img/ut08/02.jpg "02"
