# Configuración de la web de Tropicana Restaurant

Web estática de un solo archivo (`index.html`, copia exacta de `web-tropicana-restaurant.html`) publicada con GitHub Pages. No necesita servidor, base de datos ni compilación.

## Datos que dependen de servicios externos

| Qué | Dónde se cambia | Notas |
|---|---|---|
| Número de WhatsApp para pedidos | `WHATSAPP_NUMBER` en el `<script>` final, y los enlaces `wa.me/18296026068` de la navegación, el hero y el pie | Formato internacional sin `+` ni guiones (`1` + `829` + número). El número no se muestra como texto en la página. |
| Dominio de la web | `<link rel="canonical">`, etiquetas `og:url` / `og:image` y el bloque JSON-LD del `<head>` | Apuntan a `https://joelbenjamin423-prog.github.io/tropicana-restaurant-web/`. Si se usa un dominio propio, hay que cambiar esas URLs. |
| Imagen al compartir el enlace | `og:image` en el `<head>` | Usa `assets/plato1.jpg`. Debe ser una URL absoluta y pública. |
| Ubicación en Google Maps | Enlace "Ver en Google Maps" en Contacto | `https://maps.app.goo.gl/qY8KhEaYszNsF7xS6` |
| Instagram | Pie de página y `sameAs` del JSON-LD | `https://instagram.com/tropicana_restaurant` |
| Tipografías | `<link>` de Google Fonts en el `<head>` | Si Google Fonts no carga, se usan fuentes del sistema. |

## Cómo funciona el pedido

1. Cada plato con precio tiene botones **−** y **+**. Las pizzas tienen un control para **Grande** y otro para **Pequeña**.
2. El precio sale del atributo `data-price` de cada `.qty-controls`, y el nombre que llega a WhatsApp, de `data-name`.
3. El pedido se guarda en el navegador del cliente (`localStorage`, clave `tropicana-pedido-v1`), así que no se pierde al recargar la página. Si el navegador bloquea el almacenamiento, el carrito sigue funcionando, solo que no se guarda.
4. El botón "Enviar pedido por WhatsApp" abre `wa.me` con el mensaje ya escrito: platos, cantidades, subtotales y total, con la nota "ITBIS no incluido".

Para **añadir un plato**, copia una tarjeta `.menu-card` de la misma categoría y cambia el nombre, la descripción, el precio visible, `data-id` (debe ser único), `data-name` y `data-price`.

## Pendiente de confirmar por el restaurante

- **Pizza de pepperoni:** la carta original indica `RD$85 / RD$175`, en orden inverso al de las demás pizzas (grande primero). Parece una errata, así que la web muestra "Consultar" y no permite añadirla al carrito. Cuando se confirmen los precios, convierte esa tarjeta al formato de las otras pizzas (dos `.variant-row`).
- **Costillas nuevas:** la carta original no muestra precio. Aparece como "Consulta disponibilidad", sin botón de compra.
- **Horario del fin de semana:** la fuente solo dice "Sáb–Dom hasta las 10:00 p. m.", sin hora de apertura. Por eso los datos estructurados para Google solo incluyen el horario de lunes a viernes.
- **Fotos por plato:** las fotos disponibles son genéricas y no corresponden a platos concretos de la carta. Por eso el carrusel muestra cada foto con su propia descripción, no con el nombre de un plato. Si el restaurante envía una foto de cada plato, se pueden añadir a las tarjetas de la carta.

## Archivos multimedia

- `assets/fondo2.mp4` (vídeo del hero) y `assets/ruleta.mp4` (sección Nosotros). Los vídeos solo se reproducen cuando están en pantalla, y el de Nosotros no se descarga hasta que el visitante se acerca a esa sección.
- `assets/*.jpg`: fotos de la galería y del carrusel, de 1400 px de ancho, con carga diferida.
- Para comprimir más los vídeos hace falta una herramienta externa (por ejemplo, HandBrake o ffmpeg), que no está instalada en este equipo.

## Publicar cambios

Edita `web-tropicana-restaurant.html`, cópialo sobre `index.html`, haz commit y push a `master`. GitHub Pages se actualiza en 1-2 minutos.
