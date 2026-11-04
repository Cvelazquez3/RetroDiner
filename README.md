# Retro Diner - Auditoría y Corrección de Accesibilidad Web (WCAG 2.1 / 2.2 Nivel AA)

Este proyecto corresponde a la auditoría, corrección y optimización integral de accesibilidad web para el sitio **Retro Diner**, evaluado conforme a los estándares internacionales de las **WCAG 2.1 / 2.2 (Web Content Accessibility Guidelines)** en su nivel de conformidad **AA**, utilizando la herramienta de análisis asistido **Silktide Accessibility Checker**.

---

## 📋 Resumen Ejecutivo de la Auditoría

| Área Evaluada | Estado Inicial | Criterios WCAG Infringidos | Estado Final |
| :--- | :--- | :--- | :--- |
| **Textos Alternativos (`alt`)** | Enlaces con imágenes vacías (`alt=""`), textos extensos o redundantes, errores de sintaxis HTML. | **1.1.1 Non-text Content**<br>**2.4.4 Link Purpose** | **100% Corregido (Concisos y Factuales)** |
| **Navegación por Teclado** | Foco completamente suprimido (`outline: 0`), recuadros recortados en la barra de navegación, sin skip-link. | **2.1.1 Keyboard**<br>**2.4.7 Focus Visible**<br>**2.4.1 Bypass Blocks**<br>**1.4.11 Non-text Contrast** | **100% Corregido (Recuadros visibles)** |
| **Jerarquía de Encabezados** | Inexistencia de `<h1>` en inicio, teléfono en `<h3>`, menú sin estructura jerárquica clara (`H1` -> `H2` -> `H3`). | **1.3.1 Info & Relationships**<br>**2.4.6 Headings & Labels** | **100% Corregido (H1 Menú -> H2 Categoría -> H3 Platillo)** |
| **Regiones Semánticas (Landmarks)** | Maquetación estructurada 100% en `<div>`, sin puntos de referencia semánticos para lectores de pantalla. | **1.3.1 Info & Relationships** | **100% Corregido** |
| **Formularios e Identificación** | Campos de datos personales sin propósito programático declarado y botones de envío sin nombre accesible. | **1.3.5 Identify Input Purpose**<br>**4.1.2 Name, Role, Value** | **100% Corregido** |
| **Contraste de Color** | Enlaces del menú principal con contraste deficiente (1.6:1). | **1.4.3 Contrast (Minimum)** | **100% Corregido** (6.5:1) |
| **Idioma del Documento** | Documentos HTML sin declarar el atributo de idioma `lang="en"`. | **3.1.1 Language of Page** | **100% Corregido** |

---

## 🔍 Detalle Técnico de los Cambios Implementados

### 1. Adecuación de Textos Alternativos (`alt` Concisos y Factuales)
Conforme a las mejores prácticas de accesibilidad, el texto alternativo debe ser **breve, objetivo y describir exactamente lo que se ve**, sin palabras de relleno ni adjetivos de marketing innecesarios:
- **Logotipo:** `alt="Retro Diner"`
- **Banner principal:** `alt="Burger specials"`
- **Tarjetas del Home y Menú:** 
  - `hotdogs.jpg` $\rightarrow$ `alt="Hotdog"`
  - `shakes.jpg` $\rightarrow$ `alt="Shakes"`
  - `breakfast.jpg` $\rightarrow$ `alt="Breakfast"`
  - `burger.jpg` $\rightarrow$ `alt="Burger"`
  - `breakfast2.jpg` $\rightarrow$ `alt="Breakfast"`
  - `hotdog.jpg` $\rightarrow$ `alt="Hotdog"`
  - `shake.jpg` $\rightarrow$ `alt="Shakes"`
- **Imágenes Decorativas:** La mesera (`waitress.png`) es un elemento puramente estético; se configuró con `alt=""` y `aria-hidden="true"` para que los lectores de pantalla la ignoren por completo. Se eliminó la etiqueta de cierre `</a>` huérfana y la etiqueta mal formada `<a img src="...">`.

### 2. Navegación por Teclado y Recuadros Visibles (*Focus Visible*)
- **Recuadros en la barra de navegación:** 
  - Se eliminó el `overflow: hidden;` en `#header div ul.navigation` que recortaba el anillo de foco superior e inferior.
  - Se asignó `display: inline-block;` y espaciado adecuado a los enlaces para que al presionar `Tab` se dibuje un **recuadro rectangular completo y nítido** (`outline: 3px solid #ffda79 !important; outline-offset: 2px;`) alrededor de cada pestaña (`Home`, `About`, `Menu`, `Contact`, `Blog`), idéntico al comportamiento en el resto del sitio.
- **Enlace de salto (*Skip to Content*):**
  - Se implementó un enlace `<a href="#body" class="skip-link">Skip to main content</a>` al inicio de cada documento.
  - Se añadió `tabindex="-1"` a `<main id="body">` para garantizar el desplazamiento directo del foco del navegador.
- **Elementos interactivos:** Íconos de redes sociales y botones de descarga ahora cuentan con `aria-label` y texto `.sr-only`.

### 3. Jerarquía Estricta de Encabezados (*Headings*)
- **Estructura en las páginas de Menú (`burger.html`, `hotdog.html`, `shake.html`, `breakfast.html`):**
  - `<h1>Menu</h1>`: Encabezado principal del documento.
  - `<h2 class="category-title">Burgers</h2>` (o `Hotdogs`, `Shakes`, `Breakfast`): Encabezado de la categoría seleccionada.
  - `<h3>Burger Special</h3>`, `<h3>Veggie Burger</h3>`, etc.: Título de cada platillo dentro de la categoría.
  - Barra lateral: `<h2>Categories</h2>`.
  - Esta estructura cumple con una jerarquía perfecta: `H1` (Menú) $\rightarrow$ `H2` (Categoría) $\rightarrow$ `H3` (Platillos específicos).
- **Número de Teléfono en el Pie de Página:**
  - Se eliminó la etiqueta `<h3>Call 0-123-456-789</h3>` y se reemplazó por un párrafo semántico `<p class="phone">Call 0-123-456-789</p>`, manteniendo su tipografía y estilo visual mediante CSS. Los números telefónicos son datos de contacto y no títulos de sección.
- **Página de Inicio (`index.html`):** Título principal en `<h1>Welcome to Retro Diner!</h1>`.
- **Página de Contacto (`contact.html`):** Subtítulo en `<h2 class="subtitle">` en lugar de saltar a `<h4>`.

### 4. Regiones Semánticas (*Landmarks*)
Se reemplazaron los contenedores genéricos `<div>` por etiquetas estándar de HTML5 con roles ARIA explícitos:
- `<header id="header" role="banner">`
- `<nav aria-label="Main Navigation">` (con `aria-current="page"` para el enlace activo)
- `<main id="body" role="main" tabindex="-1">`
- `<aside class="sidebar" aria-label="...">`
- `<footer id="footer" role="contentinfo">`
- `<nav aria-label="Footer Navigation">`

### 5. Propósito Programático de Campos (*WCAG 1.3.5*)
- Se añadió el atributo `autocomplete="email"` al campo de suscripción del pie de página en todas las páginas, y `autocomplete="name"` y `autocomplete="email"` en el formulario de contacto.

---

## 🛠️ Herramientas Utilizadas
- **Silktide Accessibility Checker** (Extensión para Google Chrome / Microsoft Edge).
- **Validador de Código y Contrastes WCAG 2.1 / 2.2**.
- **Servidor Local XAMPP** (Apache).
