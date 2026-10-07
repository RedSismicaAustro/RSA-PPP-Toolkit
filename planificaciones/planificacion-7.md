---
titulo: "Plan de Trabajo de Prácticas Preprofesionales: Migración de Proyectos EDA Institucionales de Altium Designer a KiCad y Estandarización PCBA (JLCPCB)"
proyecto: "Migración de EDA (Altium a KiCad) y Estandarización para Ensamblaje Automatizado PCBA"
codigo_proyecto: "RSA-PPP-2026-07"
area_tematica: "Diseño de Hardware Electrónico, CAD/EDA Libre (KiCad 10), Automatización de Librerías (jlc2kicadlib) y Manufactura PCBA"
estado: "Culminado"
version: "1.0"
fecha_creacion: "2026-04-20"
fecha_actualizacion: "2026-06-30"

pasante:
  nombre: "Edgar Joel Suárez Jaigua"
  cedula: "N/D"
  correo: "edgar.suarez@ucuenca.edu.ec"
  carrera: "Ingeniería en Telecomunicaciones / Electrónica"
  institucion: "Universidad de Cuenca"

tutoria:
  tutor_institucional: "Ing. Milton Muñoz. (Red Sísmica del Austro - RSA)"
  correo_tutor_institucional: "milton.munozc@ucuenca.edu.ec"

cronograma:
  duracion_total_horas: 144
  dedicacion_semanal_horas: 14
  duracion_semanas: 10.5
  fecha_inicio: "2026-04-20"
  fecha_fin_estimada: "2026-06-30"
  modalidad: "Presencial"

tecnologias:
  - "Software de Diseño Electrónico EDA: KiCad 10 (Eeschema, PCB Editor, Gestor de Paquetes PCM)"
  - "Entorno de Automatización y Scripts: Python 3 (entorno virtual), jlc2kicadlib (descarga automatizada de símbolos, huellas y modelos 3D STEP)"
  - "Plugins y Herramientas PCBA: Fabrication Toolkit (JLCPCB Tools) para generación automatizada de Gerbers, BOM y CPL"
  - "Estandarización de Componentes: Catálogo de componentes LCSC / JLCPCB (priorización de Basic Parts y Preferred Parts)"
  - "Fabricación y Verificación Industrial: Visor Web de JLCPCB para auditoría de offsets, rotaciones de componentes y verificación DRC"
  - "Control de Versiones y Gestión de Librerías: Git, GitHub, librerías locales portables en `/libs` con direccionamiento relativo (${KIPRJMOD})"

repositorio:
  url: "https://github.com/RedSismicaAustro/RSA-Intern-SHM"
  rama_base: "main"
---

---

## 1. Antecedentes y Justificación Técnica

1. **Contexto del Sistema y Estado Inicial del Artefacto:** 
   La Red Sísmica del Austro (RSA) históricamente ha desarrollado sus circuitos impresos (PCBs) para instrumentación geotécnica y monitorización de salud estructural (SHM) utilizando plataformas de software privativo como Altium Designer. Aunque estos diseños se encuentran validados funcionalmente en campo, el modelo privativo genera dependencia tecnológica de licencias comerciales, fragmentación en los flujos de trabajo colaborativos y dificultades para la reproducibilidad de los proyectos por parte de nuevos estudiantes e investigadores. Además, la mayoría de los diseños heredados empleaban componentes *through-hole* (THT) o librerías de componentes dispersas sin vinculación directa a catálogos de manufactura electrónica moderna.

2. **Limitaciones Críticas o Cuellos de Botella Identificados:** 
   * **Dependencia de Herramientas Privativas:** La imposibilidad de abrir, editar y auditar los archivos `.PcbDoc` y `.SchDoc` en entornos de código abierto limitaba la democratización del conocimiento técnico y la integración con pipelines continuos de hardware.
   * **Incompatibilidad con Ensamblaje Automatizado (PCBA):** El ensamble manual de componentes en laboratorio resulta lento, costoso y propenso a defectos de soldadura. Los proyectos originales carecían de asignación de códigos de parte de fabricante (LCSC Part Numbers), archivos de coordenadas de colocación (*Component Placement List* - CPL) y marcas fiduciarias para máquinas *Pick & Place*.
   * **Rompimiento de Librerías y Rutas Absolutas:** Al transferir proyectos entre estaciones de trabajo, las huellas y modelos 3D solían desvincularse debido a dependencias con rutas absolutas locales de la máquina del diseñador original.
   * **Desalineación de Rotaciones en Fabricación:** La exportación tradicional de archivos CPL produce desfasajes comunes de $90^\circ$ o $180^\circ$ en la orientación de componentes polares (diodos, circuitos integrados, conectores) en el visor del fabricante.

