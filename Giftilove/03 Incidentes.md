---
tags: [giftilove, incidentes, lecciones]
actualizado: 2026-09-14
---
# Incidentes — causa raíz y lección

Antes de declarar algo arreglado, revisa si ya pasó aquí. Formato: síntoma →
causa raíz → arreglo → **lección**.

## "Me saca de la app" — fueron TRES causas distintas (2026-09-13)
Si vuelve, revisa en este orden:
1. **Video de bienvenida tapando la pantalla.** YouTube Shorts no se
   reconocía (cortina negra), luego se marcaba "visto" por tienda y volvía en
   cada tarjeta, y el reproductor de YouTube tenía su barra que abría YouTube.
   Commits `e3e7457`, `9b45e38`.
2. **Dos clientes de Supabase.** El de Lovable lanza excepción si falta
   `VITE_SUPABASE_PUBLISHABLE_KEY` (Vercel no la tiene); lo usaban el carrusel
   de publicidad y un middleware global. Commit `09d7054`.
3. **Guardián del botón "atrás".** Dos `useBackGuard` compartían un solo
   centinela del historial; al entrar a una tienda uno hacía `history.back()`
   sobre una entrada ajena y el navegador salía del sitio. Commit `fd84178`.
- **Lección:** la pista decisiva fue que la pestaña terminaba en
  `chrome://newtab` — una página no puede navegar ahí, sólo el navegador. Y la
  segunda: **probé "home → tarjeta" y declaré arreglado**; el error sólo salía
  con la secuencia exacta de Carlos ("+" → flecha → tarjeta). **Siempre
  reproducir la secuencia exacta que describe Carlos.**

## Correos
- **Bienvenida nunca se mandaba** — nadie la encolaba (la plantilla existía).
  Arreglo: disparador en `profiles`. Commit `f7eb6c0`.
- **El reparto entero estaba muerto** — `pg_net` corta a los 5 s y el cron no
  tenía `timeout_milliseconds`; los correos quedaban en cola sin error. Las
  únicas corridas con 200 eran con la cola vacía. **Lección:** la trampa ya
  estaba escrita en CLAUDE.md y aun así se creó el cron sin ella. No hay
  síntoma, sólo silencio.
- **Texto blanco sobre lima** — la tinta se decidía contra UNA constante de
  color. Arreglo: luminancia. `email-drain` v13.
- **Avisos al socio se tiraban en silencio** (sin plantilla se marcaban como
  enviados). 2026-09-05.

## Acceso y sesión
- **dgcarlospedraza no entraba a Corporativo** — la contraseña servía; le
  faltaba fila en `staff_members`. **Lección:** revisa `staff_members` y
  `auth.users.last_sign_in_at` antes de culpar a la contraseña.
- **giftiloveloyalty "no entra"** — era contraseña incorrecta (logs 400).
- **Etiqueta "Giftilove" en la puerta de Corporativo** — la dejó Lovable al
  renombrar `super`→`corporativo` el 4-sep. Commit `1cee19a`.
- **`partner_save` y `platform_settings_save` sin respaldo de sesión** — editar
  socios y los interruptores de plataforma estaban rotos. Commit `6518c61`.
- **`upload-video` y `giftcard-stripe` comparaban el rol con `'super'`**.
  Commits `bcf1e5f`, `0da55fe`.

## Datos y repositorio
- **Migraciones no versionadas** — 1 archivo vs 129 aplicadas. Se
  reconstruyeron desde `supabase_migrations.schema_migrations`. Commit `5caabbd`.
- **Funciones declaradas zombi por error** — se buscó sólo en `src/`; 9 edge
  functions sí las usaban. **Lección:** las edge functions no viven en el repo.
- **"Bienvenidas" mostraba credenciales inventadas** y era código muerto.
  Commit `fda8446`.
- **Video partido en dos verdades** — la URL venía de la plataforma y el
  interruptor del video propio de la tienda. Commit `fda8446`.
- **Manifest pedía un icono de 1024 que nunca existió** (el de Android).
  Commit `25de499`.

