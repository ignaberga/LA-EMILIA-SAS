# CLAUDE.md — La Emilia SAS

## Qué es La Emilia SAS

Sociedad familiar formada por **Javier (el papá)** y **Daniel (el hermano)** de
Ignacio, dueño de este repositorio. Trabajan en el campo con producción mixta
(agricultura y ganadería) y además prestan servicios de siembra, trilla,
fumigación, etc.

Ignacio es **contador público** y los ayuda con la organización interna. Hasta
ahora llevaban los gastos en papel y de forma desordenada. La idea es que carguen
todo en esta app para que Ignacio arme una base de datos y, más adelante,
informes contables y de gestión.

## Quién usa la app

- **Daniel** (hermano): carga movimientos. Tiene **iPhone**.
- **Javier** (papá): carga movimientos. Tiene **Android**.
- **Ignacio**: también la tiene en el celular. Revisa los datos y la planilla.

Son tres teléfonos. Ninguno de los tres programa: hay que pensar cada cambio para
alguien que usa el celular en el campo, con poca señal y poca paciencia. Menos
pasos es mejor.

## Cómo trabajar con Ignacio

- **Ignacio no sabe programar.** Claude hace todos los cambios e Ignacio los
  prueba en el celular. Las explicaciones tienen que ser en castellano simple
  (rioplatense) y sin jerga técnica.
- **Preguntar todo antes de actuar.** Antes de cambiar código, publicar o tocar
  datos, explicar qué se va a hacer y esperar su confirmación. La idea es no
  trabajar de más en algo que no quería.
- Cualquier push a `main` **publica la app al instante para los tres
  teléfonos** (GitHub Pages). Nunca publicar un cambio de la app sin el OK
  explícito de Ignacio.
- Si algo puede hacer perder datos cargados, avisarlo claramente y primero.

## El proyecto

- `index.html`: toda la app en un solo archivo (HTML, CSS y JS juntos), sin
  librerías externas, sin build y sin servidor. Se publica en
  https://ignaberga.github.io/LA-EMILIA-SAS/ y se instala como acceso directo en
  la pantalla de inicio. No es app de tienda.
- `README.md`: instrucciones para la familia (instalar, vincular la planilla,
  actualizar).
- El **Apps Script** que conecta con la planilla de Google vive dentro de la
  propia planilla (Extensiones → Apps Script). Si hace falta cambiarlo, pedirle
  a Ignacio que pegue el código actual y darle el código nuevo para que lo pegue
  él, con instrucciones paso a paso (incluida la nueva implementación).

### Cómo funcionan los datos

- La **planilla de Google es la base de datos real**, compartida por los tres
  teléfonos. La app lee con `GET ?action=get_all` y escribe con `POST` en modo
  `no-cors` (no se puede leer la respuesta) con las acciones `add`, `delete`,
  `resync_all`, `config_add` y `config_remove`.
- Cada teléfono guarda una copia local en `localStorage` (`la_emilia_cache_v1`)
  para abrir al instante y funcionar sin señal.
- La dirección del Apps Script (la que termina en `/exec`) se guarda por
  teléfono en `localStorage` (`la_emilia_sheet_url`).
- Listas configurables: categorías de ingreso y gasto, clientes, proveedores,
  unidades y formas de pago.

## Reglas firmes

- **La dirección `/exec` del Apps Script nunca va en el repositorio** (ni en el
  código, ni en commits, ni en issues). Es la llave de los datos y el repo es
  público.
- **Mantener un solo archivo, simple y sin dependencias externas.**
- **No cambiar la forma en que se guardan los datos** (campos de un movimiento,
  claves de `localStorage`, acciones hacia el Apps Script) sin avisarle a
  Ignacio. Un cambio así puede romper la planilla o dejar datos huérfanos.
- **Actualizar la app nunca debe borrar datos.** No cambiar la dirección web
  del sitio, porque para el navegador sería otro sitio y se perdería lo guardado
  en cada teléfono.
- Una respuesta vacía o incompleta de la planilla nunca debe pisar los datos del
  teléfono (el código ya se cuida de esto en `fetchAllFromSheet`; mantenerlo).
- **Textos de la app en castellano rioplatense y con tildes.** Hoy la app está
  sin tildes y hay que corregirla.
- En iPhone, Safari y el ícono de la pantalla de inicio guardan los datos por
  separado, e iOS puede borrar datos de sitios poco visitados. Tenerlo en
  cuenta en todo lo que dependa de `localStorage`.

## Problemas conocidos y pedidos (septiembre 2026)

- En el celular de Daniel se "perdió" el link de la planilla: cargó gastos que
  no viajaron. Probablemente es el tema de iPhone con Safari y el ícono. Además,
  el aviso de "sin vincular" solo se ve en Config. Al revincular, la app hoy
  reemplaza lo local por lo de la planilla, así que lo cargado sin link se
  pierde (en este caso no importa: eran tres gastos que Ignacio tiene anotados).
- Los proveedores recién agregados a veces desaparecen. La sospecha es que la
  relectura que la app hace 2,5 s después de guardar trae la lista vieja; falta
  revisar el Apps Script para confirmarlo.
- Borrar un movimiento: preguntar claramente "¿Estás seguro de que querés
  eliminarlo?" y mostrar qué se va a borrar.
- Cada teléfono debería recordar quién lo usa (Daniel o Javier) para no
  preguntarlo en cada carga.
- En el código, los colores y los IDs de las etiquetas están cruzados: Daniel
  usa `padre`/`pPadre` y Javier usa `hermano`/`pHermano`. Hay que corregirlo
  (Javier es el papá y Daniel el hermano).
- Ignacio quiere que las actualizaciones lleguen solas o con un botón
  "Buscar actualización", sin que se borren los gastos.
