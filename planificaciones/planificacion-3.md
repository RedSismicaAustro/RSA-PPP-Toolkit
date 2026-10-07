---
titulo: "Plan de Trabajo de Prácticas Laborales: Optimización de Almacenamiento en Tarjetas SDHC/SDXC y Adaptación de Firmware para Monitoreo de Salud Estructural de Puentes"
proyecto: "Sistema de Monitorización de Salud Estructural (SHM Puentes y Nodos Acelerógrafos)"
codigo_proyecto: "RSA-PPP-2024-03"
area_tematica: "Sistemas Embebidos, Almacenamiento Masivo en Flash, Firmware y Monitoreo de Puentes"
estado: "Culminado"
version: "1.0"
fecha_creacion: "2024-02-08"
fecha_actualizacion: "2024-03-26"

pasante:
  nombre: "Henry Maldonado"
  cedula: "N/D"
  correo: "henrry.maldonado@ucuenca.edu.ec"
  carrera: "Ingeniería en Telecomunicaciones"
  institucion: "Universidad de Cuenca"

tutoria:
  tutor_institucional: "Ing. Milton Muñoz. (Red Sísmica del Austro - RSA)"
  correo_tutor_institucional: "milton.munozc@ucuenca.edu.ec"

cronograma:
  duracion_total_horas: 96
  dedicacion_semanal_horas: 16
  duracion_semanas: 6
  fecha_inicio: "2024-02-08"
  fecha_fin_estimada: "2024-03-22"
  modalidad: "Presencial"

tecnologias:
  - "Microcontrolador y Arquitectura: Microchip dsPIC33EP256MC202 (16 bits, módulo SPI 1)"
  - "Entorno de Desarrollo e IDE: MikroC PRO for dsPIC (Compilador C, depuración y emulador)"
  - "Protocolos de Almacenamiento y Buses: Interfaz SPI (Modo 0, SLOW a 625 kHz / FAST a alta velocidad)"
  - "Especificación de Memorias: SD Physical Layer Specification V2.0+ (Tarjetas SDSC, SDHC y SDXC en formato MicroSD)"
  - "Herramientas de Análisis Forense y Validación: Editor Hexadecimal HxD, Analizador Lógico, Multímetro"
  - "Hardware y PCBs de Aplicación: Nodos Sensores y Placas del Sistema de Monitoreo de Puentes (Módulos ACS722, NRF24L01, RTC DS3234)"

repositorio:
  url: "No disponible / Trabajo local sin repositorio remoto asignado"
  rama_base: "N/A"
---

---

## 1. Antecedentes y Justificación Técnica

1. **Contexto del Sistema y Estado Inicial del Artefacto:** 
   La Red Sísmica del Austro (RSA) implementa redes distribuidas de acelerógrafos para la monitorización continua de la salud estructural (SHM) en puentes vehiculares y obras de infraestructura civil. Cada nodo sensor autónomo utiliza un microcontrolador dsPIC33EP256MC202 y requiere persistir las series temporales de vibración en una memoria MicroSD local para prevenir la pérdida de datos ante desconexiones de red. La librería de almacenamiento existente en el firmware estaba restringida a tarjetas antiguas de capacidad estándar (SDSC $\le 2\text{ GB}$), las cuales se encuentran obsoletas y descatalogadas comercialmente. Asimismo, las placas de circuito impreso (PCBs) implementadas en el sistema de puentes contaban con esquemáticos dispersos y diferencias de conexionado en los módulos auxiliares (sensor de corriente ACS722, enlace NRF24L01, RTC DS3234 y zócalo MicroSD) que requerían un análisis sistemático antes de adaptar el firmware de registro continuo.

2. **Limitaciones Críticas o Cuellos de Botella Identificados:** 
   * **Incompatibilidad con Tarjetas SDHC y SDXC:** Al insertar tarjetas modernas ($\ge 4\text{ GB}$ hasta 32 GB o superiores), el protocolo SPI anterior fallaba en la secuencia de inicialización al no soportar el comando `CMD8` (validación de interfaz y rango de voltaje) ni gestionar el argumento `HCS` (High Capacity Support) en `ACMD41`.
   * **Modo de Direccionamiento Erróneo:** Las tarjetas SDSC utilizan direccionamiento físico por bytes, mientras que las tarjetas SDHC/SDXC operan con direccionamiento directo por bloques físicos de 512 bytes. La librería previa intentaba multiplicar la dirección de sector por 512 en memorias modernas, generando desbordamiento de argumentos de 32 bits y corrupción de memoria.
   * **Ausencia de Detección de Tarjeta por Hardware:** En los esquemáticos de las placas de puentes, el pin mecánico de detección de tarjeta (*card-detect* / SH) del zócalo MicroSD no se encontraba ruteado al microcontrolador, obligando a realizar intentos reiterados de inicialización por software ante inserciones en caliente.
   * **Incompatibilidad entre Fabricantes (Kingston vs. SanDisk):** Se identificaron diferencias críticas en los tiempos de respuesta y secuencias de inicialización entre fabricantes; particularmente en memorias SanDisk, las cuales no admitían reinicializaciones directas por software y presentaban fallos de respuesta ante el comando de lectura de bloque único (`CMD17`).

