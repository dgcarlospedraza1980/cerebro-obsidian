---
tags: [giftilove, base-de-datos, catalogo]
actualizado: 2026-09-14
---
# Base de datos — catálogo completo (verificado 2026-09-14)

**Proyecto:** Supabase `tnkpxnpqicsmlsggisyz` (giftiloveloyalty01).

**Qué es cada cosa:**
- **Esquema completo:** las 150 migraciones de `supabase/migrations/`. Éste es el mapa para no tener que leerlas todas.
- **Datos de configuración:** `supabase/estado/configuracion-2026-09-14.json`.

| Cosa | Cuántos |
|---|---|
| Tablas (public) | 62 — **todas con RLS encendido** |
| Funciones (public) | 188 |
| Triggers (public/auth/storage) | 19 |
| Políticas RLS (public/storage) | 51 |
| Crons | 13 |
| Buckets | 4 |
| Extensiones | plpgsql, pg_stat_statements, uuid-ossp, pgcrypto, supabase_vault, pg_cron, pg_net |
| Migraciones | 150 (2026-08-19 → 2026-09-15 UTC) |

**Datos vivos al 2026-09-14:**
- 8 usuarios de Auth y 8 perfiles.
- 5 tiendas.
- 7 filas de equipo: 2 corporativos, 4 socios y 1 staff.
- 7 correos en cola, los 7 enviados.
- 2 transacciones de monedero (modo prueba).
- 0 franquicias, 0 gerentes, 0 cupones de Giftilunch, 0 vendedores y 0 comisiones.

## Tablas (columna:tipo, `!` = obligatoria) · filas

**Tiendas y plataforma**
- `partners` (5): id:text!, slug!, name!, tagline, description, city, segment, color, image_url, membership_plan_id!, welcome_gift_enabled=true, show_other_stores=true, show_ads=true, home_delivery=false, welcome_video_url, created_at, is_demo=false, status='activo', status_changed_at/by, status_reason, cravings text[], distance_km, rating, quotas jsonb, monthly_price_mxn int, cards_addon, giftilonch_enabled=false, gifti_card, gifti_master_love, contact_name/email/phone/phone2, address_short, map_url, stars, **parent_partner_id** (franquicia), points_per_mxn=1, **welcome_video_enabled=true**, **video_propio=false**.
- `membership_plans` (4): id!, label!, price_mxn, trial_months, max_clients!, recommendations='no', ads, overage_price_mxn, overage_clients, features jsonb, quotas jsonb, orden, actualizado, actualizado_por.
- `platform_settings` (1, id=true): welcome_video_url, giftilonch_enabled=true, quotas_enforced=true, updated_at, addon_tarjetas_precio_mxn=1000, addon_tarjetas_meses_gratis=2, addon_tarjetas_planes text[].
- `billing_settings` (1): aviso_dias_antes=5, aviso_diario, dias_rescate=2, dias_para_borrar=5, iva_pct=16.
- `partner_config` (2): partner_id!, section!, data jsonb, version=1, updated_by, updated_at.
- `partner_config_snapshots` (0): id, partner_id!, kind!, label!, snapshot jsonb!, notes, created_by, created_at.
- `partner_subscriptions` (1): partner_id!, plan!, stripe_customer_id, stripe_subscription_id, status='activa', cards_addon, cards_free_until, current_period_end, grace_until, disabled_at, sold_by uuid, sold_at.
- `partner_stripe_accounts` (1): partner_id!, stripe_account_id, onboarding_complete=false, payouts_enabled=false.
- `partner_notices` (0) / `partner_notice_reads` (0): avisos de Giftilove al socio (titulo, cuerpo, liga, urgente).
- `partner_categories` (0): partner_id!, ambito!, nombre!, orden.
- `tool_usage` (1): partner_id!, tool!, ref_id, periodo date!, created_by, note. **Cuotas reales.**
- `support_access_log` (3): partner_id, actor, action!, detail.

**Personas y equipo**
- `profiles` (8): id!, full_name, phone, role='customer', home_partner_id, first_name, last_name_paternal, last_name_maternal, email, phone_country_code='+52', phone_country_name='México', birthdate, **has_kids_in_college=false**, university, photo_url, accepted_privacy_at/terms_at/marketing_at, last_active_at, lifecycle_status='active', ghl_contact_id, ghl_synced_at, public_code. (No hay `last_name`.)
- `staff_members` (7): id, pin, name!, role! (corporativo/socio/gerente/staff), partner_id, title, active=true, user_id, email, password_temporal=false, phone.
- `user_partner_activity` (0), `legal_consents` (0), `email_unsubscribes` (0), `referral_credits` (0).
- `sellers` (0): user_id, name!, email!, phone, ref_code!, status='pendiente', approved_by/at, demo_partner_id, notes, seller_number, applied_at, interview_notes.
- `commissions` (0), `seller_awards` (0), `sales_goals` (2), `demo_passes` (0).

**Cupones, ofertas, puntos, sellos**
- `partner_offers` (9): id, partner_id!, kind! (coupon/punch_card/courtesy/free_gift/paid_gift/points_catalog/giftilunch_coupon), origin, title!, description, terms_text, emoji, image_url, details jsonb, status='borrador', published_at, expires_at, billed_month, editable_until, created_by, megafono=false, releva_a, requiere_corporativo=false, activada_at, activada_por, activacion_motivo.
- `user_coupons` (0): user_id!, partner_id!, title!, note, emoji, image_url, source='wheel', status='activo', code!, expires_at, redeemed_at, giftable, redeemable_from, offer_id.
- `coupons` (0), `group_coupons` (0), `gift_catalog_items` (0), `staff_codes` (0), `ticket_codes` (0).
- `user_points` (0), `point_moves` (0), `punch_cards` (0), `punch_card_progress` (0), `punch_progress` (0).

