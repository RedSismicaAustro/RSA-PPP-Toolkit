---
titulo: "Plan de Trabajo de Prácticas Laborales: Migración de Acelerógrafo a ESP32 y Sistema de Telemetría MQTT para Señales Acelerométricas"
proyecto: "Sistema de Registro Continuo de Señales Acelerométricas y Telemetría IoT en ESP32"
codigo_proyecto: "RSA-PPP-2024-04"
area_tematica: "Sistemas Embebidos, Procesamiento en Tiempo Real (FreeRTOS), IoT y Telemetría Sísmica"
estado: "Culminado"
version: "1.0"
fecha_creacion: "2024-02-08"
fecha_actualizacion: "2025-01-20"

pasante:
  nombre: "Jorge Zhangallimbay"
  cedula: "N/D"
  correo: "jorge.zhangallimbay@ucuenca.edu.ec"
  carrera: "Ingeniería en Telecomunicaciones"
  institucion: "Universidad de Cuenca"

tutoria:
  tutor_institucional: "Ing. Milton Muñoz. (Red Sísmica del Austro - RSA)"
  correo_tutor_institucional: "milton.munozc@ucuenca.edu.ec"

cronograma:
  duracion_total_horas: 240
  dedicacion_semanal_horas: 16
  duracion_semanas: 15
  fecha_inicio: "2024-02-08"
  fecha_fin_estimada: "2024-08-30"
  modalidad: "Presencial / Mixta"
  etapas_formativas:
    etapa_1: "Migración de dsPIC a ESP32 y Registro Continuo en SD (144 horas - 8 semanas)"
    etapa_2: "Sistema de Transmisión de Datos y Eventos vía MQTT en ESP32 (96 horas - 7 semanas)"

tecnologias:
  - "Microcontrolador y SoC: Espressif ESP32-WROOM-32 / DEVKIT V1 (Doble núcleo Xtensa 32-bit a 240 MHz, 520 KB SRAM)"
  - "Framework y Entornos de Desarrollo: PlatformIO IDE en VS Code, Framework Arduino-ESP32, FreeRTOS (Colas, Tareas y Mutex)"
  - "Sensores de Auscultación: Acelerómetro Triaxial de Precisión ADXL355 (Bus SPI a 250 Hz / 125 Hz con lectura continua de FIFO)"
  - "Sincronización Temporal: Módulo GPS FGPMMOPA6H (UART NMEA / pulso PPS) y Reloj RTC DS3231 con TCXO (Bus I2C / señal SQW de 1 Hz)"
  - "Almacenamiento Local: Memoria MicroSD en bus SPI dedicado (VSPI), buffers Ping-Pong en RAM y tramas binarias de 20 bytes/muestra (.bin)"
  - "Protocolos de Red e IoT: Wi-Fi 802.11 b/g/n, MQTT (PubSubClient y Paho-MQTT en Python), ArduinoJson, Last Will and Testament (LWT)"
  - "Herramientas de Análisis y Validación: Python 3 (Spyder, NumPy, SciPy, Matplotlib), Editor Hexadecimal HxD, MQTT Explorer"

repositorio:
  url: "https://github.com/RedSismicaAustro/RSA-Intern-Acelerografo_ESP32"
  rama_base: "main"
---

---

## 1. Antecedentes y Justificación Técnica

1. **Contexto del Sistema y Estado Inicial del Artefacto:** 
   La Red Sísmica del Austro (RSA) operaba sus estaciones de registro continuo de aceleraciones para monitorización de salud estructural (SHM) basadas en microcontroladores de 16 bits Microchip dsPIC33EP256MC202. Si bien dicha plataforma demostró un rendimiento confiable en la adquisición de datos del acelerómetro triaxial ADXL355, presentaba limitaciones severas de memoria RAM disponible (32 KB), capacidad de procesamiento matemático en coma flotante y una carencia total de interfaces de conectividad inalámbrica nativas. Ante este panorama, se identificó la necesidad de modernizar la arquitectura electrónica adoptando el SoC **ESP32**, cuya potencia de doble núcleo a 240 MHz, conectividad Wi-Fi integrada y soporte nativo del sistema operativo en tiempo real FreeRTOS abren la posibilidad de integrar telemetría IoT remota sin comprometer el determinismo en la adquisición acelerográfica.

