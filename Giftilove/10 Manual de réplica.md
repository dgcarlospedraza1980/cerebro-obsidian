---
tags: [giftilove, manual, replica]
actualizado: 2026-09-14
---
# Manual de réplica — reconstruir Giftilove desde cero

Este manual está pensado para que **cualquier persona o IA** levante un Giftilove idéntico al de hoy (2026-09-14). Se sigue en orden; cada paso dice cómo comprobar que quedó.
Las llaves NO están aquí: [[09 Llaves y secretos]] dice cuáles hacen falta y dónde se ponen.

## Dónde está TODO lo que hace falta
| Pieza | Dónde vive la copia completa |
|---|---|
| Código de la app (React/TanStack) | GitHub `giftilove/happygift`, rama `main`. Versión estable: etiqueta `v2026.09.13` |
| Esquema de la base (tablas, funciones, RLS, triggers, crons, buckets) | `supabase/migrations/` — **150 migraciones**, 2026-08-19 → 2026-09-13, en orden |
| Datos de configuración (planes, ajustes, plantillas de correo, crons, buckets, tiendas) | `supabase/estado/configuracion-2026-09-14.json` |
| Las 28 edge functions | `supabase/functions/<nombre>/index.ts` + `supabase/functions/README.md` |
| Reglas para agentes que tocan código | `CLAUDE.md` (raíz del repo) |
| Reglas para Lovable | Knowledge del proyecto en Lovable (texto completo abajo, sección 9) |
| Decisiones, historia, pendientes, negocio | Esta bóveda: carpeta `Giftilove/` |
| Catálogos | [[11 Base de datos]] · [[12 Edge functions]] · [[13 Frontend]] · [[14 Historial y versiones]] |

## 0. Cuentas que se necesitan (todas a nombre de `giftiloveloyalty@gmail.com`)
GitHub · Supabase · Vercel · Resend (con el dominio) · Stripe · el dominio `giftilove.com` · Google (Gmail/Drive/Calendar). Opcional: Lovable, OpenAI (Codex), Obsidian.
> Trampa conocida: al autorizar con "Continuar con Google" se autocompleta `inmobiliarianah@gmail.com`. Cerrar esa sesión o usar incógnito. Ver [[03 Incidentes]].

## 1. Código
1. `git clone https://github.com/giftilove/happygift.git` (o descargar la etiqueta `v2026.09.13`).
2. Node 24 + npm 11. `npm install` (el lockfile del repo es `bun.lock`; npm también funciona).
3. `npm run build` tiene que pasar.
4. `./node_modules/.bin/tsc --noEmit -p tsconfig.json` da **un** error conocido en `src/routes/__root.tsx:146`. No bloquea el build. Si sale cualquier otro, algo se rompió.
5. **No hay tests.** Verificar = typecheck + build + probar en navegador.

## 2. Supabase (la base)
1. Crear un proyecto nuevo (región cercana a México). Anotar el `project ref`.
2. Las extensiones que se usan son `pg_cron`, `pg_net`, `pgcrypto`, `uuid-ossp`, `supabase_vault` y `pg_stat_statements`. Las migraciones las activan cuando hacen falta; si una falla por la extensión, hay que activarla desde el panel.
3. **Aplicar las 150 migraciones en orden de nombre.** Opciones: `supabase db push` con el CLI ligado al proyecto, o una por una con `apply_migration` del MCP.
   - Los `<PIN_REDACTADO>` de 5 migraciones viejas **no hay que llenarlos**: las migraciones del 2026-09-14 reprograman los crons con la llave de la bóveda, reemplazan el trigger de cobro y vacían los PIN. La llave se genera sola al aplicar `llave_del_cron_en_la_boveda_y_referidos_por_servidor`.
   - (Histórico) antes había que sustituir en 5 migraciones el texto `<PIN_REDACTADO>`:
     - Tres crons que mandan la llave a las funciones: `20260828031410_ghl_drain_schedule`, `20260830220311_email_drain_schedule` y `20260913222411_el_reparto_de_correos_deja_de_morir_a_los_5s`.
     - El trigger que cobra el regalo al canjear: `20260903051255_cobrar_regalo_al_canjear`.
     - La que sembraba PIN de equipo que ya no se usan: `20260829030055_staff_members_bound_to_store`.
     - Mejor aún: reemplazar esos PIN por un secreto real (pendiente anotado).
   - En esas mismas y en los crons, **cambiar la URL `tnkpxnpqicsmlsggisyz.supabase.co`** por la del proyecto nuevo.
4. Comprobar contra [[11 Base de datos]]: **62 tablas, 184 funciones, 19 triggers, 53 políticas, 13 crons y 4 buckets**.
5. **Datos:** con `supabase/estado/configuracion-2026-09-14.json`, comparar y poner en la base nueva:
   - `membership_plans` (4 planes: precios 0/1200/1800/2400; cuotas; `videoPropio` sólo en gold).
   - `platform_settings` (URL del video de bienvenida, interruptores, add-on de tarjetas).
   - `billing_settings`, `sales_goals` y las 8 `email_templates`. La `bienvenida` va con el texto final del JSON.
6. **Auth (panel → Authentication):**
   - Site URL `https://giftilove.com`.
   - Redirect permitido `https://giftilove.com/nueva-contrasena`.
   - Hoy no hay forma de leer desde aquí cómo están la confirmación de correo y los proveedores. ❓ Revisarlo en el panel del proyecto original y copiarlo igual.
7. **Cuentas de Corporativo:** crear el usuario en Auth y después su fila en `staff_members` (`role='corporativo'`, `partner_id` null, `pin` null, `active` true). Hoy son dos: `giftiloveloyalty@gmail.com` y `dgcarlospedraza@gmail.com`.
   > Trampa GoTrue: si se inserta un usuario por SQL, las columnas de token van en `''`, nunca `NULL`, o esa persona no puede entrar.

