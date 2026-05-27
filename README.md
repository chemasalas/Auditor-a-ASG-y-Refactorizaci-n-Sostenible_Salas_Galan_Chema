# Informe Técnico de Auditoría-Chema Salas Galan

# Auditoría ASG y Refactorización Sostenible  
## Empresa: compramostucoche.es

## 1. Introducción

Las páginas web también generan un impacto ambiental debido al consumo de energía necesario para cargar imágenes, vídeos, anuncios y otros recursos digitales. Además, una web debe ser accesible para todos los usuarios y tratar sus datos de forma transparente y responsable.

En este trabajo se realiza una auditoría ASG de la página web de **compramostucoche.es** para analizar tres aspectos principales:

- **Ambiental (A):** consumo de recursos y eficiencia de la web.
- **Social (S):** accesibilidad y facilidad de uso para todas las personas.
- **Gobernanza (G):** privacidad, uso de cookies y tratamiento de datos personales.

Para hacer el análisis se han utilizado herramientas como **Website Carbon Calculator**, **WAVE** y las herramientas de desarrollo del navegador.

Por ultimo hago una refactorización con mejoras técnicas para reducir el consumo de recursos, mejorar la accesibilidad y hacer la web más sostenible.

## Inventario y Dimensión Ambiental

Antes de nada he medido la huella de carbono con la herramienta mencionada anteriormente. He medido la pagina web de mi empresa que es https://www.compramostucoche.es/.

<img width="1238" height="682" alt="image" src="https://github.com/user-attachments/assets/e1bd7a80-0fbf-4057-8e99-fa2d7f7a07eb" />

Vemos que no esta mal, pero se podria mejorar bastante. Segun la escala de Carbono esta pagina webb produce 0.209 de carbono

<img width="982" height="316" alt="image" src="https://github.com/user-attachments/assets/abbb584d-e9c1-4d37-8101-e6db92d2af5e" />


Ahora veremos los 3 recursos mas pesados de la pagina webb, en inspeccionar y en la parte de network lo vemos claramente

<img width="560" height="838" alt="image" src="https://github.com/user-attachments/assets/b75a31b3-d0f4-449f-941f-b2f1950cec13" />

Lo que mas pesa es Bundles JavaScript dinámico, SVGs e imágenes y Scripts de tracking / analytics

Sí, la web se ve que señales de “inflación de software”.
Hace muchas peticiones, carga varios scripts de tracking y recursos JavaScript para una funcionalidad simple. Además, 4.9 MB de recursos es bastante para una web normal.

No es un caso extremo, pero sí una web más compleja y pesada de lo necesario.

## Dimensión Social y Equidad

Aqui voy a evaluar a ver si la web es accesible para todas las personas que entren en ella, parea ello utilizare la herramienta WAVE. Vemos que tiene varios problemas para la accebilidad

<img width="766" height="418" alt="image" src="https://github.com/user-attachments/assets/0588eef4-1fe1-4729-ac6a-bf1c52244e71" />

<img width="676" height="295" alt="image" src="https://github.com/user-attachments/assets/8454541e-79c0-4612-863e-9637e98b15fc" />

El sitio no cumple con los estándares WCAG 2.2 AA, lo que implica una sostenibilidad social deficiente.

La accesibilidad es un componente esencial de la sostenibilidad digital, y su incumplimiento excluye a una parte significativa de la población.
Principales causas:

Diseño visual centrado en estética comercial, no en inclusión.

1. Falta de etiquetas semánticas y atributos accesibles.

2. Contrastes y tipografías que no respetan los mínimos de legibilidad.

3. Formularios sin estructura accesible ni validación clara.

##  Dimensión de Gobernanza y Ética

### Transparencia en cookies
El banner de cookies, prioriza el botón “Aceptar todo” en color destacado.
El botón “Configurar” está menos visible.
Requiere varios clics para rechazar cookies no esenciales.
Esto constituye un Dark Pattern de tipo obstrucción.

### Datos innecesarios

El formulario solicita:

1. Nombre

2. Email

3. Teléfono

4. Matrícula

5. Kilometraje

6. Código postal

*Algunos datos no son estrictamente necesarios para una tasación inicial, por ejemplo:*

-Nombre

-Email

Podrían pedirse solo al final, no en el primer paso.

## Propuesta de Refactorización

La web presenta una gran cantidad de scripts externos, código duplicado, librerías pesadas, tracking redundante y ausencia de optimización de activos.  
A continuación se detalla una propuesta técnica de refactorización sostenible.

---

# Optimización de activos

## Optimizar imágenes

Aunque el fragmento de código no incluye imágenes, la auditoría previa mostró imágenes grandes en formato JPG/PNG.