3. **Desacoplamiento del Alcance y Propuesta de Solución:** 
   El plan de trabajo de 96 horas se estructura en dos ejes complementarios:
   * **Revisión y Relevamiento de Hardware:** Obtención y auditoría detallada de todos los esquemas electrónicos de las PCBs utilizadas en el sistema de monitorización de salud estructural de puentes (20 horas).
   * **Optimización de Firmware y Librería de Almacenamiento SD:** Reescribir y depurar las librerías `sdcard` y `spiSD` en MikroC PRO for dsPIC para incorporar la especificación completa SD V2.0+ (CMD0, CMD8, CMD58, CMD59, CMD55, ACMD41, CMD16), soportar el bit CCS (Card Capacity Status), conmutar dinámicamente frecuencias de reloj SPI y adaptar el firmware `Nodoacelerometro.c` para registrar tramas estructuradas completas de aceleración en puentes (76 horas).

---

## 2. Objetivos del Plan de Trabajo

### 2.1. Objetivo General
Actualizar, optimizar y validar la librería de almacenamiento en tarjetas MicroSD para microcontroladores dsPIC33EP garantizando compatibilidad total con memorias modernas SDHC y SDXC, y analizar los esquemas electrónicos de las PCBs para adaptar el firmware de registro acelerométrico continuo al sistema de monitorización de salud estructural de puentes.

### 2.2. Objetivos Específicos
1. **[Análisis de PCBs de Puentes - 20 h]:** Realizar el relevamiento, análisis esquemático y verificación de pines de todas las placas de circuito impreso utilizadas en el sistema de monitoreo de puentes, identificando el mapeo de líneas SPI (RB8/MOSI, RB9/MISO, RB0/CS, RB7/SCK) y periféricos asociados.
2. **[Actualización de Librería SD - 40 h]:** Reescribir las librerías `sdcard.c/.h` y `spiSD.c/.h` en MikroC, implementando el protocolo estándar SD V2.0+ mediante el envío determinístico de comandos (`CMD0`, `CMD8`, `CMD58`, `CMD59`, `CMD55`, `ACMD41`, `CMD16`), el análisis de respuestas R1, R3 y R7, y el soporte de direccionamiento por bloques de 512 bytes mediante la lectura del bit CCS.
3. **[Pruebas Unitarias y Validación Cruzada - 10 h]:** Desarrollar la rutina `Ejemplo_uso_SD()` para ejecutar pruebas exhaustivas de escritura y lectura en sectores de prueba (ej. sector 2500), verificando la integridad de datos byte a byte frente a memorias Kingston y SanDisk, e inspeccionando los sectores crudos con el editor hexadecimal HxD.
4. **[Adaptación de Firmware de Registro Continuo - 26 h]:** Modificar y adaptar el firmware `Main_SD.c` / `Nodoacelerometro.c` implementando la función `GuardarTramaSD()` para empaquetar y persistir tramas completas de aceleración (cabeceras, timestamps y datos triaxiales equivalentes a 5 sectores consecutivos / 2512 bytes) compatibles con el sistema de monitoreo de puentes.
5. **[Documentación Técnica y Cierre]:** Documentar el comportamiento de las respuestas de memorias, caracterizar las anomalías de lectura en tarjetas SanDisk y redactar el Informe Final de Prácticas Laborales.

---

## 3. Plan de Trabajo Detallado y Checkpoints Cuantificables

