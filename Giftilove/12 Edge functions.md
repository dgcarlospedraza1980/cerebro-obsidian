---
tags: [giftilove, edge-functions, catalogo]
actualizado: 2026-09-14
---
# Edge functions — las 28, qué hace cada acción

- **Código completo (idéntico al desplegado, con los PIN tapados):** `supabase/functions/<nombre>/index.ts` en el repo. El índice con versión, `verify_jwt` y secretos está en `supabase/functions/README.md`.
- **Fuente viva:** Supabase.
- **Regla:** cada vez que se despliega una función, se actualiza su archivo en el repo en el mismo commit.

**⚠️ Push = despliegue (descubierto 2026-09-14):** Supabase está conectado a GitHub; cada push a `main` que toque `supabase/functions/` las redespliega solas desde el repo, con el `verify_jwt` que diga `supabase/config.toml`. El repo manda.

**Desde el 2026-09-14 no hay ningún PIN.** Personas → sesión. Crons y trigger → `llave` (vive cifrada en la bóveda; se valida con `llave_del_cron_valida`). Lo de abajo sobre PIN es historia.

**Cómo deciden quién puede usarlas (patrón vigente):**
1. Si viene la sesión del navegador, se le pregunta a la base **con esa sesión**: `mi_rol_de_staff`, `quien_manda(tienda)`, `puedo_administrar_equipo`, `soy_corporativo`.
2. Si en cambio viene un PIN (camino viejo, "respaldo de la mudanza"), se valida con la llave de servicio: `quien_manda_por_pin`, `staff_can_act`, `staff_by_pin`.
3. **Nunca** se compara un rol contra una cadena escrita en la función. Ese error dejó rotas varias funciones cuando `super` se renombró a `corporativo` (2026-09-04).

## Dinero (Stripe)
- **stripe-checkout** (jwt): `purpose`:
  - `giftilunch_card_topup` (child_id, amount_mxn): recarga la tarjeta de un hijo; se acredita en el webhook.
  - `wallet_topup`: modelo viejo.
  - `gift_purchase`: regalo APARTADO con `capture_method: manual`. El servicio es $20, o $10 si el regalo es de menos de $100. El apartado dura 5 días y el pago al restaurante se divide al canjear.
  - Comisión: 5% de Giftilove + colchón de Stripe (4.1% + $3).
  - ⚠️ Las `success_url` por omisión apuntan a `giftilove.lovable.app`.
- **stripe-webhook** (sin jwt; firma de Stripe): el ÚNICO webhook.
  - `checkout.session.completed`: suscripción (upsert en partner_subscriptions), gift_purchase (inserta en gifts con apartado), giftilunch_card_topup (RPC idempotente giftilunch_credit_topup), gift_card (lo ignora) o monedero.
  - `invoice.paid`: rescue_subscription + comisión del vendedor (30% la primera, 10% las recurrentes, sobre el subtotal sin IVA).
  - `invoice.payment_failed`: encola pago_rechazado, sin apagar la cuenta.
  - `customer.subscription.deleted` → cancelada. `account.updated` → estado de Connect.
- **stripe-connect-onboarding** (jwt): crea una cuenta Express MX y devuelve la liga (⚠️ URLs por omisión de lovable.app).
- **stripe-connect-status** (jwt): refresca `onboarding_complete` y `payouts_enabled`.
- **stripe-setup** (sin jwt, SUPER_PINS): crea los productos y precios mensuales con `lookup_key` (estándar 120000, premium 180000, gold 240000, tarjetas 100000 centavos; `tax_behavior` exclusive).
- **stripe-webhook-fix** (sin jwt, SUPER_PINS): `list` · `fix` (pone los 5 eventos) · `recrear` (devuelve el secreto nuevo).
- **subscriptions** (sin jwt):
  - Ruta `/webhook`: exige `STRIPE_SUBSCRIPTION_WEBHOOK_SECRET`.
  - `createCheckout` (partnerId, plan, withCards, sellerId, cardsFreeMonths): los meses gratis se dan como trial.
  - `status`.
  - ⚠️ `IVA_AUTOMATICO=false`.
- **giftcard-stripe** (jwt):
  - `create`: tarjeta pendiente_pago + checkout sin transfer.
  - `confirm`: idempotente; activa la tarjeta con su vigencia (gift_card_days).
  - `redeem`: PIN de la tienda; mueve el ledger ANTES de transferir.
  - `refundExpired`: corporativo; devuelve 90% y retiene 10%.
- **gift-payment** (sin jwt):
  - `lookup` y `canjear`: por código; captura el total y transfiere al restaurante.
  - `cobrar`: por id.
  - `liberar` (cron diario): captura sólo el servicio, suelta el resto y avisa al que mandó el regalo.
  - ✅ 2026-09-14: sin PIN. `lookup`/`canjear` con sesión de quien atiende la tienda; `cobrar`/`liberar` con la llave del cron o corporativo.

