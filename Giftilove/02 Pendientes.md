---
tags: [giftilove, pendientes]
actualizado: 2026-09-14
---
# Pendientes — con dueño

Regla: al cerrar algo, se marca `[x]` con fecha y commit, y se mueve a la
bitácora. Lo de Claude marcado **(pedirlo)** no se hace solo: por alcance
acotado espera a que Carlos lo pida.

## Carlos
- [ ] Iniciar sesión en OpenAI para el plugin de Codex: `codex login`.
- [ ] Decir cuál es el problema con el dominio con Y (`giftylove.com`). **No**
      está ligado al proyecto de Vercel (ahí sólo `giftilove.com`).
- [ ] Confirmar si el **contrato corporativo** va también en los legales del
      correo de bienvenida.
- [ ] Probar la pregunta de los hijos en el "+" y ver si sale el banner de
      Giftilunch en el Home.
- [ ] Borrar y reinstalar la app en el celular para ver el icono nuevo.
- [ ] Confirmar que el video de bienvenida se reproduce.
- [ ] Decidir si Obsidian MCP queda siempre encendido (~5,900 tokens por
      sesión) o sólo cuando se use.
- [ ] Poner el secreto `SUPPORT_PIN` en Supabase.
- [ ] Copiar al [[10 Manual de réplica]] (paso 2.6) cómo está Auth en el panel de Supabase: confirmación de correo, proveedores, Site URL y redirects. Desde las herramientas de Claude no se puede leer.
- [ ] En el panel de secretos de Supabase, ver si existen `STRIPE_WEBHOOK_SECRET`,
      `STRIPE_SUBSCRIPTION_WEBHOOK_SECRET` y `GHL_*` (sólo los nombres) y avisarle a
      Claude para actualizar [[09 Llaves y secretos]].
- [ ] Confirmar si ya cambió la contraseña de `dgcarlospedraza@gmail.com` (se pegó en el chat el 2026-09-13).
- [ ] Lista de 76: revisar Gira y gana (57), probar rifas (58), probar
      publicidad (60), probar vendedores (61), WhatsApp API de Meta (73).

## Claude Code
- [ ] **(pedirlo)** La liga `?tienda=<id>` no entra a la tienda: el efecto
      corre antes de que carguen las tiendas. Rompe el QR del vendedor. Carlos
      lo quiere ver cuando tome el rol de vendedor.
- [x] **HECHO 2026-09-14** — `ghl-sync` (ya sin PIN: sesión o llave del cron) tenía
      `verify_jwt: false` y su único candado es un PIN de 4 dígitos.
- [x] **HECHO 2026-09-14 (`685e59c`)** — `gift-payment` traía 13 PIN escritos (2 SUPER, 2 STAFF y 9 de tiendas que ya no existen) que dejan cobrar y liberar regalos. Hay que migrarla a sesión y a un secreto del cron.
- [x] **HECHO 2026-09-14** — los PIN viejos del **historial de git** ya no abren nada (todas las funciones y la base sin PIN) (se taparon en los archivos el 2026-09-14, pero los commits anteriores los conservan). Recomendación: cambiarlos por un secreto real y dejarlos inservibles.
- [x] **HECHO 2026-09-14** — Política `referral_credits_all` eliminada; referidos por RPC (`ALL USING true`): cualquiera lee y escribe créditos de referido.
- [x] **HECHO 2026-09-14** — `partner_stripe_accounts` ya no se lee sin sesión: se ven los `stripe_account_id` de los socios.
- [ ] **(pedirlo)** `membership_plans` tiene una política duplicada, `temp_open_select`.
- [x] **HECHO 2026-09-14** — Crons sin `timeout_milliseconds` (los 3 ya llevan 90 s):
      `giftilove_liberar_apartados`, `giftilove_purge_gift_videos`.
- [x] **HECHO 2026-09-14** — `email-drain` y los crons ya no usan llaves de 4 dígitos: usan la llave de la bóveda.
      secreto compartido: cambiarlas por un secreto de verdad.
- [ ] **(pedirlo)** Decidir si la herramienta `giftiMasterLove` sigue existiendo
      sin Pomelo (`membership_plans.features`).
