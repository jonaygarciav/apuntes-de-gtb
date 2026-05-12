# MongoDB

* [Introducción](#introducción)
* [MongoDB](#mongodb-1)
  * [Instalación](#instalación)
  * [Elementos de MongoDB](#elementos-de-mongodb)
  * [Creación de BBDD](#creación-de-bbdd)
  * [Creación de colecciones y documentos](#creación-de-colecciones-y-documentos)
  * [Métodos insertOne() e insertMany()](#métodos-insertone-e-insertmany)
  * [Campo obligatorio \_id](#campo-obligatorio-_id)
  * [Comandos del shell de MongoDB: use, show dbs, show collections y help](#comandos-del-shell-de-mongodb-use-show-dbs-show-collections-y-help)
  * [Borrar bases de datos, colecciones o todos los documentos de una colección](#borrar-bases-de-datos-colecciones-o-todos-los-documentos-de-una-colección)
  * [Filtrar documentos de una colección con el método find()](#filtrar-documentos-de-una-colección-con-el-método-find)
  * [Operadores relacionales $eq, $gt, $gte, $lt, $nin y $ne](#operadores-relacionales-eq-gt-gte-lt-nin-y-ne)
  * [Borrar documentos de una colección con los métodos deleteOne() y deleteMany()](#borrar-documentos-de-una-colección-con-los-métodos-deleteone-y-deletemany)
  * [Modificar un documento mediante el método updateOne()](#modificar-un-documento-mediante-el-método-updateone)
  * [Modificar múltiples documentos con el método updateMany()](#modificar-múltiples-documentos-con-el-método-updatemany)
  * [Operadores lógicos $and, $or y $not](#operadores-lógicos-and-or-y-not)


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

### Métodos insertOne() e insertMany()

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

### Filtrar documentos de una colección con el método find()

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

### Operadores relacionales $eq, $gt, $gte, $lt, $nin y $ne

En este apartado veremos cómo utilizar los operadores `$eq`, `$gt`, `$gte`, `$lt`, `$nin` y `$ne` para realizar consultas:

Partimos de los siguientes datos:

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

db.libros.find()
```

Por ejemplo, si queremos buscar aquellos libros libros cuyo precio es igual a 50:

```
db.libros.find({ precio: 50 })
```

Es decir que cuando llamamos al método `find()` pasamos un objeto literal pasando en el campo precio el valor 50, luego el método `find()` filtra todos los libros cuyo precio sean exactamente igual a 50.

Otra forma de expresar la búsqueda de todos los libros con un precio igual a 50 es:

```
db.libros.find({ precio: { $eq : 50 } })
```

Es decir que luego del campo precio pasamos otro objeto literal iniciando el operador `$eq` con el valor 50.

Mostramos esta segunda forma de consultar todos los libros con un precio igual a 50 debido a que cuando tenemos que consultar por ejemplo los libros con un precio inferior a 50, o superior a 50 etc. debemos indicar en forma obligatoria el operador a utilizar.

Para mostrar todos los libros con un precio inferior a 30 tenemos que utilizar el operador `$lt`:

```
db.libros.find({ precio: { $lt : 30 } })
```

Encontramos que hay 1 libro que tiene un precio menor a 30.

Listado de operadores relacionales:

* `$eq` equal: igual
* `$lt` low than: menor que
* `$lte` low than equal: menor o igual que
* `$gt` greater than: mayor que
* `$gte` greater than equal: mayor o igual que
* `$ne` not equal: distinto
* `$in` in: dentro de
* `$nin` not in: no dentro de

Veamos con algunos ejemplos como utilizar estos operadores para recuperar documentos que cumplen determinadas condiciones.

Recuperar todos los libros que tienen un precio mayor a 40:

```
db.libros.find({ precio: { $gt:40 }})
```

Recuperar todos los libros que en le campo cantidad tiene 50 o más:

```
db.libros.find( { cantidad: { $gte : 50 }})
```

Recuperar todos los libros que en le campo cantidad hay un valor distinto a 50:

```
db.libros.find( { cantidad: { $ne : 50 }})
```

Recuperar todos los libros cuyo precio estén comprendidos entre 20 y 45:

```
db.libros.find( { precio: { $gte : 20 , $lte : 45} })
```

Recuperar todos los libros de la editorial 'Planeta':

```
db.libros.find( { editorial: { $in : ['Planeta'] } })
```

Recuperar todos los libros que no pertenezcan a la editorial 'Planeta':

```
db.libros.find( { editorial: { $nin : ['Planeta'] } })
```

> __Nota__: estos operadores también se emplean cuando efectuemos borrados y modificaciones de documentos.

### Borrar documentos de una colección con los métodos deleteOne() y deleteMany()

Hemos visto en conceptos anteriores que podemos eliminar todos los documentos de una colección mediante el método deleteMany y pasando un objeto literal vacío:

```
db.libros.deleteMany({})
```

Aprendimos también a recuperar algunos documentos con el método `find()` empleando una serie de operadores relacionales, dichos operadores se pueden emplear en forma idéntica con los métodos `deleteMany()` y `deleteOne()`.

Hay dos métodos para eliminar documentos:

* `deleteMany()`: borra todos los documentos que cumplen la condición que le enviamos.
* `deleteOne()`: borra el primer documento que cumple la condición que le pasamos.

Partimos de los siguientes datos:

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

db.libros.find()
```

Si queremos eliminar el documento que almacena en el campo el `_id` con valor 1 luego podemos utilizar la sintaxis:

```
db.libros.deleteOne({_id: 1})
```

Lo más conveniente es utilizar el método `deleteOne()` ya que solo uno puede cumplir esa condición al ser la clave primaria del documento.

Recordemos que la sintaxis alternativa para eliminar el documento con `_id` con valor 1 es:

```
db.libros.deleteOne({_id: { $eq : 1}})
```

Para borrar todos los libros que tienen un precio mayor o igual a 50 tenemos:

```
db.libros.deleteMany({precio : {$gte : 50 }})
```

> __Nota__: la ejecución del método `deleteOne()` y `deleteMany()` informa de la cantidad de documentos eliminados.

### Modificar un documento mediante el método updateOne()

Hemos visto en conceptos anteriores como insertar un documento en una colección, recuperar un documento, borrar un documento y nos está faltando otra operación fundamental que podemos hacer con un documento que es su modificación.

Para modificar un documento en particular disponemos de un método llamado updateOne, veamos con un ejemplo algunas de sus posibilidades:

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

db.libros.find()
```

Con las bases de datos documentales tengamos en cuenta que los documentos pueden tener distintas cantidades de campos. Por ejemplo si queremos agregar el campo descripción al libro con '_id' 4 debemos utilizar la sintaxis:

```
db.libros.updateOne({_id: {$eq:4}} ,{$set : {descripcion: 'Cada unidad trata un tema fundamental de Java desde 0.'} })

db.libros.find({_id: { $eq : 4}})
```

Luego de ejecutar tenemos el documento con `_id` igual a 4 que contiene un nuevo campo llamado 'descripcion' con el valor 'Cada unidad trata un tema fundamental de Java desde 0.':

Si queremos eliminar un campo de un documento debemos emplear el operador de actualización `$unset`. Probemos ahora de eliminar el campo que acabamos de crear para el documento con `_id` igual a 4:

```
db.libros.updateOne({_id : {$eq:4}} , {$unset : {descripcion:''} })

db.libros.find({_id: { $eq : 4}})
```

Luego de ejecutar el `updateOne()` tenemos que para el documento que tiene el `_id` igual a 4 se ha eliminado el campo `descripcion`:

Es importante entender que mediante el operador `$unset` eliminamos el campo, en cambio si utilizamos el operador `$set` modificamos el contenido del campo, luego si ejecutamos:

```
db.libros.updateOne({_id : {$eq:4}} , {$set : {descripcion:''} })
```

El campo `descripcion` sigue existiendo y almacena una cadena de texto vacía, y si no existía se crea con una cadena vacía.

Disponemos también de operadores de modificación para arreglos, veamos como podemos agregar y eliminar elementos en el arreglo `editorial`:

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

db.libros.find()
```

Por ejemplo, si queremos añadir una nueva edutorial al elemento con `_id` igual a 1:

```
db.libros.updateOne({_id : {$eq:1}} , {$push : {editorial:'Atlántida'} })

db.libros.find({_id: { $eq : 1}})
```

Podemos ver que luego de ejecutarse el método `updateOne()` el arreglo `editorial` tiene una nueva componente para el documento con `_id` 1:

De forma similar para eliminar un elemento del arreglo debemos emplear el operador `$pull`:

```
db.libros.updateOne({_id : {$eq:1}} , {$pull : {editorial:'Atlántida'} })

db.libros.find({_id: { $eq : 1}})
```

### Modificar múltiples documentos con el método updateMany()

Vimos en el concepto anterior que MongoDB nos provee de un método que nos permite modificar un único documento llamado `updateOne()`. El segundo método que nos permite actualizar documentos pero en forma masiva es el método `updateMany()`.

Veamos con un ejemplo algunas variantes del método `updateMany()`:

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

db.libros.find()
```

Actualizar el campo `cantidad` a 0 de aquellos elementos con `_id` igual a 2:

```
db.libros.updateMany({_id : {$gt:2}} , {$set : {cantidad:0} })

db.libros.find()
```

Actualizar el campo `faltantes` a `true` de aquellos elementos con `cantidad` igual a 0:

```
db.libros.updateMany({cantidad : {$eq:0}} , {$set : {faltantes:true} })

db.libros.find()
```

Actualizar con todos los libros que almacenan en el campo `cantidad` el valor cero, eliminamos el campo faltantes y fijamos el campo cantidad con el valor 100:

```
db.libros.updateMany({cantidad : {$eq:0}} , {$unset : {faltantes:true}, $set:{cantidad: 100}} )

db.libros.find()
```

### Operadores lógicos $and, $or y $not

Cuando necesitamos construir consultas que deban cumplir varias condiciones utilizaremos los operadores lógicos.

El operador `$and` lo hemos utilizando en forma implícita, por ejemplo si tenemos:

```
db.libros.find({precio : 50, cantidad : 20 }) 
```

Con la condición anterior se recuperan todos los libros que tienen un precio de 50 y la cantidad es 20. Las dos condiciones deben ser verdaderas para que el documento se recupere.

La sintaxis alternativa para el `find()` es:

```
db.libros.find({$and : [{precio:50}, {cantidad:20}]  })
```

El valor para el operador $and es un arreglo con cada una de las condiciones que debe cumplir.

Para los operadores `$or` y `$not` no hay una forma de disponer una sintaxis implícita.

Veamos con ejemplos el empleo de los operadores `$or` y `$not`:

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

db.libros.find()
```

Para recuperar los libros que tienen un precio mayor o igual a 50 o la cantidad es 1 debemos implementar mediante un $or la siguiente sintaxis:

```
db.libros.find({$or: [{precio:{$gte:50}}, {cantidad:1} ]})
```

Si queremos recuperar todos los documentos de la colección libros que no tienen un precio mayor o igual a 50 la sintaxis debe ser:

```
db.libros.find({precio: {$not:{$gte:50}} })
```

Los operadores lógicos podemos utilizarlos no solo para recuperar datos, sino también cuando borramos o actualizamos documentos.

Si queremos borrar todos los libros cuyo precio no sean iguales a 50 podemos codificar:

```
db.libros.deleteMany({precio: {$not:{$eq:50}} })
```

Se eliminan dos documentos de la colección libros.

[01]: ../img/ut08/01.png "01"
[02]: ../img/ut08/02.jpg "02"