## Herramientas y configuración
- **Conectores apuntando a cuentas equivocadas** (Vercel, Resend, Gmail) —
  Google autocompletaba `inmobiliarianah@gmail.com`. **Lección:** cerrar sesión
  de Google o usar incógnito antes de autorizar; verificar la cuenta después.
- **"Supabase necesita autorización"** confundido con base inaccesible — era un
  segundo servidor (`.mcp.json`) que no puede iniciar sesión en la app de
  escritorio. Commit `a365748`. **Lección:** revisar
  `session_connectors_status` antes de afirmar que algo no conecta.
- **Obsidian MCP con HTTP 401** — se guardó literalmente el texto de ejemplo
  `PEGA_AQUI_TU_LLAVE`. **Lección:** nunca darle a Carlos comandos con textos
  para reemplazar; usar un renglón que **pregunte** el dato.
- **`sharp-cli` sobrescribió el SVG fuente** del icono y falla con rutas largas
  de Windows. **Lección:** convertir en una carpeta aparte y de ruta corta.

## 2026-09-14 — El código de las edge functions sólo existía en Supabase
- **Qué pasaba:** las 28 funciones no tenían copia en ningún otro lado, y las 5 migraciones del 13 de septiembre estaban aplicadas en la base pero no en el repo. Si se perdía el proyecto de Supabase, se perdía el backend.
- **Causa raíz:** se desplegaba con `deploy_edge_function` y se aplicaba con `apply_migration` sin escribir el archivo en el repo en el mismo movimiento.
- **Arreglo:** se bajaron las 28 con `get_edge_function` (PIN tapados) y las 5 migraciones desde `supabase_migrations.schema_migrations`. Base y repo quedan 137/137.
- **Lección:** desplegar o migrar **sin** commitear el archivo es dejar trabajo a medias. Regla en CLAUDE.md.

## 2026-09-14 — PIN escritos por todos lados, y el canje del staff roto por lo mismo
- **Qué pasaba:** 9 edge functions, 3 crons, 1 trigger y 1 fila de equipo tenían PIN; con uno de 4 dígitos se vaciaba la cola de correos o se cobraban regalos. Además `gift-payment` sólo sabía preguntar por PIN, así que el staff real (con sesión) **no podía canjear regalos**.
- **Causa raíz:** la mudanza de PIN a sesión (2026-09-04) se hizo función por función y dejó "PIN de respaldo" que nunca se quitaron.
- **Arreglo:** sesión para personas; llave generada por la base y cifrada en su bóveda para crons/triggers; PIN vaciados. Probado: con llave 200, con PIN/llave falsa/sin sesión 401.
- **Lección:** un "respaldo mientras termina la mudanza" es deuda con fecha de vencimiento: se quita en el mismo trabajo o se anota con dueño.

## 2026-09-14 — Un push a GitHub redesplegó las 28 funciones sin verify_jwt
- **Qué pasó:** Supabase está conectado a GitHub. Al subir `supabase/functions/` (commit `a9b362f`), redesplegó las 28 funciones desde el repo, y como `supabase/config.toml` no decía nada de `verify_jwt`, **todas quedaron en false** durante ~1 hora. Nadie lo sabía: no estaba anotado en ningún lado del ecosistema.
- **Impacto:** las funciones con candado propio siguieron rechazando (probado: 19 funciones contestaron con su código, 401/400), pero la pasarela dejó de exigir la llave pública. No se detectó uso indebido.
- **Arreglo:** `supabase/config.toml` con `verify_jwt` de las 28, igual que antes; push y verificación con `list_edge_functions`.
- **Lección:** GitHub es la fuente de verdad de las funciones: **push = despliegue a producción**. Revisa `config.toml` antes de cualquier push que toque `supabase/`. Las migraciones NO se aplican solas.

