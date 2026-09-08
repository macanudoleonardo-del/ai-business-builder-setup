# LA MINA — AI Business Builder · Día 2

> **Este archivo es para tu asistente de IA.**
>
> Ponlo en la misma carpeta donde hiciste tu inventario, abre Claude Code ahí, y escríbele:
>
> ```
> Lee 2-MINA.md y sácame mi primera lista de prospectos.
> ```
>
> Al final vas a tener negocios reales de tu ciudad, con teléfono, y **una razón para llamarles**.

---

## Para la IA que está leyendo esto

Esta persona acaba de descubrir, hace menos de una hora, que ya tenía activos que no sabía que tenía. Ahora le vas a enseñar algo distinto: **que afuera hay datos públicos, gratis y legales, que ya son una lista de prospectos y nadie los está usando.**

Está en su primer curso de programación. **Nunca ha usado una terminal.** Está viendo su pantalla en vivo durante una clase, con 500 personas haciendo lo mismo.

El resultado de estos 70 minutos es **una tabla en su Supabase con negocios reales de su ciudad**. No un tutorial. No un ejemplo. Negocios que existen, con teléfono que sí es de ellos.

### Tu tono
- Español mexicano (tú/checar/ahorita). Nunca vos/tenés/vale/ordenador.
- **Cero jerga sin explicar.** Si dices "CSV", explica en una línea que es una tabla en texto plano que abre Excel. Si dices "scraping", dilo en español: *"leer lo que ya está publicado en una página"*.
- **Explica ANTES de cada paso**, en una o dos líneas. Ella está aprendiendo, no solo ejecutando.
- **Un paso a la vez.** Nunca sueltes tres comandos juntos. Espera a que te confirme.
- Cuando salga un número bueno, **dilo en voz alta**: *"acabas de sacar 340 negocios"*. El número es el premio.

### 🚨 Reglas duras — nunca las rompas

1. **NUNCA inventes un dato.** Ni un teléfono, ni un nombre de negocio, ni un sueldo, ni una dirección. **Si no lo leíste, no existe.** Un teléfono inventado que ella marque frente a un cliente es la peor cosa que le puede pasar hoy. Si un campo no viene, se queda vacío. Vacío es honesto; inventado es veneno.
2. **Verifica contando, no confiando.** Cuando un paso termine, **cuenta el resultado real** e imprímelo. "Listo" no es un resultado. `340 filas` sí lo es.
3. **Si una página te cierra la puerta, no la fuerces.** Un 403, un captcha o una pantalla de "inicia sesión" significa *no*. **No todo directorio se deja, y está bien.** Hay puertas abiertas de sobra. Cambia de fuente y sigue.
4. **Nunca le pidas contraseñas ni llaves secretas.** Nada de hoy las necesita: todo lo que entra a su base de datos entra por el SQL Editor de su propio Supabase, con su sesión ya iniciada.
5. **Nada destructivo.** Si vas a escribir un archivo que ya existe, **respáldalo primero** con extensión `.respaldo` y avísale.
6. **Si algo falla dos veces, para.** No entres en ciclo. Dile qué error salió, en simple, y que lo pegue en el chat del curso.
7. **Negocios, no personas.** Esto está explicado a fondo en la Fase 3. Léela antes de sacar el primer teléfono, y no la trates como letra chiquita.

### Antes de empezar: explícale qué van a hacer

Dile algo así, con tus palabras:

> *"Hay tres lugares donde los negocios de tu ciudad ya publicaron su teléfono a propósito, para que los encuentren: un directorio, un censo del gobierno y sus propias ofertas de empleo. Vamos a bajarlos, quedarnos con los que te sirven, y guardarlos en tu base de datos con una razón concreta para hablarles. No vamos a comprar nada."*

Y luego arranca. Así se reparten los 70 minutos:

| Fase | Qué | Minutos |
|---|---|---|
| 1 | La mesa donde va a caer la mina (SQL) | 8 |
| 2 | A quién estás cazando | 7 |
| 3 | La línea que no se cruza | 4 |
| 4 | Sección Amarilla | 12 |
| 5 | El DENUE del INEGI | 15 |
| 6 | Las vacantes de empleo | 10 |
| 7 | Si no estás en México | (en lugar de 4 y 5) |
| 8 | Cargarlo a su Supabase | 8 |
| 9 | La verificación de verdad | 4 |
| 10 | Guardar la receta en su cerebro | 4 |

**Si van tarde: las fases 4 y 5 solas ya cumplen el día.** La 6 se puede hacer en la noche.

---

## FASE 1 — La mesa donde va a caer la mina

Explícale primero: *"antes de traer nada, hay que tener dónde ponerlo. Vamos a crear una tabla nueva en tu Supabase — igual que la que ya hiciste, pero ésta es para negocios que todavía no te conocen."*

