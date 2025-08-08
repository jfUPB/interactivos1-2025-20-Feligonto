# Unidad 2

## 🛠 Fase: Apply

### Actividad 4

<img width="1053" height="911" alt="image" src="https://github.com/user-attachments/assets/a41728d3-8a6f-4ad8-b6d2-7abd0138b7df" />

### Actividad 5

```python
from microbit import *
import utime

INIT_STATE = 0
CONFIG_STATE = 1
COUNT_STATE = 2
RESTART_STATE = 3

currentState = INIT_STATE
startTime = 0
interval = 0  # en milisegundos
lastSecond = -1  # para controlar cuándo cambia el número mostrado

while True:
    
    if currentState == INIT_STATE:
        interval = 20000
        startTime = 0
        currentState = CONFIG_STATE
        
    if currentState == CONFIG_STATE:
        display.show(str(int(interval / 1000)))

        if button_a.was_pressed():

            if interval <= 60000:
                interval += 1000
                display.show(str(int(interval / 1000)))

        if button_b.was_pressed():

            if interval > 10000:
                interval = max(1000, interval - 1000)
                display.show(str(int(interval / 1000)))

        if accelerometer.was_gesture('shake'):
            startTime = utime.ticks_ms()
            currentState = COUNT_STATE
            lastSecond = -1
        
    if currentState == COUNT_STATE:
        elapsed = utime.ticks_diff(utime.ticks_ms(), startTime)
        remaining = (interval - elapsed) // 1000

        if remaining != lastSecond and remaining >= 0:
            display.show(str(remaining))
            lastSecond = remaining

        if elapsed >= interval:
            display.scroll("BOOM")
            currentState = RESTART_STATE
            
    if currentState == RESTART_STATE:
        display.show(Image.HAPPY)
        if pin_logo.is_touched():
            currentState = INIT_STATE
```

Estado - Evento - Acción - Evento
- `CONFIG_STATE` - `accelerometer.was_gesture('shake')` - ` currentState = COUNT_STATE` - `COUNT_STATE`
- `COUNT_STATE` - `elapsed >= interval` - ` currentState = RESTART_STATE` - `RESTART_STATE`
- `RESTART_STATE` - `pin_logo.is_touched()` - ` currentState = CONFIG_STATE` - `CONFIG_STATE`
