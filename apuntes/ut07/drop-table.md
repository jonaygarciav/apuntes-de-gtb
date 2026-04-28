# Comando DROP TABLE


## Introducción

El comando `DROP TABLE` se utiliza para eliminar completamente una tabla de la base de datos.

A diferencia de otros comandos como `DELETE` o `TRUNCATE`:

* `DELETE`: elimina los datos, pero mantiene la tabla
* `TRUNCATE`: vacía la tabla, pero conserva su estructura
* `DROP TABLE`: elimina estructura + datos + índices + relaciones

> __Nota__: esta acción es irreversible. No se puede recuperar la tabla una vez eliminada.

Sintaxis básica:

```
DROP TABLE nombre_tabla;
```

Ejemplo:

```
mysql> DROP TABLE alumnos;
Query OK, 0 rows affected (0.01 sec)
```

La tabla `alumnos` desaparece completamente de la base de datos.

## DROP TABLE IF EXISTS

Se utiliza para evitar errores si la tabla no existe.

Sintaxis:

```
DROP TABLE IF EXISTS nombre_tabla;
```

Ejemplo:

```
mysql> DROP TABLE IF EXISTS alumnos;
```

* Si la tabla existe, se elimina.
* Si no existe, no da error.

## Eliminar varias tablas

Se pueden eliminar varias tablas a la vez separándolas por comas.

Sintaxis:

```
DROP TABLE tabla1, tabla2, tabla3;
```

Ejemplo:

```
mysql> DROP TABLE alumnos, profesores;
DROP TABLE con claves foráneas
```

Si una tabla está relacionada con otra mediante una `clave foránea`, puede dar error al eliminarla.

Ejemplo de error:

```
ERROR 1217 (23000): Cannot delete or update a parent row
```

Soluciones:

__Opción 1__: eliminar primero la tabla dependiente

```
DROP TABLE pedidos;
DROP TABLE clientes;
```

__Opción 2__: desactivar temporalmente las restricciones

```
SET FOREIGN_KEY_CHECKS = 0;

DROP TABLE clientes;

SET FOREIGN_KEY_CHECKS = 1;
```

> __Nota__: usar con cuidado: puedes romper la integridad de los datos.

## Diferencia con TRUNCATE

| Comando    | ¿Elimina datos? | ¿Elimina estructura? |
|------------|----------------|---------------------|
| DELETE     | Sí             | No                  |
| TRUNCATE   | Sí             | No                  |
| DROP TABLE | Sí             | Sí                  |