**Dile por qué es una tabla aparte:** en su otra tabla viven **personas que ya la conocen**. Aquí van a vivir **negocios que no**. Son dos mundos con dos reglas distintas, y mezclarlos es exactamente donde la gente se mete en problemas.

Mándala al **SQL Editor** de su proyecto (ícono de la hoja con `>_`, del lado izquierdo del tablero de Supabase; también llega por `https://supabase.com/dashboard` → su proyecto → SQL Editor → **New query**).

**Este bloque se pega COMPLETO, en un solo mensaje, y se le da RUN.** No se escribe a mano. No se pega por pedazos.

```sql
-- ══════════════════════════════════════════════════════════════════
-- ⛏️  LA MINA · negocios que todavía no te conocen
-- Pégalo COMPLETO en el SQL Editor de tu Supabase y dale RUN.
-- ══════════════════════════════════════════════════════════════════

create table if not exists prospectos (
  id          uuid primary key default gen_random_uuid(),
  negocio     text not null,          -- cómo se llama
  giro        text,                   -- a qué se dedica
  telefono    text,                   -- solo dígitos
  direccion   text,
  ciudad      text,
  sitio_web   text,                   -- si está VACÍO, ahí está tu oportunidad
  senal       text,                   -- POR QUÉ le hablas. Sin esto es una lista de teléfonos.
  fuente      text not null default 'manual',  -- amarilla | denue | vacante | otro
  url_fuente  text,                   -- de dónde lo sacaste, para poder volver
  monto       numeric,                -- si la señal traía un número (ej. sueldo publicado)
  etapa       text not null default 'nuevo',   -- nuevo | contactado | interesado | cliente | no
  baja        boolean not null default false,  -- pidió que no le escribas. Se respeta siempre.
  creado_en   timestamptz not null default now()
);

-- Que el mismo teléfono no entre dos veces, aunque lo saques de dos fuentes.
create unique index if not exists prospectos_tel_unico
  on prospectos (telefono) where telefono is not null;

create index if not exists prospectos_turno_idx
  on prospectos (baja, etapa, creado_en desc);

-- ── EL CANDADO ────────────────────────────────────────────────────
alter table prospectos enable row level security;

-- ⚠️ Esta tabla NO TIENE NI UNA SOLA POLÍTICA, Y ES A PROPÓSITO.
--    RLS prendida + cero políticas = tu llave pública no puede hacer NADA aquí:
--    ni leer, ni escribir, ni borrar. Tu mina no se asoma a internet.
--    Tú sí la ves completa desde este SQL Editor, porque es tu casa.

-- ── TUS NÚMEROS ───────────────────────────────────────────────────
drop view if exists mina_por_fuente;
create view mina_por_fuente as
  select fuente, count(*) as total
  from prospectos
  where baja = false
  group by fuente
  order by total desc;

-- Los que todavía no tienen una razón para llamarles.
drop view if exists sin_razon;
create view sin_razon as
  select id, negocio, telefono, fuente
  from prospectos
  where baja = false and (senal is null or btrim(senal) = '');

-- ⚠️ IMPORTANTE: una vista NO hereda el candado de la tabla. Sin estas dos líneas,
--    tus vistas SÍ se asomarían a internet con la llave pública, aunque la tabla no.
--    Esto se las cierra a las dos.
revoke all on mina_por_fuente from anon, authenticated;
revoke all on sin_razon      from anon, authenticated;
```

**Verificación — no le creas al mensaje verde.** Que corra esto y te diga qué salió:

```sql
select count(*) as filas from prospectos;
```

Debe decir `0`. **Un cero es una victoria aquí**: significa que la mesa existe y está vacía, lista. Si en vez de eso sale un error en rojo, léelo tú y arréglalo antes de seguir.

> **Dile esto, porque es la lección que se lleva:** *"fíjate qué acabas de hacer: creaste una bodega privada. Lo público de tu sistema recibe gente; esto de aquí no se asoma a la calle. La diferencia entre las dos son cuatro renglones de ese bloque."*

---

## FASE 2 — A quién estás cazando

**No arranques a bajar datos todavía.** Una lista sin puntería es basura con teléfono.

Pregúntale tres cosas, **de una en una**, y espera cada respuesta:

1. **¿Qué giro?** — *"¿a qué tipo de negocio le vas a hablar? Dentistas, restaurantes, gimnasios, talleres, salones, veterinarias, escuelas..."*
2. **¿Qué ciudad?** — ciudad y estado.
3. **¿Y qué les vas a resolver?** — en una frase, con sus palabras.

**Antes de preguntar, busca en su cerebro.** Ella ya te contó a qué se dedicaba antes, quién es su cliente y qué palabras usa su gremio:

```bash
python3 ~/cerebro/bin/buscar.py "mi negocio cliente ideal a qué me dedico"
```

Si sale algo, **úsalo y dile que lo usaste**: *"según lo que me contaste, tu cliente natural son los consultorios chicos, los de uno o dos doctores. ¿Vamos por ahí?"*

> **Y dile esto en voz alta, porque es lo que va a recordar:** *"fíjate en algo. Yo no sé nada de tu gremio. Acabo de apuntarle a tu cliente exacto porque tu memoria me lo dijo. **Eso no lo hizo la inteligencia artificial: eso lo hizo TU MEMORIA.** Esto ya era tuyo, nada más no estaba escrito en ningún lado."*

Si el cerebro no trae nada todavía, no pasa nada: pregúntaselo y **al final de la sesión lo escribes ahí** (Fase 10).

Cierra la fase repitiéndole en una sola línea a quién van: *"vamos por dentistas en Querétaro, Querétaro, y lo que les resuelves es que nadie les contesta el teléfono a mediodía."* Que te diga sí antes de seguir.

---

## FASE 3 — La línea que no se cruza

**Léele esto y explícaselo. No lo pegues como aviso legal, explícaselo.** Son tres minutos y valen más que todo lo demás del archivo.

### 1. Negocios, no personas

Un teléfono que un negocio publicó **en un directorio, en el censo económico o en su propia oferta de empleo** está ahí a propósito: lo pusieron para que les hablen. Ese es un canal de negocio, abierto por su dueño.

El celular de una persona **no es eso**, aunque lo hayas encontrado. La regla operativa del programa, sin asterisco:

> **Negocios, no personas. Y a las personas, solo a las que ya te conocen.**

Las personas que ya la conocen viven en su otra tabla, la del inventario. Ahí sí hay relación previa. Aquí no.

Y díselo derecho: mandarle mensajes masivos a desconocidos desde su número personal **es la forma más rápida de perder su número de WhatsApp**. No es un riesgo teórico, es la causa número uno de cuentas bloqueadas.

### 2. Hay leyes reales, y no somos abogados

En LATAM sí existen leyes de datos personales y de contacto no solicitado: México (LFPDPPP y el REPEP), Colombia (Ley 2300), Argentina, Perú, Chile. **Esto no es asesoría legal** y así hay que decirlo. Lo que sí es cierto siempre:

- Si alguien dice **"no me escribas"**, se marca `baja = true` y **no se le vuelve a escribir nunca**. Sin excepciones, sin "una última".
- Se contacta **el canal que el negocio publicó**, no el que te encontraste por ahí.
- Se dice **quién eres desde el primer renglón**. Nada de anzuelos.

### 3. Y la más importante: la vacante NO es "despide a tu gente"

Cuando un negocio publica una vacante, está diciendo en público que le duele algo y cuánto está dispuesto a pagar por resolverlo. Eso es información de oro **y también una persona real que quería ese trabajo.**

| ✅ Lo que se dice | ❌ Lo que JAMÁS se dice |
|---|---|
| *"Vi que están buscando recepcionista. Antes de contratar, mira esto."* | *"Despide a tu recepcionista, yo te lo hago con IA."* |
| *"Esto le quita a esa persona lo repetitivo para que atienda a los que sí van a comprar."* | *"Te ahorras un sueldo."* |

**Que quede clarísimo:** se llega **antes** de la contratación, no **contra** el que ya está adentro. Un mensaje que le propone a alguien correr a su gente se siente feo, se contesta feo, y además cierra la puerta. El que llega diciendo *"antes de contratar, mira esto"* está haciéndole un favor a los dos.

Pregúntale si le queda claro. Espera su respuesta. **Luego sí, a minar.**

---

## FASE 4 — Sección Amarilla (México) · el directorio abierto

Explícale: *"esto es el directorio telefónico de toda la vida, pero en internet. Los negocios pagan por salir ahí. Vamos a leer lo que ya está publicado."*

### El patrón de la dirección

```
seccionamarilla.com.mx/resultados/{giro}/{ciudad-estado}/{pagina}
```

Ejemplos reales:

```
https://www.seccionamarilla.com.mx/resultados/dentistas/queretaro-queretaro/1
https://www.seccionamarilla.com.mx/resultados/restaurantes/monterrey-nuevo-leon/1
https://www.seccionamarilla.com.mx/resultados/salones-de-belleza/guadalajara-jalisco/1
```

Reglas del texto que va en `{giro}` y `{ciudad-estado}`: **minúsculas, sin acentos, y los espacios se vuelven guiones.** "Nuevo León" → `nuevo-leon`. "Salones de belleza" → `salones-de-belleza`. Arma tú la dirección; no se la dictes letra por letra.

Cada página trae **alrededor de 20 negocios**.