3. **Desacoplamiento del Alcance y Propuesta de Solución:** 
   Esta pasantía de **144 horas** establece el flujo de trabajo estándar institucional de migración hacia **KiCad 10**, ejecutando la migración completa de dos proyectos institucionales de SHM:
   * **Proyecto 1:** Migración, conversión tecnológica a componentes de montaje superficial (SMD 0805/0603), gestión de librerías locales aisladas con `jlc2kicadlib`, ruteo optimizado, marcas fiduciarias y generación automatizada de archivos de fabricación con el plugin *Fabrication Toolkit*.
   * **Proyecto 2:** Migración de un segundo circuito de mayor complejidad topológica aplicando el flujo estandarizado y validado en la primera etapa.
   * **Estandarización y Guía Técnica:** Elaboración de un manual operativo exhaustivo que garantice la autonomía del repositorio (rutas relativas `${KIPRJMOD}`), la correcta asignación de componentes del catálogo LCSC y la validación en el visor web de JLCPCB.

---

## 2. Objetivos del Plan de Trabajo

### 2.1. Objetivo General
Migrar dos proyectos electrónicos institucionales de monitoreo de salud estructural (SHM) desde Altium Designer hacia el entorno de software libre KiCad 10, modernizando los circuitos a tecnología de montaje superficial (SMD), estandarizando librerías locales portables con `jlc2kicadlib` y configurando el plugin *Fabrication Toolkit* para garantizar compatibilidad total con el servicio de ensamblaje automatizado (PCBA) de JLCPCB.

### 2.2. Objetivos Específicos
1. **[Configuración de Entorno e Infraestructura - 6 h]:** Desplegar la estación de trabajo en KiCad 10, configurando un entorno virtual de Python con la herramienta `jlc2kicadlib`, instalando el complemento *Fabrication Toolkit* desde el PCM y configurando el repositorio Git con control de archivos temporales (`.gitignore`).
2. **[Importación y Reestructuración de Altium - 6 h]:** Importar nativamente los archivos esquemáticos y de PCB del Proyecto 1 en KiCad 10, depurando jerarquías, corrigiendo conversiones en etiquetas de red globales y verificando la integridad topológica del cobre.
3. **[Transición Tecnológica a SMD - 10 h]:** Catologar y sustituir componentes pasivos through-hole (THT) por encapsulados SMD estandarizados (0805 y 0603) acordes a los requerimientos de potencia y voltaje, sincronizando la placa con los nuevos componentes.
4. **[Gestión de Librerías y Asignación LCSC - 14 h]:** Inyectar masivamente el campo `LCSC` en el esquemático, asignar números de parte priorizando *Basic Parts*, descargar automáticamente símbolos, huellas y modelos 3D STEP mediante `jlc2kicadlib` y alojarlos en la carpeta local `/libs` con rutas relativas.
5. **[Optimización y Documentación de Proyecto 1 - 24 h]:** Refinar el ruteo de pistas, robustecer planos de masa, incorporar marcas fiduciarias (Fiducials), verificar modelos mecánicos 3D, generar archivos de fabricación (Gerber, Drill, BOM, CPL) y corregir desfases de rotación en el visor web de JLCPCB.
6. **[Migración de Proyecto 2 - 48 h]:** Ejecutar el ciclo completo de migración (importación, transición a SMD, asignación del 100% de partes LCSC, ruteo y DRC) para un segundo diseño institucional de mayor densidad y complejidad.
7. **[Validación Final y Guía Técnica de Migración - 36 h]:** Auditar comparativamente los esquemas originales frente a las versiones migradas, redactar el informe técnico final y elaborar el `README.md` operativo que instruya a los ingenieros de la RSA sobre la reproducibilidad del entorno.

---

## 3. Plan de Trabajo Detallado y Checkpoints Cuantificables

