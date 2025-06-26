#Organización-y-Arquitectura-de-Computadores #apunte

---
## Modo Real
Es el modo en el que arrancan todos los procesadores x86 luego de un power-up o reset.
- Trabaja en 16 bits.
- Podemos direccionar hasta 1MB de memoria.
- Modos de direccionamiento limitados.
- No hay protecciones ni niveles de privilegios, por lo tanto tenemos acceso a todas las instrucciones.
### Direcciones en Modo Real
![[image-75.webp|384x156]]
- **Dirección base**: valor de un registro de segmento (`CS, DS, ES, SS`) shifteado 4 bits a la izquierda.
- **Offset**: valor de un registro (`AX, BX, CX, DX, SP, BP, SI, DI`)
## Modo Protegido
Es el modo nativo del procesador.
- Trabaja en 32/64 bits.
- Podemos direccionar hasta 4GB de memoria.
- Hay protecciones y 4 niveles de privilegios.
- Tenemos acceso al set de instrucciones que nos permita nuestro nivel de privilegio
- Rutinas de atención a [[interrupciones]] con niveles de privilegio.