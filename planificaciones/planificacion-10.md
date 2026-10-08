---
titulo: "Plan de Trabajo de Prácticas Preprofesionales: Actualización Tecnológica de un Sensor Ultrasónico para Medición de Nivel con Precisión Milimétrica (Migración dsPIC a ESP32 con MicroPython)"
proyecto: "Sensor Ultrasónico de Nivel de Alta Precisión para Vertederos y Filtraciones de Presas (Migración a ESP32 con MicroPython)"
codigo_proyecto: "RSA-PPP-2026-10"
area_tematica: "Sistemas Embebidos, Procesamiento Digital de Señales (DSP), MicroPython (ulab) e Instrumentación Geotécnica"
estado: "En Ejecución"
version: "1.0"
fecha_creacion: "2026-08-24"
fecha_actualizacion: "2026-09-25"

pasante:
  nombre: "Franklin Andrade"
  cedula: "N/D"
  correo: "franklin.andrade@ucuenca.edu.ec"
  carrera: "Ingeniería en Telecomunicaciones"
  institucion: "Universidad de Cuenca"

tutoria:
  tutor_institucional: "Ing. Milton Muñoz. (Red Sísmica del Austro - RSA)"
  correo_tutor_institucional: "milton.munozc@ucuenca.edu.ec"

cronograma:
  duracion_total_horas: 96
  dedicacion_semanal_horas: 16
  duracion_semanas: 6
  fecha_inicio: "2026-08-24"
  fecha_fin_estimada: "2026-10-05"
  modalidad: "Presencial"

tecnologias:
  - "Microcontrolador y SoC: Espressif ESP32 / ESP32-S3 (Doble núcleo a 240 MHz, conectividad y soporte DMA)"
  - "Entorno de Programación e IDE: MicroPython oficial, Thonny IDE (depuración interactiva vía REPL)"
  - "Procesamiento Digital de Señales (DSP): Librería matemática ulab (módulo estilo NumPy para MicroPython, arreglos vectoriales y ulab.numpy.fft)"
  - "Periféricos Embebidos: machine.PWM a 40 kHz, ADC continuo con soporte DMA/I2S (≥200 kSPS), bus 1-Wire (ds18x20)"
  - "Sensores de Campo: Transductor piezoeléctrico ultrasónico de 40 kHz y sensor de temperatura sumergible DS18B20"
  - "Circuito Analógico Base: Driver emisor de alta tensión a 40 kHz y amplificador operacional TL974 con filtrado pasa-banda activo"
  - "Instrumental de Laboratorio y Prototipado: Osciloscopio Digital, Multímetro, Placa perforada de prototipado y Maqueta de filtraciones RSA"

repositorio:
  url: "https://github.com/RSA-PPP/ppp-2026-10-sensor-ultrasonico-esp32"
  rama_base: "main"

topics:
  - rsa-ppp
  - ucuenca
  - dam-monitoring
  - ultrasonic-sensor
  - esp32
  - micropython
  - dsp
  - python
---

---

## 1. Antecedentes y Justificación Técnica

1. **Contexto del Sistema y Estado Inicial del Artefacto:** 
   La Red Sísmica del Austro (RSA) opera una maqueta experimental para la monitorización de filtraciones y niveles de agua en vertederos de presas mediante ultrasonido de alta resolución. El sistema analógico original fue concebido sobre un microcontrolador Microchip dsPIC33FJ32MC202 (zócalo DIP-28), encargado de disparar un tren de pulsos de excitación a 40 kHz hacia un transductor piezoeléctrico de potencia y capturar la señal de eco acústico mediante una etapa receptora de acondicionamiento de alta ganancia basada en el amplificador operacional TL974 (con filtrado antialiasing y pasa-banda activo), incorporando además un sensor digital de temperatura DS18B20 para la compensación de la velocidad del sonido en el aire.

