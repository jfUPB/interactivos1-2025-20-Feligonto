
# Evidencias de la unidad 5

### Actividad 1

- Describe cómo se están comunicando el micro:bit y el sketch de p5.js. ¿Qué datos envía el micro:bit?

Se comunican por puerto serial con un baudrate de 115200. El micro:bit envía continuamente por UART los datos del acelerómetro (x, y) y el estado de los botones A y B. p5.js usa la librería p5.webserial para leer esos datos.

Los datos que se envian son 4  valores diferentes referentes al xValue, yValue, aState y bState, los cuales contienen posición del acelerómetro y si los botones están o no presionados.

- ¿Cómo es la estructura del protocolo ASCII usado?

`<entero>,<entero>,<booleano>,<booleano>\n`

Valores separados por coma y cada paquete termina con salto de línea \n lo que le permite a p5.js saber dónde acaba un dato y empieza el siguiente.

- Muestra y explica la parte del código de p5.js donde lee los datos del micro:bit y los transforma en coordenadas de la pantalla.

```
if (port.availableBytes() > 0) {
  let data = port.readUntil("\n");   // lee hasta salto de línea
  if (data) {
    data = data.trim();
    let values = data.split(",");   // divide en 4 valores
    if (values.length == 4) {
      microBitX = int(values[0]) + windowWidth / 2;
      microBitY = int(values[1]) + windowHeight / 2;
      microBitAState = values[2].toLowerCase() === "true";
      microBitBState = values[3].toLowerCase() === "true";
      updateButtonStates(microBitAState, microBitBState);
    }
  }
}
```
Lee una línea de texto, divide en 4 valores (split(",")), convierte x e y en coordenadas de pantalla (desplazados al centro con + windowWidth/2 y + windowHeight/2) y convierte los valores "true"/"false" en booleanos.

- ¿Cómo se generan los eventos A pressed y B released que se generan en p5.js a partir de los datos que envía el micro:bit?

Comparamos con los anteriores estados, entonces, si A pasa de false → true se dispara “A pressed” y si B pasa de true → false se dispara “B released”.

- Capturas de pantalla de los algunos dibujos que hayas hecho con el sketch.

<img width="2556" height="1238" alt="20251001_143813" src="https://github.com/user-attachments/assets/f50ad2a4-9131-451e-b753-88df0c9237bb" />

<img width="2556" height="1238" alt="20251001_144219" src="https://github.com/user-attachments/assets/34e8a44d-c42c-4acb-8361-7a6096f8690e" />

<img width="2556" height="1238" alt="20251001_144423" src="https://github.com/user-attachments/assets/c773d393-d1f4-420b-99c0-bfdf7221a713" />

- ¿Por que se usan las funciones push y pop?

En p5.js, push() guarda el estado actual de dibujo (traslaciones, rotaciones, colores, etc.).

pop() restaura ese estado al que estaba antes.

Esto se hace para que las transformaciones aplicadas a un objeto (por ejemplo translate() o rotate()) no afecten a los siguientes elementos que se dibujen.

- ¿En que consiste la funcion nf?

nf() significa “number format”, convierte un número en una cadena de texto con formato fijo, útil para ceros a la izquierda o número de decimales.

- ¿Que devuelve y para que sirve la funcion year?

year() devuelve el año actual del sistema (en número entero), sirve para crear timestamps, fechas, o usar la fecha en tus programas.

---

**Struct Pack**

- ¿Por qué se ve este resultado?

<img width="1197" height="203" alt="Captura de pantalla 2025-10-01 144751" src="https://github.com/user-attachments/assets/075f37b3-2cd0-466f-aea4-e2df7d32419f" />

ya no se mandan caracteres, sino los valores codificados en bytes fijos.

-  Lo que ves ¿Cómo está relacionado con esta línea de código? `data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))`

<img width="1199" height="204" alt="Captura de pantalla 2025-10-01 145051" src="https://github.com/user-attachments/assets/234d0acb-da88-4a0a-b1cc-583607496e4c" />

se observa de forma mas clara cada uno de los bytes que se envian, se deduce que los 2 primeros bytes son el valor de xValue, los proximos 2 son el valor de yValue, el siguiente es el de aState y el último el de bState.

- ¿Qué ventajas y desventajas ves en usar un formato binario en lugar de texto en ASCII?

ASCII > fácil de leer y depurar, pero más lento y pesado.

Binario > eficiente y preciso, pero ilegible para la vista humana y menos flexible.

- ¿Cuántos bytes se están enviando por mensaje?

6 bytes por paquete.

- ¿Cómo se relaciona esto con el formato '>2h2B'?

2h > (2 de xValue, 2 de yValue), 2B > (1 de aState y 1 de bState). 

- ¿Qué significa cada uno de los bytes que se envían?

**Primeros 2 bytes → xValue (h)**

Tipo: entero corto con signo (16 bits).

Representa la aceleración en el eje X del micro:bit.

**Siguientes 2 bytes → yValue (h)**

Tipo: entero corto con signo (16 bits).

Representa la aceleración en el eje Y.

**Quinto byte → aState (B)**

Tipo: entero sin signo de 1 byte.

Representa el estado del botón A.

**Sexto byte → bState (B)**

Tipo: entero sin signo de 1 byte.

Representa el estado del botón B.

- ¿Cómo se verían esos números en el formato '>2h2B'?

En hexagecimal un valor positivo se escribe como 00 o 0000 0000 en binario, y el valor negativo FF en HEX y 1111 1111 en binario.

**ASCII y STRUCT**

<img width="220" height="207" alt="image" src="https://github.com/user-attachments/assets/b03c4eb8-2e1d-4aa7-a671-d5e8218c8caf" />

- ¿Qué diferencias ves entre los datos en ASCII y en binario?



- ¿Qué ventajas y desventajas ves en usar un formato binario en lugar de texto en ASCII?



- ¿Qué ventajas y desventajas ves en usar un formato ASCII en lugar de binario?



- Explica por qué en la unidad anterior teníamos que enviar la información delimitada y además marcada con un salto de línea y ahora no es necesario.



- Explica por qué en la unidad anterior teníamos que enviar la información delimitada y además marcada con un salto de línea y ahora no es necesario.

