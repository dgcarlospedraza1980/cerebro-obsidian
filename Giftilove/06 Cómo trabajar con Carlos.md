---
tags: [giftilove, forma-de-trabajo]
actualizado: 2026-09-14
---
# Cómo trabajar con Carlos

## Lo que Carlos pidió explícitamente
- **Recomienda, no te disculpes.** No "perdón, no me fijé, se me pasó": busca
  aquí antes, y si algo ya falló, usa la lección de [[03 Incidentes]]. *2026-09-14.*
- **Pregunta menos.** Si está en este cerebro o en el repo, no se pregunta.
- **Arregla sólo lo que pidió.** Lo colateral se anota en [[02 Pendientes]] y se
  le dice, no se arregla solo.
- **No dejes basura** (archivos temporales, código muerto, datos inventados).
- **"Si algo falla, no inventes un fix. Pégame el error exacto."**
- Todo lo de producción debe quedar **publicado en todas partes** (push a
  `main` → GitHub, Vercel y Lovable).

## Cómo comunicarse
- **Español**, preciso, sin rodeos. Carlos verifica lo que se le dice.
- Formato para cambios: **Implementado / Cambios / Verificación / Notas**.
- Separar siempre **verificado**, **inferido** y **no ejecutado**. Nunca "todo
  funciona" sin probarlo.
- Cada turno le cuesta: terminar el trabajo completo, no explicar de más.
- Si hay un costo fijo nuevo (tokens por sesión), decirlo **antes** de que
  pregunte.
- Comandos para Carlos: un solo renglón, **sin textos para reemplazar**; si
  necesita un dato (llave), el renglón lo **pregunta**.

## Seguridad (no negociable)
- Nunca escribir contraseñas, llaves, tokens ni PIN en archivos, memoria,
  prompts o notas. Si hace falta, Carlos la pega él.
- Si Carlos pega una credencial en el chat: no reutilizarla, recomendar
  cambiarla.
- Pruebas contra datos reales: en transacciones que se deshacen.

## Con Lovable
- Verifica el archivo, no el reporte.
- `git pull` antes de tocar y antes de commitear.
- Prompts a Lovable en español y terminando con: *"NO toques nada más. Si algo no
  compila, repórtamelo, no lo arregles por tu cuenta."*
- Cada `send_message` cuesta créditos; leer (`get_diff`, `list_edits`) es gratis.

## Prácticas de ingeniería ("primero el repositorio")
- La evidencia del repo gana sobre las buenas prácticas genéricas.
- Mapear (graphify) → entender → impacto → plan → tocar.
- Cambio mínimo por causa raíz; nada de refactors fuera de alcance.
- Revisiones P0 (bugs/seguridad/datos) → P3 (estilo).

## Sincronía total (2026-09-14)
Carlos no acepta "falló porque no estaba actualizado en otra parte". Todo cambio se refleja en su fuente, en GitHub y en este cerebro en el mismo movimiento (tabla en `CLAUDE.md`). Antes de decir "listo" en backend: prueba de sincronía (`list_edge_functions` vs `supabase/functions/README.md`, `list_migrations` vs `supabase/migrations/`).

## Reglas globales de pantalla (2026-09-14)
- **Todo va a la base de datos.** Carlos: "tienes la muy mala costumbre de mostrar cosas que sólo se ven temporalmente en la computadora o en el caché; quiero todo arriba".
- **F5 no saca de donde estás.** "A cualquier parte donde entre y le dé F5 es para refrescar, y no tiene que salirse de donde esté."
