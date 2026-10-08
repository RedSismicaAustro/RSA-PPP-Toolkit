---
titulo: "Plan de Trabajo de Prácticas Preprofesionales: [Nombre Corto y Descriptivo del Tema]"
proyecto: "[Nombre Formal Completo del Proyecto o Iniciativa Institucional]"
codigo_proyecto: "[Código oficial asignado, ej. RSA-PPP-2026-12]"
# Si el proyecto es continuación de una pasantía previa, indicar el código y URL; en caso contrario colocar "N/A"
proyecto_precedente:
  codigo: "[ej. RSA-PPP-2026-09 o N/A]"
  url: "[ej. https://github.com/RSA-PPP/ppp-2026-09-... o N/A]"
area_tematica: "[ej. Sistemas Embebidos, Instrumentación Sísmica, Redes y Telecomunicaciones, Desarrollo de Software]"
estado: "Borrador" # Opciones: [Borrador / En Revisión / Aprobado / En Ejecución / Culminado]
version: "1.0"
fecha_creacion: "YYYY-MM-DD"
fecha_actualizacion: "YYYY-MM-DD"

practicante:
  nombre: "[Nombres y Apellidos del Practicante]"
  cedula: "[Número de Cédula o Identificación]"
  correo: "[correo.estudiante@ucuenca.edu.ec]"
  carrera: "[Ingeniería en Telecomunicaciones]"
  institucion: "Universidad de Cuenca"

tutoria:
  tutor_institucional: "Ing. Milton Muñoz (Red Sísmica del Austro - RSA)"
  correo_tutor_institucional: "milton.munozc@ucuenca.edu.ec"

cronograma:
  duracion_total_horas: 144 # Valor numérico entero según convenio académico (típicamente 96 o 144)
  dedicacion_semanal_horas: 14 # Horas estimadas por semana
  duracion_semanas: 10.5 # Semanas calendario estimadas
  fecha_inicio: "YYYY-MM-DD"
  fecha_fin_estimada: "YYYY-MM-DD"
  modalidad: "Presencial" # Opciones: [Presencial / Híbrida / Remota]

tecnologias:
  - "[Hardware / Microcontroladores / SBCs: ej. ESP32, dsPIC33, STM32, Raspberry Pi]"
  - "[Lenguajes de Programación: ej. C, C++, Python, Bash]"
  - "[Entornos de Desarrollo e IDEs: ej. MPLAB X, PlatformIO, VS Code, MikroC PRO]"
  - "[Protocolos y Buses: ej. SPI, I2C, UART, RS485, CAN, MQTT, TCP/IP]"
  - "[Bases de Datos y Monitorización: ej. InfluxDB, Grafana, Telegraf, Docker]"
  - "[Instrumental y Software de Medición: ej. Osciloscopio, Multímetro, Analizador Lógico, HxD]"

repositorio:
  url: "https://github.com/RSA-PPP/[nombre-repositorio]"
  rama_base: "main" # En proyectos individuales de RSA-PPP la rama de trabajo directo es siempre main

# Topics oficiales del repositorio (About en GitHub)
# Seguir el vocabulario controlado de docs/taxonomia_topics.md (kebab-case, minúsculas, sin espacios)
topics:
  - rsa-ppp # Obligatorio
  - ucuenca # Obligatorio
  - "[dominio: ej. shm / seismic-monitoring / dam-monitoring / iot / daq]"
  - "[hardware: ej. dspic33 / esp32 / ni-usb-6210 / raspberry-pi]"
  - "[sensor-periferico: ej. adxl355 / microsd / ultrasonic-sensor / strain-gauges]"
  - "[protocolo-arquitectura: ej. rs485 / daisy-chain / spi / mqtt / ping-pong-buffer]"
  - "[software-entorno: ej. python / c / micropython / mikroc / kicad / grafana]"
---

---

## 1. Antecedentes y Justificación Técnica

