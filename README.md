# RSA-PPP-Toolkit

> **Repositorio de Gobernanza, Estandarización y Memoria Técnica de Prácticas Preprofesionales (PPP)**  
> **Red Sísmica del Austro (RSA) — Universidad de Cuenca**

Este repositorio centraliza las directrices metodológicas, plantillas operativas, catálogo de proyectos históricos y herramientas asistidas por IA para la formulación, ejecución, transición técnica y cierre de las **Prácticas Preprofesionales (PPP)** de la Red Sísmica del Austro.

---

## 🏛️ Propósito y Rol en el Ecosistema RSA

El conocimiento y desarrollo tecnológico en la RSA se gestiona a través de una arquitectura federada:

```text
               +-------------------------------------------+
               |           ORGANIZACIÓN CENTRAL            |
               |         github.com/Red-Sismica-del-Austro |
               +-------------------------------------------+
                   |                     |              |
                   v                     v              v
        [RSA-Agent-Toolkit]    [RSA-Metodologias]   [RSA-PPP-Toolkit]
        (Skills y Reglas IA)   (Índice y ADRs)      (Gobernanza PPP)
                                                        |
                                                        | Directrices,
                                                        | plantillas y
                                                        | seguimiento
                                                        v
                                       +-------------------------------+
                                       |      ORGANIZACIÓN PPP         |
                                       |         github.com/RSA-PPP    |
                                       +-------------------------------+
                                            • ppp-2026-09-shm-...
                                            • ppp-2026-11-shm-...
                                            • ppp-2026-12-galgas-...
                                            • ... (Repositorios alumnos)
```

1. **`RSA-PPP-Toolkit` (Este repositorio):** Vive en la organización matriz institucional. Contiene las plantillas maestras (`plantillas/`), la compilación histórica de las 144 horas de cada estudiante (`planificaciones/`) y los lineamientos de control de calidad.
2. **`RSA-PPP` (Organización de Prácticas):** Aloja los repositorios individuales donde cada estudiante desarrolla su código, firmware o diseños de PCB (`github.com/RSA-PPP/[nombre-del-repo]`).

---

## 📂 Estructura del Repositorio

```text
RSA-PPP-Toolkit/
├── planificaciones/           # Planes de trabajo oficiales (144h) de todos los pasantes
│   ├── planificacion-1.md     # Henry Castro (RSA-PPP-2023-01)
│   ├── planificacion-2.md     # Jhonatan Cambisaca (RSA-PPP-2024-02)
│   ├── ...
│   ├── planificacion-9.md     # David Timbi (RSA-PPP-2026-09)
│   ├── planificacion-10.md    # Franklin Andrade (RSA-PPP-2026-10)
│   ├── planificacion-11.md    # Geovanny Cullquicondo (RSA-PPP-2026-11)
│   ├── planificacion-12.md    # Christopher Carchipulla (RSA-PPP-2026-12)
│   └── planificacion-13.md    # Walter Calderón (RSA-PPP-2026-13)
│
├── plantillas/                # Plantillas maestras normalizadas
│   ├── planificaciones.md     # Plan de trabajo con frontmatter YAML, fases y checkpoints
│   ├── plantilla_readme.md    # README.md estándar para repositorios en RSA-PPP
│   └── plantilla_documento_puente.md # Guía para transiciones entre proyectos continuos
│
├── docs/                      # Guías y políticas para tutores y pasantes
│   └── taxonomia_topics.md    # Vocabulario controlado de topics para About en GitHub
│
├── .agents/skills/            # Habilidades y agentes para formulación asistida de PPP
│   └── desplegar_repositorio_ppp/ # Skill para crear repositorios PPP a partir de planificaciones
│
└── README.md                  # Este documento
```

---

## 📋 Catálogo Histórico de Proyectos PPP

