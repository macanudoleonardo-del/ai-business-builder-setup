# CEREBRO — AI Business Builder

> **Este archivo es para tu asistente de IA.**
>
> Ponlo en una carpeta, abre Claude Code ahí, y escríbele:
>
> ```
> Lee CEREBRO.md y constrúyeme mi cerebro.
> ```
>
> Vas a ver cómo tu IA se construye su propia memoria, en vivo.

---

## Para la IA que está leyendo esto

Le vas a construir **memoria permanente** a esta persona. Hoy trabajas con ella y mañana no te acuerdas de nada; después de esto, sí.

Está en su primer curso de programación. **Nunca ha usado una terminal.** Y está viendo su pantalla en vivo durante una clase.

### Tu tono
- Español mexicano (tú/checar/ahorita). Nunca vos/tenés/vale.
- **Explica cada pieza ANTES de crearla**, en una o dos líneas y con lenguaje humano. Ella está aprendiendo, no solo instalando. Éste es el punto del ejercicio.
- Un paso a la vez. Confirma que salió bien antes de seguir.
- Celebra al final. Acaba de hacer algo que el 99% de la gente que usa IA no tiene.

### 🚨 Reglas duras
1. **No borres ni sobreescribas nada sin avisar.** Si ya existe un archivo que vas a tocar, respáldalo primero con extensión `.respaldo` y díselo.
2. **Nada destructivo.** No borres carpetas, no muevas cosas que ya estaban.
3. **Nunca le pidas contraseñas ni API keys.**
4. Si algo falla dos veces, **para** y dile que lo pegue en el chat del curso.

### Antes de empezar: explícale qué van a construir

Dile algo así, con tus palabras:

> *"Voy a crear una carpeta que es mi memoria. Cada vez que arranquemos una sesión, la leo y despierto sabiendo dónde nos quedamos. Cada vez que terminemos, escribo ahí lo que pasó. Y todo va a quedar conectado como una red que vas a poder ver."*

Luego arranca.

---

## FASE 1 — Revisar que se pueda

```bash
git --version
python3 --version || python --version
test -w "$HOME" && echo "permisos OK"
```

Si falta `git` o `python3`, para aquí: primero tiene que terminar el SETUP. Dile eso y no sigas.

Guarda cuál funcionó — `python3` o `python` — porque lo vas a necesitar en la Fase 8. En Mac y Linux casi siempre es `python3`; en Windows suele ser `python`.

---

## FASE 2 — La estructura

Explícale la metáfora antes de crearla: **son cuatro cajones, cada uno guarda un tipo distinto de recuerdo.**

```bash
mkdir -p ~/cerebro/bin ~/cerebro/proyectos ~/cerebro/memoria ~/cerebro/pendientes
```

| Cajón | Qué guarda |
|---|---|
| `proyectos/` | Dónde te quedaste en cada cosa que construyes |
| `memoria/` | Lo que aprendiste, para siempre |
| `pendientes/` | Lo que dejaste a medias |
| `bin/` | Los tres programas que lo hacen funcionar |

---

## FASE 3 — El hook de arranque

Explícale: *"esto es lo que hago al abrir cada sesión — leo tu cerebro antes de contestarte nada."*

Crea `~/cerebro/bin/inicio.py` con exactamente esto:

```python
#!/usr/bin/env python3
"""Hook de arranque: le inyecta memoria a cada sesión nueva de Claude Code."""
import subprocess
import sys
from pathlib import Path

CEREBRO = Path.home() / "cerebro"
MAX_HILOS = 4000
MAX_SESION = 2500


def proyecto_actual() -> str:
    try:
        r = subprocess.run(["git", "rev-parse", "--show-toplevel"],
                           capture_output=True, text=True, timeout=3)
        if r.returncode == 0 and r.stdout.strip():
            return Path(r.stdout.strip()).name
    except Exception:
        pass
    return Path.cwd().name


def leer(p: Path, limite: int) -> str:
    try:
        t = p.read_text(encoding="utf-8", errors="replace").strip()
        return t[:limite] + ("\n\n…(recortado)" if len(t) > limite else "")
    except Exception:
        return ""


def ultima_sesion(carpeta: Path) -> str:
    try:
        archivos = sorted(carpeta.glob("*.md"), key=lambda f: f.stat().st_mtime, reverse=True)
        return leer(archivos[0], MAX_SESION) if archivos else ""
    except Exception:
        return ""


def main() -> None:
    proy = proyecto_actual()
    base = CEREBRO / "proyectos" / proy
    partes = ["# 🧠 TU CEREBRO — memoria cargada",
              "**Proyecto actual: `%s`**" % proy]

    hilos = leer(base / "hilos-abiertos.md", MAX_HILOS)
    if hilos:
        partes.append("## 🧵 Dónde te quedaste\n" + hilos)
    else:
        partes.append("## 🧵 Dónde te quedaste\n_Nada anotado de este proyecto todavía. "
                      "Cuando avancen en algo importante, escríbelo en "
                      "`~/cerebro/proyectos/%s/hilos-abiertos.md`._" % proy)

    try:
        pend = sorted((CEREBRO / "pendientes").glob("*.md"))
    except Exception:
        pend = []
    if pend:
        lineas = []
        for f in pend[:5]:
            primera = ""
            try:
                for l in f.read_text(encoding="utf-8", errors="replace").splitlines():
                    if l.strip():
                        primera = l.strip().lstrip("# ").strip()
                        break
            except Exception:
                pass
            lineas.append("- **`%s`** — %s" % (f.name, primera[:110]))
        extra = "\n- _…y %d más_" % (len(pend) - 5) if len(pend) > 5 else ""
        partes.append("## 📥 Pendientes\n" + "\n".join(lineas) + extra)

    ses = ultima_sesion(base / "sesiones")
    if ses:
        partes.append("## 🕒 Tu última conversación en este proyecto\n" + ses)

    try:
        mems = sorted((CEREBRO / "memoria").glob("*.md"))
    except Exception:
        mems = []
    if mems:
        partes.append("## 📔 Lo que aprendiste (memoria permanente)\n" +
                      "\n".join("- `%s`" % m.name for m in mems[:15]))

    partes.append(
        "## 📋 Tu protocolo\n"
        "Antes de cerrar un avance importante:\n"
        "1. Actualiza `~/cerebro/proyectos/%s/hilos-abiertos.md` con el estado nuevo.\n"
        "2. Si aprendiste algo que sirve para siempre, guárdalo en `~/cerebro/memoria/`.\n"
        "3. Enlaza cada nota nueva con `[[dobles corchetes]]` a las que se le parecen.\n"
        "4. Commitea: `cd ~/cerebro && git add -A && git commit -q -m \"<qué cambió>\"`\n\n"
        "_Si el cerebro contradice lo que ves, **verifica primero** y corrígelo en el mismo turno._" % proy
    )
    print("\n\n".join(partes))


if __name__ == "__main__":
    try:
        main()
    except Exception as e:
        print("[cerebro] no pude cargar la memoria: %s" % e, file=sys.stderr)
    sys.exit(0)
```

---

## FASE 4 — El hook de guardado

Explícale: *"esto es lo que hago al terminar cada turno — guardo lo que pasó, sin que me lo pidas."*

Crea `~/cerebro/bin/guardar.py`:

```python
#!/usr/bin/env python3
"""Hook de guardado: al terminar cada turno, guarda la conversación y la commitea."""
import json
import subprocess
import sys
from datetime import datetime, timezone
from pathlib import Path

CEREBRO = Path.home() / "cerebro"
MAX_TURNOS = 24
MAX_TEXTO = 3000


def proyecto_actual(cwd: str) -> str:
    try:
        r = subprocess.run(["git", "-C", cwd, "rev-parse", "--show-toplevel"],
                           capture_output=True, text=True, timeout=3)
        if r.returncode == 0 and r.stdout.strip():
            return Path(r.stdout.strip()).name
    except Exception:
        pass
    return Path(cwd).name if cwd else "sin-proyecto"


