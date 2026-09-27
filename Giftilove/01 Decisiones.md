---
tags: [giftilove, decisiones]
actualizado: 2026-09-14
---
# Decisiones — lo que SÍ y lo que NO

Formato: **SÍ/NO** · la decisión · *fecha* · por qué · fuente.
Si una decisión cambia, no se borra: se tacha y se escribe la nueva debajo.

## Equipo, plataforma y forma de trabajar
- **SÍ** — Lovable hace sólo el **frontend visual**; Claude Code hace backend,
  base de datos, seguridad y lógica sensible. *2026-09-12.* Lovable diseña
  bien; los arreglos de fondo cuestan menos directo en el repo.
- **SÍ** — **Nada de la app depende de la nube de Lovable**; debe poder
  cambiarse a Bolt u otra herramienta. *2026-09-13.* Fuente: commit `09d7054`.
- **NO** — Usar el cliente de Supabase generado por Lovable
  (`@/integrations/supabase/*`) ni variables `VITE_SUPABASE_*`. Una sola
  conexión: `@/lib/supabase`. *2026-09-13.* Tumbó producción. → [[03 Incidentes]]
- **SÍ** — Claude entra a Supabase **sólo por el conector de claude.ai**; se
  borró el `.mcp.json` del repo. *2026-09-14.* Commit `a365748`.
- **SÍ** — Reglas escritas para Lovable (Knowledge del proyecto) + lint que
  marca error si importa el Supabase de Lovable. *2026-09-14.* Commit `d229c1f`.
- **NO** — Candado en el build contra Lovable. *2026-09-14.* Un build roto
  detiene todo el avance, también lo bueno.
- **SÍ** — Ahorrar créditos de Lovable: lo no visual se hace directo; a
  Lovable sólo diseño o imágenes. *2026-09-12.* Un reporte de estado costó 4.8
  créditos.
- **SÍ** — **Alcance acotado**: arreglar sólo lo que Carlos pide; lo colateral
  se anota y se le dice. *2026-09-12.* Cada arreglo destapaba más y costaba.
- **SÍ** — "Mapear antes de tocar" (graphify) y prácticas "primero el
  repositorio". *2026-09-12 y 2026-09-14.*
- **SÍ** — Herramientas con costo fijo de tokens por sesión se quedan
  **apagadas** salvo que se necesiten (Firecrawl apagado). *2026-09-12.*
- **SÍ** — Obsidian es el **cerebro del proyecto** (esta carpeta). *2026-09-14.*
- **SÍ** — Plugin de Codex (OpenAI) como segunda opinión; su *review gate*
  apagada. *2026-09-14.*
- **SÍ** — Versión estable etiquetada `v2026.09.13`. *2026-09-13.*
- **SÍ** — **Todo lo necesario para replicar Giftilove queda guardado**: las 28 edge functions copiadas en `supabase/functions/` (con los PIN tapados), las 137 migraciones completas, la foto de configuración en `supabase/estado/` y [[10 Manual de réplica]]. *2026-09-14.* Carlos: que otra instancia, de Claude o de otra IA, esté al día en el mismo segundo y pueda replicar sin que falte una coma.
- **NO** — Copiar el código fuente dentro de la bóveda. *2026-09-14.* Serían dos verdades: Lovable cambia el repo varias veces al día y la copia quedaría vieja en horas. La bóveda apunta al repo por commit y etiqueta, y explica todo lo que el código no dice.
- **SÍ** — Cada vez que se despliegue una edge function, su archivo en el repo se actualiza en el mismo commit. *2026-09-14.*
- **SÍ** — **Regla de sincronía total** con candados automáticos (ganchos PostToolUse y Stop): nada vive en un solo lugar del ecosistema. *2026-09-14.* Carlos: "no quiero que me digas que algo falló porque no estaba actualizado porque sólo lo tiene otra parte del ecosistema".
- **NO** — PIN en cualquier parte del sistema. *2026-09-14.* Se eliminaron todos: las personas entran con sesión y los crons/triggers con una llave que genera la base y guarda cifrada en su bóveda (`giftilove_llave_cron`). Commit `685e59c`.
- **SÍ** — Los referidos se registran y se cobran por el servidor (`referido_registrar`, `referido_cobrar`); el navegador ya no toca `referral_credits`. *2026-09-14.*

