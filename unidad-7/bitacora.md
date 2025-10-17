# Evidencias de la unidad 7

### Actividad 1

- ¿Qué URL de Dev Tunnels obtuviste? ¿Por qué crees que necesitamos usar esta URL en lugar de http://localhost:3000 o la IP local de tu computador para que el celular se conecte?

Obtuve `https://9fmv10r5-3000.use2.devtunnels.ms/`, considero que es necesarion usar este medio o puerto puesto que al ser un intercambio de información entre diferentes computadoras inteligentes, estos tienen sistemas mas rigidos de seguridad al momento de recibir esgte tipo de datos, por lo que el puerto permite una via segura y controlada.

- Describe brevemente qué hace `npm install` y `npm start`.

`npm install`se encarga de descargar cierta cantidad de paquetes de librerias o repositorios alternos para que el programa funcione correctamente.

`npm start` activa el servidor de node.js por donde van a pasar los datos del programa y brinda su ubicación URL.

- ¿Qué mensajes observaste en la terminal del servidor al conectar el cliente de escritorio y el cliente móvil? ¿Eran diferentes los mensajes o identificadores?

Aparece el mismo mensaje cada que se accede desde la pagina mobile o desktop, dice `New client connected`, asi mismo cuando se desconectan.

- Describe el comportamiento observado: ¿Funcionó la interacción? ¿Hubo algún retraso (latencia)?

Bastante interesante y divertida la dinamica, da como la sensación de estar controlando algo sin interactuar con ello,  existe una latencia perceptible pero no importante a la hora de detectar el touch y de sus transformaciones en el canvas.

### Actividad 2

- Explica con tus propias palabras: ¿Por qué es necesario Dev Tunnels en este escenario y cómo funciona conceptualmente?

Es necesario puesto que al usar las cuentas y dispositivos de la universidad las medidas de ciberseguridad son mas estrictas. Lo que en este caso se hace es conectar dos clientes (2 dispositivos) a un servidor de Node.js y crear un puente entre los clientes y el servidor, lo cual es seguro pues su deber es evitar el traspaso de información local o personal importante e innecesaria.

- Describe la función de `touchMoved()` y por qué se usa la variable `threshold` en el cliente móvil.

La función `touchMoved()` ejecuta código mientras que se esté presionando la pantalla tactil del dispositivo, y la variable `threshhold` existe para limitar la cantidad de datos que se envian respecto al tiempo, lograndolo puesto que el envió de los datos esta condicionado a que la transformacion no supere el valor de esta variable.

- Compara brevemente Dev Tunnels con simplemente usar la IP local. ¿Cuáles son las ventajas y desventajas de cada uno?

Dev Tunnels:
- Encripta los datos personales de los paquetes enviados para mayor seguridad.
- Alcance publico global por medio de internet.
- Latencia moderada.

IP Local:
- Acceso limitado pues no es accesible por internet.
- Acceso rapido.
- Latencia muy baja.

- Coloca en tu bitácora capturas de pantalla del sistema completo funcionando. Esto lo puedes hacer abriendo tanto el mobile como el desktop en tu computador y tomando una captura de pantalla de todos los involucrados (celular, computador y terminal).

<img width="389" height="649" alt="image" src="https://github.com/user-attachments/assets/d7474541-2877-4989-9076-5f4d8f65c1bc" />

<img width="397,5" height="640" alt="image" src="https://github.com/user-attachments/assets/51872888-d63e-4197-b859-899e6ee11453" />

<img width="784" height="245" alt="image" src="https://github.com/user-attachments/assets/ddf2dfbe-80d7-4b12-8180-fd593d65044d" />

<img width="396" height="644" alt="image" src="https://github.com/user-attachments/assets/19bc7201-e14f-47a5-8582-617c54d5764a" />

<img width="762" height="461" alt="image" src="https://github.com/user-attachments/assets/4fb1f409-f3cd-4145-a5cd-bce0f5896d1e" />

### Actividad 3

- ¿Cuál es la función principal de express.static(‘public’) en este servidor? ¿Cómo se compara con el uso de app.get(‘/ruta’, …) del servidor de la Unidad 6?

`express.static('public')` sirve automáticamente cualquier archivo dentro de `public/` por lo que es ideal para apps simples o estáticas.

`app.get('/ruta', …)` envía archivos específicos según la URL así que es ideal cuando hay rutas distintas o lógica asociada.

- Explica detalladamente el flujo de un mensaje táctil: ¿Qué evento lo envía desde el móvil? ¿Qué evento lo recibe el servidor? ¿Qué hace el servidor con él? ¿Qué evento lo envía el servidor al escritorio? ¿Por qué se usa socket.broadcast.emit en lugar de io.emit o socket.emit en este caso?