**Juegos, rifas, publicidad, patrocinios**
- `partner_games` (3): partner_id!, kind! (wheel/slot/raffle), title='Gira y gana', active, grant_ttl_hours=48, visibility='both', points_cost, status='activo', starts_at, expires_at, billed_month, editable_until.
- `game_prizes` (9), `game_grants` (7), `game_campaigns` (1).
- `raffles` (1), `raffle_entries` (0), `raffle_codes` (0), `raffle_draws` (0).
- `ad_campaigns` (0), `ad_placements` (0), `sponsor_campaigns` (0), `sponsor_allocations` (0).

**Regalos, tarjetas, dinero**
- `gifts` (0): title!, message, image_url, video_url, sender_type!, from_partner_id, from_user_id, to_user_id, status='pendiente', redeem_mode, recipient_contact, from_person_name, amount_mxn, stripe_session_id, stripe_payment_intent_id, split, payment_method='stripe', video_path, video_expires_at, keepsake_sent_at, keepsake_status, service_fee_mxn, hold_status, hold_expires_at, redeem_code=gen_gift_code().
- `gift_cards` (0) + `gift_card_ledger` (0): GiftiCard / GiftiMASTERLOVE.
- `wallets` (2), `wallet_transactions` (2, modo prueba).

**Giftilunch**
- `giftilunch_children` (0), `giftilunch_cards` (0), `giftilunch_card_transactions` (0), `giftilunch_menu_items` (0), `giftilunch_listings` (0), `giftilunch_cupon_codes` (0), `gifty_lunch_orders` (0).

**Correo**
- `notification_queue` (7): user_id, partner_id, kind!, payload jsonb, send_day date (hora de México), send_at, sent_at, attempts=0, last_error. Índice único `notification_queue_once_idx` (user_id, kind, send_day).
- `email_templates` (8): kind!, asunto!, titulo, cuerpo, boton, updated_by, updated_at.

## Funciones (184)
**Sólo el servidor puede ejecutarlas** (sin permiso para anon ni authenticated):
approve_seller, backfill_commissions, can_email, cuota_consumir, expire_game_grants, gen_claim_code, gen_demo_pass, gen_invite_code, gen_public_code, generate_commission, gift_videos_huerfanos, gift_videos_para_recuerdo, gift_videos_por_purgar, giftilunch_create_card, giftilunch_credit_topup, giftilunch_expire_orders, gifts_para_liberar, goals_for_month, handle_new_user, launch_game_campaign, next_seller_number, offers_used_this_month, partner_delete_preview, points_move, raffles_used_this_month, rescue_subscription, reset_demo_store, run_billing_cycle, run_raffle_draw, sales_leaderboard, set_partner_status, staff_admin_ok, staff_by_pin, staff_can_act, tools_used_this_month, unsubscribe_token.

**Sólo con sesión** (authenticated): claim_gift, claim_offer, correos_enviados.

**El resto** se puede llamar desde el navegador, pero cada una **revisa por dentro quién llama** (`soy_corporativo`, `mi_rol_de_staff`, `puedo_en_tienda`, `staff_can_act`…).

Por dominio:
- **Quién manda:**
  - Vigentes: soy_corporativo(p_pin), soy_giftilove, mi_rol_de_staff, quien_manda, puedo_administrar_equipo, puedo_en_tienda, staff_vincular_cuenta, staff_password_cambiada.
  - Viejas por PIN: quien_manda_por_pin, staff_by_pin, staff_whoami, staff_admin_ok, staff_can_act, staff_list, staff_save, staff_set_active, gen_staff_pin (2 firmas).
