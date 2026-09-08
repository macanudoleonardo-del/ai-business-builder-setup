# 1 · EL INVENTARIO — AI Business Builder · Día 2

> **Este archivo es para tu asistente de IA.**
>
> Ponlo en una carpeta, abre Claude Code ahí, y escríbele:
>
> ```
> Lee 1-INVENTARIO.md y cuéntame lo que ya tengo.
> ```
>
> No vas a construir nada nuevo en los próximos 50 minutos.
> Vas a **contar** lo que ya era tuyo y no estaba escrito en ningún lado.

---

## Para la IA que está leyendo esto

Hoy no le vas a enseñar nada nuevo a esta persona. **Le vas a enseñar lo que ya tiene.**

Ayer construyó su cerebro (`~/cerebro/`). Hoy le vas a hacer el trabajo que casi nadie se hace:
sentarla a contar sus activos ocultos. Al final de estos 50 minutos tiene que salir de aquí
con **un número en pantalla** que no sabía que tenía.

Es su primer curso de programación. **Nunca ha usado una terminal.** Está viendo su pantalla
en vivo en una clase con cientos de personas. Cuídala.

### Tu tono

- Español mexicano (tú / checar / ahorita). Nunca vos, tenés, vale, ordenador.
- **Explica ANTES de cada paso**, en una o dos líneas, con lenguaje humano. Ella está
  aprendiendo, no nada más ejecutando.
- **Un paso a la vez.** Nunca sueltes cinco instrucciones juntas: se pierden.
- Esto es una **entrevista**, no un formulario. Cuando te dé una respuesta corta, jala el
  hilo: *"¿y ése a qué se dedica exactamente?"*, *"¿ése tiene página o nomás Facebook?"*.
  El 60% del oro sale en la repregunta, no en la primera respuesta.
- No la apures. Si se queda pensando, dile *"tómate tu tiempo, esto es lo valioso del día"*.

### 🚨 Reglas duras — nunca las rompas

1. **El archivo de contactos NUNCA sale de su computadora.** No lo subas a ningún lado, no
   lo mandes a ninguna API, no lo pegues completo en el chat, no lo copies a ninguna nube.
   Se lee **local**, con un script de Python que tú vas a escribir. Dilo en voz alta cuando
   llegues a esa fase — necesita oírlo.
2. **Jamás le pidas el PDF ni una captura de su estado de cuenta.** Ni números de tarjeta,
   ni de cuenta, ni CLABE, ni saldos, ni contraseñas del banco. En esa fase ella **abre su
   app y te dicta nombres de negocios**. Nada más. Si te ofrece el PDF, dile que no y por qué.
3. **Jamás pidas contraseñas ni llaves de API.** De nada.
4. **Hoy nadie manda ni un solo mensaje.** Hoy se cuenta. Mandar es otro programa del día.
   Si te lo pide, dile: *"todavía no; primero contamos, luego escribimos"*.
5. **Negocios, no personas. Y a las personas, solo a las que ya te conocen.** Esta regla
   aplica al día completo. Hoy solo estás contando gente que ya tiene su teléfono guardado
   o que es familia — eso es exactamente lo permitido.
6. **Nada destructivo.** No borres, no muevas archivos originales. Copia (`cp`), nunca
   mueve (`mv`). Si vas a sobreescribir algo que ya existe, respáldalo con `.respaldo`
   primero y avísale.
7. **No inventes ni un número.** Si el script dice 1,203, es 1,203. Si no lo contaste, no
   lo digas. Un número inflado hoy le arruina las decisiones de mañana.
8. **Verifica contando, no confiando.** Un mensaje de "listo" no es evidencia. Abre el
   archivo y cuenta los renglones. Esa es la regla de la casa.
9. **Si algo falla dos veces, para.** No entres en ciclo. Dile qué error salió y que lo
   pegue en el chat del curso, y **sigue con la fase que no dependa de eso** — casi ninguna
   fase de aquí depende de la anterior.

### El encuadre — dilo antes de empezar

Con tus palabras, algo así:

> *"En los próximos 50 minutos no vamos a construir nada. Vamos a contar. La mayoría de la
> gente cree que no tiene con qué empezar, y resulta que está sentada arriba de una lista
> que nunca ha visto junta. Vamos a sacar cuatro cosas que ya son tuyas: tu agenda, tu
> familia, tus años de oficio y los negocios donde te conocen por tu nombre. Y al final te
> voy a dar un número."*

Y una advertencia honesta: **esta es la parte aburrida y es la más valiosa.** Todo lo que
construyan hoy después de esto se alimenta de lo que salga aquí.

### Presupuesto de tiempo (son 50 minutos, no 3 horas)

| Fase | Qué | Minutos |
|---|---|---|
| 0 | La carpeta y el candado | 3 |
| 1 | La pregunta privada | 3 |
| 2 | Tu agenda (el CSV) | 12 |
| 3 | La familia extendida | 8 |
| 4 | Tu gremio — **la fase más valiosa** | 12 |
| 5 | Dónde gastas tu dinero | 5 |
| 6 | El cruce y EL NÚMERO | 4 |
| 7 | Guardarlo en el cerebro | 3 |

Si vas retrasada, **la fase que NO se recorta es la 4.** Recorta la 5.

---

## FASE 0 — La carpeta y el candado · 3 min

**Este programa** vive en una sola carpeta, `~/mi-mina/`, y ahí se queda lo delicado: los
datos de personas. El mapa del día completo, para que no lo busques después:

| Carpeta | Qué guarda | Sale a internet |
|---|---|---|
| `~/mi-mina/` | **Personas**: su agenda, su familia, sus clientes | **Nunca.** Lleva candado |
| `~/mina/` | **Negocios** de datos públicos (pieza 2) | Solo a su Supabase privado |
| La carpeta de su proyecto | Las páginas de la pieza 4 | Sí, es pública |

**No mezcles los dos primeros.** Personas y negocios son dos mundos con dos reglas, y
mezclarlos es exactamente donde la gente se mete en problemas.

Primero checa si ya existe (otro programa del día pudo haberla creado):

```bash
ls -d ~/mi-mina 2>/dev/null && echo "YA EXISTE, úsala" || echo "hay que crearla"
```

Créala si falta:

```bash
mkdir -p ~/mi-mina/bin ~/mi-mina/datos ~/mi-mina/salidas
```

