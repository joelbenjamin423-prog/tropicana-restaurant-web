# Configuración de la web de Tropicana Restaurant

Web estática publicada con GitHub Pages. `index.html` es una copia exacta de `web-tropicana-restaurant.html`. No necesita servidor, base de datos ni compilación.

Todo el contenido que cambia (textos, fotos, carta, contacto, horario, redes, pie y eventos) está en **`contenido.json`** y se edita desde el **panel** (`admin.html`).

## Panel de la web (`admin.html`)

Dirección: https://joelbenjamin423-prog.github.io/tropicana-restaurant-web/admin.html

### Primera vez: crear la clave

1. En GitHub, abre https://github.com/settings/personal-access-tokens/new.
2. Rellena estos campos:
   - Nombre: `Panel web`.
   - Caducidad: la que prefieras. Cuando caduque, crea otra clave.
3. En **Repository access**, elige **Only select repositories** → `tropicana-restaurant-web`.
4. En **Permissions → Repository permissions**, pon **Contents** en **Read and write**.
5. Pulsa **Generate token**, copia la clave y pégala en el panel.

Con "Recordar en este dispositivo", la clave se guarda solo en ese navegador. Sin esa opción, se borra al cerrar la pestaña. Quien tenga la clave puede cambiar la web. Si se pierde o se filtra, bórrala en https://github.com/settings/personal-access-tokens y crea otra.

### Qué se puede cambiar

| Pestaña | Qué edita |
|---|---|
| Marca y colores | Nombre, subtítulo, logo (sustituye al nombre escrito), color de acento y de fondo, título y descripción para Google |
| Portada | Textos y botones, vídeo de fondo, imagen de fondo. En el título, la palabra `*entre asteriscos*` sale en dorado. |
| Destacados | Platos recomendados y fotos del carrusel con su texto |
| Nosotros | Textos y vídeo. Sin vídeo, la sección se oculta. |
| Carta | Textos de la sección, y además: <ul><li>Categorías: añadir, borrar, reordenar, cambiar la foto del banner.</li><li>Platos: nombre, descripción, precio o varios tamaños, aviso si no hay precio, foto.</li></ul> |
| Galería | Fotos, rótulo, descripción y la categoría de la carta que abre al tocarla ("Ver en la carta →"). Sin fotos, la sección se oculta. |
| Contacto y horario | Textos, teléfono, número de WhatsApp para pedidos, mensaje inicial, dirección, enlace de Maps, líneas de horario |
| Pie y redes | Texto del pie, Instagram, Facebook y TikTok. Una red vacía no aparece. |
| Eventos | Título, fecha, hora, etiqueta, precio, descripción, foto y botón de reservar por WhatsApp |

### Cómo funciona

- Hay que pulsar **Guardar cambios** (o Ctrl+S): el panel no guarda solo. Al guardar, crea un solo commit en `master` con `contenido.json` y las fotos o vídeos nuevos. La web se actualiza en 1-2 minutos.
- **Vista previa** abre la web con los cambios sin guardar, en este navegador.
- **Fotos:** se convierten solas a WebP. El lado más largo se reduce a 1600 px, a 600 px en el logo y a 1000 px en las fotos de plato. Se guardan en `assets/subidas/`.
- **Vídeos:** se suben tal cual, en MP4 de 20 MB como máximo. Para que la web cargue rápido, conviene que pesen menos de 2 MB (5-10 s, sin audio).
- **Fotos quitadas:** los archivos siguen en el repositorio y se pueden borrar a mano.
- **Si alguien guardó mientras tenías el panel abierto,** el panel avisa antes de sobrescribir.
- **Cambios en la carta:** cada plato tiene un `id` fijo, así que los pedidos ya guardados en el carrito de un cliente no se mezclan. Los platos nuevos reciben un `id` único automáticamente.

### Cómo carga la web el contenido

1. La página se oculta un instante, lee `contenido.json` y construye las secciones con esos datos.
2. Si el archivo no carga en 2,5 s, se muestra la versión escrita en el propio HTML.
3. Abriendo el HTML directamente desde el ordenador (`file://`), siempre se ve la versión del HTML, porque el navegador no deja leer el JSON.
4. `eventos.html` usa el mismo archivo para los eventos, el logo, los colores, WhatsApp y el pie.

