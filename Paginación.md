#Organización-y-Arquitectura-de-Computadores #apunte

---
Los tamaños de página son uniformes de 4Kbytes. Para un espacio lineal de 4GB dividido en páginas de 4Kb necesitamos $2^{20}$ páginas. Con 20 Bits para la dirección de la página más 12 bits de atributos conformamos un descriptor de páginas de 32 bits.
## Niveles
Tener $2^{20}$ descriptores de 4 Bytes cada uno nos ocuparía 4MB (un montón) así que la página queda organizada por niveles. Está pensado como un sistema de administración por tarea, de modo que cada tarea tiene su propia estructura de páginas.
![[image-67.webp]]
1. Una página para el directorio de tablas de pagina. Al inicio solo tendrá un descriptor válido.
2. Una página de 4kb para una tabla de páginas apuntada por la única entrada del directorio. Esta tabla puede almacenar hasta 1024 descriptores de páginas de 4kb.
De este modo se puede iniciar un proceso con solo dos tablas de 4 Kbytes para administrar hasta 1024 paginas de 4 Kbytes, es decir 4 Mbytes para acomodar su código y datos.
## Administración Dinámica
El Sistema Operativo habilita a cada proceso unas pocas paginas para código y datos. Conforme el proceso requiera memoria, se irán agregando en la tabla de páginas del segundo nivel de la estructura los descriptores necesarios. Llegado a ese límite, si solicita mas memoria, se genera un segundo descriptor en el `DPT`, que permitirá habilitar otros 4 Kbytes de memoria.
## Traducción de direcciones
![[image-68.webp]]
El registro `CR3` contiene la dirección física donde se inicia el directorio de tabla de páginas.
### Descriptores en el Directorio de Tablas de Páginas

![[image-69.webp]]
- Tiene 20 bits para indicar el número de página en la que se encuentra la tabla de páginas; y algunos bits de control.
### Descriptores en la Tabla de Páginas

![[image-70.webp]]
- Tiene 20 bits para indicar el número de la página en memoria; y algunos bits de control.

### Atributos
Son los mismos para el directorio y para la tabla
- G - **Global**: Si `CR4.PGE = 1` Tiene efecto la activación de la funcionalidad Global, que otorga ese carácter a la traducción de esa página almacenada en la TLB. La entrada no se flushea cuando se recarga el registro `CR3`.
- D - **Dirty**: Indica que la pagina ha sido modificada (está “sucia”). El Sistema Operativo lo inicializa en ’0’, y se setea en forma automática en cada escritura en la pagina. El algoritmo de swap en el momento de desalojar una pagina de la memoria RAM, analiza este bit y no la copia al disco si no ha sido modificada. Solo tiene sentido en descriptores de Pagina (de 4K o 4M) y es ignorado en las entradas de la `DPT`
- A - **Accedido**: Se setea cada vez que la pagina es accedida. El Sistema Operativo puede contabilizar los accesos de modo de elaborar estadísticas de uso que permitan identificar cual es la pagina candidata a ser desalojada llegado el momento.
- U/S - **User / Supervisor**: 
	- 0 = Supervisor (Kernel)
	- 1 = Usuario.
	En general corresponden `U/S = 0` a `DPL = 00`, y `U/S = 1` al resto de los valores de `DPL`. El procesador chequea el `CPL` del segmento de codigo para autorizar o no el acceso a las paginas de acuerdo a la combinacion de los valores de `U/S` de los diferentes descriptores de su estructura. 
- R/W - **Readable / Writable**: Establece si la pagina es Read Only (0) o si puede ser escrita (1).
- P - **Presente**: Indica si la pagina está en la memoria. Genera una excepción `Page Fault` cuando se intenta acceder a una dirección de memoria que tiene al menos un descriptor con `P = 0` a lo largo de la estructura de tablas. Cuando `P = 0`, el resto del contenido del descriptor se ignora. 