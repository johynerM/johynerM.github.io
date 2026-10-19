# 🌝 LaCueva

> ✏️ **Cómo usar este modelo:** todo lo que está en citas como esta es una nota para ti, bórralo al terminar. Los textos en `[CORCHETES]` son datos que debes poner tú. Reescribe cada explicación con tus palabras y **comprueba cada cálculo en el inspector (F12)** antes de dejarlo.

Sitio web de **LaCueva**, un local de alquiler de consolas en el centro comercial PlazaMia, Floridablanca (Santander).

---

## 1. El proyecto de negocio

**Negocio:** LaCueva es un local de alquiler de consolas por hora (inventado y cualquier parecido con la realidad mera coincidencia) ubicado en el centro comercial PlazaMia.

**Para quién es el sitio:** Para el dueño del local y para los jóvenes y grupos de amigos de la zona que quieren jugar, y que hoy solo se enteran del local por el voz a voz y no por redes ni internet.

**Qué necesita el cliente:**
- Mostrar dónde queda y su horario (mapa y tabla).
- Mostrar qué consolas y juegos tiene, con precios por hora.
- Recibir reservas sin tener que contestar mil mensajes (formulario).
- Que se vea bien en el celular, porque casi todos sus clientes entran desde ahí.
- Un apartado para ver las reseñas de sus clientes y mejorar.

---

## 2. Estructura del proyecto

```
Retos-Html/
├── index.html
├── segunda-pagina.html
├── README.md
├── CSS/
│   └── style.css
├── images/
└── capturas/
```

- `index.html`: presentación del negocio, ubicación, horario, recomendaciones, juegos y opiniones.
- `segunda-pagina.html`: consolas, tabla de tarifas, formulario de reserva y contacto.
- Las dos páginas comparten `header`, `nav`, `footer` y la hoja `css/styles.css`.

### Etiquetas semánticas que elegí

| Etiqueta | Dónde la usé | Por qué |
|---|---|---|
| `<header>` | Título `LaCueva` | Es la cabecera de la página y lleva el único `<h1>`. |
| `<nav>` | Menú de opciones | Es el bloque de enlaces principales entre páginas y secciones. |
| `<main>` | Contenido de cada página | Agrupa lo propio de la página, sin cabecera ni pie. |
| `<section>` | `#ubicacion`, `#horario`, `#juegos`... | Cada una es un tema con su propio título `<h2>`. |
| `<article>` | Cada tarjeta de juego y de consola | Cada tarjeta tiene sentido sola, podría ir en otra página. |
| `<aside>` | Aviso "¿Esperar 1 hora para jugar?" | Es un comentario lateral al contenido de `#recomendaciones`. |
| `<blockquote>` + `<cite>` | Reseñas de clientes | Son citas de otras personas con su autor. |
| `<footer>` | Frase final | Es el pie de página. |

Ejemplo real de mi código:

```html
<article class="tarjeta">
  <input type="checkbox" id="interruptor" class="tarjeta__activador">
  <label for="interruptor" class="tarjeta__imagen">...</label>
  <div class="tarjeta__datos">...</div>
</article>
```

> Se usaron h2 para las secciones principales del menú de las dos paginas y los h3 se relegaron para pequeñas sub-secciones dentro de los h2 tales como las opciones de las tarjetas de juegos

---

## 3. Codigo en CSS para el estilo de las paginas

Declaré las variables en `:root` y las uso en toda la hoja.

```css
:root{
    --color-dorado: #ffd700;
    --color-verde: #50f563;
    --fondo-tarjeta: rgba(14, 14, 54, 0.8);
    --fuente-principal: "Trebuchet MS", Verdana, sans-serif;
    --espacio-m: 1.25rem;
    --radio: 20px;
}
```

| Variable | Valor | Por qué la elegí |
|---|---|---|
| `--color-dorado` | `#ffd700` | Resalta de manera llamativa incluso con la imagen de fondo |
| `--color-verde` | `#50f563` | Resalta y permite observar cuando se interactua en ciertas partes de la pagina |
| `--fondo-tarjeta` | `rgba(14, 14, 54, 0.8)` | Combina con la paleta de colores de la imagen de fondo y no pierde la estetica ni la vision del texto |
| `--fondo-tarjeta:hover` | `rgba(26, 26, 80, 0.95)` | Permite tener un cambio visual cuando se tiene el cursor en la tarjeta |
| `--fuente-principal` | Trebuchet MS, Verdana, sans-serif | El estilo de la letra permite observar y entender bien el texto |
| `--espacio-m` | `1.25rem` | Mantiene un mismo espacio para padding y margenes |
| `--radio` | `20px` | Mantiene un estandar para el redondeo en toda la página |
| `--borde-input` | `rgba(255, 215, 0, 0.4)` | Resalta los bordes de las tarjeta aunque el fondo sea oscuro y llama la atencion más facilmente |

**Ventaja que noté:** Puedo realizar cambios estrategicamente y mucho más eficientes usando las variables que de una forma más principiante haciendo cada cambio sin usar las variables.