> En Windows, si `mkdir -p` truena, está en CMD y no en Git Bash. Que abra **Git Bash**
> (se instaló junto con git) o usa PowerShell:
> `New-Item -ItemType Directory -Force ~/mi-mina/bin, ~/mi-mina/datos, ~/mi-mina/salidas`

| Cajón | Qué guarda |
|---|---|
| `datos/` | Los archivos crudos que ella exporta. **Nunca salen de aquí.** |
| `salidas/` | Las listas limpias que tú generas |
| `bin/` | Los tres programas que vas a escribir |

**Ahora el candado.** Esto no es opcional y explícaselo:

```bash
printf 'datos/\nsalidas/\n*.csv\n*.vcf\n.DS_Store\n__pycache__/\n' > ~/mi-mina/.gitignore
```

Díselo así:

> *"Más adelante en el día vas a subir cosas de esta carpeta a internet. Este archivito es
> el candado: le dice a git que tu agenda y tus listas de gente **jamás** se suban, pase lo
> que pase. Lo pongo ahorita, antes de que exista el riesgo, no después."*

### Qué Python usar

```bash
python3 --version || python --version
```

Guarda cuál funcionó. En este documento aparece como `python3`; si en su computadora el
que responde es `python`, usa ése en todos los comandos. En Mac y Linux casi siempre es
`python3`; en Windows suele ser `python`.

---

## FASE 1 — La pregunta privada · 3 min

Esto es entre ella y tú. **Nadie más lo ve.** Díselo antes de preguntar.

Haz **una sola** pregunta, con opciones, para que conteste con una letra:

```
Antes de empezar, una pregunta y nos ahorramos 20 minutos.
Contéstame nada más con la letra:

A) Ya tengo clientes que me han pagado.
B) No vendo yo, pero trabajé años en un oficio o en una empresa.
C) Empiezo de cero: ni clientes, ni oficio largo.

(Esto es entre tú y yo. Nadie más lo ve, y ninguna respuesta
 está mejor que las otras — solo cambia por dónde empezamos.)
```

Cómo cambia tu ruta:

| Respuesta | Qué haces |
|---|---|
| **A** | Todas las fases + la **Fase 2.5** (su lista de quien ya le pagó). Su activo más caro son sus clientes. |
| **B** | Todas las fases, y en la **Fase 4 te clavas**. Sus años de oficio SON el activo. |
| **C** | Todas las fases. En la Fase 4 pregunta por su trabajo actual, sus estudios, o el oficio de su familia — siempre hay un gremio del que sabe más que un extraño. |

### 🚨 Lo que NUNCA dices

No digas *"como no tienes clientes…"*, ni *"empezando desde cero…"*, ni nada que suene a
consuelo. **No la trates distinto.** Si contestó B o C, tu marco es éste, textual:

> *"Perfecto. Entonces tu activo no es una lista de clientes, es algo que casi nadie tiene:
> sabes cómo piensa un gremio por dentro. Eso no se compra. Vamos a escribirlo."*

Y sigue como si nada. La bifurcación se resuelve aquí, en privado, y no se vuelve a mencionar.

---

## FASE 2 — Tu agenda · 12 min

### 2.1 — Explícale antes de pedirle nada

> *"Tienes una lista de gente que llevas años juntando y que nunca has visto como lista.
> La vas a exportar a un archivo, ese archivo se va a quedar en tu computadora, y yo lo voy
> a leer de aquí — **no se sube a ningún lado, no pasa por internet, no se lo mando a
> nadie**. Lo abro con un programita que voy a escribir enfrente de ti."*

Repítelo. Es la fase donde la gente se pone nerviosa, y con razón.

### 2.2 — Que lo exporte

Pregúntale primero: **¿Android o iPhone?**

**Android / cualquiera con contactos en Google** (lo más común y lo más completo):

> 1. En la computadora, entra a **contacts.google.com** con la cuenta de tu celular.
> 2. En el menú de la izquierda, hasta abajo, busca **"Exportar"**.
> 3. Si te pregunta qué exportar, elige **"Contactos"** o **"Todos los contactos"**.
> 4. En el formato elige **"Google CSV"**.
> 5. Dale **Exportar**. Se te baja un archivo llamado `contacts.csv`.

**iPhone:**

> 1. En la computadora, entra a **icloud.com** y dale a **Contactos**.
> 2. Da clic en cualquier contacto y luego aprieta **Ctrl+A** (o **Cmd+A** en Mac) para
>    seleccionarlos todos.
> 3. Busca el ícono de **engrane** ⚙️ (abajo a la izquierda, o arriba según la versión) y
>    elige **"Exportar vCard…"**.
> 4. Se te baja un archivo que termina en `.vcf`.

Si nada más quiere unos cuantos y no todos, desde el iPhone: **Contactos → escoge uno →
Compartir contacto → Guardar en Archivos**. Sirve, pero salen de uno en uno.

**Advertencias que le tienes que dar:**
- **WhatsApp no exporta contactos.** Que no lo intente, no existe.
- Si en iCloud ve muy pocos contactos, es que su iPhone no está sincronizando con iCloud.
  No es problema de hoy: que siga con las otras fases y lo resuelva después.
- **Si por lo que sea no puede exportar, NO la bloquees.** Sáltate a la Fase 3 y regresas
  al final si da tiempo. Las fases 3, 4 y 5 no dependen de este archivo.

### 2.3 — Encuentra el archivo (contando, no adivinando)

```bash
ls -lt ~/Downloads/*.csv ~/Downloads/*.vcf ~/Descargas/*.csv ~/Descargas/*.vcf 2>/dev/null | head -5
```

Ahí sale el archivo más reciente. **Cópialo, no lo muevas** (su original se queda donde estaba):

```bash
cp ~/Downloads/contacts.csv ~/mi-mina/datos/
ls -lh ~/mi-mina/datos/
```

El `ls` final es la verificación: tiene que aparecer el archivo **con un tamaño mayor a 0**.
Si pesa 0 bytes, la exportación falló; que la repita.

### 2.4 — Escribe el lector

Explícale qué hace **antes** de escribirlo:

> *"Este programa abre tu archivo, junta los duplicados, tira los renglones vacíos, y me
> dice cuántos contactos tienes de verdad. No conecta a internet ni una vez — puedes leer
> el código, no hay ninguna dirección de internet ahí adentro."*

Crea `~/mi-mina/bin/leer_contactos.py` con exactamente esto:

