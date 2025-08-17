# Unidad 3

## 🔎 Fase: Set + Seek

### Actividad 5

**Construye el modelo de la bomba 3.0. Como ya tienes el código puedes tener un modelo muy preciso.**

```python
from microbit import *
import utime

display.clear() 

class Event:
    def _init_(self):
        self.eventValue = 0

    def write(self,eventValue):
        self.eventValue = eventValue

    def read(self):
        return self.eventValue

    def clear(self):
        self.eventValue = 0

class MicroBitSensors:
    def _init_(self):
        pass

    def update(self):
        if button_a.was_pressed():
            event.write("A")   

        if button_b.was_pressed():
            event.write("A")

        if accelerometer.was_gesture('shake'):
            event.write("S")

        if pin_logo.is_touched():
            event.write("T")

class SerialTask:
    def _init_(self):
        uart.init(baudrate=115200)

    def update(self):
        if uart.any():
            data = uart.read(1)
            if data:
                if data[0] == ord('A'):
                    event.write("A")
                if data[0] == ord('B'):
                    event.write("B")
                if data[0] == ord('S'):
                    event.write("S")
                if data[0] == ord('T'):
                    event.write("T")                
        
class BombTask:
    def _init_(self):
        self.PASSWORD = ['A','B','A']
        self.key = ['']*len(self.PASSWORD)
        self.keyindex = 0
        self.count = 20
        self.startTime = utime.ticks_ms()
        self.state = 'CONFIG'
        display.clear()
        display.show(self.count,wait=False)

    def update(self):
        if self.state == 'CONFIG':
            if event.read()=="A":
                event.clear()
                self.count = min(self.count+1,60)
                display.show(self.count,wait=False)

            if event.read() == "B":
                event.clear()
                self.count = max(10,self.count-1)
                display.show(self.count, wait=False)

            if accelerometer.was_gesture('shake'):
                self.startTime = utime.ticks_ms()
                self.state = 'ARMED'

        elif self.state == 'ARMED':
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > 1000:
                self.startTime = utime.ticks_ms()
                self.count = self.count - 1
                display.show(self.count,wait=False)
                if self.count == 0:
                    display.show(Image.SKULL)
                    self.state = 'EXPLODED'

            if button_a.was_pressed():
                self.key[self.keyindex] = 'A'
                self.keyindex = self.keyindex + 1

            if button_b.was_pressed():
                self.key[self.keyindex] = 'B'
                self.keyindex = self.keyindex + 1

            if self.keyindex == len(self.key):

                passIsOK = True
                for i in range(len(self.key)):
                    if self.key[i] != self.PASSWORD[i]:
                        passIsOK = False
                        break;
                if passIsOK == True:
                    self.count = 20
                    display.show(self.count,wait=False)
                    self.keyindex = 0
                    self.state = 'CONFIG'
                else:
                    self.keyindex = 0

        elif self.state == 'EXPLODED':
            if pin_logo.is_touched():
                self.count = 20
                display.show(self.count,wait=False)
                self.startTime = utime.ticks_ms()
                self.state = 'CONFIG'

bombTask = BombTask()
event = Event()
microBitSensors = MicroBitSensors()
serialTask = SerialTask()

while True:
    serialTask.update()
    microBitSensors.update()
    bombTask.update()
```

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
| ARMED          | `count = 0`           | Muestra `Image.SKULL`                                                    | EXPLODED                        |
| EXPLODED       | Logo tocado           | Reinicia `count = 20`, muestra en display, vuelve a configurar           | CONFIG                          |
