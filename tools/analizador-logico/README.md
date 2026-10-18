# Analizador lógico (versión modificada)

`STM32F1.ino` es una versión modificada del firmware del proyecto
**logic-analyzer** de Vincenzo (<https://github.com/aster94/logic-analyzer>),
que se distribuye bajo **GPL-3.0**. Esa licencia se aplica a este directorio;
el resto del repositorio es MIT. El texto completo está en
[`LICENSE`](LICENSE).

Captura los cuatro bits altos del puerto B — `PB12`, `PB13`, `PB14` y `PB15` —
por sondeo en el bucle principal, guarda solo los cambios y los vuelca por el
puerto serie a 115200 bps cuando llena el buffer.

## Qué se le cambió respecto al original

- **Número de muestras variable en caliente.** El original fija `samples` en
  tiempo de compilación y espera una sola letra `G` para rearmar. Esta versión
  acepta `G<n>` (por ejemplo `G10`) y usa `n` como número de muestras de la
  siguiente captura, con `n` entre 1 y `samples`. De ahí el `com.write("G10\n")`
  de [`../showLogicDiagram.py`](../showLogicDiagram.py).
- **LED movido** de `PB1` a `PC13` (el LED de a bordo de la Blue Pill), porque
  `PB1` estaba ocupado. Las llamadas a `pinMode`/`digitalWrite` sobre el LED en
  `setup()` quedaron comentadas.
- `Serial.setTimeout(1000)` para que `parseInt()` no se quede colgado.
- Un **generador de señal de delco** integrado en el propio analizador, que
  quedó comentado. Se abandonó en favor del ESP32 con MicroPython
  ([`../generador-esp32/`](../generador-esp32/)), que no roba tiempo al bucle
  de muestreo.
