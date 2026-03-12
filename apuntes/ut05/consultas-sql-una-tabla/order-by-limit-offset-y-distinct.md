# Cláusulas ORDER BY, LIMIT, OFFSET y palabra clave DISTINCT

* [Introducción](#introducción)
* [Cláusula ORDER BY](#cláusula-order-by)
* [Palabra clave DISTINCT](#palabra-clave-distinct)
* [Cláusula LIMIT](#cláusula-limit)
* [Cláusula OFFSET](#cláusula-offset)
* [Uso de LIMIT y OFFSET para paginación](#uso-de-limit-y-offset-para-paginación)

## Introducción

Cuando realizamos consultas SQL es muy habitual que necesitemos:

* Ordenar los resultados
* Eliminar registros duplicados
* Limitar el número de filas que devuelve una consulta
* Mostrar resultados por páginas

Para realizar estas operaciones SQL proporciona varias cláusulas y palabras clave muy utilizadas:

* `ORDER BY`: permite ordenar los resultados de una consulta.
* `DISTINCT`: elimina registros duplicados en el resultado.
* `LIMIT`: limita el número de filas devueltas.
* `OFFSET`: permite saltar un número de filas antes de mostrar los resultados.

Estas cláusulas se utilizan habitualmente junto con SELECT para controlar mejor cómo se presentan los datos obtenidos de una tabla.

## Cláusula ORDER BY

La cláusula `ORDER BY` permite ordenar los resultados de una consulta según una o varias columnas. Por defecto el orden es ascendente, aunque también se puede especificar descendente.

Sintaxis básica:

```
SELECT columnas
FROM tabla
ORDER BY columna;
```

Para mostrar el uso de `ORDER BY` crearemos una tabla sencilla llamada `producto`.

```
DROP DATABASE IF EXISTS tienda;
CREATE DATABASE tienda CHARACTER SET utf8mb4;
USE tienda;

CREATE TABLE producto (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  precio DECIMAL(6,2) NOT NULL,
  stock INT NOT NULL
);
```

Insertamos algunos datos de ejemplo:

```
INSERT INTO producto (nombre, precio, stock)
VALUES
('Auriculares', 45.00, 10),
('Teclado', 25.90, 10),
('Ratón', 15.50, 50),
('Monitor', 199.99, 5),
('Webcam', 45.00, 20);
```

Para mostrar los productos ordenados por precio utilizamos la cláusula `ORDER BY`:

```
SELECT nombre, precio
FROM producto
ORDER BY precio;
```

El resultado aparecerá ordenado de menor a mayor precio.

También podemos ordenar de forma descendente usando `DESC`:

```
SELECT nombre, precio
FROM producto
ORDER BY precio DESC;
```

En este caso el producto más caro aparecerá primero.

También es posible ordenar por varias columnas:

```
SELECT nombre, precio, stock
FROM producto
ORDER BY precio ASC, stock DESC;
```

En este ejemplo, primero se ordena por precio: si dos productos tienen el mismo precio se ordenan por stock.

## Palabra clave DISTINCT

La palabra clave `DISTINCT` permite eliminar registros duplicados en los resultados de una consulta. Esto resulta útil cuando queremos conocer los valores únicos de una columna.

Sintaxis básica:

```
SELECT DISTINCT columna
FROM tabla;
```

Supongamos que añadimos una columna llamada categoria en la tabla producto.

```
DROP DATABASE IF EXISTS tienda;
CREATE DATABASE tienda CHARACTER SET utf8mb4;
USE tienda;

CREATE TABLE producto (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  precio DECIMAL(6,2) NOT NULL,
  stock INT NOT NULL,
  categoria VARCHAR(50) NOT NULL
);
```

Insertamos una serie de registros:

```
INSERT INTO producto (nombre, precio, stock, categoria)
VALUES
('Auriculares', 45.00, 10, 'perifericos'),
('Teclado', 25.90, 10, 'perifericos'),
('Ratón', 15.50, 50, 'perifericos'),
('Monitor', 199.99, 5, 'pantallas'),
('Webcam', 45.00, 20, 'perifericos');
```

Si realizamos una consulta normal:

```
SELECT categoria
FROM producto;
```

Podremos obtener valores repetidos.

Para mostrar únicamente las categorías distintas utilizamos la palabra clave `DISTINCT`:

```
SELECT DISTINCT categoria
FROM producto;
```

El resultado mostrará cada categoría una sola vez.

También se puede usar la palabra clave `DISTINCT` con varias columnas:

```
SELECT DISTINCT categoria, precio
FROM producto;
```

En este caso SQL eliminará únicamente las filas que sean exactamente iguales en ambas columnas.

## Cláusula LIMIT

La cláusula `LIMIT` permite limitar el número de filas que devuelve una consulta.

Esto es especialmente útil cuando:

* Solamente queremos ver algunos resultados
* Queremos mostrar los datos por páginas
* Estamos trabajando con tablas muy grandes

Sintaxis básica:

```
SELECT columnas
FROM tabla
LIMIT numero;
```

Por ejemplo, si queremos mostrar únicamente los dos primeros productos:

```
SELECT *
FROM producto
LIMIT 2;
```

La consulta devolverá solamente dos registros.

También es habitual combinar `LIMIT` con `ORDER BY` para obtener los valores más altos o más bajos.

Por ejemplo, obtener el producto más caro:

```
SELECT nombre, precio
FROM producto
ORDER BY precio DESC
LIMIT 1;
```

En este caso primero se ordenan los productos por precio descendente, después se devuelve solo el primer resultado.

## Cláusula OFFSET

La cláusula `OFFSET` permite saltar un número determinado de filas antes de comenzar a devolver resultados.

Se utiliza normalmente junto con `LIMIT` para implementar paginación de resultados.

Sintaxis:

```
SELECT columnas
FROM tabla
LIMIT numero OFFSET desplazamiento;
```

Por ejemplo:

```
SELECT *
FROM producto
LIMIT 2 OFFSET 2;
```

Esta consulta lo que hace es saltarse los dos primeros registros y mostrar los dos siguientes.

## Uso de LIMIT y OFFSET para paginación

Cuando mostramos resultados en aplicaciones web es muy común dividirlos en páginas.

Por ejemplo, si quisiéramos monstrar los datos de una tabla de 10 en 10 registros:

Página 1:

```
SELECT *
FROM producto
LIMIT 10 OFFSET 0;
````

Página 2:

```
SELECT *
FROM producto
LIMIT 10 OFFSET 10;
```

Página 3:

```
SELECT *
FROM producto
LIMIT 10 OFFSET 20;
```

Este mecanismo permite mostrar los datos en bloques pequeños sin cargar toda la tabla.
