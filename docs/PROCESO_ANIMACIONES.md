# Huertas Fighters — procedimiento obligatorio de animaciones

## Fuente de verdad
`assets/personajes/kai/walk/kai_caminar_sutil_v4.png` es la única animación vigente como referencia de proceso. Es APNG de 8 fotogramas, 480 × 380 px, RGBA transparente. No se modifica ni se sustituye sin aprobación expresa.

## Prohibiciones
1. NUNCA generar una lámina de 8/16 poses con IA ni recortarla para animarla.
2. NUNCA crear una imagen nueva cuando se pida modificar una animación ya aprobada.
3. NUNCA alterar fotogramas o zonas que el usuario ha indicado conservar.
4. NUNCA fingir transparencia usando un patrón de cuadros; debe ser canal alfa RGBA real.
5. NUNCA publicar en `main` ni declarar una animación aprobada sin revisión del usuario.
6. NUNCA usar enlaces a archivos no comprobados; verificar que existen.
7. NUNCA guardar recursos de prueba descartados en el repositorio.

## Técnica que funcionó para v4
1. Abrir APNG existente con Pillow, extraer sus ocho fotogramas RGBA.
2. Conservar tamaño 480 × 380, alineación del suelo, silueta, colores, guardia y ritmo base.
3. Calcular transformaciones pequeñas por fotograma mediante funciones periódicas y máscaras gaussianas locales.
4. Aplicar `cv2.remap` con interpolación bilineal y color **premultiplicado por alfa**, para evitar halos.
5. Movimiento de piernas/pantalón: mantener el ciclo previamente aprobado; retocar solo deformaciones continuas que no creen costuras, huecos o pies fantasma.
6. Para torso, cabeza, cinta y mechones: usar máscaras distintas y oscilaciones con fases y amplitudes pequeñas. Mantener dirección de mirada.
7. Para movimientos nuevos (puñetazo, patada): partir de una pose maestra existente; definir trayectoria geométrica de hombro-codo-muñeca o cadera-rodilla-tobillo y deformar por regiones. Si el movimiento no puede producirse limpiamente sin redibujar, parar y explicar la limitación: no improvisar una lámina.
8. Exportar únicamente APNG de prueba, 8 frames RGBA 480 × 380, loop infinito, duración uniforme salvo indicación contraria.
9. Validar automáticamente número/tamaño de frames, alfa, ausencia de recortes y diferencias localizadas; revisar visualmente transición entre último y primero.
10. Presentar APNG accesible. Esperar aprobación antes de integrarlo en la web o GitHub.

## Repositorio y despliegue
- GitHub: `lukende/huertas-fighters`.
- Producción: rama `main`; GitHub Pages publica `https://lukende.github.io/huertas-fighters/`.
- Trabajo de animación: `desarrollo-animaciones`.
- Cambios: inspeccionar → modificar lo mínimo → verificar diffs y animación → obtener aprobación → integrar en main → comprobar despliegue.
- GitHub Pages puede acumular ejecuciones de `pages build and deployment`; evitar commits repetidos mientras hay cola. Confirmar publicación efectiva antes de afirmar que está en producción.
- No tocar ni borrar recursos utilizados por el motor público hasta sustituir sus referencias y comprobar que el juego carga.

## Mecánica y gráficos
Juego de lucha 2D HTML5/JavaScript, estilo arcade, personajes Kai y Rex. Movimientos previstos: descanso, avanzar, retroceder, puñetazos, patadas, agacharse, bloqueos, saltos y golpes aéreos. Las animaciones deben usar frames fijos, pose de guardia coherente y hitboxes calculables desde geometría o máscara alfa, sin depender de límites casuales de recorte.