### Fase 1: Configuración del Entorno e Infraestructura de Trabajo
* **Duración:** 6 horas (Semana 1)
* **Objetivo:** Instalar y estandarizar las herramientas de software libre y automatización necesarias para el flujo de diseño KiCad-PCBA.
* **Actividades técnicas:**
  1. Instalar y parametrizar la suite KiCad versión 10 en la estación de trabajo.
  2. Crear un entorno virtual en Python 3 e instalar la herramienta de automatización CLI `jlc2kicadlib` para descarga directa de componentes desde la base de datos de LCSC.
  3. Instalar y configurar el complemento *Fabrication Toolkit* (JLCPCB Tools) a través del Gestor de Paquetes y Contenidos (PCM) de KiCad.
  4. Clonar el repositorio [RSA-Intern-SHM](https://github.com/RedSismicaAustro/RSA-Intern-SHM), crear la rama de desarrollo y configurar el archivo `.gitignore` adaptado para excluir cachés y temporizadores de KiCad (`*-save.kicad_pcb`, `*.kicad_prl`).
* **Checkpoints medibles para revisión:**
  - [ ] **CP 1.1:** Entorno KiCad 10 y entorno virtual de Python con `jlc2kicadlib` verificados y operativos por consola.
  - [ ] **CP 1.2:** Plugin *Fabrication Toolkit* integrado en la barra de herramientas del editor de PCBs.
  - [ ] **CP 1.3:** Repositorio Git inicializado con rama de trabajo y `.gitignore` normalizado.

---

### Fase 2: Importación de Altium y Reestructuración de Proyecto 1
* **Duración:** 6 horas (Semana 1)
* **Objetivo:** Migrar los esquemáticos y archivos de placa originales de Altium Designer hacia KiCad depurando discrepancias de conversión.
* **Actividades técnicas:**
  1. Utilizar el importador nativo de Altium integrado en KiCad 10 para cargar los esquemas `.SchDoc` y layouts `.PcbDoc` del Proyecto 1.
  2. Depurar la estructura jerárquica: corregir discrepancias en etiquetas de red globales (*global labels*), buses de comunicación y puertos de alimentación (VCC, 3.3V, GND).
  3. Inspección visual y topológica del PCB importado: verificar la correspondencia geométrica de pistas de cobre, vías de interconexión y polígonos de relleno de masa respecto al diseño original.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 2.1:** Archivos esquemáticos de Altium importados a formato `.kicad_sch` sin errores de sintaxis.
  - [ ] **CP 2.2:** Layout de PCB cargado en `.kicad_pcb` conservando dimensiones mecánicas y topología de cobre original.

---

### Fase 3: Transición Tecnológica a SMD y Ajuste Esquemático
* **Duración:** 10 horas (Semana 2)
* **Objetivo:** Modernizar el circuito reemplazando componentes voluminosos THT por encapsulados de montaje superficial aptos para producción automatizada.
* **Actividades técnicas:**
  1. Elaborar el catastro de componentes pasivos básicos (resistencias, condensadores de desacoplo, diodos de protección) implementados en tecnología through-hole.
  2. Estandarizar encapsulados SMD: seleccionar huellas industriales (0805 para líneas de potencia o señales críticas y 0603 para desacoplos generales) verificando disipación de potencia y voltajes de ruptura.
  3. Sustituir los símbolos en el esquemático de KiCad, actualizando designadores, valores paramétricos y tolerancias.
  4. Ejecutar la sincronización entre esquemático y PCB (*Update PCB from Schematic* - F8) para importar los nuevos footprints al espacio de trabajo.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 3.1:** 100% de pasivos tradicionales THT sustituidos por huellas estandarizadas SMD 0805/0603.
  - [ ] **CP 3.2:** Sincronización esquemático-PCB completada con nuevos footprints posicionados en la placa.

---

### Fase 4: Gestión de Librerías y Asignación para PCBA (jlc2kicadlib)
* **Duración:** 14 horas (Semanas 2 y 3)
* **Objetivo:** Vincular los componentes con la base de datos de JLCPCB/LCSC y construir la librería local portable del proyecto.
* **Actividades técnicas:**
  1. Inyectar masivamente el campo de metadatos `LCSC` en todos los símbolos del esquemático mediante el editor de campos de símbolos de KiCad.
  2. Búsqueda y catalogación de números de parte en el catálogo web de LCSC/JLCPCB, priorizando componentes de inventario *Basic Parts* para eliminar recargos de calibración de alimentadores SMT.
  3. Ejecutar scripts automatizados con `jlc2kicadlib` para descargar símbolos (`.kicad_sym`), huellas (`.kicad_mod`) y modelos 3D (`.step`) de circuitos integrados y conectores que no forman parte de las librerías nativas.
  4. Estructurar la carpeta `/libs` en la raíz del repositorio y vincular las tablas de librerías locales mediante rutas relativas `${KIPRJMOD}/libs/...`.
  5. Auditar huellas descargadas: verificar correspondencia física del Pin 1 y orientación espacial correcta de los modelos 3D STEP.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 4.1:** 100% de los componentes del esquemático con código de inventario LCSC asignado.
  - [ ] **CP 4.2:** Carpeta `/libs` creada en el repositorio conteniendo todos los símbolos, huellas y modelos 3D locales.
  - [ ] **CP 4.3:** Primer commit atómico en Git conteniendo esquemático, placa y activos locales portables.

---

### Fase 5: Optimización del Ruteo, Verificación 3D y Documentación de Proyecto 1
* **Duración:** 24 horas (Semanas 4 y 5)
* **Objetivo:** Refinar el diseño físico del Proyecto 1, validar el ensamble 3D y generar los paquetes automatizados de producción PCBA.
* **Actividades técnicas:**
  1. Rutar las nuevas huellas SMD respetando criterios de integridad de señal: trazas de alimentación robustas, desacoplos capacitivos adyacentes a pines VCC y vertido de planos de masa sólidos (GND).
  2. Incorporar al menos 3 marcas fiduciarias (*Fiducials*) en las esquinas de la PCB para calibración óptica de las máquinas *Pick & Place*.
  3. Verificar el ensamble mecánico en el visor 3D de KiCad, confirmando la orientación del Pin 1 en circuitos integrados y polaridad en diodos/condensadores.
  4. Generar mediante el plugin *Fabrication Toolkit* el paquete de fabricación: archivos Gerber RS-274X, taladrado NC Drill, Lista de Materiales (`BOM.csv`) y archivo de posiciones de componentes (`CPL.csv`).
  5. Cargar los archivos en el visor web de JLCPCB y validar la alineación de componentes; corregir desfases angulares ($90^\circ, 180^\circ, 270^\circ$) ajustando los offsets en la configuración del plugin.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 5.1:** DRC de KiCad aprobado con 0 violaciones de aislamiento y ruteo completado al 100%.
  - [ ] **CP 5.2:** Ensamble 3D auditado sin colisiones de componentes ni desfasajes de footprint.
  - [ ] **CP 5.3:** Paquete de producción PCBA validado en el visor web de JLCPCB con alineación de partes al 100%.

---

### Fase 6: Migración y Estandarización de Proyecto 2 (Diseño Complejo)
* **Duración:** 48 horas (Semanas 6 a 8)
* **Objetivo:** Aplicar la metodología validada para migrar un segundo circuito institucional de mayor densidad y complejidad.
* **Actividades técnicas:**
  1. Importar los archivos esquemáticos y layout de Altium Designer del Proyecto 2 a KiCad 10.
  2. Resolver dependencias jerárquicas complejas, buses multicanal y puertos de alimentación.
  3. Actualizar y estandarizar la totalidad de componentes a variantes SMD según disponibilidad en catálogo LCSC.
  4. Asignar los números de parte LCSC a la totalidad de símbolos y descargar los activos requeridos en `/libs` mediante `jlc2kicadlib`.
  5. Ejecutar el rediseño y ruteo de pistas en PCB, integrando planos de tierra mallados, marcas fiduciarias y optimización de retornos de corriente.
  6. Ejecutar la verificación de reglas de diseño (DRC) exhaustiva y generar el paquete PCBA completo con *Fabrication Toolkit*.
  7. Validar offsets y orientación de componentes en el visor industrial de JLCPCB.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 6.1:** Proyecto 2 importado, jerarquía depurada y componentes asignados con códigos LCSC al 100%.
  - [ ] **CP 6.2:** PCB del Proyecto 2 ruteada y aprobada con 0 errores de DRC.
  - [ ] **CP 6.3:** Paquete PCBA del Proyecto 2 exportado y validado en la plataforma de JLCPCB.

---

### Fase 7: Auditoría Comparativa, Guía de Migración e Informe Final
* **Duración:** 36 horas (Semanas 9 y 10.5)
* **Objetivo:** Auditar la equivalencia eléctrica entre los diseños originales y migrados, consolidar el repositorio y redactar la documentación técnica.
* **Actividades técnicas:**
  1. Ejecutar una auditoría comparativa exhaustiva entre los esquemas originales de Altium y las versiones finales en KiCad 10 (continuidad netlist, asignación de pines y tolerancias).
  2. Redactar el archivo `README.md` operativo en la raíz del repositorio [RSA-Intern-SHM](https://github.com/RedSismicaAustro/RSA-Intern-SHM) detallando:
     - Versión de KiCad requerida (v10).
     - Guía paso a paso de instalación del entorno virtual y uso de `jlc2kicadlib`.
     - Instrucciones de uso del plugin *Fabrication Toolkit* para regenerar paquetes PCBA con un solo clic.
     - Convenciones para asignación de códigos LCSC y gestión de librerías locales aisladas.
  3. Redactar la Guía Técnica y el Informe Final de Prácticas Preprofesionales documentando la metodología, comparativas antes/después y lecciones aprendidas.
  4. Revisión técnica con el tutor institucional y suscripción de actas de finalización.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 7.1:** Auditoría eléctrica y topológica completada certificando 100% de equivalencia funcional.
  - [ ] **CP 7.2:** `README.md` operativo y repositorios autónomos clonables en cualquier máquina sin dependencias rotas.
  - [ ] **CP 7.3:** Informe Técnico Final formal de prácticas aprobado por el tutor institucional de la RSA.

---

## 4. Resumen de Fases y Cronograma Semanal (144 Horas)

El trabajo se ejecutó en **10.5 semanas** (del 20 de abril al 30 de junio de 2026) con una dedicación promedio de **14 horas semanales**:

| Fase | Semanas | Horas | Enfoque Principal |
| :---: | :---: | :---: | :--- |
| **Fase 1** | Semana 1 | **6 h** | Configuración de KiCad 10, entorno Python, `jlc2kicadlib`, Fabrication Toolkit y Git. |
| **Fase 2** | Semana 1 | **6 h** | Importación nativa de Altium a KiCad de Proyecto 1 y resolución de etiquetas de red. |
| **Fase 3** | Semana 2 | **10 h** | Catastro de pasivos THT, transición a SMD (0805/0603) y sincronización con PCB. |
| **Fase 4** | Semanas 2–3 | **14 h** | Inyección de metadatos LCSC (Basic Parts), descarga automatizada con `jlc2kicadlib` a `/libs`. |
| **Fase 5** | Semanas 4–5 | **24 h** | Ruteo optimizado, marcas fiduciarias, ensamble 3D, exportación PCBA y corrección en JLCPCB. |
| **Fase 6** | Semanas 6–8 | **48 h** | Migración completa de Proyecto 2 (diseño complejo): importación, SMD, LCSC, ruteo y DRC. |
| **Fase 7** | Semanas 9–10.5 | **36 h** | Auditoría comparativa, `README.md` operativo, paquetes de release e Informe Final. |
| **Total** | **~10.5 Semanas** | **144 h** | **Planificación Global de Prácticas Preprofesionales** |

---

## 5. Entregables y Criterios de Éxito de la Pasantía

1. **Repositorio Autónomo y Portable en KiCad 10:**
   * Proyectos 1 y 2 completos y funcionales en formato KiCad 10 alojados en: [RSA-Intern-SHM](https://github.com/RedSismicaAustro/RSA-Intern-SHM).
   * Directorio `/libs` autónomo conteniendo símbolos (`.kicad_sym`), huellas (`.kicad_mod`) y modelos 3D (`.step`) con rutas relativas basadas en `${KIPRJMOD}`.
2. **Paquetes de Fabricación Automatizada (Production Release):**
   * Carpetas de fabricación para ambos proyectos generadas con *Fabrication Toolkit* conteniendo Gerbers RS-274X, archivos de taladrado NC Drill, Lista de Materiales (`BOM.csv`) y archivo de colocación de componentes (`CPL.csv`).
   * Rotaciones y offsets de componentes SMD 100% calibrados y validados en el visor de JLCPCB.
3. **Guía Técnica y README Operativo:**
   * Archivo `README.md` exhaustivo en la raíz del repositorio con instrucciones de instalación de herramientas, flujo de asignación de partes LCSC y regeneración de manufactura.
4. **Informe Técnico Final:**
   * Documento formal de validación y certificación de la migración tecnológica, acreditando las 144 horas de prácticas preprofesionales suscritas por el tutor de la RSA.
