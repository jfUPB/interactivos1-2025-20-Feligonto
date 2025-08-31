# Evidencias de la unidad 4

## Código

[Enlace a la aplicación a modifica](https://editor.p5js.org/generative-design/sketches/M_1_5_02)

Código a modificar:

``` js

'use strict';

var sketch = function(p) {
  var agents = [];
  var agentCount = 4000;
  var noiseScale = 300;
  var noiseStrength = 10;
  var overlayAlpha = 10;
  var agentAlpha = 90;
  var strokeWidth = 0.3;
  var drawMode = 1;

  p.setup = function() {
    p.createCanvas(p.windowWidth, p.windowHeight);

    for (var i = 0; i < agentCount; i++) {
      agents[i] = new Agent();
    }
  };

  p.draw = function() {
    p.fill(255, overlayAlpha);
    p.noStroke();
    p.rect(0, 0, p.width, p.height);

    // Draw agents
    p.stroke(0, agentAlpha);
    for (var i = 0; i < agentCount; i++) {
      if (drawMode == 1) agents[i].update1(noiseScale, noiseStrength, strokeWidth);
      else agents[i].update2(noiseScale, noiseStrength, strokeWidth);
    }
  };

  p.keyReleased = function() {
    if (p.key == 's' || p.key == 'S') p.saveCanvas(gd.timestamp(), 'png');
    if (p.key == '1') drawMode = 1;
    if (p.key == '2') drawMode = 2;
    if (p.key == ' ') {
      var newNoiseSeed = p.floor(p.random(10000));
      p.noiseSeed(newNoiseSeed);
    }
    if (p.keyCode == p.DELETE || p.keyCode == p.BACKSPACE) p.background(255);
  };
};

var myp5 = new p5(sketch);
```

[Enlace a la aplicación modificada](URL)

Código modificado:

``` js
'use strict';

var sketch = function(p) {
  var agents = [];
  var agentCount = 4000;
  var noiseScale = 300;
  var noiseStrength = 10;
  var overlayAlpha = 10;
  var agentAlpha = 90;
  var strokeWidth = 0.3;
  var drawMode = 1;

  // Variables for serial communication
  let port;
  let connectBtn;
  let microBitX = 0;
  let microBitY = 0;
  let microBitAState = false;
  let microBitBState = false;
  let prevMicroBitX = 0;
  let prevMicroBitAState = false;
  let prevMicroBitBState = false;

  p.setup = function() {
    p.createCanvas(p.windowWidth, p.windowHeight);

    for (var i = 0; i < agentCount; i++) {
      agents[i] = new Agent();
    }
    
    // Initialize serial communication and button
    port = p.createSerial();
    connectBtn = p.createButton("Conectar micro:bit");
    connectBtn.position(p.width - 160, 20); // Posiciona el botón en la esquina superior derecha
    connectBtn.mousePressed(connectBtnClick);
  };

  p.draw = function() {
    p.fill(255, overlayAlpha);
    p.noStroke();
    p.rect(0, 0, p.width, p.height);

    // Read and process data from micro:bit
    if (port.availableBytes() > 0) {
      let data = port.readUntil("\n");
      if (data) {
        data = data.trim();
        let values = data.split(",");
        if (values.length === 4) {
          microBitX = p.int(values[0]);
          microBitY = p.int(values[1]);
          microBitAState = values[2].toLowerCase() === "true";
          microBitBState = values[3].toLowerCase() === "true";
        }
      }
    }

    // Update controls with micro:bit data
    updateMicrobitControls();

    // Draw agents
    p.stroke(0, agentAlpha);
    for (var i = 0; i < agentCount; i++) {
      if (drawMode == 1) agents[i].update1(noiseScale, noiseStrength, strokeWidth);
      else agents[i].update2(noiseScale, noiseStrength, strokeWidth);
    }
    
    // Update previous states for next iteration
    prevMicroBitX = microBitX;
    prevMicroBitAState = microBitAState;
    prevMicroBitBState = microBitBState;
    
    if (!port.opened()) {
      connectBtn.html("Conectar micro:bit");
    } else {
      connectBtn.html("Desconectar");
    }
  };

  // Function to handle micro:bit events
  function updateMicrobitControls() {
      // Control drawMode with accelerometer X
      if (microBitX >= 500 && prevMicroBitX < 500) {
        drawMode = 1;
        p.background(255);
      } else if (microBitX <= -500 && prevMicroBitX > -500) {
        drawMode = 2;
        p.background(255);
      }

      // Control new noise seed with button A
      if (microBitAState === true && prevMicroBitAState === false) {
        p.noiseSeed(p.floor(p.random(10000)));
      }

      // Control screen clear with button B
      if (microBitBState === true && prevMicroBitBState === false) {
        p.background(255);
      }
  }

  // Function to handle the click on the connect button
  function connectBtnClick() {
    if (!port.opened()) {
      port.open("MicroPython", 115200);
    } else {
      port.close();
    }
  }

};
var myp5 = new p5(sketch);
```

## Video

[Video demostratativo](URL)