2. **Limitaciones Críticas o Cuellos de Botella Identificados:** 
   * **Monociclo de Trabajo Bloqueante en dsPIC:** La gestión simultánea del acelerómetro ADXL355, la sincronización por pulsos de GPS/RTC y la escritura física de sectores en la MicroSD provocaba riesgos de desbordamiento de búfer y pérdida de muestras en el microcontrolador previo ante demoras en el bus SPI.
   * **Precisión y Deriva Temporal:** Para aplicaciones sismológicas y de ingeniería estructural, las series de tiempo exigen una sincronización temporal estricta ($\le 1\text{ ms}$). Se requería implementar un esquema híbrido de sincronismo en ESP32 que combinara la hora satelital absoluta del GPS (tramas NMEA) y el pulso por segundo (PPS/SQW) del RTC DS3231 para gobernar un temporizador de hardware de alta resolución.
   * **Sobrecarga de Serialización en Telemetría:** Al habilitar la transmisión inalámbrica hacia servidores centrales mediante el protocolo MQTT, la transmisión de 250 muestras por segundo en formato JSON tradicional satura el ancho de banda y la memoria del microcontrolador debido al excesivo overhead textual, exigiendo la evaluación rigurosa de tramas binarias compactas.
   * **Resiliencia ante Desconexiones de Red:** Los nodos sísmicos operan en ambientes hostiles; el firmware requería mecanismos autónomos de reconexión Wi-Fi/MQTT y la emisión automática de mensajes de estado (*Last Will and Testament* - LWT) para notificar caídas de enlace al centro de monitoreo.

3. **Desacoplamiento del Alcance y Propuesta de Solución:** 
   El plan de trabajo del estudiante se dividió en **dos proyectos complementarios** que suman un total de **240 horas**:
   * **Etapa 1 (144 Horas - Migración de Hardware/Firmware y Adquisición Continua):** Sustituir el dsPIC por el ESP32, desarrollar en PlatformIO con FreeRTOS los controladores modulares para el ADXL355 (lectura de FIFO a 250 Hz por SPI), módulo GPS FGPMMOPA6H (UART), RTC DS3231 (I2C y temporizador de 1 ms guiado por SQW), y persistencia desatendida en MicroSD con estructuras binarias de 20 bytes por muestra validadas mediante scripts de Python y HxD.
   * **Etapa 2 (96 Horas - Transmisión Telemática y Eventos Bajo Demanda por MQTT):** Diseñar e implementar el cliente MQTT en el ESP32 con gestión de red Wi-Fi, publicación periódica de estados en JSON, testamento LWT, protocolo de transmisión de ventanas de eventos bajo demanda (`request`/`response`/`data`), y desarrollar un software receptor en Python (Paho-MQTT) para cuantificar la eficiencia de la transmisión binaria frente a JSON en términos de latencia, jitter y pérdida de paquetes.

---

## 2. Objetivos del Plan de Trabajo

### 2.1. Objetivo General
Diseñar, implementar y caracterizar el circuito electrónico y firmware modular en ESP32 bajo FreeRTOS para la migración completa del sistema acelerográfico continuo (ADXL355, GPS, RTC y MicroSD) asegurando adquisición determinística a 250 Hz con sincronismo temporal de precisión (144 h), e implementar una plataforma de telemetría IoT mediante el protocolo MQTT sobre Wi-Fi para la transmisión de estados, testamento LWT y eventos sísmicos bajo demanda evaluados en Python (96 h).