## Correo
- **email-drain** (sin jwt; llave del cron o service role):
  - Vacía `notification_queue`, 25 por corrida.
  - Plantilla editable de la base o, si no hay, la del código. Tokens: {{tienda}} {{nombre}} {{correo}} {{dias}} {{cupones_frase}} {{cupones_frase_mayus}} {{numero}} {{refCode}}.
  - Tinta por luminancia. Respeta la baja (`can_email`) y pone liga de baja + List-Unsubscribe.
  - Destinatario, en este orden: `payload.para` → correo de la tienda (partners.contact_email) → perfil.
  - Tipos con plantilla: bienvenida, coupons_expiring_d5/d2/d0, suscripcion_por_vencer, pago_rechazado, suscripcion_inhabilitada, socio_aviso, socio_cambio_de_plan, staff_alta, vendedor_aprobado.
  - Si falta la plantilla: attempts=5 con el motivo escrito.
- **password-reset** (sin jwt):
  - `pedir`: siempre contesta lo mismo (anti-enumeración); manda el enlace directo por Resend, sin pasar por la cola.
  - `enviarEnlace`: sólo corporativo.
  - `temporal`: sólo corporativo; contraseña dictable, marca password_temporal y queda en la bitácora.

## Tiendas, equipo y administración
- **socio-alta** (jwt, sólo corporativo): tienda (partner_alta) + fila de socio (staff_alta_por_correo) + cuenta de Auth + correo staff_alta con contraseña temporal. Hereda de la matriz si viene `matrizId`. Si algo falla, deshace todo.
- **socio-relevar** (jwt, corporativo): apaga al responsable anterior (`socio_relevar`), da de alta al nuevo y actualiza el contacto. El motivo es obligatorio.
- **staff-invitar** (jwt): socio o gerente dan de alta a gerente o staff. Contraseña temporal + correo. Si la cuenta ya existía, sólo la vincula.
- **admin-users** (jwt, corporativo): `list` (sólo clientes, sin equipo) · `delete` · `deleteAllCustomers`.
- **partner-config** (jwt): `load` / `save` / `seed` por sección. El contacto SIEMPRE sale de `partners` y nunca se guarda en el config.
- **support-console** (jwt): dos llaves, corporativo + `SUPPORT_PIN` (503 si falta). `open` toma una foto automática, `fix` guarda versión de soporte, `restore`, `log`.
- **offers-admin** (jwt): `quotas` · `list` · `save` · `publish` · `retire`. Hay 2 horas de gracia: retirada = el cupo regresa; vencida = el cupo se gasta. Suma los cupos de offers, games y raffles.
- **coupon-redeem** (jwt): `lookup` · `redeem`, validando contra la tienda DEL cupón. La regla "hasta mañana" la pone un trigger.
- **upload-image** (jwt): data URL → `public-images/<tienda>/<carpeta>/<uuid>`, máximo 5 MB, nunca pisa un archivo.
- **upload-video** (jwt): liga firmada a `welcome-videos`. Sin `partnerId` es el video de la plataforma y exige `soy_corporativo`.

## Juegos, rifas, regalos con video, vendedores, publicidad
- **games-admin** (jwt):
  - `staffGrant`: giro con código o asignado a un cliente.
  - `list`, `saveGame` (sin id crea una pieza nueva), `savePrizes`, `deleteGame` (sólo dentro de las 2 h).
  - `raffleList`, `raffleSave`, `raffleSetStatus`, `raffleDelete`, `raffleToggle`, `raffleCodesList`, `raffleCodesCreate`, `raffleCodeToggle`.
  - Sólo corporativo: `superList`, `launchCampaign` (→ launch_game_campaign, que gasta cuota del megáfono), `listCampaigns`.
- **raffle-draw** (jwt): `listOpen` · `ready` · `draw` (run_raffle_draw; el ganador lo elige el servidor) · `finish`.
- **gift-video** (sin jwt):
  - `subir`: liga firmada a gift-videos.
  - `adjuntar`: el video vive 6 días.
  - `ver`: liga de 2 h.
  - `purgar` (cron): primero manda la tarjeta de recuerdo con el video adjunto; después borra los cumplidos y los huérfanos.
- **sellers** (sin jwt; sesión o SUPER_PINS):
  - Del vendedor: `solicitar` · `miEstado` · `miPanel` (sólo activos) · `demoPlan` · `demoReset`.
  - Sólo corporativo: `listar` · `aprobar` · `cambiarEstado` · `guardarNotas` · `carrera` · `guardarMetas` · `porPagar` · `pagarComisiones`.
- **sponsorships** (jwt): `listCampaigns` · `saveCampaign` · `setCampaignStatus` · `offer` (ofrecer, nunca imponer) · `withdraw` · `myOffers` · `respond`.
- **ads-admin** (jwt): `list` · `save` · `setStatus` · `remove` · `place` (tienda por tienda).
- **ghl-sync** (sin jwt, SUPER_PINS): `ping` · `listFields` · `upsertContact` · `addTags` · `drain`. **Nunca ha sincronizado nada. Inseguro: arreglar antes de poner `GHL_PIT`.**

