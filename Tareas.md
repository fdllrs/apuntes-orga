Unidad de trabajo que un procesador puede despachar, ejecutar, y detener a voluntad.
## Definiciones
- **Espacio de ejecución**: el conjunto de secciones de código, datos y pila que componen la tarea.
- **Contexto de ejecución**: valores delos registros del procesador en cada momento.
- **Espacio de Contexto de ejecución**: un bloque de memoria en el SO que almacena el contexto completo de ejecución.
## Context Switch
El proceso que hay que hacer para cambiar de tarea.
1. Resguardar los registros core y cualquier otro del modelo de programador en un área de memoria accesible solo para el kernel. En general el contexto forma parte de una estructura más amplia llamada Task Control Block (TCB) o Process Control Block (PCB).
2. Determinar la ubicación del TCB de la siguiente tarea en la lista de tareas en ejecución.
3. Extraer el contexto de la nueva tarea y copiar registro a registro a la CPU
## Scheduler
- Define el intervalo de tiempo o *time frame* que es la unidad de tiempo de trabajo.
- Maneja las prioridades de las tareas, asignándole un porcentaje del time frame. Se tiene en cuenta la prioridad, preemption, atomicidad de operaciones, concurrencia, latencia, paralelismo, etc. a la hora de determinar que tarea se va a ejecutar.
El scheduler le da a cada tarea unos milisegundos para progresar, luego de los cuales se suspende la tarea y se despacha la siguiente de la lista.
### Operatoria multitarea
1. El procesador está ejecutando la tarea 1 durante un determinado tiempo T1
2. Se cambia la tarea: se guarda el contexto (todos los registros) de la tarea 1 y se pasa a la tarea 2 (definida por el scheduler según las prioridades)
3. Se ejecuta la tarea 2 durante algún tiempo T2 (según la prioridad)
4. Se repite el proceso de cambio de tarea.
# Task Switch
## Task Status Segment (TSS)
Es el lugar de memoria previsto como el espacio del contexto de una tarea.
![[image-71.webp|233x291]]
- Se guardan los valores de `SS` y `ESP` para los stacks de nivel 2, 1, y 0. El del nivel 3 eventualmente estará en los registros `SS:ESP`.
- El Flag T genera una excepción de debug cada vez que se conmuta de tarea (Pentium Pro en adelante), si esta en `1`. 
- I/O Map Base Address: Offset de 16 bits desde el inicio del `TSS` hasta el inicio del Mapa de permisos de E/S
## Descriptor TSS

![[image-72.webp]]
- B - **Busy**: Sirve para evitar recursividad en el anidamiento de tareas.
- **Límite**: debe ser mayor o igual a `67h` (mínimo tamaño del segmento es 0x68, o $103_{10}$. De otro modo se genera una excepción TSS inválido, tipo 0x0A.