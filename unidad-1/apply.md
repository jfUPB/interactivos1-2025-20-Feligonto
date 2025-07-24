# Unidad 1

## 🛠 Fase: Apply

### Actividad 5

El sistema tiene integrado en el programa del micro:bit los botónes que actuan como input principal del microbit el cual genera un output serial que genera un input para el editor el cual actua segun la información que recibe. De esta forma es que al validar que un botón fue presionado, por medio de la función is_pressed y el registro serial (baudrate), el display del editor es capaz de cambiar de color en función del dato recibido en tiempo real.

### Actividad 6



```js
let port;
    let connectBtn;
    let posactual;
    let posc;

    function setup() {
        posactual = 200
        createCanvas(400, 400);
        background(220);
        port = createSerial();
        connectBtn = createButton("Connect to micro:bit");
        connectBtn.position(80, 300);
        connectBtn.mousePressed(connectBtnClick);
    }

    function draw() {
        background(220);
      
        posc = createVector(posactual,200);
      
        if (port.availableBytes() > 0) {
            let dataRx = port.read(1);
            if (dataRx=="A") {
              // ingrese movimiento izquierda
              posc.x--;
              posactual=posc.x
            }else{
              if (dataRx=="B"){
                // ingrese movimiento derecha
                posc.x++;
                posactual=posc.x
              }
            }
          }
      
        circle(posc.x,posc.y,25);

        if (!port.opened()) {
            connectBtn.html("Connect to micro:bit");
        } else {
            connectBtn.html("Disconnect");
        }
    }

    function connectBtnClick() {
        if (!port.opened()) {
            port.open("MicroPython", 115200);
        } else {
            port.close();
        }
    }
```

