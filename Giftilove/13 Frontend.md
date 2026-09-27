---
tags: [giftilove, frontend, catalogo]
actualizado: 2026-09-14
---
# Frontend — mapa completo

- **Stack:**
  - React 19 + TypeScript + Tailwind v4 + **TanStack Start (SSR)**, con Vite y la configuración `@lovable.dev/vite-tanstack-config` 2.13.1.
  - Servidor con `nitro`. Supabase JS 2.106.
  - UI: radix + shadcn (46 componentes en `src/components/ui`), sonner, vaul, recharts, embla, lucide.
- **No hay `index.html`:** el armazón es `src/routes/__root.tsx`.
- **Archivos:** 436 versionados; 265 en `src/`.
- **Comandos:** `npm run dev` · `npm run build` · `npm run lint` · `./node_modules/.bin/tsc --noEmit -p tsconfig.json` (hay 1 error conocido en `__root.tsx:146`).

## Rutas (`src/routes/`)
| Archivo | URL | Qué es |
|---|---|---|
| `__root.tsx` | — | Armazón HTML, metas PWA, errorComponent |
| `index.tsx` | `/` | **Nivel 0**: directorio de tiendas + entrada a la tienda. Tiene su propio menú de abajo, `conCuenta` y `useBackGuard` |
| `app.tsx` | `/app` | Entrada a la app |
| `socios.tsx` | `/socios` | Panel del socio |
| `staff.tsx` | `/staff` | Área del staff (caja/escáner) |
| `demo.tsx` | `/demo` | Demostración para el vendedor |
| `regalo.$id.tsx` | `/regalo/:id` | Ver un regalo recibido |
| `nueva-contrasena.tsx` | `/nueva-contrasena` | Pantalla del enlace de recuperación |
| `baja.tsx` | `/baja` | Baja de correos |
| `legal.privacidad.tsx`, `legal.datos-usuario.tsx`, `legal.datos-corporativo.tsx`, `legal.cancelacion.tsx` | `/legal/...` | Legales |
| `README.md` | — | Nota de rutas |

`routeTree.gen.ts` se genera solo. `router.tsx`, `server.ts` y `start.ts` (este último registra el middleware `adjuntaSesion`).

