---
tags: [giftilove, herramientas]
actualizado: 2026-09-14
---
# Herramientas del ecosistema

Cada herramienta tiene su sección con cuatro cosas: **qué es para Giftilove · qué se hizo · hasta dónde
llego yo (Claude) · estado**. Las llaves de cada una están en [[09 Llaves y secretos]]
(sólo dónde viven, nunca el valor). Las cuentas y los correos están en [[05 Cuentas y herramientas]].

Leyenda: ✅ verificado · ⚠️ inferido · ❓ desconocido · ❌ falta

---

## Plataforma (donde vive la app)

### Supabase: base de datos, auth, storage y edge functions
- **Proyecto:** `giftiloveloyalty01` (`tnkpxnpqicsmlsggisyz`). Es **nuestro**, no el de Lovable.
- **Qué se hizo:**
  - RLS y 129+ migraciones versionadas en `supabase/migrations/`.
  - Se quitó el PIN y se pasó a sesión (`soy_corporativo`, `mi_rol_de_staff`).
  - Correos maestros (`email_templates_listar/guardar`) e historial `correos_enviados` (sin payload).
  - Disparador de bienvenida `encolar_bienvenida_del_cliente`.
  - Video de plataforma (`welcome_video_enabled`, `video_propio`).
  - Cron de correos con timeout de 90 s.
- **Edge functions: son 28.** La fuente viva es Supabase; la copia completa está en `supabase/functions/` desde el 2026-09-14. Catálogo en [[12 Edge functions]].
  - Stripe: stripe-checkout, stripe-webhook, stripe-connect-onboarding,
    stripe-connect-status, stripe-setup, subscriptions, stripe-webhook-fix, giftcard-stripe.
  - Correo: email-drain (v14), password-reset.
  - Admin y operación: admin-users, games-admin, raffle-draw, sponsorships,
    support-console, partner-config, ads-admin, offers-admin, staff-invitar,
    socio-alta, socio-relevar, sellers.
  - Otras: ghl-sync, coupon-redeem, upload-image, upload-video, gift-video, gift-payment.
  - **Con `verify_jwt: false`** (abiertas a internet; el candado lo pone cada función):
    stripe-webhook, stripe-webhook-fix, ghl-sync, stripe-setup, subscriptions,
    email-drain, sellers, gift-video, gift-payment, password-reset.
- **Crons: 13** (lista completa en [[11 Base de datos]]). Tres llaman funciones por pg_net:
  - `giftilove_email_drain`: con timeout de 90 s ✅.
  - `giftilove_liberar_apartados` y `giftilove_purge_gift_videos`: **sin timeout** ⚠️.
  - Los tres mandan un PIN escrito en el comando (ver [[09 Llaves y secretos]]).
- **Hasta dónde llego:**
  - Con el conector de Claude leo y escribo SQL, aplico migraciones, leo y despliego
    edge functions, y veo logs y advisors.
  - **NO puedo ver ni poner los valores de los secretos** de las edge functions. Eso sólo
    se hace en el panel, y lo hace Carlos.
  - **El conector a veces pierde la autorización.** Se reconecta desde los conectores de
    claude.ai. Mientras tanto no tengo acceso a la base de datos.
- **Estado:** ✅ en producción.

### Vercel: hosting de giftilove.com
- **Equipo:** `giftilove-vercel`.
- **Proyecto:** `giftilove` (`prj_zpD8e0mGKdVrg5HMN1razE9uKfVq`).
- **Dominios:** `giftilove.com`, `www.giftilove.com` y `*.vercel.app`.
- **Qué se hizo:**
  - Se corrigió la cuenta (antes era `nahinmobiliaria`).
  - Carlos puso `SUPABASE_SERVICE_ROLE_KEY` el 2026-09-14; se verificó con "eliminar socio".
  - Cada push a `main` despliega solo.
- **Hasta dónde llego:** listar despliegues y ver logs de build y de runtime, errores y
  analítica. **NO puedo** leer ni escribir variables de entorno, ni tocar el DNS de los dominios.
- **Pendiente:** `giftylove.com` (con **Y**) **no** está en el proyecto. Falta que
  Carlos diga qué pasa con ese dominio.
- **Estado:** ✅

### GitHub: repo `giftilove/happygift`
- La rama `main` es producción. Lovable **también escribe aquí**, así que hay que hacer `git pull` antes y después.
- **Etiqueta estable:** `v2026.09.13` (commit `fd84178`).
- **Qué se hizo:** todo el trabajo de Claude se commitea y se empuja desde la máquina de Carlos
  (git + Git Credential Manager). También existe el conector "GiftiLoveConector".
