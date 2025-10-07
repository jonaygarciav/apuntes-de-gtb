# Sistemas de Codificación

* Introducción
* Sistemas de codificación numéricos
  * Sistema binario
  * Sistema hexadecimal
* Sistemas de codificación de caracteres
  * Codigo ASCII
  * ASCII Extendido
  * Unicode
  * Codificación en Sitemas Windows

## Introducción

Los _sistemas de codificación_ son métodos utilizados para representar caracteres, símbolos, y datos en un formato que pueda ser entendido y procesado por ordenadores y otros dispositivos digitales. Estos sistemas asignan un valor numérico a cada carácter, permitiendo que el texto, los números y otros símbolos se almacenen y transmitan electrónicamente. Los _sistemas de codificación_ son fundamentales en la informática, ya que permiten la comunicación y el procesamiento de datos en múltiples plataformas y lenguajes.

> __Nota__: sin una codificación adecuada, los textos y símbolos podrían interpretarse incorrectamente, lo que llevaría a errores en la visualización.

## Sistemas de codificación numéricos

### Sistema binario

El _sistema binario_ es un sistema de numeración en el que solamente se utilizan dos dígitos: el 0 y el 1. Este sistema es la base de la informática, ya que los ordenadores trabajan internamente con este formato debido a su facilidad para representar estados de encendido y apagado (1 y 0).

A continuación, se muestra una tabla con la conversión de los números en decimal del 0 al 15 a binario en grupos de 8 bits:

| **Decimal** | **Binario**   | **Decimal** | **Binario**   |
|-------------|---------------|-------------|---------------|
| 0           | 0000 0000     | 8           | 0000 1000     |
| 1           | 0000 0001     | 9           | 0000 1001     |
| 2           | 0000 0010     | 10          | 0000 1010     |
| 3           | 0000 0011     | 11          | 0000 1011     |
| 4           | 0000 0100     | 12          | 0000 1100     |
| 5           | 0000 0101     | 13          | 0000 1101     |
| 6           | 0000 0110     | 14          | 0000 1110     |
| 7           | 0000 0111     | 15          | 0000 1111     |

> __Nota__: un grupo de 8 bits se conoce como _byte_ u _octeto_.

### Sistema hexadecimal

El _sistema hexadecimal_ es un sistema de numeración que utiliza 16 dígitos: 0-9 y las letras A-F (donde A representa 10, B representa 11, y así sucesivamente hasta F, que representa 15). Cada dígito es un grupo de 4 bits:

| **Binario**         | **Hexadecimal** | **Binario**         | **Hexadecimal** |
|---------------------|-----------------|---------------------|-----------------|
| 0000                | 0               | 1000                | 8               |
| 0001                | 1               | 1001                | 9               |
| 0010                | 2               | 1010                | A               |
| 0011                | 3               | 1011                | B               |
| 0100                | 4               | 1100                | C               |
| 0101                | 5               | 1101                | D               |
| 0110                | 6               | 1110                | E               |
| 0111                | 7               | 1111                | F               |

Sus principales usos son:

* __Representación de Colores en Web (HTML/CSS)__: los colores se representan comúnmente en formato hexadecimal en diseño web. Por ejemplo, el color blanco se representa como #FFFFFF y el negro como #000000.
* __Direcciones MAC__: las direcciones MAC de los dispositivos de red (como tarjetas de red y routers) se escriben en formato hexadecimal, por ejemplo, 00:1A:2B:3C:4D:5E.
* __Codificación de Caracteres Unicode__: los caracteres Unicode pueden ser representados en hexadecimal, por ejemplo, U+0041 representa la letra "A".
* __Criptografía__: los valores de hashes y claves de cifrado, como MD5 o SHA-256, suelen presentarse en formato hexadecimal.
* __Direcciones IPv6__: las direcciones IPv6 utilizan el sistema hexadecimal para comprimir largas secuencias de bits en un formato más manejable, por ejemplo, 2001:0db8:85a3:0000:0000:8a2e:0370:7334.

> __Nota__: un grupo de 4 bits se conoce como _nibble_.

## Sistemas de codificación de caracteres

### Codigo ASCII

_ASCII_ (_American Standard Code for Information Interchange_) es un código de caracteres que se utiliza para representar texto y otros símbolos en un ordenador, dispositivos de comunicación y otros equipos que utilizan texto. ASCII se basa en el alfabeto inglés y define 128 caracteres, que incluyen letras, números, signos de puntuación, caracteres de control y otros símbolos. Cada carácter se representa mediante un valor numérico en el rango de 0 a 127 y utiliza un conjunto de 7 bits para representar cada caracter ASCII.

Los primeros 32 caracteres (0-31) y el carácter 127 son caracteres de control, utilizados para controlar dispositivos periféricos como impresoras o para enviar comandos especiales, como nueva línea o tabulación.

A continuación, se muestra la tabla de caracteres ASCII:

| **Código ASCII** | **Binario (7 bits)** | **Hexadecimal** | **Carácter**                | **Código ASCII** | **Binario (7 bits)** | **Hexadecimal** | **Carácter**                |
|------------------|----------------------|-----------------|-----------------------------|------------------|----------------------|-----------------|-----------------------------|
| 0                | 0000000              | 00              | NUL (Carácter Nulo)         | 64               | 1000000              | 40              | @                           |
| 1                | 0000001              | 01              | SOH (Inicio de Encabezado)  | 65               | 1000001              | 41              | A                           |
| 2                | 0000010              | 02              | STX (Inicio de Texto)       | 66               | 1000010              | 42              | B                           |
| 3                | 0000011              | 03              | ETX (Fin de Texto)          | 67               | 1000011              | 43              | C                           |
| 4                | 0000100              | 04              | EOT (Fin de Transmisión)    | 68               | 1000100              | 44              | D                           |
| 5                | 0000101              | 05              | ENQ (Consulta)              | 69               | 1000101              | 45              | E                           |
| 6                | 0000110              | 06              | ACK (Reconocimiento)        | 70               | 1000110              | 46              | F                           |
| 7                | 0000111              | 07              | BEL (Campana)               | 71               | 1000111              | 47              | G                           |
| 8                | 0001000              | 08              | BS (Retroceso)              | 72               | 1001000              | 48              | H                           |
| 9                | 0001001              | 09              | HT (Tabulación Horizontal)  | 73               | 1001001              | 49              | I                           |
| 10               | 0001010              | 0A              | LF (Salto de Línea)         | 74               | 1001010              | 4A              | J                           |
| 11               | 0001011              | 0B              | VT (Tabulación Vertical)    | 75               | 1001011              | 4B              | K                           |
| 12               | 0001100              | 0C              | FF (Avance de Página)       | 76               | 1001100              | 4C              | L                           |
| 13               | 0001101              | 0D              | CR (Retorno de Carro)       | 77               | 1001101              | 4D              | M                           |
| 14               | 0001110              | 0E              | SO (Desplazamiento Salida)  | 78               | 1001110              | 4E              | N                           |
| 15               | 0001111              | 0F              | SI (Desplazamiento Entrada) | 79               | 1001111              | 4F              | O                           |
| 16               | 0010000              | 10              | DLE (Escape de Enlace de Datos) | 80           | 1010000              | 50              | P                           |
| 17               | 0010001              | 11              | DC1 (Control de Dispositivo 1) | 81            | 1010001              | 51              | Q                           |
| 18               | 0010010              | 12              | DC2 (Control de Dispositivo 2) | 82            | 1010010              | 52              | R                           |
| 19               | 0010011              | 13              | DC3 (Control de Dispositivo 3) | 83            | 1010011              | 53              | S                           |
| 20               | 0010100              | 14              | DC4 (Control de Dispositivo 4) | 84            | 1010100              | 54              | T                           |
| 21               | 0010101              | 15              | NAK (Reconocimiento Negativo) | 85            | 1010101              | 55              | U                           |
| 22               | 0010110              | 16              | SYN (Sincronización Inactiva) | 86            | 1010110              | 56              | V                           |
| 23               | 0010111              | 17              | ETB (Fin de Bloque)         | 87               | 1010111              | 57              | W                           |
| 24               | 0011000              | 18              | CAN (Cancelar)              | 88               | 1011000              | 58              | X                           |
| 25               | 0011001              | 19              | EM (Fin de Medio)           | 89               | 1011001              | 59              | Y                           |
| 26               | 0011010              | 1A              | SUB (Sustituto)             | 90               | 1011010              | 5A              | Z                           |
| 27               | 0011011              | 1B              | ESC (Escape)                | 91               | 1011011              | 5B              | [                           |
| 28               | 0011100              | 1C              | FS (Separador de Archivo)   | 92               | 1011100              | 5C              | \                           |
| 29               | 0011101              | 1D              | GS (Separador de Grupo)     | 93               | 1011101              | 5D              | ]                           |
| 30               | 0011110              | 1E              | RS (Separador de Registro)  | 94               | 1011110              | 5E              | ^                           |
| 31               | 0011111              | 1F              | US (Separador de Unidad)    | 95               | 1011111              | 5F              | _                           |
| 32               | 0100000              | 20              | Espacio                     | 96               | 1100000              | 60              | `                           |
| 33               | 0100001              | 21              | !                           | 97               | 1100001              | 61              | a                           |
| 34               | 0100010              | 22              | "                           | 98               | 1100010              | 62              | b                           |
| 35               | 0100011              | 23              | #                           | 99               | 1100011              | 63              | c                           |
| 36               | 0100100              | 24              | $                           | 100              | 1100100              | 64              | d                           |
| 37               | 0100101              | 25              | %                           | 101              | 1100101              | 65              | e                           |
| 38               | 0100110              | 26              | &                           | 102              | 1100110              | 66              | f                           |
| 39               | 0100111              | 27              | '                           | 103              | 1100111              | 67              | g                           |
| 40               | 0101000              | 28              | (                           | 104              | 1101000              | 68              | h                           |
| 41               | 0101001              | 29              | )                           | 105              | 1101001              | 69              | i                           |
| 42               | 0101010              | 2A              | *                           | 106              | 1101010              | 6A              | j                           |
| 43               | 0101011              | 2B              | +                           | 107              | 1101011              | 6B              | k                           |
| 44               | 0101100              | 2C              | ,                           | 108              | 1101100              | 6C              | l                           |
| 45               | 0101101              | 2D              | -                           | 109              | 1101101              | 6D              | m                           |
| 46               | 0101110              | 2E              | .                           | 110              | 1101110              | 6E              | n                           |
| 47               | 0101111              | 2F              | /                           | 111              | 1101111              | 6F              | o                           |
| 48               | 0110000              | 30              | 0                           | 112              | 1110000              | 70              | p                           |
| 49               | 0110001              | 31              | 1                           | 113              | 1110001              | 71              | q                           |
| 50               | 0110010              | 32              | 2                           | 114              | 1110010              | 72              | r                           |
| 51               | 0110011              | 33              | 3                           | 115              | 1110011              | 73              | s                           |
| 52               | 0110100              | 34              | 4                           | 116              | 1110100              | 74              | t                           |
| 53               | 0110101              | 35              | 5                           | 117              | 1110101              | 75              | u                           |
| 54               | 0110110              | 36              | 6                           | 118              | 1110110              | 76              | v                           |
| 55               | 0110111              | 37              | 7                           | 119              | 1110111              | 77              | w                           |
| 56               | 0111000              | 38              | 8                           | 120              | 1111000              | 78              | x                           |
| 57               | 0111001              | 39              | 9                           | 121              | 1111001              | 79              | y                           |
| 58               | 0111010              | 3A              | :                           | 122              | 1111010              | 7A              | z                           |
| 59               | 0111011              | 3B              | ;                           | 123              | 1111011              | 7B              | {                           |
| 60               | 0111100              | 3C              | <                           | 124              | 1111100              | 7C              | |                           |
| 61               | 0111101              | 3D              | =                           | 125              | 1111101              | 7D              | }                           |
| 62               | 0111110              | 3E              | >                           | 126              | 1111110              | 7E              | ~                           |
| 63               | 0111111              | 3F              | ?                           | 127              | 1111111              | 7F              | DEL (Suprimir)              |

* __Caracteres imprimibles__: son aquellos que representan símbolos visibles que se pueden mostrar en la pantalla o imprimir en papel. Estos incluyen letras, números, signos de puntuación y símbolos especiales.
* __Caracteres de control__:  son aquellos que no se representan como símbolos visibles, sino que se utilizan para controlar dispositivos (como impresoras) o para gestionar el flujo de datos en la comunicación. Estos caracteres suelen realizar funciones específicas como mover el cursor, iniciar una nueva línea o enviar comandos a periféricos.

Ventajas de ASCII:

* __Simplicidad y facilidad de uso__: ASCII es fácil de entender y utilizar, ya que asigna un valor numérico a cada carácter común, como letras, números y símbolos.
* __Compatibilidad universal__: debido a su antigüedad y uso extensivo, ASCII es compatible con casi todos los sistemas informáticos y dispositivos de comunicación.
* __Tamaño reducido__: utiliza solo 7 bits por carácter, lo que lo hace eficiente en términos de almacenamiento y procesamiento de datos.
* __Estándar para la comunicación__: ASCII se convirtió en un estándar para la transmisión de texto en comunicaciones y protocolos de red, como SMTP para correo electrónico.
* __Soporte para caracteres de control__ : incluye caracteres de control que permiten manejar tareas como tabulaciones, saltos de línea y comandos de dispositivos periféricos.
* __Base para otros códigos__: ASCII sirvió de base para la creación de otros estándares, como ASCII Extendido y UTF-8, que amplían el soporte a caracteres de diferentes idiomas.

Desventajas de ASCII:

* __Limitación de caracteres__: ASCII solo puede representar 128 caracteres, lo que lo hace insuficiente para manejar idiomas con alfabetos más grandes, como chino, árabe o cualquier otro idioma que no use el alfabeto latino.
* __Sin soporte para caracteres especiales y acentos__: no incluye caracteres con acentos, símbolos de monedas internacionales, o letras no latinas, lo que limita su utilidad en aplicaciones globales.
* __Obsoleto para aplicaciones modernas__: ASCII no puede manejar emojis, caracteres especiales modernos o gráficos, que son comunes en la comunicación actual.
* __No escalable__: la limitación de 7 bits hace que ASCII no pueda ampliarse sin perder compatibilidad con los sistemas que lo utilizan, de ahí la necesidad de códigos más completos como _ASCII Extendido_.

### ASCII Extendido

__ASCII Extendido__ es una ampliación del código ASCII original de 7 bits que añade 128 caracteres adicionales, ampliando así el rango de 0 a 255 (utilizando 8 bits en lugar de 7). Esta extensión permite incluir más símbolos, caracteres gráficos, acentos y letras especiales que no están presentes en el estándar _ASCII_ básico.

Los primeros 128 caracters de código ASCII Extendido coinciden con los 128 caracteres de ASCII, con lo cual el código _ASCII Extendido_ es compatible con el código _ASCII_, mientras que los últimos 128 caracteres se utilizan para representar distintos caracteres extra que no se encuentran en la codificación _ASCII_ en función de la zona geográfica concreta. Hay distintos tipos de formatos _ASCII Extendido_ en función de la zona geográfica:

* __ISO 8859-1__: ASCII extendido para Europa Occidental (incluye símbolos como ñ o β)
* __ISO 8859-2__: ASCII extendido para Europa Central y del Este (incluye símbolos como Ź o č)
* __ISO 8859-3__: ASCII extendido para Europa del Sur (incluye símbolos como Ġ o Ï)
* __ISO 8859-4__: ASCII extendido para Europa del Norte (incluye símbolos como ø o å)
* __ISO 8859-5__: ASCII extendido para alfabeto cirílico (incluye símbolos como д o Ж)
* __ISO 8859-6__: ASCII extendido para alfabeto árabe (incluye símbolos como ن o ي)
* __ISO 8859-7__: ASCII extendido para alfabeto griego moderno (incluye símbolos como φ o α)
* __ISO 8859-8__: ASCII extendido para alfabeto hebreo (incluye símbolos como ץ o ק)
* __ISO 8859-9__: ASCII extendido, versión de 8859-1 que incluye símbolos turcos en lugar de otros poco utilizados.
* __ISO 8859-10__: ASCII extendido, versión de 8859-4 que incluye símbolos más utilizados en las lenguas nórdicas actuales.
* __ISO 8859-11__: ASCII extendido para alfabeto tailandés (incluye símbolos como ๗ o ๔)
* __ISO 8859-12__: ASCII extendido para alfabeto devanagari de India y Nepal que ya no se usa
* __ISO 8859-13__: ASCII extendido para alfabetos bálticos con símbolos que no estaban en 8859-4
* __ISO 8859-14__: ASCII extendido para alfabeto celta (incluye símbolos como ŵ o Ẃ)
* __ISO 8859-15__: ASCII extendido, versión de 8859-1 que incluye el símbolo del euro y símbolos de lenguas bálticas. Es el recomendado actualmente para Europa Occidental.
* __ISO 8859-16__: ASCII extendido, versión de 8859-1 pensada para los países del sureste de Europa

> __Nota__: para el alfabeto español se usa la codificación ASCII Extendido _ISO 8859-15_.

A continuación, se muestra la tabla de caracteres ASCII Extendido _ISO 8859-15_:

| **Código ASCII Extendido** | **Binario (8 bits)** | **Hexadecimal** | **Carácter** | **Código ASCII Extendido** | **Binario (8 bits)** | **Hexadecimal** | **Carácter** |
|----------------------------|----------------------|-----------------|--------------|----------------------------|----------------------|-----------------|--------------|
| 128                        | 10000000             | 80              | €            | 192                        | 11000000             | C0              | À            |
| 129                        | 10000001             | 81              |              | 193                        | 11000001             | C1              | Á            |
| 130                        | 10000010             | 82              | ‚            | 194                        | 11000010             | C2              | Â            |
| 131                        | 10000011             | 83              | ƒ            | 195                        | 11000011             | C3              | Ã            |
| 132                        | 10000100             | 84              | „            | 196                        | 11000100             | C4              | Ä            |
| 133                        | 10000101             | 85              | …            | 197                        | 11000101             | C5              | Å            |
| 134                        | 10000110             | 86              | †            | 198                        | 11000110             | C6              | Æ            |
| 135                        | 10000111             | 87              | ‡            | 199                        | 11000111             | C7              | Ç            |
| 136                        | 10001000             | 88              | ˆ            | 200                        | 11001000             | C8              | È            |
| 137                        | 10001001             | 89              | ‰            | 201                        | 11001001             | C9              | É            |
| 138                        | 10001010             | 8A              | Š            | 202                        | 11001010             | CA              | Ê            |
| 139                        | 10001011             | 8B              | ‹            | 203                        | 11001011             | CB              | Ë            |
| 140                        | 10001100             | 8C              | Œ            | 204                        | 11001100             | CC              | Ì            |
| 141                        | 10001101             | 8D              |              | 205                        | 11001101             | CD              | Í            |
| 142                        | 10001110             | 8E              | Ž            | 206                        | 11001110             | CE              | Î            |
| 143                        | 10001111             | 8F              |              | 207                        | 11001111             | CF              | Ï            |
| 144                        | 10010000             | 90              |              | 208                        | 11100000             | D0              | Ð            |
| 145                        | 10010001             | 91              | ‘            | 209                        | 11100001             | D1              | Ñ            |
| 146                        | 10010010             | 92              | ’            | 210                        | 11100010             | D2              | Ò            |
| 147                        | 10010011             | 93              | “            | 211                        | 11100011             | D3              | Ó            |
| 148                        | 10010100             | 94              | ”            | 212                        | 11100100             | D4              | Ô            |
| 149                        | 10010101             | 95              | •            | 213                        | 11100101             | D5              | Õ            |
| 150                        | 10010110             | 96              | –            | 214                        | 11100110             | D6              | Ö            |
| 151                        | 10010111             | 97              | —            | 215                        | 11100111             | D7              | ×            |
| 152                        | 10011000             | 98              | ˜            | 216                        | 11101000             | D8              | Ø            |
| 153                        | 10011001             | 99              | ™            | 217                        | 11101001             | D9              | Ù            |
| 154                        | 10011010             | 9A              | š            | 218                        | 11101010             | DA              | Ú            |
| 155                        | 10011011             | 9B              | ›            | 219                        | 11101011             | DB              | Û            |
| 156                        | 10011100             | 9C              | œ            | 220                        | 11101100             | DC              | Ü            |
| 157                        | 10011101             | 9D              |              | 221                        | 11101101             | DD              | Ý            |
| 158                        | 10011110             | 9E              | ž            | 222                        | 11101110             | DE              | Þ            |
| 159                        | 10011111             | 9F              | Ÿ            | 223                        | 11101111             | DF              | ß            |
| 160                        | 10100000             | A0              |              | 224                        | 11110000             | E0              | à            |
| 161                        | 10100001             | A1              | ¡            | 225                        | 11110001             | E1              | á            |
| 162                        | 10100010             | A2              | ¢            | 226                        | 11110010             | E2              | â            |
| 163                        | 10100011             | A3              | £            | 227                        | 11110011             | E3              | ã            |
| 164                        | 10100100             | A4              | ¤            | 228                        | 11110100             | E4              | ä            |
| 165                        | 10100101             | A5              | ¥            | 229                        | 11110101             | E5              | å            |
| 166                        | 10100110             | A6              | Š            | 230                        | 11110110             | E6              | æ            |
| 167                        | 10100111             | A7              | §            | 231                        | 11110111             | E7              | ç            |
| 168                        | 10101000             | A8              | ¨            | 232                        | 11111000             | E8              | è            |
| 169                        | 10101001             | A9              | ©            | 233                        | 11111001             | E9              | é            |
| 170                        | 10101010             | AA              | ª            | 234                        | 11111010             | EA              | ê            |
| 171                        | 10101011             | AB              | «            | 235                        | 11111011             | EB              | ë            |
| 172                        | 10101100             | AC              | ¬            | 236                        | 11111100             | EC              | ì            |
| 173                        | 10101101             | AD              | ­            | 237                        | 11111101             | ED              | í            |
| 174                        | 10101110             | AE              | ®            | 238                        | 11111110             | EE              | î            |
| 175                        | 10101111             | AF              | ¯            | 239                        | 11111111             | EF              | ï            |
| 176                        | 10110000             | B0              | °            | 240                        | 11110000             | F0              | ð            |
| 177                        | 10110001             | B1              | ±            | 241                        | 11110001             | F1              | ñ            |
| 178                        | 10110010             | B2              | ²            | 242                        | 11110010             | F2              | ò            |
| 179                        | 10110011             | B3              | ³            | 243                        | 11110011             | F3              | ó            |
| 180                        | 10110100             | B4              | ´            | 244                        | 11110100             | F4              | ô            |
| 181                        | 10110101             | B5              | µ            | 245                        | 11110101             | F5              | õ            |
| 182                        | 10110110             | B6              | ¶            | 246                        | 11110110             | F6              | ö            |
| 183                        | 10110111             | B7              | ·            | 247                        | 11110111             | F7              | ÷            |
| 184                        | 10111000             | B8              | ¸            | 248                        | 11111000             | F8              | ø            |
| 185                        | 10111001             | B9              | ¹            | 249                        | 11111001             | F9              | ù            |
| 186                        | 10111010             | BA              | º            | 250                        | 11111010             | FA              | ú            |
| 187                        | 10111011             | BB              | »            | 251                        | 11111011             | FB              | û            |
| 188                        | 10111100             | BC              | ¼            | 252                        | 11111100             | FC              | ü            |
| 189                        | 10111101             | BD              | ½            | 253                        | 11111101             | FD              | ý            |
| 190                        | 10111110             | BE              | ¾            | 254                        | 11111110             | FE              | þ            |
| 191                        | 10111111             | BF              | ¿            | 255                        | 11111111             | FF              | ÿ            |

Ventajas del ASCII extendido:

* __Mayor soporte de caracteres especiales__: incluye caracteres adicionales como letras con acentos, diéresis, símbolos de moneda y otros caracteres gráficos, lo que facilita la escritura en varios idiomas europeos.
* __Compatibilidad con sistemas antiguos__: fue ampliamente utilizado en sistemas operativos antiguos como MS-DOS y primeras versiones de Windows, lo que lo hace útil para mantener la compatibilidad con software y hardware legados.
* __Representación de símbolos gráficos__: proporciona varios símbolos gráficos que permitieron crear interfaces de usuario y gráficos simples en los sistemas de texto antiguos.
* __Soporte para símbolos de moneda__: incluye símbolos de moneda como el euro (€), libra (£) y yen (¥), lo cual es útil para aplicaciones financieras básicas.
* __Mayor flexibilidad lingüística__: permite escribir en idiomas europeos, con acceso a caracteres que no están presentes en el ASCII estándar, como las letras acentuadas y otros símbolos necesarios para el alemán, español, francés, etc.

Desventajas del ASCII Extendido: 

* __Falta de estandarización universal__: no existe una única versión del ASCII extendido; lo que puede causar inconsistencias y problemas de compatibilidad entre sistemas.
* __Limitaciones en la codificación de idiomas globales__: a pesar de su extensión, no cubre muchos idiomas no europeos, como el chino, japonés, árabe, o ruso, limitando su uso en un contexto verdaderamente internacional.
* __Problemas de codificación y pérdida de datos__: los problemas de compatibilidad entre diferentes versiones de ASCII extendido pueden llevar a la pérdida de datos o caracteres incorrectamente interpretados, especialmente en la migración de datos antiguos a sistemas modernos.
* __Limitado a 8 bits__: a pesar de su extensión, sigue estando limitado a 8 bits, lo que restringe el número total de caracteres a 256, insuficiente para cubrir todas las necesidades de representación de texto y símbolos en la actualidad.
* __Incompatibilidad con estándares modernos__: no es compatible con Unicode (parcialmente con _UTF-8_), lo que dificulta la integración con sistemas y aplicaciones actuales que dependen de este estándar más amplio y flexible.
* __Obsoleto en sistemas modernos__: con la introducción de Unicode, que soporta miles de caracteres de todos los idiomas, el ASCII Extendido ha quedado obsoleto y rara vez se utiliza en aplicaciones modernas.

> __Notas__: El _ASCII Extendido_ fue una evolución necesaria del ASCII original, añadiendo soporte para más caracteres y símbolos. Sin embargo, su falta de estandarización y capacidad limitada han llevado a su reemplazo por estándares más robustos como Unicode. Aunque sigue siendo útil para trabajar con sistemas antiguos y específicos, en el contexto moderno, sus desventajas superan las ventajas, especialmente cuando se trata de aplicaciones globales y multilingües.

### Unicode

_Unicode_ es un estándar de codificación de caracteres que se diseñó para proporcionar un conjunto único y universal para representar texto en la mayoría de los sistemas de escritura del mundo. Su objetivo es superar las limitaciones de los estándares anteriores, como ASCII y sus variantes extendidas, permitiendo una codificación coherente y consistente de textos multilingües en aplicaciones y sistemas informáticos.

Características de Unicode:
* __Universalidad__: soporta la mayoría de los caracteres de todos los lenguajes, incluidos los símbolos, emoticonos y caracteres especiales.
* __Escalabilidad__: puede representar más de un millón de caracteres diferentes gracias a su estructura flexible.
* __Compatibilidad__: compatible con ASCII y muchas otras codificaciones, facilitando la interoperabilidad entre sistemas antiguos y modernos.

Unicode puede ser codificado de diferentes maneras, siendo las más comunes _UTF-8_, _UTF-16_ y _UTF-32_. Cada una tiene sus propias características, ventajas y desventajas.

__UTF-8 (8-bit Unicode Transformation Format)__

_UTF-8_ es una codificación de longitud variable que utiliza entre 1 y 4 bytes para representar un carácter. Los primeros 128 caracteres (0-127) coinciden con los caracteres del código ASCII, lo que garantiza la compatibilidad con ASCII y con los primeros 128 primeros caracteres de cualquier codificación ASCII Extendido. Es muy eficiente en términos de almacenamiento para textos predominantemente en inglés y otros idiomas que utilizan caracteres latinos:
* __Carácter de 1 byte__: los caracteres ASCII básicos, como letras y números, utilizan solo 1 byte en UTF-8, por ejemplo, el carácter A, código UTF-8 41 (Hexadecimal).
* __Carácter de 2 bytes__: caracteres acentuados y símbolos de lenguas europeas que no están en el conjunto ASCII, como la ñ, utilizan 2 bytes, por ejemplo, el carácter ñ, código UTF-8 C3 B1 (Hexadecimal)
* __Carácter de 3 bytes__: los caracteres de lenguas asiáticas como el chino, japonés o coreano generalmente requieren 3 bytes. Por ejemplo, 日 (Kanji para "sol" en japonés), código UTF-8 E6 97 A5 (Hexadecimal)
* __Carácter de 4 bytes__: los caracteres especiales como emojis y algunos símbolos raros requieren 4 bytes.  Por ejemplo, 😀 (Emoji de cara sonriente).

La siguiente tabla contiene una pequeña muestra de caracteres de la codificación _UTF-8_:

| **Carácter (1 byte)** | **Código UTF-8** | **Bytes (Hexadecimal)** | **Carácter (3-4 bytes)** | **Código UTF-8** | **Bytes (Hexadecimal)** |
|-----------------------|------------------|-------------------------|--------------------------|------------------|-------------------------|
| A                     | \u0041           | 41                      | ñ                        | \u00F1           | C3 B1                   |
| B                     | \u0042           | 42                      | Ñ                        | \u00D1           | C3 91                   |
| C                     | \u0043           | 43                      | á                        | \u00E1           | C3 A1                   |
| D                     | \u0044           | 44                      | é                        | \u00E9           | C3 A9                   |
| E                     | \u0045           | 45                      | í                        | \u00ED           | C3 AD                   |
| F                     | \u0046           | 46                      | ó                        | \u00F3           | C3 B3                   |
| G                     | \u0047           | 47                      | ú                        | \u00FA           | C3 BA                   |
| H                     | \u0048           | 48                      | ü                        | \u00FC           | C3 BC                   |
| I                     | \u0049           | 49                      | À                        | \u00C0           | C3 80                   |
| J                     | \u004A           | 4A                      | Á                        | \u00C1           | C3 81                   |
| K                     | \u004B           | 4B                      | Â                        | \u00C2           | C3 82                   |
| L                     | \u004C           | 4C                      | Ã                        | \u00C3           | C3 83                   |
| M                     | \u004D           | 4D                      | Ä                        | \u00C4           | C3 84                   |
| N                     | \u004E           | 4E                      | È                        | \u00C8           | C3 88                   |
| O                     | \u004F           | 4F                      | É                        | \u00C9           | C3 89                   |
| P                     | \u0050           | 50                      | Ê                        | \u00CA           | C3 8A                   |
| Q                     | \u0051           | 51                      | Ë                        | \u00CB           | C3 8B                   |
| R                     | \u0052           | 52                      | Ì                        | \u00CC           | C3 8C                   |
| S                     | \u0053           | 53                      | Í                        | \u00CD           | C3 8D                   |
| T                     | \u0054           | 54                      | Î                        | \u00CE           | C3 8E                   |
| U                     | \u0055           | 55                      | Ï                        | \u00CF           | C3 8F                   |
| V                     | \u0056           | 56                      | Ò                        | \u00D2           | C3 92                   |
| W                     | \u0057           | 57                      | Ó                        | \u00D3           | C3 93                   |
| X                     | \u0058           | 58                      | Ô                        | \u00D4           | C3 94                   |
| Y                     | \u0059           | 59                      | Õ                        | \u00D5           | C3 95                   |
| Z                     | \u005A           | 5A                      | Ö                        | \u00D6           | C3 96                   |
| a                     | \u0061           | 61                      | ù                        | \u00F9           | C3 B9                   |
| b                     | \u0062           | 62                      | Ù                        | \u00D9           | C3 99                   |
| c                     | \u0063           | 63                      | Ú                        | \u00DA           | C3 9A                   |
| d                     | \u0064           | 64                      | Û                        | \u00DB           | C3 9B                   |
| e                     | \u0065           | 65                      | Ü                        | \u00DC           | C3 9C                   |
| f                     | \u0066           | 66                      | €                        | \u20AC           | E2 82 AC                |
| g                     | \u0067           | 67                      | ¿                        | \u00BF           | C2 BF                   |
| h                     | \u0068           | 68                      | ¡                        | \u00A1           | C2 A1                   |
| i                     | \u0069           | 69                      | 😀                       | \u1F600          | F0 9F 98 80             |
| j                     | \u006A           | 6A                      | ❤️                       | \u2764           | E2 9D A4                |
| k                     | \u006B           | 6B                      | ☀                       | \u2600           | E2 98 80                |
| l                     | \u006C           | 6C                      | ⚡                       | \u26A1           | E2 9A A1                |
| m                     | \u006D           | 6D                      | 🌙                       | \u1F319          | F0 9F 8C 99             |
| n                     | \u006E           | 6E                      | 🌟                       | \u1F31F          | F0 9F 8C 9F             |
| o                     | \u006F           | 6F                      | 🐍                       | \u1F40D          | F0 9F 90 8D             |
| p                     | \u0070           | 70                      | 🎉                       | \u1F389          | F0 9F 8E 89             |
| q                     | \u0071           | 71                      | 🥳                       | \u1F973          | F0 9F A5 B3             |
| r                     | \u0072           | 72                      | 🚀                       | \u1F680          | F0 9F 9A 80             |
| s                     | \u0073           | 73                      | 🎶                       | \u1F3B6          | F0 9F 8E B6             |
| t                     | \u0074           | 74                      | 📚                       | \u1F4DA          | F0 9F 93 9A             |
| u                     | \u0075           | 75                      | 📅                       | \u1F4C5          | F0 9F 93 85             |
| v                     | \u0076           | 76                      | 💡                       | \u1F4A1          | F0 9F 92 A1             |
| w                     | \u0077           | 77                      | 🔥                       | \u1F525          | F0 9F 94 A5             |
| x                     | \u0078           | 78                      | ⭐                       | \u2B50           | E2 AD 90                |
| y                     | \u0079           | 79                      | 🧡                       | \u1F9E1          | F0 9F A7 A1             |
| z                     | \u007A           | 7A                      | 💯                       | \u1F4AF          | F0 9F 92 AF             |
| +                     | \u002B           | 2B                      | 👑                       | \u1F451          | F0 9F 91 91             |
| -                     | \u002D           | 2D                      | 🌈                       | \u1F308          | F0 9F 8C 88             |
| *                     | \u002A           | 2A                      | 🎂                       | \u1F382          | F0 9F 8E 82             |
| /                     | \u002F           | 2F                      | 🍀                       | \u1F340          | F0 9F 8D 80             |
| .                     | \u002E           | 2E                      | 🎨                       | \u1F3A8          | F0 9F 8E A8             |
| ,                     | \u002C           | 2C                      | 🎁                       | \u1F381          | F0 9F 8E 81             |
| !                     | \u0021           | 21                      | 🌍                       | \u1F30D          | F0 9F 8C 8D             |

Ventajas:
* __Ahorra espacio__: utiliza solo un byte para los caracteres más comunes.
* __Uso extendido__: es la codificación preferida para la web, ya que HTML y muchos otros protocolos están optimizados para _UTF-8_.

Desventajas:
* Para caracteres de scripts más complejos (como chino o japonés), requiere más bytes (hasta 4), lo que puede hacer que los archivos sean más grandes.
* No siempre es la opción más rápida para operaciones que requieren acceso aleatorio a los caracteres debido a su longitud variable.

__UTF-16 (16-bit Unicode Transformation Format)__

_UTF-16_ utiliza uno o dos bloques de 16 bits (2 o 4 bytes) para representar un carácter. Utiliza 2 o 4 bytes para representar los caracteres: los caracteres más comunes se representan con un solo bloque de 16 bits, mientras que los caracteres menos comunes se representan con dos bloques de 16 bits.

Ventajas:
* Es eficiente para representar la mayoría de los idiomas asiáticos, ya que muchos de estos caracteres encajan en un solo bloque de 16 bits.
* Buena opción para aplicaciones que necesitan un equilibrio entre espacio y facilidad de acceso aleatorio.

Desventajas:
* Aunque los primeros 128 caracteres de UTF-16 corresponden a los mismos caracteres de ASCII, no son directamente compatibles en cuanto a la representación de bytes. ASCII utiliza 7 bits (generalmente agrupados en 8 bits) por carácter, mientras que UTF-16 usa 16 bits (2 bytes) o 32 bits (4 bytes) por carácter.
* Mayor uso de espacio para textos que usan predominantemente caracteres latinos o ASCII.

__UTF-32 (32-bit Unicode Transformation Format)__

__UTF-32__ utiliza un bloque fijo de 4 bytes (32 bits) para cada carácter, lo que significa que todos los caracteres se representan con exactamente 4 bytes. Debido a su longitud fija, cada carácter tiene una dirección y tamaño predecible, lo que facilita el acceso y la manipulación de texto.

Ventajas:
* Ideal para sistemas donde la simplicidad y el acceso aleatorio rápido a los caracteres son más importantes que la eficiencia en el uso del espacio.
* No hay ambigüedad en la codificación, ya que cada carácter ocupa siempre 4 bytes.

Desventajas:
* Uso de memoria extremadamente alto ya que ocupa mucho más espacio que UTF-8 y UTF-16, lo que lo hace ineficiente para la mayoría de las aplicaciones.
* Poco utilizado en la web debido a su ineficiencia de almacenamiento.
* Aunque los primeros 128 caracteres de UTF-32 corresponden a los mismos caracteres de ASCII, no son directamente compatibles en cuanto a la representación de bytes. ASCII utiliza 7 bits (generalmente agrupados en 8 bits) por carácter, mientras que UTF-32 usa 32 bits (4 bytes) por carácter.

Comparativa Resumida de UTF-8, UTF-16 y UTF-32:

| **Codificación** | **Bytes por carácter** | **Ventajas**                                        | **Desventajas**                                      |
|------------------|------------------------|-----------------------------------------------------|------------------------------------------------------|
| **UTF-8**        | 1-4                    | Compatibilidad con ASCII, eficiente para caracteres latinos | Ineficiente para algunos idiomas asiáticos           |
| **UTF-16**       | 2 o 4                  | Buena para muchos idiomas, equilibrio en espacio y acceso | No compatible con ASCII, mayor uso de espacio para caracteres latinos |
| **UTF-32**       | 4                      | Simplicidad y acceso directo                        | Muy ineficiente en espacio, uso limitado             |

Conclusión:
* __UTF-8__: es la codificación más utilizada y recomendada para la web y aplicaciones generales debido a su eficiencia y compatibilidad.
* __UTF-16__: se utiliza principalmente en entornos que requieren un compromiso entre espacio y rendimiento, como algunas aplicaciones de Windows y Java.
* __UTF-32__ es ideal para sistemas que necesitan una codificación fija por carácter, aunque su alto consumo de memoria limita su uso.

Estas codificaciones permiten a Unicode representar y gestionar texto de cualquier idioma de manera coherente y flexible, facilitando la comunicación y la interoperabilidad en la era digital.

A continuación, se muestra la palabra "_Año_" codificada en distintos sistemas de codificación como _ASCII_, _ISO 8859-15_, _UTF-8_, _UTF-16_ y _UTF-32_:

| **Codificación**               | **Bytes**                              | **Tamaño (bytes)** | **Descripción**                                                |
|------------------------------- |--------------------------------------- |------------------- |--------------------------------------------------------------- |
| ASCII                          | 41 - 6E - 6F                              | 3                  | No admite caracteres acentuados como ñ.                        |
| ISO 8859-15                    | 41 - F1 - 6F                              | 3                  | Incluye ñ (F1 en hexadecimal).                                 |
| UTF-8                          | 41 - C3 B1 - 6F                           | 4                  | ñ se representa en dos bytes C3 B1.                            |
| UTF-16                         | 00 41 - 00 F1 - 00 6F                     | 6                  | Cada carácter se codifica en 2 bytes.                          |
| UTF-32                         | 00 00 00 41 - 00 00 00 F1 - 00 00 00 6F   | 12                 | Cada carácter se codifica en 4 bytes.                          |

> __Nota__: ASCII original utiliza 7 bits para cada carácter, pero comúnmente se presenta en 8 bits con el bit más significativo (MSB) situado más a la izquierda configurado a 0 para alinearse con los sistemas de 8 bits.

## Codificación en Sistemas Windows

Windows utiliza varias páginas de códigos (code pages) para manejar la representación de caracteres en diferentes idiomas y contextos. Estas páginas de códigos determinan cómo se interpretan los bytes en archivos de texto, aplicaciones de consola, y otros entornos:
* __Code Page 437__ (_OEM - Estados Unidos_): la página de códigos original de IBM PC para inglés de EE.UU., usada principalmente en DOS. Soporta caracteres ASCII estándar y algunos gráficos de líneas.
* __Code Page 850__ (_OEM - Multilingüe Latin 1_): una codificación de caracteres usada principalmente en Europa Occidental y América Latina. Es una versión extendida del CP 437 y fue diseñada para soportar más caracteres acentuados y símbolos especiales comunes en lenguas europeas. Incluye caracteres ASCII y amplía con letras * acentuadas y caracteres especiales.
* __Code Page 1252__ (_ANSI - Latin 1_): similar al CP 850, pero es una codificación Windows estándar (ANSI) usada en aplicaciones gráficas y archivos de texto. Incluye caracteres europeos occidentales, acentuados, símbolos monetarios, y caracteres de control.
* __Code Page 1250__ (_Central European_): utilizada para lenguas centroeuropeas como polaco, checo, húngaro, etc. Incluye caracteres específicos de lenguas eslavas y centroeuropeas.
* __Code Page 65001__ (_UTF-8_): codificación moderna que soporta una amplia gama de caracteres Unicode. Se utiliza para aplicaciones internacionales y es compatible con muchos idiomas. Soporta virtualmente todos los caracteres definidos en Unicode, incluyendo emojis, caracteres asiáticos, etc.

__Code Page 850__

La _Code Page 850_ es una de las páginas de códigos más utilizadas en entornos europeos y de habla hispana, especialmente en contextos de consola o sistemas antiguos. Esta página de códigos extiende la CP 437 con más caracteres latinos, lo que la hace más adecuada para trabajar con idiomas que requieren acentos y símbolos adicionales. Se basa en ASCII extendido, que incluye los 128 caracteres básicos de ASCII (como letras inglesas, números y símbolos comunes) y añade 128 caracteres adicionales específicos de la región, como letras acentuadas y símbolos gráficos.
ASCII extendido fue diseñado para proporcionar soporte básico para idiomas europeos occidentales en sistemas que no requerían soporte para los miles de caracteres que Unicode puede representar.

Características de Code Page 850:

* __Diseño multilingüe__: está diseñada para soportar varios idiomas europeos occidentales, incluyendo español, francés, alemán, italiano, y portugués, entre otros.
* __Compatibilidad__: es compatible con la mayoría de los caracteres utilizados en aplicaciones y sistemas de consola en Windows, especialmente aquellos desarrollados en la era de MS-DOS.
* __Conjunto de caracteres__: soporta caracteres ASCII básicos (del 0 al 127), extendiéndose con caracteres latinos acentuados y otros símbolos (del 128 al 255), como "ñ", "á", "é", "í", "ó", "ú", y símbolos como "ç" y "ß".
* __Uso común__: se utiliza frecuentemente en la consola de Windows para aplicaciones heredadas o cuando se necesita compatibilidad con sistemas antiguos que no soportan UTF-8 o Unicode.

Limitaciones:
* No soporta caracteres de otros alfabetos como cirílico, griego, o caracteres asiáticos.
* Puede presentar problemas al manejar caracteres especiales que no estén incluidos en su rango, como algunos símbolos modernos o emojis.

Ejemplos de caracteres de Code Page 850:

* __Letras sin acentuadas__: A (0x41), B (0x42), C (0x43), D (0x44).
* __Letras acentuadas__: á (0xA0), é (0x82), í (0xA1), ó (0xA2), ú (0xA3), ñ (0xA4).
* __Símbolos especiales__: ç (0x87), ß (0xE1), £ (0x9C), § (0xA7).
* __Caracteres de control y gráficos__: incluye algunos caracteres gráficos como líneas y cuadros, útiles para aplicaciones de consola.

Conclusión:
La _Code Page 850_ sigue siendo relevante en entornos donde la compatibilidad con aplicaciones antiguas es esencial. Aunque ha sido en gran parte reemplazada por UTF-8 en aplicaciones modernas, _Code Page 850_ es la codificación de caracteres utilizada principalmente en sistemas DOS.

A continuación, se muestra la tabla de caracteres de la codificación CP 850 (Code Page 850), que muestra los caracteres correspondientes a los códigos del 0 al 255:

| **Código** | **Hexadecimal** | **Carácter** | **Código** | **Hexadecimal** | **Carácter** | **Código** | **Hexadecimal** | **Carácter** | **Código** | **Hexadecimal** | **Carácter** |
|------------|-----------------|--------------|------------|-----------------|--------------|------------|-----------------|--------------|------------|-----------------|--------------|
| 0          | 0x00            | NUL          | 32         | 0x20            | (espacio)    | 64         | 0x40            | @            | 96         | 0x60            | `            |
| 1          | 0x01            | SOH          | 33         | 0x21            | !            | 65         | 0x41            | A            | 97         | 0x61            | a            |
| 2          | 0x02            | STX          | 34         | 0x22            | "            | 66         | 0x42            | B            | 98         | 0x62            | b            |
| 3          | 0x03            | ETX          | 35         | 0x23            | #            | 67         | 0x43            | C            | 99         | 0x63            | c            |
| 4          | 0x04            | EOT          | 36         | 0x24            | $            | 68         | 0x44            | D            | 100        | 0x64            | d            |
| 5          | 0x05            | ENQ          | 37         | 0x25            | %            | 69         | 0x45            | E            | 101        | 0x65            | e            |
| 6          | 0x06            | ACK          | 38         | 0x26            | &            | 70         | 0x46            | F            | 102        | 0x66            | f            |
| 7          | 0x07            | BEL          | 39         | 0x27            | '            | 71         | 0x47            | G            | 103        | 0x67            | g            |
| 8          | 0x08            | BS           | 40         | 0x28            | (            | 72         | 0x48            | H            | 104        | 0x68            | h            |
| 9          | 0x09            | TAB          | 41         | 0x29            | )            | 73         | 0x49            | I            | 105        | 0x69            | i            |
| 10         | 0x0A            | LF           | 42         | 0x2A            | *            | 74         | 0x4A            | J            | 106        | 0x6A            | j            |
| 11         | 0x0B            | VT           | 43         | 0x2B            | +            | 75         | 0x4B            | K            | 107        | 0x6B            | k            |
| 12         | 0x0C            | FF           | 44         | 0x2C            | ,            | 76         | 0x4C            | L            | 108        | 0x6C            | l            |
| 13         | 0x0D            | CR           | 45         | 0x2D            | -            | 77         | 0x4D            | M            | 109        | 0x6D            | m            |
| 14         | 0x0E            | SO           | 46         | 0x2E            | .            | 78         | 0x4E            | N            | 110        | 0x6E            | n            |
| 15         | 0x0F            | SI           | 47         | 0x2F            | /            | 79         | 0x4F            | O            | 111        | 0x6F            | o            |
| 16         | 0x10            | DLE          | 48         | 0x30            | 0            | 80         | 0x50            | P            | 112        | 0x70            | p            |
| 17         | 0x11            | DC1          | 49         | 0x31            | 1            | 81         | 0x51            | Q            | 113        | 0x71            | q            |
| 18         | 0x12            | DC2          | 50         | 0x32            | 2            | 82         | 0x52            | R            | 114        | 0x72            | r            |
| 19         | 0x13            | DC3          | 51         | 0x33            | 3            | 83         | 0x53            | S            | 115        | 0x73            | s            |
| 20         | 0x14            | DC4          | 52         | 0x34            | 4            | 84         | 0x54            | T            | 116        | 0x74            | t            |
| 21         | 0x15            | NAK          | 53         | 0x35            | 5            | 85         | 0x55            | U            | 117        | 0x75            | u            |
| 22         | 0x16            | SYN          | 54         | 0x36            | 6            | 86         | 0x56            | V            | 118        | 0x76            | v            |
| 23         | 0x17            | ETB          | 55         | 0x37            | 7            | 87         | 0x57            | W            | 119        | 0x77            | w            |
| 24         | 0x18            | CAN          | 56         | 0x38            | 8            | 88         | 0x58            | X            | 120        | 0x78            | x            |
| 25         | 0x19            | EM           | 57         | 0x39            | 9            | 89         | 0x59            | Y            | 121        | 0x79            | y            |
| 26         | 0x1A            | SUB          | 58         | 0x3A            | :            | 90         | 0x5A            | Z            | 122        | 0x7A            | z            |
| 27         | 0x1B            | ESC          | 59         | 0x3B            | ;            | 91         | 0x5B            | [            | 123        | 0x7B            | {            |
| 28         | 0x1C            | FS           | 60         | 0x3C            | <            | 92         | 0x5C            | \            | 124        | 0x7C            | |            |
| 29         | 0x1D            | GS           | 61         | 0x3D            | =            | 93         | 0x5D            | ]            | 125        | 0x7D            | }            |
| 30         | 0x1E            | RS           | 62         | 0x3E            | >            | 94         | 0x5E            | ^            | 126        | 0x7E            | ~            |
| 31         | 0x1F            | US           | 63         | 0x3F            | ?            | 95         | 0x5F            | _            | 127        | 0x7F            | DEL          |
| 128        | 0x80            | Ç            | 160        | 0xA0            | á            | 192        | 0xC0            | À            | 224        | 0xE0            | à            |
| 129        | 0x81            | ü            | 161        | 0xA1            | í            | 193        | 0xC1            | Á            | 225        | 0xE1            | á            |
| 130        | 0x82            | é            | 162        | 0xA2            | ó            | 194        | 0xC2            | Â            | 226        | 0xE2            | â            |
| 131        | 0x83            | â            | 163        | 0xA3            | ú            | 195        | 0xC3            | Ã            | 227        | 0xE3            | ã            |
| 132        | 0x84            | ä            | 164        | 0xA4            | ñ            | 196        | 0xC4            | Ä            | 228        | 0xE4            | ä            |
| 133        | 0x85            | à            | 165        | 0xA5            | Ñ            | 197        | 0xC5            | Å            | 229        | 0xE5            | å            |
| 134        | 0x86            | å            | 166        | 0xA6            | ª            | 198        | 0xC6            | Æ            | 230        | 0xE6            | æ            |
| 135        | 0x87            | ç            | 167        | 0xA7            | §            | 199        | 0xC7            | Ç            | 231        | 0xE7            | ç            |
| 136        | 0x88            | ê            | 168        | 0xA8            | ¬            | 200        | 0xC8            | È            | 232        | 0xE8            | è            |
| 137        | 0x89            | ë            | 169        | 0xA9            | ©            | 201        | 0xC9            | É            | 233        | 0xE9            | é            |
| 138        | 0x8A            | è            | 170        | 0xAA            | ª            | 202        | 0xCA            | Ê            | 234        | 0xEA            | ê            |
| 139        | 0x8B            | ï            | 171        | 0xAB            | «            | 203        | 0xCB            | Ë            | 235        | 0xEB            | ë            |
| 140        | 0x8C            | î            | 172        | 0xAC            | ¬            | 204        | 0xCC            | Ì            | 236        | 0xEC            | ì            |
| 141        | 0x8D            | ì            | 173        | 0xAD            | (guion)      | 205        | 0xCD            | Í            | 237        | 0xED            | í            |
| 142        | 0x8E            | Ä            | 174        | 0xAE            | ®            | 206        | 0xCE            | Î            | 238        | 0xEE            | î            |
| 143        | 0x8F            | Å            | 175        | 0xAF            | ¯            | 207        | 0xCF            | Ï            | 239        | 0xEF            | ï            |
| 144        | 0x90            | É            | 176        | 0xB0            | °            | 208        | 0xD0            | Ð            | 240        | 0xF0            | ð            |
| 145        | 0x91            | æ            | 177        | 0xB1            | ±            | 209        | 0xD1            | Ñ            | 241        | 0xF1            | ñ            |
| 146        | 0x92            | Æ            | 178        | 0xB2            | ²            | 210        | 0xD2            | Ò            | 242        | 0xF2            | ò            |
| 147        | 0x93            | ô            | 179        | 0xB3            | ³            | 211        | 0xD3            | Ó            | 243        | 0xF3            | ó            |
| 148        | 0x94            | ö            | 180        | 0xB4            | ´            | 212        | 0xD4            | Ô            | 244        | 0xF4            | ô            |
| 149        | 0x95            | ò            | 181        | 0xB5            | µ            | 213        | 0xD5            | Õ            | 245        | 0xF5            | õ            |
| 150        | 0x96            | û            | 182        | 0xB6            | ¶            | 214        | 0xD6            | Ö            | 246        | 0xF6            | ö            |
| 151        | 0x97            | ù            | 183        | 0xB7            | ·            | 215        | 0xD7            | ×            | 247        | 0xF7            | ÷            |
| 152        | 0x98            | ÿ            | 184        | 0xB8            | ¸            | 216        | 0xD8            | Ø            | 248        | 0xF8            | ø            |
| 153        | 0x99            | Ö            | 185        | 0xB9            | ¹            | 217        | 0xD9            | Ù            | 249        | 0xF9            | ù            |
| 154        | 0x9A            | Ü            | 186        | 0xBA            | º            | 218        | 0xDA            | Ú            | 250        | 0xFA            | ú            |
| 155        | 0x9B            | ø            | 187        | 0xBB            | »            | 219        | 0xDB            | Û            | 251        | 0xFB            | û            |
| 156        | 0x9C            | £            | 188        | 0xBC            | ¼            | 220        | 0xDC            | Ü            | 252        | 0xFC            | ü            |
| 157        | 0x9D            | Ø            | 189        | 0xBD            | ½            | 221        | 0xDD            | Ý            | 253        | 0xFD            | ý            |
| 158        | 0x9E            | ×            | 190        | 0xBE            | ¾            | 222        | 0xDE            | Þ            | 254        | 0xFE            | þ            |
| 159        | 0x9F            | ƒ            | 191        | 0xBF            | ¿            | 223        | 0xDF            | ß            | 255        | 0xFF            | ÿ            |

Para ver la codificación que estamos utilizando en la Terminal de Windows se utiliza el comando _chcp_:

```
C:\Users\Jonay>chcp
Página de códigos activa: 850
```

Si queremos cambiar la codificación de la consola de Windows a _UTF-8_ para soportar más caracteres especiales, podemos hacerlo con el comando _chcp_:

```
C:\Users\Jonay>chcp 65001
```

Esto cambiará la página de códigos a _UTF-8_, permitiendo manejar caracteres Unicode.

> __Nota__: recuerda que la codificación afecta cómo se muestran los caracteres en la consola y cómo se manejan archivos de texto.
