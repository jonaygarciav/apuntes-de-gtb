# Character Set y Collations

* Introducción
* Comando `SHOW CHARACTER SET`
* Comando `SHOW COLLATIONS`

# Introducción

Un `CHARACTER SET` es un conjunto de símbolos y sus respectivas codificaciones, mientras que un `COLLATE` es un conjunto de reglas que define cómo se comparan los caracteres dentro de un `CHARACTER SET`.

Supongamos que tenemos un alfabeto con cuatro letras: `A`, `B`, `a`, `b` y asignamos un número a cada letra:

```
A = 0
B = 1
a = 2
b = 3
```

* La letra `A` es un símbolo.
* El número `0` es la codificación para `A`.
* La combinación de las letras con sus números `A=0`, `B=1`, `a=2`, `b=3` conforma un conjunto de caracteres.

Ahora, queremos comparar dos valores de texto: `A` y `B`. La forma más simple de hacerlo es comparar sus codificaciones:

* Codificación de A: 0
* Codificación de B: 1

Como `0 < 1`, decimos que `A` es menor que `B`. Esta comparación se basa en comparar las codificaciones. A este tipo de collation se le llama __binary collation__.

¿Qué pasa si queremos que las letras en minúsculas y mayúsculas sean equivalentes? En este caso, necesitamos al menos dos reglas en nuestra collation:

1. Tratar las letras minúsculas (`a`, `b`) como equivalentes a sus respectivas mayúsculas (`A`, `B`).
2. Luego, comparar las codificaciones.

Esto se llama una __case-insensitive collation__ y no distingue entre mayúsculas y minúsculas. Es un poco más compleja que la __binary collation__.

En la vida real:
* Los conjuntos de caracteres reales contienen no solo letras como `A` y `B`, sino alfabetos completos (a veces múltiples alfabetos) o sistemas de escritura orientales con miles de caracteres, además de símbolos especiales y signos de puntuación.
* Las collations reales pueden tener muchas reglas.
    * Reglas de mayúsculas y minúsculas: Decidir si `a` y `A` son equivalentes.
    * Reglas de acentos: Decidir si `Ö` en alemán es igual a `OE` o se trata como un carácter diferente.
    * Mapeos de múltiples caracteres: Por ejemplo, en alemán, `Ö = OE` en ciertas collations.

## Comando SHOW CHARACTER SET

El comando `SHOW CHARACTER SET` muestra los conjuntos de caracteres disponibles. la cláusula `LIKE`, si está presente, nos permite filtrar los conjuntos de caracteres por una patrón. La cláusula `WHERE` permite seleccionar conjuntos de caracteres haciendo uso de expresiones.

