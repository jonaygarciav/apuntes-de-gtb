# Normalización

## Introducción

El diseño de una base de datos relacional puede iniciarse representando el mundo real a través de un modelo conceptual, como el modelo entidad-relación (E/R). Este modelo permite obtener un esquema conceptual que, en una segunda fase, se convierte en un modelo relacional mediante reglas específicas de transformación. Alternativamente, aunque menos recomendable, es posible desarrollar directamente el esquema relacional sin pasar por el modelo conceptual. En ambos enfoques, es fundamental (y en el modelo relacional directo, obligatorio) aplicar un conjunto de reglas conocidas como la Teoría de la normalización, que asegura que el esquema relacional cumple ciertas propiedades, evitando así:

* __Redundancia de datos__: repetición innecesaria de datos en el sistema.
* __Anomalías de actualización__: inconsistencias en los datos debido a redundancias y modificaciones incompletas.
* __Anomalías de eliminación__: pérdida involuntaria de datos al eliminar otros registros relacionados.
* __Anomalías de inserción__: dificultad para agregar datos nuevos debido a la dependencia de otros datos ausentes.

En la práctica, si una base de datos se diseña usando un modelo conceptual como el modelo E/R, suele ser innecesaria una normalización posterior. Sin embargo, cuando se trata de una base de datos creada sin un diseño previo, es probable que necesite un proceso de normalización.

En la teoría de bases de datos relacionales, las formas normales (FN) son criterios que determinan la vulnerabilidad de una tabla a inconsistencias y anomalías lógicas. Cuanto más avanzada sea la forma normal de una tabla, menor será su susceptibilidad a errores y problemas de consistencia. Edgar F. Codd definió las tres primeras formas normales (1FN, 2FN y 3FN) en 1970. Estas formas se resumen en la necesidad de que todos los atributos sean atómicos, dependan completamente de la clave y de manera directa (sin dependencia transitiva). La Forma Normal de Boyce-Codd (FNBC) fue introducida en 1974 por los autores que llevan su nombre. Más adelante, las formas cuarta y quinta (4FN y 5FN) fueron desarrolladas por Fagin en 1977 y 1979, abordando específicamente la representación de relaciones complejas como muchos-a-muchos y uno-a-muchos entre atributos. Cada forma normal incluye los requisitos de las anteriores, proporcionando así una estructura de datos más optimizada y consistente.

![][01]

Algunas consideraciones previas que debemos conocer:

* __Dependencia funcional__: la dependencia funcional, representada como `A → B`, indica que el atributo B depende funcionalmente del atributo A. Esto significa que, para cada valor de A, siempre existe un valor específico de B. Por ejemplo, si A es el D.N.I. (Documento Nacional de Identidad) y B es el Nombre, entonces cada número de D.N.I. se asocia con un nombre de titular único. Por lo tanto, el nombre depende funcionalmente del D.N.I.
* __Dependencia funcional parcial__: en una relación con una clave primaria compuesta (formada por dos o más atributos), una dependencia funcional parcial ocurre cuando un atributo no clave depende solamente de una parte de la clave primaria, en lugar de depender de la clave primaria completa. Esto implica que el atributo no clave está asociado a uno de los componentes de la clave primaria, en vez de estar relacionado con toda la clave compuesta, lo cual genera redundancia y dificulta la normalización.
* __Dependencia transitiva__: la dependencia transitiva ocurre cuando un atributo C depende indirectamente de un atributo A a través de un tercer atributo B, es decir, A → B → C. Por ejemplo, si A es el D.N.I. de un alumno, B es la localidad donde vive y C es la provincia de esa localidad, entonces la provincia (C) depende de forma transitiva del D.N.I. (A) a través de la localidad (B).

## Primera Forma Normal

Para que una relación esté en la __Primera Forma Normal (1FN)__, cada atributo debe contener solo valores atómicos (es decir, valores únicos e indivisibles). Esto significa que no debe haber conjuntos de datos o listas de valores en una sola celda.

Por ejemplo, consideremos la siguiente tabla `Alumnos`, que contiene datos sobre estudiantes de un centro educativo:

| DNI       | Nombre  | Curso   | FechaMatrícula   | Tutor     | LocalidadAlumno | ProvinciaAlumno | Teléfonos                    |
|-----------|---------|---------|------------------|-----------|-----------------|-----------------|------------------------------|
| 11111111A | Eva     | 1ESO-A  | 01-Julio-2016    | Isabel    | Écija           | Sevilla         | 660111222                    |
| 22222222B | Ana     | 1ESO-A  | 09-Julio-2016    | Isabel    | Écija           | Sevilla         | 660222333 660333444 660444555|
| 33333333C | Susana  | 1ESO-B  | 11-Julio-2016    | Roberto   | Écija           | Sevilla         |                              |
| 44444444D | Juan    | 2ESO-A  | 05-Julio-2016    | Federico  | El Villar       | Córdoba         |                              |
| 55555555E | José    | 2ESO-A  | 02-Julio-2016    | Federico  | El Villar       | Córdoba         | 661000111 661000222          |