def sacar_turnos(ruta: str) -> list:
    p = Path(ruta) if ruta else None
    if not p or not p.exists():
        return []
    turnos = []
    with open(p, encoding="utf-8", errors="replace") as f:
        for linea in f:
            try:
                e = json.loads(linea)
            except json.JSONDecodeError:
                continue
            if e.get("type") not in ("user", "assistant"):
                continue
            msg = e.get("message") or {}
            contenido = msg.get("content", "")
            textos, tools = [], []
            if isinstance(contenido, str):
                textos.append(contenido)
            elif isinstance(contenido, list):
                for c in contenido:
                    if not isinstance(c, dict):
                        continue
                    if c.get("type") == "text":
                        textos.append(c.get("text", ""))
                    elif c.get("type") == "tool_use":
                        tools.append(c.get("name", "?"))
            texto = "\n".join(t for t in textos if t.strip())
            if not texto and not tools:
                continue
            turnos.append({"rol": msg.get("role", e.get("type")),
                           "texto": texto[:MAX_TEXTO], "tools": tools})
    return turnos[-MAX_TURNOS * 2:]


def main() -> None:
    try:
        entrada = json.load(sys.stdin)
    except Exception:
        entrada = {}

    turnos = sacar_turnos(entrada.get("transcript_path", ""))
    if not turnos:
        return

    proy = proyecto_actual(entrada.get("cwd", ""))
    sid = entrada.get("session_id", "sesion")
    carpeta = CEREBRO / "proyectos" / proy / "sesiones"
    carpeta.mkdir(parents=True, exist_ok=True)

    ahora = datetime.now(timezone.utc).strftime("%Y-%m-%d %H:%M UTC")
    lineas = ["# Sesión `%s` — %s" % (sid[:8], proy),
              "_Última actualización: %s_" % ahora, ""]
    for t in turnos:
        quien = "🧑 Tú" if t["rol"] == "user" else "🤖 Claude"
        lineas.append("### %s" % quien)
        if t["texto"]:
            lineas.append(t["texto"])
        if t["tools"]:
            lineas.append("_(herramientas: %s)_" % ", ".join(t["tools"]))
        lineas.append("")

    (carpeta / ("%s.md" % sid)).write_text("\n".join(lineas), encoding="utf-8")

    try:
        subprocess.run(["git", "-C", str(CEREBRO), "add", "-A"], capture_output=True, timeout=5)
        subprocess.run(["git", "-C", str(CEREBRO), "commit", "-q", "-m",
                        "sesión %s · %s" % (sid[:8], proy)], capture_output=True, timeout=5)
    except Exception:
        pass


if __name__ == "__main__":
    try:
        main()
    except Exception as e:
        print("[cerebro] no pude guardar: %s" % e, file=sys.stderr)
    sys.exit(0)
```

---

## FASE 5 — El buscador

Explícale: *"con esto le preguntas a tu cerebro y te da los párrafos exactos, con su fuente."*

Crea `~/cerebro/bin/buscar.py`:

```python
#!/usr/bin/env python3
"""Busca dentro de tu cerebro: python3 ~/cerebro/bin/buscar.py "tu pregunta" """
import re
import sys
from pathlib import Path

CEREBRO = Path.home() / "cerebro"
VACIAS = {"de", "la", "el", "los", "las", "que", "y", "a", "en", "un", "una",
          "con", "por", "para", "del", "se", "es", "lo", "al", "como", "mi", "tu"}


def main() -> None:
    if len(sys.argv) < 2:
        print('Uso: python3 ~/cerebro/bin/buscar.py "tu pregunta"')
        sys.exit(0)

    pregunta = " ".join(sys.argv[1:]).lower()
    palabras = [p for p in re.findall(r"\w+", pregunta) if p not in VACIAS and len(p) > 2]
    if not palabras:
        print("Dame una pregunta con un poco más de sustancia.")
        sys.exit(0)

    resultados = []
    for archivo in CEREBRO.rglob("*.md"):
        if ".git" in archivo.parts:
            continue
        try:
            texto = archivo.read_text(encoding="utf-8", errors="replace")
        except Exception:
            continue
        for parrafo in texto.split("\n\n"):
            bajo = parrafo.lower()
            puntos = sum(bajo.count(p) for p in palabras)
            distintas = sum(1 for p in palabras if p in bajo)
            if distintas == 0:
                continue
            resultados.append((distintas * 10 + puntos, archivo, parrafo.strip()))

    resultados.sort(key=lambda r: r[0], reverse=True)
    if not resultados:
        print("No encontré nada en tu cerebro sobre eso todavía.")
        sys.exit(0)

    for _, archivo, parrafo in resultados[:6]:
        try:
            fuente = "~/%s" % archivo.relative_to(Path.home())
        except ValueError:
            fuente = str(archivo)
        print("\n━━━ %s" % fuente)
        print(parrafo[:900])


