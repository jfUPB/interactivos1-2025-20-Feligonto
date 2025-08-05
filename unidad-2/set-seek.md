# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 1

**- Describe detalladamente cómo funciona este ejemplo.**

Este programa es capaz de referenciar dos luces diferentes por medio de la clase `Pixel` con sus respectivos objetos y hace uso de la libreria `utime` para mediar sus estados. 

Al ejecutar el programa, los objetos `pixel1` y `pixel2` leen sus parametros asignados, donde `pixelX` señala el valor de posición en el eje x a la función `display.set_pixel()`, `pixelY` señala el valor de posición en el eje y a la función `display.set_pixel()`, `initState` guarda en `pixelState`, el cual se usa en la función `display.set_pixel()`, el estado con que iniciarán los objetos, e `interval` que alacena el valor en milisegundos para determinar el flujo de interacción al compararlo con la diferencia entre el comienzo `startTime` y el tiempo ocurrido en tiempo real `ticks_ms()`, de un ciclo. Luego, empezando dede el estado Init se pasa al estado `WaitTimeout` para que los objetos queden allí generando un bucle de encendido y apagado de los focos.

**- ¿Cuáles son los estados en el programa?**

1. Init: Pseudo-estado inicial el cual no vuelve a ser usado despues de ejecutar.
2. WaitTimeout: Estado en el cual permanece el programa despues de la ejecución .

**- ¿Cuáles son los eventos/inputs en el programa?**

Los eventos son los tiempos quetienen que esperear cada objeto para un cambio.

**- ¿Cuáles son las acciones en el programa?**

Las acciones son determinar el brillo del foco, sea alto o nulo.

### Actividad 2



### Actividad 3

**- Explica por qué decimos que este programa permite realizar de manera concurrente varias tareas.**

Esto se dice ya que existen tareas en el programa que pueden activarse correctamente durante el funcionamiento de otra, luego volver al funcionamiento normal.

**- Identifica los estados, eventos y acciones en el programa.**

Estados: 
1. STATE_INIT
2. STATE_HAPPY
3. STATE_SMILE
4. STATE_SAD

Eventos:
1. Paso del tiempo determinado para cada estado, determinado por las variables: `HAPPY_INTERVAL`, `SMILE_INTERVAL` y `SAD_INTERVAL`.
2. Detección de botón activado.

Acciones:
1. Cambio de display
2. Inicio de contador `utime.ticks_ms()`
3. Cambio de intervalo
4. Cambio de estado

**- Describe y aplica al menos 3 vectores de prueba para el programa.**

Estado - Evento - Acción

1. `STATE_HAPPY` - `utime.ticks_diff(utime.ticks_ms(), start_time) > interval` - `display.show(Image.SMILE)`
2. `STATE_SAD` - `button_a.was_pressed()` - `interval = SMILE_INTERVAL`
3. `STATE_INIT` - `current_state == STATE_INIT` - `current_state = STATE_HAPPY`
