# Modelo Relacional

## Introducción

Los __Sistemas de Gestión de Bases de Datos__ (SGBD) se pueden clasificar según diferentes criterios, como el modelo lógico que soportan, el número de usuarios que pueden gestionar simultáneamente, el número de puestos de trabajo, o incluso el coste. Sin embargo, la clasificación más significativa de los SGBD está relacionada con el modelo lógico que utilizan. Los principales modelos que predominan en el mercado son:

* __Modelo Jerárquico__: este modelo organiza los datos en una estructura de árbol, donde cada nodo padre puede tener varios nodos hijo, pero cada nodo hijo tiene un solo nodo padre.
* __Modelo en Red__: similar al modelo jerárquico, pero más flexible, ya que permite que un nodo hijo tenga múltiples nodos padre, lo que facilita las relaciones más complejas entre datos.
* __Modelo Relacional__: este es el modelo más común y se basa en tablas bidimensionales que representan las entidades y sus relaciones. Este es el modelo en el que nos vamos a centrar.
* __Modelo Orientado a Objetos__: este modelo combina conceptos de bases de datos con los principios de la programación orientada a objetos, permitiendo almacenar objetos completos en la base de datos.

El modelo relacional ha dominado el mercado de los SGBD principalmente por dos razones:

* __Fundamentos sólidos__: el modelo relacional se basa en el álgebra relacional, que es un modelo matemático con principios bien establecidos, lo que le otorga una base teórica robusta y confiable. Esto garantiza un manejo de datos eficaz y coherente.
* __Simplicidad y eficacia__: el modelo relacional utiliza tablas (también llamadas relaciones) como estructura fundamental, lo que resulta en un sistema simple pero potente para representar y manipular los datos. Las tablas están formadas por filas y columnas, donde cada fila (o tupla) representa una instancia de una entidad, y cada columna (o atributo) representa una propiedad de dicha entidad.

Si en una base de datos queremos representar la entidad __PERSONA__, el modelo relacional la traducirá a una tabla llamada __PERSONAS__. Los atributos de la entidad, como nombre, edad, y dirección, se convertirán en las columnas de la tabla. Cada fila de la tabla __PERSONAS__ contendrá los datos específicos de una persona, es decir, cada fila será un registro o una instancia de la entidad __PERSONA__.

Este enfoque permite que las bases de datos relacionales sean fácilmente escalables y adaptables a una gran variedad de aplicaciones, lo que contribuye a su popularidad y uso extendido en muchos sistemas comerciales y de gestión de información.

## Estructura de datos relacional

| DNI        | Nombre   | Apellido1 | Apellido2 | Fecha Nacimiento  | Sexo | Estado Civil |
|------------|----------|-----------|-----------|-------------------|------|--------------|
| 45.123.654 | Pedro    | Gómez     | Suárez    | 12/03/1980        | H    | Casado       |
| 23.456.789 | Ana      | Martínez  | Torrez    | 05/07/1975        | M    | Soltera      |
| 78.654.321 | Carlos   | Pérez     | González  | 19/11/1990        | H    | Divorciado   |

En el modelo relacional, una relación se refiere únicamente a la definición estructural de una tabla. Esta definición incluye el nombre de la relación (o tabla) y la lista de atributos que componen dicha relación. Es importante destacar que, en este contexto, la relación no implica aún los datos almacenados, sino que se enfoca exclusivamente en cómo está organizada la tabla.

Una manera de representar esta definición sería listando el nombre de la relación seguido de los nombres de los atributos que la conforman. Por ejemplo:

* __Relación__: PERSONAS
* __Atributos__: DNI, Nombre, Apellido1, Apellido2, Fecha de Nacimiento, Sexo, Estado Civil

En este caso, se define la estructura de la tabla __PERSONAS__ con sus correspondientes atributos o columnas: DNI, Nombre, Apellido1, Apellido2, etc.

![][01]

Para identificar de manera única cada registro en una tabla del modelo relacional, se utiliza lo que se conoce como clave primaria o principal. Esta clave se compone de uno o varios atributos de la tabla, y su función es garantizar que no haya dos registros (filas) con el mismo valor en estos atributos.