Describir el contexto técnico del sistema, los desarrollos previos y la necesidad de esta práctica preprofesional:

1. **Contexto del Sistema y Estado Inicial del Artefacto:** 
   * Detallar el estado actual del hardware, firmware o software al inicio de la práctica (ej. versiones de PCB fabricadas, librerías disponibles, arquitectura de red preliminar, pruebas de concepto previas).
   * Identificar los subsistemas operativos y el punto de partida técnico.
   * *Si el proyecto es continuación de una pasantía previa:* Mencionar explícitamente el proyecto predecesor (código `RSA-PPP-YYYY-NN`, autor y repositorio) y referenciar el documento de transición `docs/referencia_proyecto_anterior.md`.
2. **Limitaciones Críticas o Cuellos de Botella Identificados:** 
   * Exponer la problemática técnica, fallo recurrente, falta de optimización o requerimiento no cubierto en iteraciones anteriores (ej. problemas de latencia, pérdida de muestras, fallos de almacenamiento, falta de sincronismo, incompatibilidad de componentes).
3. **Desacoplamiento del Alcance y Propuesta de Solución:** 
   * Justificar el alcance específico de la práctica preprofesional: delimitar claramente qué módulos o capas se van a intervenir y qué etapas quedan fuera de este ciclo.
   * Explicar el impacto directo de esta solución en la operatividad, fiabilidad y objetivos de la institución/laboratorio.

---

## 2. Objetivos del Plan de Trabajo

### 2.1. Objetivo General
[Verbo en infinitivo: Implementar / Desarrollar / Diseñar / Validar / Caracterizar] [objeto técnico o sistema principal de la práctica preprofesional], integrando [módulos clave, arquitecturas de software/hardware o protocolos], mediante [herramientas, metodologías o tecnologías utilizadas], con el propósito de [impacto técnico, criterio de éxito o finalidad operativa del proyecto].

### 2.2. Objetivos Específicos
1. [Objetivo 1 - Hardware/Entorno]: Adecuar, reparar o configurar la infraestructura de hardware, instrumental o entorno de desarrollo requerido para las pruebas.
2. [Objetivo 2 - Firmware/Módulos de Bajo Nivel]: Desarrollar, optimizar o depurar las rutinas de bajo nivel, controladores de periféricos o librerías de comunicación.
3. [Objetivo 3 - Protocolos / Sincronización]: Implementar el protocolo de intercambio de datos, adquisición o sincronización temporal entre los módulos del sistema.
4. [Objetivo 4 - Gestión de Memoria / Arquitectura]: Diseñar esquemas de búferes, colas de datos o mecanismos de almacenamiento local sin bloqueos ni pérdidas de información.
5. [Objetivo 5 - Integración de Sensores / Actuadores]: Integrar los sensores/transductores finales reemplazando tramas sintéticas por datos físicos reales.
6. [Objetivo 6 - Herramientas de Software en PC / Análisis]: Construir aplicaciones o scripts (ej. en Python) para el volcado, decodificación, visualización y análisis de las variables medidas.
7. [Objetivo 7 - Validación Experimental y Documentación]: Ejecutar ensayos de validación experimental en condiciones controladas/reales y redactar el Informe Técnico Final formal junto con la guía de solución de problemas (*troubleshooting*).

---

## 3. Plan de Trabajo Detallado y Checkpoints Cuantificables