- **Solicitudes de demo (2026-09-14):** `demo_requests` (RLS sin políticas); `demo_solicitar(nombre, correo, negocio, whatsapp, dia, hora)` abierta a anon (valida, WhatsApp con `telefono_whatsapp`, 3 por correo/hora); `demo_solicitudes()` y `demo_solicitud_actualizar(id, estado, notas)` sólo Giftilove.
- **Categorías (2026-09-14):** `partner_categories` ya se usa desde la app (`categoria_guardar` / `categoria_quitar`).
- **Todo a la base (2026-09-14):** `user_favorites` (RLS por dueño), `referral_invites` (sin políticas: sólo funciones), `profiles.giftilunch_interes_at`; `tarjeta_avisos_aceptar(card, avisos)` → `legal_consents` (party 'destinatario'); `referido_invitacion`, `referido_calificar`, `referido_cobrar_mio` (suma con `points_move`). Borradas: `seed_points`, `referido_registrar`, `referido_cobrar`.
- **Logo (2026-09-14):** `partners.logo_url` (URL de imagen subida, nunca `data:`). Lo guarda `partner_branding_save` (también `welcome_gift_enabled`). Ya no vive en `partner_config.branding` (se quitaron `logoDataUrl` y `welcomeGiftEnabled` del paquete).
- **Estrellas (2026-09-14):** una sola columna `partners.rating` (0–5, un decimal). `stars` BORRADA. La escriben `mi_tienda_contacto` (socio, valida) y `partner_save` (Corporativo); la leen el directorio, `red_de_socios` (como `estrellas`) y `partner-config`.
- **WhatsApp (2026-09-14):** `telefono_whatsapp(text)` — normaliza a E.164 (10 dígitos → +52; +521… → +52…) y rechaza lo inválido. La usa `mi_tienda_contacto` para `contact_phone`/`contact_phone2`. Migración 20260915015038; convirtió el teléfono de dunia a +52.
- **Contacto de la tienda (2026-09-14):** `mi_tienda_contacto(p_partner_id, p_datos)` — socio de esa tienda o corporativo; acepta `contact_email` (validado, en minúsculas, no se puede borrar), responsable, teléfonos, dirección y mapa.
- **Tiendas y red:** partner_alta, partner_alta_deshacer, partner_save, partner_branding_save, partner_cambiar_plan, partner_herramientas, partner_pausar, partner_eliminar, partner_purgar, partner_delete_preview, set_partner_status, partner_dashboard, red_de_socios, franquicias_de, mi_tienda_completa, mi_tienda_contacto, save_partner_config, categoria_guardar, categoria_quitar, color_reservado, hex_a_oklab, socio_relevar.
- **Equipo:** staff_alta_por_correo (2 firmas), staff_asignar_correo, staff_baja, equipo_de_mi_tienda, staff_todas_las_cuentas.
- **Planes y cuotas:** planes_listar, plan_guardar, addon_tarjetas_guardar, platform_settings_save, cuota_estado, cuota_limite, cuota_consumir, cuota_periodo, tools_used_this_month, offers_used_this_month, raffles_used_this_month, corporate_gifts_by_partner.
- **Ofertas y cupones:** claim_offer, claim_welcome_gift, welcome_gift_offer, mi_cupon_de_pieza, claim_points_reward, offer_is_editable, cortesia_relevar, expire_due_offers, expire_due_coupons, cap_coupon_expiry (trg), cap_coupon_to_offer (trg), block_early_redeem (trg), set_offer_defaults (trg), set_billed_month (trg).
- **Puntos, sellos, tickets:** my_points, points_move, seed_points, my_punches, punch_stamp, ticket_emitir, ticket_canjear.
- **Juegos y rifas:** buy_game_grant_with_points, claim_game_code, play_game_grant, game_points_expectation, launch_game_campaign, expire_game_grants, expire_due_games, set_game_defaults (trg), set_game_prize_delay (trg), enter_raffle, claim_raffle_code, raffle_entry_count, raffle_show, raffle_prize_image, run_raffle_draw, finish_raffle_draw, raffles_normalize (trg), raffle_game_visibility (trg), set_raffle_defaults (trg), gen_raffle_code, gen_claim_code.
- **Regalos y tarjetas:** claim_gift, gift_cash_create, gift_cash_cobrar, gift_cash_pendientes, gifts_cobrar_al_canjear (trg), gifts_para_liberar, gift_videos_*, gen_gift_code, gift_card_balance, gift_card_by_code, gift_card_days, gen_gift_card_code, expire_due_gift_cards.
- **Giftilunch:** giftilunch_agregar_hijo, giftilunch_mis_hijos, giftilunch_quitar_hijo, claim_giftilunch_invite, gen_invite_code, giftilunch_create_card (trg), giftilunch_credit_topup, giftilunch_spend, giftilunch_catalogo, giftilunch_menu_guardar, giftilunch_menu_hijo, giftilunch_pedir, giftilunch_checkout, giftilunch_mis_pedidos, giftilunch_pedidos_hijo, giftilunch_pendientes_tienda, giftilunch_entregar, giftilunch_expire_orders, giftilunch_discontinue, giftilunch_listings_admin, giftilunch_listing_guardar, giftilunch_listing_quitar, giftilunch_wallet_balance, giftilunch_wallet_id, gen_giftilunch_code, giftilunch_cupon_guardar, giftilunch_cupon_retirar, giftilunch_cupones_de, giftilunch_cupones_por_activar, giftilunch_cupon_activar, giftilunch_menu_cupones, giftilunch_cupon_emitir, giftilunch_cupon_canjear.
- **Vendedores, cobranza, demo:** my_seller_status, approve_seller, next_seller_number, gen_ref_code, generate_commission, backfill_commissions, goals_for_month, sales_leaderboard, run_billing_cycle, rescue_subscription, check_demo_pass, my_demo_pass, gen_demo_pass, purge_demo_passes, reset_demo_store.
- **Publicidad, patrocinio, avisos:** ads_for_partner, sponsor_campaign_remaining, check_sponsor_allocation (trg), aviso_publicar, aviso_leido, avisos_sin_leer, mis_avisos.
- **Correo:** email_templates_listar, email_template_guardar, correos_enviados, encolar_bienvenida_del_cliente (trg), queue_monthly_expiry_notices, can_email, unsubscribe_email, unsubscribe_token.
- **Llave del cron y referidos (2026-09-14):** llave_del_cron (sólo la base la ejecuta), llave_del_cron_valida (sólo service_role), referido_registrar, referido_cobrar (anon/authenticated).
- **Registro y utilidades:** handle_new_user (trg), gen_public_code, video_propio_sigue_al_paquete (trg), set_updated_at (trg), mx_month_end, mx_tomorrow_start.