## Librería (`src/lib/`, 77 archivos) — lo que cada una resuelve
- **Conexión (PROTEGIDOS):** `supabase.ts` (el único cliente), `supabase-servidor.ts` (service role desde env), `adjunta-sesion.ts` (middleware global), `config.server.ts`.
- **Navegación (PROTEGIDO):** `edge-swipe.ts`: `useBackGuard` y `useEdgeSwipeBack`. Cada guardián devuelve sólo su propio centinela (`esMio`).
- **Marca:** `branding.tsx` (resuelve el video de bienvenida en un solo lugar y aplica `--lime`), `marca.ts` (MARCA_GIFTILOVE, para que la tienda no repinte la plataforma), `destello.ts`, `celebrate.ts`.
- **Sesión y registro:** `client-auth.ts` (incluye `guardarHijosEnUniversidad`), `client-registration.ts`, `staff-session.ts`, `password-reset.ts`, `profile.ts`, `demo-mode.ts`, `demo-store.ts`, `demo-data.ts`.
- **Segunda vuelta (2026-09-14):** fuera `DEFAULT_PARTNER_ID`, `DEMO_CATEGORIES` (`DemoCategory` = string), `DEMO_FRANCHISES`, `INITIAL_FRANCHISES`, `DEMO_NOW`. Nuevos: `lib/categorias.ts`, `CampoCategoria.tsx`, `CorporativoDemos.tsx` (pestaña "demos"). `routes/socios.tsx` → `demo_solicitar`. Ojo: `listPublishedOffers` sin tienda trae toda la red.
- **Todo a la base (2026-09-14):** `favorites.ts` (user_favorites; el corazón pide cuenta), `profile.ts` → `useMiPerfil` (profiles), `referrals.ts` (código = public_code; la invitación espera en la URL hasta que haya sesión), `gifty-lunch-identity.ts` (sólo correo de la cuenta), `gift-cards.ts` (`volvimosDePagarTarjeta`, `tarjetaPorConfirmar`, avisos en legal_consents), `demo-store.ts` (plan de `partners`). Borrados: megafono.ts, claimed-free-gifts.ts, plan-change-notices.ts, points-code.ts.
- **Vista recordada (2026-09-14):** `src/lib/vista-recordada.ts` → `useVistaRecordada(llave, inicial, validos)`, `leerVista`, `guardarVista` (localStorage `giftilove:vista:*`). Usado en AdminDashboard, CorporativoPanel, CorporativoSupport, CorporativoSellers, ClientApp (tienda y pantalla) y MobileHome.
- **Logo (2026-09-14):** `Partner.logo` y `Partner.welcomeGiftEnabled` salen de `partners`; `branding.tsx` los aplica en `applyPartner` y se pone al día al releer la lista.
- **Estrellas (2026-09-14):** `estrellasDeTexto()` en `mi-tienda.ts` (texto → número 0–5 con un decimal); `ContactoDeTienda.estrellas` es texto. La tarjeta de `PartnerDirectory` pinta `rating.toFixed(1)`.
- **Marca y video (2026-09-14):** `AdminBranding` sin autoguardado (`usePartnerSectionState(..., {autoguardar:false})`), botón Guardar abajo; `savePartnerSection` devuelve boolean. `TelefonoWhatsApp.tsx` + `lib/telefono.ts`. F5: `routes/index.tsx` recuerda la vista (`giftilove:vista-rol`) y la tienda elegida por Giftilove (`giftilove:socio-tienda`); los Gates reconocen la sesión con `useMiRol()`.
- **Contacto de la tienda (2026-09-14):** `mi-tienda.ts` → `useContactoDeTienda(partnerId)` lee de `partners` y se refresca en todas las pantallas al guardar; `guardarContactoDeTienda` (socio) y `guardarCorreoDeTienda` (sólo el correo, Corporativo). Nunca leer el correo de `branding.contactEmail` para decidir si falta.
- **Tiendas y planes:** `partners.ts` (PartnerFeatures, videoPropio, mapeo), `planes.ts`, `platform-settings.ts`, `partner-config.ts`, `partner-status.ts` + `partner-status.functions.ts` (server functions), `red-socios.ts`, `socios.ts`, `mi-tienda.ts`, `plan-change-notices.ts`, `cuotas.ts` (el contador real).
  - Viejos, por confirmar si se borran: `megafono.ts` y `quota-usage.ts`.
- **Cupones, ofertas, puntos:** `offers.ts`, `claim-offer.ts`, `mis-cupones.ts`, `won-coupons.ts`, `courtesy-gifts.ts`, `claimed-free-gifts.ts`, `welcome-gift.ts`, `points.ts`, `points-code.ts`, `punch-cards.ts`, `referrals.ts`, `favorites.ts`.
- **Juegos y rifas:** `games.ts`, `raffles.ts`.
- **Regalos y tarjetas:** `gift-redeem.ts`, `gift-video.ts`, `gift-fee.ts`, `gift-cards.ts`, `received-gifts.ts`, `brand-gifts.ts`, `stripe-payouts.ts`.
- **Giftilunch:** `giftilunch.ts`, `giftilunch-hijos.ts`, `giftilunch-listings.ts`, `giftilunch-cupones.ts`, `gifty-lunch-catalog.ts`, `gifty-lunch-children.ts`, `gifty-lunch-identity.ts`, `gifty-lunch-notifications.ts`.
- **Corporativo:** `admin-users.ts`, `correos-maestros.ts` (CATALOGO de fichas por tipo de correo), `support-console.ts`, `sellers.ts`, `staff.ts`, `ads.ts`.
- **Utilidades:** `video.ts` (YouTube: watch/youtu.be/embed/shorts/live, `esVideoReproducible`), `image-upload.ts`, `image-downscale.ts`, `legal-content.ts`, `country-codes.ts`, `site-url.ts`, `register-sw.ts`, `edge-errors.ts`, `error-capture.ts`, `error-page.ts`, `lovable-error-reporting.ts`, `utils.ts`, `api/example.functions.ts`.

