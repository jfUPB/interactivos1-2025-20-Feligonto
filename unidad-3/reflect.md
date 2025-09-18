# Unidad 3

## 🤔 Fase: Reflect

### Actividad 5

**Construye el modelo de la bomba 3.0. Como ya tienes el código puedes tener un modelo muy preciso.**

<img width="882" height="744" alt="image" src="https://github.com/user-attachments/assets/3833d041-9021-42b8-b1d2-6a9b66c9e376" />

**Crear una tabla con los vectores de prueba.**

| Estado inicial | Evento disparador     | Acciones                                                                 | Estado final                    |
| -------------- | --------------------- | ------------------------------------------------------------------------ | ------------------------------- |
| CONFIG         | Botón A presionado    | Incrementa `count` (máx. 60) y muestra en display                        | CONFIG                          |
| CONFIG         | Botón B presionado    | Decrementa `count` (mín. 10) y muestra en display                        | CONFIG                          |
| CONFIG         | Acelerómetro: *shake* | Guarda tiempo inicial, pasa a cuenta regresiva                           | ARMED                           |
| ARMED          | Temporizador 1s       | Decrementa `count` y muestra en display                                  | ARMED / EXPLODED (si llega a 0) |
| ARMED          | Botón A presionado    | Guarda "A" en la clave ingresada                                         | ARMED                           |
| ARMED          | Botón B presionado    | Guarda "B" en la clave ingresada                                         | ARMED                           |
| ARMED          | Clave correcta (ABA)  | Reinicia `count = 20`, muestra en display, regresa al modo configuración | CONFIG                          |
| ARMED          | Clave incorrecta      | Reinicia índice de clave                                                 | ARMED                           |
| ARMED          | Temporizador llega a 0           | Muestra `Image.SKULL`                                                    | EXPLODED                        |
| EXPLODED       | Logo tocado           | Reinicia `count = 20`, muestra en display, vuelve a configurar           | CONFIG                          |

### Actividad 7

**p5.js**

```js
let bomb;

class Bomb {
  constructor() {
    this.password = ["A", "B", "A"];
    this.key = [];
    this.count = 20;
    this.startTime = millis();
    this.state = "CONFIG";
  }

  update() {
    background(0);
    fill(255);
    textSize(50);
    textAlign(CENTER, CENTER);

    if (this.state === "ARMED") {
      if (millis() - this.startTime > 1000) {
        this.startTime = millis();
        this.count--;
      }
      if (this.count <= 0) {
        this.state = "EXPLODED";
      }
    }

    if (this.state === "EXPLODED") {
      fill(255, 0, 0);
      text("💀", width / 2, height / 2);
    } else {
      text(this.count, width / 2, height / 2);
    }
  }

  press(k) {
    if (this.state === "CONFIG") {
      if (k === "A") this.count = min(this.count + 1, 60);
      if (k === "B") this.count = max(this.count - 1, 10);
      if (k === "S") {
        this.startTime = millis();
        this.state = "ARMED";
      }
    } else if (this.state === "ARMED") {
      if (k === "A" || k === "B") {
        this.key.push(k);
        if (this.key.length === this.password.length) {
          if (this.key.join("") === this.password.join("")) {
            this.count = 20;
            this.key = [];
            this.state = "CONFIG";
          } else {
            this.key = [];
          }
        }
      }
    } else if (this.state === "EXPLODED") {
      if (k === "L") {
        this.count = 20;
        this.state = "CONFIG";
      }
    }
  }
}

function setup() {
  port = createSerial();
  connectBtn = createButton("Connect to micro:bit");
  connectBtn.position(80, 300);
  connectBtn.mousePressed(connectBtnClick);

  createCanvas(400, 300);
  bomb = new Bomb();

  createButton("A")
    .position(50, 260)
    .mousePressed(() => bomb.press("A"));
  createButton("B")
    .position(100, 260)
    .mousePressed(() => bomb.press("B"));
  createButton("S")
    .position(150, 260)
    .mousePressed(() => bomb.press("S"));
  createButton("L")
    .position(200, 260)
    .mousePressed(() => bomb.press("L"));
}

function draw() {
  if (port.availableBytes() > 0) {
    let dataRx = port.read(1);
    if (dataRx == "A") {
      bomb.press("A")
    } else {
      if (this.dataRx == "B") {
        bomb.press("B")
      } else {
        if (this.dataRx == "S") {
          bomb.press("S")
        } else {
          bomb.press("L")
        }
      }
    }
  }

  if (!port.opened()) {
    connectBtn.html("Connect to micro:bit");
  } else {
    connectBtn.html("Disconnect");
  }

  bomb.update();
}

function connectBtnClick() {
  if (!port.opened()) {
    port.open("MicroPython", 115200);
  } else {
    port.close();
  }
}
```

Link al codigo:
[Actividad 7 - p5.js](https://editor.p5js.org/felipegtupb/sketches/sOc6fRFJP)

**micro:bit**

```python
from microbit import *
import utime

uart.init(baudrate=115200)

while True:
    if button_a.was_pressed():
        uart.write("A")
    if button_b.was_pressed():
        uart.write("B")
    if accelerometer.was_gesture("shake"):
        uart.write("S")
    if pin_logo.is_touched():
        uart.write("L")
    utime.sleep_ms(100)
```