- **partner-config (2026-09-14):** la carga lee `partners.rating` y lo entrega como `stars` dentro de branding/contacto (la columna `stars` ya no existe).

## 2026-09-15 / 16 — Qué cambió

Son **29** (entró `megafono-compra`). Recordatorio: cada push a `main` que
toque `supabase/functions/` las redespliega TODAS y sube su número de versión.

| Función | Cambio |
|---|---|
| **megafono-compra** *(nueva)* | `crear` (Stripe Checkout con el total ya descontado) y `confirmar` (acredita, idempotente) |
| **stripe-webhook** | acredita megáfonos (`purpose=megafono`), aplica el plan con `suscripcion_aplicar_plan` y encola el comprobante `pago_recibido` en `invoice.paid` |
| **subscriptions** | acepta `periodicidad` (mensual / semestral / anual) y **cerró su hueco**: `createCheckout` y `status` exigen sesión + `puedo_administrar_equipo` |
| **stripe-setup** | ya NO trae los montos escritos: los lee de `membership_plans` y `billing_settings` y crea los 9 precios (3 planes × 3 periodicidades). Su botón vive en Corporativo → Planes |
| **admin-users** | acciones `search`, `resumen` y `universidades`, llamando al RPC con la sesión de quien pregunta |
| **email-drain** | arma el correo del megáfono con la pieza viva al momento de mandarlo; nuevo tipo `pago_recibido` |
| **offers-admin / games-admin** | ventana de 30 minutos; al retirar o borrar, apagan el megáfono de esa pieza |

**Ojo con las dos cuentas de Stripe:** el webhook registrado en la cuenta que
usa la app (`acct_1U60si3Z7W1PRu8Q`) apunta a **`stripe-webhook`**. El que
apunta a `subscriptions/webhook` vive en la otra cuenta y no recibe nada útil;
por eso su 503 por falta de `STRIPE_SUBSCRIPTION_WEBHOOK_SECRET` no rompe nada.

## 2026-09-17 — Formas de pago dinámicas
- **subscriptions**: acepta cualquier forma ACTIVA de `formas_de_pago` y manda
  `periodicidad` en la metadata de la sesión y de la suscripción.
- **stripe-webhook**: pasa `p_periodicidad` a `suscripcion_aplicar_plan`.
- **stripe-setup**: catálogo = plan × forma activa. **Reemplaza** los precios que
  cambiaron (precio nuevo con `transfer_lookup_key`, el viejo se apaga); antes
  no tocaba una llave que ya existía aunque el precio hubiera cambiado.

## 2026-09-17 — email-drain
- Nuevo tipo **`giftilove_tiendas_descuidadas`**: correo a Giftilove (rosa, "GIFTILOVE ·
  CORPORATIVO") con la lista de tiendas y el motivo. El destinatario viene en
  `payload.para`. Nombres de tienda escapados.

## 2026-09-18 — 31 funciones (verificado contra Supabase)
- **Nueva `demo-prospecto`** (v5, sin JWT): el cliente del vendedor entra a /demo con nombre, correo, teléfono y clave; lo guarda en `prospectos` y le da cuenta de demostración.
- **`coupon-redeem`** (v45): canje por SESIÓN del staff (`puedo_en_tienda`). Antes sólo PIN → nadie canjeaba.
- **`giftcard-stripe`** (v47): cobro de GiftiCARD por sesión; sólo en la tienda que la emitió; `master` rechazado.
- **`email-drain`** (v48): molde sin patines con logo; cuentas de demostración → correo real de `prospectos`; fichas de comisión con `$`.
- **`sellers`** (v37): `demoReset` reinicia la tienda del vendedor; `demoPlan` → 410.
- `gift-payment`, `gift-video`, `password-reset`: fuera Georgia (letra sin patines).
- Versiones de las 31 en `supabase/functions/README.md`.


## Cambios 2026-09-18 / 19
- **cupon-pago** (jwt, nueva): paga con tarjeta CUALQUIER cupón con precio (`crear` → Stripe Checkout; `confirmar` → `cupon_pagado_con_tarjeta`). Reemplaza a `poker5-pago`.
- **poker5-pago**: **huérfana**, sigue desplegada en Supabase (33 desplegadas vs 32 en el repo) pero su RPC ya no existe. El conector no puede borrar funciones: la borra Carlos en el panel.
- **coupon-redeem**: el lookup enseña precio; el canje de un cupón con precio pendiente exige `cobrado:true` ("Primero cobra $X en caja") y queda `pagado_caja`; si dice pagado con tarjeta, lo comprueba con Stripe.
- **games-admin**: `saveGame` al crear gasta 1 de `ruletas` (la demo cuenta sin frenar); `deleteGame` se puede siempre (status 'terminado', apagada, cancela megáfono, no devuelve cupo); `list` ya no trae `poker5` ni terminadas; `staffGrant` gasta `wheelGamesStaff`; `savePrizes` guarda precio del cupón ganador.
- Ojo: el push a `main` redespliega TODAS las funciones, así que el número de versión de cada una sube en cada push; el README lleva la versión aproximada, no exacta.