> [!NOTE]
> **Flexibilidad según el perfil del proyecto:**
> Las siguientes 7 fases representan el arquetipo de referencia institucional (habitual en instrumentación y sistemas embebidos: Hardware $\rightarrow$ Drivers $\rightarrow$ Red/Comunicaciones $\rightarrow$ RAM/Buffering $\rightarrow$ Sensores $\rightarrow$ Software PC $\rightarrow$ Cierre).
> - Si el proyecto es de **Desarrollo de Software / Cloud / Backend**: adaptar las fases a (Entorno/Arquitectura $\rightarrow$ Modelos/BD $\rightarrow$ Lógica de Negocio/APIs $\rightarrow$ Pruebas de Carga/Estrés $\rightarrow$ Frontend/Dashboards $\rightarrow$ Integración/CI-CD $\rightarrow$ Documentación).
> - Si el proyecto es de **Telecomunicaciones / Redes**: adaptar las fases a (Acondicionamiento $\rightarrow$ Protocolos/Enlaces $\rightarrow$ Caracterización de Canal/Tráfico $\rightarrow$ Pruebas de Cobertura $\rightarrow$ Monitoreo $\rightarrow$ Validación $\rightarrow$ Documentación).
> - En cualquier caso, **la sumatoria de horas de todas las fases debe coincidir exactamente con `duracion_total_horas`** (ej. 144 h o 96 h). Cada fase debe incluir checkpoints (`CP X.Y`) con criterios de aceptación verificables de forma binaria (Cumple / No Cumple).

### Fase 1: [Nombre de la Fase: ej. Adecuación de Hardware, Entorno y Pruebas Preliminares]
* **Duración:** [X] horas (Semana [N])
* **Objetivo:** [Definir en una frase el propósito técnico y el resultado esperado de esta fase].
* **Actividades técnicas:**
  1. [Actividad 1: Inspección, ensamblaje o acondicionamiento del hardware/software base].
  2. [Actividad 2: Verificación de señales, voltajes, consumo eléctrico o configuración de entorno].
  3. [Actividad 3: Carga de firmwares/scripts de prueba elemental para comprobar conectividad básica].
* **Checkpoints medibles para revisión:**
  - [ ] **CP 1.1:** [Criterio de aceptación 100% verificable, ej. Placas verificadas eléctricamente con voltajes nominales dentro del rango].
  - [ ] **CP 1.2:** [Criterio de aceptación, ej. Firmware básico de diagnóstico operativo respondiendo a estímulos de hardware].
  - [ ] **CP 1.3:** [Evidencia documental o de registro generada, ej. Hoja de caracterización y registro de pruebas iniciales].

---

### Fase 2: [Nombre de la Fase: ej. Controladores de Bajo Nivel y Pruebas Unitarias]
* **Duración:** [X] horas (Semanas [N–M])
* **Objetivo:** [Propósito técnico del desarrollo de bajo nivel o módulos funcionales iniciales].
* **Actividades técnicas:**
  1. [Actividad 1: Desarrollo o adaptación de drivers/librerías para el periférico o módulo principal].
  2. [Actividad 2: Calibración de velocidades de bus, temporizaciones y máquinas de estado].
  3. [Actividad 3: Creación de pruebas unitarias para validar escritura/lectura o envío/recepción de datos].
* **Checkpoints medibles para revisión:**
  - [ ] **CP 2.1:** [Código fuente modular compilado sin advertencias críticas y versionado en el repositorio].
  - [ ] **CP 2.2:** [Test unitario aprobado con 100% de coincidencia entre datos emitidos y recibidos].
  - [ ] **CP 2.3:** [Capturas o registros en analizador/editor de datos que certifiquen la integridad física del protocolo].

---

### Fase 3: [Nombre de la Fase: ej. Comunicación, Protocolo de Red y Transmisión de Datos/Tiempo]
* **Duración:** [X] horas (Semana [N])
* **Objetivo:** [Propósito de la capa de comunicación y sincronización entre dispositivos o servicios].
* **Actividades técnicas:**
  1. [Actividad 1: Definición de la estructura formal de tramas binarias/mensajes (cabecera, payload, CRC)].
  2. [Actividad 2: Implementación de la rutina de transmisión y recepción por interrupciones o DMA].
  3. [Actividad 3: Ensayos de enlace sostenido durante intervalos controlados (ej. 10, 30 y 60 minutos)].
