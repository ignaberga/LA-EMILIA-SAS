# La Emilia — ingresos y gastos del campo

App para que Daniel y Javier carguen los movimientos del campo desde el celular.
Es un solo archivo, `index.html`, sin instalación ni servidor.

**Dirección de la app:** https://ignaberga.github.io/LA-EMILIA-SAS/

## Dónde viven los datos

**En la planilla de Google**, que es la base de datos compartida: los tres
teléfonos leen de ahí al abrir la app y escriben cada cambio, así todos ven lo
mismo. Cada teléfono guarda además una copia local para abrir al instante y
seguir cargando sin señal.

La planilla se conecta desde Config → Vincular con Google Sheets, pegando la
dirección de la aplicación web de Apps Script (la que termina en `/exec`). No es
el link de la planilla. Hay que hacerlo una vez en cada teléfono.

**Esa dirección es la llave de los datos y nunca va en este repositorio.**
Se comparte por mensaje directo, no se publica en ningún lado.

## Instalar en el celular

Abrir la dirección de arriba y agregarla a la pantalla de inicio:

- **iPhone** — en Safari: Compartir → Agregar a inicio.
- **Android** — en Chrome: menú de tres puntos → Agregar a pantalla principal.

No es una app de tienda: es la página web, así que se actualiza sola.

## Actualizar la app

Reemplazar `index.html` en este repositorio. La versión nueva llega a todos los
teléfonos la próxima vez que abren la app. El navegador guarda una copia por
unos diez minutos, así que un cambio puede tardar ese rato.

**Actualizar la app no borra datos.** Lo único que los borraría es cambiar la
dirección web, porque para el navegador sería otro sitio. Aun así, la planilla
conserva todo.