## Triggers (19)
- `auth.users` → `on_auth_user_created` AFTER INSERT → handle_new_user.
- `public.profiles` → `trg_bienvenida_del_cliente` AFTER INSERT → encolar_bienvenida_del_cliente.
- `public.partners` → `trg_video_propio_sigue_al_paquete` BEFORE UPDATE.
- `public.giftilunch_children` → `on_giftilunch_child_created` AFTER INSERT → giftilunch_create_card.
- `public.gifts` → `gifts_cobrar_al_canjear` AFTER UPDATE.
- `public.partner_games`: `partner_games_defaults` (BEFORE INSERT) y `partner_games_raffle_visibility` (BEFORE INSERT/UPDATE).
- `public.partner_offers` → `partner_offers_defaults` BEFORE INSERT.
- `public.raffles`: `raffles_defaults` (BEFORE INSERT) y `raffles_normalize_trg` (BEFORE INSERT/UPDATE).
- `public.sponsor_allocations` → `sponsor_allocations_check` BEFORE INSERT/UPDATE OF units.
- `public.user_coupons`, en este orden:
  - `user_coupons_block_early_redeem` (BEFORE UPDATE OF status)
  - `user_coupons_cap_expiry` (BEFORE INSERT/UPDATE OF expires_at)
  - `zz_user_coupons_prize_delay` (BEFORE INSERT)
  - `zzz_user_coupons_cap_to_offer` (BEFORE INSERT)
  - Los prefijos `zz`/`zzz` fijan el orden a propósito.
- `set_updated_at` en group_coupons, partner_stripe_accounts, punch_card_progress y wallets.
- Los 3 de storage son del sistema: enforce_bucket_name_length, protect_buckets_delete, protect_objects_delete (+ update_objects_updated_at).

## Políticas RLS (53) — lo esencial
- **Lectura pública:**
  - ad_campaigns (activas y en fecha), ad_placements, game_prizes (active), partner_games (active), partner_offers (publicada y vigente), raffles (scheduled/open/spinning/completed), raffle_draws.
  - membership_plans (**dos políticas iguales**: `planes_lectura_publica` y `temp_open_select`), partner_categories, platform_settings.
  - partners (no eliminados, o corporativo), giftilunch_listings (activas de tiendas con Giftilunch).
- **Sólo lo mío** (auth.uid()): profiles (select/insert/update), user_coupons (select/update), user_points, point_moves, punch_progress, game_grants, raffle_entries, gift_cards, gift_card_ledger, legal_consents, user_partner_activity, gifts (de/para mí), commissions y seller_awards (vendedor dueño), giftilunch_* (papá o hijo ligado), gifty_lunch_orders.
- `tool_usage`: Giftilove o staff activo de esa tienda. `giftilunch_cupon_codes`: quien lo emitió o puedo_en_tienda.
- **Cerradas el 2026-09-14:** `referral_credits_all` y `stripe_socio_lectura` ya no existen (esas tablas sólo se tocan por funciones). Queda la duplicada `temp_open_select` en membership_plans (inofensiva: repite una lectura pública).
- **Storage:**
  - avatars: lectura pública; cada quien sube/cambia/borra en su carpeta `<uid>/`.
  - gift-videos: privado; cada quien en su carpeta.
  - public-images: lectura pública; escribe sólo service_role.
  - welcome-videos: se sube con liga firmada.

## Crons (13) — hora UTC
| Nombre | Cuándo | Qué |
|---|---|---|
| giftilunch-expiry-sweep | :07 cada hora | giftilunch_expire_orders() |
| game-grants-expiry | :23 cada hora | expire_game_grants() |
| giftilove_email_drain | cada 5 min | http_post → email-drain {llave, limit 25}, timeout 90 s |
| giftilove_expire_coupons | 06:10 | expire_due_coupons() |
| giftilove_expire_offers | 06:15 | expire_due_offers() |
| giftilove_expire_giftcards | 06:20 | expire_due_gift_cards() |
| giftilove_expire_games | 06:25 | expire_due_games() |
| giftilove_billing_cycle | 06:30 | run_billing_cycle() |
| giftilove_purge_gift_videos | 06:35 | http_post → gift-video {llave, purgar}, timeout 90 s |
| giftilove_backfill_commissions | 06:40 | backfill_commissions() |
| giftilove_liberar_apartados | 06:45 | http_post → gift-payment {llave, liberar}, timeout 90 s |
| giftilove_purge_demo_passes | 06:50 | purge_demo_passes() |
| giftilove_expiry_notices | 15:00 | queue_monthly_expiry_notices() |

`giftilove_ghl_drain` se dio de baja (jobid 6).

## Buckets (4)
| Bucket | Acceso | Límite | Tipos |
|---|---|---|---|
| avatars | público | 2 MB | jpeg/png/webp |
| public-images | público | 5 MB | jpeg/png/webp/gif |
| gift-videos | **privado** | 8 MB | webm/mp4/quicktime |
| welcome-videos | público | 25 MB | mp4/webm/quicktime |

## Tiendas y equipo vivos (2026-09-14)
| Tienda | Plan | Ciudad | Nota |
|---|---|---|---|
| dunia-pizzeria | gold | Puebla | video_propio true; socio duniapanaderiaartesanal@gmail.com; staff de prueba prueba.mesero@giftilove.test |
| nikkey-sushi | premium | CDMX Roma Norte | socio design01.magnet@gmail.com |
| mc-dowell-s-burgers | estandar | Puebla Angelópolis | socio elcampeonfrituras@gmail.com |
| silver-fish | prueba | Puebla | socio holashopyshop@gmail.com |
| demo (slug american-burguers) | estandar | Demostración | is_demo; la usa el vendedor |
Corporativos: giftiloveloyalty@gmail.com y dgcarlospedraza@gmail.com.