- [ ] **(pedirlo)** Error de tipos en `src/routes/__root.tsx:146` (no bloquea el
      build).
- [ ] **(pedirlo)** 22 funciones con `p_pin`, parámetros fantasma y funciones
      zombi — ver CLAUDE.md.
- [ ] **(pedirlo)** Confirmar si `src/lib/megafono.ts` y `quota-usage.ts` se usan
      antes de borrarlos.
- [ ] **(pedirlo)** Cablear `cuota_consumir` en las herramientas que faltan
      (sólo el megáfono cuenta de verdad).
- [ ] **Suscripción recurrente de Stripe** (punto 72) — el pendiente más grande:
      bloquea el cobro del plan y las comisiones de vendedores.
- [ ] **(pedirlo — antes de cobrar)** En `subscriptions`, `IVA_AUTOMATICO` está en `false`, y el
      `success_url` por omisión de stripe-checkout/subscriptions apunta a `giftilove.lovable.app`.
- [ ] Lista de 76, pendientes de Claude: 29 (patrocinados como inventario),
      30 y 32 (campañas a miles), 45 (tarjetas recurrentes).
- [ ] Lista de 76, **servidor listo sin pantalla**: 9, 27, 46, 47, 50, 51, 52,
      53, 54, 55.

## Lovable (visual)
- [ ] 67 — **URGENTE**: no se puede entrar a Staff.
- [ ] 31, 39, 41, 42, 43, 44, 56, 59, 62, 63.

## Fase final — GHL y WhatsApp
- [ ] Averiguar con qué correo se creó GHL (no hay rastro en
      `giftiloveloyalty@gmail.com`; sospecha: `inmobiliarianah@gmail.com`).
- [x] Arreglar la seguridad de `ghl-sync` antes de poner el token (2026-09-14).
- [ ] Conectar GHL y la API de WhatsApp; los 20 avisos (punto 14);
      automatizaciones de vendedores y megáfono.
- [ ] Traer la skill `agente-whatsapp` del repo `arranque` cuando toque.

## Siguiente de la revisión "todo va a la base"
- [ ] El "código para cargar puntos" del perfil no lo lee ninguna caja: definir cómo carga puntos el staff con él.

## Decisión de Carlos
- [ ] **Aviso por correo de una solicitud de demo** (a Giftilove y al prospecto): hoy se confirma a mano desde Corporativo. Decidir si se agrega el correo automático. (2026-09-14)
- [ ] **Estrellas automáticas desde Google Maps**: requiere llave de Google Places con cobro. Mientras, se escriben a mano en Marca y video y ya salen en la portada. (2026-09-14)

## Cerrado recientemente
- [x] Segunda vuelta "todo va a la base": fuera la tienda "p2", categorías por tienda, fuera franquicias inventadas y reloj de muestra; formulario de demo de /socios guardando en la base (2026-09-14).
- [x] Revisión "todo va a la base": favoritos, perfil, referidos, avisos de tarjeta, interés en Giftilunch, plan de la demo y regreso de Stripe ya viven en Supabase; borrados 4 módulos de navegador; fuera `seed_points` (2026-09-14). Falta que Carlos lo vea en pantalla.
- [x] Estrellas: una sola columna (`rating`), con decimal, visibles en la tarjeta de la portada (2026-09-14). Falta que Carlos lo vea.
- [x] Marca y video: botón Guardar abajo, logo que sí se guarda, WhatsApp con lada, dos personas, F5 no saca; color de espectro en alta de socio (2026-09-14). Falta que Carlos lo vea en pantalla.
- [x] "Falta tu correo de contacto" salía con el correo guardado y el socio no podía escribirlo; Corporativo no tenía dónde. Arreglado y publicado (2026-09-14).
- [x] Réplica completa guardada: 28 edge functions en `supabase/functions/`, 5 migraciones del 13-sep que faltaban (ahora 137/137), foto de configuración y notas 10–14 (2026-09-14).
- [x] 26 — Correos maestros editables: Carlos lo marcó cerrado en la lista
      (visto 2026-09-14).
- [x] `package-lock.json` suelto borrado (2026-09-14).
- [x] Variable `SUPABASE_SERVICE_ROLE_KEY` en Vercel: verificada con eliminar
      socio (2026-09-14).