### 2.2. Objetivos Específicos
1. **[Configuración de Entorno y Hardware Base - 18 h]:** Configurar el entorno de desarrollo profesional en PlatformIO (VS Code) e implementar el prototipo de interconexión física en protoboard integrando el ESP32 con el acelerómetro ADXL355, módulo GPS FGPMMOPA6H, RTC DS3231 y zócalo MicroSD.
2. **[Controlador SPI del ADXL355 y FIFO - 36 h]:** Desarrollar la librería modular en C++ para el acelerómetro ADXL355, configurando registros para muestreo a 250 Hz (y 125 Hz) con lectura continua y eficiente del búfer FIFO interno de 32 niveles.
3. **[Sincronización Híbrida GPS/RTC - 36 h]:** Desarrollar la lógica de sincronismo de tiempo absoluto capturando tramas NMEA desde el GPS por UART, sincronizando el RTC DS3231 vía I2C y configurando una interrupción externa en el flanco de bajada de la señal SQW (1 Hz) acoplada a un temporizador de hardware de 1 ms.
4. **[Almacenamiento Concurrente en MicroSD - 36 h]:** Diseñar la arquitectura multihilo en FreeRTOS con doble búfer en RAM para empaquetar muestras de aceleración triaxial con estampas temporales (trama binaria de 20 bytes) y persistirlas continuamente en el archivo `data0.bin` de la tarjeta MicroSD.
5. **[Validación de Adquisición y Suite en Python - 18 h]:** Desarrollar un script en Python (Spyder) para decodificar las tramas binarias de la MicroSD, validar el cumplimiento estricto de la tasa de muestreo de 250 Hz sin vacíos temporales y graficar las ondas sísmicas.
6. **[Infraestructura Wi-Fi y Broker MQTT - 32 h]:** Configurar la pila de red Wi-Fi en ESP32 con reconexión desatendida y programar el cliente MQTT (PubSubClient) con tópicos de estado del dispositivo (`status`), reconexión y testamento *Last Will and Testament* (LWT) en formato JSON.
7. **[Transmisión de Eventos Sísmicos Bajo Demanda - 36 h]:** Diseñar e implementar el protocolo bidireccional de solicitud y despacho de eventos (`request`, `response` y `data`), evaluando el desempeño de transmisión entre cargas útiles JSON y tramas binarias compactas.
8. **[Receptor en Python, Benchmarking y Documentación Final - 28 h]:** Implementar el cliente suscriptor en Python utilizando la librería Paho-MQTT para recepción en tiempo real, cálculo de latencia de red y pérdida de paquetes, y redactar la Guía de Usuario formal del sistema (`RSA_User_Guide.pdf` / `Informe 4.pdf`).

---

## 3. Plan de Trabajo Detallado y Checkpoints Cuantificables

### PARTE I: Migración de dsPIC a ESP32 y Registro Continuo Acelerométrico (144 Horas)

#### Fase 1: Introducción, Entorno PlatformIO y Prototipado Físico
* **Duración:** 18 horas (Semana 1)
* **Objetivo:** Analizar la arquitectura heredada en dsPIC, configurar el entorno PlatformIO y ensamblar el circuito base del acelerógrafo con ESP32.
* **Actividades técnicas:**
  1. Revisar la documentación técnica del nodo sensor dsPIC33EP, esquemas de conexión y librerías previas del ADXL355, GPS y RTC.
  2. Instalar y configurar VS Code, la extensión PlatformIO IDE, toolchain de Espressif y soporte del framework Arduino.
  3. Estructurar el proyecto modular `MEMS0/` (`platformio.ini`, carpetas `include/`, `src/`, `lib/`).
  4. Realizar el montaje físico de los módulos en protoboard según el diagrama esquemático: ESP32-WROOM-32, ADXL355, GPS FGPMMOPA6H, RTC DS3231 y zócalo MicroSD.
  5. Cargar firmwares de testeo elemental de puertos GPIO, UART, I2C y buses SPI.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 1.1:** Entorno PlatformIO configurado y compilando correctamente programas básicos para el ESP32.
  - [ ] **CP 1.2:** Montaje en protoboard completado e inspeccionado eléctricamente con líneas de alimentación desacopladas (3.3V / 5V).
  - [ ] **CP 1.3:** Proyecto base `MEMS0` inicializado en el repositorio institucional de GitHub.

---

#### Fase 2: Comunicación SPI y Controlador Modular del ADXL355
* **Duración:** 18 horas (Semana 2)
* **Objetivo:** Implementar las rutinas de bajo nivel en C++ para la comunicación SPI y configuración de registros del sensor acelerométrico ADXL355.
* **Actividades técnicas:**
  1. Analizar el datasheet del acelerómetro ADXL355 (mapa de registros, rangos de escala $\pm 2g$, $\pm 4g$, $\pm 8g$ y filtros ODR).
  2. Implementar en `ADXL355.cpp/.h` las funciones básicas de bus SPI:
     - `writeRegister(byte thisRegister, byte thisValue)`: Escritura de parámetros en registros de control.
     - `readRegistry(byte thisRegister)`: Lectura de registros de estado e identificación (DEVID_AD, PARTID).
  3. Desarrollar la rutina `isDataReady()` para consultar el bit `DTA_RDY` en el registro `STATUS`.
  4. Programar la lectura directa de los registros de aceleración (`XDATA3..1`, `YDATA3..1`, `ZDATA3..1`) y conversión a valores físicos en $\text{cm/s}^2$ mediante `convert_data()`.
  5. Ensayos en reposo e inspección visual de las lecturas triaxiales en el monitor serie.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 2.1:** Identificadores del ADXL355 leídos correctamente por SPI coincidiendo con los valores de fábrica.
  - [ ] **CP 2.2:** Módulo `ADXL355.cpp/.h` integrado con funciones de lectura/escritura operativas.
  - [ ] **CP 2.3:** Lectura coherente de gravedad estática ($\approx 9.81\text{ m/s}^2$ en el eje Z) documentada.

