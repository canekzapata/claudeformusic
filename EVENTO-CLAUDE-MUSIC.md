# Visuales que reaccionan a la música: de la escalera de herramientas a Hydra

Guion y guía para el evento de Claude (código y música, México). Dos partes:

1. **Una presentación corta** que baja por nivel de complejidad hasta Hydra y explica Traspuesta como un JS de arte adaptado a Hydra.
2. **Una guía para leer el trabajo de otra persona** (imágenes, GIF, video, JS, p5, 3D, renders de arquitectura) y decidir qué se puede conectar a Hydra para que reaccione a la música.

Escrito el 28 de octubre de 2026. Lo que dice «probado» se probó el día de hoy en Chrome con la copia local de Hydra (`vendor/hydra-synth.js`), con `experimentos/fuentes-hydra.html`. Lo que dice «sin probar» es lo que sé por la documentación o por la plataforma web, no por haberlo corrido aquí.

---

## Parte 1 · La presentación (unos 8 minutos)

### La idea en una frase

> Casi nada de lo que ves en un concierto grande reacciona a la música «de verdad». Lo que sí puedes hacer con código es una cosa más pequeña y más viva: que la imagen **escuche**.

### La escalera, de arriba (más caro y más seguro) a abajo (más barato y más vivo)

| Nivel | Cómo sabe qué hacer | Quién lo usa | Qué aprendes |
|---|---|---|---|
| 0. Timecode / clic | La pista manda LTC, MIDI o MTC a la consola de luces y al servidor de video. Todo cae exacto en el compás, todas las noches. | Giras grandes, estadios | **No es reactivo: es una partitura.** |
| 1. Disparos a mano | Un operador toca cues y faders siguiendo a la banda. | Festivales, bandas que improvisan | El humano también es un sensor. |
| 2. Sensores y tracking | MIDI de los instrumentos, OSC, cámaras que siguen el cuerpo. | Instalaciones, shows con tracking | La imagen sigue **a una persona**, no al sonido. |
| 3. Análisis de audio | FFT, detección de golpes y envolventes sacadas de la mezcla. | VJs, live coding | Sirve para **modular** (brillo, glitch, escala), no para estructurar. |
| 4. Código que escucha | Un programa que recibe esas señales y dibuja. | Hydra, p5, GLSL, TouchDesigner, Notch | **Tú escribes qué significa «escuchar».** |

Herramientas por nivel (para decirlas en voz alta, no para explicarlas):
- **Video y clips:** Resolume Arena, VDMX, Modul8.
- **Generativo:** TouchDesigner, Notch, Unreal, vvvv, Max/Jitter.
- **Servidores de gira:** disguise, Pixera, Hippotizer.
- **Con código, en el navegador:** Hydra, p5.js, GLSL.
- **Luces:** grandMA3, ChamSys, Avolites; por red, Art-Net y sACN.
- **Protocolos que lo unen:** MIDI/MTC, OSC, Ableton Link, NDI/Spout/Syphon.

### Por qué Hydra, y qué es

Hydra es un sintetizador de video que vive en el navegador y se escribe en una línea:

```js
osc(10, 0.1, 1).kaleid(5).out()
```

- Cada paso (`osc`, `kaleid`, `modulate`) es un filtro encadenado; el resultado se calcula en la tarjeta de video, a tiempo real.
- Se **edita en vivo**: cambias la línea y la imagen cambia, sin compilar. Por eso el «código en pantalla» ya es parte del espectáculo.
- Las perillas pueden ser **funciones**: `() => a.fft[0] * 0.2` vuelve la perilla un cable al audio.
- Lo más importante para hoy: **Hydra no sólo genera, también transforma.** Le puedes dar una imagen, un video o un lienzo de otro programa (`s0`) y tratarlo.

### Traspuesta: un JS de arte adaptado a Hydra

Traspuesta empezó siendo otra cosa: un generador de láminas de montañas para plotter. No se hizo pensando en Hydra. Se **adaptó** en tres pasos, y esos tres pasos son lo que se puede repetir con la obra de cualquiera:

