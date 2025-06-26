#Organización-y-Arquitectura-de-Computadores #apunte

---
## Formato de un Selector de Segmento

![[image-64.webp]]
- **Index**: índice en la tabla de descriptores.  `Index = n` corresponde al n-ésimo elemento de la Tabla. Cada tabla puede alojar $2^{13}$ (8192) descriptores. 
- **Table Indicator**: selecciona en que tabla de descriptores debe buscarse el segmento seleccionado: 
	- `TI = 0` $\Rightarrow$ `GDT`
	- `Si TI = 1`$\Rightarrow$ `LDT` 
- **Requested Privilege Level**: nivel de privilegio que declara tener el dueño del segmento, o sea el grado de autorización que se tiene para cada acceso: `00` es el mayor privilegio, y `11` el menor.
## Formato de un Descriptor de Segmento de 32 Bits

![[image-65.webp]]
- **Dirección Base**: es la dirección a partir de la cual se despliega en forma continua el segmento.
- **Límite**: especifica el máximo offset que puede tener un byte direccionable dentro del segmento. Suele confundirse este concepto con el tamaño del segmento. En realidad el límite es el tamaño del segmento menos 1, ya que el offset del primer byte del segmento es 0.
- G - **Granularity**. Establece la unidad de medida del campo límite:
	- `G = 0` $\Rightarrow$ máximo offset de un byte $=$ límite
	- `G = 1` $\Rightarrow$ máximo offset $=$ límite $*$ 0x1000 $+$ 0xFFF. 
- D/B - **Default / Big**: Configura el tamaño de los segmentos. 
	- `D/B = 0` $\Rightarrow$ segmento es de 16 bits. 
	- `D/B = 1` $\Rightarrow$ segmento de 32 bits.
- **L**. El procesador solo mira este bit en el Modo IA-32e.
- AVL - **Available**: Este bit no es usado por el procesador
- P - **Present**: indica si segmento correspondiente está presente en la memoria RAM. Si es 0, el segmento está en la memoria virtual (disco). Un acceso a un segmento cuyo bit P está en 0, genera una excepción de segmento No Presente. Esto permite al kernel solucionar el problema, efectuando el “swap” entre el disco a memoria para ponerlo accesible en RAM.
- A - **Accedido**: Se setea cada vez que se accede una dirección en el segmento. Permite al Sistema Operativo contabilizar los accesos para elaborar estadísticas de uso que permitan identificar cual es el segmento a ser desalojado llegado el momento. 
- DPL - **Descriptor Privilege Level**. Nivel de privilegio que debe tener el segmento que contiene el código que pretende acceder a este segmento.
- S - **System**: permite administrar en las tablas de descriptores, dos clases bien determinadas de segmentos:
	- Segmentos de Código o Datos
	- Segmentos de Sistema. Tienen diferentes formatos y en general no se refieren a zonas de memoria (salvo TSS). En general se refieren a mecanismos de uso de recursos del procesador por parte del kernel (por ello reciben el nombre de descriptores de Sistema, ya que son para uso exclusivo del Sistema Operativo) 
- **Tipo**. Este campo de 4 bits indica el tipo del descriptor según una tabla.
## Generación de la [[Tipos de Memoria|Dirección Lineal]]

![[image-66.webp]]
1. El procesador evalúa el estado del bit 2 del selector, es decir TI. En este caso TI = 0 entonces va a la [[Global Descriptor Table|GDT]]. 
2. El registro GDTR del procesador contiene en su campo dirección base la dirección física en donde comienza la [[Global Descriptor Table|GDT]]
3. El valor n contenido por los 13 bits del campo Index del selector referencia al n-ésimo elemento de la [[Global Descriptor Table|GDT]].
4. El procesador accede a la dirección de memoria física dada por: $GDT.Base + 8 ∗ Index$ y lee 8 bytes a partir de ella. 
5. Una vez leído el descriptor, internamente reordena la dirección base y el límite y agrupa los Atributos.
6. La Unidad de protección verifica que el offset contenido en el registro correspondiente de la dirección lógica corresponda al rango de offsets validos del segmento de acuerdo al valor del campo límite, y de los bits de atributos `G`, `D/B`, y `ED`.
7. La Unidad de protección chequea que la operación a realizarse en el segmento se corresponda con los bits de Atributos `R` y `C`, si es de código, `W` si es de datos, que el código de acceso tenga los privilegios necesarios de acuerdo a los bits `DPL` del descriptor, que `P = 1`, entre los mas comunes. 
8. Si todo esta de manera correcta, el procesador suma el valor de offset contenido en la dirección lógica con la dirección base del segmento y conforma la dirección lineal.
