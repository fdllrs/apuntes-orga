#Organización-y-Arquitectura-de-Computadores #apunte

---
## Estructura de un Programa
 - **Directivas de pre procesador**: instrucciones que va a aplicar el pre procesador antes de pasarle el texto al ensamblador.
 - **Directivas de ensamblador**: instrucciones que le indican al ensamblador como interpretar las siguientes líneas de texto para armar el binario.
 - **Pseudo-instrucciones**: instrucciones que atenderá el ensamblador y no quedarán en el binario resultante que ejecutaría un procesador.
 - **Sección .data**: define los datos del programa.
 - **Sección .text**: define el código del programa.
## Instrucciones
- **Instrucciones sin operandos** (o con operandos implícitos). Ejemplo: `ret` para retornar a la función llamadora.
- **Instrucciones con un operando**: puede ser un registro, una posición de memoria, un inmediato. Ejemplo: `jmp .fin` salta a la etiqueta `.fin`, `call foo` llama a la función foo.
- **Instrucciones con dos operandos**: por lo general el primer operando es el destino `dst`, donde se almacenará el resultado de ejecutar la instrucción, y el segundo operando es la fuente `src`.
    - Los pares `dst-src` pueden ser registro-memoria, registro-registro, memoria-registro, registro-inmediato y memoria-inmediato. 
      No es posible un par memoria-memoria, las lecturas siempre deben acabar en un registro y las escrituras siempre deben partir de uno o bien de un valor inmediato.
    - Al trabajar con lecturas y escrituras de memoria hay que tener en cuenta el tamaño del dato que se está leyendo/escribiendo. Para especificar el tamaño es preciso utilizar el tamaño de registro correcto y añadir el tamaño (BYTE, WORD, DWORD) en el operando de memoria.
    - Ejemplos:
        - **Registro-Registro**:
            - `add rdi, rsi`
        - **Memoria-Registro**:
            - `mov [LABEL], rax`
            - `sub r10, [rdx + 4*rax + OFFSET]` (Direccionamiento visto en la teórica: \[Base + Índice * Escala + Desplazamiento\])
            - `mov ecx, DWORD [rbp - 8]` 
            - `add BYTE [rdi], sil`
        - **Registro-Inmediato**:
            - `sub rsp, 8`
## Pseudo-Instrucciones
```nasm
db 0x55                                 ; define sólo un byte, 0x55
db 0x55,0x56,0x57                       ; 3 bytes sucesivos
db 'a',0x55                             ; 0x97, 0x55
db 'hello',13,10,'$'                    ; strings como cadenas de bytes, cada 
										caracter es un byte
db `hola\nmundo\n\0`                    ; strings con "C-style \-escapes"
dw 0x1234                               ; define una word, los bytes quedan:
										0x34 0x12 (por little-endianness se 
										guarda primero el byte menos 
										significativo)
dd 0x12345678                           ; define una double word, los bytes 
										quedan: 0x78 0x56 0x34 0x12
dq 0x123456789abcdef0                   ; define una quad word,constante de 8 
										bytes
times 4 db 'ja'                         ; define 4 veces la tira de bytes 
										'ja', quedando "jajajaja"
```