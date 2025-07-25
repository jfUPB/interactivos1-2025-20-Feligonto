# Unidad 1

## 🤔 Fase: Reflect

### Actividad 07
**Parte 1: recuperación de conocimiento (Retrieval Practice)**

- **Basándote en los ejemplos que vimos de sistemas físicos interactivos al iniciar el curso, describe las tres características que definen a un sistema físico interactivo**

1. Aplicación de metodos automatizados o semi automatizados, donde el diseñador utiliza herramientas digitales que pueden instruir con un programa para crear algo.
2. Busca o logra de alguna forma involucrarse en nuestra realidad, por esto mismo se relaciona regularmente con los sentidos y las emociones.
3. Pueden ser de caracter artístico o de diseño, donde la principal diferencia son los parametros que el diseñador tiene en cuenta a la hora de crear, regularmente implicadas en identidad de marca o producto.

- **Explica el modelo input-process-output de Patrick Hübner usando como ejemplo el sistema que construiste con micro:bit y p5.js en la Actividad 06. ¿Cuál fue el input, cuál fue el proceso y cuál fue el output?**

Se identifican y aplican los inputs en la instrucción, los cuales permiten generar y reescribir datos que pueden ser leídos inmediatamente o ser enviados a un sistema alterno. el sistema es capaz, a partir de estos datos y lo que indica la instrucción, de generar un output de caracter sensorial o en forma de información transferida via Serial.

Según el sistema construido en la Actividad 06, los inputs son los botónes A y B del *micro:bit*, y la información que cruza a travez del puerto serial; el proceso es la reescritura de los datos y la comunicación serial; el output son la información enviada por *micro:bit* y los cambios visuales generados por el display de *p5.js*.

- **¿Cuál es la función de la línea uart.write('A') en el código del micro:bit y qué función en p5.js se encarga de “escuchar” ese mensaje?**

`uart.write('A')` reescribe los datos enviados por *micro:bit*, y la función `port.read(1)` los lee para ser reescritos en la comunicación serial.

- **¿Cuál es la diferencia fundamental entre el arte/diseño tradicional y el arte/diseño generativo?**

La diferencia fundamental es el uso de sistemas y programas como herramienta de diseño.

- **Imagina que quieres que un círculo en p5.js cambie a un color aleatorio cada vez que sacudes el micro:bit. Describe qué tendrías que programar en el micro:bit y qué tendrías que programar en p5.js para lograrlo**

En *micro:bit* tendría que crear una variable que permita el acceso al acelerómetro, luego un condicional que lea su retorno y reescriba los datos, concicionando con la función `is.pressed()`, a la letra C, y N en su defecto. Luego en p5.js, declaramos las variables globales `coloractual`, `color` y `port`. En la funcion `setup()` creamos nuestro canvas e inicializamos la variable `port` almacenandole un serial con `createSerial()`. En la función `draw()`, Inicializamos color almacenandole la función `fill()` y añadiendole la función `random(225)` en sus 3 componentes, condicionamos el cambio de color dependiendo del dato que reciba por el puerto serial, en este caso si recibe C accederá a hacer el cambio de color pero si recibe N permanecerá en el último color (coloractual), creamos el círculo con su respectiva posición y almacenamos color en color actual.

**Parte 2: reflexión sobre tu proceso (Metacognición)**

- **¿Qué fue más desafiante para ti en esta unidad: la parte conceptual (entender qué es un sistema físico interactivo) o la parte técnica (hacer que el micro:bit y p5.js se comunicaran)? ¿Por qué?**



- **Describe el momento “¡Aha!” que tuviste cuando lograste que una acción en el micro:bit (presionar un botón, sacudirlo) tuviera un efecto visible en el canvas de p5.js por primera vez. ¿Qué fue lo que entendiste en ese instante?**
- **Al inicio de la unidad te pregunté: “¿Este curso para qué me sirve?”. Después de experimentar y construir tu primer prototipo, ¿Cómo ha cambiado o se ha vuelto más concreta tu respuesta a esa pregunta?**
- **El tutorial de la Actividad 05 te llevó paso a paso. ¿Cómo te sentiste con ese método de aprendizaje? ¿Te dio seguridad o preferirías haberlo intentado por tu cuenta desde el principio?**

### Actividad 08

- **Tu compañero resolvió el problema de manera diferente a ti, qué hizo diferente, qué aprendiste de su solución. En tu bitácora documenta lo anterior y escribe, como si le estuvieras explicando, lo que tú hiciste y por qué es diferente a lo que hizo tu compañero.**

Mi compañero Daniel Mendoza, uso las palabra reservada ***width*** para la ubicación de su objeto, un elipse (`ellipse()`) al que le asignó además un color con el formato rgb. Me recordó la reescritura que usó a la hora de enviar datos que no importa que datos especificos se envíen siempre y cuando coicidan. Usó la función constrain para delimitar el movimiento del elipse lo cual me parece  muy adecuado a la hora de pensarlo como una experiencia interactiva.

En mi caso uso un vector (`createVector()`) para ajustar la posición de mi círculo (`circle()`) donde determino su posición en x con una variable `posactual`, su posición en y con un valor literal (200) que es la mitad exacta del ancho total y su tamaño con valor 25. Usé en la reescritura de datos los nombres literales de los botónes. No hice una delimitación alguna, respecto al movimiento.

### Actividad 09