| Código | Proyecto / Tema | Pasante | Estado |
| :---: | :--- | :--- | :---: |
| `RSA-PPP-2023-01` | Sistema de Adquisición, Monitoreo IoT y Procesamiento de Sensores Estructurales (Presa Chanlud) | Henry Castro | Culminado |
| `RSA-PPP-2024-02` | Diseño Detallado de PCBs para Red SHM (Concentrador y Nodos Sensores Daisy Chain) | Jhonatan Cambisaca | Culminado |
| `RSA-PPP-2024-03` | Optimización de Almacenamiento en MicroSD y Firmware para Monitoreo de Puentes | Henry Maldonado | Culminado |
| `RSA-PPP-2024-04` | Migración de Acelerógrafo a ESP32 y Telemetría MQTT de Aceleraciones | Jorge Zhangallimbay | Culminado |
| `RSA-PPP-2025-05` | Diseño de PCBs en Autodesk EAGLE para Acelerógrafo ESP32 (Modular y SMD) | Daniel Loja | Culminado |
| `RSA-PPP-2025-06` | Dashboard en Tiempo Real y Telemetría IoT para Estaciones RSA (Stack TIG-MQTT) | Mauro Bravo | Culminado |
| `RSA-PPP-2026-07` | Migración de Proyectos EDA de Altium a KiCad 10 y Estandarización PCBA (JLCPCB) | Joel Suárez | Culminado |
| `RSA-PPP-2026-08` | Migración, Auditoría y Optimización de PCB de Acelerógrafo ESP32 en KiCad 10 | Joel Suárez | Culminado |
| `RSA-PPP-2026-09` | Ensamblaje, Programación, Daisy Chain y PoC MicroSD para Red SHM V1.4 | David Timbi | Culminado |
| `RSA-PPP-2026-10` | Sensor Ultrasónico de Nivel de Alta Precisión (dsPIC a ESP32 con MicroPython) | Franklin Andrade | En Ejecución |
| `RSA-PPP-2026-11` | Validación Integral de Nodos Sensores para Red SHM (Doble Búfer, ADXL355 y Co-localización) | Geovanny Cullquicondo | En Ejecución |
| `RSA-PPP-2026-12` | Sistema DAQ de 16 Canales para Sensores Geotécnicos de Presa (NI USB-6210 y Python) | Christopher Carchipulla | En Ejecución |
| `RSA-PPP-2026-13` | Adquisición Electromecánica Multiplexada de 24 Canales para Galgas (Presa Chanlud) | Walter Calderón | En Ejecución |

---

## 🛠️ Guía de Uso de Plantillas

Al iniciar o preparar una nueva práctica preprofesional:

### 1. Formulación del Plan de Trabajo
* Copiar `plantillas/planificaciones.md` a `planificaciones/planificacion-[N].md` en este repositorio y al directorio `docs/planificacion.md` en el repositorio del estudiante.
* Completar el frontmatter YAML y desglosar las horas (típicamente 144 h) en fases semanales con checkpoints cuantificables (`CP X.Y`).

### 2. Creación del Repositorio en `RSA-PPP`
* Nombrar el repositorio con el formato: `ppp-YYYY-NN-[tema-en-kebab-case]`.
  - Ejemplo: `ppp-2026-11-shm-nodos-sensores`.
* Usar `plantillas/plantilla_readme.md` como base para el `README.md` del nuevo repositorio.
* Mantener la **estructura de directorios inmutable**:
  - `docs/`: Planificación, hardware, troubleshooting.
  - `firmware/` (o `software/`): Código fuente con drivers desacoplados.
  - `data/`: Registros binarios, muestras y evidencias.
  - `python/` (o herramientas PC): Scripts de soporte y análisis.

### 3. Empalme Técnico entre Pasantes (Proyectos Continuos)
* Si el proyecto es la continuación de una pasantía previa, crear en el repositorio del estudiante el archivo `docs/referencia_proyecto_anterior.md` utilizando `plantillas/plantilla_documento_puente.md`.
* Documentar claramente la **causa raíz** del problema técnico heredado para evitar que el nuevo pasante repita errores previos.

---

## 🛡️ Políticas y Convenciones de Desarrollo

1. **Rama de Trabajo:** En proyectos de pasantías individuales, el estudiante trabaja directamente en la rama **`main`**. Se descarta el uso de GitFlow o `develop` para evitar fricción operativa.
2. **Formato de Commits:** Todos los commits deben ser atómicos y en minúsculas:
   - `feat:` Nuevas funcionalidades, librerías o controladores.
   - `fix:` Correcciones de bugs o errores en circuito/código.
   - `docs:` Actualización de documentación, esquemáticos o bitácora.
   - `test:` Pruebas unitarias o experimentales.
3. **No contaminar con archivos temporales:** Asegurar un `.gitignore` estricto que excluya archivos temporales de compiladores (MikroC, MPLAB X, KiCad) y datasets masivos.

---

## 📞 Contacto y Administración

* **Tutor Institucional:** Ing. Milton Muñoz (`milton.munozc@ucuenca.edu.ec`)  
* **Organización Principal:** [Red Sísmica del Austro (RSA)](https://github.com/Red-Sismica-del-Austro)  
* **Organización de Prácticas:** [RSA-PPP](https://github.com/RSA-PPP)