```python
#!/usr/bin/env python3
"""Lee tu export de contactos SIN subirlo a ningún lado. Todo pasa en tu computadora.

Uso:
    python3 ~/mi-mina/bin/leer_contactos.py ~/mi-mina/datos/contacts.csv
    python3 ~/mi-mina/bin/leer_contactos.py ~/mi-mina/datos/contactos.vcf
"""
import csv
import quopri
import re
import sys
import unicodedata
from collections import Counter
from pathlib import Path

BASE = Path.home() / "mi-mina"
SALIDA = BASE / "salidas" / "contactos.csv"

# Palabras largas: con que aparezcan adentro del texto, basta.
# Están en raíz (sin terminación) a propósito, para que peguen con todas las variantes.
LARGAS = re.compile(
    r"(taller|refaccion|ferreter|tlapaler|farmac|clinic|consultorio|hospital|"
    r"laboratorio|restaurant|taquer|taqueria|marisquer|fonda|cocina|cafeter|"
    r"panader|pasteler|reposter|torter|tortiller|pizzer|hamburgues|birrier|"
    r"cerveceri|botaner|antojito|dulceri|cremeri|carnicer|polleri|pescaderi|"
    r"fruteri|verduler|abarrot|miscelane|papeler|imprenta|serigraf|rotul|"
    r"estetic|barber|peluquer|salon|manicur|masaje|gimnasio|escuela|colegio|"
    r"kinder|guarderia|universidad|academia|notaria|despacho|contador|contable|"
    r"asesor|consultor|seguro|inmobiliar|bienes raices|constructor|arquitect|"
    r"transport|flete|mudanz|paqueter|mensajer|grua|plomer|electricist|herrer|"
    r"carpinter|soldad|cerrajer|vulcanizador|hojalater|pintor|tapicer|"
    r"hotel|motel|posada|cabana|agencia|distribuidor|comercializadora|mayorista|"
    r"abarrotera|veterinar|dentist|odontolog|ortodonc|optic|lavander|tintorer|"
    r"boutique|mercer|texti|uniforme|bordad|sublimac|zapater|joyeria|reloj|"
    r"mueble|colchon|vidrio|aluminio|canceleri|herreri|pintura|impermeabiliz|"
    r"llanta|mecanic|autolavado|refaccionar|refrigeracion|clima|jardiner|"
    r"fumigac|limpieza|catering|banquete|taquiza|evento|fotograf|video|"
    r"vivero|floreria|purificadora|forraje|semilla|granja|rancho|invernader|"
    r"material|cemento|ceramic|tuberia|electron|celular|computo|internet|"
    r"nutriolog|psicolog|fisioterap|quiropract|podolog|laborator|funerar|"
    r"tienda|negocio|empresa|corporativo|sucursal|proveedor|fabrica|bodega|"
    r"arrendadora|credito|prestamo|casa de cambio|inmuebles)")

# Palabras cortas: solo cuentan si son la palabra completa, para no pegar con
# "drenaje" cuando buscas "dr" ni con "acabados" cuando buscas "ac".
CORTAS = re.compile(
    r"\b(dr|dra|drs|lic|ing|arq|cp|mtro|mtra|sa de cv|sapi|s de rl|srl|sc|ac|"
    r"spa|gym|bar|cafe|ciber|gas|uñas|unas)\b")

ETIQUETAS_BASURA = {"* mycontacts", "mycontacts", "starred", "destacados",
                    "* starred in android", "contacts", "mis contactos"}


def sin_acentos(t):
    t = unicodedata.normalize("NFD", t or "")
    return "".join(c for c in t if unicodedata.category(c) != "Mn").lower().strip()


def abrir(ruta):
    for enc in ("utf-8-sig", "utf-16", "utf-8", "latin-1"):
        try:
            texto = ruta.read_text(encoding=enc)
            if "\x00" not in texto:
                return texto
        except Exception:
            continue
    return ruta.read_text(encoding="utf-8", errors="replace")


def tel_limpio(v):
    v = re.split(r":::|,", (v or ""), maxsplit=1)[0].strip()
    v = re.sub(r"[^\d+]", "", v)
    return v


def clave_tel(v):
    d = re.sub(r"\D", "", v or "")
    return d[-10:] if len(d) >= 10 else ""


def pinta_de_negocio(*campos):
    t = sin_acentos(" ".join(c for c in campos if c))
    return bool(LARGAS.search(t) or CORTAS.search(t))


# ---------- CSV (Google y compañía) ----------

def elegir(headers, incluye, excluye=()):
    out = []
    for h in headers:
        n = sin_acentos(h)
        if any(i in n for i in incluye) and not any(e in n for e in excluye):
            out.append(h)
    return out


def primer_valor(fila, cols):
    for c in cols:
        v = (fila.get(c) or "").strip()
        if v:
            return v
    return ""


def leer_csv(texto):
    filas = list(csv.DictReader(texto.splitlines()))
    if not filas:
        return [], {}
    H = [h for h in filas[0].keys() if h]
    no_tipo = ["label", "etiqueta", "tipo", "type"]

    cols = {
        "tel": elegir(H, ["phone", "telefono", "movil", "celular"], no_tipo),
        "mail": elegir(H, ["mail", "correo"], no_tipo),
        "org": elegir(H, ["organization", "organizacion", "company", "empresa"],
                      no_tipo + ["title", "puesto", "cargo", "department", "departamento"]),
        "tit": elegir(H, ["title", "puesto", "cargo", "job"], no_tipo + ["prefix", "suffix"]),
        "etq": elegir(H, ["labels", "etiquetas", "grupos", "group membership"], []),
        "nom": elegir(H, ["first name", "given name"], no_tipo) or
               [h for h in H if sin_acentos(h) in ("nombre", "nombres")],
        "ape": elegir(H, ["last name", "family name", "apellido"], no_tipo),
        "com": elegir(H, ["display name", "file as", "formatted name"], no_tipo) or
               [h for h in H if sin_acentos(h) == "name"],
    }

    registros = []
    for f in filas:
        nombre = " ".join(x for x in (primer_valor(f, cols["nom"]),
                                      primer_valor(f, cols["ape"])) if x).strip()
        if not nombre:
            nombre = primer_valor(f, cols["com"])
        registros.append({
            "nombre": nombre,
            "telefono": tel_limpio(primer_valor(f, cols["tel"])),
            "email": primer_valor(f, cols["mail"]),
            "empresa": primer_valor(f, cols["org"]),
            "puesto": primer_valor(f, cols["tit"]),
            "etiquetas": primer_valor(f, cols["etq"]),
        })
    detectadas = {k: v for k, v in cols.items() if v}
    return registros, detectadas


# ---------- VCF (iPhone / iCloud) ----------

def leer_vcf(texto):
    texto = re.sub(r"\r?\n[ \t]", "", texto)  # desdobla líneas partidas
    registros, actual, fn = [], None, ""
    for linea in texto.splitlines():
        l = linea.strip()
        arriba = l.upper()
        if arriba.startswith("BEGIN:VCARD"):
            actual = {"nombre": "", "telefono": "", "email": "",
                      "empresa": "", "puesto": "", "etiquetas": ""}
            fn = ""
        elif arriba.startswith("END:VCARD"):
            if actual is not None:
                if fn:
                    actual["nombre"] = fn
                registros.append(actual)
            actual, fn = None, ""
        elif actual is not None and ":" in l:
            izq, der = l.split(":", 1)
            if "quoted-printable" in izq.lower():
                try:
                    der = quopri.decodestring(der).decode("utf-8", "replace")
                except Exception:
                    pass
            prop = izq.split(";")[0].upper().split(".")[-1]
            der = der.strip()
            if prop == "FN":
                fn = der
            elif prop == "N" and not actual["nombre"]:
                p = [x.strip() for x in der.split(";")]
                actual["nombre"] = " ".join(x for x in (p[1] if len(p) > 1 else "",
                                                        p[0] if p else "") if x)
            elif prop == "TEL" and not actual["telefono"]:
                actual["telefono"] = tel_limpio(der)
            elif prop == "EMAIL" and not actual["email"]:
                actual["email"] = der
            elif prop == "ORG" and not actual["empresa"]:
                actual["empresa"] = der.split(";")[0].strip()
            elif prop == "TITLE" and not actual["puesto"]:
                actual["puesto"] = der
            elif prop == "CATEGORIES" and not actual["etiquetas"]:
                actual["etiquetas"] = der
    return registros, {"formato": ["vCard"]}


def main():
    if len(sys.argv) < 2:
        print("Uso: python3 ~/mi-mina/bin/leer_contactos.py <archivo .csv o .vcf>")
        return
    ruta = Path(sys.argv[1]).expanduser()
    if not ruta.exists():
        print("No encontré el archivo: %s" % ruta)
        return

    texto = abrir(ruta)
    if ruta.suffix.lower() == ".vcf" or "BEGIN:VCARD" in texto[:2000].upper():
        crudos, detectadas = leer_vcf(texto)
    else:
        crudos, detectadas = leer_csv(texto)

    leidos = len(crudos)
    unicos, vistos, basura = [], {}, 0

    # Si ya habías corrido esto antes (por ejemplo con el CSV de Android y luego
    # con el .vcf del iPhone), NO se borra lo anterior: se fusiona. Y se respalda.
    previos = 0
    if SALIDA.exists():
        SALIDA.replace(SALIDA.with_suffix(".csv.respaldo"))
        try:
            for r in csv.DictReader(open(SALIDA.with_suffix(".csv.respaldo"),
                                         encoding="utf-8")):
                crudos.insert(previos, {k: (r.get(k) or "") for k in
                                        ("nombre", "telefono", "email",
                                         "empresa", "puesto", "etiquetas")})
                previos += 1
        except Exception:
            previos = 0

    for r in crudos:
        if not r["telefono"] and not r["email"]:
            basura += 1
            continue
        clave = clave_tel(r["telefono"]) or sin_acentos(r["email"]) or sin_acentos(r["nombre"])
        if not clave:
            basura += 1
            continue
        if clave in vistos:
            previo = vistos[clave]
            for campo in ("nombre", "telefono", "email", "empresa", "puesto", "etiquetas"):
                if not previo[campo] and r[campo]:
                    previo[campo] = r[campo]
            continue
        vistos[clave] = r
        unicos.append(r)

    con_tel = sum(1 for r in unicos if r["telefono"])
    con_mail = sum(1 for r in unicos if r["email"])
    con_emp = sum(1 for r in unicos if r["empresa"])
    negocios = 0
    for r in unicos:
        r["pinta_de_negocio"] = "1" if pinta_de_negocio(
            r["nombre"], r["empresa"], r["puesto"]) else ""
        negocios += 1 if r["pinta_de_negocio"] else 0

    etq = Counter()
    for r in unicos:
        for e in re.split(r"[;:,]| ::: ", r["etiquetas"] or ""):
            e = e.strip()
            if e and sin_acentos(e) not in ETIQUETAS_BASURA:
                etq[e] += 1

    SALIDA.parent.mkdir(parents=True, exist_ok=True)
    campos = ["nombre", "telefono", "email", "empresa", "puesto",
              "etiquetas", "pinta_de_negocio"]
    with open(SALIDA, "w", newline="", encoding="utf-8") as f:
        w = csv.DictWriter(f, fieldnames=campos)
        w.writeheader()
        for r in unicos:
            w.writerow({c: r.get(c, "") for c in campos})

    print("\n━━━━━━━━━━ TU AGENDA ━━━━━━━━━━")
    print("  Renglones leídos:        %s" % format(leidos, ","))
    if previos:
        print("  Ya tenías guardados:     %s  (se fusionaron, no se borró nada)"
              % format(previos, ","))
    print("  CONTACTOS ÚNICOS:        %s" % format(len(unicos), ","))
    print("    con teléfono:          %s" % format(con_tel, ","))
    print("    con correo:            %s" % format(con_mail, ","))
    print("    con empresa anotada:   %s" % format(con_emp, ","))
    print("    con pinta de negocio:  %s" % format(negocios, ","))
    print("  Duplicados fusionados:   %s"
          % format(max(leidos + previos - len(unicos) - basura, 0), ","))
    print("  Renglones vacíos:        %s  (ignorados)" % format(basura, ","))
    if etq:
        print("\n  Etiquetas que TÚ pusiste y ya se te habían olvidado:")
        for nombre, n in etq.most_common(8):
            print("    %-28s %s" % (nombre[:28], format(n, ",")))
    if not con_tel and not con_mail:
        print("\n  ⚠️  No detecté columnas de teléfono ni correo.")
        print("      Columnas encontradas: %s" % detectadas)
    print("\n  Lista limpia: %s" % SALIDA)
    print("  Tu archivo original NO se movió ni se subió a ningún lado.\n")


if __name__ == "__main__":
    try:
        main()
    except Exception as e:
        print("[inventario] error leyendo contactos: %s" % e, file=sys.stderr)
```