## Correos y comunicación
- **SÍ** — Todo el correo por **Resend**; GHL sólo para WhatsApp. Punto 15.
- **NO** — GHL antes de la fase final (GHL + API de WhatsApp +
  automatizaciones). *2026-09-12.*
- **SÍ** — Correos maestros editables desde Corporativo (8 tipos). Fuera a
  propósito: `socio_aviso`, `socio_cambio_de_plan`, `staff_alta`. *2026-09-12.*
- **SÍ** — La pantalla de Correos maestros la hizo Claude Code, no Lovable.
  *2026-09-13.* Formulario sin diseño nuevo; ahorra créditos.
- **SÍ** — "Bienvenidas" pasó a **Correos enviados**: historial real, nunca
  muestra el contenido (ahí viaja la contraseña temporal). *2026-09-13.*
- **SÍ** — Correo de bienvenida al **cliente final**, a nombre de Giftilove.
  Texto: obsequios, qué hay en cada tienda, "¡Tiene todo que ver!", "Aprende más
  de tu app en el símbolo (+)", dos recomendaciones, legales y baja.
  *2026-09-13.* Ningún cliente lo había recibido nunca. → [[03 Incidentes]]
- **NO** — Poner la contraseña en un correo. *2026-09-13.* No se puede: se
  guarda cifrada, nadie puede leerla.
- **NO** — Bloque "Tu cuenta" (nombre/correo/contraseña) en la bienvenida.
  *2026-09-13.* Carlos: "no sirve de nada si no puedes mostrar nada".
- **SÍ** — Legales del correo: Aviso de privacidad, Contrato de uso de datos
  (usuario), Política de cancelación. **NO** el contrato corporativo (es de
  socios). *2026-09-13.* Pendiente que Carlos lo confirme.
- **SÍ** — Texto de los correos en negro sobre lima (tinta por luminancia).
  *2026-09-13.*

## Producto — app del cliente
- **SÍ** — Sin cuenta **sólo se hojea el directorio**; todo lo demás pide
  registro. Excepción: modo demo del vendedor (`?tienda=demo`). *2026-09-13.*
- **SÍ** — Video de bienvenida **de la plataforma**; la tienda sólo lo deja o
  lo quita; **Gold** puede subir el suyo. *2026-09-13.*
- **SÍ** — El video se ve **una vez** (por video, no por tienda) y sin controles
  de YouTube. *2026-09-13.*
- **SÍ** — "¿Tienes hijos en la universidad?" vive en el **Perfil (+), debajo de
  Giftilunch**, no en el registro. *2026-09-13.* Alimenta el banner de Giftilunch.
- **NO** — Mostrar el código Giftilove al registrarse. *2026-09-13.* Sirve para
  recuperar la cuenta, no para estrenarla.
- **NO** — Foto de muestra para un usuario nuevo; van sus iniciales. *2026-09-13.*
- **SÍ** — Icono de la app instalada = corazón del favicon sobre guinda
  `#510021`. *2026-09-13.*
- **SÍ** — La puerta dice **"Corporativo"**, no "Giftilove". *2026-09-13.*