2. **Limitaciones Críticas o Cuellos de Botella Identificados:** 
   * **Obsolescencia y Rigidez en dsPIC:** La programación en C tradicional bajo compiladores privativos dificulta la iteración rápida de algoritmos avanzados de procesamiento digital de señales (DSP), análisis de correlación y visualización inmediata de datos en laboratorio.
   * **Compensación Térmica y Deriva Acústica:** La velocidad del sonido en el aire varía de forma no lineal con la temperatura ambiental conforme a la relación $v(T) = 331.45 \cdot \sqrt{1 + T/273.15}\text{ m/s}$. Variaciones térmicas de pocos grados en galerías o túneles de presas introducen errores métricos de varios milímetros si no se ejecuta una compensación continua con el sensor DS18B20.
   * **Zona Ciega y Ruido en la Adquisición:** La excitación inicial del transductor provoca un anillo de oscilación residual (*ringing*) que ciega el receptor durante los primeros $1.25\text{ ms}$ ($T_1$). Se requiere un control estricto de temporización para enmascarar dicha zona y evitar falsas detecciones.
   * **Latencia del Recolector de Basura en MicroPython:** Al utilizar MicroPython para cálculo numérico en tiempo real, las pausas no determinísticas del recolector de basura (*garbage collector*) pueden corromper la captura continua del conversor analógico-digital (ADC) si no se desactiva temporalmente durante las ventanas de muestreo.
   * **Fragilidad de Cableado Temporal:** La interconexión mediante cables Dupont sueltos entre la placa analógica base y el módulo ESP32 es sumamente vulnerable al ruido electromagnético y a falsos contactos, exigiendo la construcción de una placa adaptadora física provisional que encaje directamente en el zócalo del microcontrolador original.

3. **Desacoplamiento del Alcance y Propuesta de Solución:** 
   El plan de trabajo de **96 horas** aborda la modernización integral del sistema migrando el control y procesamiento hacia un **ESP32 con MicroPython**:
   * Caracterizar las señales analógicas del circuito original e interconectar el ESP32 protegiendo sus entradas a 3.3V mediante divisores o acoplamientos.
   * Desarrollar en MicroPython los módulos de emisión PWM a 40 kHz, lectura térmica 1-Wire y muestreo continuo por ADC/DMA ($\ge 200\text{ kSPS}$) con desactivación de `gc.disable()`.
   * Implementar en la librería matemática `ulab` los algoritmos de detección de envolvente y cálculo del Tiempo de Vuelo (TOF) por correlación/FFT para resolución milimétrica.
   * Construir una placa adaptadora de prototipado perforada que sustituya los cables Dupont y encaje directamente en el zócalo DIP del dsPIC33 original.
   * Ejecutar la validación metrológica en la maqueta de filtraciones de la RSA a distancias conocidas (250 mm, 400 mm, 600 mm), analizando repetibilidad y desviación estándar ($\sigma$).

---

## 2. Objetivos del Plan de Trabajo

### 2.1. Objetivo General
Migrar el sistema de control, adquisición y procesamiento de señales del sensor ultrasónico de nivel de alta precisión desde el microcontrolador dsPIC33 hacia la plataforma ESP32 utilizando MicroPython y la librería matemática `ulab`, construyendo una placa adaptadora de hardware perforada y validando metrológicamente la precisión milimétrica en la maqueta de filtraciones de la RSA.

