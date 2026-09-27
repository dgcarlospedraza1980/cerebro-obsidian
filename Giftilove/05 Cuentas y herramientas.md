---
tags: [giftilove, cuentas, herramientas]
actualizado: 2026-09-14
---
# Cuentas y herramientas

**Aquí no hay contraseñas, llaves ni PIN. Nunca.**

## Qué correo es cuál
- `giftiloveloyalty@gmail.com` — **la cuenta de Giftilove**. De ella cuelgan
  Resend, dominio, correos, Gmail conectado. Corporativo principal.
- `dgcarlospedraza@gmail.com` — correo personal de administrador de Carlos:
  Corporativo **y** usuario normal de la app.
- `inmobiliarianah@gmail.com` — cuenta de Claude de Carlos; es de su **negocio
  inmobiliario**, no de Giftilove. Google tiende a autocompletarla y así se
  conectaron mal varios servicios. Hoy también es una cuenta de prueba de la app
  (a nombre de Monica).

## Conectores de Claude (verificados)
- Supabase → proyecto `giftiloveloyalty01` (`tnkpxnpqicsmlsggisyz`).
- GitHub ("GiftiLoveConector") → usuario `giftilove`, repo `giftilove/happygift`.
- Vercel → equipo `giftilove-vercel`.
- Resend → dominio `giftilove.com`.
- Gmail → `giftiloveloyalty@gmail.com`.
- Stripe → cuenta `GiftiLoveLoyalty` (sólo modo prueba en uso).
- Lovable → workspace "Carlos' Lovable", proyecto Giftilove.
- Google Drive, Google Calendar.
- **GHL: no hay conector** (será llave en secretos de Supabase + edge functions).

## Herramientas y llaves
El detalle de cada herramienta (qué se hizo, hasta dónde llega Claude, versión, costo y estado)
vive en [[08 Herramientas del ecosistema]]. Dónde está cada llave: [[09 Llaves y secretos]].
No se repite aquí para no tener dos verdades.

## Obsidian — datos útiles
- Plugin "Local REST API with MCP" v5.1.0 (trae su propio servidor MCP).
- Cambiar la llave: Ajustes → Local REST API with MCP → "Reset all
  cryptography" → **Reset all crypto**. ("Re-generate certificates" NO cambia
  la llave.)
- Volver a conectar: renglón de PowerShell que **pregunta** la llave (en la
  memoria de Claude). Nunca comandos con textos para reemplazar.

## Máquina de Carlos (Windows)
- `python` apunta a un stub roto; el real es `C:\Python314\python.exe`.
- Rutas largas (>260) rompen herramientas: usar carpetas cortas en Temp.
- Node 24, npm 11.