### Fase 1: Levantamiento y Análisis de Esquemáticos de PCBs del Sistema de Puentes
* **Duración:** 20 horas (Semanas 1 y 2)
* **Objetivo:** Auditar los esquemas electrónicos de las placas del sistema de puentes, mapeando conexiones de buses, pines de control y periféricos del dsPIC33EP256MC202.
* **Actividades técnicas:**
  1. Recopilar y examinar los esquemáticos de hardware del nodo sensor y placas complementarias del sistema de monitoreo de puentes.
  2. Mapear la asignación de pines del microcontrolador dsPIC33EP256MC202 con el zócalo MicroSD:
     - Pin RB8: Transmisión MOSI (Master Out Slave In) del módulo SPI 1.
     - Pin RB9: Recepción MISO (Master In Slave Out) del módulo SPI 1.
     - Pin RB0: Control de selección de chip Chip Select ($\overline{\text{CS}}$).
     - Pin RB7: Señal de reloj SCK (Serial Clock).
  3. Comprobar las conexiones de periféricos complementarios en la PCB: módulo de corriente ACS722, módulo transceptor NRF24L01, reloj de tiempo real RTC DS3234 y conector de programación ICSP.
  4. Identificar la ausencia del pin mecánico *card-detect* (SH) en el zócalo MicroSD y documentar la necesidad de detección lógica por software.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 1.1:** Documento de diagrama esquemático consolidado y tabla de asignación de pines del nodo sensor para puentes.
  - [ ] **CP 1.2:** Matriz de periféricos SPI identificada descartando colisiones de líneas $\overline{\text{CS}}$ entre la MicroSD, el RTC y transceptores.
  - [ ] **CP 1.3:** Diagnóstico técnico formal sobre la omisión del pin de detección física en el zócalo SD.

---

### Fase 2: Implementación y Optimización de la Librería SD para Tarjetas SDHC y SDXC
* **Duración:** 40 horas (Semanas 2 y 3)
* **Objetivo:** Refactorizar por completo los controladores de bajo nivel `spiSD` y `sdcard` para cumplir la especificación física SD V2.0+ en microcontroladores dsPIC.
* **Actividades técnicas:**
  1. Configuración de reloj SPI en `spiSD.c`: establecer `SPISD_Init(SLOW)` a frecuencia reducida (mínimo disponible de 625 kHz en el dsPIC) y generar tren preliminar de $\ge 80$ pulsos de reloj con $\overline{\text{CS}}=1$ para sincronizar la lógica interna de la memoria.
  2. Implementar la rutina de reintentos `SD_Init_Try()` y la máquina de estados de inicialización en `SD_Init()`:
     - Enviar `CMD0` con argumento `0x00000000` y CRC `0x4A` hasta recibir respuesta R1 en estado `In Idle State` (`0x01`).
     - Enviar `CMD8` (SEND_IF_COND) con voltaje VHS (2.7–3.6 V) y patrón de prueba `0xAA` (CRC `0x43`) para discriminar tarjetas V2.0+ mediante el análisis de respuesta R7.
     - Enviar `CMD58` (READ_OCR) para verificar rangos de voltaje en el registro OCR (respuesta R3).
     - Habilitar verificación de integridad CRC7 mediante `CMD59`.
     - Ejecutar bucle de activación: `CMD55` (APP_CMD) seguido de `ACMD41` (SD_SEND_OP_COND) fijando el bit `HCS` (bit 30) en 1 para notificar soporte de host a capacidades SDHC/SDXC.
     - Iterar hasta que la respuesta R1 indique estado `DONE` (`0x00`), confirmando que la tarjeta está lista para operar.
     - Deshabilitar comprobación de CRC con `CMD59` y fijar tamaño de bloque a 512 bytes mediante `CMD16` (SET_BLOCKLEN).
  3. Evaluar el registro OCR mediante nuevo `CMD58` para inspeccionar el bit 30 (`CCS` - Card Capacity Status):
     - Si $\text{CCS} = 1$: Clasificar como memoria SDHC/SDXC; la dirección de los sectores corresponde directamente al número de bloque físico.
     - Si $\text{CCS} = 0$: Clasificar como memoria estándar SDSC; la dirección debe multiplicarse por 512.
  4. Conmutar el bus a alta velocidad `SPISD_Init(FAST)` tras completar la inicialización exitosa.
  5. Programar las funciones de transferencia de datos crudos: `SD_Write_Block()` para persistir bloques de 512 bytes con tokens de datos y `SD_Read_Block()` para recuperar sectores mediante `CMD17`.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 2.1:** Código fuente de `spiSD.c/.h` y `sdcard.c/.h` compilado en MikroC sin errores de enlace ni advertencias críticas.
  - [ ] **CP 2.2:** Inicialización exitosa de tarjetas SDHC de alta capacidad confirmada mediante la lectura del bit $\text{CCS}=1$ en el registro OCR.
  - [ ] **CP 2.3:** Rutinas de bajo nivel `SD_Write_Block()` y `SD_Read_Block()` operativas con manejo de tokens y temporizaciones controladas.