### 2.2. Objetivos Específicos
1. **[Auditoría de Hardware y Cableado Inicial - 16 h]:** Caracterizar el circuito analógico original de la maqueta RSA (etapa emisora a 40 kHz, acondicionamiento TL974 y sensor DS18B20), mapear los pines del zócalo dsPIC33 hacia el ESP32, adaptar niveles de voltaje (3.3V/5V) y capturar con osciloscopio la señal analógica del eco.
2. **[Firmware de Emisión PWM y Compensación Térmica - 10 h]:** Desarrollar en MicroPython el generador de ráfagas controladas a 40 kHz mediante `machine.PWM` (5 a 10 ciclos) y el controlador del sensor DS18B20 en bus 1-Wire para calcular en tiempo real la velocidad corregida del sonido $v(T)$.
3. **[Adquisición Continua por ADC y Sincronización - 14 h]:** Configurar el muestreo de alta velocidad del ADC del ESP32 acoplado a I2S/DMA ($\ge 200\text{ kSPS}$), deshabilitando el recolector de basura (`gc.disable()`) y aplicando una máscara temporal para anular la zona ciega del sensor ($T_1 \sim 1.25\text{ ms}$).
4. **[Procesamiento Digital de Señales con ulab - 12 h]:** Desarrollar los algoritmos de filtrado, detección de picos y Tiempo de Vuelo (TOF) mediante operaciones vectoriales y FFT/correlación cruzada con `ulab.numpy`, calculando la distancia exacta en milímetros.
5. **[Construcción del Adaptador en Placa Perforada - 10 h]:** Construir y soldar manualmente una tarjeta adaptadora en placa de prototipado perforada con tiras de pines macho que encajen directamente en el zócalo DIP-28 del dsPIC33 y zócalos hembra para el ESP32, eliminando cables Dupont.
6. **[Validación Metrológica en Maqueta RSA - 14 h]:** Instalar el hardware adaptado en la maqueta física de filtraciones y realizar campañas de medición continua a distancias patrón (250 mm, 400 mm, 600 mm), evaluando error medio, repetibilidad y desviación estándar ($\sigma$).
7. **[Documentación Técnica, Troubleshooting y Cierre - 20 h]:** Documentar el mapa de pines, estructurar la guía de solución de problemas (ruido analógico, no linealidades del ADC, gestión de memoria), modularizar el código en MicroPython y redactar el informe técnico final.

---

## 3. Plan de Trabajo Detallado y Checkpoints Cuantificables

### Fase 1: Auditoría de Hardware, Mapeo de Pines y Cableado Dupont
* **Duración:** 16 horas (Semanas 1 y 2)
* **Objetivo:** Caracterizar el circuito analógico original en la maqueta RSA, verificar compatibilidad de voltajes (3.3V/5V) e interconectar el ESP32 mediante arnés temporal de cables Dupont.
* **Actividades técnicas:**
  1. Identificar en el esquemático y zócalo DIP-28 del dsPIC33 original las líneas críticas:
     - Pin de disparo Tx (señal de 40 kHz hacia el driver de alta tensión).
     - Pin de entrada analógica Rx (señal de eco acondicionada por el TL974).
     - Pin de datos para el bus 1-Wire del sensor DS18B20.
     - Pines de alimentación (+12V, +5V, +3.3V y GND).
  2. Medir con multímetro y osciloscopio los niveles de tensión en cada línea; diseñar e incorporar divisores resistivos o diodos de protección en las entradas para salvaguardar los pines del ESP32 a 3.3V.
  3. Ensamblar y etiquetar un arnés temporal de cables Dupont para conectar la placa base con la tarjeta de desarrollo ESP32.
  4. Energizar la maqueta verificando consumos normales de corriente y estabilidad de reguladores sin calentamiento.
  5. Capturar con osciloscopio digital la señal de eco ultrasónico a la salida del TL974 al reflejarse sobre un blanco rígido.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 1.1:** Esquema de pines del zócalo dsPIC documentado con correspondencia a GPIOs del ESP32.
  - [ ] **CP 1.2:** Niveles de tensión acondicionados a 3.3V verificados con osciloscopio antes de conectar al ESP32.
  - [ ] **CP 1.3:** Arnés Dupont conectado y señal analógica de eco visualizada correctamente en osciloscopio.

---

