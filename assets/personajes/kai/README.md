# Animaciones de Kai — recursos en desarrollo

Esta rama conserva las pruebas aprobadas sin modificar la web de producción.

Animaciones: `idle` (espera), `punch` (puñetazo), `walk` (avanzar).

Cada carpeta contiene `sources.zip.b64`: un ZIP codificado en Base64 debido al método de transferencia disponible. **No es una imagen ni un archivo para cargar directamente en el juego.** Al decodificarlo se obtienen ocho PNG RGBA de fotograma fijo y `spritesheet.png`.

Para extraer los archivos desde Python:

```python
from pathlib import Path
import base64
for source in Path("assets/personajes/kai").glob("*/sources.zip.b64"):
    source.with_name("sources.zip").write_bytes(base64.b64decode(source.read_text()))
```

Las dimensiones de cada animación son constantes internamente, pero todavía **no están unificadas entre animaciones**: idle 484×385, punch 477×375 y walk 480×380. Antes de integrar en el motor habrá que fijar un lienzo común para todas las posturas y validar transparencia, pies y colisiones. Los GIF son previsualizaciones y no deben usarse como recursos del juego.

No cambiar `main` hasta validar las animaciones.