## Componentes (`src/components/happy/`, 82)
- **App del cliente:**
  - Pantalla principal y navegación: MobileHome (**menú de abajo dentro de la tienda**, `conCuenta`, segundo `useBackGuard`), MobileHomeNav, PartnerDirectory.
  - Perfil y registro: ProfileScreen (iniciales, PreguntaHijos debajo de Giftilunch), ClientRegistrationGate (sin la pregunta de hijos ni el código a la vista), InstallApp, WelcomeVideo (iframe sin controles, `pointer-events-none`).
  - Cupones y sellos: CouponCard, CouponDetail, MyCoupons, PunchCardWide, PunchCatalog, LockedCatalog, PointsCode, Countdown, FakeQR.
  - Regalos y tarjetas: GiftBoxScreen, GiftBubbleFloating, GiftCatalog, GiftView, SendGiftFlow, GiftCardsScreen, GiftCardIcon.
  - Juegos: GamesScreen, GameWheel, SlotMachine, ScratchCard.
  - Otros: OtherStores, RecommendButton, FavoriteHeart, InviteScreen, SellerScreen, StripeGate, PasswordInput, LegalDocScreen, LegalLayout.
- **Giftilunch:** GiftyLunchScreen, GiftilunchInviteCard, GiftilunchParentInvite, GiftilunchCuponesCliente, GiftilunchCuponCaja, GiftilunchCuponesSocio, GiftilunchCuponesCorporativo, PapaCatalogBuilder.
- **Panel del socio (Admin\*):** AdminDashboard, AdminBranding (el video no viaja en partner_config; video propio sólo Gold), AdminCoupons, CouponBuilder, OfferAdmin, AdminPunchCards, AdminPointsCatalog, AdminCourtesyGift, AdminWelcomeGift, AdminFreeGifts, AdminGiftSending, AdminGiftiCards, AdminGiftyLunch, AdminRaffles, AdminActivations, AdminAds, AdminStaff (jerarquía de 3 niveles), AdminStripeConnect, AdminTicketPoints, AdminQuotaHeader, AdminConfigStatus, AdminSidebarNav, AdminPartners, AdminUsersPanel.
- **Corporativo:** CorreoDeTiendaEditable (editar el correo de contacto desde la ficha de la tienda), CorporativoPanel (pestañas, incluidas "Correos maestros" y "Correos enviados"), CorporativoCorreos, CorporativoActivations, CorporativoAds, CorporativoRaffles, CorporativoSellers, CorporativoSupport, AccionesDeContrasena, WelcomeVideoUploader.
- **Equipo:** StaffArea, StaffActivations, StaffGiftCards, StaffScanner, RoleLogin (la puerta dice "Corporativo").

## Reglas de frontend que ya costaron caro (no romper)
1. **Dos menús de abajo:** `routes/index.tsx` (nivel 0) y `MobileHome.tsx` (dentro de la tienda). Los dos pasan por `conCuenta`. El modo demo (`?tienda=demo`) queda exento.
2. **Dos `useBackGuard`:** nunca `history.back()` "por si acaso". El de nivel 0 sólo se activa fuera del home del directorio y sin tienda.
3. **Una sola conexión a Supabase:** `@/lib/supabase`. El lint bloquea `@/integrations/supabase/*`.
4. **Video de bienvenida:**
   - Se marca como visto por URL (`hg_welcome_seen_<url>`).
   - YouTube va con `controls=0&modestbranding=1&fs=0&disablekb=1&iv_load_policy=3&playsinline=1&rel=0` y `pointer-events-none`.
   - Si la liga no se puede reproducir, no se pinta nada.
5. **Nada que sea DATO vive en el navegador.** `localStorage` sólo para preferencias de vista y borradores. Los códigos los emite el servidor.
6. **Icono PWA:**
   - Fuente: `public/icons/icono-fuente.svg` (corazón `#ff073b` sobre `#510021`).
   - PNG 32/180/192/512 + maskable-512, generados con `sharp-cli` en una carpeta de ruta corta, nunca dentro de `public/icons`.
   - `manifest.webmanifest` apunta a `icon-maskable-512.png`.
7. **Pendiente conocido:** `?tienda=<id>` no entra a la tienda (el efecto corre antes de que carguen las tiendas). Rompe el QR del vendedor.

## 2026-09-15 / 16 — Pantallas y piezas nuevas

