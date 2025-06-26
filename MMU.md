#Organización-y-Arquitectura-de-Computadores #apunte

---
Encargado de realizar la conversión entre la dirección de memoria física y la lógica en un procesador.
Le permite al sistema operativo asignar el mismo espacio lógico a distintos procesos, sin importarle la memoria física
![[image-63.webp]]
Proceso 1, Proceso 2 y Proceso 3 ocupan el mismo espacio de memoria lógica (tienen las mismas direcciones en sus respectivos programas) pero al traducirlo a memoria física se ve que están en distintos lugares.
## Funcionamiento
- Cada vez que el [[Scheduler de Tareas]] retoma la ejecución de una tarea, el MMU mapea su espacio de direccionamiento lógico en un área de memoria exclusiva para la tarea. De este modo conseguimos separar y proteger a todas las tareas entre si.
- La desventaja es el overhead que genera hacer el mapeo cada vez que se conmuta una tarea.
- La MMU asegura a cada tarea (proceso), la visibilidad de su propia area de memoria, y de las partes relevantes del sistema ´ operativo.
### Traducción
La MMU está compuesta de dos subunidades conectadas en cascada: 
- **La Unidad de Segmentación** que se encarga de trasladar la dirección lógica en una dirección lineal
- **La Unidad de Paginación** que traduce la dirección lineal en una dirección física que enviará por el bus de address hacia la memoria externa.
