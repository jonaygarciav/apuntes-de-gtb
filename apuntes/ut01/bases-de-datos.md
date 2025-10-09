# Bases de Datos

* [Introducción](#introducción)
* [Bases de Datos Relacionales (RDBMS)](#bases-de-datos-relacionales-rdbms)
* [Bases de Datos NoSQL](#bases-de-datos-nosql)
* [Bases de Datos Orientadas a Objetos](#bases-de-datos-orientadas-a-objetos)
* [Bases de Datos Jerárquicas y de Red](#bases-de-datos-jerárquicas-y-de-red)
* [Tipos de Bases de Datos Según la Ubicación de la Información](#tipos-de-bases-de-datos-según-la-ubicación-de-la-información)
* [SQL](#sql)
* [Conclusión](#conclusión)

## Introducción

Una base de datos es una colección organizada de datos que están estructurados para facilitar su acceso, gestión y actualización. Las bases de datos permiten almacenar grandes cantidades de información de manera que sea fácilmente recuperable, manipulable y protegida. Los elementos principales de una base de datos son:

* __Tablas__: la unidad básica en bases de datos relacionales. Cada tabla contiene registros (filas) y campos (columnas).
* __Registros__: una fila en una tabla que representa una entrada completa de datos.
* __Campos__: las columnas de una tabla que especifican el tipo de datos que se almacenan, como nombres, fechas, números, etc.
* __Índices__: estructuras que mejoran la velocidad de las operaciones de búsqueda y ordenamiento.

![][01]

Usos de las Bases de Datos:

* __Gestión de información empresarial__: almacenan y gestionan datos críticos como información de clientes, transacciones, inventarios y empleados.
* __Aplicaciones web y móviles__: mantienen datos de usuarios, contenido dinámico, y permiten funcionalidades complejas como la personalización de contenidos y el seguimiento de interacciones.
* __Sistemas de información__: utilizados en organizaciones como hospitales, escuelas, bibliotecas y gobiernos para gestionar grandes volúmenes de datos relacionados con operaciones diarias.
* __Análisis de datos y Big Data__: en análisis de grandes volúmenes de datos para obtener insights valiosos en tiempo real o casi real.

Los tipos de Bases de Datos según el _modelo de datos_ son _BBDD relacionales_, _BBDD noSQL_, _BBDD orientada a objetos_ y _BBDD jerárquicas y de red_: 

## Bases de Datos Relacionales (RDBMS)

Organizan los datos en tablas relacionadas mediante claves primarias y ajenas. Utilizan el lenguaje SQL (Structured Query Language) para la gestión y manipulación de datos:

![][02]

Características:
* _Datos estructurados_: los datos se estructuran en tablas, donde cada fila representa un registro y cada columna un campo.
* _Integridad de los datos_: mediante restricciones y reglas como, por ejemplo claves primarias, claves foráneas, podemos mantener la integridad referencial.
* _Soporte para transacciones_: atomicidad, consistencia, aislamiento y durabilidad (ACID).

Ventajas:
* Alta consistencia y precisión de los datos.
* Eficiencia en consultas complejas y uniones de tablas.
* Existe bastante documentación sobre este tipo de bases de datos
* Multitud de herramientas de desarrollo disponibles.

Ejemplos de Sistemas Gestores de Bases de Datos relacionales: MySQL, MariaDB, PostgreSQL, Oracle Database, Microsoft SQL Server.

## Bases de Datos NoSQL

Diseñadas para datos no estructurados o semiestructurados y para aplicaciones que requieren escalabilidad horizontal y flexibilidad. Los distintos tipos son:

* __Documentales__: almacenan datos en documentos (JSON, BSON). Un ejemplo de Sistema Gestor de Bases de Datos documentales sería  _MongoDB_.
* __Clave-Valor__: almacenan datos como pares clave-valor, muy rápidos y simples. Un ejemplo de Sistema Gestor de Bases de Datos de tipo clave-valor sería _Redis_.
* __Columnas__: organizan datos por columnas, optimizadas para grandes volúmenes de datos analíticos. Un ejemplo de Sistema Gestor de Bases de Datos documentales sería  _Apache Cassandra_ o _HBase_.
* __Grafos__: almacenan datos en nodos y aristas, ideales para gestionar relaciones complejas. Un ejemplo de Sistema Gestor de Bases de Datos documentales sería  _Neo4j_.

Características:
* Mayor flexibilidad y capacidad de escalar horizontalmente.
* Diseñadas para manejar grandes volúmenes de datos distribuidos.

Ventajas:
* Escalabilidad masiva y flexibilidad en el diseño de esquemas.
* Mejor rendimiento en aplicaciones con datos no estructurados o semi-estructurados.

Desventajas:
* Menor consistencia y soporte limitado para transacciones complejas.
* Curva de aprendizaje mayor para administrar modelos de datos no convencionales.

Ejemplo para Bases de Datos NoSQL basadas en ficheros JSON:

```JSON
[
  {
    "_id": 1,
    "nombre": "Juan Pérez",
    "correo": "juan.perez@mail.com",
    "fechaRegistro": "2024-09-15"
  },
  {
    "_id": 2,
    "nombre": "María García",
    "correo": "maria.garcia@mail.com",
    "fechaRegistro": "2024-09-16"
  },
  {
    "_id": 3,
    "nombre": "Carlos Sánchez",
    "correo": "carlos.sanchez@mail.com",
    "fechaRegistro": "2024-09-17"
  },
  {
    "_id": 4,
    "nombre": "Ana López",
    "correo": "ana.lopez@mail.com",
    "fechaRegistro": "2024-09-18"
  },
  {
    "_id": 5,
    "nombre": "Luis Fernández",
    "correo": "luis.fernandez@mail.com",
    "fechaRegistro": "2024-09-19"
  }
]
```

Explicación del JSON:

* ___id__: este campo es el identificador único de cada documento en MongoDB. Aquí se usa como una clave primaria similar al ID en las bases de datos relacionales.
* __nombre__: almacena el nombre del cliente.
* __correo__: guarda la dirección de correo electrónico.
* __fechaRegistro__: almacena la fecha de registro en formato YYYY-MM-DD.

## Bases de Datos Orientadas a Objetos

Integran conceptos de la programación orientada a objetos, permitiendo que los datos se almacenen como objetos con atributos y métodos. Los objetos se almacenan en una estructura que refleja las clases y herencias de los programas. Permiten almacenar datos complejos que incluyen relaciones y métodos.

Ventajas:
* Soportan almacenamiento de objetos complejos sin necesidad de descomponerlos en tablas.
* Ideal para aplicaciones con modelos de datos complejos y con requisitos de manipulación avanzada.

Ejemplos: ObjectDB, db4o.

## Bases de Datos Jerárquicas y de Red

Las Bases de Datos Jerárquicas organizan los datos en una estructura jerárquica de árbol. Cada nodo tiene uno o varios nodos hijos pero un solo nodo padre. Común en sistemas antiguos como IMS (Information Management System) de IBM.

Desventajas:
* Estructura rígida que no permite relaciones complejas entre nodos.

Las Bases de Datos de Red son similares a la jerárquica, pero permite relaciones múltiples (nodos pueden tener múltiples padres). Se emplea empleada en sistemas complejos de modelado de datos previos a las bases de datos relacionales.

Ejemplos: IDMS (Integrated Database Management System).

## Tipos de Bases de Datos Según la Ubicación de la Información

En las _Bases de Datos centralizadas_ toda la información reside en un único servidor o centro de datos.

Ventajas:
* Sencillez en la gestión y el mantenimiento.
* Mejor control de seguridad y de acceso a los datos.

Desventajas:
* Punto único de fallo, lo que puede provocar problemas de disponibilidad y pérdida de datos si ocurre un fallo.
* Limitaciones en el escalado para soportar grandes volúmenes de usuarios.

En las bases de datos distribuidas los datos se almacenan en múltiples ubicaciones físicas, pero se gestionan como una única base de datos lógica.

Ventajas:
* Mejor rendimiento y disponibilidad al distribuir la carga entre varios servidores.
* Redundancia y tolerancia a fallos mejorada.

Desventajas:
* Complejidad en la sincronización de datos y la gestión de la consistencia entre nodos.
* Mayor dificultad en la administración y en la implementación de seguridad.

## SQL

_SQL_ (_Structured Query Language_) es un lenguaje de programación estándar diseñado específicamente para gestionar, manipular y consultar datos en bases de datos relacionales. SQL permite a los usuarios interactuar con la base de datos para realizar operaciones como la creación y modificación de tablas, inserción, actualización, eliminación de datos, y la ejecución de consultas para recuperar información específica.

Características principales de SQL:

* __Consultas de datos__: permite seleccionar y recuperar datos específicos de una base de datos usando comandos como SELECT.
* __Manipulación de datos__: ofrece comandos para insertar (INSERT), actualizar (UPDATE) y eliminar (DELETE) datos en la base de datos.
* __Definición de datos__: permite definir la estructura de las bases de datos, como crear (CREATE), modificar (ALTER) y eliminar (DROP) tablas y otros objetos de base de datos.
* __Control de acceso__: proporciona comandos para definir permisos y seguridad sobre los datos, asegurando que solo usuarios autorizados puedan realizar ciertas acciones.
* __Transacciones__: soporta transacciones que permiten agrupar varias operaciones en una sola unidad de trabajo, garantizando la consistencia y la integridad de los datos.

A continuación, se muestra una tabla con la evolución de las distintas versiones del Lenguaje SQL:

| **Versión SQL**  | **Año de Lanzamiento** | **Principales Características**                                                                                                                                         |
|------------------|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **SQL-86**       | 1986                    | Primera versión estandarizada por ANSI. Proporciona las bases del lenguaje SQL, incluyendo comandos básicos como `SELECT`, `INSERT`, `UPDATE`, y `DELETE`.              |
| **SQL-89**       | 1989                    | Mejora el estándar de 1986 con correcciones y extensiones menores, como la introducción de la cláusula `AS` para alias de tablas y columnas.                             |
| **SQL-92**       | 1992                    | Amplía significativamente SQL con soporte para subconsultas, nuevas funciones de cadena, la introducción de los `JOIN`, y mejores capacidades para manejar `NULL`.       |
| **SQL:1999**     | 1999                    | Introduce características avanzadas como los procedimientos almacenados, disparadores, tipos de datos definidos por el usuario, y soporte para programación orientada a objetos. |
| **SQL:2003**     | 2003                    | Añade soporte para XML, la instrucción `MERGE`, y nuevas funciones de ventana para el análisis de datos. Mejora el manejo de secuencias y la manipulación de datos complejos. |
| **SQL:2006**     | 2006                    | Introduce mejoras en el manejo de datos XML, como la consulta y la manipulación de datos XML almacenados en la base de datos.                                            |
| **SQL:2008**     | 2008                    | Incluye nuevas especificaciones para el almacenamiento de datos espaciales, mejora las funciones de ventana y añade nuevas funciones de diagnóstico.                     |
| **SQL:2011**     | 2011                    | Añade soporte para la sintaxis de procesamiento de datos temporales, mejorando el manejo de datos históricos y vigentes en las tablas.                                   |
| **SQL:2016**     | 2016                    | Introduce mejoras significativas en las funciones de JSON, mejoras de rendimiento y nuevas funciones analíticas avanzadas.                                               |
| **SQL:2019**     | 2019                    | Se enfoca en mejorar la integración con Big Data y mejorar la eficiencia en la ejecución de consultas, introduciendo conceptos modernos como Machine Learning e IA.      |

A continuación, se muestra una imagen con un listado de los lenguajes más utilizandos según el _Índice Tiobe_, donde se encuentra el lenguaje SQL en la posición 9:

![][03]

_SQL_ es un lenguaje ampliamente utilizado en la mayoría de los Sistemas Gestores de Bases de Datos, como _MySQL_, _PostgreSQL_, _Microsoft SQL Server_ y _Oracle_.

## Conclusión

Las bases de datos son esenciales para la gestión de la información en aplicaciones modernas, proporcionando una estructura que permite a las empresas y organizaciones almacenar, acceder y analizar datos de manera segura y eficiente. La elección del tipo de base de datos depende de factores como la naturaleza de los datos, las necesidades de rendimiento, la escalabilidad y la complejidad de las relaciones entre los datos.

[01]: ../img/ut01/bases-de-datos/tabla-registros-campos-clave-indice.png "01"
[02]: ../img/ut01/bases-de-datos/bbdd-relacional.png "02"
[03]: ../img/ut01/bases-de-datos/tiobe-index.png "03"