## 2026-09-14 — "Falta tu correo de contacto" con el correo bien guardado
- **Qué pasaba:** el panel del socio decía que faltaba el correo, pero las 4 tiendas reales lo tenían en `partners.contact_email`. El aviso leía `branding.contactEmail`, una copia que sólo se llena cuando carga `partner_config`. Además el campo estaba bloqueado ("pídelo a Giftilove") y Corporativo no tenía dónde escribirlo. De paso, el formulario de contacto del socio cargaba vacío porque leía campos que `partners.ts` no trae.
- **Causa raíz:** dos lugares para el mismo dato (la costura de siempre) + una decisión vieja que bloqueaba el campo.
- **Arreglo:** una sola lectura, directa de `partners` (`useContactoDeTienda` en `src/lib/mi-tienda.ts`); el socio lo escribe en Marca y video; Corporativo en la ficha de la tienda (`CorreoDeTiendaEditable`); `mi_tienda_contacto` acepta y valida `contact_email`.
- **Lección:** un aviso de "falta X" tiene que leer de la MISMA fuente que guarda X.

## 2026-09-14 — "Llené Identidad de marca y no se guardó nada; F5 me sacó"
- **Qué pasaba:** la marca SÍ se guardaba (partner_config de dunia tenía logo, nombre y color), pero sola, sin aviso y sólo en la configuración, no en `partners`. No había botón Guardar. Y al recargar, el rol volvía a "Cliente" porque vivía en `useState`, así que parecía que todo se había perdido.
- **Causa raíz:** guardado invisible + rol en memoria.
- **Arreglo:** un botón Guardar abajo que escribe `partners` + configuración y dice "Guardado a las HH:MM"; aviso de cambios sin guardar; vista recordada y sesión reconocida con `useMiRol()`.
- **Lección:** si algo se guarda, la persona tiene que VERLO guardarse. Y cualquier panel con sesión debe sobrevivir a F5.

## 2026-09-14 — Las estrellas nunca se veían en la portada
- **Qué pasaba:** el socio escribía estrellas en `partners.stars` (y `mi_tienda_contacto` ni las guardaba); la tarjeta del directorio lee `partners.rating`.
- **Causa raíz:** dos columnas para el mismo dato.
- **Arreglo:** todo a `rating` (migraciones 20260915020634 y la que borra `stars`), `partner-config` desplegada leyendo `rating` ANTES de borrar la columna.
- **Lección:** antes de borrar una columna, buscar también en `supabase/functions/`; y desplegar primero lo que la lee.

## 2026-09-14 — "En Corporativo, Marca y video no guarda el logo"
- **Qué pasaba:** el logo SÍ llegaba a la base, pero dentro de `partner_config.branding` (un paquete que sólo se aplica si la pantalla Marca y video está montada cuando carga). F5 regresaba al Dashboard → el paquete cargaba sin esa pantalla → el logo no se pintaba. Y la app del cliente nunca lee ese paquete.
- **Causa raíz:** un dato de toda la app guardado en un paquete de una sola pantalla + pestañas que no sobrevivían a F5.
- **Arreglo:** `partners.logo_url` (y `welcome_gift_enabled`) leídos con la lista de tiendas; `partner_branding_save` los guarda; `useVistaRecordada` en todas las pestañas/pantallas. Migración 20260915023541.
- **Lección:** si un dato lo ve más de una pantalla, es columna de su tabla. Y probar siempre con F5.

## 2026-09-14 — Datos que sólo vivían en el navegador + hoyo de puntos
- **Qué pasaba:** favoritos, perfil editado, referidos, avisos de tarjeta, interés en Giftilunch y plan de la demo se guardaban en el teléfono: en otro dispositivo no existían. El código de puntos se inventaba al azar. `seed_points` aceptaba un saldo mandado por el navegador (hasta 5,000 por tienda). Los créditos de referidos nunca sumaban puntos.
- **Causa raíz:** el prototipo se construyó con localStorage y nunca se migró todo.
- **Arreglo:** migración 20260915025032 (tablas `user_favorites`, `referral_invites`; `profiles.giftilunch_interes_at`; funciones `tarjeta_avisos_aceptar`, `referido_invitacion`, `referido_calificar`, `referido_cobrar_mio`; fuera `seed_points`, `referido_registrar`, `referido_cobrar`).
- **Lección:** un dato del cliente que no está en Supabase no existe. Buscar `localStorage` antes de dar algo por terminado.