## Producto — negocio y operación
- **SÍ** — **American Burguers es la tienda ficticia de ventas** (la usan todos los vendedores); las 4 reales son Dunia, Mc Dowell's, Nikkey y Silver Fish. *2026-09-14, Carlos.*
- **SÍ** — **La tarjeta del inicio muestra la zona ("Ciudad / sucursal", ej. Puebla Centro) y la dirección completa en tooltip**, y las estrellas; todo de lo capturado. *2026-09-14.*
- **SÍ** — **Cada tienda tiene sus propias categorías** (las crea y quita el socio); ya no hay lista fija. *2026-09-14.*
- **SÍ** — **Antes de elegir tienda, la app lleva la marca Giftilove**, no la de una tienda cualquiera. *2026-09-14.*
- **SÍ** — **Las solicitudes de demo de /socios quedan en la base** y se atienden en Corporativo → Solicitudes de demo; los horarios son los que el negocio propone y Giftilove confirma. *2026-09-14.*
- **SÍ** — **Marcar favoritos pide cuenta**: se guardan en la cuenta (`user_favorites`) para verse en cualquier teléfono. *2026-09-14.* Consecuencia de "todo va a la base"; sin cuenta el corazón invita a registrarse.
- **SÍ** — **Los referidos son por cuenta** (código Giftilove) y el cobro suma puntos de verdad en el servidor. *2026-09-14.*
- **NO** — **Mudar saldos de puntos desde el navegador** (`seed_points`). *2026-09-14.* Permitía inventarse hasta 5,000 puntos por tienda.
- **SÍ** — **REGLA GLOBAL: todo va a la base de datos**, nada sólo en la computadora o en caché; si cambias de computadora, todo está ahí. *2026-09-14.*
- **SÍ** — **REGLA GLOBAL: F5 refresca y te deja donde estabas** (rol, tienda, pestaña, pantalla). *2026-09-14.*
- **SÍ** — **El logo es columna de la tienda (`partners.logo_url`)**, porque se usará en más lados. *2026-09-14.*
- **SÍ** — **Las estrellas de la tienda se escriben a mano (0 a 5, un decimal) y salen en la tarjeta de la portada.** *2026-09-14.* Una sola columna: `partners.rating`.
- **NO (por ahora)** — **Estrellas automáticas desde la liga de Google Maps.** *2026-09-14.* La liga sola no trae la calificación; necesita llave de Google Places con cobro. Decisión de Carlos.
- **SÍ** — **Marca y video guarda con UN botón "Guardar" hasta abajo**, después de los colores; ya no se autoguarda en silencio. *2026-09-14.* Carlos subió el logo, no vio botón y creyó que no se guardó.
- **SÍ** — **Teléfonos de contacto de la tienda en formato WhatsApp internacional (+52…)**, validado en el servidor. *2026-09-14.* GHL manda los avisos por WhatsApp.
- **SÍ** — **Dos personas distintas (o la misma): quien registró la tienda (cuenta socio) y la persona de contacto.** *2026-09-14.* Los avisos van a la de contacto.
- **SÍ** — **Recargar (F5) no saca del panel de Socio/Corporativo**: se recuerda la vista y se reconoce la sesión. *2026-09-14.*
- **SÍ** — **Alta de socio con selector de color de espectro completo**, además de los atajos. *2026-09-14.*
- **SÍ** — **El correo de contacto de la tienda lo escribe el socio** (Marca y video) **o Corporativo** (ficha de la tienda). *2026-09-14.* Carlos: "tiene que poder escribirlo". Reemplaza la decisión vieja del punto 40 ("el correo no lo cambia el socio"). No es el correo con el que entra el socio: ese se cambia con "Cambiar responsable" (socio-relevar).
- **NO** — **Pomelo / GIFTI LOVE MASTER con Pomelo**: descartado. *2026-09-13.*
  No es viable. GiftiCard (tarjeta interna) **sí** sigue. Punto 12.
- **SÍ** — Precios ~~0 / 1200 / 1800 / 2400~~ → **0 / 1000 / 1600 / 1800** MXN (Carlos, 2026-09-17, desde Membresías). **NO** existe Platino.
- **SÍ** — Cuotas de megáfono **1 / 2 / 4 / 6** al mes.
- **SÍ** — La cuota se gasta cuando la herramienta **alcanza al cliente** y **no
  se devuelve** al borrar. Menú de Giftilunch y regalo de bienvenida no gastan.
- **SÍ** — Cambio de plan: se aplica y se avisa (ya se confirmó por teléfono).
  Punto 9.
- **SÍ** — Campaña a miles: **NO se regala nada**; se manda cupón condicionado
  (tipo 3x2) o el banner, autorizado por el cliente. Punto 32.
- **SÍ** — Franquicia = tienda completa; **máximo 2 niveles**.
- **SÍ** — Hay **dos corporativos**: `giftiloveloyalty@gmail.com` y
  `dgcarlospedraza@gmail.com`. *2026-09-13.*
- **SÍ** — "Cerrado" significa que **Carlos lo puede ver**, no que el servidor
  esté listo.
- **SÍ** — Razón social con **"Systems"**, no "Sistems".

- **SÍ** — **Un solo teléfono de contacto por tienda (el de WhatsApp); fuera el teléfono 2 de Marca y video.** *2026-09-14.*

## 2026-09-15 / 16 — Megáfono, atribución de clientes y cobro del plan

### SÍ
- **30 minutos** para corregir o borrar una pieza sin gastar cupo (antes 2 h).
  Quitarla después **sí gasta**: se cobra el alcance, no el objeto.
