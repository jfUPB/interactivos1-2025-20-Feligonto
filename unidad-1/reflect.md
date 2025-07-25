# Unidad 1

## 🤔 Fase: Reflect

### Actividad 07
#### **Parte 1: recuperación de conocimiento (Retrieval Practice)**

- **Basándote en los ejemplos que vimos de sistemas físicos interactivos al iniciar el curso, describe las tres características que definen a un sistema físico interactivo**

1. Aplicación de metodos automatizados o semi automatizados, donde el diseñador utiliza herramientas digitales que pueden instruir con un programa para crear algo.
2. Busca o logra de alguna forma involucrarse en nuestra realidad, por esto mismo se relaciona regularmente con los sentidos y las emociones.
3. Pueden ser de caracter artístico o de diseño, donde la principal diferencia son los parametros que el diseñador tiene en cuenta a la hora de crear, regularmente implicadas en identidad de marca o producto.

- **Explica el modelo input-process-output de Patrick Hübner usando como ejemplo el sistema que construiste con micro:bit y p5.js en la Actividad 06. ¿Cuál fue el input, cuál fue el proceso y cuál fue el output?**

Se identifican y aplican los inputs en la instrucción, los cuales permiten generar y reescribir datos que pueden ser leídos inmediatamente o ser enviados a un sistema alterno. el sistema es capaz, a partir de estos datos y lo que indica la instrucción, de generar un output de caracter sensorial o en forma de información transferida via Serial.

Según el sistema construido en la Actividad 06, los inputs son los botónes A y B del *micro:bit*, y la información que cruza a travez del puerto serial; el proceso es la reescritura de los datos y la comunicación serial; el output son la información enviada por *micro:bit* y los cambios visuales generados por el display de *p5.js*.

- **¿Cuál es la función de la línea uart.write('A') en el código del micro:bit y qué función en p5.js se encarga de “escuchar” ese mensaje?**

`uart.write('A')` reescribe los datos enviados por *micro:bit*, y la función port.read(1) los lee para ser reescritos en la comunicación serial.

- **¿Cuál es la diferencia fundamental entre el arte/diseño tradicional y el arte/diseño generativo?**

La diferencia fundamental es el uso de sistemas y programas como herramienta de diseño.

- **Imagina que quieres que un círculo en p5.js cambie a un color aleatorio cada vez que sacudes el micro:bit. Describe qué tendrías que programar en el micro:bit y qué tendrías que programar en p5.js para lograrlo**



#### **Parte 2: reflexión sobre tu proceso (Metacognición)**

- **¿Qué fue más desafiante para ti en esta unidad: la parte conceptual (entender qué es un sistema físico interactivo) o la parte técnica (hacer que el micro:bit y p5.js se comunicaran)? ¿Por qué?**
- **Describe el momento “¡Aha!” que tuviste cuando lograste que una acción en el micro:bit (presionar un botón, sacudirlo) tuviera un efecto visible en el canvas de p5.js por primera vez. ¿Qué fue lo que entendiste en ese instante?**
- **Al inicio de la unidad te pregunté: “¿Este curso para qué me sirve?”. Después de experimentar y construir tu primer prototipo, ¿Cómo ha cambiado o se ha vuelto más concreta tu respuesta a esa pregunta?**
- **El tutorial de la Actividad 05 te llevó paso a paso. ¿Cómo te sentiste con ese método de aprendizaje? ¿Te dio seguridad o preferirías haberlo intentado por tu cuenta desde el principio?**

### Actividad 08
### Actividad 09