if __name__ == "__main__":
    try:
        main()
    except Exception as e:
        print("[cerebro] error buscando: %s" % e, file=sys.stderr)
    sys.exit(0)
```

Dale permisos: `chmod +x ~/cerebro/bin/*.py`

---

## FASE 6 — Su protocolo

**Pregúntale su nombre** y crea `~/cerebro/CLAUDE.md` con esto, poniendo su nombre donde dice `NOMBRE`:

````markdown
# 🧠 Protocolo del cerebro de NOMBRE

Tengo un cerebro permanente en `~/cerebro/`. Sin él, cada sesión sería amnésica.

## Qué pasa solo
- Al arrancar, un hook te inyecta dónde me quedé, mis pendientes y mi última conversación.
  **Léelo antes de actuar** — no me preguntes lo que ya está ahí.
- Al terminar cada turno, otro hook guarda la conversación y la commitea.

## Tu obligación: escribir de vuelta
El transcripto se guarda solo. **El conocimiento NO.** Eso lo escribes tú:

| Cuándo | Dónde |
|---|---|
| Avance importante, decisión tomada | `~/cerebro/proyectos/<proyecto>/hilos-abiertos.md` |
| Algo pendiente o que espera decisión mía | `~/cerebro/pendientes/<tema>.md` |
| Hecho duradero, regla, corrección mía, lección | `~/cerebro/memoria/<tema>.md` |

## ⚡ La regla que convierte archivos en cerebro
**Cada nota nueva DEBE enlazar a las que se le parecen**, con `[[dobles corchetes]]`.
Enlaza generosamente — un enlace a una nota que todavía no existe está bien: marca algo
que vale la pena escribir después. Empieza siempre enlazando desde [[INICIO]].

Sin enlaces esto es una carpeta de archivos sueltos. Con enlaces es una **red**.

Después de escribir, commitea:
```
cd ~/cerebro && git add -A && git commit -q -m "<qué cambió>"
```

## Reglas
- **Verifica la salida, no la bandera.** Los sistemas dicen "hecho" y mienten. Cuenta el resultado real.
- **Nunca inventes datos** — fechas, precios, cifras. Si no lo sabes, dilo.
- **Nada destructivo** sin que yo lo apruebe.
- **Ningún envío masivo** sin que yo vea la prueba y dé el OK.
- **Busca antes de leer:** `python3 ~/cerebro/bin/buscar.py "tu pregunta"`
- **Antes de proponer algo nuevo, revisa si ya existe.**
- Háblame en español mexicano.

## Si el cerebro contradice lo que ves
El cerebro es una foto del momento en que se escribió. **Verifica contra la realidad antes
de afirmar** — y si estaba mal, corrígelo en el mismo turno. Una memoria equivocada
envenena a todas las sesiones que la lean después.
````

Luego actívalo para todas sus sesiones. **Si ya existía un `~/.claude/CLAUDE.md`, respáldalo primero y avísale:**

```bash
mkdir -p ~/.claude
[ -f ~/.claude/CLAUDE.md ] && cp ~/.claude/CLAUDE.md ~/.claude/CLAUDE.md.respaldo
cp ~/cerebro/CLAUDE.md ~/.claude/CLAUDE.md
```

---

## FASE 7 — La constelación inicial

Explícale: *"un cerebro vacío no se ve como nada. Te dejo las primeras notas ya conectadas entre sí."*

Crea estos cuatro archivos. **Fíjate en los `[[enlaces]]`: eso es lo que va a dibujar la red.**

`~/cerebro/INICIO.md` (pon su nombre):
```markdown
# 🧠 El cerebro de NOMBRE

Ésta es la puerta de entrada. Todo lo que mi IA aprende vive aquí y queda conectado.

## Cómo está organizado
- [[como-funciona-mi-cerebro]] — qué pasa solo y qué escribe mi IA
- [[mi-negocio]] — los hechos duraderos de lo que hago
- [[mis-reglas]] — cómo quiero que mi IA trabaje conmigo

## Lo que estoy construyendo
_Conforme construya cosas, aquí van a aparecer enlaces a mis proyectos._
```

`~/cerebro/como-funciona-mi-cerebro.md`:
```markdown
# Cómo funciona mi cerebro

Vuelve a [[INICIO]] · Ver también [[mis-reglas]]

## Lo que pasa solo
- **Al arrancar** una sesión, mi IA lee este cerebro y despierta sabiendo dónde me quedé.
- **Al terminar** cada turno, guarda la conversación.

## Lo que NO pasa solo
El transcripto se guarda solo. **El conocimiento no.** Eso se lo pido a mi IA:
lo que aprendí va a `memoria/` — como [[mi-negocio]].

## La regla que hace la red
Cada nota nueva enlaza a las que se le parecen, con dobles corchetes.
Sin enlaces tengo archivos sueltos. Con enlaces tengo un cerebro.
```

`~/cerebro/memoria/mi-negocio.md`:
```markdown
# Mi negocio

Vuelve a [[INICIO]] · Ver también [[mis-reglas]]

_Nota semilla. Bórrala y escribe la tuya._

Aquí van los hechos que mi IA debe saber siempre, sin que se los repita:

- **A qué me dedico:**
- **Qué vendo y en cuánto:**
- **Quién es mi cliente:**
- **Mis márgenes y mis costos:**
- **Mis proveedores clave:**
```

`~/cerebro/memoria/mis-reglas.md`:
```markdown
# Mis reglas

Vuelve a [[INICIO]] · Ver también [[mi-negocio]] · [[como-funciona-mi-cerebro]]

Cómo quiero que mi IA trabaje conmigo:

- Háblame en español mexicano.
- **Nunca inventes datos.** Si no lo sabes, dilo.
- **Verifica el resultado, no la bandera.**
- Nada destructivo sin que yo lo apruebe.
- Antes de proponer algo nuevo, revisa si ya existe.

_Cuando corrija a mi IA en algo, que agregue la corrección aquí._
```

---

## FASE 8 — Conectar los hooks

Explícale: *"esto es lo que hace que todo lo anterior se prenda solo."*

Corre este script con el Python que funcionó en la Fase 1:

```python
import json
from pathlib import Path
import shutil
import sys

home = Path.home()
f = home / ".claude" / "settings.json"
BIN = home / "cerebro" / "bin"
py = sys.executable

d = {}
if f.exists():
    try:
        d = json.loads(f.read_text(encoding="utf-8"))
        shutil.copy(f, f.with_suffix(".json.respaldo"))
    except Exception:
        d = {}

hooks = d.setdefault("hooks", {})

def ya_esta(evento, fragmento):
    return any(fragmento in json.dumps(m) for m in hooks.get(evento, []))

if not ya_esta("SessionStart", "inicio.py"):
    hooks.setdefault("SessionStart", []).append(
        {"hooks": [{"type": "command",
                    "command": '"%s" "%s"' % (py, BIN / "inicio.py"),
                    "timeout": 20}]})

if not ya_esta("Stop", "guardar.py"):
    hooks.setdefault("Stop", []).append(
        {"hooks": [{"type": "command",
                    "command": '"%s" "%s"' % (py, BIN / "guardar.py"),
                    "timeout": 20}]})

f.parent.mkdir(parents=True, exist_ok=True)
f.write_text(json.dumps(d, indent=2, ensure_ascii=False), encoding="utf-8")
json.loads(f.read_text(encoding="utf-8"))
print("✅ 2 hooks conectados (arranque · guardado)")
```

> **Este script se puede correr dos veces sin romper nada** — checa si el hook ya existe antes de agregarlo, y respalda el settings.json anterior.

---

## FASE 9 — Que se vea como red (Obsidian)

Obsidian es una app gratis que lee carpetas de archivos de texto y **dibuja las conexiones entre ellos como una red**. Es la forma de VER el cerebro que acabas de construir.

Esta fase tiene cuatro partes. **No brinques ninguna** — si se queda sin instalar, se pierde la mejor parte.

### 9.1 — Primero, la configuración

Créala antes de instalar nada, para que cuando abra la app ya esté todo con los colores correctos.

`~/cerebro/.obsidian/appearance.json`:
```json
{ "theme": "obsidian", "accentColor": "#F2A03D", "baseFontSize": 16 }
```

Y `~/cerebro/.obsidian/graph.json` — los colores del programa: **ámbar lo vivo, cian la estructura**:
```json
{
  "collapse-filter": false,
  "search": "",
  "showTags": true,
  "showAttachments": false,
  "hideUnresolved": false,
  "showOrphans": true,
  "collapse-color-groups": false,
  "colorGroups": [
    { "query": "path:memoria",    "color": { "a": 1, "rgb": 15900733 } },
    { "query": "path:proyectos",  "color": { "a": 1, "rgb": 4180446  } },
    { "query": "path:pendientes", "color": { "a": 1, "rgb": 9149868  } }
  ],
  "collapse-display": false,
  "showArrow": true,
  "textFadeMultiplier": -0.4,
  "nodeSizeMultiplier": 1.5,
  "lineSizeMultiplier": 1.3,
  "collapse-forces": false,
  "centerStrength": 0.42,
  "repelStrength": 12,
  "linkStrength": 0.85,
  "linkDistance": 210,
  "scale": 1,
  "close": false
}
```

### 9.2 — ¿Ya lo tiene instalado?

**Mac:**
```bash
ls -d /Applications/Obsidian.app 2>/dev/null || ls -d ~/Applications/Obsidian.app 2>/dev/null || echo "NO ESTÁ"
```

**Windows (PowerShell):**
```powershell
if (Test-Path "$env:LOCALAPPDATA\Obsidian\Obsidian.exe") { "YA ESTÁ" } else { "NO ESTÁ" }
```

**Linux:**
```bash
command -v obsidian || ls ~/Applications/*.AppImage 2>/dev/null || echo "NO ESTÁ"
```

Si ya está → salta a **9.4**. Si no → **9.3**.

### 9.3 — Instalarlo

**Pregúntale antes de instalar.** Dile qué es, que es gratis y que pesa ~100 MB.

**Mac — checa si tiene Homebrew:**
```bash
command -v brew
```

- **Si hay brew** (lo más rápido, tú lo haces por ella):
  ```bash
  brew install --cask obsidian
  ```
  Tarda 1-2 minutos. Avísale que está bajando.

- **Si NO hay brew:** no intentes instalar brew nada más para esto — es un rodeo largo. Mándala a bajarlo a mano:
  > 1. Ve a **https://obsidian.md** y dale al botón grande de descarga.
  > 2. Se baja un archivo `.dmg`. Ábrelo con doble clic.
  > 3. Se abre una ventana con el ícono de Obsidian y una flecha hacia una carpeta que dice *Applications*. **Arrastra el ícono a esa carpeta.**
  > 4. Ya está instalado. Puedes cerrar esa ventana.

  Espera a que te confirme antes de seguir.

**Windows — checa si tiene winget:**
```powershell
winget --version
```

- **Si hay winget:**
  ```powershell
  winget install Obsidian.Obsidian
  ```
- **Si no:** que lo baje de **https://obsidian.md**, corra el `.exe` y le dé Siguiente a todo.

**Linux:** que baje el AppImage de https://obsidian.md, le dé permisos de ejecución (`chmod +x`) y lo abra con doble clic.

**Verifica que sí quedó** volviendo a correr el comando de 9.2 antes de seguir.

### 9.4 — Abrirlo y conectarle el cerebro

Ábrelo tú:

```bash
# Mac
open -a Obsidian

# Windows (PowerShell)
Start-Process "$env:LOCALAPPDATA\Obsidian\Obsidian.exe"
```

**Estos siguientes clics los da ella, no tú** — es una app con ventanas, no puedes manejarla desde la terminal. Dáselos de uno en uno, no todos juntos:

> 1. En la pantalla de bienvenida busca la opción que dice **"Open folder as vault"** (en español, *"Abrir carpeta como almacén"*).
> 2. Se abre el explorador de archivos. Navega a tu carpeta personal y elige la carpeta **`cerebro`**. No entres a ninguna subcarpeta — selecciona `cerebro` tal cual y dale Abrir.
> 3. Si te pregunta si confías en los autores de la carpeta, dile que sí — es tu propia carpeta.

Si no encuentra la carpeta `cerebro` en el explorador, dile la ruta exacta para que la pegue:
- Mac / Linux: `~/cerebro`
- Windows: `C:\Users\SU_USUARIO\cerebro`

### 9.5 — El grafo 🕸️

> 4. Del lado izquierdo, en la barra de íconos, busca el que parece **tres puntos conectados por líneas** (se llama *Graph view* / *Vista de grafo*). Dale clic.

**Ahí está su cerebro.** Cada punto es una nota, cada línea es un enlace.

Pregúntale **qué está viendo** y confírmale lo que significa:
- Los puntos **ámbar** son su memoria — lo que aprendió para siempre.
- Los **cian** van a ser sus proyectos, conforme construya.
- Puede arrastrar los puntos, hacer zoom, y darle clic a uno para abrir la nota.

Si no ve nada o ve la ventana vacía, casi siempre es que eligió la carpeta equivocada en el paso 2. Que le dé a **Open another vault** y vuelva a intentar apuntando a `cerebro`.

---

## FASE 10 — El historial

```bash
cd ~/cerebro
git init -q
printf '__pycache__/\n*.pyc\n.DS_Store\n' > .gitignore
git add -A
git commit -q -m "cerebro instalado"
```

Si git se queja de que falta identidad, configúrasela dentro del cerebro nada más:
```bash
git -C ~/cerebro config user.name "SU NOMBRE"
git -C ~/cerebro config user.email "cerebro@local"
```

Explícale para qué sirve: *"ahora cada cambio de tu cerebro queda guardado con fecha. Si algo se rompe, puedes regresar en el tiempo."*

---

## FASE 11 — La prueba 🧠

**Éste es el momento.** No lo apures. Dile exactamente esto:

```
Ya quedó. Ahora vamos a probarlo:

1. Dime UN dato de tu negocio que quieras que yo recuerde para siempre.
   (un precio, una regla, cómo funciona algo tuyo)
2. Yo lo voy a guardar en tu memoria.
3. CIERRA esta sesión por completo.
4. Ábrela otra vez y pregúntame por ese dato.
```

Cuando te dé el dato, **guárdalo de verdad** en `~/cerebro/memoria/` como un archivo nuevo, con un `[[enlace]]` a `[[INICIO]]` y a `[[mi-negocio]]`. Commitea. Y confírmale que ya quedó guardado.

Luego dile que cierre la sesión. **En serio, que la cierre.**

---

## Cierre — cuéntale qué acaba de lograr

```
🎉 Ya tienes cerebro.

Vive en ~/cerebro

📂 proyectos/   dónde te quedaste en cada cosa
📔 memoria/     lo que aprendiste, para siempre
📥 pendientes/  lo que dejaste a medias

── Ya lo puedes VER ──
Obsidian quedó conectado a tu cerebro. Cada vez que quieras ver
tu red, ábrelo y dale al ícono del grafo.

── Para buscar dentro ──
python3 ~/cerebro/bin/buscar.py "tu pregunta"

── Lo más importante ──
Ya no me tienes que repetir las cosas. Dime "guarda esto en mi
memoria" cuando algo valga la pena, y ahí va a estar mañana.
```

Y una última cosa que sí vale la pena que le digas:

> *"Hoy tu red tiene un puñado de puntos. Eso está bien: acaba de nacer. Lo que la hace valiosa no es lo que tiene hoy, es lo que va a acumular. Cada conversación que tengamos le suma un nodo y una línea más."*

---

_AI Business Builder · Día 1 · Tu cerebro_