1. **La pieza dibuja en un `<canvas>`.** El visor `musica.html` pinta en un lienzo y no sabe nada de Hydra.
2. **El lienzo entra a Hydra como `s0`.** `s0.init({ src: lienzo, dynamic: true })`. Hydra ve cada cuadro y lo trata; el resultado va en un segundo lienzo encima, con una **mezcla de 0 a 1** (a 0, Hydra ni se calcula).
3. **El audio entra como `a.fft[0..3]`**: graves, medios, agudos y golpe. Esos números salen de una FFT propia del visor, no de la de Hydra, y se normalizan solos (control automático de ganancia) para que no dependan del mixer.

Y encima, lo que hace que se sienta una pieza y no una demo:
- **Capas de la misma obra como fuentes extra:** `s1` es el mapa de alturas de la montaña, `s2` y `s3` son láminas dibujadas por el motor. Hydra lee la montaña, no sólo su dibujo.
- **Un editor a la vista** (`Q`) con `Ctrl+Enter`; si el código no compila, se queda el anterior.
- **Presets** (más de 35) que se eligen con el teclado o por coreografía.
- **Un actor de texto** que escribe, se equivoca y se corrige, y que teclea el código que se edita.

Para ver más: [HYDRA.md](HYDRA.md) (todo lo que se ha usado) y [TRASPUESTA-V2.md](TRASPUESTA-V2.md).

### Que se pueda regenerar en la computadora de otro

Esto es lo que hace que sea una pieza y no una grabación. Se logra con cinco decisiones:

- **Todo local.** Hydra va copiado en `vendor/` (sin CDN, funciona sin internet).
- **Un clic:** `traspuesta-suena.bat` prende un servidor local y abre Chrome. (El micrófono sólo se puede usar desde `localhost` o `https`.)
- **Semilla = obra.** Misma semilla, misma montaña; la semilla siempre se ve en pantalla.
- **Una salida de emergencia:** un modo «sin audio» (un demo a 124 bpm) para cuando falla la entrada.
- **Reloj manual** (`?reloj=manual`): el tiempo avanza por pasos, así los videos y las capturas salen iguales cada vez.

### Guion de diapositivas

| # | Diapositiva | Qué se muestra | Tiempo |
|---|---|---|---|
| 1 | «La imagen que escucha» | Una montaña de Traspuesta moviéndose con música real | 0:30 |
| 2 | La escalera | La tabla de arriba, nivel 0 a 4 | 1:30 |
| 3 | Lo que casi nadie dice | Giras grandes = partitura, no reacción | 0:45 |
| 4 | Hydra en una línea | `osc(10,0.1,1).kaleid(5).out()` en vivo | 1:00 |
| 5 | El cable al audio | `() => a.fft[0]` cambiando una perilla | 1:00 |
| 6 | Traspuesta: tres pasos | Lienzo → `s0` → `a.fft`, con el mezclador de 0 a 1 | 1:30 |
| 7 | Y tu obra | La tabla de la parte 2: qué se puede conectar | 1:00 |
| 8 | «Tráeme tu carpeta» | La lista de preguntas | 0:45 |

---

## Parte 2 · Cómo leer el trabajo de alguien para conectarlo a Hydra

### Una idea para guiar la búsqueda

Hydra entiende **una cosa**: una textura que cambia con el tiempo. Así que la pregunta nunca es «¿esto es compatible con Hydra?» sino:

> **¿Puedo sacar de esta obra un lienzo, un video o una capa que valga la pena mover con música?**

Y la mejor respuesta casi siempre es **más de una capa de la misma obra** (color, profundidad, líneas, máscara), porque es lo que dejó de ser un filtro y se volvió una pieza en Traspuesta.

### Qué buscar y cómo entra

