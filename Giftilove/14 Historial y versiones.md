---
tags: [giftilove, historial, versiones]
actualizado: 2026-09-14
---
# Historial y versiones

**En números:**
- **Repo:** `giftilove/happygift`. **Total:** 1,720 commits (al 2026-09-14).
- **De Lovable:** 1,255 son suyos con el mensaje "Changes" o "Work in progress".
- **Con mensaje propio:** el resto, casi todos de Claude.

**Cómo ver el detalle de cualquier cambio:**
- `git show <hash>`.
- `git log --date=short --pretty='%ad %h %s' | grep -vi changes`.

## Etiquetas
| Etiqueta | Commit | Qué marca |
|---|---|---|
| `v2026.09.13` | `fd84178` | **Versión estable**: la tanda del 13 de septiembre funcionando (Carlos lo pidió) |

## Línea del tiempo por fases
- **2025-01-01 / 2026-06 a 07:** plantilla inicial de Lovable y prototipo visual (home móvil, carruseles, tarjetas).
- **2026-08-02:** 100 commits de Lovable (rediseño).
- **2026-08-16/18:**
  - Logo y corazón en base64.
  - Carrusel de sellos.
  - Botón flotante de regalos.
  - `fb8fc1f` cliente Supabase de la base real `giftiloveloyalty01`.
  - `f15c322` helpers de Stripe Connect.
- **2026-08-19:**
  - Arreglos de flexbox que escondían secciones.
  - Grabar video real.
  - Giftilonche/Giftilunch (saldo, recarga Stripe, hijos, menú).
  - Comisión corregida a 5%.
  - Súper-Usuario con regalos patrocinados.
- **2026-08-20/21:**
  - Registro de 3 pasos con Supabase Auth.
  - Panel de usuarios registrados.
  - Perfil con foto.
  - Giftilunch real contra Supabase (`73fde15`), con enlace papá → hijo y un menú por hijo.
  - Razón social con "Systems" (`da331de`).
- **2026-08-22:**
  - Ruleta y rasca-y-gana (datos, componentes, hub "Activaciones").
  - Panel de Corporativo y Súper Usuario con campañas.
- **2026-08-24:** `a925a49` revertido a `075ed8d`.
- **2026-08-27/28:**
  - Rifas GiftiPlayWin (`2a0ff20`).
  - **Modelo de negocio nuevo**: 4 planes, clientes ilimitados, herramientas contadas (`7c79743`).
  - GiftiCard y GiftiMASTERLOVE.
- **2026-08-29/31:** logo de correos; (base) publicidad, cobranza, vendedores, tienda demo.
- **2026-09-02:**
  - Gesto de "atrás" que ya no saca (`6fe2554`).
  - Botón para instalar la PWA.
  - Video de felicitación con el regalo, servicio por mandar, canje del regalo pagado contra el servidor.
- **2026-09-03:** puntos y sellos del navegador al servidor (`56bc5e1`, `8269c4a`); equipo en la base; cupones reales de piezas publicadas.
- **2026-09-04:**
  - **Entrada con correo y contraseña** (`c940534`, `abd79ff`).
  - Destello en botones.
  - Colores exactos de Giftilove `#bbef1f`/`#2b2b2b` (`6ebd223`).
  - Nombres "Socio" y "Corporativo" (`bc6009c`).
  - Icono maskable.
- **2026-09-05:** puntos 74, 75 y 76: la entrada es de Giftilove, el verde de la plataforma vuelve, el buscador no nombra tiendas borradas.
- **2026-09-06:** (en la base y en funciones) punto 71: sellers, sponsorships, support-console y ads-admin pasan a sesión; recuperación de contraseña.
- **2026-09-12 — Claude Code toma el repo:**
  - `85d997b`…`e3cc2bd`: CLAUDE.md con la verdad del proyecto.
  - `5caabbd`: migraciones reconstruidas desde el historial real.
  - `6518c61`: partner_save y ajustes con sesión.
  - `e78e83b`: se borra staff_rotate_pin.
  - `bcf1e5f`, `0da55fe`: giftcard-stripe y upload-video con sesión.
  - `4a16171`: correos maestros (backend).
  - Herramientas: `6cbd92c` Graphify, `2aee266` ui-ux-pro-max, `c421c48` skills de Emil, `7766b7f` auditores de Vercel, `4bf658b` agent-browser, `8a98f96`/`036679b` Firecrawl (apagado), `175df83` MCP de Supabase por `.mcp.json`.
- **2026-09-13:**
  - `1cee19a`: la puerta dice "Corporativo".
  - `fda8446`: (26, 27) correos maestros editables y el video vuelve a ser de la red.
  - `e3e7457`: el video se ve; sin cuenta ya no se pasa del directorio.
  - `25de499`: la pregunta de hijos al perfil; icono = corazón.
  - `f7eb6c0`: bienvenida sale y se lee; el alta deja de mostrar el código.
  - `a55cee0`: docs del timeout de pg_net.
  - `9b45e38`: el video ya no expulsa.
  - `09d7054`: **(grave)** había DOS clientes de Supabase.
  - `fd84178`: **(grave)** el guardián de "atrás" sacaba del sitio → **etiqueta `v2026.09.13`**.
- **2026-09-14:**
  - `aec5ebb`: prácticas "primero el repositorio" + Codex.
  - `a365748`: una sola conexión a Supabase (se quita `.mcp.json`).
  - `d229c1f`: reglas para Lovable + lint.
  - `914cf2f`: cerebro en Obsidian + gancho SessionStart.
  - `f7316b0`: inventario de herramientas y llaves.
  - (este commit): copia de las 28 edge functions, las 5 migraciones que faltaban, foto de configuración y manual de réplica.

## Versiones desplegadas de las edge functions (2026-09-14)
stripe-checkout 25 · stripe-webhook 26 · stripe-connect-onboarding 21 · stripe-connect-status 21 · admin-users 18 · games-admin 21 · raffle-draw 18 · ghl-sync 15 · giftcard-stripe 14 · sponsorships 13 · support-console 14 · partner-config 14 · ads-admin 12 · coupon-redeem 12 · offers-admin 12 · upload-image 11 · stripe-setup 10 · subscriptions 11 · email-drain 14 · stripe-webhook-fix 4 · sellers 3 · gift-video 3 · gift-payment 3 · upload-video 3 · staff-invitar 3 · socio-alta 4 · socio-relevar 1 · password-reset 4.
