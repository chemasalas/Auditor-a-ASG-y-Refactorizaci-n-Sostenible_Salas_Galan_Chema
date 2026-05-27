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

Kilometraje

-Código postal

*Algunos datos no son estrictamente necesarios para una tasación inicial, por ejemplo:*

-Nombre

-Email

Podrían pedirse solo al final, no en el primer paso.







