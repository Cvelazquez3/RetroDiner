# Retro Diner - Auditoría y Corrección de Accesibilidad Web (WCAG 2.1 / 2.2 Nivel AA)

Este proyecto corresponde a la auditoría, corrección y optimización integral de accesibilidad web para el sitio **Retro Diner**, evaluado conforme a los estándares internacionales de las **WCAG 2.1 / 2.2 (Web Content Accessibility Guidelines)** en su nivel de conformidad **AA**, utilizando la herramienta de análisis asistido **Silktide Accessibility Checker**.

---

## 📋 Resumen Ejecutivo de la Auditoría

| Área Evaluada | Estado Inicial | Criterios WCAG Infringidos | Estado Final |
| :--- | :--- | :--- | :--- |
| **Textos Alternativos (`alt`)** | Enlaces con imágenes vacías (`alt=""`), imágenes decorativas mal tratadas y errores de sintaxis HTML. | **1.1.1 Non-text Content**<br>**2.4.4 Link Purpose** | **100% Corregido** |
| **Navegación por Teclado** | Foco completamente suprimido (`outline: 0`), sin indicador visible ni mecanismo de salto de bloques. | **2.1.1 Keyboard**<br>**2.4.7 Focus Visible**<br>**2.4.1 Bypass Blocks**<br>**1.4.11 Non-text Contrast** | **100% Corregido** |
| **Jerarquía de Encabezados** | Inexistencia de `<h1>` en página de inicio, encabezados de barra lateral como `<h1>`, saltos de nivel (`<h1>` a `<h4>`). | **1.3.1 Info & Relationships**<br>**2.4.6 Headings & Labels** | **100% Corregido** |
| **Regiones Semánticas (Landmarks)** | Maquetación estructurada 100% en `<div>`, sin puntos de referencia semánticos para lectores de pantalla. | **1.3.1 Info & Relationships** | **100% Corregido** |
| **Formularios e Identificación** | Campos de datos personales sin propósito programático declarado y botones de envío sin nombre accesible. | **1.3.5 Identify Input Purpose**<br>**4.1.2 Name, Role, Value** | **100% Corregido** |
| **Contraste de Color** | Enlaces del menú principal con contraste deficiente (1.6:1). | **1.4.3 Contrast (Minimum)** | **100% Corregido** (6.5:1) |
| **Idioma del Documento** | Documentos HTML sin declarar el atributo de idioma `lang="en"`. | **3.1.1 Language of Page** | **100% Corregido** |

---

## 🔍 Detalle Técnico de los Cambios Implementados

### 1. Adecuación de Textos Alternativos (`alt`)
- **Logotipos de Cabecera y Pie:** 
  - Cabecera: `alt="Retro Diner - Return to Home"` dentro del enlace principal, garantizando un nombre accesible adecuado.
  - Pie de página: `alt="Retro Diner"`.
- **Imágenes Decorativas vs Informativas:**
  - La imagen de la mesera (`waitress.png`) es un elemento puramente estético y de ambientación; se configuró con `alt=""` y `aria-hidden="true"` para evitar ruido a los usuarios de lectores de pantalla. Asimismo, se corrigió una etiqueta de cierre `</a>` huérfana en el HTML original.
- **Banners y Platillos del Menú:**
  - `burger-specials.png`: `alt="Special Burger Offers - Explore our burger specials menu"`.
  - Se añadieron descripciones sensoriales y contextuales en todas las imágenes de productos: `hotdogs.jpg`, `shakes.jpg`, `breakfast.jpg`, `burger.jpg`, `breakfast2.jpg`, `shake.jpg`, etc.
  - En `shake.html` se corrigió el error sintáctico de etiqueta mal formada `<a img src="shake.html">`.

### 2. Navegación por Teclado, Foco Visible y Orden
- **Restauración y Mejora del Foco (`:focus-visible`):**
  - Se eliminó la regla `a { outline: 0; }` en `style.css` que eliminaba el anillo de foco por defecto.
  - Se implementó un sistema de foco con relación de contraste superior a 3:1 (cumpliendo WCAG 1.4.11 y 2.4.11):
    - **Fondos claros (contenido principal):** Anillo de foco `:focus-visible` en color verde azulado oscuro (`#1b6568`, contraste de 6.5:1).
    - **Fondos oscuros (cabecera y pie de página):** Anillo de foco `:focus-visible` en color dorado brillante (`#ffda79`, contraste de 6.2:1).
