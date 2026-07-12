# app_leer

# Bitácora de desarrollo — ReadBok

**Proyecto:** Prototipo navegable de app de hábito de lectura "ReadBok"
**Materia:** Diseño de Interacción — 8.º ciclo
**Autores del diseño:** Oscar Duchitanga · André Ortiz
**Herramienta de desarrollo asistido:** Claude Code (agente de IA)
**Stack:** HTML + CSS + JavaScript puro · VS Code + Live Server
**Fuente de diseño:** `Entrega 1.pdf` (tareas principales, mapa de viaje, wireframes, guía de estilo y mockups en Illustrator)
**Fechas de trabajo:** 11 y 12 de julio de 2026

---

## Metodología

El desarrollo se realizó de forma iterativa mediante prompts a un agente de IA:

1. Se entregó el PDF de diseño como única fuente de verdad (guía de estilo y mockups).
2. Cada pantalla se construyó, se revisó contra el mockup con capturas comparativas y se corrigió **un cambio a la vez**.
3. Cada iteración fue verificada en navegador (medidas computadas, simulación de interacciones y detección de desbordes) antes de darse por cerrada.

A continuación se documenta cada iteración con su **prompt técnica** (redacción refinada del requerimiento original) y su **resultado**.

---

## Fase 1 — Definición y arranque

### Prompt 01 — Definición del enfoque técnico

> Necesito desarrollar el entregable de una app móvil de lectura para la materia de Diseño de Interacción. Ya cuento con wireframes y guía de estilo. Recomiéndame el proceso de desarrollo y el software adecuado, considerando que: (a) el entregable es un **prototipo navegable**, no una app nativa en producción; (b) debe ejecutarse **en el navegador** el día de la presentación; y (c) mi flujo de trabajo actual es VS Code con Live Server.

**Resultado:** Se definió el stack HTML/CSS/JS puro, visualización con Live Server y modo dispositivo del navegador (F12), y un proceso incremental: tokens de estilo → una pantalla patrón → réplica al resto → enlazado de la navegación.

### Prompt 02 — Ingesta del documento de diseño