```
mysql> SHOW CHARACTER SET;
+----------+---------------------------------+---------------------+--------+
| Charset  | Description                     | Default collation   | Maxlen |
+----------+---------------------------------+---------------------+--------+
| armscii8 | ARMSCII-8 Armenian              | armscii8_general_ci |      1 |
| ascii    | US ASCII                        | ascii_general_ci    |      1 |
| big5     | Big5 Traditional Chinese        | big5_chinese_ci     |      2 |
| binary   | Binary pseudo charset           | binary              |      1 |
| cp1250   | Windows Central European        | cp1250_general_ci   |      1 |
| cp1251   | Windows Cyrillic                | cp1251_general_ci   |      1 |
| cp1256   | Windows Arabic                  | cp1256_general_ci   |      1 |
| cp1257   | Windows Baltic                  | cp1257_general_ci   |      1 |
| cp850    | DOS West European               | cp850_general_ci    |      1 |
| cp852    | DOS Central European            | cp852_general_ci    |      1 |
| cp866    | DOS Russian                     | cp866_general_ci    |      1 |
| cp932    | SJIS for Windows Japanese       | cp932_japanese_ci   |      2 |
| dec8     | DEC West European               | dec8_swedish_ci     |      1 |
| eucjpms  | UJIS for Windows Japanese       | eucjpms_japanese_ci |      3 |
| euckr    | EUC-KR Korean                   | euckr_korean_ci     |      2 |
| gb18030  | China National Standard GB18030 | gb18030_chinese_ci  |      4 |
| gb2312   | GB2312 Simplified Chinese       | gb2312_chinese_ci   |      2 |
| gbk      | GBK Simplified Chinese          | gbk_chinese_ci      |      2 |
| geostd8  | GEOSTD8 Georgian                | geostd8_general_ci  |      1 |
| greek    | ISO 8859-7 Greek                | greek_general_ci    |      1 |
| hebrew   | ISO 8859-8 Hebrew               | hebrew_general_ci   |      1 |
| hp8      | HP West European                | hp8_english_ci      |      1 |
| keybcs2  | DOS Kamenicky Czech-Slovak      | keybcs2_general_ci  |      1 |
| koi8r    | KOI8-R Relcom Russian           | koi8r_general_ci    |      1 |
| koi8u    | KOI8-U Ukrainian                | koi8u_general_ci    |      1 |
| latin1   | cp1252 West European            | latin1_swedish_ci   |      1 |
| latin2   | ISO 8859-2 Central European     | latin2_general_ci   |      1 |
| latin5   | ISO 8859-9 Turkish              | latin5_turkish_ci   |      1 |
| latin7   | ISO 8859-13 Baltic              | latin7_general_ci   |      1 |
| macce    | Mac Central European            | macce_general_ci    |      1 |
| macroman | Mac West European               | macroman_general_ci |      1 |
| sjis     | Shift-JIS Japanese              | sjis_japanese_ci    |      2 |
| swe7     | 7bit Swedish                    | swe7_swedish_ci     |      1 |
| tis620   | TIS620 Thai                     | tis620_thai_ci      |      1 |
| ucs2     | UCS-2 Unicode                   | ucs2_general_ci     |      2 |
| ujis     | EUC-JP Japanese                 | ujis_japanese_ci    |      3 |
| utf16    | UTF-16 Unicode                  | utf16_general_ci    |      4 |
| utf16le  | UTF-16LE Unicode                | utf16le_general_ci  |      4 |
| utf32    | UTF-32 Unicode                  | utf32_general_ci    |      4 |
| utf8mb3  | UTF-8 Unicode                   | utf8mb3_general_ci  |      3 |
| utf8mb4  | UTF-8 Unicode                   | utf8mb4_0900_ai_ci  |      4 |
+----------+---------------------------------+---------------------+--------+
41 rows in set (0,00 sec)
```

El conjunto de caracteres `latin1` en MySQL es una implementación del estándar _ISO-8859-1_ (también conocido como _Latin-1_ o _Western European_). Es un conjunto de caracteres de un solo byte que puede representar 256 caracteres, lo que incluye caracteres del alfabeto latino básico y algunos caracteres adicionales comunes en lenguajes europeos occidentales. Cada carácter utiliza exactamente 1 byte. Fue el conjunto de caracteres predeterminado en MySQL hasta la versión 5.5, antes de que _UTF-8_ tomara su lugar como el conjunto de caracteres más recomendado.

```sql
mysql> SHOW CHARACTER SET LIKE 'latin%';
+---------+-----------------------------+-------------------+--------+
| Charset | Description                 | Default collation | Maxlen |
+---------+-----------------------------+-------------------+--------+
| latin1  | cp1252 West European        | latin1_swedish_ci |      1 |
| latin2  | ISO 8859-2 Central European | latin2_general_ci |      1 |
| latin5  | ISO 8859-9 Turkish          | latin5_turkish_ci |      1 |
| latin7  | ISO 8859-13 Baltic          | latin7_general_ci |      1 |
+---------+-----------------------------+-------------------+--------+
4 rows in set (0,00 sec)
```

La salida de `SHOW CHARACTER SET` tiene las siguientes columnas:
* __Charset__: el nombre del conjunto de caracteres.
* __Description__: una descripción del conjunto de caracteres.
* __Default collation__: la collation predeterminada para el conjunto de caracteres.
* __Maxlen__: el número máximo de bytes requeridos para almacenar un carácter.

El conjunto de caracteres `utf8mb4` es la implementación de MySQL para el estándar _UTF-8_ completo (4 bytes por carácter), que puede representar todo el rango de caracteres de Unicode. Esto incluye caracteres comunes, símbolos, emojis y caracteres de sistemas de escritura asiáticos, entre otros:
* __1 byte__: caracteres ASCII comunes.
* __2 bytes__: caracteres europeos básicos y algunos símbolos.
* __3 bytes__: caracteres adicionales de Unicode, como algunos alfabetos extendidos.
* __4 bytes__: emojis y caracteres de sistemas de escritura como el chino, japonés, coreano (CJK) y más.

A partir de _MySQL 8.0_, `utf8mb4` es el conjunto de caracteres predeterminado para nuevas bases de datos, tablas y columnas.