| Archivo | Qué es |
|---|---|
| `Megafono.tsx` + `lib/megafono.ts` | toda la UI del megáfono: plecas, elegir nivel, comprar paquetes, regreso de compra |
| `CampoVigencia.tsx` | el reloj en días : horas : minutos |
| `AdminQrTienda.tsx` | Socio → **Mi código QR**: crear códigos con etiqueta, descargar SVG, ver escaneos y registros, y poner el pin del local |
| `QrDeMostrador.tsx` | el staff enseña el QR de la tienda desde su teléfono |
| `ConfirmarTuTienda.tsx` | "¿en qué restaurante estás?" — sugiere las 3 cercanas y la persona confirma |
| `lib/atribucion.ts`, `lib/qr.ts`, `lib/mi-qr.ts` | leer el origen de la liga, dibujar el QR y hablar con los RPC |
| `AdminMiCuenta.tsx` + `lib/mi-cuenta.ts` | Socio → **Mi cuenta**: semáforo, plan, vencimiento, tres formas de pagar y el alta de Stripe Connect adentro |
| `CorporativoPreciosStripe.tsx` | botón de Giftilove para crear/actualizar los precios en Stripe |
| `RecortadorDeFoto.tsx` | recortador único con tiradores que no deforman; lo usan las 4 pantallas que suben fotos |

**Reglas nuevas que conviene no romper:**
- Toda pantalla que suba imágenes pasa por `useRecorteDeFoto()`, no manda el
  `File` directo.
- El **color** del semáforo de la cuenta lo decide el servidor, no la pantalla.
- Los precios y descuentos que se pintan vienen del servidor; no se calculan
  en el navegador.
- El único dato que vive en el navegador de todo esto es el **recado** del
  origen (sessionStorage) entre que alguien escanea un QR y termina su alta.

## 2026-09-17 — Piezas nuevas
| Pieza | Qué es |
|---|---|
| `CampoTipoCupon.tsx` + `lib/tipos-cupon.ts` | elegir y crear tipos de cupón en el mismo campo |
| `IndicadorDeEntorno.tsx` + `lib/entorno.ts` | "Tienes X% lleno, te falta:" en el Dashboard del socio; `EntornosDeLaRed` en Corporativo → Red de socios |
| `useFaviconDeArea` (`routes/index.tsx`) | cambia el favicon según el área: `favicon-staff.svg`, `favicon-socio.svg`, `favicon-corporativo.svg` |
| `loader` + `head` de `/` | con `?tienda=` pone `og:title`/`og:image` de esa tienda (lo que ve WhatsApp) |

**Reglas nuevas:**
- El logo se guarda al subirse (`guardarLogo` en `AdminBranding`), igual que
  la portada.
- `buildReferralUrl` siempre lleva `?tienda=`.
- Con letra grande, revisar que nada se trunque: el nombre de la tienda usa dos
  renglones y la ubicación va en su propio renglón; `PunchCardWide` crece a lo
  alto (`min-h-40`).
- Las vistas previas `og:` sólo sirven si se ponen en el servidor
  (`loader`/`head`), nunca en un efecto.

## 2026-09-17 — Ranking y formas de pago
| Pieza | Qué es |
|---|---|
| `lib/directorio.ts` (`useOrdenDelDirectorio`) | orden y búsqueda del directorio desde el servidor, espera 250 ms al escribir |
| `PartnerDirectory.tsx` | ya no inventa el orden (fuera estrellas/distancia/jitter) |
| `CorporativoFormasDePago.tsx` + `lib/formas-de-pago.ts` | creador de formas de pago en Corporativo → Membresías |
| `EntornosDeLaRed` | ahora es "Ranking del directorio" con posición, planeación, forma de pago, puntos y editor de pesos |
| `AdminMiCuenta` | tantas tarjetas de pago como formas activas |

**Regla:** la pantalla no calcula precios de formas de pago; los manda el servidor.

## 2026-09-17 — Corazones y avisos
- `useFavoritosPrimero` (`lib/directorio.ts`): el corazón de arriba del directorio,
  guardado en la cuenta. `useOrdenDelDirectorio(texto, { apagado, favoritosPrimero,
  llaveFavoritos })` vuelve a pedir el orden al marcar/quitar un corazón.
- `AvisosDeTiendas` dentro de `EntornosDeLaRed` (Corporativo → Red de socios).

## 2026-09-17 — Dona e indicador plegable
- `AdminDashboard` lee `useEntornoDeTienda` una vez (y al cambiar de pestaña) y lo
  pasa a `IndicadorDeEntorno` (plegable, `useVistaRecordada("socio-entorno")`) y a
  `DonaDeEntorno` (debajo de `AdminSidebarNav`, `hidden md:block`).
- Insignias: "Tu tienda", "De línea", "Muy recomendable" (rosa `--seller`),
  "Opcional" (contorno).