- **Todo cruza de mes** mientras la membresía esté al corriente.
- **Megáfono**: WhatsApp primero y el correo al día siguiente, escalonado, en
  ventanas internas que el socio no controla (L-V 14–16 y 17–20; S-D 8–12).
- **Precios**: megáfono $200, **Megáfono Pro $250**; descuentos por paquete
  10% desde 5 y 15% desde 10, editables. El tope por persona se queda corto a
  propósito: *"para que aprendan a usarlos cuando de verdad los necesitan"*.
- **Premium y Gold traen 1 Megáfono Pro al mes**; nadie más lo trae en plan.
- **Atribución del cliente**: QR de tienda (con etiqueta del lugar), compartir,
  el QR del mostrador y la ubicación. La ubicación **sugiere, nunca asigna**:
  el GPS falla ±50 m y en una plaza hay varios restaurantes pegados.
- **Radio de 200 m** (editable 50–1000) que se afloja hasta la precisión del
  teléfono, con tope duro de 1 km.
- **Quitar un código QR no lo borra**: se apaga, para no perder de dónde
  vinieron los que ya se registraron con él.
- **La tienda demo no se hereda** como tienda de casa.
- **Formas de pagar el plan**: mes con mes, **6 meses con 10% de descuento** y
  **el año pagando 11 meses**. Los dos números son editables.
- **El semáforo de la cuenta es de tres colores**, no de dos: verde al
  corriente, **ámbar** cuando todavía puede salvarla (por vencer / en rescate)
  y rojo cuando ya se apagó.
- **Las fotos se encuadran antes de subirse**, con el marco real de cada
  pantalla y tiradores que no deforman.
- **Los avisos grandes pasan a una línea desplegable**: siguen saliendo igual
  de seguido, pero dejan de tapar la pantalla.

### NO
- **No** asignar tienda automáticamente por cercanía, aunque sea más rápido.
- **No** calcular precios ni descuentos en el navegador: los manda el servidor
  para que no existan dos verdades.
- **No** dejar `createCheckout` / `status` de `subscriptions` abiertos: ahora
  exigen sesión y ser dueño de la tienda.

## 2026-09-16 — Dónde vive cada canje (CAMBIO de una regla vieja)

### SÍ
- **Mis cupones** guarda todo lo que la persona **canjea** en la caja: el cupón
  de la tienda, la **tarjeta de sellos completada**, lo cambiado por puntos y
  **lo ganado jugando (ruleta, Slot Play) y en las rifas**.
- **La caja de regalos** guarda lo que le **regalan**: del restaurante, del
  corporativo (fuente `gift`) y lo que le manda una persona (tabla `gifts`).
- **El reparto lo decide la base**, no el navegador: `user_coupons.destino`, lo
  pone el trigger `set_cupon_destino` con la función `cupon_destino(source)`.

### NO (cambio respecto al 27 de agosto)
- Ya **no** es cierto que *"los premios viven sólo en la caja"*. Lo ganado
  jugando es un cupón canjeable y vive en Mis cupones. Si aparece la nota
  vieja, manda ésta.
- Se borró `FUENTES_DE_CAJA` de `src/lib/mis-cupones.ts`: era la misma decisión
  escrita en el navegador, o sea una segunda verdad.

## 2026-09-16 — Tipos de cupón por ocasión, reloj y entorno lleno

### SÍ
- **El tipo de cupón es la OCASIÓN**: 8 de fábrica (estándar, cumpleaños,
  aniversario, bienvenida, te extrañamos, cliente frecuente, temporada, aviso o
  invitación) **más los que invente cada tienda** ("Martes de tacos"). El
  servidor valida que el tipo exista.
- **Todo lleva reloj menos lo de puntos**: cupones, sellos, cortesía y también
  los regalos gratis y de paga. Lo comprado o con depósito se rige por el
  apartado de Stripe (reembolso menos comisión).
- **Entorno lleno**: el socio ve "Tienes X% lleno, te falta:" con botón a cada
  pestaña. El 100% es **según su plan**: una pieza viva de cada herramienta que
  el plan le da, más logo, portada, contacto, dirección y pin del mapa.