## Historia de las migraciones (por fase)
- **08-19:**
  - Base inicial, monederos, Stripe Connect, libro de ventas, efectivo.
  - Pedidos de Giftilunch, referidos.
- **08-21/22:**
  - Perfil de registro y RLS, código público y avatares.
  - Menú y tarjeta por hijo, recargas.
  - Ruleta y rasca, campañas de activación.
- **08-27/28:**
  - Rifas GiftiPlayWin y motor de sorteo, tragamonedas.
  - Vencimientos y avisos mensuales, tarjetas de regalo.
  - Cron de GHL, patrocinios, snapshots de soporte, configuración del socio en servidor.
- **08-29/31:**
  - Premios con imagen, publicidad, staff atado a tienda.
  - Herramientas como consumibles mensuales, bucket de imágenes.
  - Cobranza y metas, baja de correos, cron de correos.
  - Vendedores y comisiones, tienda demo, pases demo.
- **09-01:** pausar/eliminar socio; candados a funciones sensibles.
- **09-03:**
  - Videos de regalo y tarjeta de recuerdo, regalo apartado y cobro al canjear.
  - Puntos y sellos en servidor, alta de equipo en base, piezas publicadas, Giftilunch al servidor.
  - Cierre de políticas abiertas, códigos de acceso serios.
- **09-04:**
  - Tiendas como fuente única en base, regalo de bienvenida real, tablero de Corporativo.
  - Giftilunch por hijo completo, marca y ajustes al servidor.
  - Equipo entra con correo y contraseña, rol gerente, contraseña temporal.
  - Nombres socio/corporativo, colores reservados de Giftilove, herramientas con sesión.
  - Alta de socio nuevo, quien_manda único.
- **09-05:**
  - Cadena del socio, cambiar plan/pausar/eliminar con confirmación, alta con responsable.
  - Purgar tienda, aviso de cambio de plan y relevo, megáfono y avisos, tickets con código real.
  - Planes editables, franquicias sin huérfanas.
- **09-12:**
  - Cuotas reales (cuota_estado/consumir), megáfono con cuota, menú de cupones de Giftilunch.
  - partner_save y ajustes con sesión, borrado de staff_rotate_pin, correos maestros editables.
- **09-13:**
  - Video de la plataforma, historial de correos real, bienvenida al cliente.
  - Cron de correos con timeout de 90 s, nombre en el sobre.

## 2026-09-15 / 16 — Megáfono, atribución, buscador y cuenta del socio

**162 migraciones aplicadas = 162 archivos en `supabase/migrations/`**
(verificado al cerrar la sesión).

### Tablas nuevas
| Tabla | Para qué |
|---|---|
| `megafono_envios` | cada lanzamiento, con su ventana de salida, nivel y audiencia |
| `megafono_compras` | megáfonos comprados con Stripe (subtotal, descuento, total) |
| `megafono_ajustes` | una fila: precios, horarios, descuentos, topes, WhatsApp |
| `megafono_whatsapp`, `whatsapp_remitentes` | lo que espera a GHL, por portafolio |
| `partner_qr` | varios códigos QR por tienda, con la etiqueta del lugar |
| `registro_origen` | de dónde salió cada cuenta (qr / compartir / staff / ubicacion / directorio) |

### Columnas nuevas
- `partners.lat`, `partners.lng` — el pin del local (sólo para sugerir tiendas cercanas).
- `partners.quitada_at` — pieza retirada después de la ventana de 30 min.
- `platform_settings.registro_radio_metros` (200).
- `billing_settings.descuento_semestral_pct` (10), `.meses_gratis_anual` (1).
- `tool_usage.origen` — `plan` | `compra`.

### Funciones que conviene conocer
- **Megáfono:** `megafono_lanzar(tipo, ref, nivel)`, `megafono_audiencia`,
  `megafono_alcance`, `megafono_cancelar`, `megafono_cuenta`, `megafono_ventana`,
  `megafono_compra_crear` / `_acreditar`, `megafono_ajustes_leer` / `_guardar`.
- **Atribución:** `qr_escaneado` (única abierta a `anon`), `qr_listar`,
  `qr_guardar`, `qr_quitar`, `registro_atribuir`, `registro_origen_resumen`,
  `tiendas_cercanas(lat, lng, limite, precision)`, `mi_tienda_ubicacion`,
  `registro_radio_leer` / `_guardar`.
- **Buscador:** `clientes_buscar(...)` (pagina en el servidor),
  `clientes_resumen`, `clientes_universidades`, `sin_acentos` (unaccent
  inmutable) + índice GIN de trigramas sobre `profiles`.
- **Cuenta del socio:** `mi_cuenta_estado(tienda)` — estado, color, plan,
  vencimiento y las tres formas de pagar con su ahorro ya calculado.
- **Una sola verdad al cobrar:** `suscripcion_aplicar_plan` (sólo
  `service_role`) escribe `partner_subscriptions` **y**
  `partners.membership_plan_id` en el mismo movimiento.
- `handle_new_user` ahora lee `origen_tienda` / `origen_medio` / `origen_qr` de
  los metadatos del alta. **Esas tres llaves son un contrato con el frontend.**
- `email_templates` tiene un tipo nuevo: **`pago_recibido`** (comprobante de
  pago exitoso), editable desde Corporativo.

## 2026-09-16 — El reparto de canjes vive en la base

- **`user_coupons.destino`** (`cupones` | `regalos`), con `check` y con índice
  `(user_id, destino, status)`. Lo llena el trigger **`set_cupon_destino`**
  usando **`cupon_destino(source)`**, que es la única verdad del reparto.
