# MongoDB

* [Introducción](#introducción)
* [MongoDB](#mongodb-1)
  * [Instalación](#instalación)
  * [Elementos de MongoDB](#elementos-de-mongodb)
  * [Creación de BBDD](#creación-de-bbdd)
  * [Creación de colecciones y documentos](#creación-de-colecciones-y-documentos)
  * [Métodos insertOne e inertMany](#métodos-insertone-e-inertmany)
  * [Campo obligatorio \_id](#campo-obligatorio-_id)
  * [Comandos del shell de MongoDB: use, show dbs, show collections y help](#comandos-del-shell-de-mongodb-use-show-dbs-show-collections-y-help)
  * [Borrar bases de datos, colecciones o todos los documentos de una colección](#borrar-bases-de-datos-colecciones-o-todos-los-documentos-de-una-colección)
  * [Filtrar documentos de una colección con el método find](#filtrar-documentos-de-una-colección-con-el-método-find)


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

### Métodos insertOne e inertMany

Para insertar un documento o un conjunto de documentos disponemos de los métodos:

* `insertOne`: inserta un documento en una colección.
* `insertMany`: inserta múltiples documentos en una colección.

En el apartado anterior vimos como utilizar el método `insertOne` para insertar un documento en la colección `libros`. Para insertar más de un documento en la colección `libros` mediante el método `insertMany` sería de la siguiente manera:

```
db.libros.insertMany(
  [
    {
      codigo: 3,  
      nombre: 'Aprenda PHP',
      autor: 'Mario Molina',
      editoriales: ['Planeta']
    },
    {
      codigo: 4,  
      nombre: 'Java en 10 minutos',
      autor: 'Barros Sergio',
      editoriales: ['Planeta','Siglo XXI']
    }
  ]
)
```

Obtendríamos la siguiente salida en la Shell de MongoDB:

```
biblioteca> db.libros.insertMany(
|   [
|     {
|       codigo: 3,
|       nombre: 'Aprenda PHP',
|       autor: 'Mario Molina',
|       editoriales: ['Planeta']
|     },
|     {
|       codigo: 4,
|       nombre: 'Java en 10 minutos',
|       autor: 'Barros Sergio',
|       editoriales: ['Planeta','Siglo XXI']
|     }
|   ]
| )
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('69fc9c89d33228a73b9df8a3'),
    '1': ObjectId('69fc9c89d33228a73b9df8a4')
  }
}
```

Ahora si quieremos ver todos los documentos de la colección `libros` usamos el método `find()`:

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
  },
  {
    _id: ObjectId('69fc9c89d33228a73b9df8a3'),
    codigo: 3,
    nombre: 'Aprenda PHP',
    autor: 'Mario Molina',
    editoriales: [ 'Planeta' ]
  },
  {
    _id: ObjectId('69fc9c89d33228a73b9df8a4'),
    codigo: 4,
    nombre: 'Java en 10 minutos',
    autor: 'Barros Sergio',
    editoriales: [ 'Planeta', 'Siglo XXI' ]
  }
]
```

Para borrar el contenido de la Shell de MongoDB se hace con el comando `cls`:

```
biblioteca> cls
```

### Campo obligatorio _id

En MongoDB todo documento requiere un campo clave que se debe llamar `_id`. Si no definimos dicho campo el mismo se crea en forma automática y se carga un valor único.

Podemos definir y cargar un valor en el campo `_id` cuando inicializamos un documento, en este caso, en una nueva colección llamada `clientes`:

```
db.clientes.insertOne(
  {
    _id: 1,  
    nombre: 'Lopez Marcos',
    domicilio: 'Colon 111',
    provincia: 'Cordoba'
  }
)
```

```
biblioteca> db.clientes.insertOne(
|   {
|     _id: 1,
|     nombre: 'Lopez Marcos',
|     domicilio: 'Colon 111',
|     provincia: 'Cordoba'
|   }
| )
{ acknowledged: true, insertedId: 1 }
```

Ahora consultamos todos los documentos de la colección `clientes` mediante el método `find()`:

```
biblioteca> db.clientes.find()
[
  {
    _id: 1,
    nombre: 'Lopez Marcos',
    domicilio: 'Colon 111',
    provincia: 'Cordoba'
  }
]
```
Vemos que hay un solo documento que tiene el campo `_id` con un valor igual a 1.

Si intentamos insertar un nuevo documento con el mismo `_id`:

```
db.clientes.insertOne(
  {
    _id: 1,
    nombre: 'Ana María',
    domicilio: 'Oliveira 3',
    provincia: 'Huelva'
  }
)
```

Obtenemos el siguiente error:

```
biblioteca> db.clientes.insertOne(
|   {
|     _id: 1,
|     nombre: 'Ana María',
|     domicilio: 'Oliveira 3',
|     provincia: 'Huelva'
|   }
| )
MongoServerError: E11000 duplicate key error collection: biblioteca.clientes index: _id_ dup key: { _id: 1 }
```

> __Nota__: no pueden hacer dos documentos con el mismo `_id`.

### Comandos del shell de MongoDB: use, show dbs, show collections y help

Ya vimos en apartados anteriores que, mediante el comando `use`, activamos la base de datos con la que queremos trabajar:

```
test> use biblioteca
switched to db biblioteca
biblioteca>
```

Para conocer todas las bases de datos del servidor de MongoDB utilizamos el comando `show dbs`:

```
biblioteca> show dbs
admin        40.00 KiB
biblioteca  120.00 KiB
config       88.00 KiB
local        80.00 KiB
```

Para ver las colecciones que contiene cada base de datos utilizamos el comando `show collections`:

```
biblioteca> show collections
clientes
libros
```

En la base de datos `biblioteca` contiene dos colecciones llamadas `clientes` y `libros` que nosotros creamos en conceptos anteriores.

Si queremos consultar más comandos para la Shell de MongoDB ejecutamos el comando `help`:

```
biblioteca> help

  Shell Help:

    log                                        'log.info(<msg>)': Write a custom info/warn/error/fatal/debug message to the log file
                                               'log.getPath()': Gets a path to the current log file

    use                                        Set current database
    show                                       'show databases'/'show dbs': Print a list of all available databases
                                               'show collections'/'show tables': Print a list of all collections for current database
                                               'show profile': Prints system.profile information
                                               'show users': Print a list of all users for current database
                                               'show roles': Print a list of all roles for current database
                                               'show log <name>': Display log for current connection, if name is not set uses 'global'
                                               'show logs': Print all logger names.
    exit                                       Quit the MongoDB shell with exit/exit()/.exit
    quit                                       Quit the MongoDB shell with quit/quit()
    Mongo                                      Create a new connection and return the Mongo object. Usage: new Mongo(URI, options [optional])
    connect                                    Create a new connection and return the Database object. Usage: connect(URI, username [optional], password [optional])
    it                                         result of the last line evaluated; use to further iterate
    version                                    Shell version
    load                                       Loads and runs a JavaScript file into the current shell environment
    enableTelemetry                            Enables collection of anonymous usage data to improve the mongosh CLI
    disableTelemetry                           Disables collection of anonymous usage data to improve the mongosh CLI
    passwordPrompt                             Prompts the user for a password
    sleep                                      Sleep for the specified number of milliseconds
    print                                      Prints the contents of an object to the output
    printjson                                  Alias for print()
    convertShardKeyToHashed                    Returns the hashed value for the input using the same hashing function as a hashed index.
    cls                                        Clears the screen like console.clear()
    isInteractive                              Returns whether the shell will enter or has entered interactive mode

  For more information on usage: https://mongodb.com/docs/manual/reference/method
```

### Borrar bases de datos, colecciones o todos los documentos de una colección

Hemos visto como se crea una base de datos, una colección y se insertan documentos en la misma.

Si queremos eliminar todos los documentos de una colección debemos utilizar el método `deleteMany()` aplicado a una colección existente, en este caso a la colección `libros` de la Base de Datos `biblioteca`:

```
use biblioteca
show collections
db.libros.deleteMany({})
show collections
```

Obtenemos la siguiente salida:

```
test> use biblioteca
switched to db biblioteca

biblioteca> show collections
clientes
libros

biblioteca> db.libros.deleteMany({})
{ acknowledged: true, deletedCount: 4 }

biblioteca> db.libros.find()

biblioteca>
```

Para eliminar todos los documentos se indica con las llaves abiertas y cerradas {}. Luego veremos que podemos borrar solamente los documentos que cumplen una determinada condición.

Es importante notar que luego de llamar al método deleteMany la colección "libros" sigue existiendo, pero vacía:

```
biblioteca> show collections
clientes
libros
```

Para eliminar los documentos de una colección y la colección propiamente dicha debemos emplear el método `drop()`:

```
use biblioteca
db.libros.drop()
show collections
```

Obtenemos la siguiente salida:

```
test> use biblioteca
biblioteca> db.libros.drop()
true
biblioteca> show collections
clientes
```

Después de llamar al método `drop()` en la colección `libros`, ésta última dejará de existir.

Para eliminar una base de datos en forma completa, es decir todas sus colecciones y documentos debemos emplear el método `dropDatabase` del objeto "db":

```
show dbs
use biblioteca
db.dropDatabase()
show dbs
```

Obtenemos la siguiente salida:

```
test> show dbs
admin        40.00 KiB
biblioteca   56.00 KiB
config      108.00 KiB
local        80.00 KiB

test> use biblioteca
switched to db biblioteca

biblioteca> db.dropDatabase()
{ ok: 1, dropped: 'biblioteca' }

biblioteca> use biblioteca
already on db biblioteca
```

El método `dropDatabase()` elimina la base de datos activa.

### Filtrar documentos de una colección con el método find

Vamos a crear de nuevo la colección `libros` desde cero insertando 4 documentos:

```
use biblioteca
db.libros.drop()

db.libros.insertOne(
  {
    _id: 1,  
    titulo: 'El aleph',
    autor: 'Borges',
    editorial: ['Siglo XXI','Planeta'],
    precio: 20,
    cantidad: 50 
  }
)
db.libros.insertOne(
  {
    _id: 2,  
    titulo: 'Martin Fierro',
    autor: 'Jose Hernandez',
    editorial: ['Siglo XXI'],
    precio: 50,
    cantidad: 12
  }
)
db.libros.insertOne(
  {
    _id: 3,  
    titulo: 'Aprenda PHP',
    autor: 'Mario Molina',
    editorial: ['Siglo XXI','Planeta'],
    precio: 50,
    cantidad: 20
  }
)
db.libros.insertOne(
  {
    _id: 4,  
    titulo: 'Java en 10 minutos',
    editorial: ['Siglo XXI'],
    precio: 45,
    cantidad: 1 
  }
)
```

Con el método `find()` obtenemos todos los documentos de la colección `libros`:

```
biblioteca> db.libros.find()
[
  {
    _id: 1,
    titulo: 'El aleph',
    autor: 'Borges',
    editorial: [ 'Siglo XXI', 'Planeta' ],
    precio: 20,
    cantidad: 50
  },
  {
    _id: 2,
    titulo: 'Martin Fierro',
    autor: 'Jose Hernandez',
    editorial: [ 'Siglo XXI' ],
    precio: 50,
    cantidad: 12
  },
  {
    _id: 3,
    titulo: 'Aprenda PHP',
    autor: 'Mario Molina',
    editorial: [ 'Siglo XXI', 'Planeta' ],
    precio: 50,
    cantidad: 20
  },
  {
    _id: 4,
    titulo: 'Java en 10 minutos',
    editorial: [ 'Siglo XXI' ],
    precio: 45,
    cantidad: 1
  }
]
```

El método `find()` nos permite añadir condiciones para filtrar los resultados, en este caso filtramos el documento cuyo `_id` es igual a 1:

```
biblioteca> db.libros.find({_id : 1})
[
  {
    _id: 1,
    titulo: 'El aleph',
    autor: 'Borges',
    editorial: [ 'Siglo XXI', 'Planeta' ],
    precio: 20,
    cantidad: 50
  }
]
```

> __Nota__: si pasamos un valor para el campo `_id` que no existe luego el método find no regresa un documento.

Para filtrar los libros cuyo precio sea igual a 50:

```
biblioteca> db.libros.find({precio : 50 })
[
  {
    _id: 2,
    titulo: 'Martin Fierro',
    autor: 'Jose Hernandez',
    editorial: [ 'Siglo XXI' ],
    precio: 50,
    cantidad: 12
  },
  {
    _id: 3,
    titulo: 'Aprenda PHP',
    autor: 'Mario Molina',
    editorial: [ 'Siglo XXI', 'Planeta' ],
    precio: 50,
    cantidad: 20
  }
]
```

Podemos filtrar documentos que cumplan más de 1 condición, en este caso filtramos los libros cuyo precio es igual a 50 y se disponen de 20 unidades:

```
biblioteca> db.libros.find({precio : 50, cantidad : 20 })
[
  {
    _id: 3,
    titulo: 'Aprenda PHP',
    autor: 'Mario Molina',
    editorial: [ 'Siglo XXI', 'Planeta' ],
    precio: 50,
    cantidad: 20
  }
]
```

[01]: ../img/ut08/01.png "01"
[02]: ../img/ut08/02.jpg "02"
