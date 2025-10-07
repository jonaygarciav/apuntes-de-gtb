# Sistemas de Almacenamiento de la Información

* [Ficheros](#ficheros)
  * [Ficheros Planos](#ficheros-planos)
  * [Ficheros Indexados](#ficheros-indexados)

## Ficheros

Los __ficheros__ son la forma más básica de almacenamiento de datos en un sistema informático. Existen diferentes tipos de ficheros según su estructura y la forma en que gestionan el acceso a la información:

### Ficheros Planos 

Son archivos simples donde los datos se almacenan en texto plano, sin ningún tipo de formato estructurado. Los datos se organizan en líneas, separadas por delimitadores (como comas en un archivo CSV). Se utilizan para almacenar configuraciones simples, logs de sistemas, o pequeños conjuntos de datos.

Ventajas:

* _Simplicidad y facilidad de uso_: los ficheros planos son fáciles de crear y utilizar. No requieren software especializado ni estructuras complejas para gestionar los datos. Pueden ser creados y editados con cualquier editor de texto básico, como Bloc de Notas, Notepad, Vim, o incluso aplicaciones de hojas de cálculo como Excel para  archivos CSV.
* _Compatibilidad y Portabilidad_: los ficheros planos son altamente portables entre diferentes sistemas operativos y plataformas. Se pueden transferir fácilmente entre sistemas Windows, Linux, MacOS y otros. No requieren drivers específicos ni software adicional para ser leídos en distintas plataformas.
* _Portabilidad_: pueden ser abiertos en cualquier sistema.
* _Bajo Consumo de recursos_: consumen pocos recursos del sistema, ya que no necesitan procesos adicionales de indexación, gestión de transacciones o bloqueo de registros. Adecuados para dispositivos de bajos recursos o sistemas embebidos.
* _Transparencia de los datos_: los datos se almacenan de forma legible. No hay necesidad de herramientas complejas para entender los datos, lo que es útil para tareas de verificación rápida.
* _Flexibilidad en el formato de datos_: no están sujetos a esquemas rígidos, lo que permite almacenar cualquier tipo de información en formato libre. Permiten añadir nuevos registros sin necesidad de alterar estructuras predefinidas.
* _Facilidad para automatización_: pueden ser fácilmente manipulados mediante scripts en lenguajes como Python, Bash o PowerShell, lo que facilita tareas automatizadas de lectura y escritura.

Desventajas:

* _Falta de estructura y relaciones_: no soportan estructuras complejas, como tablas relacionadas, claves primarias o ajenas, lo que limita su uso a casos muy básicos. Los datos suelen ser redundantes y difíciles de relacionar, lo que provoca inconsistencias.
* _Escalabilidad y rendimiento_: a medida que el tamaño del archivo crece, el rendimiento disminuye significativamente, ya que no existen mecanismos de indexación o búsqueda rápida. La lectura y escritura de grandes volúmenes de datos pueden volverse lentas, especialmente cuando se buscan registros específicos.
* _Seguridad y control de acceso_: carecen de mecanismos integrados para la gestión de permisos de acceso. Cualquiera que tenga acceso al archivo puede modificarlo. No ofrecen encriptación de datos de manera nativa, exponiendo la información a posibles accesos no autorizados.
* _Falta de integridad y control de errores_: no garantizan la integridad de los datos. No existen restricciones como las de una base de datos relacional (restricciones de tipo de datos, reglas de validación). La modificación manual de los datos puede llevar a errores fácilmente, como duplicados o registros mal formateados.
* _Dificultad en el mantenimiento_: a medida que los archivos crecen, se vuelve difícil mantener y organizar la información de manera coherente. El mantenimiento y limpieza de los datos, como la eliminación de duplicados o la corrección de errores, suele requerir procesos manuales o scripts adicionales.
* _Sin soporte para transacciones_: no pueden manejar operaciones transaccionales (conjunto de operaciones que deben completarse todas o ninguna), lo que puede llevar a estados inconsistentes si ocurre un error durante la modificación. No hay soporte para rollback (deshacer cambios) en caso de errores, lo que puede ser crítico en aplicaciones más sensibles.
* _Sin herramientas nativas para consultas complejas_: las consultas complejas requieren procesamiento manual o la creación de programas específicos, ya que no se puede realizar filtrado, agrupamiento o ordenamiento de forma eficiente sin leer el archivo completo. La ausencia de lenguajes de consulta estructurados como SQL dificulta la extracción de datos específicos.

Ejemplos de formatos de archivos planos: archivos `.txt`, `.csv`, `.log`.

Supongamos que los datos a almacenar son los siguientes:

| ID | Nombre          | Correo                   | Fecha de Registro |
|----|-----------------|--------------------------|-------------------|
| 1  | Juan Pérez      | juan.perez@mail.com      | 2024-09-15        |
| 2  | María García    | maria.garcia@mail.com    | 2024-09-16        |
| 3  | Carlos Sánchez  | carlos.sanchez@mail.com  | 2024-09-17        |
| 4  | Ana López       | ana.lopez@mail.com       | 2024-09-18        |
| 5  | Luis Fernández  | luis.fernandez@mail.com  | 2024-09-19        |

__Archivo .txt__

El archivo de texto plano simplemente presenta los datos en un formato legible, separados por un tabulador, espacios o cualquier otro separador básico.

Contenido del archivo `clientes.txt`:

```
ID: 1  Nombre: Juan Pérez     Correo: juan.perez@mail.com     Fecha de Registro: 2024-09-15
ID: 2  Nombre: María García   Correo: maria.garcia@mail.com   Fecha de Registro: 2024-09-16
ID: 3  Nombre: Carlos Sánchez Correo: carlos.sanchez@mail.com Fecha de Registro: 2024-09-17
ID: 4  Nombre: Ana López      Correo: ana.lopez@mail.com      Fecha de Registro: 2024-09-18
ID: 5  Nombre: Luis Fernández Correo: luis.fernandez@mail.com Fecha de Registro: 2024-09-19
```

__Archivo .csv__

El archivo CSV (Comma Separated Values) utiliza el carácter `,` como delimitador por defecto (aunque se puede configurar cualquier carácter) y es utilizado para intercambiar datos entre diferentes programas.

Contenido del archivo `clientes.csv`:

```
ID,Nombre,Correo,Fecha de Registro
1,Juan Pérez,juan.perez@mail.com,2024-09-15
2,María García,maria.garcia@mail.com,2024-09-16
3,Carlos Sánchez,carlos.sanchez@mail.com,2024-09-17
4,Ana López,ana.lopez@mail.com,2024-09-18
5,Luis Fernández,luis.fernandez@mail.com,2024-09-19
```

__Archivo .log__

El archivo `.log` suele usarse para almacenar registros o eventos. Puede incluir información adicional como la hora en que se registraron los datos, y cada línea puede estar precedida por una marca de tiempo o información contextual.

Contenido del archivo `clientes.log`:

```
[2024-09-15 10:00:00] INFO: ID=1, Nombre=Juan Pérez, Correo=juan.perez@mail.com, Fecha de Registro=2024-09-15
[2024-09-16 11:30:00] INFO: ID=2, Nombre=María García, Correo=maria.garcia@mail.com, Fecha de Registro=2024-09-16
[2024-09-17 09:45:00] INFO: ID=3, Nombre=Carlos Sánchez, Correo=carlos.sanchez@mail.com, Fecha de Registro=2024-09-17
[2024-09-18 15:15:00] INFO: ID=4, Nombre=Ana López, Correo=ana.lopez@mail.com, Fecha de Registro=2024-09-18
[2024-09-19 19:25:00] INFO: ID=5, Nombre=Luis Fernández, Correo=luis.fernandez@mail.com, Fecha de Registro=2024-09-19
```

Ejemplo de script en Python que genera un fichero `.csv` con los registros de la tabla de ejemplo:

```python
import csv

# Definir los datos para los 5 registros
registros = [
    {"id": 1, "nombre": "Juan Pérez", "correo": "juan.perez@mail.com", "fecha_registro": "2024-09-15"},
    {"id": 2, "nombre": "María García", "correo": "maria.garcia@mail.com", "fecha_registro": "2024-09-16"},
    {"id": 3, "nombre": "Carlos Sánchez", "correo": "carlos.sanchez@mail.com", "fecha_registro": "2024-09-17"},
    {"id": 4, "nombre": "Ana López", "correo": "ana.lopez@mail.com", "fecha_registro": "2024-09-18"},
    {"id": 5, "nombre": "Luis Fernández", "correo": "luis.fernandez@mail.com", "fecha_registro": "2024-09-19"}
]

# Variables de configuración
nombre_fichero = 'clientes.csv'  # Nombre del archivo CSV
delimitador = ','                # Especificar el caracter usado como delimitador
copiar_cabecera = False          # Establecer a True para copiar la cabecera, False en caso contrario

# Abrir y escribir en el archivo CSV
with open(nombre_fichero, mode='w', newline='', encoding='utf-8') as fichero:
    # Definir los nombres de las columnas
    campos = ['id', 'nombre', 'correo', 'fecha_registro']
    
    # Crear el objeto escritor con el delimitador especificado
    escritor = csv.DictWriter(fichero, fieldnames=campos, delimiter=delimitador)
    
    # Escribir la cabecera si se ha especificado en la variable
    if copiar_cabecera:
        escritor.writeheader()
    
    # Escribir los registros en el archivo
    for registro in registros:
        escritor.writerow(registro)

print(f"Fichero '{nombre_fichero}' creado y registros insertados correctamente.")
```

Para ejecutar el script en Linux:

```bash
$ python3 insertar_registros.py
Fichero 'clientes.csv' creado y registros insertados correctamente.

$ cat clientes.csv
id,nombre,correo,fecha_registro
1,Juan Pérez,juan.perez@mail.com,2024-09-15
2,María García,maria.garcia@mail.com,2024-09-16
3,Carlos Sánchez,carlos.sanchez@mail.com,2024-09-17
4,Ana López,ana.lopez@mail.com,2024-09-18
5,Luis Fernández,luis.fernandez@mail.com,2024-09-19
```

> __Nota__: los archivos con extensión `.csv` se pueden abrir con _LibreOffice Calc_.

* __.txt__: formato libre y simple, usado para almacenar datos de manera legible sin una estructura estricta. Es fácil de leer y editar manualmente.
* __.csv__: estandarizado para ser importado y exportado por muchas aplicaciones (como Excel), ideal para tablas de datos con delimitadores claros (comas).
* __.log__: usado principalmente para seguimiento de eventos o auditoría, puede incluir información adicional como timestamps o niveles de registro (INFO, ERROR).

### Ficheros Indexados

Son archivos que contienen índices que permiten la búsqueda rápida de registros. Los índices actúan como punteros que dirigen a la ubicación exacta de los datos dentro del archivo. Se utilizan en aplicaciones donde se requiere acceso rápido a registros específicos, como catálogos de productos o sistemas de contabilidad.

Ventajas:

* __Acceso rápido a los datos__: los ficheros indexados utilizan índices que actúan como "puntos de acceso" hacia los registros dentro del archivo. Esto permite localizar rápidamente un registro sin tener que leer todo el archivo de manera secuencial. Es especialmente beneficioso cuando se manejan grandes volúmenes de datos y se necesitan respuestas rápidas para consultas específicas.
* __Eficiencia en las búsquedas__: gracias a la estructura de índices, las búsquedas de registros específicos son mucho más rápidas en comparación con los ficheros planos. Esto se logra porque el índice reduce significativamente el número de lecturas necesarias para encontrar un registro. Soportan búsquedas complejas, como búsquedas por campos no secuenciales, lo cual no es posible en ficheros secuenciales tradicionales.
* __Actualización y modificación de datos más eficiente__: permiten la inserción, actualización y eliminación de registros de manera más eficiente ya que no es necesario reorganizar todo el archivo tras cada operación; solo se actualizan los índices correspondientes. Pueden mantener múltiples índices para diferentes campos, facilitando operaciones que impliquen varios criterios de búsqueda.
* __Mejora del Rendimiento General del Sistema__: al reducir el tiempo de acceso a los datos, mejoran el rendimiento de las aplicaciones que dependen de la lectura y manipulación frecuente de archivos de gran tamaño. Ideal para aplicaciones que requieren respuestas en tiempo real o semi-tiempo real, como sistemas de ventas, inventarios o control de acceso.
* __Flexibilidad en la gestión de datos__: permiten el acceso tanto secuencial como aleatorio, lo cual es útil para operaciones mixtas de lectura, donde algunas operaciones pueden beneficiarse de un acceso secuencial y otras de uno directo. Soportan múltiples tipos de organización de índices (B-trees, hash tables, etc.), que pueden  optimizarse según los patrones de acceso.
* __Reducción del consumo de recursos__: aunque gestionan índices adicionales, la eficiencia en el acceso a los datos puede reducir significativamente el tiempo de CPU y el uso de disco en comparación con accesos secuenciales completos.

Desventajas:

* __Mayor uso de espacio en disco__: los índices adicionales ocupan espacio adicional en el disco. En algunos casos, los índices pueden ser casi tan grandes como los datos originales, especialmente si se manejan índices complejos o múltiples índices sobre los mismos datos. A medida que crece la cantidad de datos, el tamaño de los índices también aumenta, lo cual puede generar problemas de almacenamiento.
* __Mantenimiento Complejo__: los índices deben actualizarse cada vez que se realiza una operación de inserción, eliminación o modificación de datos, lo cual puede añadir complejidad y tiempo adicional al mantenimiento del archivo. La reconstrucción de índices (cuando los datos cambian significativamente) puede ser un proceso costoso en términos de tiempo y recursos.
* __Posibles Problemas de consistencia__: si los índices no se gestionan correctamente, pueden quedar desactualizados o inconsistentes respecto a los datos reales, lo que podría causar errores en las búsquedas o en la recuperación de datos. La corrupción de un índice puede requerir su reconstrucción completa, lo cual puede ser un proceso largo y complejo.
* __Dificultad en la gestión de múltiples índices__: mantener varios índices sobre el mismo archivo puede incrementar la complejidad de la administración y la posibilidad de errores. Es necesario un control cuidadoso para asegurar que todos los índices se actualicen correctamente en cada operación de escritura. Decidir qué campos deben ser indexados es crucial, ya que índices innecesarios o mal diseñados pueden degradar el rendimiento en lugar de mejorarlo.
* __Rendimiento Variable en Inserciones y Eliminaciones__: las operaciones de inserción y eliminación pueden ser lentas debido a la necesidad de actualizar uno o más índices. En casos extremos, las inserciones de grandes cantidades de datos pueden provocar tiempos de espera elevados. Operaciones que afectan los índices, como reindexar o reconstruir, pueden bloquear el acceso a los datos temporalmente, impactando la disponibilidad.
* __Complejidad en la Recuperación de Fallos__: en caso de fallos del sistema o errores en el disco, no solo los datos, sino también los índices pueden verse afectados, complicando la recuperación de información. La pérdida o corrupción de un índice puede dificultar o imposibilitar el acceso a los datos hasta que se reconstruya adecuadamente.
* __Mayor complejidad en la implementación__: crear y gestionar ficheros indexados requiere una implementación más sofisticada que la de ficheros planos. Esto incluye el desarrollo de mecanismos para la actualización y reconstrucción de índices, así como para la sincronización entre los datos y los índices. Puede requerir conocimientos avanzados de algoritmos de búsqueda y estructuras de datos, lo cual puede no ser ideal en contextos donde la simplicidad es clave.

Ejemplos de archivos de índíces: Hash, B-tree B+ tree.

__Archivo Indexado con Hash__

Para este ejemplo, usaremos una función hash básica: _hash(ID) = ID % 5_, que asigna cada registro a una posición específica en el archivo, gestionando las colisiones de manera lineal.

Estructura del Archivo con Índice Hash:

Datos Almacenados (posiciones asignadas según hash):

```
Registro 0:
- Ningún registro (hash = 0 no apunta a datos directos)

Registro 1:
- ID: 1, Nombre: Juan Pérez, Correo: juan.perez@mail.com, Fecha de Registro: 2024-09-15 (hash(1) = 1)

Registro 2:
- ID: 2, Nombre: María García, Correo: maria.garcia@mail.com, Fecha de Registro: 2024-09-16 (hash(2) = 2)

Registro 3:
- ID: 3, Nombre: Carlos Sánchez, Correo: carlos.sanchez@mail.com, Fecha de Registro: 2024-09-17 (hash(3) = 3)

Registro 4:
- ID: 4, Nombre: Ana López, Correo: ana.lopez@mail.com, Fecha de Registro: 2024-09-18 (hash(4) = 4)

Registro 0 (colisión manejada):
- ID: 5, Nombre: Luis Fernández, Correo: luis.fernandez@mail.com, Fecha de Registro: 2024-09-19 (hash(5) = 0)
```

__Archivo Indexado con B-tree__

Balancea los registros para optimizar la búsqueda y modificación de datos. Los nodos tienen forma de árbol contienen claves y punteros a sus hijos.

Estructura del Archivo con Índice B-tree:

```
      [ID 3]
     /       \
(ID 1, 2) (ID 4, 5)
```

Nodo central:
* ID: 3, Nombre: Carlos Sánchez, Correo: carlos.sanchez@mail.com, Fecha de Registro: 2024-09-17

Nodo izquierdo:
* ID: 1, Nombre: Juan Pérez, Correo: juan.perez@mail.com, Fecha de Registro: 2024-09-15
* ID: 2, Nombre: María García, Correo: maria.garcia@mail.com, Fecha de Registro: 2024-09-16

Nodo derecho:
* ID: 4, Nombre: Ana López, Correo: ana.lopez@mail.com, Fecha de Registro: 2024-09-18
* ID: 5, Nombre: Luis Fernández, Correo: luis.fernandez@mail.com, Fecha de Registro: 2024-09-19

* __Índice Hash__: acceso directo y rápido a los registros basados en la clave hash, pero requiere manejo de colisiones. 
* __Índice B-tree__: estructura balanceada que permite búsquedas rápidas y eficientes, con nodos que almacenan datos y rutas.
* __Índice B+ tree__: similar al B-tree, pero más eficiente para consultas secuenciales gracias a la conexión directa entre nodos hoja.