- Mapeo probado con las 8 fuentes reales (y revertido): `gift` → **regalos**;
  `promo`, `punch`, `points`, `wheel`, `slot`, `scratch`, `raffle` →
  **cupones**.
- `user_coupons.source` tiene **lista cerrada** (`user_coupons_source_check`):
  wheel, scratch, slot, gift, points, staff, promo, raffle, punch. Para agregar
  una fuente hay que tocar ese check **y** `cupon_destino`.
- Quién emite cada cupón: `claim_offer` (`promo` o `gift`), `punch_stamp`
  (`punch`, al completar la tarjeta), `claim_points_reward` (`points`),
  `play_game_grant` (`wheel`/`slot`/`scratch`) y `run_raffle_draw` (`raffle`).

**163 migraciones** aplicadas = 163 archivos en el repo.

## Tipos de cupón por tienda (2026-09-16)
- `tipos_cupon_de_fabrica()` → las 8 llaves de fábrica.
- Tabla **`partner_coupon_types`** (partner_id, nombre, orden), único por
  `(partner_id, lower(nombre))`, lectura pública.
- `tipo_cupon_guardar` / `tipo_cupon_quitar` (sólo quien administra la tienda;
  no se borra uno que usen piezas vivas) y `tipo_cupon_valido(tienda, tipo)`,
  que llama `offers-admin` antes de guardar.

## Qué tan lleno está el entorno (2026-09-16)
- **`entorno_de_tienda(tienda)`** (interna) → `{tienda, plan, listos, total,
  porcentaje, items[{llave, etiqueta, pestana, listo}]}`. Cuotas = plan con
  encima las ajustadas a la tienda; cada herramienta con cuota > 0 pide una
  pieza viva; identidad siempre cuenta; megáfono no.
- **`mi_entorno_estado(tienda)`**: el socio el suyo, Giftilove cualquiera.
- **`entornos_de_la_red()`**: sólo Giftilove, tiendas activas sin la demo.
- `pestana` es la llave de la pestaña del socio: **si se renombra una pestaña
  en `AdminDashboard`, hay que cambiarla aquí**.
- Medido 2026-09-17: las 4 reales en 36% (5/14); demo 79%.

**166 migraciones** aplicadas = 166 archivos en el repo (verificado 2026-09-17).

## Ranking del directorio (2026-09-17)
- `planeacion_de_tienda(tienda)` → 0-100: por herramienta del plan, piezas vivas
  ÷ cupo (tope 1), promediado. Interna.
- `ranking_de_tienda(tienda)` → {entorno, planeacion, periodicidad, meses, bono,
  puntaje}. Interna.
- `directorio_orden()` (anon) → partner_id, posicion. **Sólo la posición.**
- `tiendas_buscar(texto, limite)` (anon) → por nombre, `sin_acentos` + trigramas
  (índice `partners_nombre_busqueda_idx`), en orden del ranking.
- `entornos_de_la_red()` ahora trae planeacion, periodicidad, puntaje y posicion.
- `platform_settings.ranking_pesos` = {entorno 60, planeacion 25, contrato 15};
  `ranking_pesos_leer` / `ranking_pesos_guardar` (corporativo).

## Formas de pago (2026-09-17)
- Tabla **`formas_de_pago`** (id, etiqueta, meses 1-12 único, descuento_pct,
  meses_gratis < meses, activa). Semilla: mensual, semestral (10%), anual (1 de
  regalo). **Se borraron** `billing_settings.descuento_semestral_pct` y
  `meses_gratis_anual`.
- `precio_de_forma(mensual, meses, descuento, regalo)` — fórmula única.
- `formas_de_pago_listar()`, `forma_de_pago_guardar(...)` (soy_giftilove).
- `partner_subscriptions.periodicidad` (FK a formas_de_pago), la llena
  `suscripcion_aplicar_plan(..., p_periodicidad)`.
- `mi_cuenta_estado` arma `pagos` con las formas activas y trae `periodicidadActual`.

**168 migraciones** aplicadas = 168 archivos en el repo (verificado 2026-09-17).

## Favoritas primero y avisos de tiendas (2026-09-17)
- `profiles.directorio_favoritos_primero` + `directorio_favoritos_primero_leer()` /
  `directorio_favoritos_primero_guardar(bool)`.
- `directorio_orden(p_favoritos_primero)` y `tiendas_buscar(texto, limite,
  p_favoritos_primero)` → partner_id, posicion, **favorito**.
- `user_favorites` (PK user_id+partner_id; RLS: cada quien ve, agrega y borra lo
  suyo). Probado con sesión simulada.
- **`entorno_diario`** (partner_id, fecha, porcentaje, puntaje).
- **`avisos_giftilove`** (partner_id, motivo `sin_movimiento`|`bajo_porcentaje`,
  detalle, creado_at, atendido_at, atendido_por).
- `revisar_entornos()` (sólo service_role) — cron **`giftilove_revisar_entornos`**
  `10 13 * * *`. Encola `giftilove_tiendas_descuidadas` con `payload.para`, uno
  por cuenta corporativa al día.
- `avisos_giftilove_listar(incluir_atendidos)`, `aviso_giftilove_atender(id)`.

**169 migraciones** aplicadas = 169 archivos en el repo (verificado 2026-09-17).

## Entorno por categoría (2026-09-17)
- `entorno_de_tienda`: cada item trae `categoria` (basico | linea | opcional |
  recomendable). % = listos / total sólo de basico + linea. Nuevo item `megafono`
  (recomendable) listo si hay `megafono_envios` sin cancelar en 30 días.