```sql
mysql> SHOW CHARACTER SET LIKE 'utf8%';
+---------+---------------+--------------------+--------+
| Charset | Description   | Default collation  | Maxlen |
+---------+---------------+--------------------+--------+
| utf8mb3 | UTF-8 Unicode | utf8mb3_general_ci |      3 |
| utf8mb4 | UTF-8 Unicode | utf8mb4_0900_ai_ci |      4 |
+---------+---------------+--------------------+--------+
2 rows in set (0,00 sec)
```

## Comando SHOW COLLATIONS

La instrucción `SHOW COLLATION` lista los collations soportados por MySQL. La cláusula `LIKE`, si está presente, nos permite filtrar los conjuntos de caracteres por una patrón. La cláusula `WHERE` permite seleccionar conjuntos de caracteres haciendo uso de expresiones.

```sql
SHOW COLLATION
    [LIKE 'pattern' | WHERE expr]
```

A continuación, se muestran los collation para el conjunto de caracteres `latin1`:

```sql
mysql> SHOW COLLATION WHERE Charset = 'latin1';
+-------------------+---------+----+---------+----------+---------+---------------+
| Collation         | Charset | Id | Default | Compiled | Sortlen | Pad_attribute |
+-------------------+---------+----+---------+----------+---------+---------------+
| latin1_bin        | latin1  | 47 |         | Yes      |       1 | PAD SPACE     |
| latin1_danish_ci  | latin1  | 15 |         | Yes      |       1 | PAD SPACE     |
| latin1_general_ci | latin1  | 48 |         | Yes      |       1 | PAD SPACE     |
| latin1_general_cs | latin1  | 49 |         | Yes      |       1 | PAD SPACE     |
| latin1_german1_ci | latin1  |  5 |         | Yes      |       1 | PAD SPACE     |
| latin1_german2_ci | latin1  | 31 |         | Yes      |       2 | PAD SPACE     |
| latin1_spanish_ci | latin1  | 94 |         | Yes      |       1 | PAD SPACE     |
| latin1_swedish_ci | latin1  |  8 | Yes     | Yes      |       1 | PAD SPACE     |
+-------------------+---------+----+---------+----------+---------+---------------+
8 rows in set (0,00 sec)
```

* __latin1_general_ci__:
    * Insensible a mayúsculas y minúsculas (ci = case-insensitive).
    * Ignora las diferencias entre mayúsculas y minúsculas durante las comparaciones.
* __latin1_general_cs__:
    * Sensible a mayúsculas y minúsculas (cs = case-sensitive).
    * Distingue entre mayúsculas y minúsculas al comparar caracteres.
* __latin1_swedish_ci__:
    * Insensible a mayúsculas y minúsculas.
    * Es la collation predeterminada para latin1.
    * Basada en las reglas de ordenamiento del idioma sueco.
* __latin1_spanish_ci__:
    * Insensible a mayúsculas y minúsculas.
    * Sigue las reglas de ordenamiento del idioma español, donde los caracteres con acentos (á, é, etc.) son tratados como sus versiones sin acento (a, e, etc.) en las comparaciones.
    * El carácter ñ se coloca después de la n en el orden alfabético.

```sql
SELECT 'á' = 'a' COLLATE latin1_spanish_ci; -- Devuelve 1 (igual)
SELECT 'ñ' = 'n' COLLATE latin1_spanish_ci; -- Devuelve 0 (diferente)
SELECT 'Ñ' > 'n' COLLATE latin1_spanish_ci; -- Devuelve 1 (true, ñ viene después de n)
```

Columnas de la salida del comando  `SHOW COLLATION`:
* __Collation__: el nombre de la collation.
* __Charset__: el nombre del conjunto de caracteres asociado con la collation.
* __Id__: el ID único de la collation.
* __Default__: indica si la collation es la predeterminada para su conjunto de caracteres.
* __Compiled__: indica si la collation está compilada en el servidor.
* __Sortlen__: relacionado con la cantidad de memoria requerida para ordenar cadenas expresadas en el conjunto de caracteres.
* __Pad_attribute__: atributo de relleno de la collation que puede tener los valores `NO PAD` o `PAD SPACE`. Este atributo afecta si los espacios finales son significativos en las comparaciones de cadenas.

Ver los collation predeterminados para cada conjunto de caracteres. Dado que _Default_ es una palabra reservada, debe estar entre comillas invertidas:

```sql
mysql> SHOW COLLATION WHERE `Default` = 'Yes';
+---------------------+----------+-----+---------+----------+---------+---------------+
| Collation           | Charset  | Id  | Default | Compiled | Sortlen | Pad_attribute |
+---------------------+----------+-----+---------+----------+---------+---------------+
| armscii8_general_ci | armscii8 |  32 | Yes     | Yes      |       1 | PAD SPACE     |
| ascii_general_ci    | ascii    |  11 | Yes     | Yes      |       1 | PAD SPACE     |
| big5_chinese_ci     | big5     |   1 | Yes     | Yes      |       1 | PAD SPACE     |
| binary              | binary   |  63 | Yes     | Yes      |       1 | NO PAD        |
| cp1250_general_ci   | cp1250   |  26 | Yes     | Yes      |       1 | PAD SPACE     |
| cp1251_general_ci   | cp1251   |  51 | Yes     | Yes      |       1 | PAD SPACE     |
| cp1256_general_ci   | cp1256   |  57 | Yes     | Yes      |       1 | PAD SPACE     |
| cp1257_general_ci   | cp1257   |  59 | Yes     | Yes      |       1 | PAD SPACE     |
| cp850_general_ci    | cp850    |   4 | Yes     | Yes      |       1 | PAD SPACE     |
| cp852_general_ci    | cp852    |  40 | Yes     | Yes      |       1 | PAD SPACE     |
| cp866_general_ci    | cp866    |  36 | Yes     | Yes      |       1 | PAD SPACE     |
| cp932_japanese_ci   | cp932    |  95 | Yes     | Yes      |       1 | PAD SPACE     |
| dec8_swedish_ci     | dec8     |   3 | Yes     | Yes      |       1 | PAD SPACE     |
| eucjpms_japanese_ci | eucjpms  |  97 | Yes     | Yes      |       1 | PAD SPACE     |
| euckr_korean_ci     | euckr    |  19 | Yes     | Yes      |       1 | PAD SPACE     |
| gb18030_chinese_ci  | gb18030  | 248 | Yes     | Yes      |       2 | PAD SPACE     |
| gb2312_chinese_ci   | gb2312   |  24 | Yes     | Yes      |       1 | PAD SPACE     |
| gbk_chinese_ci      | gbk      |  28 | Yes     | Yes      |       1 | PAD SPACE     |
| geostd8_general_ci  | geostd8  |  92 | Yes     | Yes      |       1 | PAD SPACE     |
| greek_general_ci    | greek    |  25 | Yes     | Yes      |       1 | PAD SPACE     |
| hebrew_general_ci   | hebrew   |  16 | Yes     | Yes      |       1 | PAD SPACE     |
| hp8_english_ci      | hp8      |   6 | Yes     | Yes      |       1 | PAD SPACE     |
| keybcs2_general_ci  | keybcs2  |  37 | Yes     | Yes      |       1 | PAD SPACE     |
| koi8r_general_ci    | koi8r    |   7 | Yes     | Yes      |       1 | PAD SPACE     |
| koi8u_general_ci    | koi8u    |  22 | Yes     | Yes      |       1 | PAD SPACE     |
| latin1_swedish_ci   | latin1   |   8 | Yes     | Yes      |       1 | PAD SPACE     |
| latin2_general_ci   | latin2   |   9 | Yes     | Yes      |       1 | PAD SPACE     |
| latin5_turkish_ci   | latin5   |  30 | Yes     | Yes      |       1 | PAD SPACE     |
| latin7_general_ci   | latin7   |  41 | Yes     | Yes      |       1 | PAD SPACE     |
| macce_general_ci    | macce    |  38 | Yes     | Yes      |       1 | PAD SPACE     |
| macroman_general_ci | macroman |  39 | Yes     | Yes      |       1 | PAD SPACE     |
| sjis_japanese_ci    | sjis     |  13 | Yes     | Yes      |       1 | PAD SPACE     |
| swe7_swedish_ci     | swe7     |  10 | Yes     | Yes      |       1 | PAD SPACE     |
| tis620_thai_ci      | tis620   |  18 | Yes     | Yes      |       4 | PAD SPACE     |
| ucs2_general_ci     | ucs2     |  35 | Yes     | Yes      |       1 | PAD SPACE     |
| ujis_japanese_ci    | ujis     |  12 | Yes     | Yes      |       1 | PAD SPACE     |
| utf16le_general_ci  | utf16le  |  56 | Yes     | Yes      |       1 | PAD SPACE     |
| utf16_general_ci    | utf16    |  54 | Yes     | Yes      |       1 | PAD SPACE     |
| utf32_general_ci    | utf32    |  60 | Yes     | Yes      |       1 | PAD SPACE     |
| utf8mb3_general_ci  | utf8mb3  |  33 | Yes     | Yes      |       1 | PAD SPACE     |
| utf8mb4_0900_ai_ci  | utf8mb4  | 255 | Yes     | Yes      |       0 | NO PAD        |
+---------------------+----------+-----+---------+----------+---------+---------------+
41 rows in set (0,01 sec)
```