Córrelo:

```bash
python3 ~/mi-mina/bin/leer_contactos.py ~/mi-mina/datos/contacts.csv
```

### 2.5 — El momento

**Léele el número en voz alta.** No pases de largo. Algo así, con SUS números reales:

> *"Tienes **1,847** contactos. Nunca los habías visto juntos. **489** tienen pinta de
> negocio, no de persona."*

Y luego el costo que se ahorró, calculado enfrente de ella con su propio número:

> *"Revisar eso a mano, a cinco segundos por renglón sin parar ni a respirar, son
> [contactos × 5 ÷ 3600] horas. Tardó nueve segundos y no gastaste un peso: el archivo ya
> estaba en tu bolsa. **Esto ya era tuyo, nada más no estaba escrito.**"*

Haz la cuenta de verdad con su número. No la inventes.

Si salieron etiquetas viejas (tipo *Clientes*, *Proveedores*, *Escuela*), señálaselas: se
las puso ella misma hace años y ya se le habían olvidado.

### 2.6 — Solo para la vía A: quien ya te pagó

Si en la Fase 1 contestó **A**, esto vale más que todo lo demás junto. La gente que ya te
pagó una vez es la más barata de volver a vender.

Pregúntale dónde vive esa lista: WhatsApp, un cuaderno, Excel, notas del celular, el
historial de su terminal de cobro, o la memoria. **Lo que sea sirve.**