---

#### Fase 3: Configuración Avanzada del ADXL355 y Lectura Continua del FIFO
* **Duración:** 18 horas (Semana 3)
* **Objetivo:** Configurar la tasa de datos de salida a 250 Hz (ODR) y desarrollar la lectura en ráfaga (*burst*) desde la memoria FIFO interna del sensor.
* **Actividades técnicas:**
  1. Configurar los registros de filtro del ADXL355 (`FILTER`) fijando `ODR_250_HZ = 0x05` (filtro pasobajo en 62.5 Hz).
  2. Habilitar y configurar la memoria FIFO interna (profundidad de 32 conjuntos de datos triaxiales).
  3. Implementar la función `readFIFOData(uint8_t *buffer, int length)` para descargar ráfagas completas de muestras reduciendo transacciones en el bus SPI.
  4. Medir y validar en osciloscopio la periodicidad de llenado del FIFO frente al tiempo de lectura del ESP32.
  5. Validar empíricamente la estabilidad de adquisición continua sin pérdidas a 250 Hz y 125 Hz.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 3.1:** Registro de filtro del ADXL355 configurado exitosamente a 250 Hz de tasa de salida.
  - [ ] **CP 3.2:** Rutina `readFIFOData()` operativa extrayendo ráfagas de aceleración sin desbordamiento del FIFO.
  - [ ] **CP 3.3:** Verificación de 250 conjuntos de datos triaxiales por segundo en banco de pruebas.

---

#### Fase 4: Integración y Sincronización Temporal Híbrida (GPS y RTC)
* **Duración:** 36 horas (Semanas 4 y 5)
* **Objetivo:** Establecer la sincronización horaria de alta precisión combinando el módulo satelital GPS y el reloj de tiempo real RTC compensado por temperatura.
* **Actividades técnicas:**
  1. Desarrollar el módulo `GPS.cpp/.h`: configurar puerto UART para recibir sentencias NMEA (`$GPRMC`) a 9600 baudios desde el GPS FGPMMOPA6H.
  2. Implementar funciones de parseo de tiempo satelital:
     - `RecuperarFechaGPS()`: Extracción de fecha en formato numérico `AAAAMMDD`.
     - `RecuperarHoraGPS()`: Extracción de hora en segundos transcurridos desde el inicio del día con ajuste UTC-5.
  3. Desarrollar el módulo `RTC3231.cpp/.h` en bus I2C:
     - `updateRTCfromGPS()`: Calibración automática del reloj RTC a partir de la sentencia GPS válida.
     - `updateRTCFromNTP()`: Rutina de respaldo alternativa para sincronización desde servidores de red NTP.
  4. Configurar la salida de onda cuadrada del RTC (`SQW`) a una frecuencia exacta de 1 Hz.
  5. Programar la interrupción externa `sqwInterrupt()` por flanco de bajada para capturar el pulso de 1 segundo.
  6. Configurar el temporizador de hardware de 64 bits (`hw_timer_t`) en `setupTimer()` para generar interrupciones periódicas cada 1 ms, incrementando la variable atómica `millis_timestamp`.
  7. Desarrollar la tarea FreeRTOS de sincronización `sync_task()` para reajustar periódicamente el contador interno ante la llegada de la señal SQW.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 4.1:** Módulo GPS extrayendo fecha y hora absoluta sin retardos de decodificación UART.
  - [ ] **CP 4.2:** RTC DS3231 sincronizado y emitiendo pulso de 1 Hz por su pin SQW con deriva inferior a 2 ppm.
  - [ ] **CP 4.3:** Temporizador por hardware de 1 ms calibrado y sincronizado con la estampa de tiempo absoluta del sistema.

---