* **Checkpoints medibles para revisión:**
  - [ ] **CP 3.1:** [Protocolo de comunicación operativo con confirmación de tramas válidas mediante indicadores].
  - [ ] **CP 3.2:** [Transmisión/recepción sostenida de paquetes consecutivos sin desbordamiento ni bloqueos].
  - [ ] **CP 3.3:** [Verificación analítica de la continuidad de datos y consistencia temporal en los registros].

---

### Fase 4: [Nombre de la Fase: ej. Arquitectura de Memoria, Doble Búfer y Pruebas de Estrés]
* **Duración:** [X] horas (Semanas [N–M])
* **Objetivo:** [Desacoplamiento temporal de procesos críticos y verificación bajo alta demanda].
* **Actividades técnicas:**
  1. [Actividad 1: Diseño e implementación de estructuras de datos en memoria RAM (Ping-Pong buffers / FIFOs)].
  2. [Actividad 2: Sincronización de eventos de alta prioridad (interrupciones) con tareas de fondo (escritura/envío)].
  3. [Actividad 3: Pruebas de estrés y medición de tiempos de ciclo con instrumental de laboratorio].
* **Checkpoints medibles para revisión:**
  - [ ] **CP 4.1:** [Módulo de gestión de búferes implementado con protecciones ante desbordamiento].
  - [ ] **CP 4.2:** [Prueba de estrés superada: N ciclos/sectores continuos procesados sin pérdidas de eventos].
  - [ ] **CP 4.3:** [Evidencia de integridad de paquetes y determinismo temporal verificada en registros binarios].

---

### Fase 5: [Nombre de la Fase: ej. Integración de Sensores / Módulos Físicos y Registro Real]
* **Duración:** [X] horas (Semanas [N–M])
* **Objetivo:** [Integración de componentes transductores y adquisición de variables físicas reales].
* **Actividades técnicas:**
  1. [Actividad 1: Integración del controlador del sensor/transductor en el firmware principal].
  2. [Actividad 2: Adaptación de la trama de datos para codificar las lecturas físicas en las unidades requeridas].
  3. [Actividad 3: Ensayos en reposo (calibración estática) y ensayos con estímulos dinámicos controlados].
* **Checkpoints medibles para revisión:**
  - [ ] **CP 5.1:** [Sensor/dispositivo plenamente integrado sin conflictos de buses, pines ni interrupciones].
  - [ ] **CP 5.2:** [Registros continuos con respuestas físicas coherentes tanto en condiciones estáticas como dinámicas].

---

### Fase 6: [Nombre de la Fase: ej. Software de Volcado/Análisis en PC y Validación Experimental]
* **Duración:** [X] horas (Semanas [N–M])
* **Objetivo:** [Desarrollo de herramientas de extracción/procesamiento en PC y validación experimental del sistema].
* **Actividades técnicas:**
  1. [Actividad 1: Desarrollo de script/software para volcado, desempaquetado de tramas y exportación estructurada (.csv / .parquet)].
  2. [Actividad 2: Desarrollo de interfaz o scripts de graficación interactiva para inspección de ventanas temporales].
  3. [Actividad 3: Montaje y ejecución de ensayo experimental (ej. co-localización física, pruebas de campo)].
* **Checkpoints medibles para revisión:**
  - [ ] **CP 6.1:** [Herramientas de software en PC funcionales, documentadas y capaces de procesar los datasets del sistema].
  - [ ] **CP 6.2:** [Resultados experimentales validados: correlación de señales y análisis cuantitativo de desempeño].

---

### Fase 7: [Nombre de la Fase: ej. Documentación Técnica, Troubleshooting e Informe Final]
* **Duración:** [X] horas (Semanas [N–M])
* **Objetivo:** [Consolidación de documentación de ingeniería, resolución de incidencias e informe institucional].
* **Actividades técnicas:**
  1. [Actividad 1: Redacción de la Guía de Solución de Problemas (*Troubleshooting*) con fallas típicas y soluciones].
  2. [Actividad 2: Compilación de evidencias experimentales (figuras, gráficos, capturas, esquemáticos y tablas)].
  3. [Actividad 3: Redacción del Informe Técnico Final bajo el formato formal de la RSA y la Universidad].
  4. [Actividad 4: Revisión técnica con el tutor, incorporación de mejoras y cierre del repositorio en GitHub].
