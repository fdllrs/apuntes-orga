#Organización-y-Arquitectura-de-Computadores #apunte

---
- `b <nombre del archivo>:<línea>`
  pone un breakpoint en ese archivo en la línea indicada. Se puede usar en archivos que no son el main, por ejemplo si el main llama a una función que está en otro archivo (incluso en otro lenguaje).
- `backtrace`
  permite ver el stack de llamados hasta el momento actual de la ejecución del programa
- `frame`
  permite cambiar a otro stack frame.
- `run <args>`
  iniciará la ejecución del binario cargado con los argumentos que queramos.
- `p {<tipo>} <dirección>`
  imprime una dirección de memoria interpretada como el tipo de datos que le digamos.
  Por ejemplo `p {char*} 0x00404900` imprime lo que haya en esa dirección interpretándolo como un puntero a un `char`.
- `p {<tipo>} <dirección>@<cantidad>`
  igual que el anterior, solo que le especificamos cuántos elementos queremos imprimir a partir de la dirección que le pasamos. 