- **Hasta dónde llego:**
  - Desde la máquina: push y pull.
  - Con el conector: sólo archivos completos y **sin binarios**.
- **Guardas:** el lint bloquea importar `@/integrations/supabase/*`.
- **Estado:** ✅

### Lovable: generador de frontend (y NADA más)
- **Proyecto:** `409f4680-3414-4da2-8996-6e3806e177eb`. Se sincroniza solo con GitHub y publica también en `giftilove.lovable.app`.
- **Regla de Carlos:** sólo frontend. Tenemos que poder irnos a Bolt o a otro cuando queramos.
- **Qué se hizo:**
  - Se quitaron sus 5 archivos de cliente Supabase; eran los que hacían que la app "sacara" al usuario.
  - Se escribió el Knowledge del proyecto con zonas libres y protegidas, y la convención `TODO(backend)`.
- **Hasta dónde llego:**
  - Leer archivos, diffs y mensajes es gratis.
  - `send_message` **cuesta créditos** (un reporte costó 4.8), así que se usa sólo para diseño visual.
- **Estado:** ✅ contenido
- **Knowledge del proyecto (texto completo, 2026-09-14):** ver la sección "Knowledge de Lovable" al final de esta nota.

## Correo y comunicación

### Resend: envío de correos
- **Dominio:** `giftilove.com`, verificado.
- **Remitente:** `avisos@giftilove.com`.
- **Cuenta:** corregida (antes era `monicapedraza.com`).
- **Qué se hizo:** la bienvenida al cliente ya sale y llega. Lleva texto negro sobre lima, el copy final, los legales y la baja simulada.
- **Hasta dónde llego:** el conector lo cubre todo (enviar, ver correos, dominios, llaves API…).
  **No creo ni borro llaves API sin permiso.**
- **Estado:** ✅ entrega verificada con un correo real.

### Gmail, Google Drive y Google Calendar
- Gmail está conectado a `giftiloveloyalty@gmail.com` (corregido; antes era inmobiliarianah).
- Hasta hoy sólo se usó para buscar rastro de GHL, y no hay ninguno.
- **Hasta dónde llego:** leer, buscar y hacer borradores. **Enviar sólo con permiso explícito.**
- **Estado:** ✅ conectados, poco uso.

### GoHighLevel (GHL) + WhatsApp
- **No se encontró ninguna cuenta** ni hay conector. Sospecha: se creó con `inmobiliarianah@gmail.com`.
- Existe `ghl-sync` v15:
  - Hace `ping`, `listFields`, `upsertContact`, `addTags` y `drain`.
  - Nunca sincronizó nada, y su cron se dio de baja.
  - **Es insegura** (sin JWT y con PIN de 4 dígitos): hay que arreglarla ANTES de poner el token.
- **Diseño decidido:** la etiqueta `giftilove-<kind>` dispara el workflow de GHL.
- **Estado:** ⏸️ fase final, a propósito.

## Dinero

### Stripe
- **Cuentas:** hay dos visibles y **ambas están en modo prueba** (`livemode false`): "Entorno de prueba de GiftiLoveLoyalty" y "GiftiLoveLoyalty" (la del conector).
- **Hecho:**
  - Una recarga de prueba cerró (2 filas `cs_test_…`).
  - Connect tiene 1 fila, sin terminar el onboarding.
- **Falta:**
  - La suscripción recurrente (punto 72).
  - Encender `IVA_AUTOMATICO` en `subscriptions` antes de cobrar (hoy está en `false`).
  - Cambiar el `success_url` por omisión, que apunta a `giftilove.lovable.app`.
- **Hasta dónde llego:** leer y escribir en la API con el conector. **Nunca muevo dinero.**
- **Estado:** ❌ no ha pasado ni un peso real.

## Herramientas de trabajo de Claude (máquina de Carlos)

