# Obsidian-Agent – Guía

Versión 2.0.0 · [Deutsch](README.de.md) · [English](README.en.md) · [Français](README.fr.md) · [Italiano](README.it.md) · [Română](README.ro.md)

El Obsidian-Agent es una interfaz local para la gestión de proyectos con Obsidian. Se abre en el navegador en `http://127.0.0.1:5055`, pero solo en su ordenador; no se envía ningún dato a Internet.

**Principio:** los archivos Markdown de su almacén de Obsidian son la única fuente de verdad. El agente los lee, muestra tareas, proyectos y reuniones de forma clara y crea notas nuevas en el formato correcto. La base de datos `planer.db` es solo un índice y se reconstruye en cada sincronización, así que puede seguir trabajando directamente en Obsidian en cualquier momento.

## Instalación

### Windows

1. Haga doble clic en **`Obsidian-Agent-Setup.exe`**.
   Windows puede mostrar «Windows protegió su PC» porque el programa no está firmado: **Más información → Ejecutar de todas formas**.
2. Elija el idioma y confirme con Intro.
3. El instalador copia el programa en `%LOCALAPPDATA%\Programs\Obsidian-Agent` y lo configura:
   - Si falta Python 3.10 o posterior, se instala para su cuenta con `winget` (si no, se abre python.org).
   - Un cuadro de diálogo pide la carpeta del almacén.
   - A partir de ahora la interfaz web se inicia en segundo plano en cada inicio de sesión; la sincronización diaria se ejecuta a las 04:00.
   - **Obsidian-Agent** aparece en el menú Inicio, y una entrada de desinstalación en *Configuración → Aplicaciones*.

No se necesitan derechos de administrador. Volver a ejecutar el instalador actualiza el programa; `config.json` y `planer.db` se conservan.

### macOS

1. Haga doble clic en **`Obsidian-Agent.pkg`**.
   El paquete no está notarizado por Apple. Si macOS indica que «no se puede abrir»: **clic derecho → Abrir → Abrir** (a partir de macOS 15: *Ajustes del Sistema → Privacidad y seguridad → Abrir igualmente*).
2. El paquete se instala en `~/Applications/Obsidian-Agent` (solo para usted, sin contraseña de administrador).
3. Después se abre una ventana de Terminal: elegir idioma, elegir almacén, inicio al iniciar sesión sí/no. La sincronización diaria se configura con launchd y se recupera cuando el Mac se despierta.

**Requisito:** Python 3.9 o posterior, p. ej. desde python.org o con `brew install python`.

Si el almacén está en *Documentos*, en el *Escritorio* o en *iCloud Drive*, macOS puede bloquear la sincronización en segundo plano. La configuración lo detecta e indica qué archivo añadir en *Ajustes del Sistema → Privacidad y seguridad → Acceso total al disco*.

Inicio: doble clic en `~/Applications/Obsidian-Agent/Obsidian-Agent.command` o abra el marcador `http://127.0.0.1:5055`.

### Linux

```bash
./start.sh                       # crear el entorno de Python e iniciar
.venv/bin/python agent.py init --sprache es
```
Sincronización diaria con `crontab -e`: `0 4 * * * cd /ruta/obsidian-agent && .venv/bin/python agent.py sync --geplant`

## Secciones

| Sección | Para qué sirve |
|---|---|
| **Hoy** | Tareas que vencen hoy y vencidas, los próximos días, las próximas reuniones periódicas. Crear una tarea directamente. |
| **Tareas** | Todas las tareas por Hoy / Vencidas / Planificadas / Sin fecha / Hechas, filtro por proyecto. Marcar, cambiar estado, «+1 día». |
| **Proyectos** | Proyectos con cliente, jefe de proyecto, fecha límite y número de tareas abiertas. Nuevo proyecto con estructura de carpetas fija. |
| **Actas** | Acta de reunión, decisión y solicitud de cambio a partir de plantillas; con un proyecto se guardan en la carpeta correcta. |
| **Reuniones periódicas** | Reuniones recurrentes (p. ej. equipo de proyecto, comité de dirección) con frecuencia y puntos abiertos. Cada nueva nota de reunión incluye los puntos abiertos de la serie. |
| **Equipo** | Fichas con función, contacto y tareas abiertas. Asignación mediante el campo `verantwortlich` (nombre o iniciales). |
| **Búsqueda** | Búsqueda de texto completo en todo el almacén. |
| **Ajustes** | Idioma, ruta del almacén, nombres de carpetas, hora de la sincronización diaria. |

Cada título es un enlace `obsidian://` que abre la nota directamente en Obsidian.

