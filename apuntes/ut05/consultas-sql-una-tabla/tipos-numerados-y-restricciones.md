# Tipos numerados y restricciones

* [Introducción](#introducción)
* [Tipo ENUM](#tipo-enum)
  * [Cuándo usar y cuándo no user el tipo ENUM](#cuándo-usar-y-cuándo-no-user-el-tipo-enum)
* [Tipo SET](#tipo-set)
  * [Cuándo usar y cuando no usar el tipo SET](#cuándo-usar-y-cuando-no-usar-el-tipo-set)
* [Restricciones CHECK](#restricciones-check)
  * [Cuándo usar la restricción CHECK](#cuándo-usar-la-restricción-check)
* [Comparación entre tipo ENUM, tipo SET y restricción CHECK](#comparación-entre-tipo-enum-tipo-set-y-restricción-check)

## Introducción

En bases de datos es muy común que ciertas columnas solo admitan un conjunto limitado de valores:

* El estado de un pedido: pendiente, enviado, entregado
* La prioridad de un ticket: baja, media, alta
* Los permisos de un usuario: leer, escribir, admin

MySQL ofrece varias formas de controlar esto:
* `ENUM`: un único valor dentro de una lista.
* `SET`: varios valores (una "lista") dentro de una lista predefinida.
* `CHECK`: una regla/condición que debe cumplirse para aceptar el dato (MySQL 8.0+).

La idea es mejorar la __integridad de los datos__: que en la tabla no se inserten valores inventados, inconsistentes o mal escritos.

## Tipo ENUM

El `tipo ENUM` permite definir una columna cuyos valores posibles están limitados a una lista concreta de valores especificada al crear la tabla. Cada registro de la tabla solo podrá contener uno de los valores definidos en el `tipo ENUM`.

Este tipo de datos es muy útil cuando conocemos de antemano todos los valores posibles y sabemos que no van a cambiar con frecuencia.

Ejemplos de uso comunes:

* estado de un pedido
* prioridad de una incidencia
* tipo de usuario
* nivel de acceso

Crearemos una base de datos de ejemplo llamada `tienda`, que utilizaremos para realizar algunas pruebas:

```
DROP DATABASE IF EXISTS tienda;
CREATE DATABASE tienda CHARACTER SET utf8mb4;
USE tienda;
```

A continuación, creamos una tabla llamada `pedido` que almacenará información sobre pedidos realizados por los clientes. La tabla tendrá las siguientes columnas:

* `id`: identificador del pedido
* `cliente`: nombre del cliente
* `estado`: estado del pedido (utilizando ENUM)
* `fecha`: fecha del pedido

```
CREATE TABLE pedido (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  cliente VARCHAR(100) NOT NULL,
  estado ENUM('pendiente','enviado','entregado','cancelado') NOT NULL,
  fecha DATE NOT NULL
);
```

En este caso la columna estado se define como un `tipo ENUM` que solamente admite los siguientes valores: _pendiente_, _enviado_, _entregado_ y _cancelado_.

> __Nota__: cualquier valor distinto será rechazado por la base de datos.

Una vez creada la tabla, podemos insertar datos que respeten las restricciones definidas en el campo de `tipo ENUM`.

En el siguiente ejemplo insertamos tres pedidos cuyos estados coinciden con los valores permitidos:

```
INSERT INTO pedido (cliente, estado, fecha)
VALUES
('Ana', 'pendiente', '2026-03-01'),
('Luis', 'enviado',   '2026-03-02'),
('Marta','entregado', '2026-03-03');
```

En esta inserción:
* El pedido de Ana tiene estado _pendiente_.
* El pedido de Luis tiene estado _enviado_.
* El pedido de Marta tiene estado _entregado_.

Todos estos valores forman parte de la lista definida en el `tipo ENUM`, por lo que la inserción se realiza correctamente.

Si consultamos la tabla veremos los datos almacenados:

```
SELECT * FROM pedido;
```

Ahora vamos a intentar insertar un valor que no está permitido en el `tipo ENUM`.

```
INSERT INTO pedido (cliente, estado, fecha)
VALUES ('Pepe', 'en reparto', '2026-03-04');
```

En este caso el valor `en reparto` no pertenece al `tipo ENUM`, por lo que MySQL generará un error:

```
ERROR 1265 (01000): Data truncated for column 'estado' at row 1
```

> __Nota__: Este error indica que el valor que estamos intentando insertar no coincide con ninguno de los valores definidos en el `tipo ENUM`, por lo que MySQL no puede almacenarlo correctamente.

Este comportamiento es muy útil porque evita que se introduzcan valores incorrectos o inconsistentes en la base de datos.

Las columnas de `tipo ENUM` pueden utilizarse en consultas exactamente igual que cualquier otra columna de texto. Por ejemplo, podemos obtener todos los pedidos pendientes:

```
SELECT *
FROM pedido
WHERE estado = 'pendiente';
```

También podemos buscar todos los pedidos que no estén cancelados:

```
SELECT *
FROM pedido
WHERE estado <> 'cancelado';
```

### Cuándo usar y cuándo no user el tipo ENUM

El `tipo ENUM` es recomendable cuando:

* El conjunto de valores posibles es pequeño.
* Los valores no cambian con frecuencia.
* Queremos evitar errores de escritura en los datos.
* Los valores tienen significado directo en la aplicación.

No es recomendable usar el `tipo ENUM` cuando:

* Los valores cambian con frecuencia.
* Los valores deben almacenarse en una tabla independiente.
* Necesitamos relacionar esos valores con otras tablas.
* Los valores tienen más información asociada.

En estos casos suele ser mejor crear una tabla de referencia. Esto lo veremos más adelante.

## Tipo SET

El `tipo SET` permite almacenar uno o varios valores simultáneamente dentro de una lista definida. A diferencia del `tipo ENUM`, donde solo se puede elegir un valor, en `tipo SET` se pueden seleccionar varios valores a la vez: es similar a marcar varias casillas en un formulario.

Vamos a crear una tabla llamada `usuario` que almacenará los permisos de cada usuario.

```
CREATE TABLE usuario (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  permisos SET('leer','escribir','admin') NOT NULL
);
```

La columna permisos puede contener uno o varios de los siguientes valores:

* leer
* escribir
* admin


Podemos insertar usuarios con distintos permisos_

```
INSERT INTO usuario (nombre, permisos)
VALUES ('Ana', 'leer');
```

En este caso el usuario `Ana` solamente tiene permiso de `lectura`.

También podemos insertar varios permisos simultáneamente:

```
INSERT INTO usuario (nombre, permisos)
VALUES ('Luis', 'leer,escribir');
```

Esto indica que `Luis` tiene permisos de `lectura` y `escritura`.

Para comprobar si un usuario tiene un permiso concreto se utiliza la función `FIND_IN_SET`, por ejemplo, si queremos obtener los usuarios con permiso de administrador:

```
SELECT *
FROM usuario
WHERE FIND_IN_SET('admin', permisos);
```

### Cuándo usar y cuando no usar el tipo SET

El `tipo SET` es útil cuando:

* Los valores representan opciones múltiples.
* El conjunto de opciones es pequeño.
* No necesitamos relaciones complejas con otras tablas.

Ejemplos:

permisos_usuario
etiquetas_producto
opciones_configuracion

Evitar usar el `tipo SET` cuando:

* Se necesita una estructura relacional correcta.
* Los valores pueden cambiar con frecuencia.
* Necesitamos hacer muchas consultas complejas.

En esos casos es mejor usar una tabla intermedia.

## Restricciones CHECK

La `restricción CHECK` permite definir una condición lógica que debe cumplirse para aceptar un valor en una columna. Si la condición no se cumple, la base de datos rechazará la operación `INSERT` o `UPDATE`. Esta característica está disponible en MySQL 8.0 y versiones posteriores.

Por ejemplo, creamos la tabla `ticket` con el campo `prioridad` como `restricción CHECK`:

```
CREATE TABLE ticket (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  asunto VARCHAR(200) NOT NULL,
  prioridad VARCHAR(10) NOT NULL,
  CHECK (prioridad IN ('baja','media','alta'))
);
```

En esta tabla la columna `prioridad` solamente puede contener los valores:

* baja
* media
* alta

Una vez creada la tabla, podemos insertar registros que cumplan las reglas establecidas en la `restricción CHECK`:

```
INSERT INTO ticket (asunto, prioridad)
VALUES ('No funciona el login', 'alta');
```

En este ejemplo estamos insertando un ticket con la siguiente información:

* __asunto__: "No funciona el login"
* __prioridad__: "alta"

La inserción se realiza correctamente porque el valor `alta` cumple la condición definida en la `restricción CHECK`.

Ahora vamos a intentar insertar un registro que no cumple la condición definida en la `restricción CHECK`:

```
INSERT INTO ticket (asunto, prioridad)
VALUES ('Pantalla en blanco', 'urgente');
```

En este caso estamos intentando asignar la prioridad `urgente`.

Sin embargo, este valor no pertenece a la lista de valores permitidos, que recordemos son:

* baja
* media
* alta

Como consecuencia, MySQL rechazará la operación y generará un error similar al siguiente:

```
ERROR 3819 (HY000): Check constraint 'ticket_chk_1' is violated
```

Este error indica que la operación ha sido cancelada porque se ha incumplido una `restricción CHECK` definida en la tabla.

Gracias a este mecanismo, la base de datos evita que se almacenen datos incorrectos o inconsistentes.


### Cuándo usar la restricción CHECK

La `restricción CHECK` es recomendable cuando:

* Queremos validar valores con una condición lógica.
* Queremos evitar valores incorrectos.
* Buscamos una solución compatible con otros sistemas gestores de bases de datos.

Ejemplos de uso:

```
edad >= 0
precio > 0
nota BETWEEN 0 AND 10
estado IN ('activo','inactivo')
```

## Comparación entre tipo ENUM, tipo SET y restricción CHECK

| Tipo  | Característica                                            |
| ----- | --------------------------------------------------------- |
| ENUM  | Permite un único valor de una lista                       |
| SET   | Permite varios valores de una lista                       |
| CHECK | Permite definir una condición que deben cumplir los datos |