| Herramienta | Versión | Qué se hizo / para qué | Límite o maña | Estado |
|---|---|---|---|---|
| **Obsidian MCP** | plugin Local REST API with MCP 5.1.0 | Este cerebro; el gancho SessionStart carga 00 + 02 | ~5,900 tok/sesión; Obsidian tiene que estar abierto | ✅ |
| **Plugin Codex (OpenAI)** | 1.0.6 + codex-cli 0.154.0 | Segunda opinión y rescate | **Falta `codex login`**; review gate apagado | ⚠️ inutilizable |
| **Graphify** | 0.9.61 | Mapa del código; ganchos de git post-checkout/post-commit y guardas en Claude | — | ✅ |
| **Firecrawl** | CLI 1.23.3, plugin 1.0.9 | Leer páginas como markdown | Plugin **apagado** (893 tok); el CLI funciona sin cuenta | ✅ CLI |
| **Agent Browser** | 0.37.1 | Interactuar con páginas | `open` se cuelga; no reemplaza a los navegadores integrados | ⚠️ |
| **Navegadores de Claude** | — | El Chrome real de Carlos (con su sesión) y el panel Browser | — | ✅ |
| **Subagentes de `arranque`** | 5 | arquitecto, constructor, revisor, investigador, seguridad | 0 tokens hasta usarse | ✅ |
| **Skills** | 19 | ui-ux-pro-max, 12 de Emil Kowalski, 5 auditores de Vercel | ui-ux-pro-max necesita `C:\Python314\python.exe` | ✅ |
| **Conector Supabase duplicado** (`.mcp.json`) | — | **Borrado** el 2026-09-14 (`a365748`) | Sólo queda el de claude.ai | ✅ |

**Entorno:**
- Node 24.21, npm 11.19 y Python 3.14.7 (el `python` del PATH está roto).
- El lockfile es `bun.lock`.

## Knowledge de Lovable (texto exacto, actualizado 2026-09-14 tarde)

Se lee y se escribe con `get_project_knowledge` / `set_project_knowledge`. El segundo **reemplaza todo el texto**, así que primero hay que leerlo.

```markdown
# Giftilove — cómo trabajar este proyecto

## Tu papel
Eres el equipo de **frontend visual**: diseño, pantallas, componentes, estilos y experiencia. En el mismo repositorio de GitHub trabaja otro agente (Claude Code) que mantiene la base de datos, la seguridad, la sesión y la lógica sensible. Trabajan en paralelo.

**Estas reglas nunca deben detener una tarea.** Si una parte de lo que te piden cae en una zona protegida, haz toda la parte visual y deja la otra anotada con un comentario `// TODO(backend): <qué falta y por qué>`. Claude Code la toma de ahí.

## Libre, sin preguntar
- Diseño, estilos y Tailwind, layout, animaciones, textos de interfaz, imágenes e iconos.
- Componentes y pantallas nuevas en `src/components` y `src/routes`.
- Mejoras visuales a pantallas existentes.
- Leer y mostrar datos usando las funciones que ya existen en `src/lib/`.

## Zonas protegidas — no las cambies
Aquí se arreglaron errores graves en producción. Cambiarlas vuelve a romper la app.

1. **Una sola conexión a Supabase.** Siempre `import { supabase } from "@/lib/supabase"`. Nunca importes nada de `@/integrations/supabase/*`, no actives "Lovable Cloud" ni "Connect Supabase", y no agregues variables `VITE_SUPABASE_*`. Motivo: el cliente generado de Lovable lanzaba una excepción por falta de variables y tumbaba la app al entrar a cualquier tienda. El lint lo marca como error.
2. **No edites estos archivos:** `src/lib/supabase.ts`, `src/lib/supabase-servidor.ts`, `src/lib/adjunta-sesion.ts`, `src/start.ts`, `src/server.ts`, `src/lib/edge-swipe.ts`, `vite.config.ts`, `eslint.config.js`. No agregues ni quites dependencias de `package.json` sin que te lo pidan.
3. **Base de datos y backend:** no crees tablas, migraciones, políticas RLS, funciones SQL ni edge functions, y no toques la carpeta `supabase/`. Si una pantalla necesita un dato que todavía no existe, constrúyela con su estado vacío y deja el `TODO(backend)`.
4. **Nada inventado:** sin datos de ejemplo presentados como reales, sin usuarios, contraseñas o códigos de muestra, y el navegador nunca genera códigos (los emite el servidor). Si no hay datos, se muestra un estado vacío honesto.
5. **Decisiones de negocio ya tomadas — no las "corrijas":**
   - Sin cuenta sólo se hojea el directorio de tiendas. Todo lo demás pasa por `conCuenta(...)` / `useRequireRegistration`. No quites esos candados.
   - Botón "atrás": nunca uses `history.back()` ni `history.pushState` directo; usa `useBackGuard` / `useEdgeSwipeBack` de `src/lib/edge-swipe.ts`.
   - Video de bienvenida: es uno solo de la plataforma y se ve una vez. El iframe de YouTube va sin controles y con `pointer-events-none` para que no saque a nadie de la app.
   - La pregunta "¿Tienes hijos en la universidad?" vive en el Perfil (menú "+"), debajo de Giftilunch, no en el registro.
   - El código Giftilove no se muestra al registrarse.
   - Un usuario sin foto muestra sus iniciales, nunca una foto de muestra.
   - Todo cupón lleva foto, título, descripción y vigencia.
