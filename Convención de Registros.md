#Organización-y-Arquitectura-de-Computadores #apunte

---
 Se distinguen dos tipos de registros a la hora de hacer una [[Convención de Llamada|llamada]]
 - _**no volátiles**_ o _**callee-saved**_: la función llamada debe encargarse de que, al terminar, estos registros tengan los mismos valores que al comienzo. Estos son:
    - En 64 bits: `RBX, RBP, R12, R13, R14, R15`
    - En 32 bits: `EBX, EBP, ESI, EDI`
- _**volátiles**_ o _**caller-saved**_ : la función llamada no tiene obligación de conservarlos. Si se desean preservar, será responsabilidad de la función llamadora
### Menciones Especiales

- `RSP/ESP` se puede considerar como un registro no volátil dada su función específica como tope de la pila.
- `R10 y R11` no están ni en el grupo de los registros por los que se pasan parámetros ni en el grupo de los registros no volátiles.
- `XMM8 a XMM15` no están en el grupo de los registros por los que se pasan parámetros pero están a disposición.