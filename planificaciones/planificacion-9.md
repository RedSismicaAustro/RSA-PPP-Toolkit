---
titulo: "Plan de Trabajo de Prácticas Preprofesionales: Ensamblaje, Programación, Validación en Daisy Chain y Prueba de Concepto MicroSD para Red de Monitorización SHM"
proyecto: "Ensamblaje, Programación y Validación de Red Distribuida de Monitorización de Salud Estructural (Serie Acelerógrafo V1.4)"
codigo_proyecto: "RSA-PPP-2026-09"
area_tematica: "Sistemas Embebidos, Ensamblaje Electrónico, Comunicaciones RS485 (Daisy Chain), Sincronización Temporal y Almacenamiento SPI"
estado: "Culminado"
version: "1.0"
fecha_creacion: "2026-07-30"
fecha_actualizacion: "2026-09-08"

pasante:
  nombre: "David Timbi"
  cedula: "N/D"
  correo: "david.timbi@ucuenca.edu.ec"
  carrera: "Ingeniería en Telecomunicaciones"
  institucion: "Universidad de Cuenca"

tutoria:
  tutor_institucional: "Ing. Milton Muñoz. (Red Sísmica del Austro - RSA)"
  correo_tutor_institucional: "milton.munozc@ucuenca.edu.ec"

cronograma:
  duracion_total_horas: 96
  dedicacion_semanal_horas: 16
  duracion_semanas: 6
  fecha_inicio: "2026-07-30"
  fecha_fin_estimada: "2026-09-08"
  modalidad: "Presencial"

tecnologias:
  - "Microcontrolador y Arquitectura: Microchip dsPIC33EP256MC202-I/SP (Encapsulado DIP-28, 80 MHz con oscilador FRCPLL interno)"
  - "Entorno de Desarrollo y Programación: MikroC PRO for dsPIC (Compilación C), MPLAB IPE v6.20 y programador hardware PICkit 3"
  - "Transceptores y Comunicaciones Diferenciales: MAX485 (Canal de datos a 2 Mbps) y MAX483 (Línea de sincronización dedicada de baja velocidad/slew-rate)"
  - "Cableado y Topología Física: Conectores RJ45 (8P8C), Cable UTP Cat 5e/6 ponchado bajo norma T-568B en Cascada (Daisy Chain)"
  - "Instrumental de Medición y Diagnóstico: Osciloscopio Digital Hantek (canales CH1/CH2), Multímetro Digital, Tester de Continuidad de Cable de Red"
  - "Almacenamiento y Buses de Expansión: Mapeo PPS para SPI1, controladores sdcard.c/spiSD.c y tarjetas MicroSD Kingston (16/32 GB SDHC)"

repositorio:
  url: "https://github.com/RSA-PPP/ppp-2026-09-shm-ensamblaje-validacion"
  rama_base: "main"

topics:
  - rsa-ppp
  - ucuenca
  - shm
  - dspic33
  - rs485
  - daisy-chain
  - time-synchronization
  - microsd
  - mikroc
  - c
---

---

## 1. Antecedentes y Justificación Técnica

1. **Contexto del Sistema y Estado Inicial del Artefacto:** 
   La Red Sísmica del Austro (RSA) completó la fase de diseño electrónico de la serie *Acelerógrafo V1.4*, conformada por un **Nodo Concentrador** y dos variantes de **Nodos Sensores** (Versión A con planos de tierra continuos y Versión B sin planos de masa) interconectados mediante una topología serie en cascada (*Daisy Chain*). Los circuitos impresos habían sido diseñados en software EDA y fabricados industrialmente, pero requerían manufactura manual en laboratorio (soldadura de componentes THT y SMD, transceptores y microcontroladores dsPIC33EP256MC202), inspección visual, programación de firmwares de control y validación metrológica con instrumental antes de ser desplegados en puentes o presas.