En esta tabla, el campo `Teléfonos` no es atómico, ya que algunas celdas contienen múltiples números de teléfono para un solo alumno, por lo que no cumple la _1FN_.

Para cumplir con la _1FN_, dividimos la tabla `Alumnos` en dos tablas: una tabla principal `Alumnos`, sin la columna `Teléfonos`, y otra tabla `Teléfonos` que almacena los números de teléfono asociados a cada alumno individualmente.

Tabla `Alumnos`:

| DNI       | Nombre  | Curso   | FechaMatrícula   | Tutor     | LocalidadAlumno | ProvinciaAlumno |
|-----------|---------|---------|------------------|-----------|-----------------|-----------------|
| 11111111A | Eva     | 1ESO-A  | 01-Julio-2016    | Isabel    | Écija           | Sevilla         |
| 22222222B | Ana     | 1ESO-A  | 09-Julio-2016    | Isabel    | Écija           | Sevilla         |
| 33333333C | Susana  | 1ESO-B  | 11-Julio-2016    | Roberto   | Écija           | Sevilla         |
| 44444444D | Juan    | 2ESO-A  | 05-Julio-2016    | Federico  | El Villar       | Córdoba         |
| 55555555E | José    | 2ESO-A  | 02-Julio-2016    | Federico  | El Villar       | Córdoba         |

Tabla `Teléfonos`:

| DNI       | Teléfono   |
|-----------|------------|
| 11111111A | 660111222  |
| 22222222B | 660222333  |
| 22222222B | 660333444  |
| 22222222B | 660444555  |
| 55555555E | 661000111  |
| 55555555E | 661000222  |

Con esta reorganización, cada número de teléfono es un valor atómico en la tabla `Teléfonos`, cumpliendo así con la Primera Forma Normal (1FN).

# Segunda Forma Normal

Una relación está en __Segunda Forma Normal__ (_2FN_) sí y solo si cumple con los requisitos de la _Primera Forma Normal (1FN)_ y todos los atributos que no forman parte de la clave principal dependen funcionalmente de esta la clave principal, es decir, los atributos que no forman parte de ninguna clave deben depender completamente de la clave principal.

Por ejemplo, si tomamos el ejemplo anterior de la tabla `Alumnos` en _1FN_, donde todos los datos se encuentran en una única tabla:

| DNI       | Nombre  | Curso   | FechaMatrícula   | Tutor     | LocalidadAlumno | ProvinciaAlumno |
|-----------|---------|---------|------------------|-----------|-----------------|-----------------|
| 11111111A | Eva     | 1ESO-A  | 01-Julio-2016    | Isabel    | Écija           | Sevilla         |
| 22222222B | Ana     | 1ESO-A  | 09-Julio-2016    | Isabel    | Écija           | Sevilla         |
| 33333333C | Susana  | 1ESO-B  | 11-Julio-2016    | Roberto   | Écija           | Sevilla         |
| 44444444D | Juan    | 2ESO-A  | 05-Julio-2016    | Federico  | El Villar       | Córdoba         |
| 55555555E | José    | 2ESO-A  | 02-Julio-2016    | Federico  | El Villar       | Córdoba         |

Análisis de dependencias funcionales:

![][02]

* `DNI → Nombre y LocalidadAlumno`: cada número de DNI identifica un único nombre y localidad.
* `Curso → Tutor`: cada curso tiene un tutor específico.
* `(DNI, Curso) → FechaMatrícula`: La fecha de matrícula depende de una combinación única de DNI y Curso.

Dado que algunos atributos dependen de partes individuales de la clave compuesta `(DNI, Curso)`, esta tabla no está en _2FN_. La única dependencia funcional en esta tabla es `(DNI, Curso) → FechaMatrícula`. Para resolver esto y pasar a _2FN_, dividimos la tabla en tres tablas más pequeñas.

Tabla `Alumnos` (contiene datos únicos de cada alumno):

| DNI       | Nombre  | LocalidadAlumno | ProvinciaAlumno |
|-----------|---------|-----------------|-----------------|
| 11111111A | Eva     | Écija           | Sevilla         |
| 22222222B | Ana     | Écija           | Sevilla         |
| 33333333C | Susana  | El Villar       | Córdoba         |
| 44444444D | Juan    | El Villar       | Córdoba         |
| 55555555E | José    | Écija           | Sevilla         |

Tabla `Matrículas` (contiene la relación entre alumnos y cursos, junto con la fecha de matrícula):

| DNI       | Curso   | FechaMatrícula   |
|-----------|---------|------------------|
| 11111111A | 1ESO-A  | 01-Julio-2016    |
| 22222222B | 1ESO-A  | 09-Julio-2016    |
| 33333333C | 1ESO-B  | 11-Julio-2016    |
| 44444444D | 2ESO-A  | 05-Julio-2016    |
| 55555555E | 2ESO-A  | 02-Julio-2016    |

Tabla `Cursos` (contiene la información específica de cada curso y su tutor):

| Curso     | Tutor     |
|-----------|-----------|
| 1ESO-A    | Isabel    |
| 1ESO-B    | Roberto   |
| 2ESO-A    | Federico  |

