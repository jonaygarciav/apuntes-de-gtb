# Funciones

* [Introducción](#introducción)
* [Cómo utilizar funciones de MySQL en la cláusula SELECT](#cómo-utilizar-funciones-de-mysql-en-la-cláusula-select)
  * [Funciones con cadenas](#funciones-con-cadenas)
    * [La función CONCAT()](#la-función-concat)
    * [La función CONCAT\_WS()](#la-función-concat_ws)
    * [La función LOWER()](#la-función-lower)
    * [La función UPPER()](#la-función-upper)
    * [La función SUBSTR()](#la-función-substr)
  * [Funciones matemáticas](#funciones-matemáticas)
    * [La función ABS()](#la-función-abs)
    * [La función POW()](#la-función-pow)
    * [La función SQRT()](#la-función-sqrt)
    * [La función PI()](#la-función-pi)
    * [La función ROUND()](#la-función-round)
    * [La función TRUNCATE](#la-función-truncate)
  * [Funciones de fecha](#funciones-de-fecha)
    * [La función NOW()](#la-función-now)
    * [La función CURDATE()](#la-función-curdate)
    * [La función CURTIME()](#la-función-curtime)
  * [Funciones de agregación](#funciones-de-agregación)

## Introducción

Las `funciones` en MySQL son herramientas que permiten realizar operaciones sobre datos y devolver un resultado. Se utilizan dentro de consultas SQL para transformar, calcular o manipular información almacenada en las tablas.

Las funciones pueden trabajar sobre:

* Texto (funciones de cadenas)
* Números (funciones matemáticas)
* Fechas y horas
* Datos agregados (sumatorio, media, ...)
* Conversión de tipos de datos

En la (documentación oficial)[https://dev.mysql.com/doc/refman/8.4/en/functions.html] puedes encontrar la referencia completa de todas las funciones y operadores disponibles en MySQL

## Cómo utilizar funciones de MySQL en la cláusula SELECT

Es posible hacer uso de funciones específicas de MySQL en la cláusula `SELECT`. MySQL nos ofrece funciones matemáticas, funciones para trabajar con cadenas y funciones para trabajar con fechas y horas. Algunos ejemplos de las funciones de MySQL que utilizaremos a lo largo del curso son las siguientes.

### Funciones con cadenas

Las `funciones de cadenas` permiten manipular y transformar texto almacenado en las bases de datos. Son muy útiles cuando trabajamos con nombres, direcciones, códigos, emails o cualquier dato textual.

Con ellas podemos:

* Unir textos
* Convertir mayúsculas y minúsculas
* Extraer partes de una cadena
* Formatear resultados

Algunas de estas funciones son las siguientes:

| Función    | Descripción                                  |
| ---------- | -------------------------------------------- |
| CONCAT     | Concatena cadenas                            |
| CONCAT_WS  | Concatena cadenas con un separador           |
| LOWER      | Devuelve una cadena en minúscula             |
| UPPER      | Devuelve una cadena en mayúscula             |
| SUBSTR     | Devuelve una subcadena                       |


#### La función CONCAT()

La función `CONCAT()` sirve para unir (concatenar) dos o más cadenas de texto en una sola. Es muy utilizada cuando queremos mostrar datos combinados que están campos distintos, por ejemplo:

* Nombre y apellidos juntos
* Dirección completa
* Código formado por varias columnas
* Texto personalizado en una consulta

Ejemplo de cómo obtener el nombre y los apellidos de todos los alumnos en una única columna utilizando la función `CONCAT()`.

```sql
SELECT CONCAT(nombre, apellido1, apellido2) AS nombre_completo
FROM alumno;

+----------------------------+
| nombre_completo            |
+----------------------------+
| MaríaSánchezPérez          |
| JuanSáezVega               |
| PepeRamírezGea             |
| LucíaSánchezOrtega         |
| PacoMartínezLópez          |
| IreneGutiérrezSánchez      |
| CristinaFernándezRamírez   |
| AntonioCarreteroOrtega     |
| ManuelDomínguezHernández   |
| DanielMorenoRuiz           |
+----------------------------+
10 rows in set (0,00 sec)
```

La función `CONCAT()` de MySQL no añade ningún espacio entre las columnas, por eso los valores de las tres columnas aparecen como una sola cadena sin espacios entre ellas. Para resolver este problema podemos añadir un carácter en blanco ` ` entre nombre y apellido1, y entre apellido1 y apellido2 para separarlos:


```sql
SELECT CONCAT(nombre, '  ', apellido1, ' ' , apellido2) AS nombre_completo
FROM alumno;

+------------------------------+
| nombre_completo              |
+------------------------------+
| María Sánchez Pérez          |
| Juan Sáez Vega               |
| Pepe Ramírez Gea             |
| Lucía Sánchez Ortega         |
| Paco Martínez López          |
| Irene Gutiérrez Sánchez      |
| Cristina Fernández Ramírez   |
| Antonio Carretero Ortega     |
| Manuel Domínguez Hernández   |
| Daniel Moreno Ruiz           |
+----------------------------+
10 rows in set (0,00 sec)
```

Hay una manera más elegante de hacer lo mismo y es hacer uso de la función `CONCAT_WS()` que nos permite definir un carácter separador entre cada columna. 

> __Nota__: la función `CONCAT()` devolverá NULL cuando alguna de las cadenas que está concatenando es igual `NULL`.

#### La función CONCAT_WS()

La función `CONCAT_WS()` significa _Concatenate With Separator (Concatenar con separador)_ y sirve para unir varias cadenas de texto añadiendo automáticamente un separador entre ellas.

En el siguiente ejemplo haremos uso de la función `CONCAT_WS()` y usaremos un espacio en blanco como separador.

```sql
SELECT CONCAT_WS(' ', nombre, apellido1, apellido2) AS nombre_completo
FROM alumno;

+------------------------------+
| nombre_completo              |
+------------------------------+
| María Sánchez Pérez          |
| Juan Sáez Vega               |
| Pepe Ramírez Gea             |
| Lucía Sánchez Ortega         |
| Paco Martínez López          |
| Irene Gutiérrez Sánchez      |
| Cristina Fernández Ramírez   |
| Antonio Carretero Ortega     |
| Manuel Domínguez Hernández   |
| Daniel Moreno Ruiz           |
+------------------------------+
10 rows in set (0,00 sec)
```

> __Nota__: la función CONCAT_WS omitirá todas las cadenas que sean igual a `NULL` y realizará la concatenación con el resto de cadenas.

#### La función LOWER()

La función `LOWER()` sirve para convertir una cadena de texto a minúsculas.

Por ejemplo, la función `LOWER()` nos permite obtener el nombre y los apellidos de todos los alumnos en una única columna con todas las letras en minúscula:

```sql
SELECT LOWER(CONCAT(nombre, ' ', apellido1, ' ', apellido2)) AS nombre_completo
FROM alumno;

+------------------------------+
| nombre_completo              |
+------------------------------+
| maría sánchez pérez          |
| juan sáez vega               |
| pepe ramírez gea             |
| lucía sánchez ortega         |
| paco martínez lópez          |
| irene gutiérrez sánchez      |
| cristina fernández ramírez   |
| antonio carretero ortega     |
| manuel domínguez hernández   |
| daniel moreno ruiz           |
+------------------------------+
```

#### La función UPPER()

La función `UPPER()` sirve para convertir una cadena de texto a mayúsculas.

Por ejemplo, la función `UPPER()` nos permite obtener el nombre y los apellidos de todos los alumnos en una única columna con todas las letras en mayúscula:

```sql
SELECT UPPER(CONCAT(nombre, ' ', apellido1, ' ', apellido2)) AS nombre_completo
FROM alumno;

+------------------------------+
| nombre_completo              |
+------------------------------+
| MARÍA SÁNCHEZ PÉREZ          |
| JUAN SÁEZ VEGA               |
| PEPE RAMÍREZ GEA             |
| LUCÍA SÁNCHEZ ORTEGA         |
| PACO MARTÍNEZ LÓPEZ          |
| IRENE GUTIÉRREZ SÁNCHEZ      |
| CRISTINA FERNÁNDEZ RAMÍREZ   |
| ANTONIO CARRETERO ORTEGA     |
| MANUEL DOMÍNGUEZ HERNÁNDEZ   |
| DANIEL MORENO RUIZ           |
+------------------------------+
10 rows in set (0,00 sec)
```

#### La función SUBSTR()

La función `SUBSTR()` sirve para extraer una parte de una cadena de texto. Es muy útil cuando queremos:

* Obtener solo una parte de un texto
* Extraer prefijos o códigos
* Trabajar con iniciales

La sintaxis es la siguiente:

```
SUBSTR(cadena, posición_inicial, longitud)
```

* `cadena`: es el texto original del que queremos extraer una parte.
* `posición_incial`: indica desde qué carácter empieza la extracción.
* `longitud`: indica cuántos caracteres queremos extraer a partir de la posición inicial.

Por ejemplo, si queremos obtener las 3 primeras letras del nombre:

```
SELECT SUBSTR(nombre, 1, 3) AS inicio_nombre
FROM alumno;

+---------------+
| inicio_nombre |
+---------------+
| Mar           |
| Jua           |
| Pep           |
| Luc           |
| Pac           |
| Ire           |
| Cri           |
| Ant           |
| Man           |
| Dan           |
+---------------+
10 rows in set (0,00 sec)
```

### Funciones matemáticas

Las `funciones matemáticas` permiten realizar cálculos numéricos directamente en las consultas SQL. Son muy útiles cuando trabajamos con:

* Operaciones aritméticas
* Cálculo de diferencias
* Redondeo de cantidades
* Potencias y raíces
* Valores absolutos
* Operaciones financieras

Algunas de estas funciones son las siguientes:

| Función     | Descripción                        |
| ----------- | ---------------------------------- |
| ABS()       | Devuelve el valor absoluto         |
| POW(x,y)    | Devuelve el valor de x elevado a y |
| SQRT()      | Devuelve la raíz cuadrada          |
| PI()        | Devuelve el valor del número PI    |
| ROUND()     | Redondea un valor numérico         |
| TRUNCATE()  | Trunca un valor numérico           |


#### La función ABS()

La función `ABS()` devuelve el valor absoluto de un número, es decir, elimina el signo negativo si lo hubiera.

Por ejemplo, para calcular el valor absoluto de -25:

```
SELECT ABS(-25) AS valor_absoluto;

+----------------+
| valor_absoluto |
+----------------+
|             25 |
+----------------+
1 row in set (0,00 sec)
```

Otro ejemplo para calcular la diferencia real (sin números negativos) entre ventas y objetivo:

```
SELECT ciudad, ABS(ventas - objetivo) AS diferencia
FROM oficinas;


+-------------+---------+---------------+
| ciudad      | region  | diferencia    |
+-------------+---------+---------------+
| New York    | Eastern |   117637.00   |
| Chicago     | Eastern |    64958.00   |
| Atlanta     | Eastern |    17911.00   |
| Los Angeles | Western |   110915.00   |
| Denver      | Western |   113958.00   |
+-------------+---------+---------------+
5 rows in set (0,00 sec)
```

#### La función POW()

La función `POW(x,y)` devuelve el valor de `x` elevado a `y`.

Por ejemplo, para calcular 2 elevado a 3:

```
SELECT POW(2,3) AS potencia;

+----------+
| potencia |
+----------+
|        8 |
+----------+
1 row in set (0,00 sec)
```

#### La función SQRT()

La función `SQRT()` devuelve la raíz cuadrada de un número.

Por ejemplo, para calcular la raíz cuadrada de 16:

```
SELECT SQRT(16) AS raiz;

+------+
| raiz |
+------+
|    4 |
+------+
1 row in set (0,00 sec)
```

#### La función PI()

La función `PI()` devuelve el valor del número π.

Por ejemplo, para mostrar el número π:

```
SELECT PI() AS numero_pi;

+--------------------+
| numero_pi          |
+--------------------+
| 3.141592653589793  |
+--------------------+
1 row in set (0,00 sec)
```

#### La función ROUND()

La función `ROUND()` redondea un número según la siguiente regla matemática:

* Si el siguiente decimal es mayor o igual a 5, redondea hacia arriba.
* Si el siguiente decimal es menor que 5, redondea hacia abajo.

La sintaxis de la función `ROUND()`es la siguiente:

```
ROUND(número, decimales)
```

* __número__: número a redondear
* __decimales__: el número de decimales a conservar

Por ejemplo, queremos redondear el número 3.14159 a partir del segundo decimal: el tercer decimal es 1, así que, como es menor que 5, el segundo decimal no cambia:

```
SELECT ROUND(3.14159, 2) AS valor_redondeado;

+------------------+
| valor_redondeado |
+------------------+
|             3.14 |
+------------------+
1 row in set (0,00 sec)
```

> __Nota__: si ponemos un 0 en el segundo parámetro, redondea a número entero.

> __Nota__: si omitimos el segundo parámetro, redondea a número entero también.


#### La función TRUNCATE

La función `TRUNCATE()` elimina decimales sin redondear.

la sintaxis es:

```
TRUNCATE(número, decimales)
```

* __número__: número que queremos truncar.
* __decimales__: indica cuántos decimales queremos conservar.

Por ejemplo, truncar el número 3.14159 conservando 2 decimales:

```
SELECT TRUNCATE(3.14159, 2) AS valor_truncado;

+----------------+
| valor_truncado |
+----------------+
|           3.14 |
+----------------+
1 row in set (0,00 sec)
```

> __Nota__: si ponemos 0 en el segundo parámetro elimina todos los decimales.

### Funciones de fecha

Las `funciones de fecha` y hora permiten trabajar con valores temporales dentro de las consultas SQL. Son muy útiles cuando necesitamos:

* Obtener la fecha actual
* Registrar la hora de una operación
* Comparar fechas
* Calcular diferencias de tiempo

Algunas de estas funciones son:

| Función   | Descripción                        |
| --------- | ---------------------------------- |
| NOW()     | Devuelve la fecha y la hora actual |
| CURDATE() | Devuelve la fecha actual           |
| CURTIME() | Devuelve la hora actual            |

Estas funciones las trabajaremos cuando trabajemos con fechas.

#### La función NOW()

La función `NOW()` devuelve la fecha y la hora actual del servidor MySQL.

Por ejemplo, para obtener la fecha actual:

```
SELECT NOW() AS fecha_actual;

+---------------------+
| fecha_actual        |
+---------------------+
| 2026-02-12 19:37:45 |
+---------------------+
1 row in set (0,00 sec)
```

#### La función CURDATE()

La función `CURDATE()` devuelve solamente la fecha actual, sin la hora.

Por ejemplo, para obtener la fecha actual:

```
SELECT CURDATE() AS fecha_actual;
+--------------+
| fecha_actual |
+--------------+
| 2026-02-12   |
+--------------+
1 row in set (0,00 sec)
```

#### La función CURTIME()

La función `CURTIME()` devuelve solamente la hora actual, sin la fecha.

Por ejemplo, para obtener la hora actual:

```
SELECT CURTIME() AS hora_actual;

+-------------+
| hora_actual |
+-------------+
| 18:42:10    |
+-------------+
1 row in set (0,00 sec)
```

### Funciones de agregación

Las `funciones de agregación` son funciones que operan sobre varias filas y devuelven un único valor. Se utilizan normalmente con `GROUP BY`. Son útiles cuando necesitamos:

* Calcular el total de valores numéricos
* Obtener la media de un conjunto de datos
* Contar registros en una tabla
* Encontrar el valor mínimo o máximo
* Generar estadísticas o resúmenes de información

Algunas de estas funciones son:

| Función | Descripción                                      |
|----------|--------------------------------------------------|
| SUM()    | Devuelve la suma total de una columna numérica  |
| AVG()    | Devuelve la media (promedio)                    |
| COUNT()  | Cuenta el número de registros                   |
| MIN()    | Devuelve el valor mínimo                        |
| MAX()    | Devuelve el valor máximo                        |