| Qué tiene la persona | Cómo entra a Hydra | Estado | Trampas |
|---|---|---|---|
| **Imágenes fijas** (PNG, JPG, render, foto, dibujo) | `s0.initImage(ruta)` | Sin probar hoy (la ruta es la misma que la del video; Traspuesta entra sus mapas como lienzo) | Que sean locales. Con fondo transparente, lo más probable es que se vea sobre negro: sin probar. |
| **GIF** | **Convertirlo a MP4 o WebM** y usar `initVideo` | **Probado: el GIF NO se anima como textura** (queda congelado en un cuadro) | Es la trampa más común. `ffmpeg -i x.gif -movflags faststart -pix_fmt yuv420p x.mp4` |
| **Video** (MP4 H.264) | `s0.initVideo(ruta)` | **Probado:** reproduce, hace loop y cambia cuadro a cuadro | Local, nunca por streaming. Videos oscuros o casi fijos casi no se notan al tratarlos. |
| **JS con `<canvas>`** (arte generativo propio) | `s0.init({ src: canvas, dynamic: true })` | **Probado** (es lo que hace Traspuesta) | Pedir el canvas, no la ventana; que no use el ratón para arrancar. |
| **p5.js** | `s0.init({ src: p5.canvas, dynamic: true })` | **Probado** con p5 1.9.4 | Ocultar el canvas de p5 (`cn.hide()`) y dejar que Hydra lo muestre. |
| **three.js / WebGL** | `s0.init({ src: renderer.domElement, dynamic: true })` | **Probado** con three r128, con y sin `preserveDrawingBuffer` | Si el lienzo sale negro, activar `preserveDrawingBuffer: true`. |
| **Shaders GLSL** | Funciones propias con `setFunction` de Hydra | Sin probar | Hay que traducir el shader a la forma que pide Hydra. |
| **Modelos 3D** (GLB, OBJ, FBX) | Cargarlos en three.js y usar su lienzo | Camino probado (three.js sí entra); modelo real, sin probar | Tamaño de los archivos y texturas pesadas. |
| **Cámara o pantalla en vivo** | `s0.initCam()` / `s0.initScreen()` | Sin probar aquí | Pide permisos; sirve para instalaciones. |
| **Vectores y planos** (SVG, DXF, plantas) | Dibujarlos en un canvas 2D y entrarlo como lienzo | Es lo que hace Traspuesta con sus láminas | Trazos finos: darles grosor antes de tratarlos. |
| **Texto, poemas** | Un actor que escriba en un canvas (como el de Traspuesta) | Hecho en Traspuesta | La tipografía debe viajar con la pieza. |

### Ejemplo: una persona que hace renders de arquitectura

La pregunta es qué **capas** de un render se pueden separar. Casi cualquier motor (Blender, V-Ray, Lumion, Twinmotion, Enscape, Unreal) las puede exportar:

| Capa del render | Qué es | Qué se le puede hacer con la música |
|---|---|---|
| **Color final** | El render tal cual | Base: `src(s0)`; se mueve con eco, rgb, caleidoscopio |
| **Profundidad (Z-pass)** | Gris: cerca = claro, lejos = oscuro | Como el mapa de alturas de Traspuesta: **el agua sube** y se llena lo lejano; `modulate` desplaza el cerca más que el lejos |
| **Normales** | El color es hacia dónde mira cada superficie | Luz que barre al ritmo: lo que mira al lado se enciende con los graves |
| **Líneas / contornos** (Freestyle, aristas) | Sólo el dibujo de la arquitectura, sobre negro | Chispas que sólo nacen en las aristas (el preset «luciérnagas») |
| **Máscaras por material** | Vidrio, concreto, vegetación | El vidrio vibra con los agudos, el concreto con los graves |
| **Plantas y cortes** (vector) | Dibujo técnico | Se trazan con pluma (como las láminas) y entran como capa clara sobre negro |

Tres caminos, del más fácil al más vivo:

1. **Recorrido en video** (turntable, walkthrough): lo más rápido. Un MP4 y listo; se trata con Hydra como cualquier video.
2. **Render por pasadas:** tres o cuatro imágenes o videos de la **misma toma** (color, profundidad, líneas). Hydra los trata juntos; es la ruta de mejor resultado y la que hace que no se vea «un filtro».
3. **Modelo en vivo con three.js:** el modelo se carga en el navegador, la cámara orbita con la música, y su lienzo entra a Hydra. Máxima libertad, y más trabajo de preparación.

Una regla que aprendimos con Traspuesta: **un mapa crudo tapa la obra** (cuando metimos el mapa de alturas a pantalla completa, borró las líneas). Pasa lo mismo con un render completo o un Z-pass: **entran como capas que se suman o se usan de máscara, no como fondo**, y el fondo se queda negro.

### Cómo buscar en la carpeta de la persona: diez preguntas

