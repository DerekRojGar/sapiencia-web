# Sitio Web Editorial Sapiencia

Documentación técnica y guía de desarrollo del sitio institucional de Editorial Sapiencia. El proyecto es un sitio estático basado en Bootstrap con mejoras de UI/UX, animaciones y un sistema de diseño centralizado en variables CSS.

## Tabla de contenidos
- Visión general
- Tecnologías y librerías
- Arquitectura y estructura de carpetas
- Sistema de diseño (colores, tipografías, espaciado)
- Componentes y secciones
- Animaciones y microinteracciones
- Contenido dinámico (catálogo)
- Formulario de contacto
- Accesibilidad (A11y)
- Rendimiento y buenas prácticas
- SEO y metadatos
- Desarrollo local y despliegue
- Convenciones de código y contribución
- Créditos y licencias

---

## Visión general
- Tipo: sitio estático one-page con secciones.
- Objetivo: comunicar identidad, servicios, catálogo y contacto de la editorial.
- Puntos clave: sistema de colores de marca, tipografías coherentes, animaciones sutiles, componentes reutilizables y accesibilidad básica.

## Tecnologías y librerías
- HTML5 + CSS3 + JavaScript (vanilla)
- Bootstrap 5.3.3 (grid, utilidades)
- Bootstrap Icons
- AOS (Animate On Scroll)
- GLightbox (visores de imágenes/medios)
- Swiper (carrusel del catálogo)
- ImagesLoaded + Isotope (layouts filtrables; si aplica en portafolio)
- PureCounter (contadores animados)
- PHP básico para `forms/contact.php`
- Google Fonts: Roboto (texto), Poppins (nav), Raleway (títulos)

## Arquitectura y estructura de carpetas
```
index.html
assets/
  css/
    main.css            # Estilos del tema y sistema de diseño
  js/
    main.js             # Comportamiento UI (nav, AOS, etc.)
  img/                  # Imágenes del sitio
  vendor/               # Librerías de terceros (Bootstrap, AOS, Swiper, etc.)
  catalogo.json         # Datos del catálogo (libros)
forms/
  contact.php           # Endpoint para formulario de contacto
```

Notas:
- Todo el estilo se centraliza en `assets/css/main.css` usando variables CSS.
- Los estilos inline en `index.html` son específicos de ciertas secciones. Se recomienda migrarlos a `main.css` cuando sea posible para mantener consistencia.

## Sistema de diseño

### Paleta de colores
- Naranja principal (logo): `#FF5A00`
- Morado oscuro elegante: `#4B1D73`
- Violeta claro de acento: `#A974FF`
- Gris suave para fondos: `#F4F4F4`
- Negro carbón para texto: `#222222`

Variables en `:root` (main.css):
```css
:root {
  --background-color: #F4F4F4;
  --default-color: #222222;
  --heading-color: #4B1D73;
  --accent-color: #A974FF;
  --surface-color: #ffffff;
  --contrast-color: #ffffff;

  /* Tokens de marca */
  --brand-orange: #FF5A00;
  --brand-purple: #4B1D73;
  --brand-violet: #A974FF;

  /* Navegación */
  --nav-color: #ffffff;
  --nav-hover-color: var(--accent-color);
  --nav-dropdown-background-color: #ffffff;
  --nav-dropdown-color: #444444;
  --nav-dropdown-hover-color: var(--accent-color);
}
```
Excepción del header: para mantener su estética original, en `.header` se sobreescribe localmente `--accent-color` a `#f0ab51`.

### Tipografías
- Títulos: Raleway (700 recomendado para títulos principales)
- Navegación: Poppins
- Texto base: Roboto

### Espaciado y layout
- Bootstrap Grid 5.x; usar utilidades (p-*, m-*, gap-*, row/col) y evitar CSS ad-hoc cuando sea posible.

## Componentes y secciones
- Header fijo con menú responsivo (desktop/mobile). El color del header no debe modificarse salvo decisión de marca.
- Hero: imagen/cover con overlay y CTAs.
- About/Identidad/Misión-Visión/Valores: bloques de contenido con títulos y copy. Usar `.section-title` para consistencia.
- Servicios: tarjetas con icono circular.
- Catálogo: carrusel Swiper con tarjetas de libros, datos obtenidos desde `assets/catalogo.json`.
- Portafolio (si se usa): filtros y tarjetas con overlay.
- Estadísticas: contadores con PureCounter.
- Contacto: tarjeta WhatsApp + formulario con validación.
- Footer: enlaces sociales y créditos.