#### Fase 5: Manejo de Búferes, Empaquetamiento Binario y Almacenamiento MicroSD
* **Duración:** 36 horas (Semanas 6 y 7)
* **Objetivo:** Implementar la arquitectura multihilo de almacenamiento continuo en tarjeta MicroSD mediante tramas binarias estructuradas de 20 bytes.
* **Actividades técnicas:**
  1. Definir la estructura formal binaria de cada muestra de aceleración (20 bytes totales):
     - `Fecha`: 4 bytes (`uint32_t`, formato AAAAMMDD).
     - `Milisegundos`: 4 bytes (`uint32_t`, segundos del día $\times 1000$ + ms).
     - `Aceleración X`: 4 bytes (formato entero de 32 bits procesado).
     - `Aceleración Y`: 4 bytes.
     - `Aceleración Z`: 4 bytes.
  2. Desarrollar el módulo `SD_mod.cpp/.h` utilizando una instancia dedicada `SPIClass spiSD(VSPI)` para evitar colisiones con el bus del ADXL355.
  3. Diseñar la estrategia de doble búfer en memoria RAM: `writeBuffer` (búfer activo de escritura a tarjeta) y `myBuffer` (búfer colector de muestras).
  4. Programar la tarea FreeRTOS `acelerometroTask()` para recolectar las muestras del FIFO, asociarles la estampa de tiempo generada por `generate_buffer_timestamp()` y volcar los bloques hacia la MicroSD en el archivo `data0.bin` mediante `writeToFile()`.
  5. Realizar pruebas sostenidas de registro (1, 6 y 12 horas) y calcular la tasa de consumo de almacenamiento (~432 MB por día a 250 Hz).
* **Checkpoints medibles para revisión:**
  - [ ] **CP 5.1:** Estructura de trama de 20 bytes implementada y verificada a nivel binario.
  - [ ] **CP 5.2:** Tarea FreeRTOS `acelerometroTask()` operando concurrentemente sin interferir con la sincronización temporal.
  - [ ] **CP 5.3:** Registro continuo sostenido en archivo binario `data0.bin` en la MicroSD sin bloqueos del bus SPI.

---

#### Fase 6: Pruebas Integradas de Larga Duración, Suite en Python y Validación HxD
* **Duración:** 18 horas (Semana 8)
* **Objetivo:** Validar experimentalmente la integridad, continuidad y exactitud de las series de tiempo mediante herramientas de análisis en PC.
* **Actividades técnicas:**
  1. Desarrollar un script en Python (Spyder) para la extracción y procesamiento de `data0.bin`:
     - Desempaquetado de bloques binarios mediante `struct.unpack`.
     - Algoritmo de verificación de continuidad temporal: conteo del número exacto de muestras por segundo (confirmación de 250 muestras/s).
     - Graficación interactiva de las componentes triaxiales ($X, Y, Z$) ante perturbaciones inducidas.
  2. Ejecutar análisis forense de la tarjeta MicroSD utilizando el editor hexadecimal HxD, comprobando cabeceras, monotonía de marcas temporales e integridad de los bloques de 20 bytes.
  3. Ejecutar pruebas comparativas frente al acelerógrafo original dsPIC para corroborar respuesta dinámica y piso de ruido.
  4. Documentar los resultados de la migración y consolidar el firmware en el repositorio GitHub.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 6.1:** Script en Python operativo graficando series temporales y certificando 250 muestras exactas por segundo.
  - [ ] **CP 6.2:** Registros binarios inspeccionados en HxD sin corrupción de bytes ni solapamiento de sectores.
  - [ ] **CP 6.3:** Cierre de la Etapa 1 con firmware de adquisición continua 100% operativo en ESP32.

---

### PARTE II: Sistema de Telemetría IoT y Transmisión de Eventos vía MQTT (96 Horas)

