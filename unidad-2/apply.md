# Unidad 2

## 🛠 Fase: Apply

### Actividad 4

Estados:
- Modo de configuración (`CONFIG_STATE`)
- Modo de cuenta regresiva (`COUNT_STATE`)

Eventos:
- `button_a.was_pressed()`, condición, presión al botón up.
- `button_b.was_pressed()`, condición, presión al botón down.
- `accelerometer.was_gesture('shake')`,
- condición, temporizador en 0.
- Condición, click al Botón *touch*.

Acciones:
- Subir el contador.
- Bajar el contador.
- Cambio de estado a `COUNT_STATE`.
- Bomba explota, Imprimir BOOM.
- Cambio de estado a `CONFIG_STATE`.