- **Mecanismo de Salto de Bloques (*Skip to Content*):**
  - Se implementó un enlace `<a href="#body" class="skip-link">Skip to main content</a>` al inicio de cada documento.
  - Se añadió `tabindex="-1"` al contenedor `<main id="body">` para que el foco del navegador se desplace directamente al contenido al activarlo.
- **Enlaces y Botones sin Nombre Accesible:**
  - Los íconos de redes sociales (`facebook`, `twitter`, `googleplus`) y el botón `.download` contaban únicamente con fondos CSS o `&nbsp;`. Se les agregaron atributos `aria-label` y texto oculto para lectores de pantalla mediante la clase `.sr-only`.
  - Los botones de envío `<input type="submit" value="">` ahora cuentan con `aria-label="Send message"` y `aria-label="Subscribe to email newsletter"`.

### 3. Encabezados (*Headings*) y Jerarquía Lógica
- **Página de Inicio (`index.html`):** No existía ningún `<h1>`. Se promovió el título `"Welcome to Retro Diner!"` a `<h1>`, manteniendo su diseño visual en CSS.
- **Páginas de Menú (`burger.html`, `breakfast.html`, etc.):** Se conservó un único `<h1>` por categoría y los títulos de la barra lateral se ajustaron a `<h2>Menu</h2>` para evitar la colisión de múltiples `<h1>`.
- **Página de Contacto (`contact.html`):** 
  - Se corrigió el salto de nivel donde `<h1>Contact</h1>` pasaba directamente a `<h4>Comments and Suggestions...</h4>`, reemplazándolo por `<h2 class="subtitle">`.
  - Los cuatro encabezados `<h1>` de la barra lateral (`Open Everyday`, `Address`, `Phone Number`) se corrigieron a `<h2>`.
- **Blog (`blog.html`):** Se introdujo un encabezado de nivel superior `<h1>Retro Diner Blog</h1>` oculto visualmente (`.sr-only`), permitiendo que los títulos de publicaciones sean `<h2>` y los metadatos `<h3>`.

### 4. Regiones Semánticas (*Landmarks*)
Se reemplazó la estructura basada en `<div>` por etiquetas estándar de HTML5 con roles ARIA explícitos:
- `<header id="header" role="banner">`
- `<nav aria-label="Main Navigation">` (con `aria-current="page"` para el elemento activo)
- `<main id="body" role="main" tabindex="-1">`
- `<aside class="sidebar" aria-label="...">` (con nombres descriptivos según la página)
- `<nav aria-label="Menu categories">` (en barras laterales de menú)
- `<footer id="footer" role="contentinfo">`
- `<nav aria-label="Footer Navigation">`

### 5. Propósito de Campos de Entrada (*Programmatic Field Purpose*)
- Conforme a **WCAG 2.1 Criterio de Éxito 1.3.5 (Nivel AA)**, se añadió el atributo `autocomplete="email"` al campo de suscripción del pie de página en todas las páginas, y `autocomplete="name"` y `autocomplete="email"` en el formulario de contacto.

---

## 🛠️ Herramientas Utilizadas
- **Silktide Accessibility Checker** (Extensión para Google Chrome / Microsoft Edge).
- **Validador de Código y Contrastes WCAG 2.1 / 2.2**.
- **Servidor Local XAMPP** (Apache).

---

## 💻 Estructura de Archivos del Proyecto

```text
retrodiner/
├── css/
│   └── style.css            # Hoja de estilos con soporte para :focus-visible, skip-link y contraste accesible
├── fonts/                   # Tipografías web locales
├── images/                  # Activos visuales y optimizaciones gráficas
├── index.html               # Página de inicio accesible
├── about.html               # Página 'Acerca de nosotros'
├── burger.html              # Menú de hamburguesas
├── breakfast.html           # Menú de desayunos
├── hotdog.html              # Menú de hot dogs
├── shake.html               # Menú de malteadas
├── contact.html             # Formulario de contacto accesible
├── blog.html                # Blog y artículos accesibles
└── README.md                # Documentación técnica de accesibilidad
```