En algunas tablas, puede haber más de una combinación de atributos que permita identificar de manera única una fila; estas combinaciones se conocen como claves candidatas. Sin embargo, entre todas las claves candidatas, se selecciona una que será utilizada como la clave primaria de la relación. Es importante destacar que los atributos que forman parte de la clave primaria no pueden contener valores nulos, ya que su función es asegurar la unicidad de los registros.

* __Clave primaria__: atributo o conjunto de atributos que identifican de manera única cada fila de una tabla. No pueden ser nulos.
* __Claves candidatas__: otras combinaciones posibles de atributos que también podrían identificar unívocamente las filas, pero de las cuales solo una se elige como clave primaria.

## Elementos y propiedades del modelo relacional

Una relación o tabla en el modelo relacional representa las entidades de las que se desea almacenar información en la base de datos (BD). Está compuesta por:

* __Filas__ (también llamadas _registros_ o _tuplas_): cada fila representa una ocurrencia específica de la entidad.
* __Columnas__ (también conocidas como _atributos_ o _campos_): cada columna corresponde a una propiedad de la entidad.

Propiedades de las relaciones:

* __Nombre único__: cada relación tiene un nombre distinto de todas las demás relaciones en la misma base de datos.
* __Nombres de atributos únicos__: no pueden existir dos atributos con el mismo nombre dentro de la misma relación.
* __El orden de los atributos no es importante__: no hay un orden fijo para las columnas o atributos.
* __Tuplas únicas__: cada tupla debe ser única; no se permiten tuplas duplicadas en una relación. Como mínimo, las tuplas deben diferenciarse en su clave primaria.
* __El orden de las tuplas no es importante__: no hay un orden predeterminado para los registros o filas en la tabla.

Conceptos clave:

* __Clave candidata__: es un atributo o conjunto de atributos que puede identificar de manera única una tupla en la relación. Existen varias claves candidatas, y cualquiera de ellas podría usarse como clave principal.
* __Clave principal__: es la clave candidata que se elige como identificador único de las tuplas. Sirve para asegurar que cada registro en la tabla sea único.
* __Clave alternativa__: todas las claves candidatas que no se seleccionan como clave principal se denominan claves alternativas.
* __Integridad de la entidad__: la clave principal no puede contener valores nulos. Esto garantiza que cada tupla esté identificada de manera única.
* __Dominio de un atributo__: el conjunto de valores permitidos que un atributo puede asumir.
* __Clave externa__ (_foránea_ o _ajena_): es un atributo o conjunto de atributos que actúa como clave principal en otra relación. Las claves externas son fundamentales para mantener la integridad referencial, ya que los valores presentes en la clave externa deben coincidir con valores existentes en la clave principal de la tabla relacionada.

Este conjunto de reglas asegura la coherencia y consistencia de los datos en una base de datos relacional, permitiendo una organización eficiente y evitando duplicidades o inconsistencias en la información.

## Transformación de un esquema E/R a un esquema relacional

Para traducir un Modelo Entidad/Relación (E/R) al modelo relacional, se deben seguir varias reglas o normas que guían el proceso de transformación de las entidades, relaciones y atributos del modelo conceptual a una estructura lógica basada en tablas.

![][02]

### Entidades

Cada entidad se transforma en una tabla. El identificador (o identificadores) de la entidad pasa a ser la clave principal de la relación y aparece subrayada o con la indicación: PK (Primary Key). Si hay clave alternativa esta se pone en «negrita».

Ejemplo: Todas las entidades del ejemplo anterior generan tabla. En concreto, la entidad AULA genera la siguiente tabla:

![][03]

### Relaciones binarias 

__Relaciones N:M__: Es el caso más sencillo. Siempre generan tabla. Se crea una tabla que incorpora como claves ajenas o foráneas FK (Foreign Key) cada una de las claves de las entidades que participan en la relación. La clave principal de esta nueva tabla está compuesta por dichos campos. Es importante resaltar que no se trata de 2 claves primarias, sino de una clave primaria compuesta por 2 campos. Si hay atributos propios, pasan a la tabla de la relación. Se haría exactamente igual si hubiera participaciones mínimas 0. Orden de los atributos en las claves compuestas: Se deben poner a la izquierda todos los atributos que forman la clave. El orden de los atributos que forman la clave vendrá determinado por las consultas que se vayan a realizar. Las tuplas de la tabla suelen estar ordenadas siguiendo como índice la clave. Por tanto, conviene poner primero aquel/los atributos por los que se va a realizar la consulta.

