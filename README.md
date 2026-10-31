# AstrAudia
*El universo tiene imágenes. Nosotros les damos voz.*

Versión 1.0. © 2026 Gabriel Loza Medina. Todos los derechos reservados (ver `LICENSE`). Desarrollado por **Gabriel Loza Medina**: [Instagram @gabriellozamedina](https://www.instagram.com/gabriellozamedina/) y [Facebook /gabriellozam](https://www.facebook.com/gabriellozam).

AstrAudia convierte una astrofotografía (JPG o PNG) en una sonificación (WAV) y en un video con la imagen, la animación del modo elegido y el audio sincronizado. Todo se procesa en el navegador; la imagen no se envía a ningún servidor.

## Compartir
`index.html` funciona solo: no depende de otros archivos. Basta con enviarlo; quien lo reciba debe descargarlo y abrirlo en Chrome o Edge (la vista previa de WhatsApp o del correo no lo ejecuta). Solo la tipografía se descarga de internet.

## Publicación protegida
La carpeta del proyecto tiene dos partes:
- **`fuente/`** (privada, no se sube a un repositorio público): `index.html` legible, este README, `LICENSE` y `ofuscar.js`. Aquí se hacen los cambios.
- **`publicar/`** (lo que se sube al repositorio de GitHub Pages): `index.html` con el JavaScript propio ofuscado y `LICENSE`.

Qué hace la ofuscación (javascript-obfuscator 5.8.1):
- Interfaz y animación: nombres internos ilegibles, código compactado en una línea y textos codificados en base64.
- Motor de audio (worker y utilidades compartidas): nombres ilegibles y compactado, sin codificar textos, porque eso volvía la síntesis 1,5 a 2 veces más lenta.
- Los nombres de las funciones globales se conservan, porque los scripts y el worker los comparten. Las bibliotecas de terceros (mp4-muxer y webm-muxer, MIT) se dejan con su aviso de licencia.
- Ninguna ofuscación impide leer el código a quien insista: solo lo vuelve muy difícil de entender y modificar. La protección de fondo es el aviso de derechos de autor (comentario al inicio del HTML, pie de la página y `LICENSE`).

Para regenerar la versión publicable después de editar `fuente/index.html`:
```
npm install javascript-obfuscator@5.8.1
node ofuscar.js index.html ../publicar/index.html
```

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

## Audio del video en iPhone
- **MP4 con AAC (WebCodecs):** se pide explícitamente AAC sin cabeceras ADTS y, si el codificador no entrega la descripción del formato, se añade la AudioSpecificConfig de AAC-LC a 48 kHz en estéreo. Sin esa descripción el video puede sonar en el navegador pero perder el audio al recortarlo en Fotos de iOS. Si el codificador no produce audio, se pasa a la captura en tiempo real.
- **Captura en tiempo real (iPhone sin AudioEncoder):** el contexto de audio se crea y desbloquea en el mismo toque del botón (iOS solo permite activarlo ahí) y el grafo se conecta casi en silencio a la salida para que corra.
- **Verificación:** en Safari, si el video terminado no tiene pista de audio, se avisa y se sugiere el WAV o generarlo desde una computadora.
- No verificado en un iPhone real: las pruebas automáticas usan Chrome y WebKit de Linux, que no traen los codificadores de iOS.

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

## Zonas a ignorar (textos, firmas, logos)
En el paso 1, “Zonas a ignorar”. Las zonas marcadas se tratan como cielo vacío: no suenan, no cuentan como fuentes ni cráteres y no influyen en el centro automático. La imagen del video no cambia, salvo que se active “Oscurecer en el video las zonas ignoradas” (opciones avanzadas del video).
- **Detectar textos automáticamente:** busca piezas que sobresalen de su entorno inmediato (umbral local, funciona también sobre nebulosas brillantes), las agrupa en renglones y solo acepta los que tienen rasgos de escritura: 3 o más piezas no redondas, alturas parecidas, una línea base común y una franja exterior con la misma textura que el resto de la imagen (texto sobreimpreso). Los renglones de 3 piezas, como «M42», deben ser aún más regulares. Une renglones cercanos en un solo bloque.
- **Pintar zonas / Borrar zonas:** pincel con el dedo o el ratón en la vista Análisis; tamaño ajustable. “Quitar todas” limpia la máscara.
- Cada cambio vuelve a analizar la imagen y, con la actualización automática, regenera el sonido.

## Efecto cinematográfico
- Opción en el paso de Video, “Efecto cinematográfico”: Ninguno (por defecto), Acercamiento lento o Viaje entre estrellas. Se ve en la vista previa y se graba en el video; funciona con los 11 modos y sus animaciones siguen a la cámara.
- **Cámara:** avanza hacia lo más interesante de la foto: el planeta más grande, el núcleo del cometa, el centro del Sol, el cráter más grande o, en galaxias y nebulosas, el centro de la luz (centroide del brillo al cubo). El avance acumula la intensidad del sonido (más rápido en los momentos intensos). El acercamiento máximo depende de la resolución de la foto para no pixelar (hasta 1,55× en Acercamiento lento y 1,9× en Viaje). En Viaje, un leve balanceo de la cámara.
- **Viaje entre estrellas:** la imagen se separa en dos capas: el fondo sin estrellas (cada una de las 260 más brillantes se cubre con el color del fondo, tomado a 2,8 veces su radio, con borde difuminado) y cada estrella como luz propia (su recorte menos el color del fondo, sumado como luz). Al avanzar, las estrellas se alejan del centro más rápido que el fondo según una profundidad aleatoria estable (paralaje). Partículas finas pasan junto a la cámara desde el punto de destino, con estela, y una viñeta suave enmarca la imagen.
- Unos 28 ms por cuadro a 1080p.

## Visual lunar: contorno, regolito y espectro frío
- **Contorno con la forma real:** el borde de cada cráter (círculo o elipse) se traza conforme la línea lo cruza, en línea continua: plata clara del lado iluminado y azul pizarra del lado en sombra. Dos puntas de luz marcan dónde la línea toca el borde, arriba y abajo. Al salir se desvanece. El cruce y el sonido del borde usan el tamaño real de la elipse.
- **Polvo de regolito:** desde los dos puntos de contacto se levantan partículas plateadas (4 por punto en cada emisión, hasta 90 emisiones por cráter) que suben despacio y vuelven a caer con baja gravedad, durante 1,9 s; más brillantes del lado iluminado y más abundantes en cráteres grandes. Se calcula según el tiempo con números pseudoaleatorios deterministas, así que el video y la vista previa coinciden cuadro a cuadro.
- **Línea y espectro con estilo frío:** línea plateada con halo escarchado; barras finas plata y azul hielo con una punta brillante; las marcas de los cráteres en espera, en plata tenue y con su forma.
- Unos 30 ms por cuadro a 1080p.

## Detección de cráteres por plantilla
- Con luz rasante, los cráteres se buscan con una plantilla a muchos tamaños (de 3 px a 16 % de la imagen) y en cinco formas (círculo y elipses achatadas horizontal y vertical, para cráteres vistos en perspectiva como Platón). En cada posición se mide el brillo en un anillo interior (0,8 R, sobre las paredes) y uno exterior (1,45 R). Firma de un cráter: por dentro, la pared del lado del Sol en sombra y la opuesta iluminada (el anillo sigue un coseno en la dirección de la luz); por fuera, al revés. Una montaña da el patrón opuesto y se descarta sola.
- Reglas: patrón suficientemente claro (más exigente para los pequeños, que en terreno montañoso se imitan con huecos entre picos); el borde exterior no repite el patrón (en los pequeños, debe estar iluminado del lado del Sol); alrededores calmados (en terreno caótico se descarta); la pared del lado del Sol claramente más oscura que la pared iluminada; la pared iluminada más brillante que los alrededores (descarta mares y rayos); un interior oscuro solo vale con pared iluminada nítida (Platón sí, un mar no); superficie alrededor (no cielo ni el borde del disco).
- Duplicados en dos pasos: primero, para cada cráter, el tamaño que mejor encaja; luego, los grandes primero, descartando los que caen sobre su borde o dentro siendo demasiado grandes. Se permiten cráteres pequeños dentro de uno grande.
- Dirección de la luz: si se distingue el lado del Sol (el brillo de la superficie crece hacia él), la luz de los cráteres va en sentido contrario y solo cuentan las parejas compatibles.
- La vista Análisis dibuja cada cráter con su forma (círculo o elipse).
- Recomendación: las reglas de Luna solo se aplican a fotos con pocas estrellas (menos de 150; las de cielo profundo tienen cientos).
- Pruebas: Platón, Cassini, Copérnico y sus vecinos detectados con su forma; sin falsos en el borde del disco de la Luna gibosa ni sobre mares o rayos; mucho menos ruido en zonas montañosas; detección en 1,4–1,6 s.

## Recorrido del borde de cada cráter
- Mientras la línea cruza un cráter (de un borde al otro; mínimo 0,3 s), lo toca en dos puntos, arriba y abajo: cada uno es una voz (una octava sobre la resonancia del cráter). Las voces se separan en tono a medida que el cráter se abre bajo la línea (hasta una tercera menor) y se juntan al salir, así se escucha la forma del borde. Del lado en sombra suenan oscuras (filtro cerrado) y con un batido inquietante; al llegar al borde iluminado se aclaran y suman un brillo agudo. Control “Recorrido de los bordes” (70 % por defecto).
- El impacto grave ahora cae cuando la línea entra en el cráter (a un cuarto del cruce).
- **Visual:** el borde se dibuja conforme pasa la línea (la parte ya recorrida), frío del lado en sombra y cálido del lado iluminado según la luz real, y se desvanece al salir.
- Durante el cruce de un cráter grande, el recorrido del borde cambia más de la mitad de lo que suena.

## Cráteres con dramatismo de luz y sombra
- Cada cráter guarda la intensidad de su sombra y la de su borde iluminado (de la pareja que lo detectó), y la dirección real de la luz en la foto.
- **Tamaño → tono:** escala In desde 55 Hz a lo largo de cuatro octavas; los pequeños agudos y breves, los grandes un abismo grave (golpe de 30 Hz) y largo (hasta 6,8 s).
- **Sombra → dramatismo:** cuanto más profunda, más disonante (se suman una segunda menor y un tritono), más oscura (filtro) y con una cola y ecos más largos (tercer eco a 1,63 s).
- **Borde iluminado → destello:** parciales de cristal (6,1; 8,3; 11,7) al inicio del impacto, según su brillo.
- **La luz recorre el sonido:** cada impacto se desplaza en estéreo del lado de la sombra al lado iluminado durante el primer segundo y medio.
- **Inhalación:** los cráteres grandes crecen al revés justo antes del golpe (hasta 1,1 s).
- **Ambiente lunar:** más oscuro y con un leve temblor donde hay más sombras.
- **Visual discreto:** al sonar, la sombra del cráter se ahonda del lado del Sol, el arco del borde opuesto brilla y un anillo fino se expande y se desvanece. Los cráteres que esperan su turno apenas se marcan.

## Crédito de AstrAudia permanente
- “Sonificación: AstrAudia · Gabriel Loza Medina” se dibuja siempre en el video, con cualquier combinación de opciones (también en “Video limpio” y con todo desactivado), en horizontal y vertical. Antes la casilla de créditos discretos podía quitarlo.
- La casilla ahora se llama “Mostrar el autor de la fotografía en la esquina” y solo controla el crédito del autor de la foto; una nota aclara que el de AstrAudia siempre aparece. La tarjeta de créditos final, el título, los datos y lo demás siguen siendo opcionales.

## Firma de cada cometa
- El canto conserva su carácter (zumbido, voces fantasmales, pulso, gorjeos), pero cada foto elige sus detalles a partir de lo que tiene; la misma foto suena siempre igual (la firma sale de una huella estable de la geometría y el color).
- **Color del halo de la coma** (anillo alrededor del núcleo, pesando más lo que tiene más color; el centro suele estar saturado): verde → canto de «o» a «u»; azulado → de «e» a «i»; cálido → de «a» a «o»; blanco → de «a» a «u».
- **Tono base:** una de cinco notas graves (sol, la♭, la, si♭, si) según la dirección de la cola y la huella de la foto.
- **Segunda voz:** según qué tan alargada es la cola: quinta, cuarta, tercera menor o tritono.
- **Acentos del pulso:** compás de 3, 4 o 5, con la intensidad de cada tiempo tomada del brillo a lo largo de la cola.
- **Velocidad del pulso** (±12 % sobre el control “Ritmo del pulso”) según la longitud de la cola, y gorjeos con frecuencia y barrido propios.
- El texto para acompañar el video menciona la firma de la foto.

## Núcleo del cometa en su lugar
- El núcleo se buscaba sobre la luz difusa (imagen sin estrellas, que también borra el pico central), cuyo centro queda desplazado hacia la cola: la animación y el sonido empezaban un poco adelante del núcleo (hasta 68 píxeles en Tsuchinshan-ATLAS). Ahora, al final del análisis y sin cambiar ninguna medida, el núcleo se afina con la luminancia real: el centro de la zona más brillante (≥ 92 % del máximo) cerca de la coma, que funciona aunque el núcleo esté saturado. La cola se alarga lo que haya entre ese punto y el pico difuso. Desfase con el punto más brillante: de 4–68 a 2–8 píxeles.

## Modo Planetas
- **Detección (uno o varios):** discos compactos claramente más brillantes que el cielo (fondo = percentil bajo, para que un Sol que llena la foto no cuente como “fondo”). Se exige borde nítido (el brillo justo dentro del borde cae al del cielo justo afuera, medido en las direcciones iluminadas), cielo vacío alrededor, núcleo compacto y que no sea un anillo con el centro oscuro. Así no se confunden comas de cometas, núcleos de galaxias, nebulosas ni zonas brillantes del Sol.
- **Rasgos de cada planeta (sin decir cuál es):** anillos (la mancha se extiende mucho más allá de un globo redondo), fase (media luna: núcleo muy alargado o fracción iluminada baja), bandas (contraste entre franjas horizontales que coincide en las dos mitades del disco y es uniforme a lo largo de cada franja; así los mares de la Luna o la textura de un disco borroso no cuentan) y color (rojizo, azulado).
- **Sonido (música de las esferas de Kepler, Harmonices Mundi, 1619):** cada planeta canta un motivo de dos notas que va y viene; el intervalo depende de su aspecto (anillos: tercera mayor; bandas: tercera menor; rojizo: quinta; fase: un semitono, “casi una sola nota”, como Kepler le dio a Venus; azulado: cuarta; liso: segunda). Más grande → más grave. El sonido gira alrededor de quien escucha (paneo circular con leve vaivén de tono por efecto Doppler). Las bandas laten siguiendo su perfil, como la rotación; los anillos suman parciales agudos inarmónicos (5,4; 6,83; 8,93) que brillan despacio; el color cambia los armónicos y un planeta en fase suena más cerrado. Los planetas suenan de izquierda a derecha (tiempo según su tamaño) y al final todos juntos en un acorde. Controles: Giro alrededor, Latido de las bandas, Brillo de los anillos y Acorde final.
- **Animación discreta:** órbita tenue (inclinada como los anillos, si los hay) y un pequeño satélite que gira al ritmo del paneo alrededor del planeta que suena; en el acorde final, en todos.
- **Recomendación:** varios planetas comparables, o uno con anillos o bandas reales, van antes que el Sol y el cometa; un planeta solo, liso o en fase, también se reconoce (si coincide con un supuesto núcleo de cometa, gana el planeta).
- **Miniatura:** Júpiter y Saturno, fotografía de Gabriel Loza Medina. Sección Sistema Solar.

## Modo Cometas
- **Detección:** se quitan las estrellas (apertura morfológica 7×7) y el fondo del cielo, calculado solo con el cielo (se excluyen las siluetas oscuras con contorno nítido, como árboles; un cielo que se oscurece poco a poco no cuenta como silueta). Se ignora la luz de zonas con mucha textura fina (follaje). Candidatos a núcleo: picos de luz difusa aislados (medidos a varias distancias, para comas pequeñas y grandes) y también puntos brillantes tipo estrella (comas diminutas en fotos de campo amplio); sin cola, pierden. Desde cada candidato se lanzan 90 rayos; en cada distancia se resta la mediana de todas las direcciones y la cola es el haz estrecho que más destaca frente a sus vecinos a ±20°. Se mide el brillo y el ancho de la cola a lo largo de 128 puntos.
- **Cometa claro (para recomendar el modo):** el núcleo domina la luz difusa (2 veces o más), la cola se apaga al alejarse (85 % o más de los tramos), poca luz difusa en el resto de la foto (9 % o menos) y una cola claramente asimétrica (3 veces) o alargada (2,4 veces). Si se elige el modo a mano, se usa el mejor núcleo y cola aunque no sean “claros”.
- **Núcleo a mano:** en la vista Análisis del modo Cometas, tocar el núcleo lo fija y la cola se busca desde ahí.
- **Sonido (del núcleo al final de la cola):** zumbido grave (55 Hz) que late, canto fantasmal de dos voces en diente de sierra filtradas por formantes que pasan de «a» en el núcleo a «u» en la cola y bajan de tono al alejarse, gorjeos (barridos rápidos de tono) en cada pulso, más frecuentes donde la cola brilla más, inspirados en el «canto» del cometa 67P registrado por Rosetta (ESA, 2014), y un soplo de polvo que crece con el ancho de la cola. Todo late al “Ritmo del pulso” (2,6 por segundo por defecto) y viaja en estéreo en la dirección de la cola. Controles: Ritmo del pulso, Canto fantasmal, Zumbido grave y Gorjeos.
- **Animación:** una sonda luminosa viaja del núcleo por la cola, con anillos a cada pulso; el tramo recorrido queda iluminado y el núcleo late.
- **Miniatura:** fotografía de cometa de Gabriel Loza Medina. El modo está en la sección Sistema Solar.
- **Modo Sol:** para recomendarlo con un disco brillante se exige además un borde nítido (un cúmulo o una galaxia redonda se desvanecen poco a poco).

## Texto para acompañar el video
- Al generar el sonido aparece en el paso 3 la tarjeta “Texto para acompañar tu video”, con un botón “Copiar” (no aparece en el video). Explica en pocas frases y sin tecnicismos cómo se escuchó esa foto, con los datos reales del análisis: cómo se recorrió la imagen, qué significa cada sonido y lo que se detectó (estrellas azuladas y rojizas, capas, cráteres y de dónde llega la luz, manchas, filamentos, regiones brillantes, protuberancias, fase del eclipse, notas del piano y campanas, etc.). Empieza con el nombre del objeto (si se escribió) y termina con el autor de la foto (si se escribió) y “Sonificación: AstrAudia · Gabriel Loza Medina.”
- Se rehace al cambiar el nombre, el autor o el idioma, y al regenerar el sonido.

## Sonido lunar misterioso
- **Cráter:** impacto que resuena en el vacío, en lugar de los chasquidos agudos de polvo (que sonaban a gotas de agua). Golpe grave (38–88 Hz) que se hunde de tono, resonancia hueca de cavidad (parciales inarmónicos 1; 1,006; 1,47; 2,09; 2,76, con un leve batido entre los dos primeros) y dos ecos lejanos, más oscuros y del lado contrario (0,42 s y 0,91 s). Más grande → más grave y más largo (1,2 a 4,4 s); sombra más profunda → más fuerte. La resonancia se afina en la escala japonesa In (0, 1, 5, 7, 8 semitonos) a partir de mi grave (82,4 Hz), así que los cráteres forman una melodía inquietante.
- **Ambiente lunar:** zumbido grave que respira (raíz en mi, 41,2 Hz, con segunda menor, tritono y octava) y un soplo muy grave, que sigue el brillo de la superficie en cada momento: presente en las zonas iluminadas, casi en silencio en las sombras. Control “Ambiente lunar” (55 % por defecto).
- En los momentos de los cráteres, la energía en graves frente a agudos pasó de 0,32 a 0,97.

## Cráteres de la Luna
- **Luminancia real:** en el modo Luna y planetas el brillo se normaliza con los extremos de la propia foto (percentiles 0,5 y 99,7), sin restar un “fondo de cielo”: en una foto de cerca no hay cielo y antes el mar oscuro quedaba en cero (no sonaba ni se buscaban cráteres ahí).
- **Sombra + borde iluminado:** con luz rasante, cada cráter es una pareja de sombra (del lado que mira al Sol, por dentro) y borde iluminado (del lado opuesto). Se buscan manchas oscuras y brillantes a 8 escalas (aceptando formas de media luna) con contraste relativo al brillo de su zona, y se emparejan.
- **Dirección de la luz:** histograma de las direcciones sombra→borde de todas las parejas; la luz es el sector que destaca claramente sobre los demás.
- **Cráteres frente a montañas:** en una montaña la pareja va al revés (lado iluminado hacia el Sol, sombra detrás). El lado del Sol se deduce del oscurecimiento hacia el terminador (gradiente del brillo muy suavizado dentro de la superficie); si el pico del histograma apunta hacia el Sol, son montañas y se usan las parejas opuestas.
- **Cráter:** centro entre la sombra y el borde, radio según su separación y escala; se exige superficie alrededor de todo el cráter (descarta el borde del disco contra el cielo) y un tamaño máximo (un mar no es un cráter). Con luz frontal (Luna llena) se usan manchas redondas, como antes.
- **Vista Análisis:** en este modo se muestran los cráteres detectados (naranja: pareja sombra-borde; tenue: hoyo pequeño) con el conteo y el tipo de luz, y se actualizan al cambiar la sensibilidad o el máximo.
- **Recomendación:** una foto gris con muchos cráteres de luz rasante se reconoce como superficie aunque no se vea el disco completo.

## Flujo automático y filamentos como energía
- **Al cargar una foto:** se ignoran los textos, se detecta el tipo de foto (Sol, superficie, galaxia, nebulosa, campo de estrellas), se aplica el modo recomendado y se genera el sonido de inmediato, con un solo aviso (“Foto analizada: modo …. Generando el sonido…”). El ajuste automático del umbral de estrellas solo se hace si el modo elegido usa estrellas (Fuentes estelares, Radar, Onda, Orquesta) y ya no muestra avisos. Después, cualquier ajuste regenera el sonido como antes.
- **Sonido de filamento (“energía que fluye”):** flujo de plasma (ruido en banda con filtro de variable de estado, cuyo centro sube durante el recorrido) con pulsos que avanzan, más un núcleo tonal de dos osciladores ligeramente desafinados que se desliza hacia arriba (una cuarta en filamentos oscuros, una quinta en brillantes). El sonido viaja en estéreo de un extremo del filamento al otro y dura todo el cruce del radar.
- **En la animación:** mientras suena, el filamento se ilumina completo y un pulso de luz blanca con estela lo recorre de un extremo al otro.
- **Filamentos anchos:** el detector de crestas trabaja a tres escalas (delgados, medianos y anchos) y en cada punto toma la respuesta más fuerte.
- **Limbo brillante:** para manchas y regiones brillantes, la referencia combina el perfil de brillo por anillos (que capta el borde brillante u oscuro del limbo) con la corrección local de iluminación; así ya no aparecen regiones brillantes falsas a lo largo del borde. Los filamentos usan la referencia local simple.

## Todos los estilos de fotos del Sol
Probado con H-alfa sobre fondo amarillo, H-alfa sobre fondo claro (filamentos y protuberancias oscuros), H-alfa con contraste invertido sobre negro (filamentos brillantes) y SDO/AIA 304 Å de la NASA.
- **Borde del disco con fondo claro:** en cada dirección, si el paso del disco al fondo es un salto brusco (borde oscuro del limbo), el borde es ese salto; si el brillo sube poco a poco, decide el final de la textura de la superficie. Se elige el círculo de mejor ajuste.
- **Filamentos con su forma:** detector de crestas y valles (matriz Hessiana del brillo suavizado): una curvatura fuerte a lo ancho y casi nula a lo largo. Funciona con filamentos oscuros y brillantes. Cada filamento se guarda como polilínea y con el arco de ángulos que ocupa: en Análisis, en la ventana flotante y en el video se dibuja como una curva rosa, que se ilumina completa mientras el radar la cruza; suena con un tono sostenido durante todo ese cruce (más agudo si el filamento es brillante).
- **Manchas y regiones a dos escalas:** una fina (manchas pequeñas) y una amplia (regiones activas, fáculas grandes), con el umbral medido en cada escala; lo que ya forma parte de un filamento no se repite como mancha.
- **Hasta el limbo:** el análisis del disco llega al 98,5 % del radio (antes 95 %) y solo se descartan los detalles pegados al borde mismo.
- **Del limbo hacia afuera:** el umbral de las protuberancias se mide con la variación típica alrededor del disco (mediana + 4 desviaciones), no relativo a la más grande, así que las pequeñas también suenan. Con fondo claro se cuentan también las protuberancias oscuras.
- **Brillo relativo:** la búsqueda de detalles y la comprobación de que el disco tiene luz propia usan brillo relativo al cielo de alrededor, así que funcionan con imágenes casi monocromas (AIA 304 Å en rojo).
- **Eclipse solo si la Luna tapa de verdad:** la lógica de eclipse en fase se aplica solo si el detector marcó el disco como incompleto (o es manual); los canales oscuros de 304 Å ya no recortan el recorrido.

## Sol en H-alfa
- **Disco más oscuro que el fondo:** en fotos en H-alfa con fondo claro o color invertido, el disco se busca como la zona bajo el umbral de Otsu que no toca el marco (círculo robusto RANSAC, que ignora los huecos de las fáculas) y luego se afina con la textura: la superficie del Sol tiene detalle fino y el fondo es liso, así que el borde real está donde termina la textura, aunque cerca del limbo el disco se vea más claro. En una foto real de 2048 px el círculo quedó sobre el limbo.
- **Detalles del disco:** la textura fina (fibrillas, granulación) se suaviza antes de buscar. Lo claramente más oscuro que su entorno se separa por su forma (momentos de segundo orden): compacto → mancha (cuenco grave); alargado (más de 3,2 veces más largo que ancho) → filamento (tono sostenido, más largo cuanto más largo es); lo claramente más brillante → región brillante o fácula (campana aguda, control “Regiones brillantes” en Más ajustes). En la vista Análisis: naranja, rosa (elipse orientada) y amarillo claro, con el conteo arriba.
- Con fondo claro artificial no hay corona real: no suenan el viento de corona ni el coro de protuberancias, ni se aplica la lógica de eclipse en fase.

## Español e inglés
- **Idioma inicial:** el que la persona eligió antes (se guarda en el navegador); si no hay elección, el del dispositivo: español si el primer idioma preferido empieza por “es” (cualquier región hispanohablante), inglés en los demás casos.
- **Selector ES | EN** en el encabezado: cambia el idioma al instante, sin recargar y sin perder la foto, los ajustes ni el sonido generado.
- **Cómo funciona:** un diccionario español → inglés con unas 590 entradas. Los textos fijos de la página se traducen recorriendo el documento; cada texto recuerda su original, así que al volver al español se restaura exacto (aunque dos textos distintos compartan traducción, como “Foto” y “Fotografía” → “Photo”). Los textos que genera la app (avisos, resultados, etiquetas de ajustes, progreso del motor, mensajes de error) pasan por `_tr()` y las plantillas con valores por `_trt()` con marcadores {0}, {1}…, que permiten cambiar el orden de las palabras en inglés. Los textos dibujados en la animación y en el video (título, datos del panel, créditos, avisos de zonas de Instagram, lupa) se traducen al dibujarse.
- Los nombres de archivo de descarga no cambian con el idioma. `LICENSE` incluye una versión en inglés.
- **Auditoría:** con la app en inglés se recorrió el flujo completo (cuatro fotos, los nueve modos, pincel, explicación científica y video) capturando todo el texto del documento y todo lo dibujado en el lienzo: no quedó texto en español. Al volver al español no quedan textos en inglés salvo un aviso temporal ya oculto.

## Zonas a ignorar a la vista y pincel visible
- “Zonas a ignorar” (Detectar textos, Pintar, Borrar, Quitar todo) está visible en el paso 1; solo la detección de estrellas y los datos técnicos quedan en “Detección y datos de la imagen” (plegado).
- Al pintar o borrar aparece una barra sobre la imagen con el texto de la acción, el control de tamaño del pincel y el botón “Listo”. Un círculo del tamaño exacto del pincel sigue al cursor o al dedo (relleno rojo al pintar, punteado al borrar); al mover el control de tamaño sin el puntero encima, el círculo se muestra un momento en el centro. Con ratón, el cursor normal se oculta mientras se pinta.

## Fases delgadas
- El brillo de referencia del Sol es ahora lo más brillante del disco (percentil 95 de 720 muestras), no un promedio. En una fase delgada casi todo el disco es Luna y el promedio decía “disco oscuro”, así que el eclipse no se reconocía y el radar recorría el círculo completo.
- En un eclipse parcial (foto con filtro) no hay corona visible: la altura automática sobre el borde se limita al 15 % del radio, y el resplandor del filtro ya no suena como corona.
- La detección de la fase es una función propia que se usa al cargar (para dibujar) y al sintetizar.

## Eclipses en fase y manchas en todo el disco
- **Recorrido sobre la fase:** en un eclipse parcial o anular se mide, en cada dirección, si el borde del Sol es visible y desde qué distancia del centro empieza la parte iluminada (el borde de la Luna se reconoce como una franja oscura continua, tan oscura como el cielo; una mancha no cuenta). Solo se trata como eclipse si una parte grande del disco está oscura. El radar recorre únicamente el arco visible más largo, así que todo el tiempo del audio cae sobre la media luna; dentro del disco el brazo se dibuja solo sobre la parte iluminada y el panel muestra el porcentaje iluminado en esa dirección.
- **Voz de la fase:** la anchura iluminada en la dirección del brazo suena como una voz sostenida (fundamental, quinta y octava): más ancha, más aguda y más fuerte. Así se oye la forma de la media luna. Control “Fase del eclipse” en Más ajustes.
- **Manchas en todo el disco:** antes solo se tomaban las primeras 80 en orden de ángulo, y se perdían las del lado izquierdo; ahora se eligen las 160 más grandes y oscuras y luego se ordenan para el recorrido. La referencia de brillo es local (convolución normalizada que ignora lo oscuro y el cielo), así funciona aunque la foto tenga iluminación desigual.
- **Reproducir:** si se pulsa reproducir estando en Análisis, la vista cambia sola a Composición.

## Modo Sol más claro
- **Altura sobre el borde automática:** el anillo punteado marca hasta dónde se escuchan la corona y las protuberancias. Ahora se calcula solo: se mide, en anillos cada vez más alejados del borde, el brillo de la dirección más luminosa (percentil 97) y la altura llega hasta donde ese brillo sigue claramente por encima del cielo, más un margen. En luz blanca, sin corona visible, queda en un margen delgado (8 % del radio); con coronógrafo o en eclipse se extiende con la corona (49 % y 88 % en las pruebas). Se puede fijar a mano en “Más ajustes”.
- **Manchas visibles al cargar:** la detección del disco también busca las manchas, así que en la vista Análisis (y en la ventana flotante) aparecen marcadas en naranja desde el inicio, con el conteo arriba (“8 manchas solares”, “Sin manchas solares visibles” o, en un eclipse total, “Disco tapado: suenan la corona y las protuberancias”). En el modo Sol ya no se muestran las detecciones del detector general de estrellas, que sobre el disco solar no tienen sentido. Con el disco manual, las manchas se recalculan al soltar el círculo o cambiar el radio.

## Manchas solares y sección Sistema Solar
- **Manchas solares (modo Sol):** en discos brillantes (luz blanca, H-alfa, eclipse parcial) se busca en el interior del disco lo que es claramente más oscuro que el brillo típico a esa misma distancia del centro (mediana por anillos), para no confundir el oscurecimiento natural hacia el borde con una mancha. Se aplica una apertura morfológica que borra estructuras de 1 o 2 píxeles (líneas, recuadros dibujados) antes de agrupar. Se descartan lo diminuto, lo pegado al limbo y lo enorme (la Luna en un eclipse parcial). El disco se compara con el cielo que lo rodea (anillo justo afuera del borde), no con el fondo de toda la foto, porque cuando el Sol llena el encuadre ese “fondo” es el propio Sol.
- **Sonido:** cada mancha suena como un cuenco metálico grave (parciales inarmónicos 1; 2; 2,76; 4,07; 5,4) cuando el brazo del radar pasa sobre ella. Más grande, más grave y más larga; más oscura, más fuerte. Control “Manchas solares” en los ajustes del modo. En la animación las manchas se marcan y destellan al sonar, y el brazo cruza el disco.
- **Recomendación:** además del color cálido (ahora por mediana, robusto ante recuadros o textos grises sobre el disco), se reconoce el Sol por su oscurecimiento hacia el borde, así que un Sol gris en luz blanca también recibe el modo Sol; basta con 8 direcciones sin tapar (eclipses parciales).
- **Sistema Solar:** los modos se agrupan en “Galaxias, nebulosas y estrellas” (Barrido, Fuentes estelares, Paisaje, RGB), “Sistema Solar” (Sol y eclipses; Luna y planetas) y “Técnicas de la NASA” (Radar, Onda, Orquesta). El modo de superficies se llama ahora “Luna y planetas”.

## Interfaz simplificada
Principio: primero lo esencial, lo avanzado plegado. No se quitó ninguna función.
- **Cuatro pasos:** 1 Foto · 2 Modo · 3 Escuchar · 4 Video (antes seis). Los ajustes del modo están bajo las tarjetas de modos; el formato y la generación del video, en un solo paso.
- **Paso 1:** botón para elegir la foto, nombre opcional y la recomendación. Detección de estrellas, zonas a ignorar y datos técnicos (archivo, resolución, fondo, ruido, histograma) están en “Herramientas de la imagen” (plegado), con un resumen de una línea (estrellas detectadas y porcentaje ignorado).
- **Paso 2:** tarjetas con nombre, para qué foto sirven y una frase; la recomendación y el crédito de la imagen de ejemplo solo en la tarjeta elegida. Debajo, “Ajustes” con lo esencial y las secciones plegables Efectos cósmicos, Más ajustes y Detección (avanzado). Las ayudas largas de los controles principales se ocultan.
- **Paso 3:** “Generar sonido”. El resultado aparece solo cuando existe, con un texto corto; el detalle técnico queda en la descripción emergente. “Opciones de la vista previa” (plegado): actualización automática y animación.
- **Paso 4:** orientación (YouTube / Reels y TikTok), resolución y autor de la foto. “Generar video” crea antes el sonido si hace falta (ya no hay un segundo botón). FPS, calidad, giro, duración, formato y opciones de superposición, en “Opciones avanzadas del video”.
- **Vista previa:** las capas (estrellas detectadas, mapa de intensidad, realzar zonas débiles, tamaño real) en el menú “Capas”; quedan a la vista −, + y Ajustar. Los controles de reproducción aparecen cuando hay sonido; el progreso se muestra en la barra flotante. Con muchas estrellas los marcadores se dibujan más pequeños y tenues.
- **Ajuste automático:** al cargar, la foto se ajusta completa y centrada, y se reajusta sola si cambia el tamaño del área, hasta que la persona hace zoom o la mueve; “Ajustar” vuelve a la vista completa. En el celular el alto de la vista previa se adapta a la forma de la foto.
- **Gestos en el celular:** un dedo sobre la imagen desplaza la página (no mueve la foto); dos dedos hacen zoom y la mueven. Los modos de pintar y de mover el centro o el disco siguen usando un dedo.
- **Encabezado en el celular:** sin lema ni estado (el estado se ve en la barra flotante); la ayuda queda en un botón “?”.

## Experiencia de uso
- **Diales a prueba de desplazamiento:** en pantallas táctiles un dial solo cambia con un arrastre horizontal deliberado (al menos 8 px y más horizontal que vertical). Un deslizamiento vertical que empieza sobre el dial desplaza la página (`touch-action: pan-y`) y, si el navegador alcanzó a mover el valor, se restaura sin disparar la actualización. Con ratón y teclado funcionan como siempre.
- **Ventana flotante:** muestra siempre el análisis (detecciones, zonas, centro o disco) para ver al momento el efecto de cada ajuste; solo cambia a la composición mientras se reproduce y vuelve al análisis al pausar o detener.
- **Pincel que se apaga solo:** después de pintar, al desplazarse por la página (más de 60 px), cambiar de modo o de vista, o generar el audio, el pincel se desactiva y las zonas se conservan; los toques siguientes ya no pintan. Para volver a pintar hay que tocar “Pintar zonas”. En el celular, al activarlo, la vista previa sube sola.
- **Modo recomendado al cargar:** en el paso 1 aparece “Modo recomendado para tu fotografía”, con el motivo y los botones “Usar este modo” y “Elegir otro”; la tarjeta del modo lleva la marca “Recomendado para tu foto”. No cambia nada hasta que se acepta. Reglas, en orden: disco solar oscuro o cálido → Sol y eclipses; disco grande gris o superficie gris que llena la imagen → Superficies; brillo extendido con su zona más brillante cerca del centro → Radar estelar; objeto redondo de color → Onda expansiva; campo con más de 250 estrellas y poca nebulosidad → Fuentes estelares; nebulosidad extensa y de varios colores → Orquesta; nebulosidad extensa → Paisaje sonoro; colores muy distintos → canales RGB; en otro caso, Barrido tonal.
- **Ajustes esenciales:** cada modo muestra en el paso 3 solo duración, volumen y sus 1 a 4 controles clave. Estilo, espacio profundo, fondo atmosférico y destellos están en “Efectos cósmicos” (plegado); los ajustes finos del modo en “Más ajustes de este modo”; la separación de fuentes en “Detección de fuentes (avanzado)”. Las secciones sin controles para el modo elegido no se muestran.
- **Centro del recorrido** en Radar y Onda: ahora empieza en “Centro de la luz (automático)”.

## Detección de fuentes
- **Fondo y ruido locales:** a la imagen suavizada se le resta un fondo local (desenfoque amplio), y el ruido se mide en ese residuo, globalmente (mediana robusta) y en cada zona (promedio del residuo en una ventana amplia). Una estrella cuenta si sobresale de su propio entorno. Así se detectan estrellas sobre galaxias, nebulosas o resplandor del cielo, y no se toma como fuente la textura de la corona solar o del terreno lunar. En una foto de Andrómeda con mucho resplandor se pasó de 0 a 2186 fuentes; en una foto del Sol en coronógrafo, de 1890 detecciones falsas a menos de 100.
- **Umbral automático garantizado:** cada foto empieza en 8 σ; si hay menos de 40 fuentes, se prueba la escalera 12, 10, 8, 6, 5, 4, 3,5, 3, 2,5, 2 y 1,5 σ y se usa el umbral más alto que encuentre al menos 40 (o el que más encuentre). Se avisa con un mensaje.
- **Umbral a la mano:** el control “Umbral de detección de fuentes” y el botón “Ajustar el umbral automáticamente” están en el paso 1, con el número de fuentes detectadas.
- **Textos ignorados al cargar:** la detección de textos corre sola y marca créditos y rótulos como zonas a ignorar (se avisa y se puede deshacer en “Zonas a ignorar”). Las letras gruesas ya no se confunden con estrellas: una estrella es redonda y su brillo cae desde el centro; una letra tiene brillo parejo. Los renglones cortos pegados a un bloque de texto confirmado (como «CDMX» bajo un rótulo) se suman al bloque.

## Sol y eclipses
- **Disco solar:** se busca primero un disco oscuro (eclipse, coronógrafo): la componente oscura más grande que no toca el marco, probando varios umbrales a partir del de Otsu porque en un eclipse el cielo también es negro. Si no hay, un disco brillante (H-alfa, luz blanca). El círculo se ajusta al borde por mínimos cuadrados (método de Kasa) y solo se acepta si el borde es realmente circular (error medio menor al 6 % del radio en discos oscuros y al 3,5 % en brillantes). Funciona aunque el disco esté cortado por el encuadre. En la vista Análisis se dibuja el disco detectado y la zona analizada.
- **Eclipse en fase y disco cortado:** si el disco brillante no está completo, se ajusta un círculo robusto por RANSAC sobre el borde. Entre los arcos de la media luna (Sol y Luna) se elige el círculo que más puntos explica y que contiene la parte brillante (el Sol), con al menos 110° de borde visible; luego se afina por mínimos cuadrados con esos puntos. En pruebas sintéticas (40 % cubierto, media luna fina y disco cortado por el encuadre) el centro y el radio quedaron a menos del 1 % del real.
- **Manual (arrastrar y soltar):** “Disco solar: Manual” parte del disco detectado y lleva la vista previa a la pantalla. Arrastrando dentro del círculo se mueve; arrastrando su borde (perilla ↔) cambia el radio. “Marcar 3 puntos del borde” calcula el círculo que pasa por tres toques sobre el borde visible, ideal para eclipses en fase. En Radar y Onda, el centro manual también se arrastra. Los controles numéricos siguen disponibles para afinar.
- **Miniatura del celular:** dibuja el disco y la zona analizada y se actualiza al cambiar el centro, el radio o la altura.
- **Sugerencia:** con disco oscuro, o disco brillante de color cálido (la Luna gris no se confunde con el Sol).
- **Sonido:** un brazo recorre el borde desde las 12 en punto. 24 alturas sobre el borde (hasta “Altura analizada”, 50 % del radio por defecto) son 24 voces, de grave a agudo. Para cada altura se resta la corona típica (mediana alrededor del disco): solo suena lo que sobresale, de modo que las protuberancias destacan como un coro. El brillo de la corona en cada dirección mueve un viento de ruido filtrado. Cada protuberancia empieza con un barrido ascendente y una nota a la altura que alcanza.
- **Sugerencia:** si se detecta un disco solar, la tarjeta del modo muestra “Sugerido para tu foto” (también se sugiere Fuentes estelares en campos de estrellas oscuros).
- Ejemplo: eclipse total del 21 de agosto de 2017, NASA/Carla Thomas.

## Celular: elegir el modo
En pantallas de hasta 560 px los modos se muestran como lista compacta (miniatura, nombre y para qué fotos sirven). La tarjeta elegida se despliega con su descripción y recomendación. Los nueve modos ocupan unos 960 px en lugar de casi 2700.

## Superficies y cráteres (estilo Juno y Cassini)
Sigue el método con que la NASA presenta las señales de sus sondas (ondas de radio y plasma de Juno en Ganímedes, Cassini en Saturno, Galileo en Júpiter): los datos se trasladan al rango audible y se oyen como un espectrograma.
- **La imagen es el espectrograma:** horizontal = tiempo, vertical = frecuencia (arriba agudo, abajo grave, escala logarítmica de 60 Hz a 8 kHz), brillo = intensidad. Recorrido de izquierda a derecha o de arriba hacia abajo (bandas de Júpiter).
- **Síntesis espectral inversa:** FFT inversa en ventanas de Hann de 2048 muestras con salto de 512; fases independientes por canal (estéreo amplio). “Carácter de la señal” fija cuánto de la fase es aleatoria (estática de plasma) o coherente (silbidos y tonos). “Contraste” es un exponente sobre el brillo: más alto, solo suena lo que destaca.
- **Cráteres → impactos de polvo:** detectados por diferencia de gaussianas (sin el borde del disco), suenan como racimos de chasquidos de banda ancha con resonancia breve, como los impactos que registró la sonda Stardust en el cometa Tempel 1; más grande el cráter, más impactos y un golpe grave.
- La capa cósmica se reduce a un poco de reverberación: sin acorde, coro ni brillo de octavas, para no maquillar la señal.
- Si se analiza el audio con un espectrograma, aparece la forma del objeto (en la Luna, el disco iluminado).
- Corregido: el parámetro de textura de este modo compartía nombre interno con la “Textura de ruido” del Paisaje sonoro cósmico; ya no existe y el del Paisaje sonoro vuelve a funcionar solo.

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

## Pruebas
- Foto de Venus, Marte, Júpiter y Saturno: los cuatro detectados y descritos (en fase, rojizo, con bandas, con anillos); cada uno, recortado solo, también. Sonido: cada planeta en su tramo y el acorde final; el balance izquierda-derecha oscila al ritmo de la órbita.
- Sin falsos planetas en: Andrómeda, M101, Omega Centauri, Orión, Carina, nebulosa del Cangrejo, cometas (Tsuchinshan-ATLAS y otros), Soles (luz blanca, H-alfa, AIA 304, eclipses y fases) y la Luna.

## Pruebas
- Seis fotos de cometas: núcleo y dirección de la cola correctos en cinco de forma automática (Tsuchinshan-ATLAS, Leonard y tres más); en la del follaje iluminado, marcando el núcleo a mano la cola sale en la dirección exacta. Se recomienda Cometas en cuatro (Leonard queda como “posible”, por su cola corta y tenue).
- Nueve fotos que no son cometas (Andrómeda, Orión, Carina, M101, nebulosa anular, Omega Centauri, dos de la Luna, nebulosa sintética): ninguna se confunde con un cometa. Omega Centauri ya no se recomienda como Sol.
- Sonido: la envolvente late al ritmo del pulso (componente seis veces mayor que en otra frecuencia); 35 a 48 gorjeos; sin valores inválidos.

## Pruebas
- Foto de cerca de la Luna (Platón, Alpes, Cassini; sin cielo): antes recomendaba Barrido tonal y no detectaba cráteres en el mar; ahora Luna y planetas, luz desde la izquierda, Platón, Cassini y los cráteres del mar liso detectados. Luna en cuarto: luz desde la derecha, cráteres a lo largo del terminador.

## Pruebas
- H-alfa fondo claro: disco exacto (radio 0,441), 8 manchas, 7 filamentos, 2 regiones brillantes y 10 protuberancias oscuras; todos suenan. H-alfa invertido sobre negro: 10 manchas, 16 filamentos (brillantes), 5 regiones brillantes, 4 protuberancias. AIA 304 Å: antes 0 detalles; ahora 36 filamentos y 45 regiones brillantes, y suenan todos (antes el recorrido se recortaba a 54°). H-alfa fondo amarillo: radio 0,452 sobre el limbo, 12 manchas, 11 filamentos y 23 regiones brillantes. Sol real sin manchas (NASA, 2019): ningún detalle falso.

## Pruebas
- Foto real del Sol en H-alfa con fondo amarillo (2048 × 2012): antes no se detectaba el disco y se recomendaba Paisaje sonoro; ahora se recomienda Sol y eclipses, el círculo coincide con el limbo y se detectan 24 zonas oscuras (regiones activas y filamentos) y 23 regiones brillantes, que suenan cuando el radar pasa sobre ellas. Las demás fotos del Sol, eclipses, Luna y Andrómeda mantienen su detección.

## Pruebas
- Medias lunas delgadas sintéticas en foto vertical con resplandor (Luna tapando cerca de 75 % y 85 %): reconocidas como eclipse; arcos de 189° y 174°; altura 15 %; en el video horizontal la foto se gira y el brazo recorre solo la parte iluminada.

## Pruebas
- Media luna sintética: arco visible de 210° (de 153° a 3°); el brazo no pasa por la Luna. Eclipse parcial con manchas: arco de 241° y sus 2 manchas visibles. Sol naranja con iluminación 2,5 veces desigual y 10 manchas: tratado como disco completo, 9 manchas detectadas (5 en el lado oscuro) y las 9 suenan. Sol en luz blanca: disco completo, 8 grupos.
- Reproducir desde Análisis: cambia a Composición y suena.

## Pruebas
- Altura automática: luz blanca 8 %, coronógrafo 49 %, eclipse total 88 %.
- Sol gris en luz blanca con dos franjas de manchas y textos: 8 grupos marcados al cargar, textos ignorados, sin estrellas falsas sobre el disco; con disco manual, las mismas 8 manchas.

## Pruebas
- Sol sintético con 5 manchas de tamaño conocido: las 5 detectadas en sus ángulos exactos (56°, 66°, 122°, 190°, 238°) y la mayor identificada como tal. Eclipse parcial sintético: solo las 2 manchas visibles. Sol sin manchas de la NASA (2019): ninguna mancha. Foto de la NASA con un grupo de manchas y un recuadro de ampliación: suenan las manchas reales; las del recuadro (y algún cruce de sus líneas) también, y pueden ignorarse con el pincel. Eclipses totales: sin manchas, como corresponde.
- Recomendación: Sol con manchas, eclipse parcial, Sol gris sin manchas, coronógrafo y eclipse total → Sol y eclipses; Luna → Luna y planetas.

## Pruebas
- Ajuste inicial con márgenes simétricos en computadora y celular; el zoom se respeta al cambiar el tamaño de la ventana; “Ajustar” lo restablece.
- Celular (gestos táctiles reales): un dedo desplazó la página sin mover la foto; dos dedos ampliaron de 0,43 a 1,47.
- “Generar video” sin sonido previo: generó sonido y video en un solo paso.
- Alto de la página en el celular tras cargar una foto: unos 3400 px (antes unos 5400).

## Pruebas
- Diales (gestos táctiles reales por CDP): deslizar en vertical encima del volumen no lo cambió (0,8 → 0,8); arrastrarlo en horizontal sí (0,8 → 0,73).
- Pincel en el celular: pintó 8,8 % de la imagen; al desplazarse se desactivó y conservó la zona; un toque posterior no pintó.
- Recomendación en 12 fotos: Andrómeda, M101 y M31 → Radar; Sol, eclipses y disco en fase → Sol y eclipses; Luna → Superficies; nebulosa anular → Onda; Carina → Orquesta; Orión → Paisaje sonoro.

## Pruebas
- Disco: eclipses en fase sintéticos (40 % y media luna fina) y disco cortado, centro y radio a menos del 1 %; foto de coronógrafo y eclipse de la NASA sin cambios; nebulosa, Andrómeda y Luna sin sugerencia del modo Sol.
- Edición directa: mover, cambiar radio con la perilla y 3 puntos (resultado exacto); en el celular la vista previa sube sola y la miniatura se actualiza con el radio.

## Pruebas
- Fotos de usuarios: Andrómeda con resplandor (0 → 2186 fuentes con 8 σ; título y pie de foto ignorados solos) y Sol en coronógrafo (disco detectado sobre el borde real; 9 protuberancias, entre ellas la grande a unos 59° con 49 % del radio; rótulo y créditos ignorados solos).
- Eclipse total de la NASA: disco oscuro detectado. Seis fotos sin Sol (nebulosas, cúmulo, Luna): ningún disco falso.
- Textos: cero falsos positivos en ocho fotos sin texto.

## Pruebas
- Superficies con la Luna: síntesis de 30 s en unos 2 s; estéreo casi independiente (correlación 0,01 a 0,09); 90 impactos. El espectrograma del audio reproduce el disco lunar.
- El Paisaje sonoro cósmico conserva su control “Textura de ruido” (40 %).

## Pruebas
- Detección de textos: encontró los textos sobreimpresos en dos imágenes de prueba (sobre cielo oscuro y sobre la Nebulosa de Carina) y ninguno en ocho fotos sin texto (incluida la Luna, cuyo terminador daba falsos positivos antes del ajuste). Tras marcarlos, las fuentes detectadas bajaron de 408 a 246 porque las letras ya no cuentan como estrellas.
- Pincel: pinta y borra sin mover la vista; la máscara se aplica y regenera el análisis.
- Superficies con los tres caracteres sobre la Luna: sin valores inválidos; espectrogramas con la banda de radio, estática, silbido y pitidos.

## Pruebas de la versión protegida
- Mismos resultados que la versión legible (pico, volumen y estéreo idénticos) en los ocho modos y en ambos estilos; tiempos de síntesis iguales tras dejar sin codificar los textos del motor de audio.
- Video vertical Full HD, celular (ventana flotante, lupa, umbral automático, barra de progreso) y modos Radar, Onda, Orquesta y Superficies: sin errores en la consola.

## Pruebas
- Foto real de la Luna (Artemis II, 1280 × 853): 90 cráteres detectados sobre la superficie, ninguno en el borde del disco tras el filtro; rugosidad mayor en la zona sur, la más craterizada. Síntesis de 30 s en 3,5 s; recorridos horizontal y vertical correctos.
- Contador de visitas: no se agregó. GitHub Pages no tiene servidor y las estadísticas de tráfico de GitHub cuentan visitas al repositorio, no al sitio publicado, así que un contador necesita un servicio externo.

## Pruebas
- Celular: al mover los deslizadores del centro manual apareció la lupa (195 × 195 px) centrada en 40,0 % y 43,0 %; un toque cambió el aumento de ×6 a ×12; volvió a la vista normal 2,5 s después de soltar; con 🔍 quedó fija. Arrastrar la ventana sigue funcionando y no cambia el aumento.

## Pruebas
- Celular: con la vista principal alejada, la miniatura siguió mostrando la imagen completa; ⊡ restauró el zoom ajustado; arrastrarla la movió a la posición indicada.
- Créditos de fotografía y sonificación al mismo tamaño y altura en vertical y horizontal; sin autor, solo el de sonificación.

## Pruebas
- Escritorio: resolución inicial 1920 × 1080; la miniatura flotante no aparece.
- Celular (390 px): la miniatura aparece al bajar al paso 3, se actualizó tras la regeneración automática al cambiar de modo, reproduce y pausa desde su botón y desaparece al volver a la vista completa. Vertical inicia en 1080 × 1920.

## Pruebas
- Nebulosa anular sintética descentrada (centro real en 40 %, 43 %): el centro de la luz se ubicó en 40,4 %, 43,3 % (página) y 40,3 %, 43,2 % (síntesis). Un toque en la vista Análisis fijó el centro manual en 62 %, 71 % y la onda se regeneró desde ahí.
- Créditos con y sin autores en horizontal y vertical, y tarjeta final; sin errores en la consola.

## Pruebas
- Radar, Onda expansiva y Orquesta: síntesis de 30 s en 3 a 4 s, sin valores inválidos; cuadros correctos en horizontal y vertical (también con la imagen girada).
- Corregido: tras la regeneración automática ya no queda el aviso “La previsualización aparece aquí después del paso 4” encima de la composición.

## Pruebas
- Cuatro modos en estilo cósmico y fiel: sin valores inválidos, pico en el volumen elegido. Correlación entre canales de 0,05 a 0,41 en cósmico (estéreo amplio) frente a 0,82 a 0,93 en fiel. Síntesis de 30 s en 3 a 6 s en computadora.
- Espectrograma de RGB: lecho continuo, notas deslizadas sin vibrato, destellos y colas largas.

## Pruebas
- Estilo cinematográfico en los cuatro modos: sin valores inválidos, pico en el volumen elegido, síntesis de 30 s en 2 a 3 s en computadora.
- Cambio de volumen durante la reproducción: aviso, regeneración y reproducción retomada. Cambio de umbral: aviso y número de fuentes. Cambio de estilo: los controles de espacio y fondo se ocultan en “Fiel a los datos”.

## Pruebas
- WebKit 26 (motor de Safari) con emulación de iPhone 13: carga, análisis, detección, síntesis y reproducción correctas; aviso de “no se está ejecutando” visible con JavaScript desactivado. La exportación de video no se pudo probar ahí: el WebKit de Linux no trae codificadores de video ni MediaRecorder, y su `VideoEncoder.isConfigSupported` cierra la pestaña (fallo del entorno de pruebas, no de iOS).
- Chrome: sin cambios en el comportamiento; video vertical exportado sin errores.

## Pruebas
- Vertical a pantalla completa con imágenes 9:16, 5:8, vertical de campo estelar y horizontal (girada y sin girar), en los cuatro modos: imagen completa sin recortes, título y datos legibles. Video vertical de 10 s exportado sin errores.

## Pruebas (vista de celular, 390 px)
- Imagen sin fuentes: aparece la oferta en el paso 1; con el modo de estrellas, también en el paso 4. Al aceptar, el umbral bajó de 8 a 2 σ, se detectaron fuentes y se generó el audio del modo de estrellas.
- Durante la exportación del video la barra inferior mostró el porcentaje y el cuadro en curso; al terminar mostró “Video listo” y “Ver video” llevó al video.

## Pruebas
- Imagen vertical en video horizontal (cuatro modos) e imagen horizontal en video vertical: giro correcto, imagen completa, guía y marcadores en su instante, etiquetas derechas. Con “No girar” se ve como antes.
- Video horizontal de 6 s con la imagen girada: WebM VP9 + Opus sin errores.

## Pruebas (Google Chrome sin interfaz)
- Recorrido completo de los seis pasos: carga, cambio de modo, audio, cambio a vertical (la vista previa pasa sola a la composición 720 × 1280) y video WebM VP9 + Opus de 10 s sin errores en la consola.
- Firma visible y legible en ambas orientaciones sin tapar la imagen ni los datos.
- Vista de celular (390 px): la vista previa queda arriba y los pasos debajo, en una columna.

## Pruebas (Google Chrome sin interfaz, imagen sintética de 1500 × 1000)
- Los cuatro modos en vertical y horizontal: cuadros revisados al 40 %, 50 % y 98 % de la duración. El horizontal se ve igual que antes.
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