6. **Seguridad:** nunca escribas llaves, tokens, PIN ni contraseñas en el código. Ya no existe ningún PIN en Giftilove: todo entra con correo y contraseña (sesión). No agregues campos de PIN ni mandes `pin` a ninguna función.
7. **Tablas que el navegador ya no escribe:** los referidos van por `referido_registrar` y `referido_cobrar` (en `src/lib/referrals.ts`); no leas ni escribas `referral_credits` ni `partner_stripe_accounts` directo.

## Sincronía (regla de todo el proyecto)
Nada vive en un solo lugar. Todo lo que hagas queda en GitHub (se publica solo en Vercel) y Claude Code revisa cada cambio tuyo. Tu parte:
- Trabaja siempre sobre lo último de `main`; no reviertas ni reescribas cambios recientes de otros sin que te lo pidan.
- Si cambias una pantalla que depende de un dato del servidor, no inventes el dato: deja `// TODO(backend)` y descríbelo en el chat.
- Si al trabajar notas que algo del frontend no coincide con lo que dice esta guía, avísalo en el chat en lugar de "arreglarlo" por tu cuenta.

## Al terminar cada cambio
- Toca sólo lo que se pidió.
- Si algo no compila, o una de estas reglas te impide terminar, repórtalo en el chat. No lo arregles metiéndote en una zona protegida.
```

- **Knowledge de Lovable (2026-09-14):** se agregaron las reglas 8 (todo va a la base de datos; logo = `partner.logo`) y 9 (F5 no saca: `useVistaRecordada`), y `src/lib/vista-recordada.ts` a los archivos protegidos.

## Knowledge de Lovable — texto exacto vigente
_Copia exacta de lo que tiene el proyecto de Lovable (actualizado 2026-09-14, reglas 8 y 9)._

````markdown
# Giftilove — cómo trabajar este proyecto

## Tu papel
Eres el equipo de **frontend visual**: diseño, pantallas, componentes, estilos y experiencia. En el mismo repositorio de GitHub trabaja otro agente (Claude Code) que mantiene la base de datos, la seguridad, la sesión y la lógica sensible. Trabajan en paralelo.

**Estas reglas nunca deben detener una tarea.** Si una parte de lo que te piden cae en una zona protegida, haz toda la parte visual y deja la otra anotada con un comentario `// TODO(backend): <qué falta y por qué>`. Claude Code la toma de ahí.

## Libre, sin preguntar
- Diseño, estilos y Tailwind, layout, animaciones, textos de interfaz, imágenes e iconos.
- Componentes y pantallas nuevas en `src/components` y `src/routes`.
- Mejoras visuales a pantallas existentes.
- Leer y mostrar datos usando las funciones que ya existen en `src/lib/`.

## Zonas protegidas — no las cambies
Aquí se arreglaron errores graves en producción. Cambiarlas vuelve a romper la app.