Ver los collation asociados al _CHARACTER SET_ `utf8mb4`::


```sql
mysql> SHOW COLLATION WHERE CHARSET LIKE 'utf8mb4';
+----------------------------+---------+-----+---------+----------+---------+---------------+
| Collation                  | Charset | Id  | Default | Compiled | Sortlen | Pad_attribute |
+----------------------------+---------+-----+---------+----------+---------+---------------+
| utf8mb4_0900_ai_ci         | utf8mb4 | 255 | Yes     | Yes      |       0 | NO PAD        |
| utf8mb4_0900_as_ci         | utf8mb4 | 305 |         | Yes      |       0 | NO PAD        |
| utf8mb4_0900_as_cs         | utf8mb4 | 278 |         | Yes      |       0 | NO PAD        |
| utf8mb4_0900_bin           | utf8mb4 | 309 |         | Yes      |       1 | NO PAD        |
| utf8mb4_bg_0900_ai_ci      | utf8mb4 | 318 |         | Yes      |       0 | NO PAD        |
| utf8mb4_bg_0900_as_cs      | utf8mb4 | 319 |         | Yes      |       0 | NO PAD        |
| utf8mb4_bin                | utf8mb4 |  46 |         | Yes      |       1 | PAD SPACE     |
| utf8mb4_bs_0900_ai_ci      | utf8mb4 | 316 |         | Yes      |       0 | NO PAD        |
| utf8mb4_bs_0900_as_cs      | utf8mb4 | 317 |         | Yes      |       0 | NO PAD        |
| utf8mb4_croatian_ci        | utf8mb4 | 245 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_cs_0900_ai_ci      | utf8mb4 | 266 |         | Yes      |       0 | NO PAD        |
| utf8mb4_cs_0900_as_cs      | utf8mb4 | 289 |         | Yes      |       0 | NO PAD        |
| utf8mb4_czech_ci           | utf8mb4 | 234 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_danish_ci          | utf8mb4 | 235 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_da_0900_ai_ci      | utf8mb4 | 267 |         | Yes      |       0 | NO PAD        |
| utf8mb4_da_0900_as_cs      | utf8mb4 | 290 |         | Yes      |       0 | NO PAD        |
| utf8mb4_de_pb_0900_ai_ci   | utf8mb4 | 256 |         | Yes      |       0 | NO PAD        |
| utf8mb4_de_pb_0900_as_cs   | utf8mb4 | 279 |         | Yes      |       0 | NO PAD        |
| utf8mb4_eo_0900_ai_ci      | utf8mb4 | 273 |         | Yes      |       0 | NO PAD        |
| utf8mb4_eo_0900_as_cs      | utf8mb4 | 296 |         | Yes      |       0 | NO PAD        |
| utf8mb4_esperanto_ci       | utf8mb4 | 241 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_estonian_ci        | utf8mb4 | 230 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_es_0900_ai_ci      | utf8mb4 | 263 |         | Yes      |       0 | NO PAD        |
| utf8mb4_es_0900_as_cs      | utf8mb4 | 286 |         | Yes      |       0 | NO PAD        |
| utf8mb4_es_trad_0900_ai_ci | utf8mb4 | 270 |         | Yes      |       0 | NO PAD        |
| utf8mb4_es_trad_0900_as_cs | utf8mb4 | 293 |         | Yes      |       0 | NO PAD        |
| utf8mb4_et_0900_ai_ci      | utf8mb4 | 262 |         | Yes      |       0 | NO PAD        |
| utf8mb4_et_0900_as_cs      | utf8mb4 | 285 |         | Yes      |       0 | NO PAD        |
| utf8mb4_general_ci         | utf8mb4 |  45 |         | Yes      |       1 | PAD SPACE     |
| utf8mb4_german2_ci         | utf8mb4 | 244 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_gl_0900_ai_ci      | utf8mb4 | 320 |         | Yes      |       0 | NO PAD        |
| utf8mb4_gl_0900_as_cs      | utf8mb4 | 321 |         | Yes      |       0 | NO PAD        |
| utf8mb4_hr_0900_ai_ci      | utf8mb4 | 275 |         | Yes      |       0 | NO PAD        |
| utf8mb4_hr_0900_as_cs      | utf8mb4 | 298 |         | Yes      |       0 | NO PAD        |
| utf8mb4_hungarian_ci       | utf8mb4 | 242 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_hu_0900_ai_ci      | utf8mb4 | 274 |         | Yes      |       0 | NO PAD        |
| utf8mb4_hu_0900_as_cs      | utf8mb4 | 297 |         | Yes      |       0 | NO PAD        |
| utf8mb4_icelandic_ci       | utf8mb4 | 225 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_is_0900_ai_ci      | utf8mb4 | 257 |         | Yes      |       0 | NO PAD        |
| utf8mb4_is_0900_as_cs      | utf8mb4 | 280 |         | Yes      |       0 | NO PAD        |
| utf8mb4_ja_0900_as_cs      | utf8mb4 | 303 |         | Yes      |       0 | NO PAD        |
| utf8mb4_ja_0900_as_cs_ks   | utf8mb4 | 304 |         | Yes      |      24 | NO PAD        |
| utf8mb4_latvian_ci         | utf8mb4 | 226 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_la_0900_ai_ci      | utf8mb4 | 271 |         | Yes      |       0 | NO PAD        |
| utf8mb4_la_0900_as_cs      | utf8mb4 | 294 |         | Yes      |       0 | NO PAD        |
| utf8mb4_lithuanian_ci      | utf8mb4 | 236 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_lt_0900_ai_ci      | utf8mb4 | 268 |         | Yes      |       0 | NO PAD        |
| utf8mb4_lt_0900_as_cs      | utf8mb4 | 291 |         | Yes      |       0 | NO PAD        |
| utf8mb4_lv_0900_ai_ci      | utf8mb4 | 258 |         | Yes      |       0 | NO PAD        |
| utf8mb4_lv_0900_as_cs      | utf8mb4 | 281 |         | Yes      |       0 | NO PAD        |
| utf8mb4_mn_cyrl_0900_ai_ci | utf8mb4 | 322 |         | Yes      |       0 | NO PAD        |
| utf8mb4_mn_cyrl_0900_as_cs | utf8mb4 | 323 |         | Yes      |       0 | NO PAD        |
| utf8mb4_nb_0900_ai_ci      | utf8mb4 | 310 |         | Yes      |       0 | NO PAD        |
| utf8mb4_nb_0900_as_cs      | utf8mb4 | 311 |         | Yes      |       0 | NO PAD        |
| utf8mb4_nn_0900_ai_ci      | utf8mb4 | 312 |         | Yes      |       0 | NO PAD        |
| utf8mb4_nn_0900_as_cs      | utf8mb4 | 313 |         | Yes      |       0 | NO PAD        |
| utf8mb4_persian_ci         | utf8mb4 | 240 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_pl_0900_ai_ci      | utf8mb4 | 261 |         | Yes      |       0 | NO PAD        |
| utf8mb4_pl_0900_as_cs      | utf8mb4 | 284 |         | Yes      |       0 | NO PAD        |
| utf8mb4_polish_ci          | utf8mb4 | 229 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_romanian_ci        | utf8mb4 | 227 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_roman_ci           | utf8mb4 | 239 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_ro_0900_ai_ci      | utf8mb4 | 259 |         | Yes      |       0 | NO PAD        |
| utf8mb4_ro_0900_as_cs      | utf8mb4 | 282 |         | Yes      |       0 | NO PAD        |
| utf8mb4_ru_0900_ai_ci      | utf8mb4 | 306 |         | Yes      |       0 | NO PAD        |
| utf8mb4_ru_0900_as_cs      | utf8mb4 | 307 |         | Yes      |       0 | NO PAD        |
| utf8mb4_sinhala_ci         | utf8mb4 | 243 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_sk_0900_ai_ci      | utf8mb4 | 269 |         | Yes      |       0 | NO PAD        |
| utf8mb4_sk_0900_as_cs      | utf8mb4 | 292 |         | Yes      |       0 | NO PAD        |
| utf8mb4_slovak_ci          | utf8mb4 | 237 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_slovenian_ci       | utf8mb4 | 228 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_sl_0900_ai_ci      | utf8mb4 | 260 |         | Yes      |       0 | NO PAD        |
| utf8mb4_sl_0900_as_cs      | utf8mb4 | 283 |         | Yes      |       0 | NO PAD        |
| utf8mb4_spanish2_ci        | utf8mb4 | 238 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_spanish_ci         | utf8mb4 | 231 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_sr_latn_0900_ai_ci | utf8mb4 | 314 |         | Yes      |       0 | NO PAD        |
| utf8mb4_sr_latn_0900_as_cs | utf8mb4 | 315 |         | Yes      |       0 | NO PAD        |
| utf8mb4_sv_0900_ai_ci      | utf8mb4 | 264 |         | Yes      |       0 | NO PAD        |
| utf8mb4_sv_0900_as_cs      | utf8mb4 | 287 |         | Yes      |       0 | NO PAD        |
| utf8mb4_swedish_ci         | utf8mb4 | 232 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_tr_0900_ai_ci      | utf8mb4 | 265 |         | Yes      |       0 | NO PAD        |
| utf8mb4_tr_0900_as_cs      | utf8mb4 | 288 |         | Yes      |       0 | NO PAD        |
| utf8mb4_turkish_ci         | utf8mb4 | 233 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_unicode_520_ci     | utf8mb4 | 246 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_unicode_ci         | utf8mb4 | 224 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_vietnamese_ci      | utf8mb4 | 247 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_vi_0900_ai_ci      | utf8mb4 | 277 |         | Yes      |       0 | NO PAD        |
| utf8mb4_vi_0900_as_cs      | utf8mb4 | 300 |         | Yes      |       0 | NO PAD        |
| utf8mb4_zh_0900_as_cs      | utf8mb4 | 308 |         | Yes      |       0 | NO PAD        |
+----------------------------+---------+-----+---------+----------+---------+---------------+
89 rows in set (0,00 sec)
```