## 2026-09-17 — Pestaña `ranking` en Corporativo
- `CorporativoPanel`: tab `ranking` (Trophy) entre `network` y `users`; renderiza `EntornosDeLaRed`.
- `favicon-socio.svg`: pulgar arriba rojo.

## 2026-09-17 — Home de la tienda y regalo abierto
- `MobileHome`: fuera el buscador dentro de la tienda; botón del video pegado al
  canto; un solo "Ver todas" en sellos; el botón de regalos decide destino y
  aparece sólo si hay algo.
- `GiftCatalog`: prop `inicial` ("free" | "paid") y la pestaña se llama
  "Regala a quien amas".
- `GiftView`: enseña la foto del regalo (`RegaloReal.imagen`) y el mapa se arma
  con la dirección real de la tienda.

- `VideoBubble`: burbuja flotante del video (sólo play y tache), en la esquina arriba del menú. Se cierra y se recuerda por video.

## El banner principal (2026-09-17)
- `BannerPrincipal.tsx` — lo que ve el cliente: carrusel con flechas y puntos,
  insignia de tipo, `Countdown` cuando es cupón y el botón que dice **Canjear** o
  **Abrir el regalo** según `details.entrega`. La animación es la misma de
  `PunchCardPager`: `key` que remonta la tarjeta + `animate-in fade-in` con
  `slide-in-from-right-8` / `left-8`, 300 ms, y `motion-reduce` la apaga.
- `AdminBanners.tsx` — el administrador del socio, calcado de `AdminPunchCards`:
  cupo, Vigentes/Historial, ventana de 30 minutos, megáfono, tipo de cupón y vista
  previa con el banner de verdad. Pestaña **Banner principal** en `AdminDashboard`.
- `CouponBuilder` acepta `aspecto` y `tituloRecorte`: el banner recorta en **16:9**.
- `MobileHome` ya no pinta el bloque del regalo de bienvenida: se lo pasa al banner
  como primera tarjeta (`bienvenida` con su `regaloListo`).

## Fuera la cortesía y el interruptor de bienvenida (2026-09-17)
- Borrados: `AdminCourtesyGift.tsx`, `AdminWelcomeGift.tsx` y `src/lib/welcome-gift.ts`.
- `AdminBranding` ya no tiene el bloque "Regalo de bienvenida"; `AdminDashboard`
  perdió la pestaña "Regalo cortesía" y el banner sólo sale si su plan lo trae.
- `MobileHome`: el banner se basta solo (ya no recibe `bienvenida`), y
  "Regala gratis" mira los `free_gift` publicados, no las cortesías.
- `LockedCatalog`: "Regalos patrocinados" ya sólo lista los de marca.
- `courtesy-gifts.ts` se queda **sólo** para "para_regalar" (kind `free_gift`).

## La puerta de la demo (2026-09-17)
- `PuertaDeLaDemo.tsx` + `src/lib/demo-acceso.ts`. La montan los caminos de
  entrada de `ClientApp` (liga, tienda recordada, directorio y `goStoreById`).
- La tienda recordada ya no reabre la demo: por eso giftilove.com llevaba a
  American Burguers.
- `SellerScreen` → Mi demo: fuera el QR y la liga para el prospecto.

## 2026-09-18
- **Panel de ventas** (`SellerScreen`): número, clave para el cliente (WhatsApp/Copiar), su tienda demo, Staff, carrera, cuánto puedo ganar (precios reales), clientes potenciales. Sin cambio de plan. Perfil dice "Panel de ventas" al aprobado.
- **`PuertaDeLaDemo`**: dos modos, "cliente" (nombre, correo, teléfono, clave) y "vendedor". `demo-acceso.ts`: `entrarComoClienteDelVendedor`, `esCuentaDeDemo`, `miTiendaDemo`.
- `routes/index.tsx`: `abrirMiDemo` abre la tienda demo de la cuenta; `PartnerDirectory demoId` enseña sólo ésa.
- **Canje real**: `QrDeCanje` (QR de verdad; fuera `FakeQR`), `EscanerQr` (cámara con jsqr), `StaffArea` canjea cupones GC-/PC-/GP-.
- Corporativo → Vendedores: punto de partida de la demo y "Tienda de cada vendedor" (días sin vender, Liberar/Asignar).
- **GiftiCARD** (nombre nuevo en todo); fuera GiftiMASTERLOVE de perfil, icono de tarjeta, /socios, Corporativo, Staff. El icono de tarjeta abre "Mis GiftiCARDs".
- **Banner principal**: formato de siempre (16:9, degradado negro, título y botón sobre la foto), cronómetro arriba a la DERECHA, sin puntitos.
- **Video de bienvenida**: se recorta la franja del título/logo de YouTube (`WelcomeVideo`).
- Fuera "¿Tienes hijos en la universidad?" del perfil; Giftilunch: "Agregar hijos o beneficiario".
- `public/email/logo-giftilove.png` era texto base64 → ya es PNG real.