Ejemplo: Realicemos el paso a tablas de la relación N:M entre MÓDULO (1,n) y ALUMNO (1,n). Este tipo de relación siempre genera tabla y los atributos de la relación, pasan a la tabla que ésta genera.

![][04]

__Relaciones 1:N__: Por lo general no generan tabla. Se dan 2 casos:

* Caso 1: Si la entidad del lado «1» presenta participación (0,1), entonces se crea una nueva tabla para la relación que incorpora como claves ajenas las claves de ambas entidades. La clave principal de la relación será sólo la clave de la entidad del lado «N».
* Caso 2: Para el resto de situaciones, la entidad del lado «N» recibe como clave ajena la clave de la entidad del lado «1». Los atributos propios de la relación pasan a la tabla donde se ha incorporado la clave ajena.

Ejemplo de caso 1: Realicemos el paso a tablas de la relación 1:N entre PROFESOR (1,n) y EMPRESA (0,1). Como en el lado «1» encontramos participación mínima 0, se generará una nueva tabla.

![][05]

Ejemplo de caso 2: Realicemos el paso a tablas de la relación 1:N entre MÓDULO (1,1) y TEMA (1,n). Como no hay participación mínima «0» en el lado 1, no genera tabla y la clave principal del lado «1» pasa como foránea al lado «n».

![][06]

Relaciones 1:1: Por lo general no generan tabla. Se dan 3 casos:

* Caso 1: Si las dos entidades participan con participación (0,1), entonces se crea una nueva tabla para la relación.
* Caso 2: Si alguna entidad, pero no las dos, participa con participación mínima 0 (0,1), entonces se pone la clave ajena en dicha entidad, para evitar en lo posible, los valores nulos.
* Caso 3: Si tenemos una relación 1:1 y ninguna tiene participación mínima 0, elegimos la clave principal de una de ellas y la introducimos como clave clave ajena en la otra tabla. Se elegirá una u otra forma en función de como se quiera organizar la información para facilitar las consultas. Los atributos propios de la relación pasan a la tabla donde se introduce la clave ajena.

Ejemplo de caso 1: No se presenta ninguna situación así en el esquema estudiado. Una situación donde puede darse este caso es en HOMBRE (0,1) se casa con MUJER (0,1). Es similar al caso 1 del apartado anterior en relaciones 1:N, aunque en este caso debemos establecer una restricción de valor único para FK2.

![][07]

Ejemplo de caso 2 (y 3): Realicemos el paso a tablas de la relación 1:1 entre ALUMNO (1,1) y BECA (0,1). Como BECA tiene participación mínima 0, incorporamos en ella, como clave foránea, la clave de ALUMNO. Esta forma de proceder también es válida para el caso 3, pudiendo acoger la clave foránea cualquiera de las entidades.

![][08]

A continuación se muestra un resumen de los casos disponibles en relaciones N:M, 1:N y 1:1.

![][09]

### Relaciones Recursivas o Reflexivas

En este tipo de relaciones se trata como una relación normal, la diferencia está es que es posible que al pasarlo al Modelo Relacional se repita dos veces un mismo campo para relacionarlo con sí mismo.

Caso 1: Se tratará como una relación 1:N. Se generará una única tabla con todos los atributos que pueda tener, incluyendo los posibles campos de relación si los hubiere, haciendo repetición de la clave primaria como clave foránea para relacionarla consigo mismo.

![][10]

> __Nota__: no puede haber dos campos con el mismos nombre en la misma tabla, por lo que debemos cambiar el nombre de la clave para que haga referencia al papel que desempeña como clave foránea.

![][11]