---

### Fase 3: Pruebas Exhaustivas de Lectura/Escritura y Validación Comparativa (Kingston vs SanDisk)
* **Duración:** 10 horas (Semana 4)
* **Objetivo:** Ejecutar ensayos rigurosos de integridad de datos byte a byte y caracterizar las diferencias operativas entre tarjetas Kingston y SanDisk.
* **Actividades técnicas:**
  1. Desarrollar la función de prueba unitaria `Ejemplo_uso_SD()` en `Main_SD.c`:
     - Generar en memoria RAM un arreglo `data_to_write[512]` con un patrón continuo incremental ($1 \dots 255$ repetido).
     - Escribir dicho arreglo en un sector específico de la tarjeta (sector 2500) mediante `SD_Write_Block()`.
     - Leer el sector grabado mediante `SD_Read_Block()` y cargarlo en un arreglo `buffer[512]`.
     - Escribir el contenido del búfer recuperado en un sector contiguo (`sector + 1` = 2501).
     - Indicar éxito mediante conmutación de LED de diagnóstico (`LED(1,1)`).
  2. Ejecutar ensayos con memorias Kingston (16 GB y 32 GB SDHC): verificar inicialización, lectura y reescritura.
  3. Ejecutar ensayos con memorias SanDisk (16 GB SDHC):
     - Validar la inicialización y activación de $\text{CCS}=1$.
     - Probar la rutina de escritura y verificar la persistencia exitosa.
     - Analizar la causa raíz de la falla de lectura ante `CMD17` (retorno de token erróneo `0xFF` en lugar de `0x00`).
     - Investigar en foros y documentación oficial soluciones de sincronismo (ciclos de reloj adicionales, frecuencias de inicialización entre 100–400 kHz y rangos de voltaje).
  4. Extraer las tarjetas MicroSD e inspeccionar los sectores físicos 2500 y 2501 en PC mediante el editor hexadecimal HxD.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 3.1:** Función `Ejemplo_uso_SD()` validada en hardware con memorias Kingston con coincidencia de datos al 100%.
  - [ ] **CP 3.2:** Evidencia fotográfica y capturas en HxD demostrando la persistencia del patrón incremental en los sectores 2500 y 2501.
  - [ ] **CP 3.3:** Informe comparativo documentando la anomalía de lectura en SanDisk y recomendaciones para futuras revisiones de firmware y hardware.

---

### Fase 4: Adaptación del Firmware de Registro Continuo para el Sistema de Puentes
* **Duración:** 26 horas (Semanas 4 y 5)
* **Objetivo:** Integrar la librería de tarjetas SD optimizada en el firmware principal del acelerógrafo para registro sostenido de vibraciones en puentes.
* **Actividades técnicas:**
  1. Integrar los módulos `sdcard` y `spiSD` en el proyecto del firmware principal `Main_SD.mcpds` / `Nodoacelerometro.c`.
  2. Implementar la función `GuardarTramaSD()` para estructurar paquetes continuos de auscultación:
     - Cabecera fija de sincronización y metadatos de estación.
     - Estampa de tiempo (timestamp proveniente del RTC DS3234 o marcas de sincronismo).
     - Carga útil de muestras de aceleración triaxial.
     - Tamaño total de trama fijado en 2512 bytes (equivalente a 5 sectores físicos de 512 bytes consecutivos).
  3. Programar la lógica de avance secuencial de punteros de sector para registro sostenido en la tarjeta MicroSD.
  4. Realizar pruebas de estrés de escritura continua en banco simulando eventos vibratorios en puentes.
  5. Inspeccionar en HxD que los 5 sectores consecutivos contengan la estructura exacta de cabecera, tiempo y aceleraciones sin solapamientos.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 4.1:** Firmware del nodo sensor compilado exitosamente con soporte unificado de acelerógrafo y almacenamiento SDHC.
  - [ ] **CP 4.2:** Función `GuardarTramaSD()` probada en banco con persistencia verificada de 5 sectores continuos (2512 bytes).
  - [ ] **CP 4.3:** Análisis en HxD certificando la coherencia de las tramas de datos simuladas para monitoreo de puentes.

---