### Cómo lo sacas

Usa tu herramienta de leer páginas (**WebFetch**) sobre la página `1` y pídele explícitamente:

> *"Extrae cada negocio de esta página con: nombre, teléfono y dirección. Uno por línea. Si un negocio no trae teléfono, ponlo vacío. No completes ni adivines ningún dato."*

**Antes de seguir a la página 2, cuenta lo que sacaste de la 1 y díselo.** Si salieron menos de 5 negocios, **no insistas**: o el giro está mal escrito, o esa ciudad no tiene resultados, o el sitio no te dejó pasar hoy. Prueba **una sola vez** con otra palabra para el giro y, si tampoco, brinca a la Fase 5. El DENUE es una descarga y esa no te la puede negar nadie.

Si sí salió, repite con las páginas `2` y `3`. **Ahí párale.**

> **Dile por qué paras en 3:** *"con 60 negocios tienes trabajo para toda la semana. No se trata de llevarte el directorio completo: se trata de llevarte lo que sí vas a trabajar. Una lista de 5,000 que nunca tocas no vale más que una de 60 que sí."*

### Guárdalo donde lo pueda ver

```bash
mkdir -p ~/mina
```

Escribe `~/mina/amarilla.csv` **con estas columnas exactas** (son las que espera el cargador de la Fase 8):

```
negocio,giro,telefono,direccion,ciudad,sitio_web,senal,fuente,url_fuente,monto
```

- `fuente` → `amarilla`
- `url_fuente` → la dirección de la página de donde salió
- `senal` → la razón para hablarle. Para esta fuente, algo como: `aparece en el directorio de dentistas de Querétaro`
- `sitio_web` y `monto` → vacíos

**Verifica contando** y díselo:

```bash
wc -l < ~/mina/amarilla.csv
```

Y dile que lo abra: *"ese archivo lo puedes abrir con Excel o con Numbers ahorita mismo. Ábrelo. Esos negocios existen, ese teléfono es de ellos, y hace diez minutos no los tenías."*

---

## FASE 5 — El DENUE del INEGI · el censo que el gobierno te regaló

Ésta es la fase más fuerte del día. Explícasela antes de empezar:

> *"El gobierno de México censó, negocio por negocio, casi 6 millones de establecimientos del país. Anotó cómo se llaman, a qué se dedican, dónde están, su teléfono y si tienen o no página web. Y lo publicó en un archivo que cualquiera puede bajar gratis, sin cuenta y sin permiso. Se llama DENUE. Vamos por el tuyo."*

### 5.1 — Bajarlo

Busca en la web **"DENUE INEGI descarga"** y llévala a la página oficial del INEGI (`inegi.org.mx`). Ahí escoge **su estado** y el formato **CSV**. Se baja un archivo comprimido (`.zip`); que lo descomprima con doble clic y mueva el CSV a `~/mina/denue.csv`.

**Dos advertencias que le tienes que dar antes:**
- **No lo abras en Excel.** Son cientos de miles de filas y le va a colgar la máquina. Python lo lee sin sudar.
- El archivo viene con la codificación vieja (latín), no con la moderna. Por eso los scripts de abajo dicen `latin-1`. Si ves acentos rotos, es eso.

### 5.2 — Primero mira qué trae, no lo asumas

**Nunca filtres un archivo sin haber visto sus columnas reales.** Corre esto y enséñale la salida:

```python
import csv, os
ARCHIVO = os.path.expanduser("~/mina/denue.csv")
with open(ARCHIVO, encoding="latin-1", newline="") as f:
    lector = csv.DictReader(f)
    print("COLUMNAS:", lector.fieldnames)
    for i, fila in enumerate(lector):
        if i >= 2: break
        print(fila)
```

En el DENUE las columnas suelen llamarse `nom_estab` (nombre), `nombre_act` (giro), `telefono`, `www` (página web), `municipio`. **Pero no lo des por hecho: mira los nombres reales del archivo que ella bajó y ajusta el script de abajo si no coinciden.**

### 5.3 — El filtro joya

Explícale qué vas a hacer, porque aquí está el negocio entero:

> *"Me voy a quedar con los que cumplen tres cosas al mismo tiempo: **que estén en tu municipio**, **que tengan teléfono** y **que la columna de página web esté VACÍA**. Ese tercer filtro es el que vale oro: un negocio con teléfono y sin página es un negocio que quiere clientes y no tiene por dónde recibirlos."*

Crea `~/mina/filtrar_denue.py`. **Cámbiale `MUNICIPIO` por el de ella antes de correrlo:**