* **Checkpoints medibles para revisión:**
  - [ ] **CP 7.1:** [Borrador consolidado del Informe Técnico Final con anexos y guía de troubleshooting listo para revisión].
  - [ ] **CP 7.2:** [Informe aprobado por los tutores, repositorio limpio y documentado, y actas de culminación suscritas].

---

## 4. Resumen de Fases y Cronograma Semanal ([Duración Total] Horas)

El trabajo se organiza en bloques semanales de **[Dedicación Semanal] horas** para facilitar el seguimiento periódico y la evaluación continua mediante checkpoints cuantificables:

| Fase | Semanas | Horas | Enfoque Principal |
| :---: | :---: | :---: | :--- |
| **Fase 1** | Semana 1 | **[X] h** | [Adecuación inicial, diagnóstico de hardware/entorno y verificación eléctrica/funcional] |
| **Fase 2** | Semanas [N–M] | **[X] h** | [Desarrollo de bajo nivel, controladores de periféricos y pruebas unitarias] |
| **Fase 3** | Semana [N] | **[X] h** | [Protocolo de comunicación, transmisión de datos/tiempo y pruebas de enlace] |
| **Fase 4** | Semanas [N–M] | **[X] h** | [Arquitectura de búferes en memoria, pruebas de estrés y almacenamiento/envío robusto] |
| **Fase 5** | Semanas [N–M] | **[X] h** | [Integración de sensores/módulos reales y adquisición continua en condiciones operativas] |
| **Fase 6** | Semanas [N–M] | **[X] h** | [Suite de software en PC (volcado/graficación) y ensayo experimental comparativo] |
| **Fase 7** | Semanas [N–M] | **[X] h** | [Guía de *troubleshooting*, consolidación de anexos y redacción del Informe Final] |
| **Total** | **~[N] Semanas** | **[Total] h** | **Planificación Global de Prácticas Preprofesionales** |

---

## 5. Entregables y Criterios de Éxito de la Práctica Preprofesional

1. **Hardware / Entorno Físico Validado:**
   * [Placas electrónicas, cableado, módulos o instrumental ensamblado, verificado eléctricamente y operativo].
   * [Matriz o reporte de caracterización eléctrica, consumo de corriente o condiciones ambientales].
2. **Firmware / Software Embebido:**
   * Código fuente modular, debidamente estructurado y documentado en el repositorio institucional, incluyendo:
     * [Controladores y drivers de bajo nivel para periféricos clave].
     * [Implementación de protocolos de comunicación y empaquetado de datos].
     * [Mecanismo de buffering, colas o gestión de memoria en tiempo real].
     * [Adquisición y procesamiento de señales de los sensores/transductores integrados].
3. **Software de Soporte y Análisis (PC / Servidor):**
   * [Scripts o herramientas (ej. Python) para la extracción, volcado y conversión de datos brutos a formatos estándar (.csv, .npy, etc.)].
   * [Herramientas de visualización gráfica e inspección temporal/espectral para validación de mediciones].
4. **Evidencia Experimental y Validación:**
   * [Registros técnicos de integridad de datos (capturas de osciloscopio, analizador lógico o visor hexadecimal)].
   * [Gráficas y análisis cuantitativo de ensayos experimentales (co-localización, pruebas de estrés, deriva temporal)].
5. **Informe Técnico Final y Guía de Troubleshooting:**
   * [Informe Técnico Final en formato institucional que compile antecedentes, metodología, arquitectura, resultados y conclusiones].
   * [Guía práctica de resolución de problemas (*troubleshooting*) documentando incidencias encontradas y sus soluciones técnicas].