2. **Limitaciones Críticas o Cuellos de Botella Identificados:** 
   * **Incertidumbre en Retardos de Propagación en Cascada:** Al interconectar nodos sensores en serie compartiendo un bus diferencial de sincronismo dedicado, era imprescindible determinar experimentalmente si la topología Daisy Chain introducía retardos acumulativos significativos entre nodos consecutivos que pudieran degradar la coherencia temporal de las señales sísmicas.
   * **Problemas de Configuración del Oscilador:** El microcontrolador dsPIC opera sin cristal externo; un error en la palabra de configuración (*Configuration Bits*) forzaba al oscilador a operar en modo *Primary* en lugar de *FRCPLL*, impidiendo alcanzar la frecuencia nominal de 80 MHz y descalibrando la tasa de baudios del UART (2 Mbps).
   * **Discrepancias de Hardware y Daño en Señalización:** El uso de una variante física distinta del dsPIC33EP en una placa derivó en pines en corto que averiaron el LED de diagnóstico de uno de los sensores, requiriendo su sustitución y ajuste de código. Asimismo, las placas reales implementaron el transceptor MAX483 en la línea de sincronismo en lugar del MAX485 previsto en esquemáticos de referencia.
   * **Limitación Crítica en el Zócalo MicroSD:** El almacenamiento local en tarjeta MicroSD resultaba vital para respaldo de datos; sin embargo, se identificó que el zócalo montado carecía del contacto mecánico de detección física (*card-detect*), afectando la inicialización estándar del firmware.

3. **Desacoplamiento del Alcance y Propuesta de Solución:** 
   El plan de trabajo de **96 horas** fusiona la validación integral de hardware/red con una prueba de concepto (PoC) avanzada de almacenamiento:
   * Ensamblar y soldar manualmente las 3 placas del sistema (1 Concentrador y 2 Nodos Sensores).
   * Fabricar el arnés de cableado UTP bajo norma T-568B y validar su continuidad.
   * Desarrollar, adaptar y programar en MikroC PRO for dsPIC el firmware para Concentrador y Sensores A y B, resolviendo la configuración de reloj a 80 MHz y la mensajería RS485 a 2 Mbps.
   * Caracterizar mediante osciloscopio los retardos de propagación (en microsegundos y nanosegundos) y niveles de tensión de la señal de sincronismo.
   * **Prueba de Concepto (PoC) de Almacenamiento en MicroSD:** Integrar la librería SPI (`sdcard.c`/`spiSD.c`), remapear pines PPS y evaluar la escritura de sectores crudos de 512 bytes ante pulsos de sincronismo, documentando exhaustivamente la limitación física del zócalo y el diagnóstico mediante códigos de error LED.

---

## 2. Objetivos del Plan de Trabajo

### 2.1. Objetivo General
Ensamblar, programar, auditar y validar experimentalmente el hardware y firmware de una red distribuida de monitorización de salud estructural (Acelerógrafo V1.4) compuesta por un Nodo Concentrador y dos Nodos Sensores en topología Daisy Chain bajo el estándar T-568B, caracterizando con osciloscopio los retardos de sincronización y ejecutando una prueba de concepto para el almacenamiento local de sectores en tarjetas MicroSD mediante dsPIC33EP.