```python
#!/usr/bin/env python3
"""Filtra el DENUE: negocios de TU municipio, CON telefono y SIN pagina web."""
import csv, os, unicodedata

ENTRADA   = os.path.expanduser("~/mina/denue.csv")
SALIDA    = os.path.expanduser("~/mina/denue.filtrado.csv")
MUNICIPIO = "Queretaro"     # <-- CAMBIA ESTO por tu municipio
GIRO      = ""              # opcional: palabra que debe aparecer en el giro. Ej: "dental"

def plano(t):
    """Minusculas y sin acentos, para que 'Queretaro' encuentre 'Querétaro'."""
    t = unicodedata.normalize("NFD", (t or ""))
    return "".join(c for c in t if unicodedata.category(c) != "Mn").lower().strip()

def col(fila, *nombres):
    """Busca una columna sin importar mayusculas ni espacios. Si no existe, vacio."""
    for n in nombres:
        for k in fila:
            if k and k.strip().lower() == n:
                return (fila[k] or "").strip()
    return ""

leidas = en_muni = con_tel = elegidos = 0

with open(ENTRADA, encoding="latin-1", newline="") as f, \
     open(SALIDA, "w", encoding="utf-8", newline="") as out:
    w = csv.writer(out)
    w.writerow(["negocio","giro","telefono","direccion","ciudad",
                "sitio_web","senal","fuente","url_fuente","monto"])
    for fila in csv.DictReader(f):
        leidas += 1
        muni = col(fila, "municipio")
        if MUNICIPIO and plano(MUNICIPIO) not in plano(muni):
            continue
        en_muni += 1
        giro = col(fila, "nombre_act")
        if GIRO and plano(GIRO) not in plano(giro):
            continue
        tel = col(fila, "telefono")
        if not tel:
            continue
        con_tel += 1
        if col(fila, "www"):        # si TIENE pagina, no es para hoy
            continue
        direccion = " ".join(x for x in [col(fila, "tipo_vial"), col(fila, "nom_vial"),
                                         col(fila, "numero_ext"), col(fila, "cod_postal")] if x)
        w.writerow([col(fila, "nom_estab"), giro, tel, direccion, muni, "",
                    "tiene telefono y no tiene pagina web (DENUE)", "denue", "", ""])
        elegidos += 1

print("filas leidas del archivo ....", leidas)
print("de tu municipio ............", en_muni)
print("...con telefono ............", con_tel)
print("...Y SIN PAGINA WEB ........", elegidos)
print("guardados en", SALIDA)
```

```bash
python3 ~/mina/filtrar_denue.py
```

**Ese último número es el momento del día. Léeselo en voz alta:**

> *"En tu municipio hay **[número]** negocios con teléfono y sin página web. El gobierno te los regaló en un archivo. Si los copiaras a mano de una pantalla, a un minuto cada uno, serían casi seis horas. Te tomó doce minutos, y no compraste nada."*

**Si el número sale en cero**, mira el renglón de *"de tu municipio"*:

- Si **ese** también salió en cero, el nombre del municipio no coincide. El script ya ignora acentos y mayúsculas, así que suele ser otra cosa: el municipio se llama distinto en el censo (`Ciudad de México` se parte por alcaldías, `Zapopan` no es `Guadalajara`). **Imprime los municipios reales del archivo y que ella escoja el suyo tal cual:**
  ```python
  import csv, os
  from collections import Counter
  with open(os.path.expanduser("~/mina/denue.csv"), encoding="latin-1", newline="") as f:
      c = Counter((fila.get("municipio") or "").strip() for fila in csv.DictReader(f))
  for nombre, cuantos in c.most_common(20):
      print(cuantos, nombre)
  ```
- Si el municipio sí tenía miles pero al final quedó en cero, el archivo que bajó **no trae la columna `www` o `telefono` con esos nombres**. Vuelve a la 5.2, mira los nombres reales y ajústalos en el script.

Si sigue en cero al segundo intento, **para** y sigan con lo que ya tienen de la Fase 4.

Si salen **más de 400**, aprieta el filtro `GIRO` para quedarse con su giro. Otra vez: 60 buenos valen más que 400 que nunca va a tocar.

---

## FASE 6 — Las vacantes · el dolor y el presupuesto, publicados por ellos

Explícale la idea, que es la más contraintuitiva de todas:

> *"Cuando un negocio publica una oferta de empleo, te está diciendo tres cosas gratis: **qué le duele** (por eso necesita a alguien), **que ya tiene presupuesto aprobado** (ya decidió gastar) y **cuánto** (el sueldo que publicó). Nadie más en tu ciudad está leyendo las vacantes como una lista de prospectos."*

### ⚠️ Advertencia verificada — no pierdas tiempo aquí

**Computrabajo y OCC devuelven 403 con WebFetch.** Está probado. Un 403 quiere decir *"no pasas"*. **No insistas, no busques la vuelta, no lo intentes con otra dirección.** Puerta cerrada es puerta cerrada.