* __utf8mb4_0900_ai_ci__:
    * Conjunto de caracteres asociado: `utf8mb4`
    * ID: 255
    * Descripción: 
        * Insensible a mayúsculas y acentos (ai = accent-insensitive, ci = case-insensitive).
        * Es la collation predeterminada para `utf8mb4` en MySQL 8.0.
        * No distingue entre caracteres con y sin acentos (`á` = `a`) ni entre mayúsculas y minúsculas (`A` = `a`).
        * Atributo de relleno (Pad_attribute): NO PAD. Los espacios finales no son significativos en las comparaciones.
        * Ideal para texto multilingüe en aplicaciones modernas.

* __utf8mb4_0900_as_ci__:
    * _Conjunto de caracteres asociado_: `utf8mb4`
    * _ID_: 305
    * _Descripción_:
        * Sensible a acentos (as = accent-sensitive), pero insensible a mayúsculas (ci = case-insensitive).
        * Distingue entre caracteres con y sin acentos (á ≠ a), pero no entre mayúsculas y minúsculas (A = a).
        * Atributo de relleno (Pad_attribute): NO PAD. Los espacios finales no son significativos en las comparaciones.
        * Útil cuando los acentos son importantes, pero las mayúsculas no lo son.

* __utf8mb4_0900_as_cs__:
    * _Conjunto de caracteres asociado_: `utf8mb4`
    * _ID_: 278
    * _Descripción_:
        * Sensible a acentos (as = accent-sensitive) y mayúsculas (cs = case-sensitive).
        * Distingue entre caracteres con y sin acentos (á ≠ a) y entre mayúsculas y minúsculas (A ≠ a).
        * Atributo de relleno (Pad_attribute): NO PAD. Los espacios finales no son significativos en las comparaciones.
        * Adecuado para aplicaciones donde tanto los acentos como las mayúsculas son importantes.

