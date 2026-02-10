# Consultas SQL sobre varias tablas. Composición interna y cruzada

* [Introducción](#introducción)
* [Consultas multitabla SQL 1](#consultas-multitabla-sql-1)
  * [Composiciones cruzadas (Producto cartesiano)](#composiciones-cruzadas-producto-cartesiano)
  * [Composiciones internas (Intersección)](#composiciones-internas-intersección)
* [Consultas multitabla SQL 2](#consultas-multitabla-sql-2)
  * [Composiciones cruzadas](#composiciones-cruzadas)
  * [Composiciones internas](#composiciones-internas)
  * [Composiciones externas](#composiciones-externas)
* [El orden en las tablas no afecta al resultado final](#el-orden-en-las-tablas-no-afecta-al-resultado-final)
* [Uso de alias en las tablas](#uso-de-alias-en-las-tablas)
* [Unir tres o más tablas](#unir-tres-o-más-tablas)
* [Errores comunes](#errores-comunes)

## Introducción

Las consultas multitabla nos permiten consultar información en más de una tabla. La única diferencia respecto a las consultas sencillas es que vamos a tener que especificar en la cláusula `FROM` cuáles son las tablas que vamos a usar y cómo las vamos a relacionar entre sí.

Para realizar este tipo de consultas podemos usar dos alternativas:
* La sintaxis de `SQL 1 (SQL-86)`, que consiste en realizar el producto cartesiano de las tablas y añadir un filtro para relacionar los datos que tienen en común.
* La sintaxis de `SQL 2 (SQL-92 y SQL-2003)` que incluye todas las cláusulas de tipo JOIN.

## Consultas multitabla SQL 1

### Composiciones cruzadas (Producto cartesiano)

El __producto cartesiano__ de dos conjuntos, es una operación que consiste en obtener otro conjunto cuyos elementos son todas las parejas que pueden formarse entre los dos conjuntos. Por ejemplo, tendríamos que coger el primer elemento del primer conjunto y formar una pareja con cada uno de los elementos del segundo conjunto. Una vez hecho esto, repetimos el mismo proceso para cada uno de los elementos del primer conjunto.

![][01]

__Ejemplo__

Suponemos que tenemos una base de datos con dos tablas: __empleado__ y __departamento__.

```sql
SELECT *
FROM empleado;

+--------+-----------+--------------+-----------+-----------+---------------------+
| codigo | nif       | nombre       | apellido1 | apellido2 | codigo_departamento |
+--------+-----------+--------------+-----------+-----------+---------------------+
|      1 | 32481596F | Aarón        | Rivero    | Gómez     |                   1 |
|      2 | Y5575632D | Adela        | Salas     | Díaz      |                   2 |
|      3 | R6970642B | Adolfo       | Rubio     | Flores    |                   3 |
+--------+-----------+--------------+-----------+-----------+---------------------+
```

```sql
SELECT *
FROM departamento;

+--------+------------------+-------------+
| codigo | nombre           | presupuesto |
+--------+------------------+-------------+
|      1 | Desarrollo       |      120000 |
|      2 | Sistemas         |      150000 |
|      3 | Recursos Humanos |      280000 |
+--------+------------------+-------------+
```

El _producto cartesiano_ de las dos tablas se realiza con la siguiente consulta:

```sql
SELECT *
FROM empleado, departamento;
```

El resultado sería el siguiente:

```sql
+--------+-----------+--------+-----------+-----------+---------------------+---------------------------+-------------+--------+
| codigo | nif       | nombre | apellido1 | apellido2 | codigo_departamento | codigo | nombre           | presupuesto | gastos |
+--------+-----------+--------+-----------+-----------+---------------------+---------------------------+-------------+--------+
| 1      | 32481596F | Aarón  | Rivero    | Gómez     | 1                   | 1      | Desarrollo       | 120000      | 6000   |
| 2      | Y5575632D | Adela  | Salas     | Díaz      | 2                   | 1      | Desarrollo       | 120000      | 6000   |
| 3      | R6970642B | Adolfo | Rubio     | Flores    | 3                   | 1      | Desarrollo       | 120000      | 6000   |
| 1      | 32481596F | Aarón  | Rivero    | Gómez     | 1                   | 2      | Sistemas         | 150000      | 21000  |
| 2      | Y5575632D | Adela  | Salas     | Díaz      | 2                   | 2      | Sistemas         | 150000      | 21000  |
| 3      | R6970642B | Adolfo | Rubio     | Flores    | 3                   | 2      | Sistemas         | 150000      | 21000  |
| 1      | 32481596F | Aarón  | Rivero    | Gómez     | 1                   | 3      | Recursos Humanos | 280000      | 25000  |
| 2      | Y5575632D | Adela  | Salas     | Díaz      | 2                   | 3      | Recursos Humanos | 280000      | 25000  |
| 3      | R6970642B | Adolfo | Rubio     | Flores    | 3                   | 3      | Recursos Humanos | 280000      | 25000  |
+--------+-----------+--------+-----------+-----------+---------------------+---------------------------+-------------+--------+
```

### Composiciones internas (Intersección)

La __intersección__ de dos conjuntos es una operación que resulta en otro conjunto que contiene sólo los elementos comunes que existen en ambos conjuntos.

![][02]

Para poder realizar una operación de intersección entre las dos tablas debemos utilizar la cláusula WHERE para indicar la columna con la que queremos relacionar las dos tablas. Por ejemplo, para obtener un listado de los empleados y el departamento donde trabaja cada uno podemos realizar la siguiente consulta:

```sql
SELECT *
FROM empleado, departamento
WHERE empleado.codigo_departamento = departamento.codigo
```

El resultado sería el siguiente:

```sql
+--------+-----------+--------+-----------+-----------+---------------------+--------+------------------+-------------+--------+
| codigo | nif       | nombre | apellido1 | apellido2 | codigo_departamento | codigo | nombre           | presupuesto | gastos |
+--------+-----------+--------+-----------+-----------+---------------------+--------+------------------+-------------+--------+
| 1      | 32481596F | Aarón  | Rivero    | Gómez     | 1                   | 1      | Desarrollo       | 120000      | 6000   |
| 2      | Y5575632D | Adela  | Salas     | Díaz      | 2                   | 2      | Sistemas         | 150000      | 21000  |
| 3      | R6970642B | Adolfo | Rubio     | Flores    | 3                   | 3      | Recursos Humanos | 280000      | 25000  |
+--------+-----------+--------+-----------+-----------+---------------------+--------+------------------+-------------+--------+
```

> __Nota__: tener en cuenta que con la operación de intersección sólo obtendremos los elementos de existan en ambos conjuntos. Por lo tanto, en el ejemplo anterior puede ser que existan filas en la tabla empleado que no aparecen en el resultado porque no tienen ningún departamento asociado, al igual que pueden existir filas en la tabla departamento que no aparecen en el resultado porque no tienen ningún empleado asociado.

![][03]

## Consultas multitabla SQL 2

### Composiciones cruzadas

Producto cartesiano:
* `CROSS JOIN`

Ejemplo de `CROSS JOIN`:

```sql
SELECT *
FROM empleado CROSS JOIN departamento
```

Esta consulta nos devolvería el producto cartesiano de las dos tablas.

###  Composiciones internas

Join interna:
* `INNER JOIN` o `JOIN`

Ejemplo de `INNER JOIN` utilizando la cláusula `ON`:

```
SELECT *
FROM empleado INNER JOIN departamento
ON empleado.codigo_departamento = departamento.codigo
```

Esta consulta nos devolvería la intersección entre las dos tablas.

La palabra reservada INNER es opcional, de modo que la consulta anterior también se puede escribir así:

```sql
SELECT *
FROM empleado JOIN departamento
ON empleado.codigo_departamento = departamento.codigo
```

> __NOTA__: tener en cuenta que si olvidamos incluir la cláusula `ON` obtendremos el producto cartesiano de las dos tablas.

Por ejemplo, la siguiente consulta nos devolverá el producto cartesiano de las tablas empleado y departamento.

```
SELECT *
FROM empleado INNER JOIN departamento
```

Cuando queremos realizar una composición interna entre dos tablas y las columnas que queremos relacionar tienen el mismo nombre en ambas tablas podemos utilizar la cláusula `USING`.

Ejemplo de `INNER JOIN` utilizando la cláusula `USING`:

Supongamos que la clave primaria de la tabla departamento se llama _codigo\_departamento_ y la clave ajena de a tabla empleado se llama también _codigo\_departamento_. En este caso podríamos realizar la siguiente consulta:

```sql
SELECT *
FROM empleado INNER JOIN departamento
USING  (codigo_departamento)
```

### Composiciones externas

Join externa:
LEFT OUTER JOIN
RIGHT OUTER JOIN

Ejemplo de LEFT OUTER JOIN:

```sql
SELECT *
FROM empleado LEFT JOIN departamento
ON empleado.codigo_departamento = departamento.codigo
```

Esta consulta devolverá todas las filas de la tabla que hemos colocado a la izquierda de la composición, en este caso la tabla empleado. Y relacionará las filas de la tabla de la izquierda (empleado) con las filas de la tabla de la derecha (departamento) con las que encuentre una coincidencia. Si no encuentra ninguna coincidencia, se mostrarán los valores de la fila de la tabla izquierda (empleado) y en los valores de la tabla derecha (departamento) donde no ha encontrado una coincidencia mostrará el valor NULL.

Ejemplo de `RIGHT OUTER JOIN`:

```sql
SELECT *
FROM empleado RIGHT JOIN departamento
ON empleado.codigo_departamento = departamento.codigo
```

Esta consulta devolverá todas las filas de la tabla que hemos colocado a la derecha de la composición, en este caso la tabla departamento. Y relacionará las filas de la tabla de la derecha (departamento) con las filas de la tabla de la izquierda (empleado) con las que encuentre una coincidencia. Si no encuentra ninguna coincidencia, se mostrarán los valores de la fila de la tabla derecha (departamento) y en los valores de la tabla izquierda (empleado) donde no ha encontrado una coincidencia mostrará el valor NULL.

## El orden en las tablas no afecta al resultado final

El orden en las tablas a la hora de realizar la operación de INNER JOIN no afecta al resultado final de la consulta, siempre que se indique el mismo orden de las columnas en la cláusula `SELECT`.

En el siguiente ejemplo se muestran dos consultas donde hemos modificado el orden en el que aparecen las tablas al realizar la operación de `INNER JOIN`. Sin embargo, las dos consultas devuelven el mismo resultado porque en la cláusula SELECT hemos indicado el mismo orden de las columnas.

```sql
SELECT empleado.apellido1, empleado.nombre, departamento.nombre
FROM empleado INNER JOIN departamento
ON empleado.codigo_departamento = departamento.codigo
SELECT empleado.apellido1, empleado.nombre, departamento.nombre
FROM departamento INNER JOIN empleado
ON empleado.codigo_departamento = departamento.codigo
El resultado de ambas consultas sería el mismo:

+--------------------+-----------------+---------------------+
| empleado.apellido1 | empleado.nombre | departamento.nombre |
+--------------------+-----------------+---------------------+
| ...                | ...             | ...                 | 
```

Si en las consultas anteriores hubiésemos utilizado el operador * en la cláusula `SELECT`, el resultado de ambas consultas habría sido diferente, ya que el orden de las columnas dependería del orden en el que aparecen las tablas en la operación de `INNER JOIN`.

Por ejemplo, estas dos consultas devolverían resultados diferentes:

```sql
/* 1 */
SELECT *
FROM empleado INNER JOIN departamento
ON empleado.codigo_departamento = departamento.codigo
/* 2 */
SELECT *
FROM departamento INNER JOIN empleado
ON empleado.codigo_departamento = departamento.codigo
El resultado de las dos consultas anteriores sería diferente:
```sql

```sql
/* 1 */
+--------+-----------+--------+-----------+-----------+---------------------+--------+------------------+-------------+--------+
| codigo | nif       | nombre | apellido1 | apellido2 | codigo_departamento | codigo | nombre           | presupuesto | gastos |
+--------+-----------+--------+-----------+-----------+---------------------+--------+------------------+-------------+--------+
/* 2 */
+--------+------------------+-------------+--------+--------+-----------+--------+-----------+-----------+---------------------+
| codigo | nombre           | presupuesto | gastos | codigo | nif       | nombre | apellido1 | apellido2 | codigo_departamento | 
+--------+------------------+-------------+--------+--------+-----------+--------+-----------+-----------+---------------------+
```

## Uso de alias en las tablas

Para crear un alias en una tabla podemos utilizar la palabra reservada `AS` o escribir el nombre del alias directamente después del nombre de la tabla.

A continuación se muestra un ejemplo de cada caso.

Ejemplo: Cómo crear una alias de una tabla utilizando la palabra reservada `AS`.

```sql
SELECT *
FROM empleado AS e INNER JOIN departamento AS d
ON e.codigo_departamento = d.codigo
```

Ejemplo: Cómo crear un alias de una tabla sin utilizar la palabra reservada AS.

```sql
SELECT *
FROM empleado e INNER JOIN departamento d
ON e.codigo_departamento = d.codigo
```

## Unir tres o más tablas

Podemos unir tres o más tablas en una misma operación de INNER JOIN.

Ejemplo:

```sql
SELECT *
FROM cliente INNER JOIN empleado 
ON cliente.codigo_empleado_rep_ventas = empleado.codigo_empleado
INNER JOIN pago
ON cliente.codigo_cliente = pago.codigo_cliente;
```
## Errores comunes

Nos olvidamos de incluir en el WHERE la condición que nos relaciona las dos tablas.

```sql
-- Consulta incorrecta
SELECT *
FROM producto, fabricante
WHERE fabricante.nombre = 'Lenovo';

-- Consulta correcta
SELECT *
FROM producto, fabricante
WHERE producto.codigo_fabricante = fabricante.codigo AND fabricante.nombre = 'Lenovo';
```

Nos olvidamos de incluir ON en las consultas de tipo INNER JOIN.

```sql
-- Consulta incorrecta

SELECT *
FROM producto INNER JOIN fabricante
WHERE fabricante.nombre = 'Lenovo';

-- Consulta correcta
SELECT *
FROM producto INNER JOIN fabricante
ON producto.codigo_fabricante = fabricante.codigo
WHERE fabricante.nombre = 'Lenovo';
```

Relacionamos las tablas utilizando nombres de columnas incorrectos.

```sql
-- Consulta incorrecta
SELECT *
FROM producto INNER JOIN fabricante
ON producto.codigo = fabricante.codigo;

-- Consulta correcta
SELECT *
FROM producto INNER JOIN fabricante
ON producto.codigo_fabricante = fabricante.codigo;
```

Cuando hacemos la intersección de tres tablas con INNER JOIN nos olvidamos de incluir ON en alguna de las intersecciones.

```sql
-- Consulta incorrecta
SELECT DISTINCT nombre_cliente, nombre, apellido1
FROM cliente INNER JOIN empleado
INNER JOIN pago
ON cliente.codigo_cliente = pago.codigo_cliente;

-- Consulta correcta
SELECT DISTINCT nombre_cliente, nombre, apellido1
FROM cliente INNER JOIN empleado 
ON cliente.codigo_empleado_rep_ventas = empleado.codigo_empleado
INNER JOIN pago
ON cliente.codigo_cliente = pago.codigo_cliente;
```

[01]: ../img/ut05/consultas-sql-varias-tablas-composicion-interna-y-cruzada/01.png "01"
[02]: ../img/ut05/consultas-sql-varias-tablas-composicion-interna-y-cruzada/02.png "02"
[03]: ../img/ut05/consultas-sql-varias-tablas-composicion-interna-y-cruzada/03.png "03"
