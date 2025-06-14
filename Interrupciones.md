#Organización-y-Arquitectura-de-Computadores #apunte

---

## Interrupciones de Hardware
Son señales eléctricas enviadas desde un dispositivo I/O que genera un evento en el procesador.
Se manejan a través de un controlador llamado controlador de interrupciones (PIC) que las gestiona y prioriza y envía a la CPU secuencialmente.
**Características:** Son asincrónicas y no determinísticas.
## Interrupciones de Software
Se producen cuando un programa ejecuta la instrucción `INT <TYPE>`. 
Por ejemplo tareas que necesitan invocar servicios del kernel (ej.: printf).
**Características:** Son determinísticas y sincrónicas ya que se trata de instrucciones.
## Interrupciones Internas / Excepciones
Son generadas por la CPU como consecuencia de una situación que le impide completar la ejecución de una instrucción en curso.
Por ejemplo: división por cero, page fault, violación de segmento.
Se identifican con un valor numérico de 8 bits denominado `Tipo`, eso quiere decir que podemos tener 256 fuentes de interrupción distintas.
Se dividen en tres tipos:
- **Fault:** excepción que se puede corregir y retomar la ejecución. El proceso guarda en la pila la dirección de la instrucción que produjo la falla.
- **Trap:** Excepción producida inmediatamente a continuación de una instrucción de trap. Algunas permiten retomar la ejecución y otras no. El procesador guarda en la pila la dirección de la instrucción siguiente.
- **Abort:** Excepción que no permite recuperar la ejecución de la tarea. En algunos casos tampoco se puede determinar el causante. Reporta errores severos de hardware o inconsistencias en tablas del sistema.
## Manejo de Interrupciones
El Kernel es el encargado de manejar las interrupciones, por lo que si se estaba ejecutando código a nivel usuario se debe realizar un cambio en el nivel de privilegios.
En una PC, las interrupciones las gestiona un módulo llamado [[PIC|PIC: programable interrupt controller]].
DUDA:
	como es el tema de lo que se guarda en la pila antes de pasar al habdler?
	no entiendo  bien esta foto
	![[image-58.webp]]
# Manejo en Modo Protegido
Se define una `IDT` (Interrupt Descriptor Table) que almacena [[descriptores]]. Tiene únicamente 256 entradas, una para cada `tipo` de interrupción.
Los descriptores son de tipo:
- Interrupt Gate (16, 32, o 64 bits)
- Trap Gate (16, 32, o 64 bits)
- Task Gate (32 bits) (SOLO EXISTEN EN 32 BITS)
En todos los casos se trata de descriptores del sistema (`bit S=0`). No se debe definir un descriptor de datos ni de código.
El procesador carga el Selector de segmento y el offset y fetchea la instrucción que corresponde al handler de la interrupción.
## Formato de Los Descriptores
![[image-56.webp]]
### Vectorización de Interrupción
![[image-57.webp]]
Mediante el registro `IDTR` localizamos la `IDT`. Luego, según el `type` de la interrupción que se haya levantado, buscamos la entrada en la tabla y miramos el descriptor. El campo de selector de segmento apunta a una entrada en la `GDT/IDT` (para la materia usamos siempre GDT). De este selector obtenemos el segmento donde está definida la rutina de atención a la interrupción. A esta dirección base le sumamos el offset del primer descriptor y obtenemos el punto de entrada de la rutina.

DUDA:
	por que la puerta para int n tiene un campo de selector de segmento? se refiere a que la rutina está en ese segmento? por que entonces luego va a la idt? no puede ir directamente?