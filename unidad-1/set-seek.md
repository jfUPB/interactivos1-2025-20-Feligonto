# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 1

- **¿Qué es un sistema físico interactivo?**

Es un sistema diseñado con la intención de generar una experiencia que logre intervenir, de forma fisica o virtual, con nuestra realidad. Generalmente interviniendo también con los sentidos humanos.

- **¿Cómo podrías aplicar lo que has visto en tu perfil profesional?**

Realizar experiencias enfocadas a la atracción sensorial y emocional de posibles usuarios, quienes puedan además interesarse en una marca o producto.

### Actividad 2

- **¿Qué es el diseño/arte generativo?**

Es la capacidad de comunicación y expresión a partir de sistemas varios, donde un artista plasma una obra usando una o varias herramientas de forma ordenada, mediante la práctica del uso de sistemas autonomos o semi-autonomos.

Diferenciando entre diseño y arte generativo, En el arte generativo se establecen un conjunto de reglas propias donde pueden haber variaciones que logran diversificar un producto sin limitaciones explicitas, solo la creatividad y la imaginación.

Mientras que en el diseño generativo, aqui existe un establecimiento de reglas partiendo de la necesidad corporativa de representar algo más que una figura, una experiencia. Se busca representar una marca u organización, las cuales tienen establecidos parametros a seguir que el diseñador tendrá en cuenta durante su proceso creativo. Estas limitaciones pueden ser en cuanto a sus herramientas, principios o patrones de diseño, como colores o figuras.

- **¿Cómo podrías aplicar lo que has visto en tu perfil profesional?**

Enfocando mis experiencias a funcionar de forma sistémica y más automátizada, esto para lograr una representación mas precisa de mis ideas y además agilizando el proceso creativo.

## Actividad 3

- **En este sistemas físico interactivo identifica los inputs, outputs y el proceso.**

### Microbit

**Inputs**

- **Botón A**, Input de presion fisica con nombre A. 
- **Botón B**, Input de presion fisica con nombre B.
- **Acelerometro**, Input detector de vibración fisica.
- **Comunicación Serial**, Recibimiento de información a traves del puerto Serial.

**Outputs**

- **Comunicación Serial**, Transferencia de información a traves del puerto Serial.
- **Display**, Pequeña pantalla compuesta con varias luces programables.

### PC

**Inputs**

- **Botón Send Love**, botón virtual de la interfaz de *p5.js*.
- **Comunicación Serial**, Recibimiento de información a traves del puerto Serial.

**Outputs**

- **Comunicación Serial**, Transferencia de información a traves del puerto Serial.

### Proceso

Se tienen dos sistemas independientes conectados por un cable de datos, al ser independientes se les atribuye sus propias caracteristicas de procesamiento y de accesibilidad.

Despues de conectar el dispositivo Microbit a p5.js via web, Se puede interactuar desde ambos dispositivos por medio de los inputs físicos (botónes), estos generaran una reescritura de las instrucciones (código) para generar un output en forma de información **serial** a través del puerto. El otro dispositivo recibe el **serial** como input para generar un output sensorial.

## Actividad 4

- **Escribe el enlace a tu programa en el editor de p5.js.**

[Trabajo actual (continuar)](https://editor.p5js.org/Feligonto/sketches/jGJ-OdAdi)

- **Copia el código de tu programa en la bitácora (recuerda insertarlo usando markdown y el lenguaje javascript).**

```js
let posSq;
let velSq;

function setup() {
  createCanvas(300,400);
  // console.log("Hola");
  posSq = createVector(147, 350);
  velSq = createVector(0, -20);
}

let t1=0;
let t2=0;
let pos2x;

function draw() {
  background(120,120);
  
  let pos1 = createVector(100*sin(t1/20)+175, 0);
  let pos2 = createVector(100*sin(t1/20)+147, 0);
  let pos3 = createVector(100*sin(t1/20)+119, 0);
  
  posSq.add(velSq);
  
  if (posSq.y < 0) {
    posSq.y= 350;
    posSq.x= pos2.x
  }
  
  triangle(pos1.x, 375, pos2.x, 350, pos3.x, 375);
  square(posSq.x,posSq.y,10)
  t1++;
  t2++;
}
```

- **Muestra una captura de pantalla del resultado de tu programa.**

<img width="472" height="641" alt="image" src="https://github.com/user-attachments/assets/cceca9e3-028e-431d-8059-1ff6e305df78" />

<img width="476" height="637" alt="image" src="https://github.com/user-attachments/assets/73624f9e-ee63-4076-a3a3-8e3252e32600" />