---

## 4. Especificidad

Notación: **(IDs, clases, etiquetas)**. Mi hoja no usa IDs en los selectores, así que el primer número siempre es 0.

### Conflicto 1: el color del título dentro de una tarjeta

Reglas que compiten sobre el mismo `<h3>`:

```css
h3{ color: var(--color-dorado-oscuro); }              /* (0,0,1) */
.tarjeta__datos h3{ color: var(--color-naranja); }    /* (0,1,1) */
```

Gana `.tarjeta__datos h3` porque tiene 1 clase contra 0 clases de `h3`. No importa que `h3` esté antes en el archivo.

![Conflicto 1](capturas/cap1.jpeg)

### Conflicto 2: el fondo del ítem del menú al pasar el mouse

```css
.menu__item{ background-color: var(--fondo-tarjeta); }          /* (0,1,0) */
.menu__item:hover{ background-color: var(--fondo-tarjeta-hover); } /* (0,2,0) */
```

Gana `.menu__item:hover` porque la pseudoclase `:hover` cuenta como una clase: 2 clases contra 1. Con el inspector activé `:hov → :hover` para verlo.

![Conflicto 2](capturas/cap2.jpeg)

### Conflicto 3: la tarjeta que se abre

```css
.tarjeta__datos{ max-height: 0; }                                       /* (0,1,0) */
.tarjeta__activador:checked ~ .tarjeta__datos{ max-height: 20rem; }     /* (0,3,0) */
```

Gana la segunda: `.tarjeta__activador` (clase) + `:checked` (pseudoclase) + `.tarjeta__datos` (clase) = 3 clases. El combinador `~` no suma especificidad.

![Conflicto 3](capturas/cap3.jpeg)

### Conflicto 4 (empate resuelto por orden): el color del título dorado

Reglas que compiten sobre el mismo `<h2 class="seccion__titulo seccion__titulo--dorado">`:

```css
.seccion__titulo{ color: var(--color-verde); }            /* (0,1,0) */
.seccion__titulo--dorado{ color: var(--color-dorado); }   /* (0,1,0) */
```

Empatan en (0,1,0), así que gana la que está **después** en el archivo: `.seccion__titulo--dorado`. Lo comprobé moviendo la regla `--dorado` antes de la base: el título se volvió verde.

![Conflicto 4](capturas/cap4.jpeg)



### Conflicto 5: las filas pares de la tabla

```css
.tabla th, .tabla td{ background-color: var(--fondo-cafe); }                  /* (0,1,1) */
.tabla tbody tr:nth-child(even) td{ background-color: var(--fondo-cafe-oscuro); } /* (0,2,3) */
```

Gana la segunda: 2 clases (`.tabla` y `:nth-child`) contra 1, aunque la primera tiene menos etiquetas.

![Conflicto 5](capturas/cap5.jpeg)

---

## 5. Herencia

**Una propiedad que se hereda: `color`.** La declaré solo en `body` (`color: var(--color-texto)`) y la reciben los `<p>`, `<li>` y demás sin que yo la repita. Se nota en que casi todo el texto del sitio es claro sobre fondo oscuro.

**Una propiedad que no se hereda: `border`.** El borde dorado de `.reserva` no pasa a los `label` ni a los `input` que están dentro. Cada `input` necesita su propio `border`.

**Otro caso que me pasó con `font-family`:** el `body` define `font-family`, pero los `input`, `select` y `textarea` no la heredan por defecto, porque el navegador les pone su propia fuente. Por eso en `.reserva__campo input` escribí `font-family: inherit;` para forzar la herencia.

---

## 6. Modelo de cajas

### Cálculo del ancho: `.cofre`

```css
.cofre{
    width: 100%;
    max-width: 300px;
    padding: 25px;
    border: 2px solid var(--color-dorado);
}
*, *::before, *::after{ box-sizing: border-box; }
```

| | content-box | border-box (el que uso) |
|---|---|---|
| Contenido | 300 px | 300 − 50 − 4 = **246 px** |
| Padding (25 × 2) | 50 px | incluido |
| Borde (2 × 2) | 4 px | incluido |
| **Total en pantalla** | 300 + 50 + 4 = **354 px** | **300 px** |

Con `content-box` el cofre medía 354 px, y en un celular de 360 px (menos 8 px de margen del `body` por lado) solo caben 344 px, así que se salía. Con `border-box` mide 300 px y entra.

![tamaño ](capturas/cap6.jpeg)

### Cálculo de una segunda caja: `.tarjeta`

`max-width: 400px`, `padding: 15px`, `border: 2px`, con `border-box`: total **400 px**, contenido 400 − 30 − 4 = **366 px**.

### Márgenes que se solapan

Las reseñas (`.resena`) están una debajo de otra con `margin: 20px 0`. El espacio entre ellas es de **20 px y no de 40**, porque los márgenes verticales de bloques hermanos se colapsan y se queda el mayor.

En cambio, las `.tarjeta` (también tienen `margin: 20px 0`) están dentro de `.galeria`, que es `display: flex`: en un contenedor flex los márgenes **no** se colapsan.

