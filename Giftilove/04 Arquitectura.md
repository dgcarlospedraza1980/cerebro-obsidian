---
tags: [giftilove, arquitectura]
actualizado: 2026-09-14
---
# Arquitectura — cómo está construido

Detalle y reglas de código: `CLAUDE.md` del repo. Aquí el mapa.

## Stack
- React 19 + TypeScript + Tailwind v4 + **TanStack Start (SSR)**. No hay
  `index.html`: el armazón está en `src/routes/__root.tsx`.
- Build: `npm run build` (Vite, config `@lovable.dev/vite-tanstack-config`; no
  tocar `vite.config.ts`). Lockfile: `bun.lock`.
- **No hay tests.** Verificar = `tsc --noEmit` + `npm run build` + probarlo en el
  navegador con la secuencia exacta.
- Repo local: `C:\Users\Admin\Documents\happygift` · GitHub `giftilove/happygift`.

## Conexión a Supabase (una sola)
- `src/lib/supabase.ts` — cliente del navegador; URL y llave anon escritas (la
  seguridad la da RLS).
- `src/lib/supabase-servidor.ts` — llave de servicio desde variable de entorno
  `SUPABASE_SERVICE_ROLE_KEY` (sólo servidor). Usado por pausar/eliminar socio.
- `src/lib/adjunta-sesion.ts` — middleware global que pega el token de sesión a
  las funciones de servidor (registrado en `src/start.ts`).
- `src/integrations/supabase/` — de Lovable. **No se importa**; sólo queda
  `types.ts` sin uso.

## Base de datos (Supabase `tnkpxnpqicsmlsggisyz`, "giftiloveloyalty01")
- Postgres + RLS + Auth + Storage. Migraciones versionadas en
  `supabase/migrations/` (crear siempre con `apply_migration`).
- Registro: trigger `handle_new_user` en `auth.users` crea `profiles` (con
  `public_code`); trigger `encolar_bienvenida_del_cliente` en `profiles` encola
  el correo de bienvenida.
- Acceso de equipo: fila en `staff_members` (roles socio / gerente / staff /
  corporativo). `mi_rol_de_staff()`, `soy_corporativo()`.
- Planes en `membership_plans`; ajustes globales en `platform_settings`.
- Cuotas: `tool_usage`, `cuota_estado()`, `cuota_consumir()` (sólo servidor).
- Cola de avisos: `notification_queue` (candado: una por usuario/tipo/día).
- Plantillas editables: `email_templates`. Historial: `correos_enviados()` (sin
  payload).

## Edge functions (viven en Supabase, NO en el repo)
Editar con `get_edge_function` / `deploy_edge_function`. Conocidas:
`email-drain` (v14), `upload-video`, `upload-image`, `giftcard-stripe`,
`ghl-sync`, `password-reset`, `socio-alta`, `sellers`, `sponsorships`,
`support-console`, `ads-admin`, `partner-config`, `games-admin`, `raffle-draw`,
`coupon-redeem`, `offers-admin`, `gift-video`, `gift-payment`, `stripe-checkout`,
`stripe-connect-onboarding`, `stripe-connect-status`. Total: 28.

## Crons (pg_cron)
- `giftilove_email_drain` cada 5 min → `email-drain`, con timeout 90 s.
- `giftilove_liberar_apartados`, `giftilove_purge_gift_videos` → **sin timeout**.
- Diarios: expiraciones (cupones, tarjetas, ofertas, juegos, pases),
  `run_billing_cycle`, `backfill_commissions`, avisos de vencimiento.
- `giftilove_ghl_drain` ya **no existe** (se dio de baja).

## Correos
Resend, dominio `giftilove.com`, remitente `avisos@giftilove.com`. Flujo:
algo encola en `notification_queue` → cron → `email-drain` pinta la plantilla
(de `email_templates` si existe, si no la del código), sustituye fichas
(`{{tienda}}`, `{{nombre}}`, `{{correo}}`, `{{dias}}`…) → Resend. Tinta por
luminancia. Cada correo lleva liga de baja real.

## Frontend — piezas con trampa
- **Dos menús de abajo**: nivel 0 / directorio (`src/routes/index.tsx`) y
  dentro de la tienda (`MobileHome.tsx`). Los dos usan `conCuenta(...)`.
- **Dos `useBackGuard`** (`src/lib/edge-swipe.ts`): cada uno devuelve sólo su
  propio centinela.
- Video de bienvenida: resolución única en `src/lib/branding.tsx`; se marca
  visto por URL; iframe sin controles.
- Marca de la plataforma vs la del socio: `src/lib/marca.ts`.

## Despliegue
- **Vercel**: equipo `giftilove-vercel` (`team_B1BTjQY2CJAkPa9jdI1kb3YC`),
  proyecto `giftilove` (`prj_zpD8e0mGKdVrg5HMN1razE9uKfVq`). Dominios:
  `giftilove.com`, `www.giftilove.com` y `.vercel.app`.
- **Lovable** se sincroniza solo desde GitHub. Proyecto
  `409f4680-3414-4da2-8996-6e3806e177eb` (`giftilove.lovable.app`).
- Un `git push` a `main` publica en GitHub → Vercel → Lovable.