### 2.2. Objetivos Específicos
1. **[Ensamblaje e Inspección de Hardware - 16 h]:** Realizar el montaje y soldadura manual de componentes en 3 placas de circuito impreso (1 Concentrador y 2 Sensores A y B), verificando la ausencia de puentes, inspeccionando soldaduras y comprobando la estabilidad de voltajes de alimentación (12V, 5V y 3.3V).
2. **[Confección de Cableado Daisy Chain - 10 h]:** Fabricar y ponchar los arneses de cableado UTP con conectores RJ45 conforme a la norma de distribución T-568B (Pares: Alimentación 12V, Datos RS485 A/B, Sincronismo SINC_A/SINC_B y GND), comprobando la continuidad eléctrica con tester de red.
3. **[Desarrollo y Programación de Firmware en MikroC - 22 h]:** Configurar el oscilador interno FRCPLL a 80 MHz y desarrollar en MikroC el firmware del Concentrador (emisión de pulsos periódicos de 1000 ms / 1 ms pulse, bus UART2 a 2 Mbps, puente SPI esclavo) y de los Nodos Sensores (captura de interrupción externa INT1, validación de IDNODO y eco de enlace 0xF1/0xD2), programando los dsPIC vía MPLAB IPE y PICkit 3.
4. **[Validación Metrológica con Osciloscopio - 18 h]:** Medir experimentalmente con osciloscopio Hantek los tiempos de retardo y niveles de tensión de la señal de sincronismo entre el Concentrador y los Nodos Sensores, y entre los dos Sensores entre sí, evaluando la inmunidad al ruido de la variante con plano de masa frente a la variante sin plano.
5. **[Prueba de Concepto (PoC) de Almacenamiento MicroSD - 18 h]:** Integrar los módulos `sdcard.c/.h` y `spiSD.c/.h` en el firmware del dsPIC mediante remapeo PPS (SPI1), programar la rutina de diagnóstico por parpadeos LED e implementar la captura de sectores crudos de 512 bytes ante cada pulso de sincronismo, documentando la limitación física del zócalo sin pin *card-detect*.
6. **[Troubleshooting, Consolidación e Informe Final - 12 h]:** Documentar la resolución de anomalías (oscilador, variante de microcontrolador, sustitución de LED dañado, transceptor MAX483 vs MAX485 y zócalo SD), consolidar el repositorio en GitHub y redactar el Informe Técnico Final de prácticas.

---

## 3. Plan de Trabajo Detallado y Checkpoints Cuantificables

### Fase 1: Ensamblaje, Soldadura e Inspección de Hardware
* **Duración:** 16 horas (Semana 1)
* **Objetivo:** Manufacturar manualmente las 3 placas electrónicas de la serie Acelerógrafo V1.4 y comprobar su integridad eléctrica preliminar.
* **Actividades técnicas:**
  1. Montaje y soldadura manual de componentes en la placa del Nodo Concentrador: zócalo DIP-28 para dsPIC33EP256MC202, 2 transceptores diferenciales (MAX485 para datos y MAX483 para sincronismo), reguladores de tensión, conector RJ45 hembra (J1), terminales de potencia y LEDs de estado.
  2. Montaje y soldadura manual en las 2 placas de Nodos Sensores (Versión A y Versión B): microcontroladores dsPIC, acelerómetro triaxial ADXL355, transceptor receptor MAX483, transceptor MAX485, dos puertos RJ45 pasantes (J2 entrada, J3 salida) y lector MicroSD.
  3. Inspección óptica con lupa de aumento para descartar puentes de estaño, uniones frías o cortocircuitos entre pines de paso fino.
  4. Sustitución del LED de estado dañado en el Nodo Sensor tras identificarse un pin en corto por discrepancia de variante de microcontrolador.
  5. Energización individual de cada placa: verificar voltajes regulados estables (+5.0 V y +3.3 V) y comprobar que no exista sobrecalentamiento térmico en reguladores.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 1.1:** Tres placas físicas (1 Concentrador y 2 Nodos Sensores) completamente soldadas y limpias.
  - [ ] **CP 1.2:** Inspección visual aprobada sin cortocircuitos; sustitución del LED dañado completada.
  - [ ] **CP 1.3:** Mediciones de multímetro confirmando voltajes nominales dentro de tolerancias en las 3 placas.

---

