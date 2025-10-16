# Modelo Entidad/Relación Extendido

* [Introducción](#introducción)
* [Relaciones de jerarquía](#relaciones-de-jerarquía)

## Introducción

El __modelo Entidad/Relación extendido__ (E/R extendido) se basa en el modelo clásico Entidad/Relación, pero añade nuevas características para manejar situaciones más complejas, como las relaciones de jerarquía. Este tipo de relación ocurre cuando una entidad está vinculada a otras entidades a través de una relación de tipo "_es un tipo de_". En esencia, estas relaciones permiten modelar jerarquías o clasificaciones dentro de los datos.

Imaginemos que queremos crear una base de datos (BD) para los animales de un zoológico. En este caso, tendríamos la entidad ANIMAL como la entidad general, y otras entidades más específicas como __FELINO__, __AVE__, __REPTIL__ e __INSECTO__. Cada una de estas entidades más específicas estaría relacionada con __ANIMAL__ bajo una relación jerárquica, donde se podría decir que "Felino es un tipo de Animal", "Ave es un tipo de Animal", etc.

Si intentáramos representar estas relaciones utilizando el modelo clásico Entidad/Relación, podría volverse muy complicado y difícil de manejar, ya que tendríamos que repetir las relaciones y atributos comunes para todas las entidades específicas. Sin embargo, el E/R extendido permite manejar esto de manera más eficiente mediante la relación jerárquica "_es un tipo de_", lo que simplifica el diseño y evita la redundancia.

Este tipo de modelado es especialmente útil cuando se manejan sistemas con jerarquías de objetos, donde una entidad puede tener subtipos que comparten características comunes con la entidad padre, pero también pueden tener atributos o relaciones específicas.

![][01]

Para simplificar y evitar la repetición innecesaria de la misma relación en un diagrama, en el modelo Entidad/Relación extendido (E/R extendido) se introducen símbolos especiales para manejar las relaciones jerárquicas. En lugar de utilizar múltiples rombos para la relación "_es un tipo de_", se reemplaza por un triángulo invertido. Este triángulo indica que las entidades que se encuentran en la parte inferior son subtipos o entidades hijas de la entidad en la parte superior, que se denomina supertipo o entidad padre.

En este tipo de relaciones, las entidades hijas heredan características comunes de la entidad padre, lo que permite evitar la duplicación de atributos y relaciones en el diseño. Las relaciones jerárquicas se basan en un atributo común que define cómo se organiza la jerarquía. Este atributo, llamado atributo discriminador, se coloca junto a la relación jerárquica, representada por "_es\_un_" o similar. Por ejemplo, en un zoológico, el atributo "tipo" podría ser utilizado para distinguir entre los diferentes tipos de animales como felinos, aves, reptiles, e insectos.

En lugar de tener múltiples relaciones repetidas como "Felino es un tipo de Animal", "Ave es un tipo de Animal", etc., el diagrama se simplifica utilizando el triángulo invertido. La entidad ANIMAL estaría en la parte superior (supertipo), y las entidades __FELINO__, __AVE__, __REPTIL__ e __INSECTO__ se conectarían al triángulo, indicando que son subtipos de ANIMAL. El atributo discriminador "tipo" se colocaría junto a esta relación para identificar el tipo de animal.

Este enfoque permite representar las jerarquías de manera más clara y eficiente, haciendo que el diagrama sea más fácil de interpretar y manteniendo la integridad del diseño sin redundancias.

![][02]

## Relaciones de jerarquía

Las relaciones de jerarquía en bases de datos permiten organizar entidades en subentidades, representando categorías o clasificaciones dentro de una entidad más amplia. Existen varios tipos de relaciones de jerarquía que determinan cómo se distribuyen y manejan estas subentidades en una base de datos:

* __Relación total__: en este tipo de jerarquía, se subdivide una entidad en varias subentidades, y todos los elementos de la entidad principal deben pertenecer a una de estas subentidades. POr ejemplo, Si subdividimos la entidad Empleado en Ingeniero, Secretario y Técnico, todos los empleados en la base de datos deben pertenecer a uno de estos tres grupos. No puede haber empleados que no estén clasificados como alguno de esos tipos.
* __Relación parcial__: en este caso, la entidad principal se subdivide en subentidades, pero no todos los elementos de la entidad principal deben pertenecer a una de estas subentidades. Por ejemplo, si subdividimos Empleado en Ingeniero, Secretario y Técnico, pero pueden existir empleados que no pertenezcan a ninguno de estos tres tipos. Por ejemplo, podría haber Gerentes que no encajan en esas categorías.
* __Relación solapada__: en esta jerarquía, es posible que un mismo elemento de la entidad principal pertenezca a más de una subentidad al mismo tiempo. Por ejemplo, un empleado puede ser Ingeniero y Secretario al mismo tiempo. En este caso, hay empleados que cumplen con las condiciones para estar en dos o más subentidades.
* __Relación exclusiva__: aquí, se subdivide la entidad en subentidades, pero cada elemento de la entidad principal puede pertenecer solo a una subentidad. No es posible que un elemento esté en más de una subentidad simultáneamente.
Ejemplo: Un empleado solo puede ser Ingeniero, Secretario o Técnico, pero no puede ser más de uno de estos a la vez.

Estas relaciones son útiles para modelar jerarquías en bases de datos, permitiendo flexibilidad y precisión en cómo se organiza la información según las reglas y necesidades del sistema.

![][03]

__Jerarquía solapada y parcial__

![][04]

Un empleado podría ser simultáneamente técnico, científico y astronauta o técnico y astronauta, etc. (solapada). Además puede ser técnico, astronauta, científico o desempeñar otro empleo diferente (parcial).

__Jerarquía solapada y total__

![][04]

Un empleado podría ser simultáneamente técnico, científico y astronauta o técnico y astronauta, etc. (solapada). Además puede ser solamente técnico, astronauta o científico (total).

__Jerarquía exclusiva y parcial__

![][05]

Un empleado sólo puede desempeñar una de las tres ocupaciones (exclusiva) . Además puede ser técnico, o ser astronauta, o ser científico o también desempeñar otro empleo diferente, por ejemplo, podría ser FÍSICO (parcial).

![][06]

__Jerarquía exclusiva y total__

![][07]

Un empleado puede ser solamente técnico, astronauta o científico (total) y no ocupar más de un puesto (exclusiva).

[02]: ../img/ut02/modelo-entidad-relacion-extendido/modelo-er-extendido01.png "02"
[03]: ../img/ut02/modelo-entidad-relacion-extendido/modelo-er-extendido02.png "03"
[04]: ../img/ut02/modelo-entidad-relacion-extendido/relaciones-jerarquia01.png "04"
[05]: ../img/ut02/modelo-entidad-relacion-extendido/relaciones-jerarquia02.png "05"
[06]: ../img/ut02/modelo-entidad-relacion-extendido/relaciones-jerarquia03.png "06"
[07]: ../img/ut02/modelo-entidad-relacion-extendido/relaciones-jerarquia04.png "07"
[08]: ../img/ut02/modelo-entidad-relacion-extendido/relaciones-jerarquia05.png "08"