- [ ] **Soporte pide la contraseña de soporte (`SUPPORT_PIN`)**: el secreto lo tiene que poner Carlos en Supabase para que la consola abra. (recordatorio 2026-09-14)

## Actualización 2026-09-15 / 16

### Cerrado en esta sesión
- [x] **La liga `?tienda=` ya entra a la tienda** — corría antes de que
      cargaran las tiendas; el QR del vendedor llevaba roto desde que se
      escribió (`53d6ea0`).
- [x] **Suscripción de Stripe probada punta a punta** en modo prueba: pago,
      webhook 200, suscripción activa en la base y cancelación registrada sola.
- [x] **Ya hay botón para pagar el plan** (Socio → Mi cuenta), con mes, 6 meses
      y año.
- [x] **`subscriptions` dejó de estar abierta a internet** (createCheckout y
      status exigen sesión y ser dueño de la tienda).
- [x] **Comprobante de pago** (`pago_recibido`): no existía ningún correo de
      pago exitoso.
- [x] **De dónde viene cada cliente**: QR por tienda, compartir, mostrador y
      ubicación, todo escrito por el servidor.
- [x] **Buscador de clientes en el servidor**, con paginado.
- [x] **Recortador de fotos** con tiradores en las 4 pantallas que suben
      imágenes.

### Nuevo — Carlos
- [ ] **Correr Corporativo → Planes → "Crear o actualizar los precios"**. Sin
      eso, pagar 6 meses o el año responde "no existe el precio
      giftilove_gold_semestral". Repetirlo cada vez que cambie un precio o un
      descuento.
- [ ] **Que cada socio ponga su pin de ubicación** (Socio → Mi código QR →
      "Estoy aquí ahora"). Hoy **ninguna tienda tiene coordenadas**, así que la
      sugerencia por ubicación no tiene qué sugerir.
- [ ] **Pasar Stripe a modo real**: llaves live + registrar el webhook de
      producción con los mismos eventos.
- [ ] Probar en pantalla: Mi cuenta, el QR (escanear con otro teléfono y darse
      de alta), y el recortador de fotos.

### Nuevo — Claude Code (pedirlo)
- [ ] **El vendedor no puede generarle la liga de pago a un prospecto** desde
      su panel; hoy sólo el socio paga desde su cuenta.
- [ ] **La tienda demo tiene dos verdades de plan**: `partner_subscriptions`
      dice `prueba` y `partners.membership_plan_id` dice `estandar`.
- [ ] `IVA_AUTOMATICO` sigue en `false` en `subscriptions` (hay que prenderlo
      antes de cobrarle a un socio real: sin él, el recibo no desglosa IVA).

### 2026-09-17
#### Carlos
- [ ] Decidir si el **% de entorno ordena el directorio** del cliente y si
      Giftilove recibe **aviso de tiendas vacías** (¿desde qué %?).
- [ ] Probar en pantalla: subir logo y luego portada y dar F5; el indicador de
      entorno en el Dashboard; la ventana "Mostrar QR" con letra grande.
#### Claude Code (anotado, no pedido)
- [ ] La vista previa del home sin tienda (`og:image` de `__root.tsx`) sigue
      siendo una captura vieja de `lovable.app`.
#### Cerrado hoy
- [x] Logo que se borraba al subir la portada.
- [x] Favicon por área (staff / socio / corporativo).
- [x] Vista previa de "Recomendar" con la portada de la tienda.
- [x] Letra más grande en tarjetas y QR.
- [x] Indicador animado de entorno lleno (socio) y vista de la red (Corporativo).

### 2026-09-17 (ranking)
#### Carlos
- [ ] Crear el paquete de 3 meses en Corporativo → Membresías → Formas de pago y
      luego tocar **"Crear o actualizar los precios"** (si no, pagar esa forma
      responde "No existe el precio…").
- [ ] Revisar los pesos del ranking (60 / 25 / 15) en Corporativo → Red de socios.
- [ ] Decir desde qué % llega el aviso de tiendas vacías.
#### Claude Code (anotado)
- [ ] El directorio todavía baja la lista completa de `partners`: con miles de
      tiendas hay que paginarla (el orden y la búsqueda ya son del servidor).