### Fase 2: Confección de Cableado Estructurado y Topología Daisy Chain
* **Duración:** 10 horas (Semana 2)
* **Objetivo:** Fabricar los arneses de cableado UTP bajo la norma T-568B y establecer la interconexión física en cascada.
* **Actividades técnicas:**
  1. Ponchado de cables UTP Categoría 5e con conectores RJ45 siguiendo estrictamente la norma T-568B:
     - **Par 1 (Azul / Blanco-Azul):** Alimentación positiva (+12 V).
     - **Par 2 (Naranja / Blanco-Naranja):** Bus de datos RS485 diferencial (Líneas A / B).
     - **Par 3 (Verde / Blanco-Verde):** Señal de sincronización diferencial dedicada (SINC_A / SINC_B).
     - **Par 4 (Marrón / Blanco-Marrón):** Retorno común de alimentación (GND).
  2. Validación de continuidad física y correspondencia pin a pin mediante un tester de cables de red.
  3. Interconexión en cascada: Concentrador (J1) $\rightarrow$ Nodo Sensor A (J2 entrada, J3 salida pasante) $\rightarrow$ Nodo Sensor B (J2 entrada).
  4. Comprobar la correcta distribución de energía a lo largo de la cadena (alimentación centralizada desde el Concentrador hacia los sensores).
* **Checkpoints medibles para revisión:**
  - [ ] **CP 2.1:** Cables UTP ponchados y aprobados con tester de red con mapeo correcto bajo T-568B.
  - [ ] **CP 2.2:** Cadena Daisy Chain interconectada físicamente propagando alimentación y líneas diferenciales.

---

### Fase 3: Desarrollo y Adaptación de Firmware en MikroC para dsPIC
* **Duración:** 22 horas (Semanas 2 y 3)
* **Objetivo:** Desarrollar e implementar el firmware de control en MikroC PRO for dsPIC, corrigiendo la fuente de reloj y habilitando la comunicación RS485 a 2 Mbps.
* **Actividades técnicas:**
  1. **Corrección de Configuración del Oscilador:** Modificar los *Configuration Bits* del dsPIC en MikroC para conmutar de *Primary Oscillator* a *FRCPLL* (Fast RC interno con multiplicador PLL), alcanzando 80 MHz nominales (40 MIPS) sin cristal externo.
  2. **Firmware del Nodo Concentrador:**
     - Configurar UART2 a 2 Mbps (8N1) para el bus RS485 de datos.
     - Configurar SPI1 en modo esclavo para comunicación con Raspberry Pi.
     - Implementar generador de pulso en interrupción `Timer3Int` (PR3=15625, prescaler 1:256): generar pulso de $1000\,\mu\text{s}$ cada 1000 ms en el pin `INT_SINC_1` hacia el MAX483 y conmutar LED D5 (*heartbeat*).
     - Implementar máquina de estados de recepción en `urx_2` con cabecera `[0x3A, Dir, Func, NumL, NumM]`, LED D2 de tráfico y timeout en Timer2 (300 ms).
  3. **Firmware de los Nodos Sensores A y B:**
     - Configurar interrupción externa `int_1` (RB14) acoplada a la salida del transceptor MAX483 (en escucha continua), conmutando el LED D3 (`TEST1`) en fase con el Concentrador.
     - Implementar interrupción `urx_1` para filtrado de tramas por `IDNODO` (1 para Nodo A, 2 para Nodo B).
     - Implementar función de eco de enlace (`0xF1`/`0xD2`) para responder con el ID del nodo y certificar comunicación bidireccional.
  4. Compilar firmwares en MikroC, generar archivos `.hex` y programar los dsPIC usando MPLAB IPE v6.20 y PICkit 3 (alimentando los sensores desde el enlace UTP).
* **Checkpoints medibles para revisión:**
  - [ ] **CP 3.1:** Frecuencia de reloj a 80 MHz verificada, logrando comunicación UART estable a 2 Mbps.
  - [ ] **CP 3.2:** Firmwares compilados y programados exitosamente con PICkit 3 en Concentrador y Sensores.
  - [ ] **CP 3.3:** LED D5 del Concentrador y LEDs D3 de ambos Nodos Sensores parpadeando sincrónicamente a 1 Hz.