- **2026-09-18 · `AdminSidebarNav` con `menu`**: los botones se reacomodan jalando la agarradera de la orilla izquierda (o con Alt+flecha). El orden viene de `src/lib/orden-de-menu.ts` (`useOrdenDeMenu`, `aplicarOrden`); una pestaña nueva entra sola en su lugar de origen. Lo usan Socio (`menu="socio"`) y Corporativo (`menu="corporativo"`).


## Cambios 2026-09-18 / 19 — GiftiGames y precio de cupones
- **Poker 5**: `Poker5.tsx` (cliente: mesas publicadas, lobby con código+QR, cartas grandes; la carta es sólo número + un palo grande en SVG, `SimboloDePalo`), `AdminPoker5.tsx` (socio/corporativo: **3 pestañas** Ganar/Pagar/Por puntos con `useVistaRecordada("poker5-tipo-de-mesa")`; cada mesa es un renglón que se abre; Eliminar con confirmación; megáfono en cada renglón; al crear Megáfono y Megáfono Pro; partidas recientes plegadas dentro de su pestaña), `src/lib/poker5.ts`.
- **Ruleta** (`AdminActivations.tsx`): tres círculos de cupo (Ruletas creadas, giros del staff, giros con puntos); Eliminar siempre con confirmación; el borrador dice "Nueva · sin guardar"; los botones "Nueva ruleta" se quedan siempre abajo; megáfono visible aunque la ruleta esté apagada ("enciéndela para usarlos").
- **Cupo del paquete**: la verdad son las `quotas` de `membership_plans` (`usePlanesVivos`, `registrarCuotasDePlanes`); etiquetas en `PARTNER_QUOTA_LABEL` ("Mesas de Poker 5", "Ruletas creadas"). Si agregas un contador nuevo: llave en `PartnerQuotas`, en los 4 `MEMBERSHIP_TIERS`, etiqueta y orden.
- **Precio de cupones**: `CampoPrecioCupon` (CouponBuilder `conPrecio`), `PagoDelCupon.tsx`, `CobroDelCupon.tsx` (Staff: "Ya cobré $X · canjear"), `src/lib/cupon-pago.ts` (regreso de Stripe por recado de pestaña, lo confirma `MobileHome`).
- **Menús** reordenables (`AdminSidebarNav` prop `menu`, `src/lib/orden-de-menu.ts`, orden en la base).

- **Dados (2026-09-19)**: `DadosMesa.tsx` (three + cannon-es cargados sólo en el navegador; simula todo el tiro sin pintar, elige una de las 24 rotaciones del cubo para que caiga el número del servidor y reproduce en cámara lenta 0.75), `DadosJuego.tsx` (cliente: tiros, canje con puntos, código del staff, qué se gana con su probabilidad de 36), `AdminDados.tsx` (3 círculos de cupo, renglones, reglas con cupón y precio, megáfono, eliminar), sección "Dados" en `PanelGiftiGames`. Cupos nuevos en `partners.ts`: `dados`, `diceGamesStaff`, `diceGamesApp`.

## Metaetiqueta de verificación de dominio de Meta (2026-09-19)

Vive en el `head` de `src/routes/__root.tsx`, junto a las demás `meta`. **No hay
`index.html` en este proyecto** (TanStack Start), así que cualquier instrucción de
"pégala en el HTML de tu página de inicio" se cumple aquí. Va en el armazón, no en
`index.tsx`, para que salga en el HTML que manda el servidor en **todas** las rutas:
Meta no la lee si la pinta JavaScript después.

**No la quites** aunque la verificación ya haya pasado — Meta la vuelve a leer cada
tanto y el dominio se desverifica si desaparece. Si algún día hay que verificar otro
dominio o subdominio, se agrega otra `meta` igual con su propio valor; conviven.

Comprobado contra el sitio publicado: `https://www.giftilove.com/` la trae en la
fuente; el dominio sin `www` responde 308 hacia `www`.