### Fase 5: Pruebas Integradas, Documentación Técnica e Informe Final de Prácticas
* **Duración:** 14 horas (Semana 6)
* **Objetivo:** Consolidar el repositorio local de código fuente, compilar los resultados experimentales y redactar el informe técnico formal de prácticas laborales.
* **Actividades técnicas:**
  1. Limpiar y estructurar el directorio de trabajo `Main_SD` conteniendo proyectos, headers, fuentes `.c`, binarios `.hex` y archivos de configuración del dsPIC.
  2. Sistematizar las tablas de comandos SPI, argumentos, códigos CRC7 y tipos de respuesta (R1, R2, R3, R7) documentadas a partir de la especificación física de la SD Association.
  3. Redactar el documento formal institucional: `Informe_Actividad__Prácticas_Laborales.pdf` conteniendo:
     - Justificación teórica del protocolo SPI en memorias SD.
     - Diagrama de flujo de inicialización SDHC/SDXC.
     - Código fuente documentado de las funciones de prueba.
     - Análisis comparativo Kingston vs. SanDisk y recomendaciones para el diseño de hardware futuro.
  4. Presentación de resultados y revisión técnica con el tutor institucional.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 5.1:** Paquete consolidado `Main_SD.zip` organizado con código fuente limpio y archivos de proyecto MikroC.
  - [ ] **CP 5.2:** Informe Técnico Final (`Informe_Actividad__Prácticas_Laborales.pdf`) concluido y aprobado por el tutor institucional de la RSA.

---

## 4. Resumen de Fases y Cronograma Semanal (96 Horas)

El trabajo se distribuyó a lo largo de **6 semanas** con una carga de **16 horas semanales** (alcanzando hasta 20 horas en semanas de mayor intensidad de desarrollo y pruebas en laboratorio):

| Fase | Semanas | Horas | Enfoque Principal |
| :---: | :---: | :---: | :--- |
| **Fase 1** | Semanas 1–2 | **20 h** | Levantamiento y análisis esquemático de PCBs del sistema de monitoreo de puentes. |
| **Fase 2** | Semanas 2–3 | **40 h** | Reescrutura y optimización de la librería SD para soporte SDHC/SDXC en dsPIC (MikroC). |
| **Fase 3** | Semana 4 | **10 h** | Pruebas exhaustivas de lectura/escritura (Ejemplo_uso_SD) y contraste Kingston vs SanDisk en HxD. |
| **Fase 4** | Semanas 4–5 | **26 h** | Adaptación del firmware de registro continuo (`GuardarTramaSD`, 5 sectores) para puentes. |
| **Fase 5** | Semana 6 | **14 h** | Pruebas integradas finales, empaquetado de firmware y redacción del Informe Final de Prácticas. |
| **Total** | **6 Semanas** | **96 h** | **Planificación Global de Prácticas Laborales** |

---

## 5. Entregables y Criterios de Éxito de la Pasantía

1. **Análisis Técnico de Esquemáticos de PCBs para Puentes:**
   * Informe de relevamiento de hardware de las tarjetas de circuito impreso para auscultación de puentes, con la caracterización de líneas SPI, periféricos dsPIC y diagnóstico del pin *card-detect*.
2. **Librería de Tarjetas SD Optimizada para dsPIC33EP (MikroC):**
   * Módulos `sdcard.c/.h` y `spiSD.c/.h` con soporte completo de tarjetas SDHC y SDXC mediante direccionamiento de bloques físicos de 512 bytes guiado por el bit CCS del registro OCR.
   * Máquina de estados de inicialización robusta basada en la especificación SD V2.0+ (CMD0, CMD8, CMD58, CMD59, CMD55, ACMD41, CMD16).
3. **Código de Prueba y Validación Cruzada:**
   * Rutina unitaria `Ejemplo_uso_SD()` implementada en `Main_SD.c` que escribe y valida la lectura y reescritura de sectores con patrones numéricos.
   * Evidencias y capturas del editor hexadecimal HxD certificando la integridad de datos en memorias Kingston.
4. **Firmware de Registro Continuo para Puentes:**
   * Función `GuardarTramaSD()` integrada en el firmware del acelerógrafo para persistir tramas de 2512 bytes (5 sectores contiguos) con cabeceras, tiempo y señales acelerométricas.
   * Paquete compilado `Main_SD.zip` conteniendo los archivos fuente, ejecutables `.hex` y configuraciones de bits del oscilador del dsPIC.
5. **Informe Técnico Final de Prácticas:**
   * Documento formal `Informe_Actividad__Prácticas_Laborales.pdf` que consolida la arquitectura del protocolo, la caracterización de fallas en memorias SanDisk y directrices técnicas para futuros desarrollos en la RSA.