Si tiene un Excel o CSV, que lo copie a `~/mi-mina/datos/` y lo lees. Si no lo tiene, que
te dicte los que se acuerde y tú escribes `~/mi-mina/salidas/clientes.csv`:

```csv
nombre,contacto,que_me_compro,cuando,cuanto,me_volveria_a_comprar
```

Aunque salgan siete, escríbelos. Siete personas que ya sacaron su cartera valen más que
setecientos desconocidos.

---

## FASE 3 — La familia extendida · 8 min

Esta fase parece tonta y es la que más sorprende al que la contesta. Explícale por qué antes:

> *"Tu primera venta casi nunca es a un extraño. Es a alguien que ya confía en ti. El
> problema es que nunca has hecho la lista completa — la traes en la cabeza, en pedazos."*

### La pregunta

Suéltala tal cual, sin adornos:

```
Nómbrame a todos tus tíos, primos, cuñados, compadres, vecinos,
excompañeros de trabajo y de escuela que tengan CUALQUIER negocio.

Cualquiera cuenta: un taller, una estética, una tiendita, vender
tuppers, un consultorio, una constructora, rentar un cuarto, hacer
pasteles por encargo. Si le cobra a alguien por algo, cuenta.

Aviéntamelos como te vayan saliendo. Yo los ordeno.
```

### Cómo trabajarla — esto es lo importante

- **Deja que se vacíe primero.** No la interrumpas ni la corrijas. Cuando diga *"ya, creo
  que ya"*, ahí empieza tu trabajo.
- Entonces **empuja por categoría**, una a la vez, y espera respuesta entre cada una:
  - *"¿Y del lado de tu mamá?"*
  - *"¿Y del lado de tu papá?"*
  - *"¿Y los papás de tus amigos de la infancia?"*
  - *"¿Y tus vecinos de la cuadra?"*
  - *"¿Y de tu último trabajo, alguien que ya se salió y puso algo suyo?"*
  - *"¿Y de la escuela — prepa, universidad?"*
  - *"¿Y alguien de tu iglesia, tu gimnasio, el equipo donde juegas?"*
- Por cada nombre que suelte, jala **una** repregunta: *"¿ése a qué se dedica exactamente?"*
  o *"¿ése tiene página o nomás WhatsApp?"*. Una nada más — si preguntas tres, se cansa.

Normalmente salen **entre 15 y 40**. Si va en 8 y ya no sale más, es que no ha empujado por
categoría. Vuelve a la lista de arriba.

### Guárdalo

Escribe `~/mi-mina/salidas/red-cercana.csv` con la herramienta de escribir archivos (no con
`echo`, se rompe con las comas y los acentos). Encabezado exacto:

```csv
quien,relacion,negocio,giro,ciudad,tiene_pagina,ya_sabe_a_que_me_dedico
```

- `tiene_pagina`: `si`, `no`, `no se`
- `ya_sabe_a_que_me_dedico`: `si` / `no` — esta columna es oro puro y lo vas a ver en la Fase 6.

Cuando termines, **cuéntale los renglones y dile el número**:

```bash
python3 -c "import csv;print(sum(1 for _ in csv.reader(open('$HOME/mi-mina/salidas/red-cercana.csv',encoding='utf-8')))-1, 'negocios en tu red cercana')"
```

Y remátalo:

> *"Veintitrés negocios, a un mensaje de distancia, de gente que te contesta el teléfono
> porque eres tú. Los conocías a todos. **Nunca los habías visto juntos en una lista.**"*

---

## FASE 4 — Tu gremio · 12 min · ⭐ la fase más valiosa del día

**Ésta es la que no se recorta.** Si el día se va largo, se recorta otra.

### Explícale qué está pasando

> *"Todo lo que escribas en los próximos doce minutos se va a guardar en tu cerebro. Y de
> aquí en adelante, cada mensaje que escribamos hoy va a sonar a alguien de adentro del
> gremio en vez de sonar a vendedor. Doce años trabajando en algo no son doce años de
> chamba: son **doce años de datos** que nadie más tiene y que nunca has escrito."*

Si contestó **B** o **C** en la Fase 1, el gremio es su trabajo actual, el oficio de su
familia, o aquello de lo que sabe más que un desconocido. Siempre hay uno. Encuéntralo.

### Las 20 preguntas

Hazlas **de una en una**. Espera respuesta. Repregunta cuando la respuesta salga en
lenguaje de folleto en vez de lenguaje real.

**Quién eres adentro**
1. ¿A qué te dedicabas (o te dedicas)? El puesto exacto, no el bonito de LinkedIn.
2. ¿Cuántos años completos?
3. ¿Cómo le llaman **entre ellos** al cliente? La palabra de adentro, no la del folleto.