---

### Fase 4: Integración de Red y Validación Metrológica con Osciloscopio
* **Duración:** 18 horas (Semanas 3 y 4)
* **Objetivo:** Cuantificar con osciloscopio digital los retardos de propagación del pulso de sincronismo y los niveles de tensión en topología Daisy Chain.
* **Actividades técnicas:**
  1. Configurar osciloscopio Hantek en base de tiempo rápida (200 ns/div y 40 ns/div) con disparo por flanco.
  2. Medición de retardos temporales:
     - Enlace Concentrador $\rightarrow$ Nodo Sensor A: Retardo medido de **$1.200\,\mu\text{s}$**.
     - Enlace Concentrador $\rightarrow$ Nodo Sensor B: Retardo medido de **$1.200\,\mu\text{s}$**.
     - Enlace inter-nodo (Nodo Sensor A $\rightarrow$ Nodo Sensor B): Retardo diferencial medido de apenas **$14.0\text{ ns}$**.
  3. Análisis físico del retardo: Comprobar que el retardo mayor ($1.2\,\mu\text{s}$) corresponde al tiempo de propagación intrínseco del transceptor y a la latencia de entrada de la interrupción `INT1`, mientras que el retardo acumulativo por propagación en el cableado entre nodos es despreciable ($14\text{ ns}$).
  4. Medición de niveles de tensión en la señal de sincronismo:
     - Nivel en Concentrador: $4.88\text{ V}$.
     - Nivel recibido en Nodo A y Nodo B: $3.36\text{ V}$ (caída de $1.52\text{ V}$ atribuible a diferencias en la regulación local de tensión).
     - Diferencia de nivel entre Nodos A y B: apenas $160\text{ mV}$.
  5. Comparación cualitativa de ruido eléctrico entre la placa del Sensor A (con plano GND) y la del Sensor B (sin plano GND).
* **Checkpoints medibles para revisión:**
  - [ ] **CP 4.1:** Tabla y capturas de oscilogramas documentando los retardos medidos ($1.2\,\mu\text{s}$ y $14.0\text{ ns}$).
  - [ ] **CP 4.2:** Caracterización de niveles lógicos y caída de tensión documentada.
  - [ ] **CP 4.3:** Demostración experimental de que la topología Daisy Chain no introduce retardos acumulativos críticos.

---

### Fase 5: Prueba de Concepto (PoC) de Almacenamiento en Tarjetas MicroSD
* **Duración:** 18 horas (Semanas 4 y 5)
* **Objetivo:** Implementar y evaluar el firmware de almacenamiento local por SPI en el dsPIC, diagnosticando el comportamiento del zócalo de memoria.
* **Actividades técnicas:**
  1. Configuración de periféricos por software: implementar la rutina en ensamblador `ConfigurarPPS_SPI1` para desbloquear registros de mapeo de pines del dsPIC (RPOR2, RPOR3, RPINR20), enrutando:
     - RB8 a SDO1 (MOSI)
     - RB9 a SDI1 (MISO)
     - RB7 a SCK1 (Reloj SPI)
     - RB0 a Chip Select ($\overline{\text{CS}}$)
  2. Integrar los módulos de bajo nivel `sdcard.c` y `spiSD.c` en el proyecto del Nodo Sensor:
     - Inicialización en baja velocidad `SPISD_Init(SLOW)` a 625 kHz y 80 ciclos de reloj previos.
     - Secuencia estándar de comandos: `CMD0`, `CMD8`, `CMD58`, `CMD55`, `ACMD41`, `CMD16`.
     - Funciones de bloque: `SD_Write_Block()` y `SD_Read_Block()` para tramas de 512 bytes.
  3. Programar rutinas de diagnóstico visual por código de parpadeos: `LED()` para avance normal y `LED_Error()` codificando el número de falla mediante destellos cortos seguidos de pausas largas.
  4. Implementar la rutina de registro acoplada a la interrupción de sincronismo: ante cada pulso, estructurar una trama de 512 bytes con ID de nodo, contador secuencial y patrón incremental, escribiéndola en el sector correspondiente.
  5. **Auditoría física y limitación encontrada:** Identificar que el zócalo MicroSD soldado carece del pin mecánico de detección de presencia de tarjeta (*card-detect*). Como mitigación, forzar en software `SD_DETECCION_HARDWARE = 0` y `sdflags.detected = true`.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 5.1:** Módulo SPI1 remapeado y compilado con librerías `sdcard.c` y `spiSD.c` en el dsPIC.
  - [ ] **CP 5.2:** Sistema de diagnóstico visual por códigos de destello LED operativo.
  - [ ] **CP 5.3:** Limitación de hardware del zócalo MicroSD documentada técnicamente, registrando el alcance de la prueba.