* __utf8mb4_spanish2_ci__:
    * _Conjunto de caracteres asociado_: `utf8mb4`
    * _ID_: 238
    * _Descripción_:
        * Insensible a mayúsculas y acentos (ci = case-insensitive).
        * Sigue las reglas de ordenamiento del idioma español moderno.
        * Los caracteres con y sin acentos son equivalentes (á = a).
        * Coloca la ñ después de la n en el orden alfabético.
        * Atributo de relleno (Pad_attribute): PAD SPACE.
        * Los espacios finales son significativos en las comparaciones.
        * Útil para textos en español donde se necesita un ordenamiento específico del idioma.

* __utf8mb4_spanish_ci__:
    * _Conjunto de caracteres asociado_: `utf8mb4`
    * _ID_: 231
    * _Descripción_:
        * Insensible a mayúsculas y acentos (ci = case-insensitive).
        * Sigue reglas de ordenamiento del español tradicional.
        * Considera caracteres con y sin acentos como equivalentes (á = a), pero trata a la ñ como equivalente a la n en el orden alfabético.
        * Atributo de relleno (Pad_attribute): PAD SPACE.
        * Los espacios finales son significativos en las comparaciones.
        * Útil para textos en español con un ordenamiento menos estricto en comparación con `utf8mb4_spanish2_ci`.

