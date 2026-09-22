# La Emilia — ingresos y gastos del campo

App para que Daniel y Javier carguen los movimientos del campo desde el celular.
Es un solo archivo, `index.html`, sin instalación ni servidor.

**Dirección de la app:** https://ignaberga.github.io/LA-EMILIA-SAS/

## Dónde viven los datos

**En la planilla de Google**, que es la base de datos compartida: los tres
teléfonos leen de ahí al abrir la app y escriben cada cambio, así todos ven lo
mismo. Cada teléfono guarda además una copia local para abrir al instante y
seguir cargando sin señal.

Lo que se carga sin señal queda guardado en el celular como pendiente y se manda
solo cuando vuelve la conexión. Mientras haya pendientes, en Inicio aparece un
cartel amarillo que dice cuántos cambios faltan enviar. Si el celular no está
conectado a la planilla, aparece un cartel rojo.

La conexión con la planilla es la dirección de la aplicación web de Apps Script
(la que termina en `/exec`). No es el link de la planilla.

**Esa dirección es la llave de los datos y nunca va en este repositorio.**
Se comparte por mensaje directo, no se publica en ningún lado.

## Instalar en el celular (una sola vez)

1. Desde un celular ya conectado, entrar a **Config → Planilla de Google** y
   tocar **Compartir link de instalación para Daniel** (o para Javier).
   Mandarlo por WhatsApp.
2. La persona abre ese link y, **desde esa misma página**, agrega la app a la
   pantalla de inicio:
   - **iPhone:** en Safari, Compartir → Agregar a inicio. Si el link se abrió
     dentro de WhatsApp, primero tocar el ícono de Safari para abrirlo ahí.
   - **Android:** en Chrome, menú de tres puntos → Agregar a pantalla principal.
3. Listo: el ícono queda con la conexión a la planilla y el nombre de la persona.
   No hay que pegar nada, ni ahora ni después.

Si ya había un ícono viejo, borrarlo antes y usar solo el nuevo.

No es una app de tienda: es la página web, así que se actualiza sola.

## Apps Script

El código que corre dentro de la planilla está respaldado en
[`apps-script/Code.gs`](apps-script/Code.gs). Al principio del archivo están los
pasos para actualizarlo sin que cambie el link.

## Actualizar la app

Reemplazar `index.html` en este repositorio. La versión nueva llega a todos los
teléfonos la próxima vez que abren la app. El navegador guarda una copia por
unos diez minutos, así que un cambio puede tardar ese rato.

**Actualizar la app no borra datos.** Lo único que los borraría es cambiar la
dirección web, porque para el navegador sería otro sitio. Aun así, la planilla
conserva todo.