> [!NOTE]
> **Observación sobre el Alcance Alcanzado en la Prueba de Concepto (PoC):**
> La lógica de firmware y los controladores de comunicación SPI fueron desarrollados, integrados y probados satisfactoriamente según la especificación física de tarjetas SD. No obstante, debido a que el zócalo MicroSD montado en las placas carecía físicamente del contacto mecánico de detección (*card-detect*), la inicialización y persistencia de datos directa en sectores no pudo completarse de manera confiable en hardware real, incluso tras forzar la presencia por software (`sdflags.detected = true`). La prueba de concepto alcanzó exitosamente el nivel de integración de firmware, configuración de periféricos PPS y diagnóstico mediante parpadeos LED, dejando documentada la necesidad indispensable de sustituir el zócalo por uno con pin de detección física en la siguiente revisión de hardware.

---

### Fase 6: Troubleshooting, Consolidación de Repositorio e Informe Final
* **Duración:** 12 horas (Semana 6)
* **Objetivo:** Consolidar la guía técnica de solución de problemas, estructurar el repositorio en GitHub y formalizar el informe técnico de prácticas.
* **Actividades técnicas:**
  1. Estructurar la **Guía de Solución de Problemas (Troubleshooting):**
     - *Problema 1 (Velocidad UART):* Corrección del bit de configuración del oscilador de *Primary* a *FRCPLL* (80 MHz).
     - *Problema 2 (Variante dsPIC y daño de LED):* Corrección de pines en corto por variante física y reemplazo de componente averiado.
     - *Problema 3 (Limitación MicroSD):* Diagnóstico formal de la falta de pin mecánico de *card-detect* en el zócalo.
     - *Desviación de diseño:* Justificación del uso del transceptor MAX483 en la línea de sincronismo frente al MAX485 previsto.
  2. Organizar y limpiar el repositorio oficial en GitHub [RSA-Intern-Ensamblaje_SHM](https://github.com/RedSismicaAustro/RSA-Intern-Ensamblaje_SHM) en la rama `dev-pasantias`:
     - Firmwares del Concentrador y Nodos Sensores A y B.
     - Módulo común RS485 y controladores de tarjeta MicroSD.
     - Carpeta de evidencias gráficas y capturas de osciloscopio.
  3. Redactar el Informe Técnico Final formal de prácticas preprofesionales (`David_Timbi_Informe_Practicas.pdf`, 20 páginas) en formato institucional de la Universidad de Cuenca.
  4. Revisión técnica final con el tutor institucional y suscripción de actas de acreditación de 96 horas.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 6.1:** Guía de troubleshooting y memoria técnica de fallas/soluciones concluida.
  - [ ] **CP 6.2:** Repositorio en GitHub sincronizado en la rama `dev-pasantias` con código fuente completo y comentado.
  - [ ] **CP 6.3:** Documento formal institucional (`David_Timbi_Informe_Practicas.pdf`) aprobado por el tutor institucional de la RSA.

---

## 4. Resumen de Fases y Cronograma Semanal (96 Horas)

El trabajo se ejecutó a lo largo de **6 semanas** (del 30 de julio al 08 de septiembre de 2026) con una carga de **16 horas semanales**:

| Fase | Semanas | Horas | Enfoque Principal |
| :---: | :---: | :---: | :--- |
| **Fase 1** | Semana 1 | **16 h** | Ensamblaje manual de 3 placas (Concentrador, Sensores A y B), inspección visual y energización. |
| **Fase 2** | Semana 2 | **10 h** | Confección de arneses UTP ponchados bajo norma T-568B e interconexión en cascada (Daisy Chain). |
| **Fase 3** | Semanas 2–3 | **22 h** | Firmware en MikroC (Concentrador y Sensores A/B), oscilador FRCPLL 80 MHz y UART RS485 a 2 Mbps. |
| **Fase 4** | Semanas 3–4 | **18 h** | Medición de retardos con osciloscopio Hantek (1.2 µs y 14.0 ns) y niveles de tensión en Daisy Chain. |
| **Fase 5** | Semanas 4–5 | **18 h** | Prueba de Concepto MicroSD en dsPIC, remapeo PPS, diagnóstico LED y auditoría de limitación de zócalo. |
| **Fase 6** | Semana 6 | **12 h** | Guía de *troubleshooting*, consolidación de repositorio en GitHub e Informe Técnico Final. |
| **Total** | **6 Semanas** | **96 h** | **Planificación Global de Prácticas Preprofesionales** |

---

## 5. Entregables y Criterios de Éxito de la Pasantía

1. **Hardware Ensamblado y Verificado (3 Placas Físicas):**
   * 1 Placa del Nodo Concentrador (dsPIC33EP256MC202, MAX485 datos, MAX483 sincronismo, puerto RJ45 J1) completamente operativa.
   * 2 Placas de Nodos Sensores (Nodo A con plano de tierra continuo y Nodo B sin plano de masa) con puertos pasantes RJ45 J2/J3 y transceptores operativos.
   * Arnés de cableado UTP con conectores RJ45 ponchados bajo norma T-568B y validados con tester de red.
2. **Firmware de Control en MikroC PRO for dsPIC:**
   * Código fuente completo del Nodo Concentrador: generación de pulso de sincronismo de $1000\,\mu\text{s}$ a 1 Hz, máquina de estados UART2 a 2 Mbps y puente SPI esclavo.
   * Código fuente de los Nodos Sensores A y B: interrupción `INT1` para sincronismo, filtrado de tramas por `IDNODO` y eco de enlace.
3. **Validación Metrológica con Osciloscopio:**
   * Reporte de oscilogramas certificando un retardo Concentrador-Nodo de $1.200\,\mu\text{s}$ y un retardo inter-nodo en cascada de apenas $14.0\text{ ns}$, demostrando que la topología Daisy Chain no añade desfasajes acumulativos críticos.
4. **Prueba de Concepto (PoC) de Almacenamiento MicroSD:**
   * Código fuente integrado de controladores `sdcard.c` y `spiSD.c` con remapeo de pines PPS para SPI1.
   * Rutina de diagnóstico visual mediante parpadeos codificados de LED (`LED_Error()`).
   * Informe de auditoría física documentando la limitación del zócalo sin pin *card-detect* y recomendaciones para la siguiente revisión de PCB.
5. **Repositorio Institucional e Informe Técnico Final:**
   * Código fuente, archivos `.hex`, configuraciones de proyecto y diagramas alojados en: [RSA-Intern-Ensamblaje_SHM](https://github.com/RedSismicaAustro/RSA-Intern-Ensamblaje_SHM/tree/dev-pasantias).
   * Documento formal institucional de 20 páginas (`David_Timbi_Informe_Practicas.pdf`) aprobado por el tutor institucional de la RSA.