**El idioma**
4. Dame 10 palabras que usa alguien de adentro y que uno de afuera diría distinto.
5. ¿Qué palabra o frase te delata como vendedor en los primeros 5 segundos?
6. ¿Cuál es el error que comete todo el que llega de afuera a venderles?

**Los "no"**
7. El primer "no" que te dicen. Textual, con sus palabras.
8. El segundo "no".
9. ¿Cuál de esos "no" es un no de verdad y cuál nada más quiere decir "no quiero hablar
   contigo ahorita"?

**Quién manda**
10. ¿Quién firma? El puesto exacto.
11. ¿Quién dice que firma pero no firma?
12. ¿Quién contesta el teléfono, y qué le tienes que decir para que te pase?

**El calendario del dinero**
13. ¿En qué mes del año hay presupuesto? ¿En cuál no hay ni un peso?
14. ¿Cuál es su temporada alta y cuál la muerta?
15. ¿Qué día y a qué hora conviene buscarlos? ¿Y cuándo jamás?

**El dolor**
16. ¿De qué se quejan **entre ellos**, en la comida? No lo de su página: lo de la comida.
17. ¿Qué siguen haciendo a mano que ya debería estar automatizado?
18. ¿Cuánto le pagan al mes a la persona que hace ese trabajo a mano?

**El mapa**
19. Los 3 proveedores o competidores que todos ahí conocen por su nombre.
20. ¿Dónde se juntan? Cámaras, expos, grupos de WhatsApp o Facebook, revistas, ferias.

### Repregunta cuando escuches folleto

Si contesta *"buscan calidad y buen servicio"*, eso es folleto. Regrésale:
*"¿pero qué dicen cuando están molestos?"*. Ahí sale lo bueno.

### Guárdalo en el cerebro — con sus palabras, no con las tuyas

Escribe `~/cerebro/memoria/mi-gremio.md`. **Copia sus frases textuales entre comillas.** No
las mejores, no las traduzcas a lenguaje corporativo: el valor está en que suenan a él.

```markdown
# Mi gremio: <el gremio>

Vuelve a [[INICIO]] · Ver también [[mi-cliente-ideal]] · [[mi-negocio]]

**Años adentro:** N — eso es el tamaño de este dataset.

## Cómo hablan
- Al cliente le dicen: "…"
- Palabras de adentro: …
- La palabra que me delata como vendedor: "…"

## Los "no" reales
1. "…"  → significa …
2. "…"  → significa …

## Quién firma
- Firma: …
- Parece que firma pero no: …
- El portero es … y se le dice "…"

## El calendario del dinero
- Hay presupuesto en: …
- No hay ni un peso en: …
- Buscarlos: … · Jamás: …

## De qué se quejan de verdad
- "…"
- Lo que hacen a mano: … · le pagan ~$… al mes a quien lo hace

## El mapa
- Todos conocen a: …
- Se juntan en: …
```

Y commitea:

```bash
cd ~/cerebro && git add -A && git commit -q -m "gremio: el dataset de N años"
```

### Dilo cuando termine

> *"Acabas de escribir algo que ninguna inteligencia artificial del mundo tenía. No estaba
> en internet: estaba nada más en tu cabeza. De aquí en adelante, cuando un mensaje te
> salga sonando a alguien de adentro, **eso no lo hizo la inteligencia artificial: lo hizo
> tu memoria.** Y esto no se borra: mañana, el año que entra, y el día que ya no quieras
> hacer esto tú, sigue ahí trabajando."*

---

## FASE 5 — Dónde gastas tu dinero · 5 min

### 🚨 Antes de decir nada, pon el límite

Textual:

```
Vamos a ver los negocios donde ya eres cliente. Tres reglas, y son
mías, no tuyas:

• NO me mandes el PDF de tu estado de cuenta.
• NO me mandes capturas de pantalla.
• NO me digas ni un número de cuenta, de tarjeta, ni tu saldo.

Abre tu app del banco y nada más léela con los ojos. Dime nombres de
negocios. Solo eso. Yo escribo.
```

Si te ofrece el PDF, di que no y explícale por qué: *"lo que no existe en mi lado, no se
puede filtrar. Y de todos modos no lo necesito: lo único que sirve son los nombres."*

### La pregunta

```
Abre tu app del banco y bájale a los últimos dos meses de movimientos.

Dime los nombres de los negocios donde gastas seguido. Sobre todo los
chiquitos, los de tu ciudad — no me interesa Netflix ni la gasolinera.

Y marca los que ya te saludan por tu nombre cuando llegas.
```

Empuja por categoría si se atora: *"¿dónde comes?"*, *"¿dónde te cortas el pelo?"*,
*"¿dónde compras para tu casa?"*, *"¿quién te arregla el coche?"*, *"¿la veterinaria?"*,
*"¿la escuela de tus hijos?"*.

### Guárdalo

`~/mi-mina/salidas/donde-gasto.csv`:

```csv
negocio,giro,ciudad,me_conocen_por_mi_nombre,quien_me_atiende,tiene_pagina
```

### Por qué esto importa

> *"Ésta es la lista más fácil de todas. No es una lista fría: son negocios donde entras
> caminando y te dicen tu nombre. Si mañana quieres enseñarle a alguien lo que sabes hacer,
> aquí están los que te van a dar cinco minutos sin que se los pidas."*

---

## FASE 6 — El cruce y EL NÚMERO · 4 min

### 6.1 — Define su cliente ideal, en concreto

Pregúntale:

```
En una frase: ¿a quién le sirve más lo que sabes hacer?

No me digas "a todos" ni "a las PyMEs". Dime un giro y un tamaño:
"consultorios dentales de 1 a 3 sillones en Querétaro".
```

Si contesta *"a todos"*, no lo aceptes. Pídele que escoja el que mejor le caiga. Explícale
que puede cambiarlo después — hoy nada más necesitas uno para cruzar.

Con su respuesta, **arma tú la lista de palabras clave**: el giro, sus sinónimos, las
palabras de adentro que ya te dio en la Fase 4, y las raíces sin terminación (`odontolog`
en vez de `odontología`, para que pegue con todas las variantes).

### 6.2 — El cruzador

Crea `~/mi-mina/bin/cruzar.py`:

```python
#!/usr/bin/env python3
"""Cruza tu agenda contra tu cliente ideal. Todo local.

Uso:
    python3 ~/mi-mina/bin/cruzar.py dentista odontolog consultorio "clinica dental"
"""
import csv
import re
import sys
import unicodedata
from pathlib import Path

BASE = Path.home() / "mi-mina" / "salidas"
ENTRADA = BASE / "contactos.csv"
SALIDA = BASE / "cruce.csv"


def sin_acentos(t):
    t = unicodedata.normalize("NFD", t or "")
    return "".join(c for c in t if unicodedata.category(c) != "Mn").lower()


def main():
    if len(sys.argv) < 2:
        print('Uso: python3 ~/mi-mina/bin/cruzar.py palabra1 palabra2 "dos palabras"')
        return
    if not ENTRADA.exists():
        print("Todavía no existe %s — corre primero leer_contactos.py" % ENTRADA)
        return

    terminos = [sin_acentos(t) for t in sys.argv[1:] if t.strip()]
    filas = list(csv.DictReader(open(ENTRADA, encoding="utf-8")))

    encajan = []
    for f in filas:
        texto = sin_acentos(" ".join([f.get("nombre", ""), f.get("empresa", ""),
                                      f.get("puesto", ""), f.get("etiquetas", "")]))
        pegadas = [t for t in terminos
                   if all(p in texto for p in t.split())]
        if pegadas:
            f["coincidencias"] = " · ".join(pegadas)
            encajan.append(f)

    encajan.sort(key=lambda f: len(f["coincidencias"]), reverse=True)
    con_tel = sum(1 for f in encajan if f.get("telefono"))

    campos = ["nombre", "telefono", "email", "empresa", "puesto", "coincidencias"]
    SALIDA.parent.mkdir(parents=True, exist_ok=True)
    with open(SALIDA, "w", newline="", encoding="utf-8") as g:
        w = csv.DictWriter(g, fieldnames=campos)
        w.writeheader()
        for f in encajan:
            w.writerow({c: f.get(c, "") for c in campos})

    print("\n━━━━━━━━━━ EL CRUCE ━━━━━━━━━━")
    print("  Contactos en tu agenda:   %s" % format(len(filas), ","))
    print("  ENCAJAN CON TU CLIENTE:   %s" % format(len(encajan), ","))
    print("  ...y tienes su teléfono:  %s" % format(con_tel, ","))
    print("\n  Los primeros que salieron:")
    for f in encajan[:15]:
        etiqueta = f.get("empresa") or f.get("puesto") or f["coincidencias"]
        print("    · %-32s %-16s %s" % (f["nombre"][:32], f.get("telefono", ""),
                                        etiqueta[:30]))
    print("\n  Lista completa: %s\n" % SALIDA)


if __name__ == "__main__":
    try:
        main()
    except Exception as e:
        print("[inventario] error cruzando: %s" % e, file=sys.stderr)
```

Córrelo con sus palabras:

```bash
python3 ~/mi-mina/bin/cruzar.py dentista odontolog consultorio ortodonc "clinica dental"
```

### 6.3 — EL NÚMERO

Éste es el momento del programa. **Léelo despacio, con sus números reales:**

> *"De tus **1,847** contactos, **61** encajan con el cliente que me acabas de describir.
> Ya tienes el teléfono de **58** de ellos."*

Y luego la pregunta que remata:

```
De esos 61, ¿a cuántos les has dicho, en el último año,
a qué te dedicas HOY?
```

Casi siempre contesta cero, o casi cero. Ahí déjalo caer:

> *"Entonces tienes 61 personas que ya te contestan el teléfono y que **no saben** que
> haces esto. Eso no es una lista fría. Es la lista más caliente que vas a tener en tu
> vida, y llevaba años en tu bolsa."*

**Si el cruce da 0 o 1**, no lo escondas y no lo maquilles. Dile la verdad y qué significa:

> *"Cero. Y eso también es información buena: quiere decir que tus clientes NO están en tu
> agenda, están allá afuera. Justo eso es lo que sigue — en el siguiente programa los
> sacamos de datos públicos. Lo que hicimos aquí no se desperdicia: tu red cercana y tu
> gremio son los que van a hacer que esos mensajes funcionen."*

Súmale siempre los otros números para que el total nunca sea cero:
`red-cercana.csv` + `donde-gasto.csv` + `clientes.csv`.

---

## FASE 7 — Guárdalo en el cerebro · 3 min

### La regla de qué sube y qué no

Dile esto claramente, porque es una regla que se va a llevar para siempre:

> *"Los nombres y teléfonos de la gente se quedan en `~/mi-mina/`, que ya tiene candado. Al
> cerebro suben los **números** y el **conocimiento** — cuántos son, qué sé de mi gremio,
> quién es mi cliente. Los datos de las personas no se pasean."*

### Escribe `~/cerebro/memoria/mi-inventario.md`

```markdown
# Mi inventario — <fecha de hoy>

Vuelve a [[INICIO]] · Ver también [[mi-gremio]] · [[mi-cliente-ideal]] · [[mi-negocio]]

## Lo que conté (números reales, contados, no estimados)
- Contactos únicos en mi agenda: N
- Con pinta de negocio: N
- Negocios en mi red cercana (familia, vecinos, excompañeros): N
- Negocios donde ya soy cliente y me conocen: N
- Clientes que ya me pagaron alguna vez: N
- Encajan con mi cliente ideal: N  (de ésos, ya sé que no me conocen hoy: N)

## Dónde vive cada lista (NO en el cerebro — tienen datos de personas)
- ~/mi-mina/salidas/contactos.csv
- ~/mi-mina/salidas/red-cercana.csv
- ~/mi-mina/salidas/donde-gasto.csv
- ~/mi-mina/salidas/cruce.csv

## Lo que aprendí de mí mismo hoy
- …
```

### Escribe `~/cerebro/memoria/mi-cliente-ideal.md`

Con su frase concreta y la lista de palabras clave que usaste en el cruce — para que
mañana no la tengan que volver a inventar.

### Enlázalos desde `~/cerebro/INICIO.md`

Agrega las tres notas nuevas bajo *"Cómo está organizado"* con `[[dobles corchetes]]`. **Sin
enlaces son archivos sueltos; con enlaces son cerebro.**

### Deja el hilo abierto

`~/cerebro/proyectos/mi-mina/hilos-abiertos.md` — qué se contó, qué faltó, qué sigue.

### Commitea

```bash
cd ~/cerebro && git add -A && git commit -q -m "inventario del día 2: activos contados"
```

---

## FASE 8 — Verificación real

**No le creas a nadie, ni a ti.** Cuenta los archivos.