#### Fase 7: Conectividad Wi-Fi Robusta y Reconexión Automática en ESP32
* **Duración:** 16 horas (Semana 9)
* **Objetivo:** Implementar la capa de red inalámbrica Wi-Fi en el ESP32 garantizando conectividad ininterrumpida y recuperación automática ante fallas de enlace.
* **Actividades técnicas:**
  1. Estudiar y configurar la librería `WiFi.h` en el ESP32 para conexión en modo estación (STA).
  2. Implementar una máquina de estados para gestión de conexión con credenciales predefinidas.
  3. Desarrollar la rutina de reconexión automática no bloqueante en caso de pérdida de señal o reinicio del punto de acceso.
  4. Programar un sistema de registro de eventos (logs) en puerto serie y memoria para auditar estados de conexión, dirección IP asignada y RSSI.
  5. Ejecutar ensayos de estrés simulando caídas de red y cortes de energía para medir tiempos de restablecimiento.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 7.1:** Módulo Wi-Fi operativo con conexión estable a la red de prueba institucional.
  - [ ] **CP 7.2:** Reconexión automática validada en laboratorio con tiempo de recuperación inferior a 5 segundos tras caída de red.

---

#### Fase 8: Enlace con Broker MQTT, Mensajería de Estado y Testamento LWT
* **Duración:** 16 horas (Semana 10)
* **Objetivo:** Establecer la comunicación con el broker MQTT institucional e implementar mensajería de diagnóstico y detección de caídas mediante LWT.
* **Actividades técnicas:**
  1. Configurar la librería `PubSubClient.h` en el ESP32 y verificar conexión con el broker MQTT mediante la herramienta MQTT Explorer.
  2. Definir la topología de tópicos base: `status`, `events/{id_dispositivo}/request`, `events/{id_dispositivo}/response`, `events/{id_dispositivo}/data`.
  3. Diseñar la estructura de mensajes en JSON utilizando `ArduinoJson`:
     - Publicación de `{"id": "ESP32_001", "status": "on"}` al arrancar el nodo.
     - Publicación de `{"id": "ESP32_001", "status": "online"}` tras reconexión exitosa.
  4. Configurar el mecanismo de **Testamento MQTT (Last Will and Testament - LWT)** en el broker:
     - Formato del mensaje: `{"id": "ESP32_001", "status": "offline"}` en el tópico `status`.
  5. Validar la emisión del testamento desconectando abruptamente la alimentación del ESP32.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 8.1:** Comunicación bidireccional estable entre ESP32 y broker MQTT con credenciales de autenticación.
  - [ ] **CP 8.2:** Mensajes JSON de encendido y reconexión recibidos correctamente en el tópico `status`.
  - [ ] **CP 8.3:** Publicación automática del mensaje LWT verificada en el broker tras desconexión no planificada del nodo.

---

#### Fase 9: Arquitectura de Solicitud y Transmisión de Eventos Bajo Demanda
* **Duración:** 20 horas (Semana 11)
* **Objetivo:** Implementar el protocolo de telemetría bajo demanda para despachar ventanas de datos acelerométricos a petición del servidor central.
* **Actividades técnicas:**
  1. Diseñar el esquema de solicitud (Request):
     - Tópico de escucha: `events/{id_dispositivo}/request`.
     - Carga útil JSON: `{"id": "ESP32_001", "timestamp": "1698789123", "duration": 5}`.
  2. Programar en el ESP32 la función callback de recepción y parseo del mensaje de solicitud.
  3. Implementar el canal de confirmación (Response):
     - Tópico: `events/{id_dispositivo}/response`.
     - Mensaje JSON con estados progresivos: `received`, `processing`, `completed` o `error`.
  4. Vincular la solicitud con el subsistema de almacenamiento para ubicar el puntero de datos del evento solicitado según el timestamp.
  5. Ensayos de despacho simulando comandos de solicitud manuales desde MQTT Explorer.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 9.1:** Callback MQTT operativo interpretando parámetros de timestamp y duración sin bloqueos.
  - [ ] **CP 9.2:** Máquina de estados de respuesta emitiendo confirmaciones en `events/{id}/response` con estados coherentes.

---

#### Fase 10: Optimización de Formatos de Transmisión: JSON vs. Binario
* **Duración:** 16 horas (Semana 12)
* **Objetivo:** Desarrollar y contrastar experimentalmente dos alternativas de empaquetado de muestras para el tópico `events/{id_dispositivo}/data`.
* **Actividades técnicas:**
  1. **Alternativa 1 (Formato JSON):**
     - Empaquetar bloques de 1 segundo de muestras estructurados con timestamp y arreglos de coordenadas: `{"timestamp": "...", "data": [{"x": ..., "y": ..., "z": ...}, ...]}`.
     - Evaluar la fragmentación y memoria requerida en el ESP32.
  2. **Alternativa 2 (Formato Binario Compacto):**
     - Empaquetar las muestras directamente en un búfer de bytes plano optimizado (reduciendo overhead de encabezados textuales).
  3. Medir el volumen de bytes transmitidos por segundo en ambas modalidades para una tasa de 250 Hz.
  4. Documentar el consumo de ancho de banda y la carga computacional en el ESP32.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 10.1:** Rutina de empaquetado JSON funcional para transmisión de eventos en MQTT.
  - [ ] **CP 10.2:** Rutina de transmisión binaria compacta implementada y validada en el tópico de datos.
  - [ ] **CP 10.3:** Tabla analítica comparativa de consumo de ancho de banda (JSON vs. binario).