#### Cerrado
- [x] El % de entorno ordena el directorio (con planeación y pago por adelantado).
- [x] Buscador de tiendas por nombre en el servidor.
- [x] Formas de pago dinámicas de punta a punta.

### 2026-09-17 (corazones y avisos)
#### Carlos
- [ ] Probar con tu cuenta de cliente: marcar un corazón en una tienda, tocar el
      corazón de arriba (sube arriba), dar F5 (sigue encendido).
- [ ] Desde el 19-09 pueden empezar a llegar correos "tiendas que se están
      descuidando" (la primera medición real es el 18-09).
#### Después (Carlos lo decidió)
- [ ] Paginar la lista del directorio cuando existan muchas tiendas.
#### Cerrado
- [x] Aviso de tienda vacía: 7 días sin publicar o % que baja.
- [x] Favoritas primero guardado en la cuenta y ordenado en el servidor.

### 2026-09-17 (dona y categorías)
- [ ] Carlos: ver en el panel del socio la dona debajo del menú y el indicador
      plegable del Dashboard (no se pudo ver sin contraseña).
- [ ] Carlos: confirmar si el megáfono cuenta para el % o se queda sólo como
      "muy recomendable".
- [x] Indicador plegable, dona en el menú y categorías línea / opcional / muy
      recomendable.

## Demo de ventas (2026-09-18)
- [ ] **Carlos decide:** ¿el vendedor ve Corporativo? Hoy NO (vería clientes y tiendas reales). Opción: vitrina de Corporativo sólo con datos de la demo.
- [ ] GHL: sincronizar `prospectos` (ghl_estado='esperando_ghl') como clientes potenciales con etiqueta por vendedor; automatizaciones de vendedores y megáfono (fase GHL).
- [ ] Al pasar Stripe a real: la demo cobra con llaves de PRUEBA (secreto aparte + webhook de prueba) para que ninguna cita cobre de verdad.
- [ ] Probar en pantalla la cita completa: clave → cliente en /demo → cupón desde Socio → canje con cámara en Staff.
- [ ] Reenviar a GL-001 el correo de bienvenida corregido (sólo con permiso de Carlos).

- [x] (2026-09-18) Decidido: el vendedor NO ve Corporativo. Cada vendedor con su tienda demo.
- [x] (2026-09-18) Reenviado a GL-001 el correo de bienvenida corregido (con permiso de Carlos).
- [ ] WhatsApp de la demo (y de todo) cuando entre GHL.
- [ ] `profiles.has_kids_in_college` ya no se pregunta: decidir si el banner de "papá detectado" se basa mejor en tener hijos dados de alta en Giftilunch.

- [ ] **Carlos**: ampliar en Membresías los giros de ruleta al mes de cada paquete: "Ruleta · giros que reparte tu staff" y "Ruleta · giros canjeados con puntos". Hoy son 4/12/14/18 (2026-09-18).

- [ ] **Poker 5 · torneos** (eliminatorias con byes y gran premio). Lo demás de Poker 5 ya está (2026-09-18).
- [ ] **Carlos → Lovable**: prompt de diseño visual de Poker 5, sólo diseño y sin tocar lógica (2026-09-18).
- [ ] **Carlos**: ajustar en Membresías "Mesas de Poker 5" y "Ruletas creadas" (hoy 4/12/14/18; se gastan al publicar/crear).
- [ ] **Función huérfana `poker5-pago`** sigue desplegada en Supabase sin uso (su RPC ya no existe; la reemplazó `cupon-pago`). El conector no puede borrar funciones: Carlos la borra en Supabase → Edge Functions (2026-09-19).
- [ ] **Carlos: probar en pantalla** crear una mesa de Poker 5 y una ruleta y ver que baja su contador; eliminar una y ver que desaparece (2026-09-19).

- [ ] **Dados**: invitar a un amigo (el spec lo pide "como Poker 5"); logo propio para la mesa (hoy usa el logo de la tienda). (2026-09-19)
- [ ] **Carlos**: crear unos dados en Socio → GiftiGames → Dados, repartir un tiro desde Staff y tirarlo en la app. (2026-09-19)
- [ ] **Decidir**: Megáfono Pro hoy viene en Premium **y** Gold (1 al mes); Carlos dijo "pro sólo gold lo tiene". Si es así, se cambia en Membresías. (2026-09-19)

