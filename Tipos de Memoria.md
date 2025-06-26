#Organización-y-Arquitectura-de-Computadores #apunte

---
## Dirección Física
Es la dirección real, la que sale afuera del procesador mediante el bus de address y a la que se quiere acceder en la memoria RAM.
## Dirección Lógica
Es a la que hacemos referencia al programar. Al iniciar un programa se le asigna una porción de memoria (segmentos o páginas) a la que tiene acceso, y las direcciones deben ser convertidas por el procesador a memoria física antes de poder acceder a ellas. El [[MMU]] es el encargado de mapear la dirección física con la lógica.
## Dirección Virtual / Lineal
El espacio lineal de direcciones es al igual que el espacio físico. Un rango de direcciones contiguas plano, que inicia en la dirección 0, y llega al máximo valor que puede ocupar un segmento de acuerdo al modo de trabajo. 
Por ejemplo en [[Modo Real y Modo Protegido|modo protegido]] de 32 bits el rango de direcciones lineales arranca en 0 y puede llegar hasta $2^{32} − 1$, es decir `0xFFFFFFFF`. En el modo 64 bits la dirección lógica resultante es de 64 bits y debe estar representada en formato canónico.