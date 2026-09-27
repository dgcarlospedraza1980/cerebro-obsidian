---
tags: [giftilove, negocio]
actualizado: 2026-09-14
---
# Negocio

## Qué es
App de lealtad, cupones y regalos **de marca blanca** para tiendas físicas en
México. El dueño es **Carlos**. Un **socio** es la tienda que paga por usarla;
sus clientes entran a la app **del socio**. Dominio: **giftilove.com**.
Razón social con **"Systems"**.

## Niveles
1. **socio** — dueño de la tienda.
2. **corporativo** — equipo Giftilove: ve todo, da de alta socios, aprueba
   vendedores, edita planes y precios.
3. **gerente** — manda en una tienda. *Nunca probado con datos reales.*
4. **staff / vendedor** — atiende en mostrador; corporativo aprueba vendedores.
- **Franquicia**: tienda completa (dueño, plan y cobro propios); hereda de la
  matriz plan, color, ciudad y giro como valores por omisión; máximo 2 niveles.
  *Nunca se ha creado una.*

## Planes (en la base, editables desde Membresías)
| Plan | Precio MXN/mes | Megáfono/mes | Video propio |
|---|---|---|---|
| Prueba | 0 (3 meses) | 1 | no |
| Estándar | 1,200 | 2 | no |
| Premium | 1,800 | 4 | no |
| Gold | 2,400 | 6 | **sí** |
- Platino **no existe**.
- Add-on de tarjetas: precio y meses gratis en `platform_settings`.
- Modelo de cuota: "cuánto se puede gastar al mes"; se gasta al alcanzar al
  cliente y no se devuelve.

## Vendedores (según el correo de aprobación)
- 30% de la primera mensualidad y 10% de cada mes siguiente mientras el
  restaurante siga pagando. Ejemplo Gold: $720 el primer mes y $240 después.

## Giftilunch
Comida prepagada por hijo + **menú de cupones**: el socio lo crea, el
corporativo lo activa (sin activación nadie lo ve), se canjea en caja con código
`GL-` de 20 minutos. No gasta cupo. *Nunca usado con datos reales.*

## Tiendas hoy (2026-09-14)
| Tienda | Plan |
|---|---|
| Dunia Pizzería | Gold |
| Nikkey Sushi | Premium |
| Mc Dowell's Burgers | Estándar |
| Silver Fish | Prueba |
| American Burguers (`demo`) — **FICTICIA, la tienda con la que venden todos los vendedores** | Estándar |

## Dinero
- **Ningún peso real ha pasado.** Sólo 2 recargas en modo prueba.
- Stripe Connect nunca se completó; **no existe la suscripción recurrente**.
- No decir que el cobro funciona.

## Vendedores (2026-09-17)
- Primera vendedora aprobada: **Monica Pedraza, GL-001** (código de referido 2MV7YP).
- Comisión: 30% de la primera mensualidad y 10% de cada mes siguiente
  (`sales_goals`, editable por mes). Con Gold a $1,800: $540 y $180.
- La venta es del vendedor sólo si el dueño escribe su número al darse de alta.
- La demo (American Burguers) es una sola tienda Gold que usan todos los vendedores.

## Venta con demo (2026-09-18)
- En persona o por videollamada. Laptop del vendedor: Socio (su tienda demo) y Staff. Celular del vendedor: Panel de ventas y Staff. Celular del cliente: /demo con la clave temporal.
- Cada vendedor tiene su tienda "American Burguers · GL-00X". Si pasa una semana sin vender, Giftilove la libera y la asigna a otro (Corporativo → Vendedores).
- El vendedor NO ve Corporativo.
- Clientes potenciales en `prospectos` → base de GHL (fase GHL).
- **GiftiCARD**: tarjeta de UNA tienda, se compra y se cobra sólo ahí. **GiftiMASTERLOVE fuera** por ahora.
