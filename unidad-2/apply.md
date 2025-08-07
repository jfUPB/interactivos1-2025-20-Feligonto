# Unidad 2

## 🛠 Fase: Apply

### Actividad 4

**Estados:**
- Init (`INIT_STATE`)
- Modo de configuración (`CONFIG_STATE`)
- Modo de cuenta regresiva (`COUNT_STATE`)
- Modo de reinicio (`AGAIN_STATE`)

**Eventos:**
CONFIG_STATE
- `button_a.was_pressed()`, condición, presión al botón A (UP).
- `button_b.was_pressed()`, condición, presión al botón B (DOWN).
- `accelerometer.was_gesture('shake')`, condición, agitar el dispositivo.
COUNT_STATE
- condición, temporizador en 0.
RESTART_STATE
- Condición, click al Botón *touch*.

**Acciones:**
`button_a.was_pressed()`
- Subir el contador.
`button_b.was_pressed()`
- Bajar el contador.
`accelerometer.was_gesture('shake')`
- Cambio de estado a `COUNT_STATE`.
*temporizador en 0*
- Bomba explota, Imprimir BOOM.
- Cambio de estado a `AGAIN_STATE`.
click al Botón *touch*
- Cambio de estado a `CONFIG_STATE`.