**Qué no cambia el panel.** Estos elementos del `<head>` son fijos y están pensados para Google y para compartir el enlace:

- Los datos estructurados (JSON-LD).
- `og:image` y `og:url`.
- La URL canónica.

Si cambian la dirección, el teléfono o el horario, actualízalos también ahí a mano.

## Datos que dependen de servicios externos

| Qué | Dónde se cambia | Notas |
|---|---|---|
| Número de WhatsApp para pedidos | Panel → Contacto y horario | Formato internacional sin `+` ni guiones (`1` + `829` + número). El número no se muestra como texto en la página. |
| Dominio de la web | `<link rel="canonical">`, etiquetas `og:url` / `og:image` y el bloque JSON-LD del `<head>` | Apuntan a `https://joelbenjamin423-prog.github.io/tropicana-restaurant-web/`. Si se usa un dominio propio, hay que cambiar esas URLs. |
| Imagen al compartir el enlace | `og:image` en el `<head>` | Usa `assets/plato1.jpg`. Debe ser una URL absoluta y pública. |
| Ubicación en Google Maps | Panel → Contacto y horario | `https://maps.app.goo.gl/qY8KhEaYszNsF7xS6` |
| Instagram | Panel → Pie y redes; también el `sameAs` del JSON-LD | `https://instagram.com/tropicana_restaurant` |
| Tipografías | `<link>` de Google Fonts en el `<head>` | Si Google Fonts no carga, se usan fuentes del sistema. |

## Cómo funciona el pedido

1. Cada plato con precio tiene un botón "Agregar al carrito". Los platos con varios tamaños (pizzas, parrilladas) tienen un botón por tamaño.
2. Al pulsarlo se abre una ventana con:
   - **Guarnición** (incluida): el cliente elige una, y es obligatoria.
   - **Guarnición adicional** (opcional, solo si está activada en el panel): las que pida de más se cobran aparte. Ahora está desactivada.
   - **Comentario** (opcional, hasta 140 caracteres): por ejemplo "sin cebolla" o "término medio".
   - **Cantidad.**

   Las dos guarniciones solo aparecen si hay guarniciones en el panel y el plato no está marcado como "No lleva guarnición". Si no, la ventana solo pide el comentario y la cantidad.
3. **Un mismo plato con opciones distintas** va en líneas separadas del carrito. En el carrito se puede cambiar la cantidad y, con **Editar**, la guarnición o el comentario. En la tarjeta del plato, **−** quita una unidad de la última línea de ese plato.
4. **Guarniciones y su precio:** se configuran en el panel → Carta → Guarniciones. Si el precio del adicional está vacío, el cliente puede pedirlas igual, y el carrito y el mensaje dicen "precio por confirmar".
5. **El pedido se guarda en el navegador del cliente** (`localStorage`, clave `tropicana-pedido-v2`), así que no se pierde al recargar la página. Los pedidos guardados con la versión anterior (`tropicana-pedido-v1`) se recuperan solos. Si el navegador bloquea el almacenamiento, el carrito sigue funcionando, solo que no se guarda.
6. **El botón "Enviar pedido por WhatsApp"** abre `wa.me` con el mensaje ya escrito: platos, cantidades, guarnición, adicionales, nota, subtotales y total, con la nota "ITBIS no incluido".

Un plato sin precio muestra "Consultar" y su aviso (por defecto "Consulta disponibilidad"), y no se puede añadir al carrito.

## Pendiente de confirmar por el restaurante

- **Carta:** actualizada con la carta impresa del restaurante (noviembre 2026).
- **Guarniciones:** la lista está puesta (noviembre 2026). Cada plato lleva una sola guarnición; las adicionales están desactivadas. Para activarlas: panel → Carta → Guarniciones → "Permitir pedir guarniciones adicionales", y pon su precio (si queda vacío, el carrito y el mensaje dicen "precio por confirmar").
- **Horario:** todos los días de 8:00 a. m. a 12:00 a. m. (medianoche), confirmado por el restaurante (noviembre 2026). En los datos estructurados para Google figura como 08:00–23:59. Si cambia, actualízalo en el panel (Contacto y horario) y en el bloque JSON-LD del `<head>`.
- **Fotos por plato:** son de banco o generadas con IA, no de los platos reales del restaurante. Cuando el restaurante tenga una foto de cada plato, se pueden subir desde el panel (Carta → plato → Foto del plato).