## 3. Edge functions
1. Desplegar las 28 desde `supabase/functions/`, **cada una con el `verify_jwt` de la tabla** del README. `stripe-webhook` y `subscriptions` DEBEN quedar en `false`.
2. Se despliegan tal cual: desde el 2026-09-14 no tienen PIN (sesión o llave del cron).
3. Secretos (panel → Edge Functions → Secrets): `RESEND_API_KEY`, `STRIPE_SECRET_KEY`, `STRIPE_WEBHOOK_SECRET`, `STRIPE_SUBSCRIPTION_WEBHOOK_SECRET`, `SUPPORT_PIN`. `GHL_*` se queda sin poner hasta la fase GHL.
4. Comprobar: `list_edge_functions` devuelve 28 ACTIVE.

## 4. Frontend apuntando a la base nueva
- `src/lib/supabase.ts`: URL y **anon key** del proyecto nuevo (son públicas; las protege RLS).
- `.env` (versionado, sólo públicas): `SUPABASE_URL`, `SUPABASE_PUBLISHABLE_KEY`, `SUPABASE_PROJECT_ID` y sus gemelas `VITE_`.
- **NUNCA** usar `src/integrations/supabase/*` (el lint lo marca). Una sola conexión: `@/lib/supabase`.

## 5. Vercel (hosting)
1. Importar el repo de GitHub. Vercel detecta la compilación sola (Vite + TanStack Start/nitro).
2. Variable de entorno `SUPABASE_SERVICE_ROLE_KEY` (Production). La usa `src/lib/supabase-servidor.ts` para pausar y eliminar socios.
3. Dominios: `giftilove.com` y `www.giftilove.com`, con el DNS apuntando a Vercel.
4. Comprobar: eliminar socio abre su diálogo sin error, y los logs de runtime salen limpios.

## 6. Resend (correo)
1. Agregar el dominio `giftilove.com` y poner sus registros DNS (SPF/DKIM) hasta que salga *verified*.
2. Crear una API key y ponerla como `RESEND_API_KEY` en Supabase.
3. Remitente: `avisos@giftilove.com`.
4. Comprobar: registrar un cliente nuevo → en ≤5 min llega "Bienvenido a Giftilove.com", con texto negro sobre lima.

## 7. Stripe (dinero)
1. Llaves de la cuenta → `STRIPE_SECRET_KEY`. Hoy sólo se usa **modo prueba**.
2. Correr la función `stripe-setup` una vez: crea los productos y precios (Estándar 1200, Premium 1800, Gold 2400 y Tarjetas 1000, con IVA por fuera).
3. Webhook → `https://<ref>.supabase.co/functions/v1/stripe-webhook`.
   - Eventos: `checkout.session.completed`, `account.updated`, `invoice.paid`, `invoice.payment_failed`, `customer.subscription.deleted`.
   - Su secreto va en `STRIPE_WEBHOOK_SECRET`. `stripe-webhook-fix` sirve para recrearlo si el secreto no aparece.
4. Stripe Connect (Express, MX) para que los socios cobren.
5. **Antes de cobrar de verdad:**
   - Encender `IVA_AUTOMATICO` en `subscriptions`.
   - Cambiar las `success_url` por omisión (hoy apuntan a `giftilove.lovable.app`).
   - Construir la suscripción recurrente (punto 72).

## 8. Crons
Los crea la migración correspondiente. Revisar que sean **13**, con los horarios de [[11 Base de datos]]. Ahí también se revisa que los que llaman funciones lleven `timeout_milliseconds` (email-drain 90000; los otros dos hoy no lo tienen).

## 9. Lovable (sólo si se usa como generador de frontend)
- Conectar el repo.
- En *Project knowledge* pegar el texto de reglas: está completo en `08 Herramientas del ecosistema` → Lovable → "Knowledge".
- Lovable **no** hace backend. Si lo necesita, deja `// TODO(backend)`.

## 10. Herramientas de trabajo de Claude (opcional, para seguir trabajando igual)
- `CLAUDE.md` y `.claude/` ya vienen en el repo: 5 subagentes y los ganchos de graphify y del cerebro.
- Obsidian:
  - Plugin "Local REST API with MCP".
  - `claude mcp add --scope user --transport http obsidian http://127.0.0.1:27123/mcp/` con la llave.
  - Copiar la carpeta `Giftilove/`.
- Graphify: `graphify update .`.
- Skills (no versionadas):
  - `npx skills add https://github.com/nextlevelbuilder/ui-ux-pro-max-skill --skill ui-ux-pro-max`.
  - Los skills de emilkowalski y los 5 de vercel-labs (ver [[08 Herramientas del ecosistema]]).

## 11. Prueba final (todo debe pasar)
1. Abrir `giftilove.com` sin cuenta: se hojean las tarjetas; tocar "+" o una tienda pide registro.
2. Registrar un cliente: sin código Giftilove a la vista, sin foto de desconocido (salen sus iniciales), y en ≤5 min llega la bienvenida.
3. Ya con cuenta, secuencia "+" → flecha atrás → tocar una tarjeta: **se queda dentro de la app**.
4. El video de bienvenida se ve una vez y sin controles de YouTube.
5. En el Perfil (+), debajo de Giftilunch, sale la pregunta de hijos en la universidad, se guarda y aparece el banner.
6. Corporativo entra con correo y contraseña; la puerta dice "Corporativo".
7. En Corporativo funcionan "Correos maestros" (vista previa) y "Correos enviados" (historial sin contenido).
8. Pausar o eliminar un socio contesta sin error.
9. `net._http_response` muestra 200 en las corridas de `email-drain`.
