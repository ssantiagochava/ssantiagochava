<div align="center">

<img src="docs/screenshots/inicio-hero.png" alt="ToolKar — portada" width="100%">

# ToolKar

**Sitio web cinemático para un taller de mecánica de alta gama**

Landing con animaciones de scroll · Catálogo de servicios · Formulario de solicitud · Tablero kanban

[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)](#)
[![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white)](#)
[![JavaScript](https://img.shields.io/badge/JavaScript-ES2020-F7DF1E?style=flat-square&logo=javascript&logoColor=black)](#)
[![GSAP](https://img.shields.io/badge/GSAP-3.12-88CE02?style=flat-square&logo=greensock&logoColor=white)](https://gsap.com)
[![Lenis](https://img.shields.io/badge/Lenis-1.3-000000?style=flat-square)](https://lenis.darkroom.engineering)
[![Sin build](https://img.shields.io/badge/build-ninguno-2ea44f?style=flat-square)](#-cómo-ejecutarlo)
[![Pruebas](https://img.shields.io/badge/pruebas-12%20p%C3%A1ginas%20%C2%B7%200%20fallos-2ea44f?style=flat-square)](#-pruebas)
[![Licencia MIT](https://img.shields.io/badge/licencia-MIT-blue?style=flat-square)](LICENSE)

[Demo](#-vista-previa) · [Características](#-características) · [Ejecutar](#-cómo-ejecutarlo) · [Arquitectura](#-arquitectura) · [Pruebas](#-pruebas) · [Decisiones](#-decisiones-de-diseño)

</div>

---

## 📌 Sobre el proyecto

**ToolKar** es un taller especializado en Porsche, Maserati, Audi, Mercedes-Benz, Land Rover, Jaguar y Lexus. Este repositorio contiene su sitio web completo: una **landing cinemática** donde las imágenes se expanden suavemente al hacer scroll, un **catálogo filtrable** de 15 servicios con secciones por marca, un **formulario de solicitud** con validación en vivo y un **tablero kanban** para que el equipo gestione las citas.

Todo está hecho con **HTML, CSS y JavaScript planos — sin frameworks ni build**: el sitio se abre con doble clic en `index.html`. Las animaciones usan GSAP + ScrollTrigger y Lenis; las imágenes se generaron con IA (Higgsfield) bajo una misma dirección de arte ("showroom nocturno").

Proyecto desarrollado para el curso **Ingeniería de Software II** (Universidad Latina de Costa Rica).

## ✨ Características

| | |
|---|---|
| 🎬 **Hero expandible** | La imagen del taller empieza enmarcada y crece hasta llenar la pantalla mientras el título se disuelve (`ScrollTrigger` con pin y `clip-path`). |
| 🏎️ **Siete marcas, siete escenas** | Cada marca tiene su propia sección con imagen que se expande al llegar al centro del viewport. |
| 🧭 **Scroll suave** | Lenis sincronizado con el reloj de GSAP; el header se esconde al bajar y reaparece al subir. |
| 🔎 **Catálogo filtrable** | 15 servicios en 5 categorías; los filtros animan la salida y entrada de tarjetas y no pierden clics rápidos. |
| 📝 **Formulario inteligente** | Se preselecciona desde la URL (`?servicio=`, `?marca=`), resumen en vivo, validación por campo con `aria-invalid`, confirmación animada. |
| 📋 **Tablero kanban** | Cuatro estados, arrastrar y soltar o `<select>`, las tarjetas *vuelan* entre columnas con GSAP Flip, datos de ejemplo con un clic, pestañas en móvil. |
| 💾 **Sin backend** | Las solicitudes persisten en `localStorage` con una forma estable que comparten formulario y tablero. |
| ♿ **Accesible** | Respeta `prefers-reduced-motion` (sin pin ni scroll suave, todo visible), foco visible, patrón ARIA en pestañas y diálogo, contraste AA. |
| 🛡️ **Robusto** | Respaldo local de las librerías si el CDN falla, escape de HTML en todo lo que viene del usuario, 78 aserciones de prueba en navegador. |

## 🖼️ Vista previa

<table>
  <tr>
    <td width="50%"><img src="docs/screenshots/listado.png" alt="Catálogo de servicios"><br><sub><b>Servicios</b> — hero con parallax y filtros sticky</sub></td>
    <td width="50%"><img src="docs/screenshots/formulario.png" alt="Formulario de solicitud"><br><sub><b>Solicitud</b> — columna fija con resumen en vivo</sub></td>
  </tr>
  <tr>
    <td width="50%"><img src="docs/screenshots/dashboard.png" alt="Tablero kanban"><br><sub><b>Tablero</b> — kanban con Flip y datos de ejemplo</sub></td>
    <td width="50%"><img src="docs/screenshots/marca-jaguar.jpg" alt="Sección de marca Jaguar"><br><sub><b>Marcas</b> — una de las 12 imágenes generadas con IA</sub></td>
  </tr>
</table>

<div align="center">
  <img src="docs/screenshots/inicio-movil.png" alt="Inicio en móvil" width="260">
  <br><sub><b>Móvil</b> — header de dos filas y hero adaptado</sub>
</div>

## 🚀 Cómo ejecutarlo

No hay nada que instalar.

```bash
git clone https://github.com/TU-USUARIO/toolkar.git
cd toolkar
```

Abre `index.html` con doble clic (o con la extensión **Live Server** de VS Code). Para ver el tablero con datos, entra a `dashboard.html` y pulsa **Cargar ejemplos**.

> Las solicitudes se guardan en el `localStorage` del navegador: persisten al recargar y se borran con **Vaciar panel** o al limpiar los datos del sitio.

## 🧱 Arquitectura

```
index.html · listado.html · formulario.html · dashboard.html   ← las 4 páginas (en la raíz, para abrir con doble clic)
src/
├── css/
│   ├── tokens.css        variables: colores, tipografía, medidas        ← "¿qué color / fuente?"
│   ├── base.css          reset, tipografía global, layout, reduced-motion
│   ├── components.css    header, botones, tarjetas, campos, footer, modal
│   ├── motion.css        marcado de los efectos de scroll (.expand, .parallax, .zoom)
│   └── pages/            estilos exclusivos de cada página
└── js/
    ├── lib/core.js       motor: TK.reduced / TK.mobile, Lenis + GSAP, header, TK.ready(), TK.scrollTo()
    ├── lib/fx.js         efectos reutilizables: TK.fx.expandOnScroll, parallax, revealText, staggerIn, counter, marquee…
    ├── lib/data.js       datos del negocio (servicios, marcas) y acceso a localStorage: TK.data.*
    ├── pages/            lógica de cada página (pinta el contenido desde data.js y enciende los efectos)
    └── vendor/           copias locales de GSAP, ScrollTrigger, Flip y Lenis (respaldo si el CDN falla)
assets/img/               12 imágenes del rediseño (≤ 350 KB cada una)
tests/                    12 páginas de prueba + harness (ver abajo)
docs/decisiones.md        bitácora de decisiones · docs/superpowers/ especificación y plan
```

**Cómo fluye una solicitud:** `listado.html` → *Solicitar* → `formulario.html?servicio=…` (preselecciona) → enviar → `TK.data.agregarSolicitud()` guarda en `localStorage` con estado `pendiente` → `dashboard.html` la muestra; mover de columna actualiza el estado y persiste.

**Orden de carga** (importa): `tokens → base → components → motion → CSS de página`, y al final del `<body>`: `GSAP → ScrollTrigger → (Flip) → Lenis → core.js → fx.js → data.js → script de página`. Cada archivo de `src/` empieza con una cabecera *Qué hace · Depende de · Lo usan · Ojo con*.

### Stack

| Capa | Tecnología | Por qué |
|---|---|---|
| Estructura | HTML5 semántico | Abre desde `file://`; sin build, fácil de evaluar y desplegar |
| Estilo | CSS3 (custom properties, `clamp()`, `clip-path`) | Tipografía fluida y efectos sin JS extra |
| Animación | [GSAP 3.12](https://gsap.com) + ScrollTrigger + Flip | Control fino del scroll, pin y transiciones de estado |
| Scroll | [Lenis 1.3](https://lenis.darkroom.engineering) | Scroll suave sincronizado con el ticker de GSAP |
| Tipografía | Cormorant Garamond + Manrope (Google Fonts) | Contraste serif elegante / sans limpia |
| Imágenes | Higgsfield (`z_image`) + retoque | Set coherente de 12 escenas nocturnas |

## 🧪 Pruebas

Las pruebas corren **en el navegador** con un harness propio de 30 líneas (`tests/harness.js`): cada `tests/*.test.html` carga la página real en un `<iframe>`, dispara eventos reales (clics, `submit`, `DragEvent`) y comprueba el DOM y `localStorage`. Hace falta un servidor local porque `fetch` no funciona sobre `file://`:

```bash
npx --yes serve -l 5501 .
```

Abre `http://localhost:5501/tests/run-all.html` y entra a cada prueba: cada página termina con `TESTS: N passed, 0 failed` en la consola.

| Página de prueba | Qué cubre |
|---|---|
| `data` | catálogo, formato de precio y fecha, `localStorage` corrupto, ejemplos |
| `core` · `fx` | header inteligente, `scrollTo`, los 7 efectos y la división de líneas |
| `home` · `listado` · `formulario` · `dashboard` | cada página: estructura, enlaces, filtros, preselección, envío, kanban, escape de HTML |
| `images` · `links` · `cleanup` · `design-system` | peso de imágenes, cero enlaces muertos, tokens del sistema visual |

## 🧠 Decisiones de diseño

- **Estética "showroom nocturno"** — carbón `#0B0B0C`, hueso `#EDE9E3`, champán `#C9A96E`: coherente con las marcas y con fotos oscuras de luz puntual.
- **Arquitectura híbrida de scroll** — la expansión de imágenes se reserva para momentos clave (hero, marcas); el resto usa parallax y reveals, para que la página no se vuelva interminable en móvil.
- **Sin build a propósito** — scripts clásicos (no módulos ES, que fallan sobre `file://`) y respaldo de CDN con `document.write`, que mantiene el orden de carga.
- **Los títulos esperan a las fuentes** — `revealText` divide el texto en líneas solo después de `document.fonts.ready`; con la fuente de respaldo los cortes salían distintos.
- **El tablero no usa Lenis** — el scroll suave interfiere con arrastrar tarjetas.

La bitácora completa está en [`docs/decisiones.md`](docs/decisiones.md); la especificación y el plan de implementación en [`docs/superpowers/`](docs/superpowers/).

## 🗺️ Roadmap

- [ ] Backend real (API + base de datos) en lugar de `localStorage`
- [ ] Notificaciones por correo al cliente al confirmar la cita
- [ ] Panel de autenticación para el tablero
- [ ] Internacionalización (inglés)

## 👤 Autor

**Santiago Chavarría** — Ingeniería de Software II, Universidad Latina de Costa Rica

## 📄 Licencia

Distribuido bajo la licencia **MIT**. Consulta [`LICENSE`](LICENSE) para más información.

Las marcas mencionadas (Porsche, Maserati, Audi, Mercedes-Benz, Land Rover, Jaguar, Lexus) pertenecen a sus respectivos dueños y se usan únicamente con fines ilustrativos en un proyecto académico.
