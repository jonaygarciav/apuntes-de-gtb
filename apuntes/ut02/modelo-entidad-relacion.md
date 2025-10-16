# Modelo Entidad/Relación

* [Introducción](#introducción)
* [Diseño de bases de datos](#diseño-de-bases-de-datos)
  * [Fase de Análisis: Especificación de Requisitos de Software (E.R.S.)](#fase-de-análisis-especificación-de-requisitos-de-software-ers)
  * [Fase 1 del Diseño: Diseño Conceptual – Modelo Entidad/Relación (E/R)](#fase-1-del-diseño-diseño-conceptual--modelo-entidadrelación-er)
  * [Fase 2 del Diseño: Diseño Lógico – Modelo Relacional](#fase-2-del-diseño-diseño-lógico--modelo-relacional)
  * [Fase 3 del Diseño: Diseño Físico – Modelo Físico](#fase-3-del-diseño-diseño-físico--modelo-físico)
* [Modelo Entidad/Relación](#modelo-entidadrelación-1)
  * [Entidades](#entidades)
  * [Atributos](#atributos)
  * [Relaciones](#relaciones)

## Introducción

En este tema, abordaremos cómo realizar tanto el diseño conceptual como el diseño lógico de una base de datos. Comenzaremos elaborando un modelo conceptual utilizando diagramas Entidad-Relación (E/R) y versiones extendidas de estos diagramas. Este tipo de diseño es de alto nivel y se encuentra más cerca de la perspectiva del usuario final, alejándose aún del diseño físico de la base de datos. Luego, a partir del modelo Entidad-Relación, procederemos a generar el modelo relacional, que está más próximo a la implementación física de la base de datos. Veremos las reglas de transformación necesarias para convertir el modelo conceptual en el relacional. Finalmente, será importante llevar a cabo la normalización de las tablas generadas, con el fin de evitar redundancias y optimizar la base de datos. En resumen, los dos tipos de modelos lógicos que analizaremos en este tema, ordenados de mayor a menor nivel de abstracción, son:

* Modelo Entidad-Relación (extendido)
* Modelo Relacional

En el próximo tema, pasaremos al diseño físico, donde transformaremos el modelo relacional en un modelo implementable en un sistema de gestión de bases de datos (SGBD).

## Diseño de bases de datos

El diseño de bases de datos es el proceso mediante el cual se identifican y extraen todos los datos relevantes asociados a un problema determinado. Por ejemplo, si estamos diseñando una base de datos para una empresa que vende productos informáticos, deberemos identificar los datos clave del proceso de facturación. O, si estamos creando una base de datos para un centro de radiología, es esencial determinar qué datos se requieren para llevar un control adecuado de las pruebas diagnósticas. Para llevar a cabo este análisis, se debe hacer un estudio detallado del problema, lo que permitirá definir qué información es esencial para la base de datos y qué datos pueden ser descartados.

Una vez identificados los datos clave, se procede a construir los modelos adecuados. Esto implica usar herramientas de diseño para crear un esquema que exprese con precisión todos los datos que el problema necesita almacenar. En el tema anterior ya mencionamos que este proceso es similar a la elaboración de un plano antes de construir un edificio. También discutimos las diversas fases que forman parte del proceso de diseño de una base de datos. Antes de comenzar con el diseño, es necesario realizar una fase preliminar de análisis.

### Fase de Análisis: Especificación de Requisitos de Software (E.R.S.)

Antes de comenzar el diseño de una base de datos, es fundamental tener una idea clara de qué es lo que se pretende lograr. Para ello, es común que los desarrolladores o analistas se reúnan con los usuarios futuros del sistema para recopilar la información necesaria. A menudo, esto comienza con una reunión inicial para conocer el alcance general del proyecto. Posteriormente, se elabora una serie de preguntas que serán utilizadas en entrevistas con los usuarios finales, en reuniones posteriores, para obtener información más detallada. El resultado de estas entrevistas es un documento clave, conocido como Especificación de Requisitos del Software (E.R.S.), que contiene toda la información necesaria para la modelización de los datos. Este documento será la base para todas las fases posteriores del diseño.

### Fase 1 del Diseño: Diseño Conceptual – Modelo Entidad/Relación (E/R)

La modelización de datos en esta fase suele estar a cargo de un analista que no necesariamente es un experto en el dominio específico del problema que se está resolviendo (puede ser medicina, contabilidad, gestión de reservas, etc.). Por ello, es crucial contar con la colaboración de un experto en el área que sí conozca a fondo el funcionamiento del negocio o sistema en cuestión. Este experto, a su vez, puede no tener conocimientos en informática, pero su experiencia es vital para asegurar que el sistema diseñado cumpla con las expectativas del usuario final.

El objetivo en esta fase es representar toda la información recogida en la fase de análisis (contenida en el E.R.S.) utilizando estándares y herramientas que permitan comunicar el modelo de datos a la comunidad informática. El modelo conceptual utilizado en esta etapa es el Modelo Entidad/Relación (E/R), que tiene un alto poder expresivo y es ideal para facilitar la comunicación entre usuarios no técnicos y los desarrolladores. A lo largo de este tema profundizaremos en el uso de este modelo.

### Fase 2 del Diseño: Diseño Lógico – Modelo Relacional

El diseño lógico es una etapa más técnica que la anterior, ya que está orientada principalmente a profesionales de la informática. En esta fase, se traduce el modelo conceptual en un modelo relacional, que es más cercano a la implementación en un sistema de bases de datos. Este modelo describe cómo los datos serán organizados y gestionados dentro de un SGBD específico. Cabe señalar que la implementación varía dependiendo del tipo de base de datos, como una base de datos relacional, jerárquica u orientada a objetos. En este curso, nos centraremos en el Modelo Relacional.

### Fase 3 del Diseño: Diseño Físico – Modelo Físico

La última fase del diseño implica la creación del modelo físico, que se basa en la aplicación del modelo lógico a un sistema de gestión de bases de datos específico (SGBD). En esta etapa, el diseño se transforma en un lenguaje de programación de bases de datos, como SQL. Para ello, se utilizan sublenguajes como el __DDL__ (_Data Definition Language_) para definir la estructura de la base de datos en términos de tablas, relaciones e índices. En el próximo tema, exploraremos cómo llevar a cabo esta transformación utilizando SQL.

![][01]

## Modelo Entidad/Relación

El __modelo Entidad-Relación (E/R)__ es uno de los enfoques más utilizados para el diseño conceptual de bases de datos. Introducido por Peter Chen en el año 1976, este modelo se fundamenta en la idea de representar los datos mediante entidades y las conexiones o relaciones entre ellas. En este modelo, una entidad representa un objeto o concepto del mundo real que tiene relevancia para el sistema, como por ejemplo un cliente o un producto. Por su parte, las relaciones indican cómo estas entidades interactúan entre sí, como cuando un cliente realiza un pedido o un empleado trabaja en un departamento.

![][02]

El uso de este modelo facilita la comprensión de la estructura de la base de datos por parte de desarrolladores y usuarios no técnicos, ya que presenta una visión simplificada y gráfica de los datos y sus interacciones. Los símbolos principales que utiliza el modelo E/R para representar entidades, relaciones y atributos suelen expresarse mediante rectángulos (para entidades), rombos (para las relaciones) y elipses (para los atributos de las entidades). Este sistema de representación gráfica ayuda a visualizar fácilmente el diseño general de la base de datos antes de pasar a las etapas más técnicas.

Además de los símbolos básicos, el modelo E/R también incluye extensiones para manejar casos más complejos, como _entidades débiles_ (aquellas que dependen de otras para existir) y _relaciones con cardinalidad_ (que muestran cuántas instancias de una entidad pueden estar asociadas a otra).

En resumen, el modelo Entidad-Relación proporciona una manera intuitiva y estructurada de organizar los datos en un sistema, lo que permite una transición fluida desde el diseño conceptual hasta la implementación física en un sistema de gestión de bases de datos.

### Entidades

Una __entidad__ es cualquier objeto o concepto del mundo real sobre el cual se pueda guardar información dentro de una base de datos. Las entidades pueden representar elementos tangibles como personas, objetos, productos o edificios, o bien pueden ser abstracciones, como una fecha o un evento. Gráficamente, las entidades se muestran con rectángulos, dentro de los cuales aparece el nombre de la entidad. Es importante destacar que una entidad no puede ser representada más de una vez dentro del esquema conceptual de la base de datos.

Existen principalmente dos tipos de entidades:

* __Entidades fuertes__: estas son entidades que no dependen de ninguna otra para su existencia. Es decir, su identidad y características pueden mantenerse de manera independiente dentro de la base de datos.
* __Entidades débiles__: este tipo de entidad no puede existir por sí sola, ya que depende de otra entidad para definir su identidad. La entidad débil necesita apoyarse en una entidad fuerte que la respalde, ya que su existencia está condicionada a la presencia de la otra entidad.

![][03]

### Atributos

Las _entidades_ están definidas por una serie de __atributos__, también conocidos como _propiedades_ o _campos_. Estos atributos son las características que permiten distinguir una entidad de otra y proporcionan los detalles específicos sobre la entidad que representan. Cada atributo puede tomar un valor específico de un conjunto predeterminado de valores permitidos, llamado dominio del atributo. Al asignar valores a los atributos, obtenemos las diferentes instancias u ocurrencias de una entidad, lo que nos permite diferenciar entre ellas.

![][04]

Tipos de atributos:
* __Atributos identificadores o identificativos__: son aquellos cuyo valor no se repite entre diferentes instancias de una misma entidad o relación, permitiendo identificar de manera inequívoca cada ocurrencia. Estos atributos son fundamentales en la estructura de una base de datos y actúan como claves primarias. Un ejemplo común es el Código de Cuenta Corriente (CCC), que se utiliza para identificar una cuenta bancaria, o el ISBN en los libros. Es importante señalar que un atributo identificador puede ser compuesto, es decir, puede descomponerse en varios subatributos. Por ejemplo, el CCC puede dividirse en tres partes: el número del banco, el número de la sucursal y el número de cuenta.
* __Atributos descriptores o descriptivos__: estos atributos son los que describen las propiedades o características de una entidad o incluso de una relación. Son los atributos más comunes, ya que se utilizan para proporcionar detalles adicionales que no necesariamente identifican a la entidad, pero que son útiles para describirla. Por ejemplo, en una entidad "Empleado", atributos como el nombre, la edad o la dirección serían descriptores.
* __Atributos discriminadores o discriminantes__: estos atributos se utilizan para diferenciar entre varias ocurrencias de una entidad débil dentro de la entidad fuerte de la que depende. Un atributo discriminante se representa gráficamente con un círculo de color distinto al de los atributos identificadores o descriptores. Un ejemplo es el número de transacción en una cuenta corriente (CCC) o el número de ejemplar dentro de un ISBN.
* __Atributos derivados__: estos son atributos cuyos valores no se ingresan directamente, sino que se calculan a partir de otros atributos. Un buen ejemplo de atributo derivado sería la edad de una persona, que puede calcularse a partir de su fecha de nacimiento, o el precio total de un producto, que se obtiene sumando el precio base más el porcentaje de impuestos.
* __Atributos multivaluados__: este tipo de atributo puede contener varios valores dentro de un mismo dominio. Un ejemplo sería almacenar varios correos electrónicos o números de teléfono para una misma persona. Si solo se necesita guardar un único valor, se usaría un atributo descriptivo común.
* __Atributos compuestos__: a menudo, estos atributos pueden confundirse con los atributos multivaluados, pero en realidad son diferentes. Un atributo compuesto es aquel que se puede dividir en otros subatributos. Por ejemplo, un "nombre completo" podría descomponerse en "nombre", "apellido paterno" y "apellido materno". Cada subatributo perteneciente a un atributo compuesto puede tener su propio dominio y valor.

![][05]

> __Nota__: siempre es necesario contar con al menos un atributo que actúe como identificador para asegurar la unicidad de cada instancia dentro de la base de datos.

### Relaciones

Una __relación__ en el contexto de bases de datos es la conexión o vínculo que se establece entre dos o más entidades. Cada relación debe tener un nombre que refleje de forma clara la función que desempeña o la interacción que representa entre las entidades involucradas. En los diagramas de Entidad-Relación (E/R), las relaciones se ilustran con la forma de un rombo, dentro del cual se coloca el nombre de la relación.

![][06]

Por lo general, el nombre de la relación suele ser descriptivo de la relación, por ejemplo, si estamos relacionando las entidades _CLIENTE_ y _COCHE_, podríamos asignar _compra_, para indicar la acción que conecta a las entidades. Sin embargo, estos nombres más descriptivos pueden a veces causar confusión cuando estamos tratando de identificar la cardinalidad (es decir, el número de ocurrencias que puede tener una entidad en la relación), por lo que es importante elegir la nomenclatura con cuidado.

Cada relación involucra un conjunto de entidades participantes, que son las entidades vinculadas por esa relación. El grado de la relación hace referencia al número de entidades que participan en ella. Si dos entidades están conectadas, hablamos de una relación de grado 2, también conocida como relación binaria. Por ejemplo, la relación CLIENTE-COCHE es binaria porque conecta dos entidades distintas.

Un caso particular es cuando una entidad se conecta consigo misma, situación que se denomina relación __recursiva__ o __reflexiva__. En este tipo de relaciones, una misma entidad puede tener varias interacciones consigo misma, como, por ejemplo, en un organigrama de empleados donde un empleado puede estar subordinado a otro empleado, lo que genera una relación jerárquica dentro de la misma entidad.

![][07]

__El papel o rol de una entidad o relación__

El rol o papel que desempeña una entidad dentro de una relación es fundamental para entender cómo interactúan las entidades entre sí. Los roles permiten aclarar el significado y la función de cada entidad en el contexto de la relación que las vincula. Estos roles se especifican cuando es necesario hacer más evidente el propósito o el papel que una entidad juega en una relación en particular.

A continuación, mostramos ejemplos basados en los casos mencionados previamente, esta vez incluyendo la designación de los roles o papeles de cada una de las entidades dentro de las relaciones. Este enfoque ayuda a clarificar cómo cada entidad contribuye a la relación y su significado dentro del sistema:

![][08]

__La cardinalidad de una relación__

La __cardinalidad de una relación__ se refiere al número de veces que una entidad puede estar vinculada a otra entidad dentro de una relación. En el caso de una relación binaria, donde dos entidades están involucradas, la cardinalidad expresa cuántas instancias de una entidad pueden asociarse con una instancia de la otra. Existen principalmente tres tipos de cardinalidades en estas relaciones:

Una relación _uno a uno_ (1:1): Aquí, cada elemento de la primera entidad está relacionado con un solo elemento de la segunda entidad, y viceversa. En otras palabras, cada entidad tiene una única correspondencia con la otra, sin duplicaciones en ninguno de los lados.
Este tipo de relación se puede representar gráficamente para visualizar cómo los elementos de ambas entidades están vinculados de manera exclusiva entre sí:

![][09]

Una relación _uno a muchos_ (1:N) indica que un elemento de la entidad A puede estar vinculado a varios elementos de la entidad B, pero cada elemento de B solo puede estar conectado a un único elemento de A. Esto significa que, desde la perspectiva de A, hay varias asociaciones posibles, mientras que desde la perspectiva de B, solo hay una relación con A.

![][10]

Una relación _muchos a muchos_ (N:M) indica que cualquier cantidad de elementos de una entidad A puede estar asociada con múltiples elementos de una entidad B, y viceversa. Esto significa que no existe una restricción en el número de asociaciones entre los elementos de ambas entidades; un elemento de A puede estar vinculado a varios de B, y un elemento de B puede estar relacionado con varios de A.

![][11]

__La participación de una entidad__

La __participación de una entidad__ en una relación también se denomina cardinalidad de esa entidad dentro de dicha relación. Este concepto define cómo se comporta una entidad en relación con otra, y puede variar según la relación específica en la que se encuentre. En términos simples, la participación describe cuántas instancias de una entidad están asociadas con una instancia de otra entidad en una relación concreta.

Para calcular la participación, primero se debe seleccionar una ocurrencia de una entidad y determinar cuántas ocurrencias de la otra entidad están asociadas con ella, tanto en términos mínimos como máximos. Luego, el mismo análisis debe realizarse en el sentido contrario. Estas ocurrencias mínimas y máximas (también llamadas participación de una entidad) se representan mediante números colocados entre paréntesis, escritos en minúsculas y ubicados al lado de la relación que corresponde a la entidad evaluada.

Por ejemplo, en una relación Cliente - Pedido, un cliente puede realizar como mínimo un pedido o incluso no hacer ninguno, y como máximo puede realizar muchos. Este mínimo y máximo se indicarán en la representación gráfica de la relación.

Al final, la cardinalidad de la relación se representa combinando las participaciones máximas de ambas entidades, utilizando letras mayúsculas y dos puntos para separar las cantidades.

_Ejemplo 1_: en este ejemplo, un conductor puede conducir como mínimo un coche y como máximo un solo coche. Por lo tanto, la participación de la entidad Conductor en la relación se expresa como (1,1), lo que indica que no puede conducir más de un coche. Esta participación se coloca gráficamente en el lado opuesto al de la entidad Conductor, es decir, cerca de la entidad Coche.

De manera similar, un coche solo puede ser conducido por un conductor, tanto como mínimo como máximo. Así, la participación de Coche en la relación también se representa como (1,1) y se coloca junto a la entidad Conductor en la representación gráfica. Por lo tanto, al combinar las participaciones máximas de ambas entidades, obtenemos una cardinalidad 1:1, lo que significa que cada conductor está asociado con un solo coche y viceversa.

![][12]

_Ejemplo 2_: en este caso, un cliente puede comprar como mínimo un coche, pero también puede adquirir más de uno, lo que se representa con la letra "n". Esto indica que la participación del Cliente en la relación con Coche es de (1,n), y se coloca junto a la entidad Coche en el diagrama.

Por otro lado, un coche puede ser comprado como mínimo por un cliente y como máximo también por un solo cliente, lo que da una participación de (1,1). Este valor se coloca junto a la entidad Cliente en la representación gráfica. Al juntar las participaciones máximas, obtenemos una cardinalidad de 1, lo que significa que un cliente puede estar relacionado con varios coches, pero cada coche solo puede estar asociado con un único cliente.

![][13]

_Ejemplo 3_: en este ejemplo, un empleado trabaja en al menos un departamento, pero puede estar asignado a más de uno. Este "varios" se representa con la letra "n", lo que da una participación de (1,n). Esta participación se coloca en el lado opuesto a la entidad Empleado, es decir, junto a Departamento en el diagrama.

De manera similar, un departamento puede tener como mínimo un empleado, pero puede incluir a varios empleados. Esta participación también se representa como (1,n), y se coloca junto a la entidad Empleado en el gráfico. Al combinar ambas participaciones máximas, obtenemos una cardinalidad N, lo que significa que un empleado puede trabajar en varios departamentos, y un departamento puede tener varios empleados.

![][14]

__Atributos propios de una relación__

En una relación de base de datos, los atributos propios de una relación son aquellos que no pertenecen a ninguna de las entidades que se están relacionando, sino que son exclusivos de la relación en sí. Esto ocurre porque estos atributos dependen de ambas entidades que participan en la relación.

Ejemplo: Relación `compra` entre Cliente y Producto:

![][15]

Imagina que tienes dos entidades: Cliente y Producto. Cada una tiene sus propios atributos:

* __Cliente__:  código, nombre, dirección, edad, teléfono.
* __Producto__: código, nombre, descripción, precio por unidad.

Sin embargo, cuando un cliente compra un producto, podrías necesitar registrar cuántas unidades de ese producto ha comprado. Aquí es donde entra en juego el atributo Cantidad. Este valor no pertenece exclusivamente ni al cliente ni al producto, sino que depende de ambos, ya que un cliente puede comprar varios productos en diferentes cantidades, y un mismo producto puede ser comprado por varios clientes en diferentes cantidades.

Por eso, _cantidad_ es un atributo propio de la relación `compra`, ya que solo tiene sentido dentro del contexto de la interacción entre un cliente y un producto.

[01]: ../img/ut02/modelo-entidad-relacion/fases-diseno-bbdd.png "01"
[02]: ../img/ut02/modelo-entidad-relacion/elementos-modelo-entidad-relacion.png "02"
[03]: ../img/ut02/modelo-entidad-relacion/tipos-entidades.png "03"
[04]: ../img/ut02/modelo-entidad-relacion/tipos-atributos01.png "04"
[05]: ../img/ut02/modelo-entidad-relacion/tipos-atributos02.png "05"
[06]: ../img/ut02/modelo-entidad-relacion/tipos-relaciones01.png "06"
[07]: ../img/ut02/modelo-entidad-relacion/tipos-relaciones02.png "07"
[08]: ../img/ut02/modelo-entidad-relacion/tipos-relaciones03.png "08"
[09]: ../img/ut02/modelo-entidad-relacion/cardinalidad01.png "09"
[10]: ../img/ut02/modelo-entidad-relacion/cardinalidad02.png "10"
[11]: ../img/ut02/modelo-entidad-relacion/cardinalidad03.png "11"
[12]: ../img/ut02/modelo-entidad-relacion/participacion-entidad01.png "12"
[13]: ../img/ut02/modelo-entidad-relacion/participacion-entidad02.png "13"
[14]: ../img/ut02/modelo-entidad-relacion/participacion-entidad03.png "14"
[15]: ../img/ut02/modelo-entidad-relacion/atributos-propios-relacion.png "15"