Con esta estructura, cada tabla cumple con _2FN_:
* La tabla `Alumnos` almacena los datos únicos de cada alumno, como su nombre y localidad, que dependen únicamente de DNI.
* La tabla `Matrículas` relaciona a cada alumno con un curso y la fecha de matrícula, eliminando la dependencia parcial.
* La tabla `Cursos` contiene la relación entre cada curso y su tutor, donde Tutor depende totalmente de Curso.

Al estructurar las tablas de esta forma, cada atributo depende completamente de la clave primaria de su respectiva tabla, cumpliendo con la _Segunda Forma Normal_ (_2FN_).

## Tercera Forma Normal

Una relación está en __Tercera Forma Normal__ (_3FN_) si y solo si:

* Está en 2FN.
* No existen dependencias funcionales transitivas (es decir, todos los atributos que no son clave dependen exclusivamente de la clave primaria).

Continuamos con el ejemplo de los datos de los alumnos. Partimos de una tabla `Alumnos` en _2FN_, pero que tiene una dependencia transitiva: `DNI → Localidad → Provincia`. Esto significa que _Provincia_ depende indirectamente de _DNI_ a través de _Localidad_:

| DNI       | Nombre  | Localidad   | Provincia |
|-----------|---------|-------------|-----------|
| 11111111A | Eva     | Écija       | Sevilla   |
| 22222222B | Ana     | Écija       | Sevilla   |
| 33333333C | Susana  | El Villar   | Córdoba   |
| 44444444D | Juan    | El Villar   | Córdoba   |
| 55555555E | José    | Écija       | Sevilla   |

Las dependencias funcionales existentes son las siguientes. Como podemos observar existe una dependencia funcional transitiva: `DNI → Localidad → Provincia`:

![][03]

Para eliminar la dependencia transitiva, debemos separar _Provincia_ de la tabla `Alumnos`. Creamos una nueva tabla llamada `Localidades` que almacene la relación entre _Localidad_ y _Provincia_. Así, la tabla `Alumnos` se referirá a _Localidad_, mientras que _Provincia_ se obtiene a través de la tabla `Localidades`.

Tabla `Alumnos`:

| DNI       | Nombre  | Localidad   |
|-----------|---------|-------------|
| 11111111A | Eva     | Écija       |
| 22222222B | Ana     | Écija       |
| 33333333C | Susana  | El Villar   |
| 44444444D | Juan    | El Villar   |
| 55555555E | José    | Écija       |

Tabla `Localidades`:

| Localidad   | Provincia |
|-------------|-----------|
| Écija       | Sevilla   |
| El Villar   | Córdoba   |

Ahora mostramos todas las tablas de nuestro sistema completamente en 3FN, separadas según sus dependencias.

Tabla `Alumnos`:

| DNI       | Nombre  | Localidad   |
|-----------|---------|-------------|
| 11111111A | Eva     | Écija       |
| 22222222B | Ana     | Écija       |
| 33333333C | Susana  | El Villar   |
| 44444444D | Juan    | El Villar   |
| 55555555E | José    | Écija       |

Tabla `Localidades`:

| Localidad   | Provincia |
|-------------|-----------|
| Écija       | Sevilla   |
| El Villar   | Córdoba   |

Tabla `Teléfonos`:

| DNI       | Teléfono   |
|-----------|------------|
| 11111111A | 660111222  |
| 22222222B | 660222333  |
| 22222222B | 660333444  |
| 22222222B | 660444555  |
| 55555555E | 661000111  |
| 55555555E | 661000222  |

Tabla `Matrículas`:

| DNI       | Curso   | FechaMatrícula |
|-----------|---------|----------------|
| 11111111A | 1ESO-A  | 01-Julio-2016  |
| 22222222B | 1ESO-A  | 09-Julio-2016  |
| 33333333C | 1ESO-B  | 11-Julio-2016  |
| 44444444D | 2ESO-A  | 05-Julio-2016  |
| 55555555E | 2ESO-A  | 02-Julio-2016  |

Tabla `Cursos`:

| Curso   | Tutor    |
|---------|----------|
| 1ESO-A  | Isabel   |
| 1ESO-B  | Roberto  |
| 2ESO-A  | Federico |

Explicación final:
* La tabla `Alumnos` contiene solo los datos directamente relacionados con el alumno.
* La tabla `Localidades` almacena la relación entre cada localidad y su provincia, eliminando la dependencia transitiva y cumpliendo así con la 3FN.
* Las tablas `Teléfonos`, `Matrículas`, y `Cursos` también están en 3FN, ya que cada columna depende únicamente de la clave primaria de su respectiva tabla.
* Esta estructura minimiza la redundancia y asegura la integridad de los datos en cada tabla, cumpliendo con la Tercera Forma Normal (3FN).

[01]: ../img/ut02/normalizacion/normalizacion.webp "01"
[02]: ../img/ut02/normalizacion/2fn.webp "02"
[03]: ../img/ut02/normalizacion/3fn.webp "03"