### Solución

- Convertir imágenes a **WebP** o **AVIF**
- Añadir `loading="lazy"` para carga diferida

### Ejemplo antes

```html
<img src="/static/banner.jpg">
```

### Ejemplo después

```html
<picture>
  <source srcset="/static/banner.avif" type="image/avif">
  <source srcset="/static/banner.webp" type="image/webp">
  <img src="/static/banner.jpg" loading="lazy" alt="Tasación de coches">
</picture>
```

---

# Reducción de peticiones HTTP

El código muestra más de 12 scripts externos, entre ellos:

- React (production)
- ReactDOM
- PropTypes
- GDPR Banner React
- Scripts de GTM
- Librerías GDPR
- Scripts de experimentación A/B
- Scripts de tracking

Esto genera múltiples peticiones, muchas de ellas innecesarias para una página de tasación simple.

## Problemas detectados

- React se carga únicamente para el banner de cookies → sobredimensionado
- Scripts duplicados de GDPR
- GTM se inyecta dinámicamente varias veces
- Código minificado muy extenso (más de 500 líneas solo para GDPR)

## Soluciones

### ✔ Sustituir React por Web Components o Vanilla JS

El banner de cookies no requiere React.

**React + ReactDOM ≈ 120 KB innecesarios**

### Antes (React)

```html
<script src="react.production.min.js"></script>
<script src="react-dom.production.min.js"></script>
<script src="gdpr-banner-react.js"></script>
```

### Después (Vanilla JS)

```html
<script src="/static/gdpr-banner.js" defer></script>
```

### Beneficio estimado

- Ahorro de entre **120–150 KB por visita**
- Menor consumo energético
- Reducción del tiempo de carga

---

# 4.3 Eliminación de código no utilizado

El código contiene:

- Funciones de A/B testing no utilizadas
- Scripts de experimentación (`wkdaConfigs`) sin configuración activa
- Código GDPR duplicado
- Funciones de tracking que se ejecutan incluso sin consentimiento del usuario

## Ejemplo de código innecesario detectado

```js
window.wkdaConfigs = [];

// A/B testing logic...
// Redirect logic...
// Experiment variants...
```

## Propuesta

- Eliminar completamente el bloque de experimentación si no está en uso
- Cargar scripts de marketing solo tras aceptación explícita de cookies

## Implementación recomendada

```js
if (gdprPreferences.marketing === true) {
   loadScript('/static/gtm.js');
}
```

### Beneficios

- Reducción de JavaScript ejecutado
- Menor consumo de CPU
- Disminución del tráfico innecesario
- Mejora de privacidad y cumplimiento GDPR

---

# 4.4 Aplazamiento de scripts (`defer` / `async`)

Muchos scripts se cargan antes del contenido principal, bloqueando el renderizado inicial.

## Ejemplo antes

```html
<script src="gdprlib.js"></script>
<script src="react.production.min.js"></script>
```

## Ejemplo después

```html
<script src="gdprlib.js" defer></script>
<script src="react.production.min.js" defer></script>
```

## Beneficios

- Reducción del **Total Blocking Time (TBT)**
- Mejora del **Largest Contentful Paint (LCP)**
- Renderizado más rápido del contenido principal

---

# 4.5 Mejora social (S)

Aunque el código no muestra toda la interfaz visual, se detectan problemas de accesibilidad.

## ✔ Añadir roles ARIA al banner de cookies

### Antes

```html
<div id="gdpr-banner-root"></div>
```

### Después

```html
<div id="gdpr-banner-root" role="dialog" aria-live="polite"></div>
```

---

## ✔ Botones accesibles

Los botones del banner no incluyen atributos `aria-label`.

### Solución

```html
<button aria-label="Aceptar todas las cookies">
  Aceptar todas
</button>
```

## Beneficios

- Mejor experiencia para usuarios con lectores de pantalla
- Mayor cumplimiento WCAG
- Mejora de inclusión digital

---

# 🛡 4.6 Mejora de gobernanza (G)

El código evidencia un posible **Dark Pattern**:

- El botón **“Aceptar todas”** está visualmente destacado
- La opción **“Configuración”** requiere más pasos
- No existe un botón visible de **“Rechazar todas”**

## Solución: igualdad visual y funcional

### Antes

```js
"button": "Aceptar todas",
"secondaryButton": {
  "default": "Configuración"
}
```

### Después

- Botones del mismo tamaño y color
- Opción **“Rechazar todas”** visible desde el primer nivel
- Mismo número de clics para aceptar o rechazar cookies

## Beneficios

- Cumplimiento ético y normativo
- Mejora de transparencia
- Reducción de prácticas manipulativas
- Mejor experiencia de usuario

---