### Fase 2: Desarrollo del Firmware de Adquisición y Procesamiento en MicroPython
* **Duración:** 36 horas (Semanas 2 y 3)
* **Objetivo:** Programar los módulos en MicroPython para emisión de trenes de pulso, lectura de temperatura, muestreo continuo por ADC/DMA y cálculo de Tiempo de Vuelo con `ulab`.
* **Actividades técnicas:**
  1. Flashear la versión oficial más reciente de MicroPython en el ESP32 y configurar el entorno interactivo Thonny IDE vía puerto REPL.
  2. **Emisión de Ráfaga Tx:** Programar el módulo `machine.PWM` para generar trenes controlados de 5 a 10 ciclos a una frecuencia exacta de 40 kHz con ciclo de trabajo del 50%.
  3. **Compensación Térmica:** Implementar la lectura del sensor DS18B20 utilizando los módulos nativos `onewire` y `ds18x20`, calculando la velocidad del sonido:
     $$v(T) = 331.45 \cdot \sqrt{1 + \frac{T}{273.15}} \quad [\text{m/s}]$$
  4. **Adquisición por ADC continuo:** Configurar el conversor analógico-digital del ESP32 a alta tasa de muestreo ($\ge 200\text{ kSPS}$) acoplado a I2S/DMA para capturar la ventana de tiempo del eco sin bloqueos de CPU.
  5. **Control de recolección de basura:** Invocar `gc.disable()` durante el disparo y ventana de muestreo para evitar latencias impredecibles del runtime de MicroPython, aplicando una máscara de zona ciega ($T_1 \sim 1.25\text{ ms}$).
  6. **Procesamiento de señal con `ulab`:** Cargar la librería matemática `ulab` y desarrollar el algoritmo de cálculo de Tiempo de Vuelo (TOF) mediante operaciones vectoriales, filtrado y detección de envolvente por correlación/FFT (`ulab.numpy.fft`), transformando el tiempo en distancia milimétrica.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 2.1:** Generador PWM emitiendo ráfagas limpias de 40 kHz en el pin de disparo hacia el driver piezoeléctrico.
  - [ ] **CP 2.2:** Módulo DS18B20 reportando temperatura con cálculo automático de $v(T)$ en MicroPython.
  - [ ] **CP 2.3:** Rutina de ADC continuo adquiriendo buffers de muestras sin pausas por recolección de basura.
  - [ ] **CP 2.4:** Script con `ulab` calculando distancias estimadas y desplegándolas en la consola REPL de Thonny.

---

### Fase 3: Pruebas en Maqueta y Construcción de Adaptador en Placa Perforada
* **Duración:** 24 horas (Semanas 4 y 5)
* **Objetivo:** Construir una placa adaptadora de hardware provisional para eliminar cables Dupont sueltos y ejecutar la validación metrológica en la maqueta de filtraciones.
* **Actividades técnicas:**
  1. **Construcción del Adaptador Físico:**
     - Cortar y preparar una placa de prototipado perforada (paso estándar de 2.54 mm).
     - Soldar dos hileras de pines macho espaciadas a 0.3 pulgadas dispuestas para encajar mecánicamente en el zócalo DIP-28 del dsPIC33 original.
     - Soldar conectores hembra para alojar e intercambiar el módulo ESP32.
     - Realizar el cableado punto a punto con alambre estañado de las conexiones de alimentación, masa, disparo PWM, entrada analógica y línea 1-Wire.
     - Prueba exhaustiva de continuidad con multímetro para certificar 0 cortocircuitos antes de energizar.
  2. **Validación Metrológica en Maqueta RSA:**
     - Insertar la placa adaptadora con el ESP32 directamente en el zócalo de la maqueta de filtraciones.
     - Realizar series de 100 mediciones consecutivas a tres distancias fijas calibradas: 250 mm, 400 mm y 600 mm.
     - Calcular en MicroPython indicadores estadísticos de desempeño: media aritmética, desviación estándar ($\sigma$), repetibilidad y error pico respecto al patrón físico.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 3.1:** Placa adaptadora perforada construida, soldada, verificada eléctricamente y encajando en el zócalo DIP.
  - [ ] **CP 3.2:** Sistema operando de forma autónoma en la maqueta física de filtraciones sin cables volantes.
  - [ ] **CP 3.3:** Campaña de medición completada con desviación estándar documentada $\sigma \le 1.5\text{ mm}$.

---

