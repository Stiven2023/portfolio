# Guía rápida: cómo hice tu página responsive (paso a paso)

Esta guía está fuera del código (no afecta el build) y explica qué cambié por sección, por qué y para qué sirve. Trabajo con enfoque mobile‑first usando Tailwind CSS.

## Principios que seguí
- Mobile‑first: estilos base para móviles; luego mejoro con breakpoints (`sm`, `md`, `lg`).
- Contenedores centrados y con ancho máximo: evita líneas demasiado largas y que el contenido quede pegado al borde.
- Tipografías y espaciados responsivos: tamaños más pequeños en móvil, más grandes en desktop.
- Layouts flexibles: uso de `flex`, `flex-wrap` y `gap` para que no se rompan en pantallas pequeñas.

---

## 1) SectionContainer.astro
Ruta: `src/components/SectionContainer.astro`

- Antes: `w-full lg:w-[740px] mx-auto py-20`
- Ahora: `mx-auto w-full max-w-[740px] px-4 sm:px-6 py-12 sm:py-20`

Qué hace y por qué:
- `mx-auto`: centra el bloque.
- `max-w-[740px]`: limita el ancho para lecturabilidad tipo artículo.
- `px-4 sm:px-6`: padding lateral en móvil (evita que el texto toque los bordes) y un poco más en pantallas mayores.
- `py-12 sm:py-20`: espacio vertical cómodo; más aire en pantallas medianas.

Resultado: todos los bloques quedan centrados y con márgenes laterales correctos en móvil.

---

## 2) Welcome.astro
Ruta: `src/components/Welcome.astro`

Imagen de avatar:
- Clases: `w-20 h-20 sm:w-24 sm:h-24 md:w-32 md:h-32 object-cover`.
- Objetivo: tamaño gradual por breakpoint y recorte correcto sin deformación.

Título (H1):
- Clases: `text-3xl sm:text-4xl md:text-5xl font-bold`
- Layout: `flex flex-col sm:flex-row ... items-center sm:items-center sm:justify-center text-center`
- Nombre en una línea desktop: `<span class="md:whitespace-nowrap">...`.
- Por qué: centrado en desktop, apilado en móvil; el `nowrap` evita cortes raros del nombre.

Subtítulo (H2):
- Clases: `text-base sm:text-lg md:text-2xl max-w-prose mx-auto text-center sm:text-left leading-relaxed text-pretty`
- Por qué: texto legible en móvil (centrado y más corto), y alineado a la izquierda desde `sm` para aspecto más editorial.

Pills sociales (nav):
- Clases: `flex flex-wrap justify-center sm:justify-start gap-3 sm:gap-x-4`
- Por qué: permiten salto de línea en móvil y centrado; en pantallas mayores se alinean a la izquierda con mejor separación.

---

## 3) Header.astro
Ruta: `src/components/Header.astro`

- `flex flex-row flex-wrap gap-x-6 gap-y-2 text-sm sm:text-base opacity-80 justify-center`
- Por qué: el menú se centra y puede envolver en dos filas en móvil; `text-sm` en móvil para evitar desbordes.

---

## 4) Experience.astro (lista/timeline)
Ruta: `src/components/Experience.astro`

- Contenedor `<ol>`: `max-w-3xl mx-auto px-2 sm:px-0` para centrar y dar margen lateral móvil.
- Timeline solo en desktop: `lg:border-s ... lg:pl-6` añade el borde lateral y padding desde `lg`.
- Espaciado: `space-y-8 lg:space-y-0` (más aire entre items en móvil; compacto en desktop).

Por qué: en móviles se ve como lista vertical limpia; en desktop conserva el aspecto de timeline.

---

## 5) ExperienceItem.astro (cada item)
Ruta: `src/components/ExperienceItem.astro`

- Marcador: `absolute lg:relative w-3 h-3 bg-cyan-300 ...` → punto visible; relativo en desktop para alinear con el timeline.
- Título/Descripción: `text-base sm:text-lg` y `text-sm sm:text-base` → tipografías más pequeñas en móvil, más grandes después.
- Color y contraste: `text-white` para títulos y `text-gray-300` para cuerpo; botón degradado `from-indigo-600 via-violet-600 to-cyan-500`.

Por qué: legible en temas oscuros, consistente con la paleta del proyecto y con buen contraste.

---

## 6) Layout.astro (fondo)
Ruta: `src/layouts/Layout.astro`

Estructura aplicada dentro del `<body>`:
- Wrapper: `relative min-h-screen w-full bg-black` → asegura altura mínima y base oscura.
- Capa base radial (detrás): `absolute top-0 z-[-2] h-screen w-screen bg-neutral-950 bg-[radial-gradient(...)]`
- Patrón de rejilla: `absolute inset-0 bg-[linear-gradient(...)] bg-[size:14px_24px]` → textura sutil.
- Radial responsivo: `absolute left-1/2 top-[-10%] -translate-x-1/2 w-[60vmax] h-[60vmax] ... opacity-60`
- Contenido por encima: wrapper `relative z-10` con `<Header />` y `<slot />`.

Por qué: las capas del fondo no interfieren con clics (`pointer-events-none`) y el contenido queda siempre por encima (`z-10`). `vmax` hace que el radial escale con la pantalla.

---

## 7) Global CSS
Ruta: `src/styles/global.css`

- `:root { color-scheme: light dark; }`
- `html { background-color: #131111; line-height: 1.5; }`
- `body { font-family: 'Onest', sans-serif; min-height: 100vh; display: flex; flex-direction: column; }`

Por qué: base coherente para tipografía, altura y modo oscuro.

---

## Breakpoints útiles de Tailwind
- `sm` (640px): inicio de tablet chica/landscape phone.
- `md` (768px): tablet.
- `lg` (1024px): laptop.
- `xl` (1280px) / `2xl` (1536px): desktop grandes.

Sintaxis: `sm:text-lg md:text-xl lg:text-2xl` (aplica a partir del breakpoint hacia arriba).

---

## Patrón para hacer una sección responsive
1. Envolver en un contenedor centrado con `max-w-*` y `px-4 sm:px-6`.
2. Definir tipografías escalonadas: `text-base sm:text-lg md:text-xl`.
3. Usar `flex`/`grid` con `flex-wrap` y `gap-*` para que no se rompa en móvil.
4. Alinear distinto por tamaño: `text-center sm:text-left`, `flex-col sm:flex-row`.
5. Revisar en devtools (móvil → tablet → laptop) y ajustar uno o dos pasos de tamaños.

---

## Trucos rápidos
- Evitar líneas largas: usa `max-w-prose` en textos.
- Evitar saltos raros: `md:whitespace-nowrap` para títulos con nombres.
- Balancear párrafos: `text-pretty` y `leading-relaxed`.
- Centrar menús en móvil: `justify-center flex-wrap`.
- Timeline solo en desktop: activa bordes/posicionamiento desde `lg`.

---

¿Dudas o quieres que convierta esta guía en una checklist de PR para futuros componentes? Puedo hacerlo y agregar ejemplos de antes/después con capturas.
