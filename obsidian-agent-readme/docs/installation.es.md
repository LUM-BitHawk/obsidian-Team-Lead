# Obsidian-Agent – Guía rápida de instalación

[← Volver a la lista de guías](README.md) · [Guía completa](README.es.md)

[![Los cinco pasos del asistente de instalación (esquema)](screenshots/Setup-Assistent_Vorschau.png)](screenshots/Setup-Assistent_Vorschau.png)
<sub>Los cinco pasos del asistente de instalación (esquema)</sub>

## 🪟 Windows

1. **Descargar y descomprimir el ZIP** – Descargue `Obsidian-Agent-Setup-2.0.0-Windows.zip`, clic derecho → *Extraer todo* y doble clic en **`Obsidian-Agent-Setup.exe`**. Si aparece «Windows protegió su PC»: *Más información* → *Ejecutar de todas formas*. **No** use «Ejecutar como administrador»; de lo contrario se instala para la cuenta de administrador.
2. **Elegir idioma** – Deutsch, English, Français, Italiano, Español o Română. Se preselecciona el idioma de Windows.
3. **Bienvenida** – Confirme con *Siguiente*. Si el agente ya está instalado, se actualiza; los ajustes y la base de datos se conservan.
4. **Carpeta y opciones** – Se propone `%LOCALAPPDATA%\Programs\Obsidian-Agent` (sin derechos de administrador). Opciones: inicio al iniciar sesión, acceso directo en el menú Inicio, acceso directo en el escritorio.
5. **Elegir el almacén de Obsidian** – Con *Examinar …* elija la carpeta de su almacén o marque «Definir más tarde en Ajustes». Después, *Instalar*.
6. **Instalación** – La barra muestra el avance y *Ver detalles* el registro. Si falta Python, se instala automáticamente (requiere Internet).
7. **Terminar** – «Abrir Obsidian-Agent» inicia la interfaz en el navegador y «Mostrar la guía» abre la guía en su idioma.

## 🍎 macOS

1. Descargue `Obsidian-Agent-Mac.zip`, descomprímalo y haga doble clic en **`Obsidian-Agent.pkg`**. La primera vez: clic derecho → *Abrir* → *Abrir*.
2. El paquete se instala en `~/Applications/Obsidian-Agent`. Después se abre una ventana de Terminal: elija idioma, almacén e inicio automático.
3. Requisito: Python 3.9 o posterior (python.org).

## Después

La interfaz funciona en **http://127.0.0.1:5055**; guárdela como marcador. Registro de la instalación: `setup.log` en la carpeta del programa.