> Analiza el documento `Entrega 1.pdf` (lámina única de Illustrator, formato A3) y extrae: las 6 tareas principales del usuario, el mapa de viaje (touchpoints, objetivos y acciones), el inventario de pantallas de los wireframes y mockups, y la guía de estilo completa: paleta (#2c3e50 texto, #9e2a2b énfasis, #df9f3e corporativo, #ffffff fondo), tipografía (Montserrat, jerarquía H1 20 → H6 9, con pesos Bold/SemiBold/Light), iconografía, y el sistema de botones con sus cuatro estados (default, hover, pressed, disabled).

**Resultado:** Inventario de 9 pantallas identificadas: login, inicio/dashboard, configurar meta, carga, sesión diaria, historial, logros, insignias y tonos. Tokens de diseño documentados para su implementación como custom properties de CSS.

### Prompt 03 — Arquitectura base y primeras pantallas

> Crea la estructura del proyecto en la carpeta `app_leer` (repositorio git ya inicializado): `assets/css/variables.css` con los tokens de la guía de estilo como custom properties; `assets/css/base.css` con los componentes compartidos (marco de teléfono con bisel y esquinas redondeadas que desaparece bajo 480px para el modo dispositivo, barra de estado con hora 02:47 PM e iconos de señal/wifi, navegación inferior de 4 iconos SVG con estado activo en énfasis, sistema de botones primario/acento/secundario/destructivo con estados hover-pressed-disabled). Implementa las dos primeras pantallas: `index.html` (login: wordmark ReadBok, botones "Iniciar sesión" y "Crear cuenta", enlace "Salir") e `inicio.html` (dashboard: chip de saludo, icono de libro abierto con porcentaje de avance, tarjetas de racha y libro en curso, CTA "Ir a leer").

**Resultado:** Estructura creada y pantallas base funcionando con navegación por anclas entre archivos HTML.

### Prompt 04 — Réplica del patrón a las 7 pantallas restantes

> Replica el patrón aprobado (marco + barra de estado + nav inferior) en las pantallas restantes, cada una fiel a su mockup: `meta.html` (chips de tiempo 10/15/20/Otro con selección única en JS), `cargando.html` (pantalla de transición con redirección automática a los 2.2 s), `sesion.html` (cronómetro funcional en formato mm:ss,cc con pausa/reanudar), `historial.html` (minutos acumulados, carrusel de libros leídos, gráfica de barras semanal con línea de meta punteada en SVG), `logros.html` (insignias, metas cumplidas con estado "Completado", accesos a tonos), `insignias.html` y `tonos.html` (cuadrículas 3×3 de desbloqueados con selección en énfasis + 2×3 de bloqueados con candado). Conecta el flujo completo: Ir a leer → carga → sesión → guardar → inicio.

**Resultado:** Prototipo navegable de punta a punta con las 9 pantallas, verificado sin errores de consola y con las interacciones de selección y cronómetro operativas.

---

## Fase 2 — Fidelidad visual contra los mockups

### Prompt 05 — Corrección del dashboard (jerarquía y color)

> Compara el dashboard implementado contra el mockup y corrige las desviaciones: (1) el icono del libro debe crecer de 150 px a ~230 px de ancho con trazo más grueso e incluir el relleno de avance sobre la página derecha; (2) la tipografía dentro de las tarjetas naranjas debe ser azul `#2c3e50`, no blanca, con el dato principal en 2.2 rem/800; (3) el "65%" debe subir a ~3.8 rem y pegarse al libro; (4) el contenido debe distribuirse uniformemente en el alto disponible (`justify-content: space-evenly`) eliminando el vacío entre tarjetas y CTA.

**Resultado:** Dashboard corregido; se verificaron por JS los tamaños y colores computados.

### Prompt 06 — Reescalado global del sistema

> El prototipo se ve "pequeño dentro de un teléfono gigante" comparado con los mockups. Corrige a nivel de sistema, no por pantalla: (1) proporción del marco a 390×790 px (formato alargado tipo iPhone) con bisel de 14 px y radio de 52 px; (2) escala tipográfica base de 10 px a 16 px manteniendo la jerarquía relativa H1–H6 de la guía; (3) iconos de navegación a 38 px con trazo 2.5; (4) muesca (notch) en semicírculo colgando del bisel, oculta en modo dispositivo real; (5) eliminar la línea separadora sobre el nav que el mockup no tiene; (6) tarjetas con radio 20 px. Verifica que ninguna de las 9 pantallas desborde verticalmente tras el cambio.

**Resultado:** Sistema reescalado en un solo punto (rem base); verificación automatizada de desborde en las 9 pantallas: 0 px en todas.

### Prompt 07 — Corrección del icono de avance de lectura

> El elemento naranja de la página derecha del libro del dashboard no coincide con el mockup: al ampliar el PDF a 600 dpi se confirma que no es una "media luna" flotante sino el **relleno de avance de lectura**: la página derecha pintada desde el lomo hasta ~40 % de su ancho con borde cóncavo (la zona blanca queda con forma de luna). Redibuja el path SVG para que el relleno nazca bajo el trazo del lomo (sin filos blancos) y recorra la página de arriba a abajo con esa curva cóncava.

**Resultado:** Path corregido (`M75 22 C84 14 93 12 101 12 C84 40 84 64 100 84 C91 82 83 86 75 94 Z`); render comparado lado a lado con el recorte del PDF: coincidente.

---

## Fase 3 — Pantalla de carga y sesión

### Prompt 08 — Loader animado con el icono de marca

> Reemplaza el logotipo "B" dibujado a mano de `cargando.html` por el icono del libro abierto de ReadBok, animado como indicador de carga: rotación 3D continua sobre el eje vertical (`rotateY` 0→360°, 1.5 s, lineal, infinita, con perspectiva en el contenedor). Conserva la redirección automática.

**Resultado:** Loader con el libro girando en 3D.

### Prompt 09 — Simplificación del loader (contorno + color de énfasis)

> Ajusta el libro del loader: solo el contorno (sin rellenos de página ni avance de lectura), en el rojo de énfasis `#9e2a2b` de la guía.

**Resultado:** Libro de carga en contorno rojo; verificado por render a imagen.

### Prompt 10 — Rediseño de la sesión diaria + control del cronómetro

> La pantalla de sesión difiere del mockup y tiene un problema de UX: (1) reorganiza el layout con el reloj grande a la izquierda y el contador `00:00,00` a su derecha (fila, no columna), título y chip "Modificar meta diaria" alineados a la izquierda; (2) redibuja el reloj como aro abierto en el cuadrante superior derecho con manecillas gruesas de extremos rectos (`stroke-linecap: butt`); (3) agranda el botón de pausa (~104 px, aro de 10 px); y (4) **el cronómetro no debe autoiniciarse al entrar**: estado inicial detenido en 00:00,00 con icono ▶, arranque/pausa/reanudación exclusivamente mediante el botón, alternando ▶/❚❚.

**Resultado:** Layout fiel al mockup y cronómetro controlado por botón; comportamiento verificado por simulación de clicks (no autoarranque, conteo, congelado en pausa).

### Prompt 11 — Fix de desborde y peso visual + estados del botón secundario

> Correcciones sobre la sesión: (1) el bloque reloj+contador desborda horizontalmente y genera scrollbar — redimensiona (reloj 155 px, contador 1.8 rem, sin márgenes negativos) para caber en un viewport de 375 px y en el marco de 390 px; (2) reduce el trazo del reloj de 15 a 11 (el grosor actual hace ilegible el aro; el cambio debe ser local a este SVG, no global); (3) define los estados interactivos del botón secundario de contorno rojo en `base.css`: hover con tinte al 8 % del énfasis y `:active` con inversión (fondo énfasis, texto blanco).

**Resultado:** Cero desborde verificado en 375 px y 390 px; estados de botón secundario aplicados a todos los botones de esa clase.

---

## Fase 4 — Historial y logros

### Prompt 12 — Historial: densidad, carrusel táctil y gráfica animada

> Tres correcciones en `historial.html`: (1) elimina el vacío vertical entre secciones (la gráfica estaba empujada al fondo con `margin-top: auto`) y da al carrusel de libros sangrado hasta los bordes de pantalla como en el mockup; (2) oculta la barra de scroll horizontal del carrusel (`scrollbar-width: none` + `::-webkit-scrollbar`) y reemplázala por **arrastre directo** con Pointer Events (mouse y táctil, con cursor grab/grabbing); (3) anima las barras de la gráfica para que **crezcan desde la base al entrar** (transform `scaleY` 0→1, 0.9 s, escalonadas 0.15 s) y completa la gráfica con eje vertical, marcas en la base y etiquetas, según el mockup.

**Resultado:** Sección compacta, arrastre verificado por simulación de puntero (~110 px de desplazamiento), animación y ejes en su lugar.

### Prompt 13 — Logros: carrusel consistente y alturas uniformes

> En `logros.html`: (1) aplica al carrusel "Metas cumplidas" el mismo tratamiento del historial (scrollbar oculta + arrastre por puntero + sangrado a bordes); (2) uniforma la altura de todos los chips de meta fijando una zona de texto de 58 px con centrado flex (el chip "leer 15 min en un día" crecía más que el resto); (3) agrega a las metas pendientes una barra de progreso parcial en la franja inferior, como muestra el mockup.

**Resultado:** Chips de 76 px exactos los cuatro, carrusel arrastrable sin barra, progreso al 65 % y 40 % en las metas pendientes.

### Prompt 14 — Color semántico de la barra de progreso

> Cambia el color de la barra de progreso de las metas pendientes al naranja corporativo `#df9f3e` para diferenciarla del chip azul: en esta pantalla el naranja codifica "logro" (los chips completados son naranjas), mientras el rojo del sistema codifica estado activo/seleccionado.

**Resultado:** Barra de progreso en corporativo, verificado el color computado.

---

## Fase 5 — Navegación, sonido y estados

### Prompt 15 — Flecha de retorno en subpantallas

> Las pantallas `insignias.html` y `tonos.html` (hijas de Logros vía "Ver más") no tienen salida: agrega un patrón de encabezado con flecha de regreso (`←` SVG de 28 px alineada verticalmente con el título, enlace a `logros.html`, hover aclarado y `:active` en énfasis) como componente reutilizable en `base.css`.

**Resultado:** Flecha centrada con el título (diferencia de centros: 0 px) en ambas subpantallas.

### Prompt 16 — Tonos audibles con síntesis de audio

> Haz que los 9 tonos desbloqueados de `tonos.html` **suenen de verdad** al seleccionarse, sin archivos de audio: sintetiza cada melodía con Web Audio API (osciladores tipo triángulo con envolvente de ganancia para evitar clicks), definiendo cada tono como secuencia de `[frecuencia, duración, pausa]` con carácter propio (campanita descendente, ascendente, doble bip, trino, alterno, cascada, arpegio, timbre, juguetón). La reproducción acompaña a la selección visual; los bloqueados permanecen mudos.

**Resultado:** 9 melodías sintetizadas; AudioContext verificado en estado `running` sin errores tras probar los 9 botones.

### Prompt 17 — Estado visual de elementos bloqueados

> Los candados de insignias/tonos bloqueados se confunden con los elementos activos e incluso reaccionan al hover. Defínelos como no interactivos: opacidad 0.4, cursor default y sin cambio de color al hover, sobre la clase compartida para que aplique a ambas pantallas.

**Resultado:** Bloqueados atenuados y sin reacción, verificado por estilos computados.

---

## Fase 6 — Recordatorios y flujo de usuario nuevo

### Prompt 18 — Módulo de recordatorios (diseño libre dentro de la guía)

> Resuelve el apartado "Configurar recordatorio" de `meta.html` con libertad de diseño pero estricto apego a la guía de estilo: convierte el enlace en un desplegable (el icono + rota 45° a × al abrir) que revela un panel con selector de hora y botón "Agregar" (estilo acento naranja); cada recordatorio guardado se lista con icono de despertador, hora en formato 12 h AM/PM (consistente con la barra de estado) y botón "Eliminar" destructivo rojo de la guía. Incluye un recordatorio de ejemplo para que la demo no arranque vacía.

**Resultado:** Panel desplegable funcional con alta/baja de recordatorios, verificado por simulación (agregar 07:30 AM, eliminar, plegar).

### Prompt 19 — Flujo de onboarding y perfiles de demo

> Reordena el flujo pensando en un usuario completamente nuevo y habilita dos perfiles de demostración persistentes durante la sesión de navegación (`sessionStorage`): (1) **"Crear cuenta"** → pantalla de carga → **Configurar meta** (onboarding) → dashboard **en cero**: saludo genérico "Hola lector", 0 %, libro sin relleno de avance, racha 0 días, sin libro en curso; historial en cero (0 min, sin libros terminados, gráfica solo con ejes); (2) **"Iniciar sesión"** → carga → dashboard de Omar con todos los datos. Parametriza `cargando.html` con `?siguiente=` para reutilizar el loader en ambos caminos.

**Resultado:** Ambos flujos verificados: perfil nuevo con todas las métricas en cero y perfil Omar intacto; el loader redirige según el parámetro recibido.

---

## Estado final del prototipo

| Pantalla         | Interacciones funcionales                                             |
| ---------------- | --------------------------------------------------------------------- |
| `index.html`     | Login con dos perfiles de demo (nuevo / Omar)                         |
| `cargando.html`  | Libro de marca girando en 3D, redirección parametrizada               |
| `meta.html`      | Selección de tiempo, recordatorios con alta/baja, desplegable animado |
| `inicio.html`    | Dashboard con datos según perfil, avance dibujado en el libro         |
| `sesion.html`    | Cronómetro real: iniciar/pausar/reanudar/guardar                      |
| `historial.html` | Barras animadas al entrar, carrusel arrastrable, datos según perfil   |
| `logros.html`    | Metas con progreso, carrusel arrastrable                              |
| `insignias.html` | Selección de insignia activa, flecha de retorno                       |
| `tonos.html`     | 9 tonos audibles (Web Audio API), selección, flecha de retorno        |

**Pendientes:** conexión del repositorio local con GitHub (remoto) y commit del prototipo.