En el sketch de mobile, el evento es `touchMoved()`: `socket.emit('message', touchData);`. Cada vez que el usuario mueve el dedo, se genera un objeto `{ type: 'touch', x: mouseX, y: mouseY }` y se envía al servidor con el evento message.

En server.js de desktop, el servidor escucha el evento message y lo recibe en la variable message.

```
socket.on('message', (message) => {
    console.log('Received message =>', message);
    socket.broadcast.emit('message', message);
});
```

El servidor no modifica los datos, solo los muestra en consola (console.log) y los retransmite a los demás clientes con `socket.broadcast.emit('message', message);`

El servidor reenvía el mismo mensaje como evento message a los demás clientes conectados. El sketch de desktop lo recibe así:

```
socket.on('message', (data) => {
    if (data.type === 'touch') {
        circleX = data.x;
        circleY = data.y;
    }
});
```

y actualiza la posición del círculo en pantalla.

`socket.emit`	envía el mensaje solo al cliente que lo envió. `io.emit` envía el mensaje a todos los clientes, incluido el emisor. `socket.broadcast.emit`	envía el mensaje a todos los clientes excepto al que lo envió.

En este caso, no queremos que el móvil reciba su propio mensaje (porque él ya sabe dónde tocó), por eso se usa socket.broadcast.emit, para que solo el otro dispositivo lo reciba.

- Si conectaras dos computadores de escritorio y un móvil a este servidor, y movieras el dedo en el móvil, ¿Quién recibiría el mensaje retransmitido por el servidor? ¿Por qué?

Los dos escritorios verían moverse el círculo sincronizadamente según el movimiento del dedo.

- ¿Qué información útil te proporcionan los mensajes console.log en el servidor durante la ejecución?

Sirven para ver cuándo se conectan o desconectan clientes, monitorear los mensajes que se envían, incluyendo su contenido (coordenadas, tipo de evento, etc.) y para depurar errores de conexión o sincronización en tiempo real. Es decir, los console.log funcionan como herramientas de diagnóstico para entender el flujo de comunicación entre el móvil, el servidor y los escritorios.

### Actividad 4

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/7f397cf6-16d9-4ab3-8d23-05f10a298b29" />

### Actividad 5

El diseño que llevé a cabo fue primero idear la forma en que el usuario pudiese ineractuar con cada "beat" de la canción seleccionada, y las ondas expansivas me pareción un concepto visualmente satisfactorio. La forma que pensé de identificar estos beats era por medio del análisis FFT (Fast Fourier Transform) contenido en la libreria p5.sound, y la forma de diferenciar los componentes altos  y bajos fuese por medio de los colores de las mimas ondas expansivas. Estas ondas aparecerian en un lugar aleatorio entre un espacio designado para los altos o para los bajos según corresponda.

**index.html (desktop)**

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Visual Musical Desktop</title>
    <script src="https://cdn.jsdelivr.net/npm/p5@1.11.1/lib/p5.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/p5@1.11.1/lib/addons/p5.sound.min.js"></script>
    <script src="/socket.io/socket.io.js"></script>
    <script src="sketch_desktop.js"></script>
  </head>
  <body>
  </body>
</html>

```

**sketch_desktop.js**

```js
let socket;
let song, fft;
let circles = [];

// Variables para normalización
let avgBass = 100;
let avgMid = 100;
let avgTreble = 100;

function preload() {
  song = loadSound("song.mp3");
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  background(20);

  socket = io();

  // Al tocar el móvil, se analiza el sonido actual y se genera una onda según el rango predominante
  socket.on("touchEvent", () => {
    if (song && song.isPlaying()) analyzeFFT();
  });

  userStartAudio().then(() => {
    song.loop();
    fft = new p5.FFT(0.8, 2048);
  });
}

function draw() {
  background(20, 20, 20, 80);
  drawCircles();
}

// ---------------------------------------------------
// Analiza el espectro FFT al instante del toque móvil
// ---------------------------------------------------
function analyzeFFT() {
  let spectrum = fft.analyze();
  let bass = fft.getEnergy("bass");
  let mid = fft.getEnergy("mid");
  let treble = fft.getEnergy("treble");

  // Suavizado dinámico de promedios (tipo "moving average")
  avgBass = lerp(avgBass, bass, 0.05);
  avgMid = lerp(avgMid, mid, 0.05);
  avgTreble = lerp(avgTreble, treble, 0.05);

  let type = null;

  // 🔧 Umbrales dinámicos más balanceados:
  if (bass > avgBass * 1.3 && bass > mid && bass > treble) {
    type = "K"; // Kick - Cyan
  } else if (mid > avgMid * 1.15 && mid > bass && mid >= treble) {
    type = "B"; // Bajo - Amarillo
  } else if (treble > avgTreble * 1.05 && treble > mid && treble > bass) {
    type = "A"; // Alto - Magenta
  } else {
    // fallback: elige el rango con más energía relativa
    const maxVal = Math.max(bass / avgBass, mid / avgMid, treble / avgTreble);
    if (maxVal === bass / avgBass) type = "K";
    else if (maxVal === mid / avgMid) type = "B";
    else type = "A";
  }

  spawnCircle(type);

  // Mostrar valores en consola para análisis
  console.log(
    `🎵 bass:${bass.toFixed(1)} mid:${mid.toFixed(1)} treble:${treble.toFixed(1)} → ${type}`
  );
}

