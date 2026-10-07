---
titulo: "Plan de Trabajo de Prácticas Laborales: Diseño de Placas de Circuito Impreso (PCB) en Autodesk EAGLE para Acelerógrafo Basado en ESP32 (Versiones Modular y SMD)"
proyecto: "Sistema de Registro Continuo de Señales Acelerométricas basado en ESP32 (Hardware Electrónico y PCBs)"
codigo_proyecto: "RSA-PPP-2025-05"
area_tematica: "Diseño de Hardware Electrónico, CAD/EDA (Autodesk EAGLE), PCBs de Alta Densidad (SMD) e Instrumentación Sísmica"
estado: "Culminado"
version: "1.0"
fecha_creacion: "2025-02-10"
fecha_actualizacion: "2025-03-03"

pasante:
  nombre: "Christian Daniel Loja Chalco"
  cedula: "N/D"
  correo: "christian.loja@ucuenca.edu.ec"
  carrera: "Ingeniería en Telecomunicaciones / Electrónica"
  institucion: "Universidad de Cuenca"

tutoria:
  tutor_institucional: "Ing. Milton Muñoz. (Red Sísmica del Austro - RSA)"
  correo_tutor_institucional: "milton.munozc@ucuenca.edu.ec"

cronograma:
  duracion_total_horas: 96
  dedicacion_semanal_horas: 32
  duracion_semanas: 3
  fecha_inicio: "2025-02-10"
  fecha_fin_estimada: "2025-03-03"
  modalidad: "Presencial"
  etapas_formativas:
    fase_1: "Diseño de la Versión Modular con Breakouts (48 horas - Semanas 1 y 2)"
    fase_2: "Diseño de la Versión Compacta de Montaje Superficial SMD (48 horas - Semanas 2 y 3)"

tecnologias:
  - "Software de Diseño Electrónico EDA: Autodesk EAGLE (Editor de Esquemáticos, Layout PCB, Creación de Librerías y Scripts ERC/DRC)"
  - "Microcontrolador y Módulos de Procesamiento: Módulo ESP32 DevKit V1 (Versión Modular) y SoC ESP32-WROOM-32 / ESP32-D0WD (Versión SMD)"
  - "Sensores de Auscultación: Acelerómetro Triaxial Digital de Bajo Ruido ADXL355 (Módulo Breakout y Encapsulado LGA-14)"
  - "Sistemas de Tiempo y Posicionamiento: Módulo GPS FGPMMOPA6H / SIM28ML (UART) y Reloj en Tiempo Real RTC DS3231 (I2C con TCXO)"
  - "Almacenamiento Local y Potencia: Módulo Zócalo MicroSD (SPI dedicado), Reguladores Buck DC-DC conmutados y LDOs de ultra bajo ruido"
  - "Técnicas de Ruteo y Manufactura: Diseño multicapa/doble cara, planos de masa continuos (GND pour), apantallamiento EMI y generación de archivos Gerber RS-274X / NC Drill"

repositorio:
  url: "https://github.com/RedSismicaAustro/RSA-Intern-HW-Acelerografo_ESP32_Modular"
  repositorio_smd: "https://github.com/RedSismicaAustro/RSA-Intern-HW-Acelerografo_ESP32_SMD"
  rama_base: "main"
---

---

## 1. Antecedentes y Justificación Técnica

1. **Contexto del Sistema y Estado Inicial del Artefacto:** 
   La Red Sísmica del Austro (RSA) completó la validación experimental y la migración lógica del firmware acelerográfico desde el microcontrolador dsPIC hacia la plataforma de 32 bits ESP32, integrando en una arquitectura multihilo (FreeRTOS) el acelerómetro triaxial ADXL355 a 250 Hz, el posicionamiento y estampa satelital GPS, la sincronización por reloj RTC DS3231 y el almacenamiento en tarjetas MicroSD. No obstante, dicha implementación se encontraba enteramente montada sobre protoboards mediante cables jumper, lo cual limitaba severamente la integridad de las señales en buses rápidos (SPI y UART), aumentaba la vulnerabilidad a falsos contactos mecánicos e introducía ruido en las mediciones sismológicas debido a la falta de apantallamiento y desacoplamiento adecuado de alimentación.