Ejemplo de ordenación en cada collation:

```sql
-- utf8mb4_0900_ai_ci: ai = accent-insensitive, ci = case-insensitive
SELECT 'á' = 'a' COLLATE utf8mb4_0900_ai_ci; -- Devuelve 1 (true)
SELECT 'A' = 'a' COLLATE utf8mb4_0900_ai_ci; -- Devuelve 1 (true)

-- utf8mb4_0900_as_ci: as = accent-sensitive, ci = case-insensitive
SELECT 'á' = 'a' COLLATE utf8mb4_0900_as_ci; -- Devuelve 0 (false)
SELECT 'A' = 'a' COLLATE utf8mb4_0900_as_ci; -- Devuelve 1 (true)

-- utf8mb4_0900_as_cs: as = accent-sensitive, cs = case-sensitive
SELECT 'á' = 'a' COLLATE utf8mb4_0900_as_cs; -- Devuelve 0 (false)
SELECT 'A' = 'a' COLLATE utf8mb4_0900_as_cs; -- Devuelve 0 (false)

-- utf8mb4_spanish2_ci: ci = case-insensitive
SELECT 'á' = 'a' COLLATE utf8mb4_spanish2_ci; -- Devuelve 1 (true)
SELECT 'ñ' > 'n' COLLATE utf8mb4_spanish2_ci; -- Devuelve 1 (true)

-- utf8mb4_spanish_ci: ci = case-insensitive
SELECT 'á' = 'a' COLLATE utf8mb4_spanish_ci; -- Devuelve 1 (true)
SELECT 'ñ' = 'n' COLLATE utf8mb4_spanish_ci; -- Devuelve 1 (true)
```

| Collation           | Sensible a Acentos | Sensible a Mayúsculas | Espacios Significativos | Uso Principal           |
|---------------------|--------------------|-----------------------|-------------------------|-------------------------|
| utf8mb4_0900_ai_ci  | No                | No                     | No                      | Multilingüe y general   |
| utf8mb4_0900_as_ci  | Sí                | No                     | No                      | Sensibilidad a acentos  |
| utf8mb4_0900_as_cs  | Sí                | Sí                     | No                      | Comparaciones estrictas |
| utf8mb4_spanish2_ci | No                | No                     | Sí                      | Español moderno         |
| utf8mb4_spanish_ci  | No                | No                     | Sí                      | Español tradicional     |

Para consultar qué conjunto de caracteres y reglas de ordenación está utilizando una determinada base de datos podemos consultar el valor de las variables character_set_database y collation_database.

```sql
mysql> CREATE DATABASE prueba1;
Query OK, 1 row affected (0,03 sec)

mysql> USE prueba1;
Database changed
```

```sql
-- Opción 1
mysql> SELECT @@character_set_database, @@collation_database;
+--------------------------+----------------------+
| @@character_set_database | @@collation_database |
+--------------------------+----------------------+
| utf8mb4                  | utf8mb4_0900_ai_ci   |
+--------------------------+----------------------+
1 row in set (0,00 sec)

-- Opción 2
mysql> SELECT DEFAULT_CHARACTER_SET_NAME, DEFAULT_COLLATION_NAME
    -> FROM INFORMATION_SCHEMA.SCHEMATA WHERE SCHEMA_NAME = 'prueba1';
+----------------------------+------------------------+
| DEFAULT_CHARACTER_SET_NAME | DEFAULT_COLLATION_NAME |
+----------------------------+------------------------+
| utf8mb4                    | utf8mb4_0900_ai_ci     |
+----------------------------+------------------------+
1 row in set (0,00 sec)
```