// ---------------------------------------------------
// Crea una onda expansiva en el espacio correspondiente
// ---------------------------------------------------
function spawnCircle(type) {
  let colorMap = {
    K: [0, 255, 255],   // Cyan
    B: [255, 255, 0],   // Amarillo
    A: [255, 0, 255],   // Magenta
  };

  let sectionWidth = width / 3;
  let sectionX =
    type === "A" ? 0 :
    type === "B" ? sectionWidth :
    sectionWidth * 2;

  let x = random(sectionX + 50, sectionX + sectionWidth - 50);
  let y = random(100, height - 100);

  circles.push({
    x,
    y,
    color: colorMap[type],
    radius: 30,
    expanding: true,
  });
}

// ---------------------------------------------------
// Dibuja las ondas expansivas
// ---------------------------------------------------
function drawCircles() {
  for (let i = circles.length - 1; i >= 0; i--) {
    let c = circles[i];
    noFill();
    stroke(c.color);
    strokeWeight(3);
    ellipse(c.x, c.y, c.radius);
    c.radius += 8;

    if (c.radius > width * 1.5) circles.splice(i, 1);
  }
}
```

**index.html (mobile)**

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Móvil - Control Musical</title>
    <script src="https://cdn.jsdelivr.net/npm/p5@1.11.0/lib/p5.min.js"></script>
    <script src="https://cdn.socket.io/4.7.2/socket.io.min.js"></script>
    <script src="sketch_mobile.js"></script>
  </head>
  <body style="margin:0; overflow:hidden; background:#111;"></body>
</html>

```

**sketch_mobile.js**

```js
let socket;

function setup() {
  createCanvas(windowWidth, windowHeight);
  background(30);
  socket = io();

  textAlign(CENTER, CENTER);
  textSize(20);
  fill(255);
}

function draw() {
  background(30);
  text("🎵 TOCA PARA CREAR ONDAS 🎵", width / 2, height / 2);
}

function touchStarted() {
  socket.emit("touchEvent", { x: mouseX / width, y: mouseY / height });
  console.log("📲 Enviado toque al servidor");
  return false;
}

```

**server.js**

```js
const express = require("express");
const http = require("http");
const { Server } = require("socket.io");
const path = require("path");

const app = express();
const server = http.createServer(app);
const io = new Server(server);

const PORT = process.env.PORT || 3000;

app.use(express.static("public"));

io.on("connection", (socket) => {
  console.log("🟢 Cliente conectado:", socket.id);

  // Evento: escritorio detecta un beat o crea círculo
  socket.on("spawnCircle", (data) => {
    console.log("💠 spawnCircle desde desktop:", data);
    socket.broadcast.emit("spawnCircle", data);
  });

  // Evento: móvil toca pantalla
  socket.on("touchEvent", (data) => {
    console.log("📱 touchEvent recibido:", data);
    socket.broadcast.emit("touchEvent", data);
  });

  socket.on("disconnect", () => {
    console.log("🔴 Cliente desconectado:", socket.id);
  });
});

server.listen(PORT, () => {
  console.log(`Servidor escuchando en http://localhost:${PORT}`);
});

```

### Autoevaluación

**Actividad 1:** Leí detalladamente cada información existente y basé mis respuestas en los conocimientos adquieridos por la lectura y por averiguaciones externas. Practicamente, la actividad fluyó correctamente.

**Actividad 2:** Leí detalladamente cada información existente y basé mis respuestas en los conocimientos adquieridos por la lectura y por averiguaciones externas.

**Actividad 3:** Analicé detenidamente las preguntas existentes para hayar una respuesta compuesta entre mis opiniones personales y conocimientos externos adquiridos.

**Actividad 4:** El gráfico representa de forma simple y sencilla, el flujo de datos a traves de los tres componentes: móvil, servidor y escritorio.

Todas las actividades fueron llevadas a cabo satisfactoriamente, por lo que la nota sugerida es 5.