- `planeacion_de_tienda`: ya no cuenta courtesy ni free_gift.
- Medido: 4 reales en 42% (5/12), demo 75%.

**170 migraciones** aplicadas = 170 archivos en el repo (verificado 2026-09-17).

## La regla regalo/cupón (2026-09-17)
- `cupon_destino(source, kind)`: 'gift' + free_gift → **cupones**; 'gift' →
  **regalos**; lo demás → cupones.
- `set_cupon_destino` busca el `kind` por `offer_id` antes de decidir.
- Filas viejas reacomodadas en la misma migración.

**171 migraciones** aplicadas = 171 archivos en el repo (verificado 2026-09-17).

## El banner principal (2026-09-17)
Migración `banner_principal_como_herramienta_de_la_tienda` (md5 idéntico al archivo).
- `partner_offers.kind` acepta **`'banner'`**.
- `membership_plans.quotas.banners` y `partners.quotas.banners` = lo mismo que
  `punchCards` (4 | 8 | 12 | 18).
- `claim_offer` acepta el banner y decide la fuente con `details.entrega`:
  `regalo` → `gift` (caja), cualquier otra cosa → `promo` (Mis cupones).
- `entorno_de_tienda` lo cuenta como herramienta **de línea** (pestaña `banner`) y
  `planeacion_de_tienda` lo incluye.

**172 migraciones** aplicadas = 172 archivos en el repo (verificado 2026-09-17).

## La cortesía se mudó al banner (2026-09-17)
Migración `la_cortesia_se_muda_al_banner_principal` (md5 idéntico al archivo).
- Las piezas `courtesy` pasaron a `kind='banner'` con `details.entrega='regalo'`.
- `partner_offers_kind_check` ya **no** acepta `'courtesy'`; `claim_offer` tampoco.
- `quotas.courtesyGifts` borrado de `membership_plans` y de `partners`.
- `entorno_de_tienda` sin el renglón de cortesía; `welcome_gift_offer` busca en el
  banner y ya no mira `partners.welcome_gift_enabled` (columna que queda sin uso).
- Medido después: Dunia 46% (su regalo de bienvenida ya cuenta como banner), la
  demo 77%, las otras tres en 38%.

**173 migraciones** aplicadas = 173 archivos en el repo (verificado 2026-09-17).

## El catálogo de Stripe se revisa solo (2026-09-17)
Migración `el_catalogo_de_stripe_se_revisa_solo_cada_dia` (md5 idéntico al archivo).
- Cron **`giftilove_stripe_catalogo`** `20 13 * * *` (7:20 México) llama a
  `stripe-setup` con `public.llave_del_cron()` y `timeout_milliseconds := 90000`.
- Así un precio cambiado en Membresías o una forma de pago nueva llegan a Stripe
  sin que nadie toque un botón. **15 crons.**

**174 migraciones** aplicadas = 174 archivos en el repo (verificado 2026-09-17).

## Cobrarle a cualquier socio (2026-09-17)
Migración `precios_de_membresia_para_cobrarle_a_cualquier_socio` (md5 idéntico).
- `precios_de_membresia()`: plan × forma activa con su total, calculado con
  `precio_de_forma`. Son precios de lista, así que la puede leer cualquiera.
- Quien aplica el plan sigue siendo `suscripcion_aplicar_plan` desde el webhook.
  Probado y revertido con 3 tiendas: plan, cupos, periodicidad y ranking cambian
  al entrar el pago.

**175 migraciones** aplicadas = 175 archivos en el repo (verificado 2026-09-17).

## El plan tiene una sola verdad (2026-09-17)
Migración `el_plan_tiene_una_sola_verdad_para_socio_y_corporativo` (md5 idéntico).
- Disparadores `partner_precio_sigue_al_plan`, `suscripcion_sigue_al_plan` y
  `plan_precio_a_sus_tiendas`: precio y suscripción siguen siempre al plan.
- `partner_cambiar_plan` ignora el precio del navegador; `mi_cuenta_estado` lee
  el del plan y devuelve `cobraEnStripe`.
- Alineadas las filas desfasadas (la demo tenía gold/prueba/$2,400).

**178 migraciones** aplicadas = 178 archivos en el repo (verificado 2026-09-17).

## La demo, sólo para vendedores (2026-09-17)
Migración `la_demo_solo_para_vendedores_aprobados` (md5 idéntico).
- Tabla `demo_accesos` (RLS sin políticas) + `demo_entrar(numero)`,
  `demo_acceso_vigente()` y `demo_salir()`, sólo para `authenticated`.
- Probado y revertido: número malo, cliente cualquiera y vendedor dado de baja
  quedan fuera; número bueno abre por 12 h.

**179 migraciones** aplicadas = 179 archivos en el repo (verificado 2026-09-17).

## Correo del vendedor aprobado con montos reales (2026-09-17)
Migración `el_correo_del_vendedor_aprobado_lleva_montos_reales` (md5 idéntico).
- `approve_seller` manda `goldMxn`, `pctPrimera` y `pctRecurrente` en el aviso.

**180 migraciones** aplicadas = 180 archivos en el repo (verificado 2026-09-17).

