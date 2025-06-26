#Organización-y-Arquitectura-de-Computadores  #apunte

---
## Tamaños Típicos

| Tipo           | Bytes | Rango (decimal)                              | Rango (binario)          |
| -------------- | ----- | -------------------------------------------- | ------------------------ |
| char           | 1     | $-128$ a $127$                               | $−2^7$ a $2^7 − 1$       |
| unsigned char  | 1     | $0$ a $255$                                  | $0$ a $2^8 − 1$          |
| short          | 2     | $-32768$ a $32767$                           | $−2^{15}$ a $2^{15} − 1$ |
| unsigned short | 2     | $0$ a $65535$                                | $0$ a $2^{16} − 1$       |
| int            | 4     | $-2147483648$ a $2147483647$                 | $−2^{31}$ a $2^{31} − 1$ |
| unsigned int   | 4     | $0$ a $4294967295$                           | $0$ a $2^{32} − 1$       |
| long           | 8     | $−9,2 \times 10^{18}$ a $9,2 \times 10^{18}$ | $−2^{63}$ a $2^{63} − 1$ |
| unsigned long  | 8     | $0$ a $1,84 \times 10^{19}$                  | $0$ a $2^{64} − 1$       |
El tamaño de los tipos de datos en C no está definido por el estándar, sino que depende de la arquitectura y del compilador. Por ejemplo, en una arquitectura de 32 bits, un int puede ser de 4 bytes, mientras que en una arquitectura de 64 bits, puede ser de 8 bytes.
## Enteros de Tamaño Fijo
Se consiguen utilizando la biblioteca `stdint.h`

|          | 8 bits/1 byte | 16 bits/2 bytes | 32 bits/4 bytes | 64 bits/8 bytes |
| -------- | ------------- | --------------- | --------------- | --------------- |
| Signed   | int8_t        | int16_t         | int32_t         | int64_t         |
| Unsigned | uint8_t       | uint16_t        | uint32_t        | uint64_t        |
## Strings
C no contiene un tipo de dato nativo strings, pero sí tiene un tipo char que sirve para representar caracteres en formato ASCII. Con este tipo, podemos definir strings como un array de caracteres. La convención en C es que todo string termina en un carácter nulo `('\0')`
## Proceso de Compilación
El proceso de compilación se divide en varias etapas: 
- **Preprocesamiento**: el preprocesador de C toma el código fuente y realiza una serie de transformaciones. Los archivos fuentes suelen tener extensión .c y los headers .h. Por ejemplo, se incluyen los archivos de cabecera, se reemplazan los macros, se eliminan los comentarios, etc. 
- **Compilación**: el compilador de C toma el archivo generado por el preprocesador y lo convierte en un archivo en lenguaje ensamblador. 
- **Ensamblado**: el ensamblador toma el archivo generado por el compilador y lo convierte en un archivo objeto. Este archivo tiene extensión .o y contiene el código máquina correspondiente al código fuente. 
- **Linking**: el linker toma los archivos objeto generados por el ensamblador y los combina en un solo archivo ejecutable. El resultado de esta etapa es un archivo ejecutable. Además, en esta etapa se resuelven las referencias a funciones y variables globales. Si una función o variable global es declarada en un archivo fuente y definida en otro, el linker se encarga de resolver esta referencia. Si no se encuentra la definición de una función o variable global, el linker arroja un error. Si una definición de una función o variable global aparece en más de un archivo objeto, el linker arroja un error (ODR). Las bibliotecas estáticas y dinámicas que contienen funciones ya compiladas de la biblioteca estándar o de terceros también se linkean en esta etapa.
## Tipos de Memoria
- **Memoria de código**: es la memoria que se reserva para almacenar el código del programa. Su tamaño no puede cambiar durante la ejecución del programa. Es de solo lectura. Se define en el text segment del programa. 
- **Memoria automática**: es un tipo de memoria que se reserva en el momento de la ejecución. almacena las variables locales. Su tamaño puede cambiar durante la ejecución del programa. Es el llamado **stack** o pila. 
- **Memoria estática**: es la memoria que se reserva en el momento de la compilación. almacena las variables globales y estáticas. Su tamaño no puede cambiar durante la ejecución del programa. Se definen en el **data segment** del programa. 
- **Memoria dinámica**: es la memoria que se reserva en el momento de la ejecución, pero su tamaño puede cambiar durante la misma. Se utiliza para almacenar datos que no se conocen en tiempo de compilación. Es el llamado **heap**.