### Fase 4: Documentación Técnica, Troubleshooting e Informe Final
* **Duración:** 20 horas (Semana 6)
* **Objetivo:** Estructurar el código fuente modular en MicroPython, compilar la guía de resolución de problemas y redactar el reporte formal de pasantía.
* **Actividades técnicas:**
  1. **Modularización del software en MicroPython:**
     - `main.py`: Bucle principal de control, temporización y telemetría por UART.
     - `sensor_ultrasonico.py`: Clase de control de disparo PWM y muestreo ADC.
     - `ds18b20.py`: Módulo de compensación térmica y lectura de temperatura.
     - `dsp_utils.py`: Funciones de procesamiento vectorial, detección de envolvente y correlación con `ulab`.
  2. **Guía de Solución de Problemas (Troubleshooting):**
     - Técnicas de mitigación de ruido analógico en el ADC del ESP32 (uso de promediado y filtrado digital).
     - Compensación de la no linealidad en los extremos de escala del ADC interno de Espressif.
     - Estrategias de gestión de memoria RAM en MicroPython ante cálculos vectoriales con `ulab`.
     - Diagrama de conexionado esquemático del adaptador en placa perforada.
  3. Redacción del Informe Técnico Final en formato institucional de la RSA y la Universidad de Cuenca, compilando fotografías del montaje, oscilogramas y tablas de resultados metrológicos.
  4. Revisión técnica con el tutor institucional.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 4.1:** Repositorio local de código en MicroPython estructurado, comentado y limpio.
  - [ ] **CP 4.2:** Memoria técnica con diagrama de conexionado del adaptador y guía de troubleshooting.
  - [ ] **CP 4.3:** Informe Técnico Final formal de prácticas preprofesionales listo para aprobación del tutor.

---

## 4. Resumen de Fases y Cronograma Semanal (96 Horas)

El trabajo se organiza en un lapso de **6 semanas** con una dedicación promedio de **16 horas semanales**:

| Fase | Semanas | Horas | Enfoque Principal |
| :---: | :---: | :---: | :--- |
| **Fase 1** | Semanas 1–2 | **16 h** | Mapeo de pines zócalo dsPIC a ESP32, acondicionamiento de voltajes y captura de eco con osciloscopio. |
| **Fase 2** | Semanas 2–3 | **36 h** | Firmware en MicroPython: disparo PWM 40 kHz, lectura DS18B20, ADC/DMA continuo y DSP con `ulab`. |
| **Fase 3** | Semanas 4–5 | **24 h** | Fabricación de placa adaptadora perforada compatible con zócalo DIP y ensayos metrológicos en maqueta. |
| **Fase 4** | Semana 6 | **20 h** | Modularización de código (`.py`), guía de *troubleshooting*, análisis estadístico e Informe Final. |
| **Total** | **6 Semanas** | **96 h** | **Planificación Global de Prácticas Preprofesionales** |

---

## 5. Entregables y Criterios de Éxito de la Pasantía

1. **Hardware Adaptador Prototipado y Funcional:**
   * Placa adaptadora provisional construida sobre circuito perforado con tiras de pines para encaje directo en el zócalo DIP-28 del dsPIC33 original y conectores hembra para el ESP32.
   * Arnés de cableado Dupont de respaldo debidamente rotulado para pruebas secundarias de banco.
2. **Firmware Modular en MicroPython:**
   * Paquete de scripts en MicroPython (`main.py`, `sensor_ultrasonico.py`, `ds18b20.py`, `dsp_utils.py`) con algoritmos de correlación/TOF mediante `ulab`, compensación térmica $v(T)$ y control de `gc.disable()`.
3. **Validación Metrológica y Oscilogramas:**
   * Conjunto de capturas de osciloscopio certificando la ráfaga de excitación a 40 kHz y la envolvente del eco acústico.
   * Reporte metrológico con análisis estadístico (desviación estándar $\sigma$, error medio) en la maqueta de filtraciones de la RSA demostrando resolución milimétrica.
4. **Documentación Técnica y Guía de Troubleshooting:**
   * Diagrama de conexionado esquemático del adaptador de hardware.
   * Guía de resolución de problemas cubriendo mitigación de ruido en el ADC y gestión de memoria en MicroPython.
   * Informe Técnico Final de prácticas preprofesionales en formato institucional.