## 2026-09-14 — "Agenda tu demo" no guardaba nada
- **Qué pasaba:** el formulario de /socios mostraba "Te enviamos la invitación por correo" y no guardaba ni mandaba nada. Además: tienda por omisión "p2" que no existía, categorías fijas iguales para todas las tiendas, franquicias inventadas en Corporativo y un reloj de muestra.
- **Causa raíz:** piezas del prototipo que nunca se conectaron a la base.
- **Arreglo:** `demo_requests` + `demo_solicitar` / `demo_solicitudes` / `demo_solicitud_actualizar` (migración 20260915030400), pestaña Corporativo → Solicitudes de demo, categorías por tienda con `partner_categories`, fuera "p2", `DEMO_CATEGORIES`, `DEMO_FRANCHISES`, `INITIAL_FRANCHISES`, `DEMO_NOW`.
- **Lección:** un botón que dice "listo, enviado" tiene que haber escrito en la base. Buscar `setSent(true)` y avisos de éxito sin llamada al servidor.


## 2026-09-14 — Corporativo → Vendedores nunca cargaba
- **Qué pasaba:** `CorporativoSellers` exigía un `pin` y el panel lo monta sin él: se quedaba en "Vuelve a entrar con tu PIN".
- **Causa raíz:** la migración de PIN a sesión cambió la función `sellers` pero no la pantalla.
- **Arreglo:** fuera el candado de PIN en la pantalla.
- **Lección:** al quitar PIN, buscar también los `if (!pin)` en pantallas.

## 2026-09-14 — Los datos de Dunia terminaron en American Burguers (la demo)
- **Qué pasaba:** con la cuenta de Giftilove, el panel de Socio recordaba para siempre la última tienda abierta (localStorage). Carlos entró a "Dunia" y quedó en American Burguers: se guardaron ahí el logo de Dunia (3 veces), su portada de pizza, su correo/teléfono/dirección/mapa y sus 4.9 estrellas. Además Dunia y la demo traían sucursales inventadas de CDMX.
- **Causa raíz:** yo (Claude) guardé la tienda elegida en localStorage para que F5 no sacara, sin distinguir entre recargar y entrar de nuevo; y el letrero de tienda era chico. Me faltaba el contexto de que American Burguers es la demo de ventas.
- **Arreglo:** sessionStorage (sólo F5), letrero "Estás editando: X" (amarillo en la demo) con "Cambiar de tienda"; migraciones 20260915032535 (demo con logo/portada propios en public/demo, contacto de Giftilove, sin datos de Dunia; sucursal real de Dunia) y la de estrellas/zona de Dunia.
- **Lección:** antes de tocar datos de tiendas, confirmar cuál es real y cuál es la demo. Recordar una tienda entre entradas es peligroso con cuentas que ven varias tiendas.

## 2026-09-15 — El megáfono no tenía a quién hablarle

**Síntoma:** el megáfono normal de cualquier tienda real alcanzaba a **cero**
personas. Carlos lo dijo en una línea: *"ahora los megáfonos no saben cuál es
la red de cada restaurante"*.

**Causa raíz:** `profiles.home_partner_id` existía desde siempre y estaba en
**NULL en las 8 cuentas registradas**. `handle_new_user` nunca recibía la
tienda, porque el alta (`registerClient`) no se la mandaba. La audiencia del
megáfono se arma con esa relación, así que la consulta era correcta y el dato
estaba vacío.

**Lección:** cuando una función "no funciona", revisar primero si el dato del
que depende **alguna vez se escribió**. Un `count(*)` de la columna habría
delatado esto meses antes.

**Arreglo:** cuatro caminos de atribución (QR, compartir, mostrador,
ubicación) escritos por el servidor en el alta, dentro de su propio `exception`
para que una atribución fallida **nunca** tumbe un registro.

## 2026-09-15 — Miré la cuenta de Stripe equivocada y documenté lo contrario

