# Tipos de datos

* Introducción
* Tipos de Datos Numéricos
* Tipos de Datos de Fecha y Hora
* Tipos de Datos de Cadena (Texto y Binario)
* Tipos Espaciales (Geométricos)
* Tipos JSON
* Consideraciones al elegir Tipos de Datos
* Ejemplos prácticos

## Introducción

En MySQL, los tipos de datos definen el tipo de valor que puede almacenarse en una columna. Elegir el tipo de datos correcto es crucial para optimizar el rendimiento, el uso de memoria y la integridad de los datos.

## Clasificación de Tipos de Datos

MySQL agrupa sus tipos de datos en las siguientes categorías:

* Tipos de datos numéricos
* Tipos de datos de fecha y hora
* Tipos de datos de cadena (texto y binarios)
* Tipos espaciales (Geométricos)
* Tipos JSON

## Tipos de Datos Numéricos

Un __número entero__ es un tipo de dato numérico que representa valores sin decimales. En MySQL, los _números enteros_ se almacenan utilizando tipos de datos específicos que varían en tamaño y rango. Los tipos en MySQL para representar números enteros son:

| **Tipo**   | **Tamaño en bytes** | **Rango (sin signo)**            | **Rango (con signo)**                                  | **Descripción**                     |
|------------|---------------------|----------------------------------|--------------------------------------------------------|-------------------------------------|
| `TINYINT`  | 1                   | 0 a 255                          | -128 a 127                                             | Entero muy pequeño.                 |
| `SMALLINT` | 2                   | 0 a 65,535                       | -32,768 a 32,767                                       | Entero pequeño.                     |
| `MEDIUMINT`| 3                   | 0 a 16,777,215                   | -8,388,608 a 8,388,607                                 | Entero mediano.                     |
| `INT`      | 4                   | 0 a 4,294,967,295                | -2,147,483,648 a 2,147,483,647                         | Entero estándar (alias: `INTEGER`). |
| `BIGINT`   | 8                   | 0 a 18,446,744,073,709,551,615   | -9,223,372,036,854,775,808 a 9,223,372,036,854,775,807 | Entero grande.                      |

Un __número decimal__ es un tipo de dato numérico que representa valores con decimales. En MySQL, los _números decimales_ se almacenan utilizando tipos de datos específicos que varían en tamaño y rango. Los tipos en MySQL para representar _números decimales_ son son:

| **Tipo**    | **Tamaño en bytes** | **Rango (Aproximado)**         | **Descripción**                                             |
|-------------|---------------------|--------------------------------|-------------------------------------------------------------|
| `FLOAT`     | 4                   | ±1.5e-45 a ±3.4e38            | Número de punto flotante de precisión simple.                |
| `DOUBLE`    | 8                   | ±2.2e-308 a ±1.8e308          | Número de punto flotante de precisión doble (alias: `REAL`). |
| `DECIMAL`   | Variable            | Dependiente de la precisión   | Número decimal de precisión fija (alias: `NUMERIC`).         |

Usar `FLOAT`:
* Cuando necesitas ahorrar espacio en memoria.
* Posibles errores por redondeo.
* Cuando no es esencial la precisión absoluta (ejemplo: coordenadas GPS o promedios).
* Uso de memoria bajo (4 bytes).

Usar `DOUBLE`:
* Para cálculos científicos o cuando se requiere mayor rango y precisión que FLOAT, por ejemplo, modelos matemáticos.
* Posibles errores por redondeo, pero menos que `FLOAT`.
* Uso de memoria moderado (8 bytes).

Usar `DECIMAL`:
* En cálculos financieros o contables donde la precisión es crucial, por ejemplo, totales, precios, cálculos de impuestos.
* No existen errores de redonde, dato exacto.
* El uso de memoria depende de la precisión.

## Tipos de Datos de Fecha y Hora

Las __fechas__ y __horas__ en MySQL se almacenan utilizando tipos de datos específicos. Los tipos en MySQL para representar fechas y tiempo son:

| **Tipo**      | **Tamaño (bytes)** | **Formato**              | **Rango**                                             | **Descripción**                              |
|---------------|--------------------|--------------------------|-------------------------------------------------------|----------------------------------------------|
| `DATE`        | 3                  | `YYYY-MM-DD`             | `1000-01-01` a `9999-12-31`                           | Fecha sin hora.                              |
| `DATETIME`    | 8                  | `YYYY-MM-DD HH:MM:SS`    | `1000-01-01 00:00:00` a `9999-12-31 23:59:59`         | Fecha y hora combinadas.                     |
| `TIMESTAMP`   | 4                  | `YYYY-MM-DD HH:MM:SS`    | `1970-01-01 00:00:01` UTC a `2038-01-19 03:14:07` UTC | Fecha y hora con zona horaria.               |
| `TIME`        | 3                  | `HH:MM:SS`               | `-838:59:59` a `838:59:59`                            | Intervalos de tiempo o duración.             |
| `YEAR`        | 1                  | `YYYY`                   | `1901` a `2155`                                       | Años representados en formato de 4 dígitos.  |

## Tipos de Datos de Cadena (Texto y Binarios)

Para almacenar __cadenas de texto__, MySQL nos ofrece los siguientes tipos:

| **Tipo**       | **Tamaño**          | **Descripción**                                        |
|----------------|---------------------|--------------------------------------------------------|
| `CHAR`         | Longitud fija       | Cadena de longitud fija (hasta 255 caracteres).        |
| `VARCHAR`      | Longitud variable   | Cadena de longitud variable (hasta 65,535 caracteres). |
| `TEXT`         | Longitud variable   | Almacena grandes cantidades de texto (hasta 4GB).      |
| `TINYTEXT`     | 255 bytes           | Texto muy pequeño.                                     |
| `MEDIUMTEXT`   | Hasta 16MB          | Texto de tamaño mediano.                               |
| `LONGTEXT`     | Hasta 4GB           | Texto muy largo.                                       |

Para almacenar binarios (imágenes, archivos, ...), MySQL nos ofrece los siguientes tipos:

| **Tipo**       | **Tamaño**          | **Descripción**                       |
|----------------|---------------------|---------------------------------------|
| `BINARY`       | Longitud fija       | Datos binarios de longitud fija.      |
| `VARBINARY`    | Longitud variable   | Datos binarios de longitud variable.  |
| `TINYBLOB`     | 255 bytes           | Datos binarios muy pequeños.          |
| `BLOB`         | Hasta 65,535 bytes  | Datos binarios grandes.               |
| `MEDIUMBLOB`   | Hasta 16MB          | Datos binarios de tamaño mediano.     |
| `LONGBLOB`     | Hasta 4GB           | Datos binarios muy grandes.           |

## Tipos Espaciales (Geométricos)

Los __datos espaciales__ (o _geométricos_) son tipos de datos diseñados para almacenar información sobre ubicaciones, formas, y estructuras espaciales en un espacio bidimensional (2D).

| **Tipo**      | **Descripción**                                         |
|---------------|---------------------------------------------------------|
| `GEOMETRY`    | Representa datos geométricos en general.                |
| `POINT`       | Representa un punto en un espacio 2D.                   |
| `LINESTRING`  | Representa una línea compuesta por uno o más puntos.    |
| `POLYGON`     | Representa un polígono definido por múltiples puntos.   |

Usos comunes:
* __Ubicación geográfica__: coordenadas GPS de un lugar (`POINT`).
* __Rutas y trayectorias__: líneas que representan caminos o trayectorias (`LINESTRING`).
* __Áreas delimitadas__: polígonos para representar límites geográficos o zonas específicas (`POLYGON`).

## Tipos JSON

__JSON__ (_JavaScript Object Notation_) es un formato ligero para estructurar datos basado en texto. Es ampliamente utilizado en aplicaciones web y APIs para intercambiar datos.

| **Tipo**      | **Descripción**                                                                 |
|---------------|---------------------------------------------------------------------------------|
| `JSON`        | Almacena datos en formato JSON, compatible con operaciones específicas de MySQL.|

MySQL incluye funciones específicas para manipular datos JSON, como acceder a claves, filtrar arrays, y modificar estructuras. MySQL valida que los datos almacenados en un campo JSON tengan un formato JSON válido.

## Consideraciones al elegir Tipos de Datos

Tamaño y rendimiento:
* Elige el tipo más pequeño que cumpla tus requisitos para optimizar el uso de memoria.
* Ejemplo: Usa TINYINT en lugar de INT si los valores están en el rango de 0 a 255.

Precisión:
* Usa DECIMAL para cálculos financieros donde la precisión es crítica.
* Evita FLOAT y DOUBLE para valores que requieren precisión exacta.

Compatibilidad:
* Para texto, usa CHAR si las longitudes son consistentes, o VARCHAR si varían.

Fecha y hora:
* Usa DATETIME si necesitas almacenar fechas completas con horas.
* Usa TIMESTAMP si necesitas registrar datos con zona horaria.

## Ejemplos prácticos

Crear una tabla con diferentes tipos de datos

```sql
CREATE TABLE empleados (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50),
    salario DECIMAL(10, 2),
    fecha_contratacion DATE,
    horario TIME
);

INSERT INTO empleados (nombre, salario, fecha_contratacion, horario) VALUES
('Juan Pérez', 2500.50, '2023-01-15', '09:00:00'),
('Ana Gómez', 3000.75, '2022-11-20', '08:30:00'),
('Luis Martínez', 1800.00, '2021-06-10', '10:00:00'),
('María Fernández', 3200.90, '2023-03-01', '09:15:00'),
('Carlos López', 2000.00, '2020-09-05', '07:45:00');
```

Almacenar datos en formato _JSON_:

```sql
CREATE TABLE configuraciones (
    id INT AUTO_INCREMENT PRIMARY KEY,
    datos JSON
);

INSERT INTO configuraciones (datos) VALUES ('{"theme": "dark", "fontSize": 14}');

SELECT id, JSON_EXTRACT(datos, '$.theme') AS tema FROM configuraciones;
+----+-------+
| id | tema  |
+----+-------+
| 1  | dark  |
+----+-------+
```

Almacenar datos espaciales:

```sql
CREATE TABLE ubicaciones (
    id INT AUTO_INCREMENT PRIMARY KEY,
    coordenadas POINT
);

INSERT INTO ubicaciones (coordenadas) VALUES
(ST_GeomFromText('POINT(40.416775 -3.703790)')), -- Madrid, España
(ST_GeomFromText('POINT(48.856613 2.352222)')),  -- París, Francia
(ST_GeomFromText('POINT(51.507222 -0.127500)')), -- Londres, Reino Unido
(ST_GeomFromText('POINT(34.052235 -118.243683)')), -- Los Ángeles, EE. UU.
(ST_GeomFromText('POINT(-33.868820 151.209290)')); -- Sídney, Australia
```
