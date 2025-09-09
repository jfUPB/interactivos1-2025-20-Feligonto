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

**- Escribe el código que soluciona este problema en tu bitácora.**

```python
from microbit import *
import utime

INIT_STATE = 0
ROJO_STATE = 1
AMARILLO1_STATE = 2
VERDE_STATE = 3
AMARILLO2_STATE = 4

ROJO_INTERVAL = 1000
AMARILLO1_INTERVAL = 300
VERDE_INTERVAL = 1000
AMARILLO2_INTERVAL = 300

current_state = INIT_STATE
start_time = 0
interval = 0

while True:
    if current_state == INIT_STATE:
        
        display.set_pixel(0,0,9)
        
        start_time = utime.ticks_ms()
        interval = ROJO_INTERVAL
        current_state = ROJO_STATE
        
    elif current_state == ROJO_STATE:
        
        while utime.ticks_diff(utime.ticks_ms(), start_time) < interval:
            display.set_pixel(0,0,9)
        if utime.ticks_diff(utime.ticks_ms(), start_time) > interval:
            display.set_pixel(0,0,0)

            start_time = utime.ticks_ms()
            interval = AMARILLO1_INTERVAL
            current_state = AMARILLO1_STATE
            
    elif current_state == AMARILLO1_STATE:
        while utime.ticks_diff(utime.ticks_ms(), start_time) < interval:
            display.set_pixel(0,1,9)
        if utime.ticks_diff(utime.ticks_ms(), start_time) > interval:
            display.set_pixel(0,1,0)
            
            start_time = utime.ticks_ms()
            interval = VERDE_INTERVAL
            current_state = VERDE_STATE
        
    elif current_state == VERDE_STATE:
        while utime.ticks_diff(utime.ticks_ms(), start_time) < interval:
            display.set_pixel(0,2,9)
        if utime.ticks_diff(utime.ticks_ms(), start_time) > interval:
            display.set_pixel(0,2,0)

            
            start_time = utime.ticks_ms()
            interval = AMARILLO2_INTERVAL
            current_state = AMARILLO2_STATE

    elif current_state == AMARILLO2_STATE:
        while utime.ticks_diff(utime.ticks_ms(), start_time) < interval:
            display.set_pixel(0,1,9)
        if utime.ticks_diff(utime.ticks_ms(), start_time) > interval:
            display.set_pixel(0,1,0)

            start_time = utime.ticks_ms()
            interval = ROJO_INTERVAL
            current_state = ROJO_STATE
```

**- Identifica los estados, eventos y acciones en tu código.**

Estados:
1. `INIT_STATE` estado en el que inicia el programa.
2. `ROJO_STATE` estado del foco en posicion superior.
3. `AMARILLO1_STATE` estado del foco en posicion meddia.
4. `VERDE_STATE` estado del foco en posicion inferior.
5. `AMARILLO2_STATE` estado del foco en posicion media.

Eventos:
1. `utime.ticks_diff(utime.ticks_ms(), start_time) < interval` 
2. `utime.ticks_diff(utime.ticks_ms(), start_time) > interval`

Acciones:
1. `display.set()` condicionandose con los eventos, implementa un foco al que se le cambia su estado segun corresponda.
2. `start_time = utime.ticks_ms()` determina el comienzo de un conteo.
3. `interval` para ajustar el intervalo a usar.
4. `current_state` cambia el estado en que se encuentra el proceso.

### Actividad 3

**- Explica por qué decimos que este programa permite realizar de manera concurrente varias tareas.**

Esto se dice ya que existen tareas en el programa que pueden activarse correctamente durante el funcionamiento de otra, luego volver al funcionamiento normal.

**- Identifica los estados, eventos y acciones en el programa.**

Estados: 
1. `STATE_INIT` estado en el que inicia el programa.
2. `STATE_HAPPY` estado de cara feliz con ojos.
3. `STATE_SMILE` estado de sonrisa.
4. `STATE_SAD` estado de cara triste con ojos.

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