## Página de próximos eventos (`eventos.html`)

Enlazada desde el menú ("Eventos") y el pie de página de la portada. Los eventos se añaden en el panel, en la pestaña **Eventos**. Solo el título y la fecha son obligatorios.

- **Orden:** los eventos se ordenan solos por fecha.
- **Caducidad:** desaparecen al día siguiente, en hora de República Dominicana.
- **Etiquetas:** los de los próximos 7 días llevan "Esta semana" y los del día, "Hoy".
- **Sin eventos:** la página muestra "Pronto anunciaremos nuevos eventos" con enlace a Instagram.
- **Reservar:** este botón abre WhatsApp con el nombre y la fecha del evento ya escritos.

Para ver cómo queda el diseño con eventos, abre `eventos.html?ejemplo`: muestra tres eventos ficticios marcados como "Ejemplo". Sin `?ejemplo` en la dirección nunca se muestran.

## Archivos multimedia

- **`assets/fondo2.mp4`:** vídeo de la portada, 440 KB.
- **`assets/ruleta.mp4`:** vídeo de la sección Nosotros, 730 KB.
  - Los dos vídeos están comprimidos en H.264 (CRF 29), sin pista de audio porque se reproducen silenciados, y con `faststart` para que empiecen antes de descargarse enteros.
  - Solo se reproducen cuando están en pantalla.
  - El de Nosotros no se descarga hasta que el visitante se acerca a esa sección.
- **`assets/fondo2-poster.webp`:** primer fotograma del vídeo de la portada. Se muestra al instante mientras carga el vídeo. Si cambias el vídeo desde el panel, cambia también la imagen de fondo.
- **Fotos:** la web usa versiones `.webp`. Los `.jpg` originales se conservan porque `og:image` y los datos estructurados apuntan a `plato1.jpg`.
- **Fotos de banco (`assets/demo2/`):** son de Unsplash, con licencia gratuita que permite uso comercial sin citar al autor. No son platos del restaurante; conviene cambiarlas por fotos propias cuando las haya.

| Archivo | Uso | Foto original |
|---|---|---|
| `entradas.webp` | Banner de Entradas | https://unsplash.com/photos/la8l6y8_AHg (Div) |
| `sopas.webp` | Banner de Sopas | https://unsplash.com/photos/7ax3l4quF-M (Keesha's Kitchen) |
| `ensalada.webp`, `pizza.webp` | Banners de Ensaladas y Pizzas | Unsplash |
| `mofongos.webp` | Banner de Mofongos | Generada con IA (Google Flow) por el restaurante: el mofongo cuatro quesos, recortado |

- **Foto de la galería "Pescados y mariscos" (`assets/galeria/pescados.webp`):** la del Salmón a la plancha (Unsplash, https://images.unsplash.com/photo-1519708227418-c8fd9a32b7a2), recortada a 1400×700.
- **Fotos de los platos (`assets/platos/<id del plato>.webp`):** los 65 platos tienen foto, recortada a 640×400. Son de dos tipos:
  - **45 de Unsplash**, con la misma licencia gratuita. Pesan unos 50 KB cada una.
  - **20 generadas con IA** por el restaurante con Google Flow (noviembre 2026): los mofongos y los platos de la casa que no tenían foto de banco. Pesan unos 25 KB cada una.

  La lista de qué foto corresponde a cada plato está en `assets/platos/FUENTES.txt`.
- **Fotos del carrusel de Destacados (`assets/destacados/<id del plato>.webp`):** las de los 4 platos destacados, en grande (1000×1000) y con el nombre del plato. Si cambias los platos destacados en el panel, cambia también las fotos del carrusel (panel → Destacados) para que coincidan. Para cambiar una por una foto real: panel → Carta → plato → Foto del plato.

## Cambios en el código

Edita `web-tropicana-restaurant.html`, cópialo sobre `index.html`, haz commit y push a `master`. GitHub Pages se actualiza en 1-2 minutos. Los textos del HTML solo se ven si `contenido.json` no carga: para cambiar contenido, usa el panel.