![evidencia](capturas/cap7.jpeg)

---

## 7. Selectores

### Pseudoclase estructural: `:nth-child(even)`

```css
.tabla tbody tr:nth-child(even) td{
    background-color: var(--fondo-cafe-oscuro);
}
```

`:nth-child(even)` cuenta **todos los `<tr>` hermanos dentro del mismo `<tbody>`** (cualquier etiqueta hermana cuenta, no solo `tr`) y se queda con las posiciones 2, 4, 6...

En la tabla de tarifas el `<tbody>` tiene 5 filas:

| Posición | Fila | ¿Par? |
|---|---|---|
| 1 | PlayStation | no |
| 2 | Xbox Series X | **sí** |
| 3 | Xbox One | no |
| 4 | Xbox 360 | **sí** |
| 5 | Nintendo | no |

Aplica la regla a los `<td>` de las filas 2 y 4. La celda "Xbox" tiene `rowspan="3"` pero está escrita en la fila 2, así que pertenece a la fila 2 y también queda oscura, aunque visualmente ocupe tres filas.

`<thead>` y `<tfoot>` no cuentan porque son hijos de otro padre, no de `<tbody>`.

### Otros selectores que uso

| Tipo | Ejemplo en mi CSS |
|---|---|
| Atributo | `input[type="date"], input[type="time"]` |
| Combinador hermano general | `.tarjeta__activador:checked ~ .tarjeta__datos` |
| Combinador hermano adyacente | `.tarjeta__activador:focus-visible + .tarjeta__imagen` |
| Pseudoelemento | `.menu__enlace::before` (agrega "▸ ") |
| `:target` | `.reserva:target` (se ilumina al llegar desde el menú) |
| `:focus-within` | `.reserva__campo:focus-within label` |

Por ejemplo el target lo uso pues le da un agregado muy llamativo al visual de la pagina haciendo que se ilumine el rededor
---

## 8. Responsive

Uso un solo `@media (max-width: 480px)`. Esto cambia:

| Qué cambia | Por qué |
|---|---|
| `.cabecera__titulo`: 3.5rem → 2rem y `letter-spacing` 5px → 2px | Con 5px de espaciado entre letras el título no cabía en 360 px. |
| `.menu`: 1.5rem → 1.2rem, sin `margin-right` | El menú ocupaba demasiado ancho. |
| `.tabla`: `width: 90%` → `100%`, letra y padding más chicos | La tabla de tarifas tiene 4 columnas. |
| `.tabla__encabezado`: 2.5rem → 1.3rem | El título de la tabla se desbordaba. |
| `.galeria`: menos separación | El `gap` grande dejaba las tarjetas muy apretadas. |
| `.opiniones, .redes, .reserva`: `width: 100%` | El formulario al 90% quedaba angosto. |

Además, algunas cosas ya son flexibles sin necesidad de `@media`: `.cofre` usa `width: 100%` con `max-width: 300px`, y `.tabla-caja` tiene `overflow-x: auto` para que, si una tabla no cabe, scrollee ella sola y no toda la página.

![Celular](capturas/cap8.jpeg)
---

## 9. Problemas y soluciones

### Problema 1: la página se veía sin estilos

- **Qué pasaba:** después de renombrar carpetas, el `<link>` decía `css/styles.css` pero la carpeta y el archivo se llamaban `CSS/style.css`.
- **Por qué:** el navegador distingue mayúsculas y nombres exactos. Un solo carácter distinto hace que el archivo no se encuentre (error 404).
- **Cómo lo arreglé:** renombré con `git mv` para que coincidieran letra por letra y revisé la pestaña Network en F12.

### Problema 2: clases con errores de escritura

- **Qué pasaba:** el menú no tomaba estilos. Tenía `cabecera__Titulo`, `menu__titutlo` y `menu__listas` en el HTML, y `.cabecera__titulo`, `.menu__titulo` y `.menu__lista` en el CSS.
- **Por qué:** CSS distingue mayúsculas y compara el nombre exacto, y no avisa si no encuentra la clase.
- **Cómo lo arreglé:** busqué cada clase con `Ctrl + F` en el HTML y en el CSS hasta que coincidieron.

### Problema 3: labels que no hacían nada

- **Qué pasaba:** al hacer clic en "¿Qué día nos acompañarás?" no se enfocaba el campo.
- **Por qué:** el `label` tenía `for="dia"` pero el `input` tenía `id="fecha"`.
- **Cómo lo arreglé:** igualé `for` e `id` (`fecha` y `consola-reserva`).

---

## 10. Capturas

### Página de inicio

| Escritorio | 360 px |
|---|---|
| ![Inicio escritorio](capturas/pc-1.jpeg) | ![Inicio 360](capturas/360-1.jpeg) |

### Consolas y reservas

| Escritorio | 360 px |
|---|---|
| ![Consolas escritorio](capturas/pc-2.jpeg) | ![Consolas 360](capturas/360-2.jpeg) |