1. **Una sola conexión a Supabase.** Siempre `import { supabase } from "@/lib/supabase"`. Nunca importes nada de `@/integrations/supabase/*`, no actives "Lovable Cloud" ni "Connect Supabase", y no agregues variables `VITE_SUPABASE_*`. Motivo: el cliente generado de Lovable lanzaba una excepción por falta de variables y tumbaba la app al entrar a cualquier tienda. El lint lo marca como error.
2. **No edites estos archivos:** `src/lib/supabase.ts`, `src/lib/supabase-servidor.ts`, `src/lib/adjunta-sesion.ts`, `src/start.ts`, `src/server.ts`, `src/lib/edge-swipe.ts`, `src/lib/vista-recordada.ts`, `vite.config.ts`, `eslint.config.js`. No agregues ni quites dependencias de `package.json` sin que te lo pidan.
3. **Base de datos y backend:** no crees tablas, migraciones, políticas RLS, funciones SQL ni edge functions, y no toques la carpeta `supabase/`. Si una pantalla necesita un dato que todavía no existe, constrúyela con su estado vacío y deja el `TODO(backend)`.
4. **Nada inventado:** sin datos de ejemplo presentados como reales, sin usuarios, contraseñas o códigos de muestra, y el navegador nunca genera códigos (los emite el servidor). Si no hay datos, se muestra un estado vacío honesto.
5. **Decisiones de negocio ya tomadas — no las "corrijas":**
   - Sin cuenta sólo se hojea el directorio de tiendas. Todo lo demás pasa por `conCuenta(...)` / `useRequireRegistration`. No quites esos candados.
   - Botón "atrás": nunca uses `history.back()` ni `history.pushState` directo; usa `useBackGuard` / `useEdgeSwipeBack` de `src/lib/edge-swipe.ts`.
   - Video de bienvenida: es uno solo de la plataforma y se ve una vez. El iframe de YouTube va sin controles y con `pointer-events-none` para que no saque a nadie de la app.
   - La pregunta "¿Tienes hijos en la universidad?" vive en el Perfil (menú "+"), debajo de Giftilunch, no en el registro.
   - El código Giftilove no se muestra al registrarse.
   - Un usuario sin foto muestra sus iniciales, nunca una foto de muestra.
   - Todo cupón lleva foto, título, descripción y vigencia.
6. **Seguridad:** nunca escribas llaves, tokens, PIN ni contraseñas en el código. Ya no existe ningún PIN en Giftilove: todo entra con correo y contraseña (sesión). No agregues campos de PIN ni mandes `pin` a ninguna función.
7. **Tablas que el navegador ya no escribe:** los referidos van por `referido_registrar` y `referido_cobrar` (en `src/lib/referrals.ts`); no leas ni escribas `referral_credits` ni `partner_stripe_accounts` directo.
8. **TODO VA A LA BASE DE DATOS (regla de Carlos).** Nada de lo que la persona captura se muestra sólo desde el estado de React, `localStorage` o una caché: se guarda en Supabase y se vuelve a leer de ahí, para que se vea igual en cualquier computadora. El logo de la tienda es `partner.logo` (columna `partners.logo_url`), no un campo local. Si una pantalla necesita guardar algo que todavía no tiene tabla o función, haz la parte visual y deja `// TODO(backend)`; no lo guardes en el navegador.
9. **F5 NO SACA DE DONDE ESTÁS (regla de Carlos).** Toda pestaña o pantalla nueva usa `useVistaRecordada` (o `leerVista`/`guardarVista`) de `src/lib/vista-recordada.ts` en lugar de `useState`, para que al refrescar la persona siga en la misma pestaña, pantalla y tienda. Sólo se recuerda la vista, nunca datos.

## Sincronía (regla de todo el proyecto)
Nada vive en un solo lugar. Todo lo que hagas queda en GitHub (se publica solo en Vercel) y Claude Code revisa cada cambio tuyo. Tu parte:
- Trabaja siempre sobre lo último de `main`; no reviertas ni reescribas cambios recientes de otros sin que te lo pidan.
- Si cambias una pantalla que depende de un dato del servidor, no inventes el dato: deja `// TODO(backend)` y descríbelo en el chat.
- Si al trabajar notas que algo del frontend no coincide con lo que dice esta guía, avísalo en el chat en lugar de "arreglarlo" por tu cuenta.

## Al terminar cada cambio
- Toca sólo lo que se pidió.
- Si algo no compila, o una de estas reglas te impide terminar, repórtalo en el chat. No lo arregles metiéndote en una zona protegida.
````

## install-anti-slop (2026-09-19)
- Skill de dmmulroy/anti-slop, instalada a pedido de Carlos en `.agents/skills/install-anti-slop`. Es un instalador de un plugin de Oxlint con ~25 reglas de estilo estricto. Auditada: sólo copia archivos, sin red.
- **No aplicada al repo todavía**: agregaría `oxlint` y reglas en "error" que marcarían cientos de líneas actuales. Decisión de Carlos.

## Revisiones "thermo" (cursor/plugins, 2026-09-19)
- `thermo-nuclear-review` (bugs/seguridad del diff), `thermo-nuclear-code-quality-review` (mantenibilidad) y `thermos` (las dos en paralelo). Sólo markdown, auditadas. Sólo corren si Carlos las pide (`/thermos`); no cuestan tokens por sesión.