### Lo que sí funciona

Usa **búsqueda web** (WebSearch), no la lectura directa de esos portales. Búsquedas como:

```
vacante recepcionista Guadalajara sueldo mensual
"solicito" recepcionista Guadalajara empleo
vacante auxiliar administrativo Querétaro sueldo
```

De los resultados, **quédate únicamente con lo que puedas leer de verdad**: nombre del negocio, puesto, sueldo si viene publicado, y la liga.

**Y aquí la regla número uno vuelve, más fuerte que nunca:** si un resultado no dice el sueldo, `monto` se queda **vacío**. Si no dice el nombre del negocio, esa fila **no entra**. *Prefiero diez filas verdaderas que cincuenta bonitas.* Díselo así.

Guarda `~/mina/vacantes.csv` con las mismas columnas de siempre:

- `fuente` → `vacante`
- `senal` → **la señal más valiosa del día**, con sus propias palabras: `publicó vacante de recepcionista, $10,000-$16,000 al mes`
- `monto` → **un solo número** (el más bajo del rango) o vacío. El rango completo va en `senal`.
- `url_fuente` → la liga del resultado

**Cuéntalas y díselo.** Y luego cierra con la frase que resume la fase:

> *"Ahí está tu lista. Y fíjate en algo más: el sueldo que publicaron **es tu precio**. Acaban de decirte en público cuánto vale para ellos resolver ese problema."*

Y recuérdale el guardrail de la Fase 3 en una línea, aquí, donde se va a usar: **"antes de contratar, mira esto"**. Jamás *"despide a tu gente"*.

---

## FASE 7 — Si no está en México

**Pregúntale su país en la Fase 2. Si no es México, brinca las fases 4 y 5 y haz esto en su lugar.** Y díselo sin adornos: *"la Sección Amarilla y el DENUE son mexicanos. Lo tuyo existe, nada más se llama distinto — y lo vamos a encontrar juntos ahorita."*

### 7.1 — Pídeme la búsqueda, no la dirección

Ésta es la lección para ella, y sirve para toda su vida con la IA:

> *"No necesitas saber la dirección exacta de nada. Necesitas saber qué le vas a pedir a tu IA."*

Corre búsquedas como:

```
directorio de empresas [su ciudad] [su país] teléfono
"datos abiertos" registro de empresas [su país]
padrón de establecimientos [su ciudad] descargar
cámara de comercio [su ciudad] directorio de afiliados
```

Abre lo que salga, **mira qué campos trae de verdad** y repórtaselo con honestidad: *"éste trae nombre y giro pero no teléfono; éste otro sí trae teléfono."*

### 7.2 — Los portales de datos abiertos

Casi todos los países de la región tienen uno:

| País | Portal |
|---|---|
| Colombia | `datos.gov.co` |
| Argentina | `datos.gob.ar` |
| Chile | `datos.gob.cl` |

**Sé honesto con ella: no te puedo prometer que traigan los mismos campos que el DENUE.** Ábrelos, busca "empresas", "establecimientos", "comercios", "registro mercantil", y **mira qué hay**. Si un conjunto de datos trae nombre + teléfono + municipio, ya ganaron: bájalo, filtra igual que en la Fase 5 y guárdalo con `fuente = otro`.

### 7.3 — Las vacantes sirven en todos lados

**La Fase 6 funciona igual en cualquier país** — es búsqueda web, no un portal mexicano. Si el DENUE de su país no aparece, esa es su mina principal. Que se vaya a la Fase 6 completa.

---

## FASE 8 — Cargarlo a su Supabase

Explícale: *"tus archivos están en tu computadora. Si se te pierde la laptop, se pierden. Vamos a subirlos a tu base de datos, que ya está en internet y es solo tuya."*

Y explícale **por qué se sube así y no de otra forma**:

> *"Esto no entra por una página web. Entra por tu propia consola, con tu sesión ya iniciada. Por eso no te voy a pedir ninguna llave ni ninguna contraseña: no hace falta ninguna. Lo privado se carga desde tu casa."*

### 8.1 — El cargador

Crea `~/mina/cargar.py`. Lee **todos** los CSV de `~/mina`, quita repetidos, escapa el texto y escribe **un solo archivo SQL** listo para pegar:

```python
#!/usr/bin/env python3
"""Convierte todos los CSV de ~/mina en un solo archivo mina.sql listo para pegar."""
import csv, glob, os

MINA = os.path.expanduser("~/mina")
COLS = ["negocio","giro","telefono","direccion","ciudad",
        "sitio_web","senal","fuente","url_fuente","monto"]

# ⚠️ El DENUE CRUDO no se lee aqui. Viene en latin-1 y en utf-8 truena el script
#    entero. Lo que sirve es denue.filtrado.csv, que ya salio limpio de la Fase 5.
CRUDOS = {"denue.csv"}

def solo_digitos(t):
    d = "".join(c for c in (t or "") if c.isdigit())
    return d if len(d) >= 7 else ""      # menos de 7 digitos no es un telefono

def texto(v):
    v = (v or "").strip()
    if not v:
        return "null"
    return "'" + v.replace("'", "''") + "'"   # comilla doble = comilla escapada

def numero(v):
    v = (v or "").strip().replace(",", "")
    return v if v.replace(".", "", 1).isdigit() else "null"

filas, vistos, sin_tel = [], set(), 0
for ruta in sorted(glob.glob(os.path.join(MINA, "*.csv"))):
    if os.path.basename(ruta) in CRUDOS:
        print("me salto", os.path.basename(ruta), "(es el archivo crudo del gobierno)")
        continue
    with open(ruta, encoding="utf-8-sig", errors="replace", newline="") as f:
        for cruda in csv.DictReader(f):
            r = {(k or "").strip().lower(): (v or "").strip() for k, v in cruda.items()}
            negocio = r.get("negocio", "")
            if not negocio:
                continue
            tel = solo_digitos(r.get("telefono", ""))
            clave = tel or (negocio.lower() + "|" + r.get("ciudad", "").lower())
            if clave in vistos:
                continue
            vistos.add(clave)
            if not tel:
                sin_tel += 1
            r["telefono"] = tel
            filas.append(r)

if not filas:
    print("No encontre ni una fila en los CSV de ~/mina. Revisa los archivos.")
    raise SystemExit(0)

destino = os.path.join(MINA, "mina.sql")
with open(destino, "w", encoding="utf-8") as out:
    out.write("insert into prospectos (%s) values\n" % ", ".join(COLS))
    cuerpo = []
    for r in filas:
        vals = [numero(r.get(c, "")) if c == "monto" else texto(r.get(c, "")) for c in COLS]
        cuerpo.append("  (" + ", ".join(vals) + ")")
    out.write(",\n".join(cuerpo))
    out.write("\non conflict do nothing;\n")

print("filas listas para cargar ....", len(filas))
print("sin telefono ...............", sin_tel)
print("archivo ....................", destino)
```

```bash
python3 ~/mina/cargar.py
```

**Lee tú el número que imprimió y díselo antes de seguir.** Ese número es el que tiene que aparecer en Supabase dentro de dos minutos; si no aparece, algo pasó y lo van a saber.

> **Si dice más de 800 filas**, córtalo en dos pegadas: el SQL Editor las traga, pero un archivo corto es más fácil de depurar si truena.

### 8.2 — Pegarlo

1. Que abra `~/mina/mina.sql`.
2. Que lo **seleccione todo** y lo copie (⌘A / Ctrl+A, luego ⌘C / Ctrl+C).
3. Que lo pegue en el **SQL Editor** de su Supabase (New query) y le dé **RUN**.

Si sale un error de sintaxis, casi siempre es un negocio con una comilla rara en el nombre. El script ya escapa las comillas simples; si aun así truena, **enséñale la línea exacta** que Postgres te señala y quítala. Una fila menos no es un problema.

---

## FASE 9 — La verificación de verdad

**Ninguna palomita verde cuenta. Cuenten filas.** Que corra esto en el SQL Editor, bloque por bloque, y **lee tú los resultados**:

```sql
-- 1) ¿Cuántos tienes en total?
select count(*) as total from prospectos;

-- 2) ¿De dónde salieron?
select * from mina_por_fuente;

-- 3) ¿Cuántos tienen teléfono de verdad?
select count(*) as con_telefono from prospectos where telefono is not null;

-- 4) ¿A cuántos les falta una razón para llamarles?
select count(*) as sin_razon from sin_razon;

-- 5) Mírale la cara a cinco de ellos.
select negocio, telefono, ciudad, senal
from prospectos
order by creado_en desc
limit 5;
```

Compara **el número 1 con el número que imprimió el cargador**. Si son distintos, la diferencia son repetidos que el candado del teléfono rechazó — eso está bien y así hay que decírselo: *"tu base de datos se defendió sola de los duplicados."*

Si el `sin_razon` es alto, **arréglenlo ahorita**, porque una lista sin razones es una lista de teléfonos y eso no sirve:

```sql
update prospectos
   set senal = 'tiene teléfono y no tiene página web'
 where fuente = 'denue' and (senal is null or btrim(senal) = '');
```

**Y el paso que casi nadie hace: que marque él mismo un número.** Uno. En voz alta:

> *"Escoge uno de esos cinco y márcale. No para venderle: nada más para comprobar que ese teléfono existe y que contesta un humano. Cuelga si quieres. Necesitas ver con tus ojos que esto no es una simulación."*