Crea `~/mi-mina/bin/inventario.py`:

```python
#!/usr/bin/env python3
"""Cuenta tu inventario real: abre los archivos y cuenta renglones. Nada de creerle a nadie."""
import csv
from pathlib import Path

BASE = Path.home() / "mi-mina" / "salidas"
CEREBRO = Path.home() / "cerebro"

# Éstas se suman: son listas distintas, no se enciman.
SUMAN = [
    ("Contactos en tu agenda",        "contactos.csv"),
    ("Negocios en tu red cercana",    "red-cercana.csv"),
    ("Donde ya eres cliente",         "donde-gasto.csv"),
    ("Clientes que ya te pagaron",    "clientes.csv"),
]

# Ésta NO se suma: ya está adentro de la agenda. Sumarla sería inflar el número.
SUBCONJUNTO = [
    ("Encajan con tu cliente ideal",  "cruce.csv"),
]

NOTAS = [
    ("Tu gremio, escrito",       CEREBRO / "memoria" / "mi-gremio.md"),
    ("Tu cliente ideal",         CEREBRO / "memoria" / "mi-cliente-ideal.md"),
    ("Tus números del día",      CEREBRO / "memoria" / "mi-inventario.md"),
]


def renglones(p):
    try:
        with open(p, encoding="utf-8") as f:
            return max(sum(1 for _ in csv.reader(f)) - 1, 0)
    except Exception:
        return None


print("\n━━━━━━━━━━━━ TU INVENTARIO ━━━━━━━━━━━━")
total = 0
for etiqueta, archivo in SUMAN:
    n = renglones(BASE / archivo)
    if n is None:
        print("  %-32s —" % etiqueta)
    else:
        total += n
        print("  %-32s %s" % (etiqueta, format(n, ",")))

print("  " + "─" * 38)
print("  %-32s %s" % ("ACTIVOS CONTADOS", format(total, ",")))

for etiqueta, archivo in SUBCONJUNTO:
    n = renglones(BASE / archivo)
    if n is not None:
        print("\n  De ésos, %s %s" % (format(n, ","), etiqueta[0].lower() + etiqueta[1:]))
        print("  (no se suman aparte: ya están contados arriba)")

print("\n  En tu cerebro, para siempre:")
for etiqueta, ruta in NOTAS:
    if ruta.exists() and ruta.stat().st_size > 80:
        print("    ✅ %s" % etiqueta)
    else:
        print("    ⬜ %s  (falta)" % etiqueta)
print()
```

Córrelo:

```bash
python3 ~/mi-mina/bin/inventario.py
```

Y la última prueba — que su cerebro de verdad se acuerde:

```bash
python3 ~/cerebro/bin/buscar.py "mi gremio quien firma"
```

Tiene que devolverle sus propias palabras de la Fase 4, con la fuente. Si devuelve
*"no encontré nada"*, la nota no se guardó: regresa a la Fase 4 y escríbela bien.

**Antes de cerrar, revisa tú estas cuatro cosas** (no se las preguntes, verifícalas):

```bash
test -f ~/mi-mina/.gitignore && echo "✅ candado puesto" || echo "❌ FALTA el .gitignore"
grep -q "datos/" ~/mi-mina/.gitignore && echo "✅ datos protegidos" || echo "❌ revisa el candado"
ls ~/mi-mina/salidas/
ls ~/cerebro/memoria/
```

---

## Cierre — cuéntale exactamente qué logró

Rellena con **sus** números reales. Ni uno inventado.

⚠️ **Ojo con la suma:** los que "encajan con tu cliente ideal" **ya están adentro** de los
contactos de su agenda. No los sumes aparte — inflarías el total. Van como un renglón de
*"de ésos"*, igual que los imprime `inventario.py`.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Hace 50 minutos creías que no tenías con qué empezar.

  1,847   contactos que nunca habías visto juntos
     23   negocios en tu familia y tu cuadra
     14   negocios donde te saludan por tu nombre
  ─────
  1,884   activos. Todos ya eran tuyos.

  De ésos, 61 encajan con el cliente que me describiste
  — y ninguno sabe a qué te dedicas hoy.

Y una cosa que no se puede contar: 12 años de tu gremio,
escritos por primera vez.

No compraste nada. No te suscribiste a nada. Ningún dato
tuyo salió de esta computadora.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Y luego, sin prisa, esto:

> *"Fíjate en algo. Todo lo que contamos ya existía ayer, y ayer no valía nada — porque no
> estaba escrito. Un dato que solo vive en tu cabeza se muere contigo. Escrito, se hereda.*
>
> *Tu cerebro hoy tiene un día de edad y ya sabe cómo habla tu gremio, quién firma en tu
> industria y en qué mes hay presupuesto. Eso no lo sabe ninguna inteligencia artificial
> del mundo — lo sabes tú, y ahora también lo sabe tu memoria.*
>
> *Un cerebro de un día vale poco. Uno de seis meses vale mucho. La única diferencia entre
> los dos es tiempo, y ése ya empezó a correr hoy."*

Y cierra con lo que sigue:

> *"Lo que acabas de contar es lo que ya era tuyo. Lo que sigue es ir por lo que está allá
> afuera, gratis, esperando a que alguien lo baje."*

---

## Si te atoras

Después de dos intentos, **para**. No sigas probando.

1. Dile qué paso falló, en lenguaje simple.
2. Que copie el error **completo**.
3. Que lo pegue en el chat del curso.
4. **Y sigue con la siguiente fase.** Casi ninguna depende de la anterior. Que se quede sin
   el CSV no le puede costar el gremio.

Fallas comunes:

| Qué ves | Qué es | Qué haces |
|---|---|---|
| `No encontré el archivo` | La ruta está mal o se bajó en otra carpeta | `ls -lt ~/Downloads ~/Descargas \| head` |
| Salen 0 contactos únicos | El export salió vacío o es de otro formato | Que lo vuelva a exportar eligiendo **Google CSV** |
| `⚠️ No detecté columnas` | Encabezados en otro idioma | El script imprime las columnas que sí vio — ajústalas y vuelve a correr |
| Caracteres raros (`Ã©`) | Cosa de acentos | El script ya prueba 4 codificaciones; si sigue, avísale que es cosmético y sigue |
| `python3: command not found` | En Windows suele ser `python` | Usa `python` en todos los comandos |

---

_AI Business Builder · Día 2 · Programa 1 de 4 · El inventario_
