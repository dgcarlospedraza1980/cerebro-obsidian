---
tags: [giftilove, indice]
actualizado: 2026-09-14
---
# Giftilove — cerebro del proyecto

Este es el cerebro permanente de Giftilove. Existe porque la memoria de Claude
se pierde al compactar la conversación; esto **no se compacta**. Aquí vive lo
que se decidió (sí y no), por qué, lo que falta, lo que ya se rompió y cómo se
arregló, y el mapa de cuentas y herramientas.

## Instrucciones para Claude (obligatorias)
00. **REGLA DE SINCRONÍA TOTAL:** un cambio no está hecho hasta que todos sus espejos dicen lo mismo — Supabase/Vercel/Lovable ↔ GitHub ↔ este cerebro, en el mismo movimiento, con commit + push. Tabla completa en `CLAUDE.md` → "REGLA DE SINCRONÍA TOTAL"; la hacen cumplir los ganchos `sincronia.mjs` (PostToolUse y Stop) y `cerebro-obsidian.mjs` (SessionStart).
00b. **TODO VA A LA BASE DE DATOS** (Carlos, 2026-09-14): nada capturado se muestra sólo desde React, localStorage o caché; si lo usa más de una pantalla, es columna de su tabla. **F5 NO SACA DE DONDE ESTÁS**: pestañas y pantallas con `useVistaRecordada` (`src/lib/vista-recordada.ts`). Detalle en `CLAUDE.md` → "DOS REGLAS GLOBALES DE PANTALLA".
00c. **LAS TIENDAS:** 4 reales — Dunia Pizzería, Mc Dowell's Burgers, Nikkey Sushi, Silver Fish. **American Burguers (`demo`) es FICTICIA: la tienda con la que venden todos los vendedores.** Nunca mezclar sus datos. Ver [[07 Negocio]].
0. **Nunca contestes "no está en la memoria", "déjame checarlo" ni "se me olvidó".** Si no lo sabes, búscalo aquí, en el repo (`CLAUDE.md`, `supabase/`) o en la base, y contesta con el dato.
1. **Antes de preguntarle algo a Carlos o de tomar una decisión, busca aquí**
   (`search_simple` con el tema). Si la respuesta está, úsala y cita la nota.
   Sólo pregunta lo que de verdad no esté.
2. **Recomienda, no te disculpes.** Si algo ya falló antes, está en
   [[03 Incidentes]]: usa esa lección en vez de repetir el error.
3. **Al terminar algo que cambie el estado del proyecto**, en el mismo
   movimiento: actualiza [[02 Pendientes]], agrega la decisión a
   [[01 Decisiones]], el incidente a [[03 Incidentes]] y una línea en la
   bitácora del día (`Bitácora/AAAA-MM-DD`).
4. **Precedencia.** Reglas para tocar código → `CLAUDE.md` del repositorio
   (lo leen también Lovable y cualquier otro agente). Decisiones, el porqué,
   historia, pendientes, negocio y cuentas → aquí. Si chocan, gana lo más
   reciente y se corrige el otro en el mismo movimiento.
5. Antes de decir "no puedo" o "ya quedó" con una herramienta o una llave, revisa
   [[08 Herramientas del ecosistema]] y [[09 Llaves y secretos]].
   **Nunca** guardes aquí contraseñas, llaves, tokens ni PIN.
6. Esta carpeta es de Claude y Carlos. Las notas de Carlos fuera de
   `Giftilove/` no se tocan sin permiso.

## Mapa
- [[01 Decisiones]] — lo que SÍ y lo que NO, con fecha y motivo
- [[02 Pendientes]] — lo que falta, con dueño
- [[03 Incidentes]] — errores graves, causa raíz y lección
- [[04 Arquitectura]] — cómo está construido y dónde vive cada cosa
- [[05 Cuentas y herramientas]] — qué cuenta es cuál, conectores, herramientas y su costo
- [[06 Cómo trabajar con Carlos]] — reglas de trabajo y preferencias
- [[07 Negocio]] — planes, precios, niveles, tiendas, dinero
- [[08 Herramientas del ecosistema]] — qué se hizo con cada herramienta y hasta dónde llega Claude
- [[09 Llaves y secretos]] — dónde vive cada llave (nunca el valor) y su estado
- [[10 Manual de réplica]] — **reconstruir Giftilove desde cero, paso a paso**
- [[11 Base de datos]] — las 62 tablas, 184 funciones, triggers, RLS, crons y buckets
- [[12 Edge functions]] — las 28 funciones y qué hace cada acción
- [[13 Frontend]] — rutas, librerías, componentes y reglas que ya costaron caro
- [[14 Historial y versiones]] — línea del tiempo, commits clave y etiquetas
- `Bitácora/` — qué pasó cada día

## Estado en pocas líneas (2026-09-14)
- Producción: **giftilove.com** (Vercel) desde el repo `giftilove/happygift`,
  rama `main`. Versión estable etiquetada: **`v2026.09.13`**.
- Historial y último commit: [[14 Historial y versiones]]. Para reconstruir todo: [[10 Manual de réplica]].
- **Todo lo necesario para replicar está guardado:** código y 28 edge functions en el repo, 137 migraciones, foto de configuración (`supabase/estado/`) y esta bóveda. Sólo faltan los valores de las llaves, a propósito.
- La app ya no se sale al entrar a tiendas (3 causas arregladas).
- **Ningún peso real ha pasado**: Stripe sólo en modo prueba, sin suscripción
  recurrente.
- GHL + WhatsApp: fase final, no empezada.
- Lista de 76 puntos: https://claude.ai/code/artifact/c273b864-6314-46eb-bd10-a8720919914d

## Estado al 2026-09-16

- **El cobro del plan ya funciona punta a punta** (modo prueba): hay botón en
  Socio → Mi cuenta, el pago entra, el webhook lo registra y llega comprobante.
  Falta pasar Stripe a modo real.
- **El megáfono ya tiene a quién hablarle**: cada cliente queda ligado a la
  tienda que lo trajo (QR, compartir, mostrador o ubicación confirmada).
- **Dos cosas dependen de Carlos para funcionar**: correr el botón "Crear o
  actualizar los precios" (Corporativo → Planes) y que cada socio ponga su pin
  de ubicación.
- Ver [[Bitácora/2026-09-15]] para el detalle de toda esa sesión.