2. **Limitaciones Críticas o Cuellos de Botella Identificados:** 
   * **Inestabilidad Mecánica y Ruido de Conexión:** Las pruebas en protoboard son incompatibles con despliegues en campo (edificios, presas y puentes) debido a vibraciones ambientales y pérdidas de contacto en líneas de reloj (SPI SCK) o pulsos de sincronismo (SQW/PPS).
   * **Falta de una Etapa Intermedia de Depuración en Laboratorio:** La transición directa desde un prototipo en protoboard hacia un circuito impreso ultra compacto y miniaturizado (SMD) conlleva un alto riesgo de error en el ruteo de señales si no se dispone previamente de una tarjeta base modular que permita aislar, medir y reemplazar módulos comerciales en caso de fallos.
   * **Volumen Físico y Eficiencia en Despliegues Permanentes:** Los módulos comerciales (*breakouts*) incorporan componentes redundantes, conectores voluminosos y reguladores lineales ineficientes que incrementan el consumo energético y el tamaño del gabinete estanco de instalación.
   * **Integridad de Señal y Disipación Térmica:** La auscultación sísmica exige pisos de ruido mínimos ($\le 25\,\mu g/\sqrt{\text{Hz}}$); las trazas de retorno de masa deben disponer de planos de cobre continuos sin islas flotantes y desacoplo capacitivo estricto cercano a los pines de alimentación analógica del acelerómetro.

3. **Desacoplamiento del Alcance y Propuesta de Solución:** 
   El plan de trabajo intensivo de **96 horas** (desarrollado del 10 de febrero al 03 de marzo de 2025) aborda el diseño electrónico completo en **Autodesk EAGLE** estructurado en dos fases de 48 horas cada una:
   * **Fase 1 (48 Horas - Versión Modular):** Diseñar una PCB basada en sockets y peinetas que aloje módulos estándar comerciales (ESP32 DevKit, ADXL355 breakout, GPS, RTC DS3231, convertidor Buck DC-DC y zócalo MicroSD) para facilitar el montaje manual, diagnóstico por osciloscopio y pruebas de laboratorio.
   * **Fase 2 (48 Horas - Versión SMD de Alta Densidad):** Diseñar una PCB optimizada de montaje superficial donde los módulos se sustituyen por sus circuitos integrados equivalentes a nivel de chip, incorporando reguladores de tensión de bajo ruido, planos de masa para disipación térmica y apantallamiento EMI, puntos de prueba (*test points*) y reducción drástica del área de circuito para fabricación industrial.

---

## 2. Objetivos del Plan de Trabajo

### 2.1. Objetivo General
Diseñar y validar dos placas de circuito impreso (PCB) en Autodesk EAGLE para el sistema de registro continuo de señales acelerométricas basado en ESP32: una primera versión modular basada en módulos comerciales para pruebas y depuración (48 horas), y una segunda versión optimizada de montaje superficial (SMD) para reducción de factor de forma, minimización de ruido y eficiencia operativa en campo (48 horas).