---

#### Fase 11: Suite de Recepción, Procesamiento y Métricas de Red en Python
* **Duración:** 16 horas (Semana 13)
* **Objetivo:** Desarrollar en PC un cliente suscriptor en Python para decodificar los eventos MQTT y cuantificar el desempeño de red.
* **Actividades técnicas:**
  1. Desarrollar el script receptor en Python utilizando la librería `paho-mqtt`:
     - Suscripción a `events/+/response` y `events/+/data`.
     - Manejo de reconexión automática y logs con marcas de tiempo locales.
  2. Implementar decodificadores específicos:
     - Parser JSON para extraer y almacenar series temporales en archivos `.csv`.
     - Parser binario para reconstruir matrices de aceleración desde flujos de bytes crudos.
  3. Implementar funciones de evaluación de rendimiento y métricas:
     - **Latencia de transmisión:** Diferencia temporal entre el timestamp del ESP32 y la hora de recepción en Python.
     - **Tasa de pérdida de paquetes:** Verificación de integridad secuencial de bloques de datos.
     - **Eficiencia de canal:** Comparación cuantitativa del rendimiento de JSON vs. binario.
  4. Generación automática de reportes gráficos y estadísticos de desempeño.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 11.1:** Script Python con Paho-MQTT recibiendo y decodificando eventos en tiempo real.
  - [ ] **CP 11.2:** Exportación estructurada de aceleraciones a archivos CSV / binarios en PC.
  - [ ] **CP 11.3:** Reporte cuantitativo de latencia, pérdida de paquetes y uso de canal generado por la suite.

---

#### Fase 12: Pruebas Integrales de Transmisión, Guía de Usuario y Cierre
* **Duración:** 12 horas (Semanas 14 y 15)
* **Objetivo:** Ejecutar ensayos integrales de campo en banco, estructurar la documentación de ingeniería y redactar el informe técnico final.
* **Actividades técnicas:**
  1. Ejecución de ensayos continuos de 24 horas: adquisición a 250 Hz con ADXL355, sincronismo GPS/RTC, almacenamiento en SD y atención concurrente de eventos por MQTT.
  2. Elaborar la Guía de Usuario detallada (`RSA_User_Guide.pdf` / `Informe 4.pdf`) conteniendo:
     - Guías de instalación paso a paso de VS Code, PlatformIO, Spyder y HxD.
     - Documentación exhaustiva de las funciones modulares (`ADXL355`, `GPS`, `RTC`, `SD_mod`).
     - Diagrama esquemático de conexiones, flujograma de FreeRTOS y estructura de tramas binarias.
     - Manual de uso del script de análisis en Python y configuración del cliente MQTT.
  3. Consolidar el repositorio en GitHub (`https://github.com/RedSismicaAustro/RSA-Intern-Acelerografo_ESP32`) con código fuente limpio, comentado y versionado.
  4. Revisión técnica final con el tutor institucional y suscripción de actas de acreditación de 240 horas.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 12.1:** Sistema completo validado en prueba de estrés continua de 24 horas sin fallos ni desincronización.
  - [ ] **CP 12.2:** Documento formal institucional (`Informe 4.pdf` / `RSA_User_Guide.pdf`, 32 páginas) entregado y aprobado.
  - [ ] **CP 12.3:** Repositorio en GitHub sincronizado y documentación de cierre de pasantías suscrita.

---

## 4. Resumen de Fases y Cronograma Semanal (240 Horas)

El plan de trabajo global comprende **240 horas** distribuidas a lo largo de **15 semanas** en dos etapas formativas y técnicas:

| Etapa | Fase | Semanas | Horas | Enfoque Principal |
| :---: | :---: | :---: | :---: | :--- |
| **I (144 h)** | **Fase 1** | Semana 1 | **18 h** | Entorno PlatformIO, arquitectura ESP32 vs dsPIC y prototipado físico. |
| **I (144 h)** | **Fase 2** | Semana 2 | **18 h** | Comunicación SPI, registros y lecturas básicas del ADXL355. |
| **I (144 h)** | **Fase 3** | Semana 3 | **18 h** | Muestreo a 250 Hz, FIFO interno y adquisición continua de aceleración. |
| **I (144 h)** | **Fase 4** | Semanas 4–5 | **36 h** | Sincronización híbrida: GPS (NMEA/PPS), RTC DS3231 (SQW 1 Hz) y timer 1 ms. |
| **I (144 h)** | **Fase 5** | Semanas 6–7 | **36 h** | Tarea FreeRTOS, doble buffer en RAM y escritura binaria en MicroSD (`data0.bin`). |
| **I (144 h)** | **Fase 6** | Semana 8 | **18 h** | Suite en Python para verificación de ODR (250 Hz), graficación y HxD. |
| **II (96 h)** | **Fase 7** | Semana 9 | **16 h** | Conectividad Wi-Fi no bloqueante y reconexión automática en ESP32. |
| **II (96 h)** | **Fase 8** | Semana 10 | **16 h** | Enlace broker MQTT, mensajes de encendido/reconexión y testamento LWT. |
| **II (96 h)** | **Fase 9** | Semana 11 | **20 h** | Protocolo de eventos bajo demanda (`request`, `response` y `data`). |
| **II (96 h)** | **Fase 10** | Semana 12 | **16 h** | Empaquetamiento y benchmark de formatos de transmisión: JSON vs. Binario. |
| **II (96 h)** | **Fase 11** | Semana 13 | **16 h** | Suite receptora en Python con Paho-MQTT, métricas de latencia y pérdida. |
| **II (96 h)** | **Fase 12** | Semanas 14–15 | **12 h** | Ensayos de estrés continuos, Guía de Usuario (`RSA_User_Guide.pdf`) y cierre. |
| **Total** | **12 Fases** | **15 Semanas** | **240 h** | **Planificación Consolidada de Prácticas Laborales** |

---

## 5. Entregables y Criterios de Éxito de la Pasantía

1. **Firmware de Adquisición Continua en ESP32 (Etapa I):**
   * Proyecto modular en PlatformIO estructurado en módulos C++:
     * `ADXL355.cpp/.h`: Controlador SPI para acelerómetro a 250 Hz con descarga de FIFO.
     * `GPS.cpp/.h`: Receptor y decodificador de sentencias NMEA satelitales.
     * `RTC3231.cpp/.h`: Sincronizador I2C y captura de interrupciones SQW a 1 Hz.
     * `SD_mod.cpp/.h`: Gestor de MicroSD en bus VSPI con doble búfer.
   * Lógica concurrente en FreeRTOS con temporizador de hardware de 1 ms (`millis_timestamp`) e interrupciones atómicas mediante mutex (`portMUX_TYPE`).
2. **Firmware de Telemetría IoT y Eventos MQTT (Etapa II):**
   * Cliente MQTT con tópicos de diagnóstico y estado (`status`) con reconexión automática.
   * Implementación de testamento *Last Will and Testament* (LWT) para notificación de desconexión.
   * Mecanismo bidireccional de atención de eventos bajo demanda por timestamp y duración en formatos JSON y binario.
3. **Suite de Procesamiento y Análisis en Python:**
   * Script para lectura y decodificación de tramas binarias de la MicroSD (`data0.bin`), verificación de tasa de 250 muestras/s y graficación interactiva de series temporales.
   * Script receptor con `paho-mqtt` para suscripción en tiempo real, guardado en CSV y análisis de latencia, tasa de pérdida y uso de ancho de banda.
4. **Repositorio Institucional en GitHub:**
   * Código fuente completo, librerías, configuraciones de PlatformIO y esquemas de conexión alojados en: [RSA-Intern-Acelerografo_ESP32](https://github.com/RedSismicaAustro/RSA-Intern-Acelerografo_ESP32).
5. **Documentación Técnica y Guía de Usuario Formal:**
   * Documento institucional formal de 32 páginas (`RSA_User_Guide.pdf` / `Informe 4.pdf`) que contiene manuales de instalación de software, descripción detallada del código, diagramas de flujo, esquemas de conexión y validaciones experimentales.
