# Unidad 3

## 🛠 Fase: Apply

### Actividad 6

```python
let bomb;

class Bomb {
  constructor() {
    this.password = ['A', 'B', 'A'];
    this.key = [];
    this.count = 20;
    this.startTime = millis();
    this.state = 'CONFIG';
  }

  update() {
    background(0);
    fill(255);
    textSize(50);
    textAlign(CENTER, CENTER);

    if (this.state === 'ARMED') {
      if (millis() - this.startTime > 1000) {
        this.startTime = millis();
        this.count--;
      }
      if (this.count <= 0) {
        this.state = 'EXPLODED';
      }
    }

    if (this.state === 'EXPLODED') {
      fill(255, 0, 0);
      text("💀", width / 2, height / 2);
    } else {
      text(this.count, width / 2, height / 2);
    }
  }

  press(k) {
    if (this.state === 'CONFIG') {
      if (k === 'A') this.count = min(this.count + 1, 60);
      if (k === 'B') this.count = max(this.count - 1, 10);
      if (k === 'S') {
        this.startTime = millis();
        this.state = 'ARMED';
      }
    } else if (this.state === 'ARMED') {
      if (k === 'A' || k === 'B') {
        this.key.push(k);
        if (this.key.length === this.password.length) {
          if (this.key.join('') === this.password.join('')) {
            this.count = 20;
            this.key = [];
            this.state = 'CONFIG';
          } else {
            this.key = [];
          }
        }
      }
    } else if (this.state === 'EXPLODED') {
      if (k === 'L') {
        this.count = 20;
        this.state = 'CONFIG';
      }
    }
  }
}

function setup() {
  createCanvas(400, 300);
  bomb = new Bomb();

  // Botones
  createButton('A').position(50, 260).mousePressed(() => bomb.press('A'));
  createButton('B').position(100, 260).mousePressed(() => bomb.press('B'));
  createButton('S').position(150, 260).mousePressed(() => bomb.press('S'));
  createButton('L').position(200, 260).mousePressed(() => bomb.press('L'));
}

function draw() {
  bomb.update();
}
```

### Actividad 7

**p5.js**

```js
let bomb;
let serial;

class Bomb {
  constructor() {
    this.password = ['A', 'B', 'A'];
    this.key = [];
    this.count = 20;
    this.startTime = millis();
    this.state = 'CONFIG';
  }

  update() {
    background(0);
    fill(255);
    textSize(50);
    textAlign(CENTER, CENTER);

    if (this.state === 'ARMED') {
      if (millis() - this.startTime > 1000) {
        this.startTime = millis();
        this.count--;
      }
      if (this.count <= 0) {
        this.state = 'EXPLODED';
      }
    }

    if (this.state === 'EXPLODED') {
      fill(255, 0, 0);
      text("💀", width / 2, height / 2);
    } else {
      text(this.count, width / 2, height / 2);
    }
  }

  press(k) {
    if (this.state === 'CONFIG') {
      if (k === 'A') this.count = min(this.count + 1, 60);
      if (k === 'B') this.count = max(this.count - 1, 10);
      if (k === 'S') {
        this.startTime = millis();
        this.state = 'ARMED';
      }
    } else if (this.state === 'ARMED') {
      if (k === 'A' || k === 'B') {
        this.key.push(k);
        if (this.key.length === this.password.length) {
          if (this.key.join('') === this.password.join('')) {
            this.count = 20;
            this.key = [];
            this.state = 'CONFIG';
          } else {
            this.key = [];
          }
        }
      }
    } else if (this.state === 'EXPLODED') {
      if (k === 'L') {
        this.count = 20;
        this.state = 'CONFIG';
      }
    }
  }
}

function setup() {
  createCanvas(400, 300);
  bomb = new Bomb();

  // Botones en pantalla
  createButton('A').position(50, 260).mousePressed(() => bomb.press('A'));
  createButton('B').position(100, 260).mousePressed(() => bomb.press('B'));
  createButton('S').position(150, 260).mousePressed(() => bomb.press('S'));
  createButton('L').position(200, 260).mousePressed(() => bomb.press('L'));

  // Serial
  serial = new p5.WebSerial();
  serial.getPorts();
  serial.on("noport", () => serial.requestPort());
  serial.on("portavailable", () => serial.open());
  serial.on("data", serialEvent);
}

function draw() {
  bomb.update();
}

function serialEvent() {
  let inString = serial.readLine().trim();
  if (inString.length > 0) {
    bomb.press(inString);
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
        uart.write("A\n")
    if button_b.was_pressed():
        uart.write("B\n")
    if accelerometer.was_gesture("shake"):
        uart.write("S\n")
    if pin_logo.is_touched():
        uart.write("L\n")
    utime.sleep_ms(100)

```