### 2.2. Objetivos Específicos
1. **[Investigación y Arquitectura Modular - 4 h]:** Relevar la arquitectura de interconexión del prototipo en protoboard, documentar los buses de comunicación (SPI, UART, I2C) y seleccionar los módulos estándar compatibles (ESP32 DevKit, ADXL355, RTC, GPS, MicroSD y convertidor Buck).
2. **[Esquemático Modular en EAGLE - 16 h]:** Dibujar el esquema eléctrico unificado en EAGLE integrando todos los bloques funcionales, asignando huellas (*footprints*) normalizadas para zócalos hembra/macho y verificando la consistencia mediante Comprobación de Reglas Eléctricas (ERC).
3. **[Layout y Ruteo de la PCB Modular - 24 h]:** Establecer las dimensiones geométricas de la placa, distribuir ergonómicamente los conectores y módulos, ejecutar el ruteo de pistas de alimentación y señales rápidas con headers de prueba, validando el diseño con Reglas de Diseño (DRC).
4. **[Salidas de Manufactura y BOM Modular - 4 h]:** Generar el paquete completo de producción Gerber RS-274X, archivos de taladrado Excellon y Lista de Materiales (BOM) para la versión modular.
5. **[Selección de Componentes SMD y Filtrado - 4 h]:** Investigar y seleccionar circuitos integrados equivalentes de montaje superficial (SoC ESP32, ADXL355 LGA-14, RTC DS3231 SOIC-16, reguladores LDO y filtros de línea), verificando disponibilidad en distribuidores globales (Mouser, Digi-Key).
6. **[Esquemático SMD en EAGLE - 16 h]:** Rediseñar el circuito esquemático sustituyendo los breakouts por componentes discretos y pasivos SMD (0805/0603), optimizando desacoplos de alimentación, protecciones ESD y superando la comprobación ERC sin advertencias.
7. **[Layout y Ruteo PCB SMD Compacta - 24 h]:** Rutar la tarjeta SMD en doble cara minimizando la superficie total, implementando planos de tierra continuos (GND pour) para disipación térmica y apantallamiento, e incorporando puntos de prueba (*test points*) con validación DRC estricta.
8. **[Documentación Técnica y Cierre de Repositorios - 4 h]:** Generar los archivos Gerber de la versión SMD, vistas 3D de ensamble, documentación técnica en repositorios de GitHub (`README.md`, esquemáticos PDF, criterios de ruteo) y tramitar la certificación institucional de culminación.

---

## 3. Plan de Trabajo Detallado y Checkpoints Cuantificables

### FASE 1: Diseño de la Versión Modular (48 Horas - Semanas 1 y 2)

#### Parte 1.1: Investigación y Análisis del Circuito
* **Duración:** 4 horas (Semana 1)
* **Objetivo:** Analizar la interconexión física del acelerógrafo ESP32 y definir las interfaces de hardware para los módulos estándar.
* **Actividades técnicas:**
  1. Auditar el esquemático preliminar y el cableado del prototipo en protoboard.
  2. Listar y verificar las interfaces de comunicación y voltajes de operación:
     - Bus SPI para ADXL355 (SCK, MISO, MOSI, CS).
     - Bus SPI dedicado (VSPI) para módulo MicroSD.
     - Bus I2C para RTC DS3231 (SDA, SCL) e interrupción SQW (1 Hz).
     - Puerto UART serie para módulo GPS FGPMMOPA6H (TX, RX) y pulso PPS.
     - Circuito de entrada de alimentación de 12V con convertidor reductor Buck DC-DC a 5V/3.3V.
  3. Establecer las especificaciones mecánicas de los módulos comerciales para la creación de footprints.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 1.1:** Matriz de interconexión y voltajes aprobada para todos los módulos del sistema.
  - [ ] **CP 1.2:** Especificaciones mecánicas y espaciado de pines recopilados para los conectores hembra.

---

#### Parte 1.2: Diseño del Esquema Modular en EAGLE
* **Duración:** 16 horas (Semanas 1 y 2)
* **Objetivo:** Capturar el esquema circuital completo en Autodesk EAGLE y validar su consistencia eléctrica.
* **Actividades técnicas:**
  1. Crear el proyecto oficial `Acelerografo_ESP32_Modular.sch` en Autodesk EAGLE.
  2. Dibujar y organizar por bloques jerárquicos los componentes:
     - Bloque 1: Socket para módulo ESP32 DevKit V1 (30 pines).
     - Bloque 2: Cabecera para acelerómetro triaxial ADXL355.
     - Bloque 3: Cabecera para módulo RTC DS3231.
     - Bloque 4: Cabecera para módulo GPS FGPMMOPA6H.
     - Bloque 5: Zócalo para módulo de tarjeta MicroSD.
     - Bloque 6: Etapa de alimentación y conector para convertidor Buck DC-DC.
  3. Configurar etiquetas de red (*nets*) de potencia y señal, asignando huellas de librería estandarizadas.
  4. Ejecutar la verificación de reglas eléctricas (*Electrical Rule Check* - ERC) y corregir nodos flotantes o conflictos lógicos.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 2.1:** Esquemático modular en EAGLE compilado con 0 errores críticos en el reporte ERC.
  - [ ] **CP 2.2:** Todas las huellas de componentes asignadas correctamente y enlazadas al editor de layout.