### NO
- **El megáfono NO es un tipo de cupón** (Carlos: "nada de megáfono como
  invitación, es sólo un ejemplo"). Cualquier cupón puede llevar megáfono.
- El megáfono **no cuenta** para el % del entorno: amplifica, no es contenido.

### Pendiente de decidir (Carlos)
- Que el % ordene el directorio del cliente (el "rankeo").
- Aviso a Giftilove de tiendas vacías: desde qué porcentaje.

## 2026-09-17 — Pantalla

### SÍ
- **Una foto subida se guarda en el mismo paso** (el logo se borraba al subir
  la portada porque esperaba el botón Guardar).
- **Favicon por área**: Staff = regalito rojo; Socio = corazón completo con
  lápiz abajo a la derecha; Corporativo = corazón completo con engrane abajo a
  la derecha; con un canal vacío que los separa. Cliente = corazón de siempre.
- **La vista previa al recomendar enseña la portada (o logo) de la tienda**, y
  la liga siempre lleva la tienda.
- **Letra más grande para gente mayor** en tarjetas del directorio, tarjetas de
  sellos y el detalle del cupón con su QR.

## 2026-09-17 — Ranking del directorio y formas de pago dinámicas

### SÍ
- **El directorio se ordena por puntaje del servidor**: entorno lleno (60%) +
  planeación = todo su cupo publicado (25%) + bono por pagar por adelantado
  (hasta 15 puntos a 12 meses; 1 mes = 0; en medio, en proporción).
- **Una mensual bien terminada le gana a una anual a medias** (Carlos: "si están
  bien terminadas pues sí van primero").
- Los pesos se editan en Corporativo → Red de socios → "Cuánto pesa cada cosa".
- **El buscador encuentra tiendas por nombre en el servidor**, sin acentos y con
  faltas de ortografía.
- **Las formas de pago son dinámicas** (Carlos: "tengo un creador de paquetes"):
  se crean en Corporativo → Membresías → Formas de pago (1 a 12 meses, descuento
  y meses de regalo) y las leen Mi cuenta, el cobro, Stripe y el ranking.

### NO
- El cliente **no ve** el puntaje ni cómo paga cada tienda: sólo el orden.
- Los meses de una forma de pago **no se cambian** (arman la llave de Stripe):
  se crea otra y se apaga la vieja. "Mes con mes" no se apaga.
- Más de 12 meses no: Stripe no cobra periodos de más de un año.

### Interpretación
- Carlos escribió "coloca en mejor posición a las que están **incompletas**"; se
  tomó como "completas" porque el 16-09 dijo "el que no pone nada no rankea bien".

## 2026-09-17 — Corazones y aviso de tiendas descuidadas

### SÍ
- **El corazón de arriba del directorio sube las favoritas** de la cuenta y debajo
  sigue el ranking. La preferencia se guarda **en la cuenta** (no en el navegador):
  sobrevive a F5 y a otro teléfono. Sin cuenta, invita a registrarse.
- **Aviso a Giftilove** cuando una tienda lleva **7 días sin publicar nada** (se
  repite cada semana mientras siga así) o **su % de entorno baja** contra la
  medición anterior. Llega **un correo al día** a cada cuenta corporativa y se ve
  en Corporativo → Red de socios con botón "Atendido". Revisión diaria 7:10.
- El paginado de la lista del directorio se deja **para cuando existan muchas
  tiendas** (Carlos).

### NO
- El aviso de tienda descuidada **no le llega al socio**: es para Giftilove.

## 2026-09-17 — Qué cuenta en el entorno: línea, opcional y muy recomendable

### SÍ
- **De línea** (cuentan para el %): sellos, cupón, premio por puntos, regalo de
  paga, ruleta, tragamonedas, rifa, más lo básico de la tienda.
- **Opcional** (no cuenta): regalo de cortesía y regalo gratis. Carlos: "no
  queremos que crea que queremos que regale nada, porque es lo que menos queremos".
- **Muy recomendable** (no cuenta, se sugiere): megáfono en los últimos 30 días.
- En el Dashboard del socio el indicador va **plegado**; afuera, **una dona debajo
  del menú** con el % y "revisa si las herramientas que faltan te sirven".

### NO
- Lo opcional no baja ni el % ni el ranking.
- El regalo de paga NO es opcional: se vende, no se regala.

### Pendiente de confirmar con Carlos
- Si el megáfono debería contar para el % (hoy no cuenta).

## 2026-09-17 — Favicons, versión final
- Staff = regalito rojo. **Socio = manita roja roquera.** **Corporativo = sólo el engrane rojo, grande.** Cliente = corazón de siempre.
- Reemplaza la del mismo día "corazón con lápiz/engrane abajo a la derecha".

## 2026-09-17 — Ranking en su pestaña y favicon de pulgar
- El **Ranking del directorio** (con avisos y pesos) tiene su propia pestaña en Corporativo, entre Red de socios y Usuarios; ya no está dentro de Red de socios.
- **Favicon de Socio = mano roja con el pulgar arriba** (dibujo propio). Reemplaza la manita roquera.

## 2026-09-17 — Regalo o cupón, la regla

### SÍ
- **Regalo** (cortesía de la tienda o algo que alguien ya pagó) → **caja de regalos**.
- **Con compra / consumo** (cupón, sellos, puntos, juegos, rifas) → **Mis cupones**.
- **Un regalo gratis que la persona se queda deja de ser regalo**: se vuelve cupón
  en Mis cupones, con el mismo cronómetro que habría tenido de haberlo mandado.
- En el home de la tienda: el botón de arriba lleva a "Regala gratis" si hay; si
  no, a "Regala a quien amas"; si no hay nada, no aparece.
- Dentro de la tienda **no hay buscador**; el botón del video queda pegado al canto.

### Banner principal (construido el 2026-09-17)
- **Herramienta nueva**, pieza de `partner_offers` con `kind = 'banner'`: carrusel
  con flechas en el home de la tienda, administrador propio, megáfono, recortador
  **apaisado 16:9** y cupo igual al de tarjetas de sellos (4 | 8 | 12 | 18).
- **Lo decide el socio en `details.entrega`**: `cupon` → dice "Canjear", enseña su
  cronómetro y el canje cae en **Mis cupones**; `regalo` → "Abrir el regalo" y cae
  en la **caja de regalos**. Es la misma regla de regalo/cupón de este día.
- El **regalo de bienvenida** es sólo la primera tarjeta del carrusel.
- El ejercicio que puso Carlos y que tiene que aguantar: "el de cortesía y luego 3
  de promociones varias", moviéndose "rápida y elegante a los lados" — usa la misma
  animación que el carrusel de tarjetas de sellos (slide-in + fade, 300 ms).

### La cortesía se mudó al banner principal (2026-09-17)
- **El regalo de cortesía deja de ser una herramienta.** Era la misma idea que el
  banner principal con dos fábricas distintas, y Carlos pidió una sola: *"regalo
  de cortesía, quítalo, ya está en banner principal, y lo diferenciaste bien: uno
  es cupón y el otro es regalo"*.
- Las cortesías que existían se volvieron **banners de regalo**, intactas. Ya no
  se pueden crear nuevas y su cupo (`courtesyGifts`) desapareció de los planes.
- **El interruptor del regalo de bienvenida salió de "Marca y video"**: esa
  tarjeta se prende publicándola y se apaga quitándola desde Banner principal.

### La demo es sólo para vendedores aprobados (2026-09-17) — CAMBIO DE REGLA
- Antes: la demo (American Burguers) se abría sin cuenta con `/demo` o
  `?tienda=demo`, para que el prospecto la jugara en su celular.
- **Ahora**: la abre un vendedor aprobado con correo, contraseña y su número de
  vendedor, o Giftilove. Lo valida la base (`demo_accesos`). El prospecto ya no
  la recibe por QR ni por liga; si quiere conocer Giftilove, agenda cita.
- `giftilove.com` siempre entra a la app de clientes: la demo no se reabre sola.

## 2026-09-18 — Método de venta con demo
- SÍ: el vendedor aprobado es socio de la demo y de nada más; maneja la demo en la laptop (Socio) y canjea en Staff.
- SÍ: el cliente entra a /demo con nombre, correo, teléfono y una CLAVE TEMPORAL de 24 h (no el número fijo del vendedor: con el fijo, cualquiera que lo oyera entraría siempre).
- SÍ: cuentas de demostración aisladas (role 'prospecto', correo inventado, candado en la base). Su correo real sólo en `prospectos` → GHL.
- NO: el vendedor ya no cambia el plan de la demo (Gold para todos, lo mueve Giftilove).
- SÍ: reiniciar regresa al punto de partida fijado por Giftilove, nunca a cero.

## 2026-09-18 — Tienda demo por vendedor
- NO: el vendedor no ve Corporativo. SÍ: usuario, staff y socio de SU tienda demo.
- SÍ: cada vendedor su copia "American Burguers · GL-00X"; número de tienda = número de vendedor.
- SÍ: tras una semana sin vender, Giftilove (a mano) libera la tienda y la asigna a otro.
- SÍ: los correos de la demo le llegan al cliente a su correo real; WhatsApp cuando entre GHL.
- SÍ: correos de Giftilove con letra sin patines.
- NO: la pregunta "¿tienes hijos en la universidad?" en el perfil; Giftilunch → "Agregar hijos o beneficiario".

## 2026-09-18 — Tarjetas
- SÍ: el producto se llama **GiftiCARD**; sólo se compra y se cobra en la tienda que la emite.
- NO (por ahora): GiftiMASTERLOVE. Fuera de todas las pantallas; datos en la base sin borrar.
- SÍ: banner principal con la foto limpia; sólo el cronómetro arriba a la izquierda.

## 2026-09-18 — GiftiGames
- SÍ: los juegos son el negocio secundario → sub-panel propio "GiftiGames (activaciones)" en Corporativo y Socio.
- SÍ: "Llenar un restaurante" (mandar un juego a toda la red) es SÓLO de Corporativo y es GLOBAL (un lugar, se elige tienda y juego).
- NO: Slot Play (no quedó bien; se rehará).
- SÍ: ruleta de 10 gajos: 1..9 puntos y un solo cupón ganador; todos los juegos con megáfono.

- **2026-09-18 — SÍ: cada cuenta acomoda el menú de su panel** jalando los botones de la orilla, y hay "Orden de origen" para regresarlo. El orden vive en la base por cuenta (`orden_de_menu`), no en el navegador. El botón de juegos se llama sólo **"GiftiGames"**, sin "(activaciones)".

- **2026-09-18 — Ruleta**:
  - **NO** existen las "horas para usar el giro": un giro se juega al momento y vive lo que viva la ruleta.
  - **SÍ**: cada ruleta dice cuántos giros puede repartir el staff en las mesas.
  - **SÍ**: el cupón ganado dura **3 días** por omisión (editable), igual si el giro vino del staff o de puntos.
  - **SÍ**: el cupo del socio y la tabla de "Quiero más de esto" salen de `membership_plans`, nunca de la tabla del código.
  - **NO**: `partners.quotas` no guarda copias del paquete, sólo los ajustes a mano de Giftilove.

- **2026-09-18 — SÍ: cada giro de ruleta se descuenta del paquete de la tienda.**
  - Los que reparte el staff gastan de `wheelGamesStaff`.
  - Los que el cliente canjea con puntos gastan de `wheelGamesApp`.
  - ~~Crear ruletas no gasta.~~ **Cambió el 2026-09-18**: cada ruleta creada gasta 1 de `ruletas` (ver abajo).
  - Hay tres tipos de ruleta (de mesa / de puntos / las dos) y el restaurante usa los que le convienen.
  - Las cantidades se ajustan en Membresías; hoy son pocas a propósito y Carlos las va a ampliar.

- **2026-09-18 — Poker 5**:
  - Lo decide todo el servidor, nunca el teléfono.
  - Hay tres tipos de mesa separados: ganar cupón, pagar cupón y por puntos.
  - ~~Cada partida gasta del paquete (`poker5Mesas`).~~ **Cambió el 2026-09-18**: gasta cada MESA publicada, jugar no gasta (ver abajo).
  - Empate exacto: al azar en el servidor.
  - Sólo quien abrió la mesa reparte.
  - Tope de 5 partidas por persona al día.
  - Si alguien no cambia cartas en 3 minutos, la mesa se resuelve sola.
  - ~~El cobro en caja lo marca el staff con el código de la mesa.~~ **Cambió**: la mesa "para pagar" se cobra como cualquier cupón con precio (escaneo en caja o tarjeta).
  - El diseño visual lo hace Lovable sin tocar la lógica.

- **2026-09-18 — Todo cupón puede tener precio** (lo que paga el cliente al canjearlo) y precio normal tachado.
  - Se paga en caja al escanearlo, o con tarjeta desde la app.
  - Pagado con tarjeta NO desaparece: dice PAGADO, la tienda recibe aviso y al escanearlo se comprueba el pago.
  - Sellos, regalos, regalo del banner y catálogo por puntos no llevan precio.

- **2026-09-18 — Poker 5: el paquete cuenta mesas creadas.** "Son 18 según el plan Gold, y cada mesa que se crea se resta de las 18." Se cobra al publicar; jugar partidas no gasta; eliminar no devuelve.

- **2026-09-18 — Ruletas: el paquete cuenta ruletas creadas Y giros.** Tres contadores: `ruletas` (creadas), `wheelGamesStaff` y `wheelGamesApp` (giros). Una ruleta "de mesa y de puntos" gasta giros del contador de donde sale cada giro. Eliminar ruletas y mesas se puede siempre; lo gastado no regresa.

- **2026-09-19 — Dados**: juego nuevo de GiftiGames que funciona igual que la ruleta. Tres contadores 4/8/10/14 (dados creados, tiros del staff, tiros con puntos), megáfono normal y Pro, premios por combinación (par exacto, cualquier par, suma, cualquiera) cada uno con su cupón y precio opcional. **Tira el servidor**, nunca el teléfono (el prototipo decidía en el teléfono; se cambió). La física y el look del prototipo no se tocan.

## 2026-09-20

**SÍ — Tercera regla global de pantalla: lo que se acumula se colapsa.**
Carlos: *"pones todo desplegado y no piensas en que todo se acumulará; todo lo que
se acumula se tiene que colapsar, usar muy poco espacio y archivar."* Se diseña
para el día 300, no para el día 1. Pieza única: `SeccionPlegable.tsx`, cerrada por
omisión, con contador y recordando su estado. Reemplazó los tres desplegables
sueltos que había. Está en `CLAUDE.md` junto a las otras dos reglas de pantalla.

**SÍ — En la caja, el staff ENSEÑA el código y el cliente lo ESCANEA.**
No al revés. Motivo de seguridad, no de comodidad: los puntos se mueven con la
sesión del cliente, así que hace falta que esté presente con su teléfono, y el
servidor rechaza que quien emitió el código se lo cobre a sí mismo.

**SÍ — La bitácora de caja no la puede borrar nadie, ni el dueño.**
Una bitácora corregible no sirve para aclarar un faltante.

**SÍ — Los permisos los decide el servidor, nunca la pantalla.**
`mis_permisos_en_tienda`: staff opera; gerente, socio y corporativo además ven la
bitácora y autorizan. Sin sesión, todo en false.

**SÍ — El código único del cliente se usará para varias cosas.**
Por eso se conserva "Ver código" aunque hoy sólo sirva para puntos y Giftilunch.

## 2026-09-24 — El alta de Stripe la hace Giftilove, no el socio

**SÍ:** conectar la cuenta donde el socio recibe su dinero es trabajo de
**Corporativo**, desde Red de socios. Carlos: *"que sólo corporativo; le
decimos al cliente 'pásame esto cuando tengas tu cuenta', o 'saca tu cuenta y
hacemos una conferencia para conectar'."*

**NO:** que el dueño del restaurante lo resuelva solo desde su panel. Se
intentó desde agosto y **en seis meses no lo terminó nadie** — una sola cuenta
creada, a medias. El panel del socio ahora sólo informa en qué va.

**SÍ: la prueba del peso.** Carlos: *"crea una acción de prueba, así en pantalla
con el cliente: mire, ya está, aquí está la prueba… que en ese momento vea que
le cae el peso."* Es un transfer real de $1.00 MXN de Giftilove a su cuenta,
que el dueño ve entrar en su panel de Stripe durante la llamada. Dinero
moviéndose, no un letrero de "quedó listo".

**Matiz técnico que no se puede saltar:** Stripe no deja tocar una cuenta que
no autorizó a la plataforma. Por eso "pásame tu dato" funciona sólo si el dueño
ya autorizó a Giftilove; cuando no, la misma pantalla le abre la cuenta y
genera el link. Los dos caminos conviven a propósito.

**Regla que se desprende:** un trámite de terceros (Stripe, Meta, un banco) que
le toque al cliente hacer solo, no se termina. Si el negocio depende de él, lo
acompaña Giftilove. Ver [[Bitácora/2026-09-24]].