**Síntoma:** reporté que el único webhook registrado apuntaba a
`subscriptions/webhook` y que estaba roto (503 por falta de secreto).

**Causa raíz:** hay **dos cuentas de Stripe**, las dos en modo prueba, y
consulté la que **no** usa la app. La cuenta buena es *"Entorno de prueba de
GiftiLoveLoyalty"* (`acct_1U60si3Z7W1PRu8Q`), cuyo webhook apunta a
`stripe-webhook` y sí recibe todo.

**Cómo no repetirlo:** la cuenta de la app es **la que tiene precios**
(`lookup_key` `giftilove_*`) y las sesiones `cs_test_…`. Si una cuenta no tiene
precios, la app nunca la usó. Se corrigió la nota en CLAUDE.md el mismo día.

**Lección:** antes de escribir una conclusión sobre un servicio externo con
varias cuentas, confirmar **en cuál** se está mirando.

## 2026-09-15 — `subscriptions` estaba abierta a internet

`verify_jwt: false` es correcto para el `/webhook` (lo llama Stripe), pero las
acciones `createCheckout` y `status` no comprobaban quién llamaba: cualquiera
podía generar ligas de pago para cualquier tienda y **leer el estado de
suscripción de cualquier socio**. Se detectó al ir a ponerle el botón de pago.
Cerrado el mismo día: sesión + `puedo_administrar_equipo`, verificado en
producción (401 sin sesión).

## 2026-09-17 — El corazón de arriba se olvidaba
- **Síntoma posible:** encender "mis favoritas primero", dar F5 o abrir en otro
  teléfono y encontrarlo apagado; sin cuenta, tocarlo no hacía nada.
- **Causa:** el corazón de arriba vivía en un `useState` del navegador, justo lo
  que prohíbe la regla "todo va a la base". Los favoritos sí estaban en la base
  (`user_favorites`), pero nunca se habían usado: la tabla tenía 0 filas.
- **Arreglo:** `profiles.directorio_favoritos_primero` y el orden lo da el
  servidor (`directorio_orden` / `tiendas_buscar` con `p_favoritos_primero`).
- **Lección:** un botón de filtro también es un dato si la persona espera
  encontrarlo igual al volver.

## 2026-09-24 — `verify_jwt = true` no es un candado

**Qué pasaba.** `stripe-connect-onboarding` y `stripe-connect-status` estaban
declaradas con `verify_jwt = true` y eso se leía como "ya está protegida". No lo
estaba: **la llave anon ES un JWT válido** y va escrita en el navegador de
cualquiera que abra la app. Con esa llave se podía leer el estado de cobro de
CUALQUIER socio (incluido el id de su cuenta bancaria en Stripe) y crear
cuentas de Stripe a nombre de CUALQUIER tienda.

**Causa raíz.** Confundir "Supabase exige un JWT" con "Supabase exige una
sesión". `verify_jwt` sólo comprueba que el token esté firmado, no **quién** es.

**Lección, y aplica a toda edge function del proyecto:** el candado se pregunta
**adentro**, con la sesión de quien llama (`soy_corporativo`,
`puedo_administrar_equipo`, `puedo_en_tienda`). Es el mismo patrón que ya se
había aplicado a `subscriptions` el 2026-09-15 — este par se quedó atrás porque
nadie las estaba mirando.

**Daño real:** ninguno conocido. La tabla tenía una sola fila y nunca se
completó un alta.

**Arreglo:** las dos contestan 410 y su trabajo vive en `stripe-connect`, que
pregunta por dentro. Y la lectura se partió en dos: `tienda_puede_cobrar` (un
sí/no, abierto, para quien va a comprar) y `stripe_connect_estado` (el detalle,
sólo para quien manda en esa tienda). De paso se quitó un viaje a Stripe en
cada pantalla de compra.

**Pendiente del mismo patrón, anotado:** vale la pena una pasada por las demás
edge functions con `verify_jwt = true` para confirmar que cada una pregunta
quién llama. Ver [[Bitácora/2026-09-24]].
