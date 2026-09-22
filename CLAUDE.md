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
- **Ignacio**: también la tiene en el celular. Revisa los datos y la planilla, y
  también carga movimientos.

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
  explícito de Ignacio. Una vez que da el OK, se publica directo en `main`:
  no quiere versiones de prueba aparte.
- Antes de publicar, probar el cambio en Chromium (Playwright) simulando la
  planilla, incluido el caso sin señal.
- No hace falta botón de "buscar actualización": la app se actualiza sola.
- Si algo puede hacer perder datos cargados, avisarlo claramente y primero.

## El proyecto

- `index.html`: toda la app en un solo archivo (HTML, CSS y JS juntos), sin
  librerías externas, sin build y sin servidor. Se publica en
  https://ignaberga.github.io/LA-EMILIA-SAS/ y se instala como acceso directo en
  la pantalla de inicio. No es app de tienda.
- `README.md`: instrucciones para la familia (instalar, vincular la planilla,
  actualizar).
- `apps-script/Code.gs`: copia de respaldo del **Apps Script** que conecta con
  la planilla. El que funciona de verdad está pegado dentro de la planilla
  (Extensiones → Apps Script) y lo actualiza Ignacio a mano. Si se cambia, darle
  los pasos: pegar, guardar y **Gestionar implementaciones → Nueva versión**
  (nunca "Nueva implementación", que cambia el link). Mantener el archivo del
  repo igual al que está en la planilla.

### Cómo funcionan los datos

- La **planilla de Google es la base de datos real**, compartida por los tres
  teléfonos. La app lee con `GET ?action=get_all` y escribe con `POST` (se lee
  la respuesta `{ok:true}`) con las acciones `add`, `delete`, `config_add` y
  `config_remove`. Todas se pueden repetir sin duplicar. `resync_all` se sacó
  a propósito: borraba la planilla y la reemplazaba con lo de un celular.
- **Cola de pendientes** (`la_emilia_pendientes_v1`): cada cambio se guarda
  primero en el celular y se manda en orden, con reintentos (cada 30 s, al
  volver la señal y al abrir la app), hasta que la planilla confirma. Al leer
  la planilla, los pendientes se aplican encima, así nada cargado sin señal
  desaparece. En Inicio se ve un cartel con la cantidad de pendientes.
- Cada teléfono guarda una copia local en `localStorage` (`la_emilia_cache_v1`)
  para abrir al instante y funcionar sin señal.
- La dirección del Apps Script (la que termina en `/exec`) se guarda en
  `localStorage` (`la_emilia_sheet_url`) y además viaja en el **link de
  instalación**: `…/LA-EMILIA-SAS/#vincular=<dirección>&quien=Daniel`. Ese link
  queda grabado en el ícono de la pantalla de inicio, así el celular no se
  desvincula nunca (en iPhone, Safari y el ícono guardan datos por separado).
  No borrar el `#…` de la dirección ni agregar un manifest con `start_url`,
  porque se perdería.
- Personas que cargan: `Daniel`, `Javier` e `Ignacio` (constante `PEOPLE`).
  Cada teléfono recuerda quién lo usa (`la_emilia_persona`: una de ellas o
  `preguntar`) y no lo vuelve a preguntar al cargar.
- Los links de instalación **no se generan dentro de la app** (Ignacio no quiere
  alargar Config): se los arma Claude en el chat cuando Ignacio pasa la
  dirección `/exec`, y nunca se guardan en el repo.
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
- **Textos de la app en castellano rioplatense y con tildes.** Los valores que
  ya están guardados en la planilla (por ejemplo la categoría "Insumos
  agricolas") no se renombran desde el código, porque dejarían huérfanos a los
  movimientos viejos.
- **Borrar un movimiento siempre pide confirmación** ("¿Estás seguro…?",
  mostrando categoría, monto y fecha).
- Ninguna sincronización puede borrar gastos de la app ni de la planilla en
  forma masiva. Lo que carga Daniel tiene que aparecerle a Ignacio y en la
  planilla, y viceversa.
- En iPhone, Safari y el ícono de la pantalla de inicio guardan los datos por
  separado, e iOS puede borrar datos de sitios poco visitados. Tenerlo en
  cuenta en todo lo que dependa de `localStorage`.

## Historial de decisiones

- Septiembre 2026: se agregó la cola de pendientes, el link de instalación, la
  persona fija por celular, la confirmación al borrar y las tildes. Se sacó
  "Sincronizar todo". Se corrigieron los nombres cruzados: en el código
  quedaron `daniel`/`javier` (antes `hermano`/`padre` estaban invertidos).
- Se agregó a Ignacio como persona que carga y se sacaron de Config los botones
  para compartir links de instalación.