## Carpetas del almacén

En la primera configuración las carpetas se crean en su idioma:

| Uso | Carpeta |
|---|---|
| Proyectos | `Proyectos/` |
| Tareas sin proyecto | `Tareas/` |
| Actas sin proyecto | `Actas/` |
| Miembros del equipo | `Equipo/` |
| Reuniones periódicas | `Reuniones/<serie>/` |
| Plantillas | `_Plantillas/` |
| Resumen diario | `Agenda/Hoy.md` |

Cada proyecto recibe estas subcarpetas (cada una con un archivo readme) y una nota de resumen `_Proyecto_<nombre>.md` con cliente, jefe de proyecto y fecha límite:

- `01_Encargo_y_contratos/` – Encargo del proyecto, oferta, contratos, pedidos y adendas.
- `02_Planificacion/` – Plan del proyecto, hitos, recursos, presupuesto y riesgos.
- `03_Contactos/` – Interlocutores, funciones, partes interesadas y vías de escalado.
- `04_Actas/` – Actas de reuniones, talleres y correspondencia importante.
- `05_Decisiones/` – Decisiones y solicitudes de cambio.
- `06_Informes/` – Informes de estado, documentos del comité de dirección e informe final.
- `07_Documentos/` – Documentación técnica, conceptos y entregables.

Si cambia el idioma más adelante, las carpetas y notas existentes **no** se renombran. Puede ajustar los nombres de las carpetas en cualquier momento en *Ajustes*.

## Formato de las tareas

```markdown
---
typ: task
titel: Preparar la prueba de aceptación
projekt: Implantación ERP
start: 2026-11-18
ende: 2026-11-20
status: offen
prioritaet: hoch
verantwortlich: AB
---
- [ ] Acordar los casos de prueba
- [x] Pedir el entorno de pruebas
```

- **Los nombres de campo y los valores son iguales en todos los idiomas** (`status: offen | in_arbeit | wartend | erledigt`, `prioritaet: hoch | mittel | niedrig`). La interfaz los muestra traducidos. Así un almacén sigue siendo legible aunque lo usen personas con idiomas distintos.
- Hoy = `start ≤ hoy ≤ ende`, vencida = `ende < hoy`, planificada = `start > hoy`.
- `projekt` es el nombre de la carpeta del proyecto como texto simple, `verantwortlich` un nombre o iniciales del equipo (varios separados por comas).
- El agente no reconoce tareas al estilo del plugin Tasks (`- [ ] … #task 📅 …`). Todo lo que deba seguirse se convierte en un archivo de tarea propio.

## Resumen diario

Cada mañana a las 04:00 (o en el siguiente inicio si el ordenador estaba apagado) el agente lee el almacén, guarda una instantánea diaria en la base de datos y escribe `Agenda/Hoy.md` y un archivo. Así puede ver el resumen también en Obsidian en el móvil.

## Adaptar las plantillas

Las plantillas están en `_Plantillas/` y se editan en Obsidian; el agente usa después su versión. Las plantillas eliminadas se vuelven a crear en la siguiente sincronización. Marcadores como `{{title}}` y `{{date}}` también funcionan con el plugin básico «Plantillas» de Obsidian.

## Desinstalación, actualización, traslado

- **Windows:** *Configuración → Aplicaciones → Obsidian-Agent → Desinstalar* o `deinstallieren.bat` en la carpeta del programa.
- **macOS:** `~/Applications/Obsidian-Agent/mac/deinstallieren_mac.command`.

La desinstalación elimina el inicio automático y la sincronización diaria, guarda `config.json` y `planer.db` en una carpeta `obsidian-agent-backup_<fecha>` en el escritorio y, si lo desea, borra la carpeta del programa. **El almacén nunca se modifica.**

Actualización: instale simplemente el nuevo instalador o paquete. Traslado a otro ordenador: instálelo allí y copie `config.json` y `planer.db` de la copia de seguridad en la carpeta del programa (la base de datos es opcional; sin ella solo falta el historial diario).

## Protección de datos

El almacén contiene datos personales e información confidencial de proyectos. El agente funciona solo en local (`127.0.0.1`) y no envía nada al exterior. Configure una copia de seguridad del almacén en una unidad segura de la empresa y use solo almacenamiento en la nube autorizado.

## Línea de comandos

```
python agent.py sync               # leer el almacén ahora
python agent.py heute              # tareas de hoy y vencidas
python agent.py projekt "Nombre"   # crear un proyecto
python agent.py init --sprache es  # definir el idioma (de, en, fr, it, es, ro)
```