## Animaciones y microinteracciones
- AOS en elementos con `data-aos` y `data-aos-delay`.
- Respeto a `prefers-reduced-motion` (se desactivan delays en móviles/usuarios con reducción de movimiento).
- Microinteracciones: botones con degradados (violeta→morado y hover naranja→violeta), preloader bicolor, botón “play” pulsante, scroll-top.

## Contenido dinámico (Catálogo)
Archivo: `assets/catalogo.json`. Esquema sugerido por elemento:
```json
{
  "titulo": "Título del libro",
  "autor": "Nombre del autor",
  "portada": "assets/img/portadas/mi-libro.jpg",
  "descripcion": "Breve sinopsis",
  "enlace": "https://..." // opcional
}
```
- El JS inserta los items en `#catalogo-swiper-wrapper` dentro de `#catalogoSwiper`.
- Optimizar las imágenes de portada (peso y dimensiones); usar formatos modernos (WebP) cuando sea posible.

## Formulario de contacto
- Frontend: validación con `assets/vendor/php-email-form/validate.js`.
- Backend: `forms/contact.php` (PHP sencillo). Recomendaciones:
  - Configurar destinatario, asunto y cabeceras de correo.
  - Sanitizar y validar inputs del lado servidor.
  - Considerar un servicio de correo transaccional (SMTP autenticado) para producción.
  - Evitar exponer claves en el repositorio.

## Accesibilidad (A11y)
- Contraste: mantener ratio AA para texto sobre fondos/degradados.
- Teclado: todos los enlaces/botones deben ser alcanzables (focus visible).
- ARIA: usar `aria-label` en íconos y CTAs (ej. WhatsApp).
- Movimiento: respetar `prefers-reduced-motion` y evitar animaciones excesivas.
- Imágenes: incluir `alt` descriptivo.

## Rendimiento y buenas prácticas
- Imágenes optimizadas y responsive (usar tamaños adecuados; `object-fit` para recortes).
- Minificar CSS/JS de ser necesario en producción.
- Cargar librerías vendor desde `assets/vendor` locales para evitar latencia externa.
- Evitar CSS inline prolongado; centralizar en `main.css`.

## SEO y metadatos
- Completar `<meta name="description">` y `<meta name="keywords">` en `index.html`.
- Títulos únicos por página/sección; encabezados jerárquicos (`h1`→`h2`→...).
- Open Graph / Twitter Cards (opcional):
```html
<meta property="og:title" content="Editorial Sapiencia" />
<meta property="og:description" content="Publicación editorial con excelencia..." />
<meta property="og:image" content="assets/img/sapiencia-logo-bg.png" />
<meta property="og:type" content="website" />
<meta property="og:url" content="https://www.editorialsapiencia.com" />
```

## Desarrollo local y despliegue
- Desarrollo local: al ser un sitio estático, basta con abrir `index.html` en el navegador o usar un servidor estático simple.
- Despliegue: compatible con GitHub Pages, Netlify, Vercel o cualquier hosting estático. Subir el contenido del proyecto manteniendo la estructura de `assets/` y `forms/`.
- Si se usa PHP (contacto), el hosting debe soportar PHP para procesar el formulario.

## Convenciones de código y contribución
- CSS: usar variables existentes; preferir tokens `--brand-*` para nuevos componentes.
- Evitar modificar el header: su acento está fijado a `#f0ab51` por decisión de diseño.
- JS: mantener código modular en `assets/js/main.js`. Comentar bloques que interactúen con el DOM.
- Nombrado: clases semánticas y consistentes con el resto del tema.
- Pull Requests: describir cambios, adjuntar capturas si impacta UI, y probar en móvil/desktop.

## Créditos y licencias
- Base del tema: KnightOne de BootstrapMade (ver licencia en comentarios de `main.css`).
- Íconos: Bootstrap Icons.
- Otras librerías: ver `assets/vendor/` según sus licencias respectivas.
- Desarrollo y adaptación: Ing. Derek Benigno Rojas García.

---

¿Dudas o mejoras? Abrir un issue con una descripción clara del cambio propuesto y su impacto en UI/UX, accesibilidad o rendimiento.