## 2026-09-18 — Demo de ventas y tienda por vendedor
**182 migraciones** aplicadas = 182 archivos en el repo (md5 idéntico, verificado 2026-09-18).
- `20260918074917_la_demo_de_ventas_vendedor_socio_prospectos_y_reinicio`:
  - `profiles.role` acepta `prospecto` (cuenta de demostración). `handle_new_user` la marca con `demo_prospecto`.
  - `clientes_buscar`/`clientes_resumen` excluyen `prospecto`.
  - Candado `cuenta_de_demo_solo_en_la_demo` (user_coupons, raffle_entries, game_grants, gifts).
  - Tablas `demo_claves` (clave 24 h), `prospectos` (clientes potenciales → GHL), `demo_base` (punto de partida).
  - Funciones `demo_clave_mia`, `demo_clave_de_quien` (servicio), `vendedor_mis_prospectos`, `demo_base_fijar`, `demo_acceso_vigente` (acepta al cliente del vendedor).
  - Plantilla `vendedor_aprobado` con fichas `pct_primera`, `pct_recurrente`, `gold`, `gold_primer_mes`, `gold_cada_mes`.
- `20260918183209_cada_vendedor_con_su_propia_tienda_demo`:
  - `sellers.demo_asignada_at`. Tienda por vendedor `demo-gl-001` "American Burguers · GL-001".
  - `demo_tienda_para`, `demo_poner_piezas`, `demo_vaciar`, `demo_liberar`, `demo_asignar`, `demo_tiendas_de_vendedores`, `mi_tienda_demo`.
  - `reset_demo_store(p_tienda)`; el molde usa `reset_demo_molde`.
  - `megafono_lanzar_interno`: en demo sale al momento y llega a los clientes del vendedor.
  - `vendedor_socio_de_la_demo`: el vendedor es socio SÓLO de su tienda.

- **2026-09-18 · `orden_de_menu`** (user_id, menu, orden text[]). Tiene RLS sin políticas y sólo la tocan `orden_de_menu_leer(menu)` / `orden_de_menu_guardar(menu, orden)` (sólo usuarios con sesión; guardar vacío borra la fila). Migraciones: **184**.

- **2026-09-18 · `20260919004009`**:
  - Nueva columna `partner_games.giros_staff`.
  - `play_game_grant`: el cupón dura `coalesce(valid_hours, 72)`.
  - `buy_game_grant_with_points`: el giro vence con la ruleta.
  - `partners.quotas` quedó en null para las demos.
  - Migraciones: **185**.

- **2026-09-18 · `20260919010956`**:
  - `cuota_consumir` avisa en español para `wheelGamesStaff` y `wheelGamesApp`.
  - `buy_game_grant_with_points` gasta `wheelGamesApp` (la demo no gasta), rechaza ruletas vencidas y las "De mesa".
  - Migraciones: **186**.


- **2026-09-18 · `20260919015425` + `20260919020337` · Poker 5**: `poker_mesas`, `poker_jugadores`, `poker_mazos` (sin políticas); `partner_games.kind='poker5'` con `poker_modo` (gana/paga/puntos), `poker_offer_id`; funciones `poker_evaluar`/`poker_resolver`/`poker_unirse`/`poker_repartir`/`poker_cambiar`/`poker_estado`/`poker_abrir`/`poker5_mesa_guardar`/`poker5_publicadas`/`poker5_config_listar`; cron `giftilove_poker5_limpieza` (5 min); `user_coupons.source` acepta `poker`. Migraciones: **188**.
- **2026-09-18 · `20260919024239`**: el cupón de la mesa vive en `game_prizes` slot 0 + `poker_mesas.premio` (foto instantánea). **189**.
- **2026-09-18 · `20260919031557` + `20260919033556` · PRECIO DE LOS CUPONES**: `user_coupons.precio_mxn / precio_normal_mxn / pago_estado (pendiente|pagado_tarjeta|pagado_caja) / pagado_at / pago_ref / pagador_id`; `game_prizes.precio_mxn/precio_normal_mxn`; trigger `cupon_toma_precio`; `cupon_pago_estado`, `cupon_pago_por_codigo`, `cupon_pagado_con_tarjeta` (service role, avisa a la tienda). Se borraron `poker_cobrar_en_caja` y `poker_pagado_con_tarjeta`. **191**.
- **2026-09-19 · `20260919051240`**: `poker5_mesa_eliminar(game)` (siempre; status 'terminado' + apagada; cancela megáfono); `poker5_config_listar` ya no trae las terminadas. **192**.
- **2026-09-19 · `20260919053030`**: `poker5Mesas` se gasta **al publicar** cada mesa (`poker5_mesa_guardar` → `cuota_consumir`, ref = id; la demo cuenta sin frenar); `poker_repartir` ya no gasta. **193**.
- **2026-09-19 · `20260919055447`**: contador nuevo `ruletas` en `membership_plans.quotas` (4/12/14/18); `cuota_consumir` lo nombra; `poker_abrir` ya no frena por cupo (con 18 mesas publicadas nadie habría podido jugar). Migraciones: **194** (verificado: 194 en la base = 194 en el repo, md5 idénticos del 18 y 19).

- **2026-09-19 · `20260919062158` · Dados**: `partner_games.kind` acepta `dice`; `game_prizes.regla jsonb` + `prioridad int`; `user_coupons.source` acepta `dice`; cupo `dados`/`diceGamesStaff`/`diceGamesApp` (4/8/10/14) en `membership_plans.quotas`; `dados_regla_aplica(regla,a,b)`; `play_dice_grant(grant)` → jsonb con `dados`, `gano`, cupón; `play_game_grant` rechaza dados; `buy_game_grant_with_points` gasta el contador de su juego; `entorno_de_tienda` suma "Unos dados". Migraciones: **195**.