---

#### Parte 1.3: Diseño y Ruteo del PCB Modular en EAGLE
* **Duración:** 24 horas (Semana 2)
* **Objetivo:** Distribuir los módulos y rutar las pistas de circuito impreso cumpliendo con directrices de integridad de señal y facilidad de ensamble.
* **Actividades técnicas:**
  1. Definir el contorno de placa (*Dimension*) considerando un factor de forma que permita alojar holgadamente los módulos comerciales.
  2. Distribuir espacialmente los sockets para minimizar cruces de pistas y facilitar el conexionado externo.
  3. Rutar las pistas de alimentación (ancho $\ge 0.8\text{ mm}$ o 32 mils) y líneas de datos ($\ge 0.3\text{ mm}$ o 12 mils).
  4. Incorporar headers adicionales de prueba y pines de tierra (GND) para conexión de sondas de osciloscopio y analizador lógico.
  5. Ejecutar la comprobación de reglas de diseño (*Design Rule Check* - DRC) con tolerancias estándar de fabricación.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 3.1:** Layout de la PCB modular con 100% de conexiones ruteadas (0 *airwires*).
  - [ ] **CP 3.2:** Reporte DRC de EAGLE aprobado con 0 violaciones de espaciado o aislamiento.
  - [ ] **CP 3.3:** Headers de prueba y etiquetas de serigrafía legibles incorporadas en la capa superior (*tNames*/*tPlace*).

---

#### Parte 1.4: Validación, Generación de Salidas y Documentación Modular
* **Duración:** 4 horas (Semana 2)
* **Objetivo:** Generar los archivos de manufactura y consolidar la documentación en el repositorio GitHub correspondiente.
* **Actividades técnicas:**
  1. Ejecutar el procesador CAM de EAGLE para exportar el paquete de producción en formato Gerber RS-274X y archivos de taladrado Excellon.
  2. Generar la lista de materiales consolidada (BOM) con especificación de conectores y módulos requeridos.
  3. Redactar el archivo `README.md` del repositorio con instrucciones de ensamble, diagrama esquemático en PDF y renderizado 3D.
  4. Publicar los cambios en el repositorio oficial [RSA-Intern-HW-Acelerografo_ESP32_Modular](https://github.com/RedSismicaAustro/RSA-Intern-HW-Acelerografo_ESP32_Modular).
* **Checkpoints medibles para revisión:**
  - [ ] **CP 4.1:** Paquete de archivos Gerber y Drill generado y verificado en visor CAM.
  - [ ] **CP 4.2:** Repositorio GitHub de la versión modular actualizado con esquemáticos, BOM y documentación técnica.

---

### FASE 2: Diseño de la Versión con Montaje Superficial - SMD (48 Horas - Semanas 2 y 3)

#### Parte 2.1: Investigación y Selección de Componentes SMD
* **Duración:** 4 horas (Semana 2)
* **Objetivo:** Sustituir los módulos comerciales por circuitos integrados y componentes pasivos SMD para miniaturización y eficiencia.
* **Actividades técnicas:**
  1. Identificar componentes a nivel de integrado:
     - SoC ESP32-WROOM-32E o chip ESP32-D0WD con oscilador de 40 MHz y memoria flash SPI.
     - CI Acelerómetro ADXL355 en encapsulado cerámico LGA-14.
     - CI RTC DS3231M o DS3231SN en encapsulado SOIC-16 con soporte de batería de respaldo (CR1220 o similar).
     - Receptor GPS SMD (ej. SIM28ML o equivalente) con conector para antena activa U.FL / SMA.
     - Zócalo MicroSD integrado tipo *push-push* con contacto mecánico de detección física (*card-detect*).
  2. Seleccionar etapas de regulación de tensión: regulador conmutado Buck de alta eficiencia para entrada industrial y LDOs de ultra bajo ruido (LDO $\le 20\,\mu\text{V}_\text{RMS}$) para el raíl analógico del ADXL355.
  3. Verificar disponibilidad de stock y huellas en distribuidores (Mouser, Digi-Key, JLCPCB SMT Parts).
  4. Estructurar la lista preliminar de materiales (BOM SMD).
* **Checkpoints medibles para revisión:**
  - [ ] **CP 5.1:** Selección de integrados SMD documentada con números de parte (MPN) y hojas de datos.
  - [ ] **CP 5.2:** Esquema de fuentes de alimentación (Buck + LDO de bajo ruido) dimensionado para minimizar rizado.

---

#### Parte 2.2: Diseño del Esquema Electrónico SMD en EAGLE
* **Duración:** 16 horas (Semanas 2 y 3)
* **Objetivo:** Diseñar el esquemático a nivel de componentes discretos en EAGLE implementando protecciones y filtrado avanzado.
* **Actividades técnicas:**
  1. Crear el proyecto `Acelerografo_ESP32_SMD.sch` en EAGLE.
  2. Implementar los circuitos de polarización y soporte para el ESP32: circuito de reset (EN), botones de Boot, circuito de autoprogramación por USB-UART (CP2102N o header ICSP) y desacoplos capacitivos de $100\text{ nF}$ y $10\,\mu\text{F}$.
  3. Diseñar la etapa analógica y digital del ADXL355 con filtros pasabajos RC en alimentación y planos de referencia limpios.
  4. Diseñar la interfaz del RTC DS3231 con batería de botón y pull-ups I2C integrados.
  5. Incorporar diodos de protección TVS en líneas de alimentación y datos expuestas para prevenir descargas electrostáticas (ESD).
  6. Realizar la verificación de errores eléctricos (ERC) y corregir advertencias.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 6.1:** Esquemático SMD finalizado en EAGLE con todas las redes debidamente etiquetadas.
  - [ ] **CP 6.2:** Verificación ERC superada con 0 errores y compatibilidad confirmada entre pines lógicos y físicos.

---

#### Parte 2.3: Layout y Ruteo de la PCB SMD de Alta Densidad
* **Duración:** 24 horas (Semana 3)
* **Objetivo:** Diseñar una placa de circuito impreso ultra compacta de doble cara, con planos de tierra continuos y óptima disipación térmica.
* **Actividades técnicas:**
  1. Definir dimensiones reducidas optimizadas para cajas estancas normalizadas.
  2. Ubicar estratégicamente el SoC ESP32 con la antena de RF orientada hacia el borde exterior de la placa libre de planos de cobre.
  3. Ubicar el sensor ADXL355 en el centro mecánico de la placa para registrar vibraciones fidedignas con simetría estructural.
  4. Rutar pistas de señal respetando anchos para señales de alta frecuencia y pistas anchas para los buses de potencia.
  5. Generar planos de cobre macizo (Copper Pour) conectados a GND en las capas superior e inferior (*Top* y *Bottom*), implementando cosido de vías (*via stitching*) para un blindaje electromagnético eficaz y baja impedancia de retorno.
  6. Disponer puntos de prueba (*test points*) SMD en líneas críticas (3.3V, 5V, GND, SQW, PPS, SPI SCK/MISO/MOSI).
  7. Ejecutar la comprobación exhaustiva de reglas de diseño (DRC) con las restricciones del fabricante de PCBs.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 7.1:** PCB SMD 100% ruteada en formato compacto de doble cara con plano de masa mallado/sólido.
  - [ ] **CP 7.2:** Reporte DRC de EAGLE limpio con 0 violaciones de espaciado, anchos o anulares de vías.
  - [ ] **CP 7.3:** Puntos de prueba y serigrafía industrial posicionados para facilitar el diagnóstico.

---

#### Parte 2.4: Validación, Archivos de Fabricación y Documentación Final
* **Duración:** 4 horas (Semana 3)
* **Objetivo:** Exportar el paquete industrial de producción, compilar la documentación técnica y entregar los repositorios.
* **Actividades técnicas:**
  1. Generar los archivos Gerber RS-274X, archivos de taladrado Excellon y archivos de posición de componentes (*Centroid / Pick and Place*) para ensamblaje automatizado SMT.
  2. Generar el renderizado 3D de la placa para inspección visual de colisiones mecánicas.
  3. Consolidar la Lista de Materiales (BOM) final con códigos de fabricante y distribuidores.
  4. Redactar el `README.md` exhaustivo en el repositorio oficial [RSA-Intern-HW-Acelerografo_ESP32_SMD](https://github.com/RedSismicaAustro/RSA-Intern-HW-Acelerografo_ESP32_SMD), detallando criterios de ruteo, desacoplamiento y notas de manufactura.
  5. Revisión técnica con el tutor institucional y suscripción del formulario de evaluación F002.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 8.1:** Paquete completo de manufactura (Gerbers, Drill, BOM y Pick&Place) verificado en visor industrial.
  - [ ] **CP 8.2:** Repositorios en GitHub de ambas versiones sincronizados, documentados y públicos.
  - [ ] **CP 8.3:** Formulario de evaluación empresarial F002 suscrito por el tutor institucional de la RSA acreditando las 96 horas.

---

## 4. Resumen de Fases y Cronograma Semanal (96 Horas)

El trabajo se ejecutó en un período de **3 semanas intensivas** (del 10 de febrero al 03 de marzo de 2025) con una carga de **32 horas semanales**:

| Fase | Semanas | Horas | Enfoque Principal |
| :---: | :---: | :---: | :--- |
| **Fase 1 (Modular)** | Semana 1 | **20 h** | Investigación del prototipo, selección de módulos y diseño esquemático modular en EAGLE (ERC). |
| **Fase 2 (Modular)** | Semanas 1–2 | **28 h** | Ruteo de PCB modular, headers de prueba, verificación DRC, exportación Gerber y BOM. |
| **Fase 3 (SMD)** | Semanas 2–3 | **20 h** | Selección de integrados SMD (SoC, ADXL355, LDO bajo ruido) y diseño esquemático SMD en EAGLE. |
| **Fase 4 (SMD)** | Semana 3 | **28 h** | Layout PCB SMD compacto, planos GND (via stitching), DRC industrial, Gerbers, Pick&Place y cierre. |
| **Total** | **3 Semanas** | **96 h** | **Planificación Consolidada de Prácticas Laborales** |

---

## 5. Entregables y Criterios de Éxito de la Pasantía

1. **Diseño de PCB Modular en Autodesk EAGLE (Etapa 1):**
   * Archivo esquemático (`.sch`) y archivo de placa (`.brd`) en EAGLE que integra módulos comerciales (ESP32 DevKit, ADXL355, RTC DS3231, GPS, MicroSD y Buck DC-DC).
   * Paquete de producción Gerber y Lista de Materiales (BOM) disponible en el repositorio: [RSA-Intern-HW-Acelerografo_ESP32_Modular](https://github.com/RedSismicaAustro/RSA-Intern-HW-Acelerografo_ESP32_Modular).
2. **Diseño de PCB SMD de Alta Densidad en Autodesk EAGLE (Etapa 2):**
   * Archivo esquemático (`.sch`) y layout (`.brd`) en tecnología de montaje superficial (SMD), optimizado para bajo ruido analógico y dimensiones reducidas.
   * Planos de masa sólidos continuos en capas Top/Bottom con costura de vías (*via stitching*) para apantallamiento y disipación térmica.
   * Puntos de prueba (*test points*) y protecciones ESD integradas.
   * Paquete completo de manufactura (Gerbers RS-274X, NC Drill, Centroid/Pick&Place y BOM industrial) alojado en: [RSA-Intern-HW-Acelerografo_ESP32_SMD](https://github.com/RedSismicaAustro/RSA-Intern-HW-Acelerografo_ESP32_SMD).
3. **Documentación Técnica y Repositorios:**
   * Archivos `README.md` detallados en ambos repositorios con esquemas de conexionado, especificaciones de alimentación y pautas para futuros desarrolladores.
   * Modelos y vistas 3D de ensamble de las placas de circuito impreso.
4. **Certificación Institucional de Prácticas:**
   * Formulario institucional F002 debidamente validado y suscrito por el tutor institucional de la RSA acreditando las 96 horas de desarrollo tecnológico de excelencia.