Caso2: se tratará como una N:M. Se generará dos tablas, una de la propia entidad y sus atributos y otra de la relación en los que incluirá las dos claves externas y los atributos de la relación si hubiere.

![][12]

![][13]

### Jerarquía 

Existen varias soluciones para realizar el paso al modelo relacional de una jerarquía. La solución que se elija en cada caso dependerá del tipo de jerarquía que estemos resolviendo: total, parcial, inclusiva o exclusiva.
Las 3 soluciones posibles que podemos aplicar son las siguientes:

1. Crear una única tabla para la superclase. En este caso todos los atributos de las subclases se guardarían en la superclase.
2. Crear una tabla sólo para las subclases. En este caso los atributos de la superclase habría que guardarlos en cada una de las subclases.
3. Crear una tabla para cada una de las entidades, tanto para la superclase como las subclases. En este caso las subclases tendrían que guardar la clave primaria de la superclase.

__Ejemplo de especialización exclusiva/total__

![][14]

En este caso sería adecuado utilizar la solución 2 o 3:

* Solución 2: Crear una tabla sólo para las subclases.
    * EPISODIO(id, título, sinopsis, imagen, archivo_vídeo, duración temporada, número)
    * PELÍCULA(id, título, sinopsis, imagen, archivo_vídeo, duración puntuación_imdb, director)

* Solución 3: Crear una tabla para cada una de las entidades.
    * VÍDEO(id, título, sinopsis, imagen, archivo_vídeo, duración, tipo)
    * EPISODIO(id, temporada, número)
        * id: FK de VÍDEO(id)
    * PELÍCULA(id, puntuación_imdb, director)
        * id: FK de VÍDEO(id)

__Ejemplo de especialización inclusiva/total__

![][15]

En este caso podríamos utilizar cualquiera de las tres soluciones, dependerá del contexto del ejercicio y de
cómo se relacionen estas entidades con el resto de entidades del diagrama.

* Solución 1. Crear una única tabla para la superclase.
    * LIBRO(id, título, isbn, año_publicación, descripción, tipo, lugar_impresión, fecha_impresión, precio_papel,  tamaño_archivo, precio_ebook)
* Solución 2: Crear una tabla sólo para las subclases.
    * LIBRO_PAPEL(id, título, isbn, año_publicación, descripción, lugar_impresión, fecha_impresión, precio)
    * LIBRO_EBOOK(id, título, isbn, año_publicación, descripción, tamaño_archivo, precio)
* Solución 3: Crear una tabla para cada una de las entidades.
    * LIBRO(id, título, isbn, año_publicación, descripción, tipo)
    * LIBRO_PAPEL(id, fecha_impresión, precio)
        * id: FK de LIBRO(id)
    * LIBRO_EBOOK(id, tamaño_archivo, precio)
        * id: FK de LIBRO(id)

[01]: ../img/ut02/modelo-relacional/entidad01.png "01"
[02]: ../img/ut02/modelo-relacional/diagrama-entidad-relacion.png "02"
[03]: ../img/ut02/modelo-relacional/entidad02.png "03"
[04]: ../img/ut02/modelo-relacional/relacion-muchos-a-muchos.png "04"
[05]: ../img/ut02/modelo-relacional/relacion-uno-a-muchos01.png "05"
[06]: ../img/ut02/modelo-relacional/relacion-uno-a-muchos02.png "06"
[07]: ../img/ut02/modelo-relacional/relacion-uno-a-uno01.png "07"
[08]: ../img/ut02/modelo-relacional/relacion_uno-a-uno02.png "08"
[09]: ../img/ut02/modelo-relacional/relaciones-resumen.webp "09"
[10]: ../img/ut02/modelo-relacional/relacion-recursiva01.png "10"
[11]: ../img/ut02/modelo-relacional/relacion-recursiva02.png "11"
[12]: ../img/ut02/modelo-relacional/relacion-recursiva03.png "12"
[13]: ../img/ut02/modelo-relacional/relacion-recursiva04.png "13"
[14]: ../img/ut02/modelo-relacional/jerarquia-exclusiva-total.png "14"
[15]: ../img/ut02/modelo-relacional/jerarquia-inclusiva-total.png "15"