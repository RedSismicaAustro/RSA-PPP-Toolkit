---
name: desplegar_repositorio_ppp
description: "Guía interactiva para desplegar, inicializar y estandarizar un repositorio de prácticas preprofesionales en RSA-PPP a partir de un archivo de planificación."
---

# Skill: Desplegar Repositorio PPP

Esta habilidad automatiza y estandariza la creación e inicialización de un nuevo repositorio de Prácticas Preprofesionales (PPP) bajo la organización [`RSA-PPP`](https://github.com/RSA-PPP), partiendo de una planificación técnica en `planificaciones/`.

---

## ⚙️ Configuración y Variables de Entorno

Antes de operar, define o verifica la ruta raíz de trabajo local para los repositorios de estudiantes. Si no se especifica otra ruta, utiliza el valor por defecto:

```text
RUTA_BASE_PPP = "C:\Users\miltonrsa\Documents\git\ppp"
ORGANIZACION_GITHUB = "RSA-PPP"
HOST_SSH_RSA = "git@github.com-rsa"
```

> 💡 **Portabilidad:** Si el usuario indica que cambió de equipo o de ruta de trabajo, solicita o actualiza `RUTA_BASE_PPP`.

---

## 🎯 Disparadores (Triggers)

La habilidad se activa cuando el usuario solicita expresiones como:
- *"despliega el repositorio para la planificación [archivo/código]"*
- *"inicializa el repositorio de pasantía a partir de [planificacion-N.md]"*
- *"crea el repositorio para la planificación de [Nombre Estudiante]"*

---

## 📋 Flujo de Ejecución Paso a Paso

El proceso consta de **4 Fases interactivas**. El agente debe acompañar al usuario y esperar confirmación en los checkpoints clave.

```text
+-----------------------+     +-----------------------+
|        FASE 1         |     |        FASE 2         |
|  Creación en GitHub   | --> | Configuración About   |
|   y Clonación Local   |     |    y Topics en Web    |
+-----------------------+     +-----------------------+
            |                             |
            v                             v
+-----------------------+     +-----------------------+
|        FASE 3         |     |        FASE 4         |
| Generación Estructura | --> |  Comandos de Commit   |
|    y Renderizado      |     |  y Push a origin/main |
+-----------------------+     +-----------------------+
```

---

### 🔹 Fase 1: Creación del Repositorio en GitHub y Clonación

1. **Lectura de la Planificación:**
   - Leer el archivo fuente en `planificaciones/` indicado por el usuario (ej. `planificaciones/planificacion-12.md`).
   - Extraer metadatos del frontmatter YAML:
     - `codigo_proyecto`: (ej. `RSA-PPP-2026-12`)
     - `titulo`: Nombre descriptivo.
     - `pasante.nombre`: Nombre del estudiante.
     - `proyecto_precedente`: Código y enlace si es proyecto continuo.
     - `topics`: Lista canónica de etiquetas.
     - `tecnologias`: Stack tecnológico para el `.gitignore`.

2. **Cálculo del Nombre Canónico del Repositorio:**
   - Formato estricto: `ppp-YYYY-NN-[tema-kebab-case]` (ej. `ppp-2026-12-multiplexor-galgas-presa`).

3. **Presentar Instrucciones de Creación Web al Usuario:**
   Mostrar una ficha clara:
   - **Organización:** `RSA-PPP` (acceder a `https://github.com/organizations/RSA-PPP/repositories/new`).
   - **Repository name:** `[nombre-calculado]`
   - **Description:** `[Sintetizar el objetivo principal del proyecto]`
   - **Public / Private:** Según la política institucional (habitualmente Público).
   - ⚠️ **Importante:** **NO marcar** "Add a README file", **NO marcar** ".gitignore", **NO marcar** "Choose a license". El repositorio debe nacer completamente vacío.

4. **Entrega del Comando de Clonación:**
   Generar el comando PowerShell exacto usando el alias SSH institucional:
   ```powershell
   git clone git@github.com-rsa:RSA-PPP/[nombre-calculado].git [RUTA_BASE_PPP]\[nombre-calculado]
   ```

5. 🛑 **Checkpoint 1 (Esperar confirmación):**
   Detenerse y pedir al usuario que ejecute la creación y clonación:
   > *"Por favor crea el repositorio en GitHub con estos parámetros y ejecute el comando de clonación. Cuando esté listo, avísame para continuar con la configuración del About y la generación de archivos."*

---

### 🔹 Fase 2: Configuración del About y Topics en GitHub

Una vez que el usuario confirma que el repositorio está creado y clonado:

1. **Entrega de Metadatos para el About en GitHub Web:**
   Instruir al usuario para hacer clic en el engranaje ⚙️ (sección **About** en la esquina superior derecha del repo en GitHub):
   - **Description:** `[Texto conciso de 1 línea sobre el proyecto]`
   - **Website:** `https://redsismicaaustro.github.io/RSA-Metodologias`
   - **Include in the home page:** Marcar *Releases* y *Packages* según corresponda.
   - **Topics:** Proporcionar la lista limpia de topics extraída de la planificación:
     ```text
     [topic-1] [topic-2] [topic-3] ...
     ```

2. *(Alternativa opcional por comando):* Si el usuario prefiere asignar los topics mediante PowerShell REST API, proveer el bloque:
   ```powershell
   # Opcional vía PowerShell (requiere GITHUB_TOKEN si el repo es privado o para evitar rate limits)
   $body = @{ names = @("topic-1", "topic-2", "topic-3") } | ConvertTo-Json
   Invoke-RestMethod -Uri "https://api.github.com/repos/RSA-PPP/[nombre-repo]/topics" -Method Put -Headers @{ Authorization = "Bearer $env:GITHUB_TOKEN"; Accept = "application/vnd.github+json" } -Body $body
   ```

---

### 🔹 Fase 3: Generación de la Estructura Local de Archivos (Autónoma)

El agente procede a escribir de forma autónoma en el directorio local `[RUTA_BASE_PPP]\[nombre-calculado]`:

1. **Creación del Árbol de Directorios Inmutable:**
   - Crear las siguientes carpetas incorporando un archivo `.gitkeep` en aquellas que comiencen vacías:
     ```text
     data/
     ├── evidence/
     └── raw_samples/
     docs/
     ├── hardware/
     └── troubleshooting/
     firmware/ (o software/)
     ├── common/
     ├── drivers/
     ├── [modulo_principal]/
     └── tests/
     python/ (o herramientas_pc/)
     ```

2. **Generación del `.gitignore` Especializado:**
   Construir el archivo `.gitignore` adaptado al stack de `tecnologias`:
   - Reglas generales de SO: Windows (`Thumbs.db`, `desktop.ini`), macOS (`.DS_Store`), VS Code (`.vscode/`), JetBrains.
   - Reglas Python: `__pycache__/`, `*.py[cod]`, `.venv/`, `env/`.
   - Si usa **MikroC PRO**: temporales de compilación (`*.dci`, `*.asm`, `*.lst`, `*.dic`, `*.log`, `*.cp`, `*.ini`, `*.dbg`).
   - Si usa **MPLAB X / XC8 / XC16**: carpetas `build/`, `dist/`, `.generated_files/`.
   - Si usa **PlatformIO**: carpetas `.pio/`.
   - Si usa **KiCad**: temporales `*-save.kicad_pcb`, `*.bak`, `_autosave-*`.

3. **Renderizado de `docs/planificacion.md`:**
   - Copiar el contenido de la planificación fuente a `docs/planificacion.md` dentro del repo del estudiante.
   - Asegurarse de que el campo `repositorio.url` apunte exactamente al nuevo repositorio en `RSA-PPP`.

4. **Generación de `README.md` a partir de la Plantilla:**
   - Leer `plantillas/plantilla_readme.md`.
   - Rellenar la ficha técnica con los datos del pasante, tutor, duración, código de proyecto y objetivos.
   - Inyectar el árbol de directorios con el nombre exacto del proyecto.
   - Si el proyecto tiene un predecesor, descomentar el aviso institucional de **Proyecto Precedente**.

5. **Generación del Documento Puente (si aplica):**
   - Si `proyecto_precedente` está definido y no es `N/A`:
     - Leer `plantillas/plantilla_documento_puente.md`.
     - Generar `docs/referencia_proyecto_anterior.md` con los enlaces y datos del proyecto anterior.

---

### 🔹 Fase 4: Entrega de Comandos Git para Despliegue

1. El agente revisa el estado local del repositorio (`git status`).
2. Proporciona los comandos exactos para que el usuario suba el primer commit limpio a la rama `main` (cumpliendo con la regla de minúsculas `commits.md`):

```powershell
cd [RUTA_BASE_PPP]\[nombre-calculado]
git add .
git commit -m "feat: inicializar estructura de repositorio, planificacion y documentacion base"
git push origin main
```

3. Confirmar que el repositorio está en estado **listo y operativo para que el estudiante comience sus prácticas**.
