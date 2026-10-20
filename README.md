# AstrAudia
*El universo tiene imágenes. Nosotros les damos voz.*

Versión 2.4. Desarrollado por **Gabriel Loza Medina**: [Instagram @gabriellozamedina](https://www.instagram.com/gabriellozamedina/) y [Facebook /gabriellozam](https://www.facebook.com/gabriellozam).

AstrAudia convierte una astrofotografía (JPG o PNG) en una sonificación (WAV) y en un video con la imagen, la animación del modo elegido y el audio sincronizado. Todo se procesa en el navegador; la imagen no se envía a ningún servidor.

## Compartir
`index.html` funciona solo: no depende de otros archivos. Basta con enviarlo; quien lo reciba debe descargarlo y abrirlo en Chrome o Edge (la vista previa de WhatsApp o del correo no lo ejecuta). Solo la tipografía se descarga de internet.

## Ejecución
1. Abre `index.html` con Chrome, Edge o Firefox recientes (doble clic basta). No requiere instalación ni conexión: las bibliotecas de empaquetado de video (mp4-muxer y webm-muxer, licencia MIT) están incrustadas. La conexión solo se usa para cargar la tipografía Saira; sin internet se usa la del sistema.
2. Si tu navegador bloquea algo al abrir el archivo directamente, sírvelo en local: `python -m http.server` dentro de la carpeta y abre `http://localhost:8000`.

## Uso
La interfaz sigue seis pasos numerados. La barra superior muestra en qué paso estás y cuáles ya completaste; la vista previa queda fija a la derecha mientras recorres los pasos.

1. **Cargar:** arrastra la imagen a la vista previa o pulsa “Elegir archivo”. Aquí se ven los datos del análisis (resolución, fondo de cielo, ruido, candidatos) y el histograma.
2. **Modo:** cuatro tarjetas con una fotografía real de ejemplo (NASA, dominio público), la etiqueta del tipo de imagen recomendado, qué hace el modo y para qué fotos conviene. Son sugerencias; cualquier modo acepta cualquier imagen.
   - Barrido tonal: nebulosas y galaxias con estructuras difusas.
   - Fuentes de iluminación estelares: campos estelares y cúmulos.
   - Paisaje sonoro cósmico: nebulosas y regiones con mucha estructura.
   - Sonificación por canales RGB: astrofotografías a color.
3. **Sonido:** ajustes principales del modo; las opciones avanzadas y la detección de fuentes quedan en secciones plegables.
4. **Previsualizar:** genera el WAV (PCM 16 bits, estéreo, 48 kHz) y muestra en la vista previa el cuadro exacto que se exportará. Reproduce, pausa, detén o salta con la barra; barra espaciadora para reproducir/pausar.
5. **Formato:** tarjetas de orientación horizontal 16:9 (YouTube y videos convencionales) o vertical 9:16 (TikTok, Instagram Reels y Facebook Reels); la vista previa cambia al elegir. Resolución, FPS, calidad y duración quedan a la vista; el formato de archivo, título, datos, cuadrícula, fundidos, firma, tarjeta de créditos y video limpio están en “Opciones avanzadas del video”.
6. **Video:** genera y descarga `AstrAudia_<Objeto>_<Modo>.mp4` o `.webm` (con `_vertical` si es 9:16). Aquí también están los perfiles y la importación/exportación de la configuración en `.json`.

## Video vertical para Reels, TikTok y Shorts
- Resoluciones 1080 × 1920 (predeterminada, Full HD, la recomendada para Instagram) o 720 × 1280 (más rápida).
- **Imagen a pantalla completa (predeterminado):** la fotografía ocupa todo el cuadro y se ve completa, sin recortes. Una imagen 9:16 llena el video entero; una con otra proporción llena el ancho o el alto y deja solo las franjas mínimas. El título va arriba y el panel de datos (con la firma) abajo, encima de la foto, sobre degradados oscuros que los hacen legibles. El panel termina antes del 16 % inferior, donde Instagram pone la descripción.
- “Acomodo en video vertical” (opciones avanzadas) ofrece también “Imagen arriba, datos debajo” y “Todo fuera de las zonas de Instagram”, que dejan la imagen más chica.
- “Vertical: marcar en la vista previa las zonas que tapa Instagram” dibuja en naranja las zonas de referencia; no se exporta.
- Instagram no acepta WebM. Para subir un Reel, exporta desde Chrome o Edge en computadora, donde se genera MP4.

## Giro automático de la imagen
- El programa compara la orientación de la imagen con la del video. Si girarla 90° la hace al menos un 5 % más grande en el espacio disponible, la gira en sentido horario. Así, una foto vertical en video horizontal, o una horizontal en video vertical, aprovecha la pantalla y se ve completa, sin recortes.
- Al girar, el lado izquierdo de la imagen queda arriba: el barrido y la guía de tiempo avanzan de arriba hacia abajo. El audio no cambia, y la imagen, la escala, el barrido, los marcadores y las capas giran juntos, así que la sincronía es la misma. Las etiquetas de texto se dibujan derechas.
- El paso 5 avisa cuando la imagen se va a girar. “Giro de la imagen: No girar” desactiva el giro.
- La pestaña *Análisis* siempre muestra la imagen en su orientación original.

## Ajuste automático del umbral
- Si al cargar una imagen no se detecta ninguna fuente, el paso 1 lo avisa y ofrece “Sí, ajustar automáticamente”. El programa prueba umbrales más bajos (6, 5, 4, 3,5, 3, 2,5 y 2 σ) y se queda con el primero que encuentra al menos 12 fuentes; si ninguno llega, usa el que encontró alguna.
- Si eliges el modo Fuentes de iluminación estelares sin fuentes detectadas y pulsas generar, la misma oferta aparece en el paso 4; al aceptarla se ajusta el umbral y se genera el audio.
- Si ni con 2 σ aparecen fuentes, el aviso sugiere usar Barrido tonal o Paisaje sonoro cósmico. El umbral se puede afinar a mano en “Detección de fuentes”.

## Aviso de progreso
Mientras se genera el audio o el video aparece una barra fija en la parte inferior de la pantalla, visible aunque estés en otro paso o en el celular: indica qué se genera, el porcentaje, el cuadro que se renderiza y pide mantener la pantalla abierta. Tiene botón para cancelar. El botón pulsado cambia a “Generando…”. Al terminar, la barra indica “Video listo” o “Audio listo” con un botón que lleva al resultado.

## Resolución y vista previa
- El video se exporta por defecto en Full HD: 1920 × 1080 en horizontal o 1080 × 1920 en vertical. 720p queda como opción más rápida.
- La vista previa se dibuja a 720p como máximo para que sea fluida en el celular; es el mismo cuadro, escalado. El video sí se renderiza a la resolución elegida.
- **Vista previa flotante (solo en pantallas de hasta 1050 px, como el celular):** cuando la vista previa principal sale de la pantalla al bajar por los pasos, aparece una miniatura en la esquina inferior derecha que muestra en vivo la composición (o el análisis) y se actualiza sola con cada cambio. En la vista Análisis siempre muestra la imagen completa con las detecciones y el centro de los modos polares, aunque la vista principal tenga zoom. Botones: reproducir o pausar, ajustar el zoom de la vista principal para ver la imagen completa (⊡), ir a la vista completa (⤢) y ocultarla. Se mueve arrastrándola desde la imagen o la barra superior; recuerda su posición y se mantiene dentro de la pantalla al girar el teléfono. En computadora no aparece.

## iPhone y iPad
- **Cómo abrirla:** en iPhone, un archivo HTML abierto desde Archivos, WhatsApp, Telegram o el correo se muestra en una vista previa que no ejecuta programas, así que la app no funciona ahí. Hay que abrirla desde un enlace web en Safari o Chrome: súbela a un hosting (por ejemplo, el de un sitio de WordPress), a GitHub Pages o a Netlify. Si se abre en esa vista previa, la página muestra un aviso naranja con estas instrucciones.
- **Fotos de iPhone:** acepta HEIC/HEIF además de JPG y PNG. Si `createImageBitmap` falla, la imagen se decodifica con un elemento `<img>`. En iPhone/iPad la copia para dibujar se limita a 2560 px por lado para cuidar la memoria.
- **Sonido:** se pide a iOS una sesión de audio de reproducción, para que la previsualización suene aunque el interruptor de silencio esté activado.
- **Video:** en iOS 26 se usa WebCodecs (MP4 H.264 + AAC). En versiones anteriores, sin AudioEncoder, se usa la captura en tiempo real con MediaRecorder, que en Safari también produce MP4. Si WebCodecs falla a medio camino, se reintenta en tiempo real. Durante la exportación se pide mantener la pantalla encendida (Wake Lock).
- **Guardar el video:** el botón “Compartir o guardar en Fotos” abre la hoja de compartir del sistema, desde la que se puede guardar en Fotos o enviar a Instagram. Solo aparece donde el navegador permite compartir archivos.
- **Procesamiento:** si el worker no responde en 4 s, el cálculo pasa al hilo principal.

## Modos inspirados en la NASA
Basados en las sonificaciones del Chandra X-ray Center y NASA’s Universe of Learning con SYSTEM Sounds (Russo, Santaguida y Arcand, *Jingle, Pluck, and Hum: Sonifications of Space Imagery Data*, ICAD 2022). Como en ellas, el brillo controla el volumen y la dirección del recorrido se elige según la forma del objeto.
- **Radar estelar** (galaxias espirales, objetos centrados; la NASA lo usó con M87, R Aquarii y Cassiopeia A): un brazo gira en sentido horario desde las 12 en punto. Rejilla polar: cada anillo de distancia al centro es un oscilador (centro grave, borde agudo, o al revés); el brillo total del brazo abre un filtro paso bajo; las estrellas suenan como arpa (Karplus-Strong). Controles: centro (imagen o fuente más brillante), dirección del tono, arpa, resolución angular y anillos de tono. Ejemplo: M101, NASA/JPL-Caltech/ESA/STScI/CXC.
- **Onda expansiva** (remanentes de supernova, nebulosas redondas; la NASA lo usó con Tycho): un anillo crece desde el centro. Cada sector del anillo es una voz: volumen por brillo, nota por tono de color (rojo grave, azul y violeta agudo; los colores poco saturados se acercan al centro de la escala). Arpa en las estrellas que alcanza. Ejemplo: Nebulosa del Cangrejo, NASA/ESA/JPL/Arizona State Univ.
- **Orquesta multibanda** (regiones de formación estelar a color; como las sonificaciones que combinan varios telescopios, p. ej. Westerlund 2): barrido de izquierda a derecha con tres instrumentos: cuerdas para el exceso de rojo en 10 franjas afinadas a un acorde abierto, piano pulsado en los máximos locales del verde y campanas FM en las estrellas detectadas. Volumen independiente para cada instrumento. Ejemplo: Westerlund 2, NASA, ESA, Hubble Heritage Team.
- **Centro del recorrido** (Radar y Onda expansiva): centro de la imagen; *centro de la luz (automático)*, centroide de la emisión por encima del fondo, útil cuando el objeto no está centrado en la foto; fuente más brillante; o *manual*: al elegirlo se abre la pestaña Análisis y un toque en la imagen marca el centro (cruz naranja); “Centro horizontal” y “Centro vertical” permiten afinarlo en décimas de porcentaje. La cruz azul muestra el centro que se usará en los modos automáticos.
- **Lupa del centro (celular, ventana flotante):** con el centro en modo manual, al mover “Centro horizontal” o “Centro vertical” la ventana flotante se convierte durante 2,5 s en una lupa cuadrada centrada en el punto elegido: imagen ampliada, retícula fina, cruz naranja, fuentes detectadas y las coordenadas en porcentaje. Un toque en la lupa cambia el aumento (×6, ×12, ×3). El botón 🔍 la deja fija mientras se ajusta; solo aparece con el centro manual en Radar u Onda expansiva.
- Los tres admiten el estilo cósmico y el fiel, la rotación automática, el formato vertical y la actualización automática. Cambiar el umbral de detección regenera también estos modos, porque usan las estrellas detectadas.

## Estilo del sonido
- **Cósmico (predeterminado):** los datos de la imagen siguen controlando el sonido igual y la sincronía con la animación no cambia; encima se agrega una cadena de espacio profundo, común a los cuatro modos:
  - *Armonía:* en Barrido tonal y RGB las frecuencias se afinan a una escala pentatónica. Fuentes estelares y Paisaje sonoro ya la usaban.
  - *Fondo atmosférico:* acorde grave (raíz, quinta, octava y un color cálido o frío) más *viento cósmico*, ruido filtrado por bandas que se desplazan lentamente y que pesa más en imágenes nebulosas. Ambos siguen la luminosidad de la imagen.
  - *Destellos cósmicos:* las estrellas detectadas suenan como campanitas lejanas en el instante en que el recorrido pasa por ellas (en Paisaje sonoro, repartidas en el tiempo). Además, la mezcla subida una y dos octavas (*shimmer*) alimenta la reverberación y da el brillo característico del sonido “espacial”.
  - *Espacio profundo:* coro lento que ensancha el estéreo, ecos de ida y vuelta (0,43 s y 0,65 s) con agudos amortiguados y reverberación de sala enorme (Freeverb con colas largas y predelay). Al final, compresión suave.
  - En Fuentes de iluminación estelares, las fuentes más brillantes suman un golpe grave.
  - **RGB cósmico:** cada franja de la imagen es una voz de tres osciladores desafinados. El canal de frecuencia elige la nota (afinada y con deslizamientos lentos); el de timbre abre y cierra un filtro paso bajo de 4 polos (barridos de filtro); el de modulación controla una pulsación lenta y la anchura del coro (ya no un vibrato rápido); el de volumen, la intensidad con ataques suaves. La franja inferior suena una octava abajo.
  - Controles en el paso 3: “Espacio profundo”, “Fondo atmosférico” y “Destellos cósmicos”.
- **Fiel a los datos:** sonificación directa, sin escala impuesta ni efectos.

## Actualización automática
Después de generar el audio, cualquier cambio de ajuste o de modo vuelve a analizar la imagen y regenera el sonido solo (tras 0,65 s sin más cambios). Si se estaba reproduciendo, se pausa y retoma en el mismo punto. La barra inferior avisa “Volviendo a analizar la imagen y actualizando el sonido…” y luego “Sonido actualizado”. Al mover el umbral de detección aparece “Volviendo a analizar la imagen…” y después el número de fuentes detectadas. La casilla “Actualizar el sonido automáticamente” del paso 4 lo desactiva; entonces vuelve el aviso con el botón “Regenerar audio”.

## Créditos en el video
- **Fotografía:** el único campo que llena quien usa la app (paso 5, “Créditos del video”, “Autor de la fotografía”). Se recuerda en el navegador y se guarda en la configuración `.json`.
- **Sonificación:** fija, no editable: “Sonificación: AstrAudia · Gabriel Loza Medina”.
- **Créditos discretos** (activados por defecto, durante todo el video): “Fotografía: …” y “Sonificación: AstrAudia · Gabriel Loza Medina” van a la misma altura, con el mismo tamaño y en lados opuestos. En horizontal, dos recuadros semitransparentes en la parte superior (izquierda y derecha); en vertical, la última fila del panel de datos, con letra de 15 unidades (legible en el celular).
- **Tarjeta final** (opcional, desactivada por defecto): Fotografía (si se escribió), Sonificación: Gabriel Loza Medina, “Creada con AstrAudia” e Instagram y Facebook.

## Imágenes de ejemplo de los modos
Galaxia de Andrómeda (NASA/JPL-Caltech, GALEX), Omega Centauri (NASA/JPL-Caltech/UCLA, WISE), Acantilados Cósmicos de Carina (NASA, ESA, CSA, STScI, Webb) y Nebulosa de Orión (NASA/JPL-Caltech/STScI). Van incrustadas en el HTML como miniaturas.

## Formato del video
- **Cuadro por cuadro (WebCodecs):** cada cuadro se dibuja en el instante `n / FPS` con el mismo plan que generó el audio, así que la sincronía no depende de la velocidad del equipo. Si el navegador tiene codificadores H.264 y AAC, el resultado es **MP4 (H.264 + AAC)**; si no, **WebM (VP9 o VP8 + Opus)**. La interfaz indica siempre el formato real.
- **Captura en tiempo real (MediaRecorder):** alternativa para navegadores sin WebCodecs. Tarda lo que dura la composición, la pestaña debe permanecer visible y la duración puede exceder la del audio por unas décimas.

## Pruebas ejecutadas (Chromium 141 sin interfaz, imágenes sintéticas)
- Nebulosa 1400×900: análisis en ~0,4 s; 190 fuentes detectadas (de 250 estrellas inyectadas, varias débiles).
- Los cuatro modos generan WAV válidos (ffprobe: pcm_s16le, 48 kHz, 2 canales, 8,000 s; pico 0,76 con volumen 80 %, sin saturación).
- Video de los cuatro modos: WebM VP9 + Opus, 1280×720, 30 FPS, 8,000 s, igual que el audio; decodifica sin errores con ffmpeg.
- Cuadro a 4,0 s de 8 s: la línea de barrido está en el 50 % del ancho. En el modo de fuentes, los marcadores activos coinciden con la línea guía de tiempo.
- Pausa: la animación queda en la misma posición que el audio; reanudar continúa desde ahí; detener vuelve a 0.
- Cambiar un parámetro después de generar muestra el aviso de composición desactualizada.
- Imagen negra: “sin señal”, 0 detecciones, y el modo de fuentes se niega a generar con un mensaje claro.
- Imagen de 6000×4000 (24 MP): análisis en 1,4 s; bloqueo máximo del hilo de la interfaz ≈ 250 ms (decodificación).
- Cancelación de la exportación y de la síntesis; regeneración correcta después de cancelar.
- Integración con el muxer MP4 verificada empaquetando VP9 + Opus en MP4 (decodifica con ffmpeg).
- Captura en tiempo real con MediaRecorder: archivo con video y audio.

## Pruebas de la versión 2.4
- Celular: al mover los deslizadores del centro manual apareció la lupa (195 × 195 px) centrada en 40,0 % y 43,0 %; un toque cambió el aumento de ×6 a ×12; volvió a la vista normal 2,5 s después de soltar; con 🔍 quedó fija. Arrastrar la ventana sigue funcionando y no cambia el aumento.

## Pruebas de la versión 2.3
- Celular: con la vista principal alejada, la miniatura siguió mostrando la imagen completa; ⊡ restauró el zoom ajustado; arrastrarla la movió a la posición indicada.
- Créditos de fotografía y sonificación al mismo tamaño y altura en vertical y horizontal; sin autor, solo el de sonificación.

## Pruebas de la versión 2.2
- Escritorio: resolución inicial 1920 × 1080; la miniatura flotante no aparece.
- Celular (390 px): la miniatura aparece al bajar al paso 3, se actualizó tras la regeneración automática al cambiar de modo, reproduce y pausa desde su botón y desaparece al volver a la vista completa. Vertical inicia en 1080 × 1920.

## Pruebas de la versión 2.1
- Nebulosa anular sintética descentrada (centro real en 40 %, 43 %): el centro de la luz se ubicó en 40,4 %, 43,3 % (página) y 40,3 %, 43,2 % (síntesis). Un toque en la vista Análisis fijó el centro manual en 62 %, 71 % y la onda se regeneró desde ahí.
- Créditos con y sin autores en horizontal y vertical, y tarjeta final; sin errores en la consola.

## Pruebas de la versión 2.0
- Radar, Onda expansiva y Orquesta: síntesis de 30 s en 3 a 4 s, sin valores inválidos; cuadros correctos en horizontal y vertical (también con la imagen girada).
- Corregido: tras la regeneración automática ya no queda el aviso “La previsualización aparece aquí después del paso 4” encima de la composición.

## Pruebas de la versión 1.9
- Cuatro modos en estilo cósmico y fiel: sin valores inválidos, pico en el volumen elegido. Correlación entre canales de 0,05 a 0,41 en cósmico (estéreo amplio) frente a 0,82 a 0,93 en fiel. Síntesis de 30 s en 3 a 6 s en computadora.
- Espectrograma de RGB: lecho continuo, notas deslizadas sin vibrato, destellos y colas largas.

## Pruebas de la versión 1.8
- Estilo cinematográfico en los cuatro modos: sin valores inválidos, pico en el volumen elegido, síntesis de 30 s en 2 a 3 s en computadora.
- Cambio de volumen durante la reproducción: aviso, regeneración y reproducción retomada. Cambio de umbral: aviso y número de fuentes. Cambio de estilo: los controles de espacio y fondo se ocultan en “Fiel a los datos”.

## Pruebas de la versión 1.7
- WebKit 26 (motor de Safari) con emulación de iPhone 13: carga, análisis, detección, síntesis y reproducción correctas; aviso de “no se está ejecutando” visible con JavaScript desactivado. La exportación de video no se pudo probar ahí: el WebKit de Linux no trae codificadores de video ni MediaRecorder, y su `VideoEncoder.isConfigSupported` cierra la pestaña (fallo del entorno de pruebas, no de iOS).
- Chrome: sin cambios en el comportamiento; video vertical exportado sin errores.

## Pruebas de la versión 1.6
- Vertical a pantalla completa con imágenes 9:16, 5:8, vertical de campo estelar y horizontal (girada y sin girar), en los cuatro modos: imagen completa sin recortes, título y datos legibles. Video vertical de 10 s exportado sin errores.

## Pruebas de la versión 1.5 (vista de celular, 390 px)
- Imagen sin fuentes: aparece la oferta en el paso 1; con el modo de estrellas, también en el paso 4. Al aceptar, el umbral bajó de 8 a 2 σ, se detectaron fuentes y se generó el audio del modo de estrellas.
- Durante la exportación del video la barra inferior mostró el porcentaje y el cuadro en curso; al terminar mostró “Video listo” y “Ver video” llevó al video.

## Pruebas de la versión 1.4
- Imagen vertical en video horizontal (cuatro modos) e imagen horizontal en video vertical: giro correcto, imagen completa, guía y marcadores en su instante, etiquetas derechas. Con “No girar” se ve como antes.
- Video horizontal de 6 s con la imagen girada: WebM VP9 + Opus sin errores.

## Pruebas de la versión 1.2 (Google Chrome sin interfaz)
- Recorrido completo de los seis pasos: carga, cambio de modo, audio, cambio a vertical (la vista previa pasa sola a la composición 720 × 1280) y video WebM VP9 + Opus de 10 s sin errores en la consola.
- Firma visible y legible en ambas orientaciones sin tapar la imagen ni los datos.
- Vista de celular (390 px): la vista previa queda arriba y los pasos debajo, en una columna.

## Pruebas de la versión 1.1 (Google Chrome sin interfaz, imagen sintética de 1500 × 1000)
- Los cuatro modos en vertical y horizontal: cuadros revisados al 40 %, 50 % y 98 % de la duración. El horizontal se ve igual que en la versión 1.0.
- Video vertical del modo por color: WebM VP9 + Opus, 1080 × 1920, 30 FPS, 30 s, 48 kHz (ffprobe). Sin errores en la consola.
- Los créditos aparecen al final en ambas orientaciones; las guías de zona segura solo se dibujan en la previsualización.

## No verificado
- **MP4 con H.264 + AAC:** los navegadores de prueba (Linux sin interfaz) no incluyen esos codificadores. El código los detecta y los usa en Chrome/Edge de escritorio, pero no pude comprobar el archivo resultante.
- Safari y Firefox no se probaron. Firefox no tiene `AudioEncoder` en todas las versiones; en ese caso se usa la captura en tiempo real.
- Las pruebas usaron imágenes sintéticas, no astrofotografías reales.

## Limitaciones
- FITS no está soportado todavía (la carga muestra un aviso).
- El análisis trabaja a un máximo de 1600 px de lado; el video usa la imagen hasta 3072 px.
- JPG/PNG ya están procesados: la sonificación refleja ese procesamiento, no los datos crudos del sensor.
- Las fuentes detectadas son candidatos, no estrellas confirmadas.
