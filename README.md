# AI Business Builder — las guías del curso

Dos archivos, en orden. Cada uno se le entrega a Claude Code y él ejecuta.

### Día 1

| # | Archivo | Qué hace | Estado |
|---|---|---|---|
| 1 | **`SETUP.md`** | Deja tu computadora lista: git · Python 3 · Claude Code · las 4 cuentas (GitHub · Supabase · Vercel · Claude) | ✅ 2026-09-07 |
| 2 | **`CEREBRO.md`** | Le construye **memoria permanente** a tu IA en `~/cerebro/`: cuatro cajones, dos hooks (arranque y guardado), un buscador, y la red de notas en Obsidian | ✅ 2026-09-08 |

### Día 2

| # | Archivo | Qué hace | Estado |
|---|---|---|---|
| 3 | **`1-INVENTARIO.md`** | No construye nada: **cuenta** lo que ya tienes. Tu agenda, tu familia, tu gremio y los negocios donde ya eres cliente. 50 minutos, y sale un número | ⬅️ sigue |

## Cómo se usa

1. Clona o descarga este repo en tu computadora.
2. Abre **Claude Code** parado en esa carpeta.
3. Escríbele la línea que toca:

```
Lee SETUP.md y ayúdame a dejar mi computadora lista.
```
```
Lee CEREBRO.md y constrúyeme mi cerebro.
```
```
Lee 1-INVENTARIO.md y cuéntame lo que ya tengo.
```

Él se encarga del resto. Tú solo contestas lo que te pregunte.

## Lo que quedó listo en la máquina (2026-09-07)

`git 2.51.0` · `Python 3.14.7` · `Claude Code 2.1.263` — más las cuatro cuentas.
Con eso, `CEREBRO.md` ya tiene todo lo que necesita para correr.

Dos tropiezos que vale la pena recordar, porque se van a repetir: la Microsoft Store se
atoró una hora en *"Checking system requirements"* (la solución fue bajar Python de
python.org), y tanto Python como Claude Code quedaron instalados **sin** estar en el PATH —
o sea, existían pero Windows no los encontraba. Las dos veces el síntoma fue el mismo
mensaje: *"is not recognized as the name of a cmdlet"*.

## ⚠️ Tiene que correr en TU máquina

No sirve correrlo en la nube. El setup instala herramientas y configura rutas **en la
computadora donde vas a trabajar**; un contenedor en la nube se borra al cerrar la sesión y
se lleva todo. Si abriste Claude Code desde el navegador, este archivo no te va a servir
hasta que lo bajes a tu equipo.

## Lo que la IA NO va a hacer (y está bien)

Está escrito en el propio `SETUP.md`, y es la parte que te protege:

- No te va a pedir **ninguna contraseña**. Las escribes tú, directo en cada sitio web.
- No va a **crear cuentas** por ti. Te guía y verifica; tú ejecutas.
- No va a tocar **datos de tarjeta**. Ese paso lo haces tú en el sitio.
- No te va a pedir que le **pegues una API key** en el chat.
- No instala nada **sin preguntarte primero**.
- Si algo falla dos veces, **se detiene** en vez de seguir probando a ciegas.

## Este repo es independiente

No tiene nada que ver con los otros proyectos (Amor Consciente / saludmetabolico.com,
Glass Experts Atlanta, Emma's Play World). Es material del curso y vive aparte, a propósito.

## Una regla del día 2 que vale la pena leer antes

`1-INVENTARIO.md` toca datos de personas: tu agenda exportada y los negocios de tu banco.
La guía misma pone los límites, y son buenos:

- **El archivo de contactos nunca sale de tu computadora.** Se lee local, con Python, sin
  tocar internet ni una vez. Vive en `~/mi-mina/datos/`, detrás de un `.gitignore` que se
  pone *antes* de que exista el riesgo.
- **Nada de PDFs ni capturas del banco.** Ni cuentas, ni tarjetas, ni saldos. Abres tu app
  y dictas **nombres de negocios**. Nada más.
- **Personas y negocios no se mezclan:** `~/mi-mina/` (gente, nunca sale) es una carpeta
  distinta de `~/mina/` (datos públicos de negocios).
- Al cerebro suben **los números y el conocimiento** — cuántos son, cómo habla tu gremio —
  **nunca los nombres y teléfonos**.
