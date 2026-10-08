# Guía para Agentes de IA en RSA-PPP-Toolkit

Este repositorio es el **centro de gobernanza, estandarización y memoria técnica de las Prácticas Preprofesionales (PPP)** de la Red Sísmica del Austro (RSA) y la Universidad de Cuenca.

Como agente de IA, lee este archivo para comprender la arquitectura de proyectos de pasantías, las normas de creación de repositorios y cómo utilizar las plantillas y habilidades disponibles.

---

## 🏛️ Arquitectura del Ecosistema PPP

El desarrollo tecnológico de pasantías se distribuye entre dos organizaciones de GitHub:

| Ámbito | Organización / Repositorio | Propósito |
| :--- | :--- | :--- |
| **Gobernanza Institucional** | `Red-Sismica-del-Austro/RSA-PPP-Toolkit` (Este repo) | Plantillas maestras, planes de trabajo históricos (144h), taxonomía de topics y skills de tutoría. |
| **Repositorios de Estudiantes** | `RSA-PPP/ppp-YYYY-NN-[tema]` | Repositorios individuales donde cada pasante desarrolla su firmware, software o PCBs. |

---

## 📂 Estructura del Repositorio

```text
RSA-PPP-Toolkit/
├── planificaciones/           # Planes de trabajo oficiales (144h) de pasantes (planificacion-N.md)
├── plantillas/                # Plantillas maestras institucionales
│   ├── planificaciones.md     # Plantilla de plan de trabajo (frontmatter YAML con topics y checkpoints)
│   ├── plantilla_readme.md    # Plantilla de README.md para repositorios en RSA-PPP
│   └── plantilla_documento_puente.md # Plantilla para proyectos que continúan una fase previa
├── docs/                      # Políticas y directrices
│   └── taxonomia_topics.md    # Vocabulario controlado obligatorio para About en GitHub
├── .agents/skills/            # Habilidades asistidas por IA para la gestión de PPP
│   └── desplegar_repositorio_ppp/ # Skill para inicializar repositorios de pasantía
├── AGENTS.md                  # Este documento
└── README.md                  # Visión general y catálogo histórico
```

---

## ⚙️ Habilidades y Flujos de Trabajo (Skills)

Las habilidades específicas de este repositorio se ubican en `.agents/skills/`.

| Skill | Activación típica | Descripción |
| :--- | :--- | :--- |
| `desplegar_repositorio_ppp` | *"despliega el repositorio para la planificación [N]"* o *"inicializa el repositorio a partir de [archivo]"* | Flujo guiado en 4 fases para crear en GitHub, configurar About con topics oficiales, generar estructura inmutable de carpetas, renderizar plantillas y preparar el commit inicial. |

---

## 🛡️ Reglas de Comportamiento Críticas para este Repositorio

1. **Nomenclatura Canónica:**
   - Todo repositorio de pasante en `RSA-PPP` debe seguir el formato: `ppp-YYYY-NN-[tema-kebab-case]`.
   - El código institucional siempre sigue el formato: `RSA-PPP-YYYY-NN` (donde `NN` es el número correlativo de proyecto).
2. **Estructura Raíz Inmutable en Repositorios de Alumnos:**
   - La estructura raíz de los repositorios de estudiantes (`data/`, `docs/`, `firmware/` o `software/`, `python/`) es inmutable y no debe alterarse ni renombrarse.
3. **Rama Principal Única:**
   - En repositorios de pasantías individuales en `RSA-PPP`, la rama de trabajo es **`main`**. No se crean ramas `develop` ni flujos GitFlow que agreguen fricción operativa al estudiante.
4. **Taxonomía de Topics Obligatoria:**
   - Los topics de GitHub deben seleccionarse estrictamente desde [`docs/taxonomia_topics.md`](docs/taxonomia_topics.md).
5. **Formato de Commits:**
   - No ejecutes commits de forma autónoma. Muestra los comandos para que el usuario los ejecute.
   - Mensajes de commit siempre en minúsculas: `tipo: descripción` (ej. `feat: ...`, `docs: ...`, `fix: ...`).
6. **Idioma:**
   - Toda la interacción, documentación, comentarios y actas deben redactarse en español.

---

## 🔗 Repositorios Relacionados

- **Toolkit General de Agentes:** [RSA-Agent-Toolkit](https://github.com/Red-Sismica-del-Austro/RSA-Agent-Toolkit)
- **Metodologías y Exocortex:** [RSA-Metodologias](https://github.com/Red-Sismica-del-Austro/RSA-Metodologias)
- **Organización de Prácticas:** [RSA-PPP](https://github.com/RSA-PPP)