- [ ] **Carlos: activar la subcuenta de GHL** (`mWmY3Bbay46sm9FJFsjA`). Los secretos ya están en Supabase, pero GHL responde "Location is not active". Luego avisar para probar `ghl-sync` y empezar la fase GHL/WhatsApp. (2026-09-19)

- [ ] **Carlos (GHL, 2026-09-19):** (1) reparar `claude_desktop_config.json` (bloque dentro de `mcpServers`, URL `/mcp/`, `Bearer`); (2) `claude mcp add` de GHL para la pestaña Code; (3) en Supabase: `GHL_LOCATION_ID = KNRTyK8mtT5zcmtbZF6t` y `GHL_PIT` = el token nuevo. Luego Claude regenera `GHL_FIELD_MAP` y prueba `ghl-sync`.

- [x] **GHL conectado** (2026-09-19): subcuenta Giftilove Loyalty, secretos en Supabase, MCP en Claude Code, 5 campos creados.
- [ ] **Fase GHL/WhatsApp**: separar qué avisos salen por GHL (WhatsApp del megáfono) de los de correo antes de programar el `drain`; armar los workflows en GHL (se disparan con etiqueta `giftilove-<kind>`); conectar WhatsApp en la subcuenta.
- [ ] (Opcional) Carlos puede borrar el secreto `GHL_FIELD_MAP` de Supabase: ya no se usa salvo respaldo.

- [x] Avisos separados: correo (email-drain) y WhatsApp (ghl-sync `whatsapp`) con colas distintas; envío encendido (2026-09-19).
- [ ] **Carlos**: conectar el número de WhatsApp en la subcuenta Giftilove Loyalty, iniciar sesión en GHL en el Chrome y pasarle a Claude el número (para darlo de alta como remitente). Luego Claude arma el workflow `giftilove-megafono` con la IA de GHL y prueba con `whatsappPrueba`. (2026-09-19)

## Caja y seguridad (abiertos al 2026-09-20)

