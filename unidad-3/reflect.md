# Unidad 3

## 🤔 Fase: Reflect

### Actividad 5

**Construye el modelo de la bomba 3.0. Como ya tienes el código puedes tener un modelo muy preciso.**

<img width="882" height="744" alt="image" src="https://github.com/user-attachments/assets/3833d041-9021-42b8-b1d2-6a9b66c9e376" />

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
| ARMED          | Temporizador llega a 0           | Muestra `Image.SKULL`                                                    | EXPLODED                        |
| EXPLODED       | Logo tocado           | Reinicia `count = 20`, muestra en display, vuelve a configurar           | CONFIG                          |
