#Organización-y-Arquitectura-de-Computadores #apunte

---
> Es la posición en la pila que le corresponde a una función durante su ejecución. Está delimitado por el puntero que se encuentra en el registro `RBP` (base pointer), y el registro `RSP` (stack pointer o tope de la pila).

## Instrucciones Para Manipular la Pila
- `PUSH` decrementa el valor de `RSP` (le resta 8 al puntero, recordemos que la pila "crece hacia abajo", o sea, crece hacia direcciones más chicas) y luego escribe sobre la posición a la que apunta.
- `POP` primero lee el valor almacenado en la posición apuntada por el `RSP` y luego incrementa su valor (le suma 8 al puntero).
- `CALL` realiza un `PUSH` de la dirección de retorno y luego escribe la dirección de salto al `RIP`.
- `RET` realiza un `POP` de la dirección de retorno y la escribe en el `RIP`.

Hay que tener en cuenta que, la convención de llamada dice que debemos llamar a las funciones de C con la pila alineada a 16, y la instrucción `CALL` hace un `PUSH` de la dirección de retorno, entonces cuando la función ejecute su primer instrucción la pila estará **desalineada**. Para alinearla y mantener en condiciones el stack frame, utilizamos dos secciones de código en toda función (que queremos que interactúe con C) denominadas prólogo y epílogo.
![[image-7.webp]]
- **Prólogo**: esta es la sección en la que alineamos la pila a la vez que establecemos la base para el stack frame. Pusheamos el valor actual de `RBP` que contiene el puntero a la base del stack frame de la función llamadora, y luego asignamos el valor del `RSP` actual al `RBP` como nueva base. En esta sección también estarían los push de los registros no volátiles que vamos a usar, para así preservar los valores que corresponden a la función llamadora.
- **Epílogo**: acá restauramos los valores de los registros no volátiles que preservamos en el prólogo, popeandolos en el orden contrario al que los pusheamos. Esto incluye al `RBP`, el primer registro no volátil pusheado, que recupera el valor de la base del stack de la función anterior y deja al `RSP` apuntando al mismo lugar al que apuntaba al ejecutarse la primer instrucción. Dadas esas condiciones, el `RET` puede recuperar correctamente el valor de retorno, y la función llamadora retomar su ejecución.

Es responsabilidad de la **función llamada** dejar todo como estaba, es decir, restaurar los registros y el stack frame antes de retornar.