- **Decisión de Carlos, pendiente:** desde qué monto de ticket se pide autorización
  del gerente, y si el gerente puede autorizarse a sí mismo (recomendación: **no**,
  es justo lo que falló en el caso de robo de Mancino's Pizza que se investigó).
- [x] ~~`user_coupons` no tiene `redeemed_by`~~ **CERRADO 2026-09-25.** Ya escriben
  en la bitácora los cinco caminos del personal: carga de puntos, cupón entregado
  en la mesa, cupón canjeado en caja, sello y tarjeta completada, regalo cobrado en
  efectivo, y giro o tiro repartido. Migraciones `20260925162607`, `20260925172408`
  y `20260925172603`.
- **No hay turnos ni cierre de caja**, ni topes de velocidad por empleado.
- **No hay diseño de caja para tablet/computadora**: el área de staff es de una
  sola columna, pensada para celular. Carlos lo pidió tipo DiDi.
- **`StaffScanner.tsx` es código muerto** (no lo importa nadie).
- **El rol `gerente` sigue con 0 filas**: se va a estrenar con esto.

## WhatsApp / GHL (abiertos al 2026-09-20)

- Plantilla de WhatsApp aprobada por Meta (la primera salió con las variables
  pegadas, que es motivo de rechazo; hay una segunda redactada lista).
- Armar el workflow `giftilove-megafono` en GHL (necesita a Carlos con sesión
  abierta) y agregarle la salida por "BAJA", que Meta exige.
- Dar de alta el remitente en `whatsapp_remitentes` — **hasta entonces no sale ni
  un WhatsApp**, a propósito.
- Verificación del negocio ante Meta para pasar de 250 a 2,000 conversaciones/24 h.

## Bloque 1 "lo roto" — CERRADO el 2026-09-24

Orden que aprobó Carlos: **Bloque 1 (lo roto) → Bloque 5 (Corporativo) →
Bloque 2 (catálogo para regalar) → banners.** Bloque 1 queda cerrado.

- [x] La app abre en el home al volver a entrar, y F5 sigue sin sacar de donde
      estás (`57bcbee`).
- [x] Crear un regalo de paga ya no exige tener Stripe: estaba **escondido, no
      roto** (`8107812`).
- [x] Fechas de inicio y fin en las piezas + el cupón de cumpleaños se suelta
      solo (`f618b07`, `b5c5198`).
- [x] La liga de cobro es nuestra y corta: `giftilove.com/pagar/XXXXXXXX`
      (`8ef74cc`).
- [x] Stripe Connect: lo conecta Giftilove desde Red de socios, con la prueba
      del peso (`1555bc4`).

### Lo que le toca a Carlos de esto
- [ ] **Probar la conexión de Stripe con un socio de verdad**: Corporativo →
      Red de socios → abrir el socio → "Dónde recibe su dinero". Yo no puedo
      hacerlo: las acciones de conectar exigen sesión de corporativo y esa
      contraseña es tuya, como debe ser.
- [ ] En esa prueba, avisar si **"Mandarle $1.00 de prueba"** responde que no
      hay saldo: significa que la cuenta de Giftilove en Stripe (modo prueba)
      está en ceros y se recarga con cualquier cobro.
- [ ] Decidir si el **contacto del socio** para pedir la conexión va a
      `contacto@giftilove.com` (es lo que dice hoy la pantalla del socio) o a
      un WhatsApp.
- [ ] Sigue pendiente **el nombre de la app en la pantalla de Android**: se te
      dijo que la etiqueta del lanzador no se puede quitar; falta que digas si
      te referías a ésa o a la del splash.

### Anotado, no arreglado (alcance acotado)
- [ ] **Pasada de seguridad por las demás edge functions con `verify_jwt =
      true`**: confirmar que cada una pregunte quién llama. `verify_jwt` sólo
      comprueba que el token esté firmado, no quién es — ver [[03 Incidentes]].
- [ ] `partner_stripe_accounts` guarda el `acct_…` de cada socio; la tabla ya
      no la lee nadie sin pasar por las funciones, pero vale confirmarlo si
      alguien le agrega una política.

### Cerrado el 2026-09-25 (lista de 76)

- [x] **45 · Tarjetas recurrentes**: se renuevan solas gastando un cupo del mes;
      sin cupo o con la membresía fuera de orden, no, y el socio recibe aviso.
- [x] **55 · Corazón con avisos del socio**: había 7 avisos reales que nadie
      había visto nunca. De paso se cerró una fuga (cualquier empleado leía los
      avisos de otra tienda).
- [x] La vista previa de `giftilove.com` dejó de colgar del CDN de Lovable.
- [x] La bitácora de caja cubre los cinco caminos del personal.
- [x] Un solo método de fecha en todos los creadores (y tres bugs que tapaba).
- [x] Los juegos cuentan como uno en el % de entorno; insignia de cupones.

#### Para que Carlos marque en el artifact (ya están hechos hace días)
9 · 39 · 43 · 44 · 46 · 47 · 50 · 55 · 59. No los palomeo yo: marcar es suyo.

#### Lo que queda de Claude en la lista
- [ ] **29** patrocinados como inventario suelto.
- [ ] **30 + 32** campaña a miles. **OJO**: `launch_game_campaign` ya reparte a
      toda la base, pero **sin mirar `accepted_marketing_at`**. Ya repartió 7
      giros cuando sólo 3 personas habían aceptado promociones. Y el 32 es una
      decisión de producto que Carlos ya escribió (cupón condicionado o banner,
      autorizado, con control de cuántos): conviene hacer las dos en el mismo
      movimiento.
- [ ] **52** relevo de la cortesía: **revisar si sigue teniendo sentido**, la
      cortesía se mudó al banner principal el 17-09 y ese carrusel ya lleva
      varias tarjetas.

#### Descuido mío que Carlos tiene que revisar
- [ ] La tarjeta de sellos de Dunia **"Rebanada de pizza pepperoni"** quedó con
      el vencimiento de omisión (16 de octubre) tras una prueba en la que no
      guardé su valor original. Si era otro, corregirlo a mano.