1. ¿Qué tiene que **se mueva o pueda moverse**? (videos, recorridos, animaciones, código con `draw()`)
2. ¿Tiene los **archivos fuente** o sólo los exportados? Las capas separadas (profundidad, líneas) sólo salen de la fuente.
3. ¿En qué **formato** están? Un GIF se convierte; un `.mov` con alfa se vuelve WebM; una escena de Blender se renderiza por pasadas.
4. ¿Es **código** (JS, p5, three, shaders)? Es lo mejor: se regenera en la computadora de otro y puede leer el audio directo.
5. ¿Qué tan **pesado** es? Un MP4 de 2 GB no cabe en un evento; uno de 20 MB sí. Videos en 1280×720 o 1920×1080.
6. ¿Funciona **sin internet** y sin cuenta? (fuentes web, CDN, modelos en la nube: se descargan y se guardan)
7. ¿De **quién son** las piezas, texturas y modelos? Si usa activos de terceros (Sketchfab, bancos de imágenes), hay que revisar la licencia **antes** de mostrarlos en vivo.
8. ¿La persona **quiere** que se toque su obra? Mejor mostrarle una prueba de 10 minutos; el trabajo es suyo.
9. ¿Qué se vuelve **más interesante con música**? Lo que ya tiene **estructura, repetición o profundidad** (una fachada con ritmo, un recorrido largo, un patrón).
10. ¿Qué **no** conviene tocar? Un logo o un retrato que debe leerse limpio: mejor dejarlo a mezcla baja y mover el fondo.

### La prueba de 10 minutos

Con una sola imagen o video de la persona:

```js
s0.initImage('mi-render.png')          // o s0.initVideo('mi-recorrido.mp4')
src(s0)
  .modulate(noise(3, 0.1), () => a.fft[0] * 0.1)   // los graves la deforman
  .hue(() => a.fft[2] * 0.5)                        // los agudos giran el color
  .out()
```

Si te gusta, subes un escalón: una segunda capa (profundidad o líneas) como `s1`, o una pasada con `diff`. Si no te gusta, es una señal buena: esa obra pide otro tratamiento, no menos.

---

## Qué se probó hoy y qué falta

Prueba: `experimentos/fuentes-hydra.html`, Chrome, copia local de Hydra, una fuente por vez, midiendo cuánto cambia la imagen de Hydra entre cuadros separados por ~1.2 s.

| Fuente | Resultado |
|---|---|
| Canvas 2D dibujando | Se mueve (cambio 14.1) |
| GIF animado con `initImage` | **No se anima** (cambio 0; tampoco dibujándolo directo en un canvas 2D) |
| MP4 con `initVideo` | Se mueve; el reloj del video avanza (2.1 s → 4.2 s) y el cambio llega a 84 con un video muy activo; con un video oscuro y casi fijo, 0.7 |
| Canvas de p5.js | Se mueve (5.9) |
| Canvas de three.js, con y sin `preserveDrawingBuffer` | Se mueve (5.1 / 4.4) |

**Sin probar:** shaders GLSL con `setFunction`, modelos GLB reales, `initCam` e `initScreen`, imágenes remotas con CORS, el editor en línea de Hydra (`hydra.ojack.xyz`), y un render arquitectónico real por pasadas. Esos son los siguientes pasos naturales si se quiere ofrecer una demo con la obra de alguien.

**Licencia:** Hydra es AGPL-3.0 (`vendor/HYDRA-LICENSE`). Si se publica una pieza que lo incluya, hay que ofrecer su código fuente.

## Lo que sigue (si se quiere llevar esto más lejos)

- Un **paquete de entrega** por persona: carpeta con su material convertido (MP4, capas), un `README`, la prueba de 10 minutos y el `.bat`.
- Un **plantilla de pieza** vacía: `musica.html` sin Traspuesta, con el lienzo, `s0`, `a.fft`, mezcla y editor, para que cualquiera meta su `draw()`.
- Un **puente OSC o MIDI** (Web MIDI) para disparar presets desde un teclado o un pedal, y **Ableton Link** para el tempo compartido.
- Si no hay entrada de audio fiable: pedir la salida estéreo (aux) de la consola, o poner un micrófono de ambiente.