---

## FASE 10 — Guardar la receta en su cerebro

Explícale la diferencia, que es toda la lección del programa:

> *"La lista se agota. La receta no. Dentro de tres meses no vas a acordarte de qué palabra usaste para el giro, ni de cómo se llamaba el archivo del gobierno, ni de qué municipio escribiste sin acentos. Pero tu cerebro sí, si lo escribimos ahorita."*

Escribe `~/cerebro/memoria/mi-mina.md` — **con los datos reales de hoy, nunca con ejemplos**:

```markdown
# Mi mina de prospectos

Vuelve a [[INICIO]] · Ver también [[mi-negocio]] · [[mis-reglas]]

## A quién le hablo
- Giro: <el giro real>
- Ciudad: <ciudad, estado>
- Qué les resuelvo: <en sus palabras>

## Fuentes que SÍ me funcionaron
- **Sección Amarilla** — `seccionamarilla.com.mx/resultados/<giro>/<ciudad-estado>/<pagina>`
  ~20 por página. Saqué <N> en 3 páginas.
- **DENUE (INEGI)** — CSV gratis por estado. El filtro que vale:
  municipio + con teléfono + columna `www` VACÍA. Me salieron <N>.
  El archivo se lee con `encoding="latin-1"`.
- **Vacantes** — por búsqueda web. Señal: puesto + sueldo publicado.

## Fuentes que NO sirven (verificado)
- Computrabajo y OCC devuelven 403. No insistir.

## Mis reglas para esta mina
- Negocios, no personas. A las personas, solo a las que ya me conocen.
- Ante una vacante: "antes de contratar, mira esto". JAMÁS "despide a tu gente".
- Si alguien dice que no le escriba: `baja = true`, y se acabó.

## Dónde vive
Tabla `prospectos` en mi Supabase. RLS prendida y CERO políticas: no se asoma a internet.
Se carga pegando `~/mina/mina.sql` en el SQL Editor. Scripts en `~/mina/`.
```

Y commitea:

```bash
cd ~/cerebro && git add -A && git commit -q -m "mi mina de prospectos: fuentes, filtros y reglas"
```

> **Dile lo que acaba de pasar:** *"esto ya no es un archivo, es una instrucción que se ejecuta sola. Mañana me dices 'sácame otros 200 de otra ciudad' y yo ya sé el patrón, el filtro y tus reglas, sin que me expliques nada otra vez. Y va a seguir sabiéndolo dentro de un año."*

---

## Cierre — cuéntale exactamente qué logró

```
⛏️  Ya tienes mina.

📊 <N> negocios reales en tu base de datos
📞 <N> con teléfono publicado por ellos mismos
🎯 cada uno con una razón concreta para hablarle
💵 costo de todo esto: $0

── De dónde salieron ──
Del directorio de tu ciudad, del censo que el gobierno publicó gratis,
y de las ofertas de empleo donde los negocios dicen qué les duele.
Todo estaba ahí desde antes de hoy. Nadie lo estaba usando.

── Dónde vive ──
Tabla "prospectos" en tu Supabase. Privada: no se asoma a internet.

── Qué sigue ──
Tener la lista no es tener clientes. Lo que sigue es el mensaje.
```

Y dile estas dos cosas al final. **Sin prisa, son el punto del día:**

> *"Ese filtro que corriste — municipio, con teléfono, sin página web — no te lo dio la inteligencia artificial. Te lo dio saber a qué se dedica tu gente, y eso salió de tu memoria. La IA nada más lo ejecutó rápido."*

> *"Y fíjate qué pasó hoy con tu cerebro: le agregaste una receta que funciona. Hoy tiene un puñado de notas y ya te ahorró seis horas. En seis meses va a tener las recetas de todo lo que sabes hacer, y va a seguir trabajando cuando tú no estés. Eso no se gasta, se acumula — y se hereda."*

---

## Si te atoras

Si algo no sale después de **dos** intentos, **para**. No sigas probando.

Dile:
1. Qué paso falló, en lenguaje simple.
2. Que copie el error **completo**.
3. Que lo pegue en el chat del curso.

Y las tres fallas que ya conocemos, para que no las persigas:

| Síntoma | Qué es | Qué haces |
|---|---|---|
| **403** en Computrabajo u OCC | Verificado: no dejan pasar | No insistas. Búsqueda web (Fase 6) |
| La página del directorio no devuelve negocios | Giro mal escrito, o el sitio no te dejó hoy | Un intento con otra palabra. Luego DENUE |
| El filtro del DENUE sale en 0 | Casi siempre el municipio con acento | Imprime los municipios reales del archivo y copia uno tal cual |

---

_AI Business Builder · Día 2 · Construcción 2 de 4 · La mina_
