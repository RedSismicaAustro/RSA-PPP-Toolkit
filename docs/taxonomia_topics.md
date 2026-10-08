# Taxonomía Oficial de Topics (Etiquetas GitHub) para Proyectos RSA-PPP

Este documento define el vocabulario controlado de **topics (etiquetas)** para todos los repositorios bajo la organización [`RSA-PPP`](https://github.com/RSA-PPP).

El objetivo es permitir la indexación, búsqueda y agrupación temática precisa tanto en GitHub como en el catálogo institucional.

---

## 📐 Reglas Generales de GitHub para Topics

1. **Formato:** Únicamente letras minúsculas, números y guiones medios (`-`).
2. **Sin espacios ni caracteres especiales:** No usar tildes, puntos, barras ni guiones bajos (`_`).
3. **Longitud:** Máximo 35 caracteres por etiqueta.
4. **Cantidad por repositorio:** Entre **6 y 10 topics** (GitHub admite hasta 20, pero se recomienda concisión).

---

## 🏛️ Estructura Canónica de Topics (Categorías Obligatorias)

Cada repositorio en `RSA-PPP` debe seleccionar sus topics siguiendo esta estructura obligatoria:

```text
[Institucionales] + [Dominio/Aplicación] + [Hardware/Arquitectura] + [Comunicaciones/Protocolo] + [Lenguajes/Software]
     (2 topics)             (1-2 topics)             (1-2 topics)                (1-2 topics)              (1-2 topics)
```

---

## 📚 Catálogo Controlado de Vocabulario

### 1. Institucionales y Organizacionales (Obligatorios en todos)
* `rsa-ppp`: Identificador de proyecto de Prácticas Preprofesionales de la RSA.
* `ucuenca`: Afiliación institucional con la Universidad de Cuenca.
* `red-sismica-austro`: Afiliación con la Red Sísmica del Austro.

### 2. Dominio y Aplicación de Ingeniería
* `shm`: Structural Health Monitoring (Monitorización de Salud Estructural).
* `seismic-monitoring`: Monitorización e instrumentación sísmica / acelerógrafos.
* `dam-monitoring`: Auscultación e instrumentación de presas y vertederos.
* `iot`: Internet de las cosas y telemetría distribuida.
* `daq`: Sistemas de Adquisición de Datos (Data Acquisition).
* `metrology`: Caracterización metrológica y calibración de sensores.
* `pcba-standardization`: Estandarización de manufactura y ensamblaje PCBA.

### 3. Hardware, Microcontroladores y Sensores
* **Microcontroladores / SBCs:**
  * `dspic33`: Microcontroladores Microchip dsPIC33 (16 bits / DSP).
  * `esp32`: Plataforma Espressif ESP32.
  * `pic-microcontroller`: Microcontroladores PIC de 8 bits (ej. PIC16F628A).
  * `raspberry-pi`: Computadoras de placa reducida (SBC).
  * `ni-usb-6210`: Tarjetas DAQ de National Instruments.
* **Sensores y Transductores:**
  * `adxl355`: Acelerómetros triaxiales de alta precisión / bajo ruido.
  * `ultrasonic-sensor`: Sensores de nivel acústicos / ultrasónicos.
  * `strain-gauges`: Galgas extensométricas y puentes de Wheatstone.
  * `vibrating-wire`: Sensores de cuerda vibrante para presas.
* **Almacenamiento y Periféricos:**
  * `microsd`: Almacenamiento masivo en memorias flash SD / SDHC.
  * `card-detect`: Detección física / mecánica de tarjetas.

### 4. Protocolos, Comunicaciones y Arquitectura
* `rs485`: Comunicación diferencial semidúplex / fulldúplex.
* `daisy-chain`: Topología física de red en cascada.
* `spi`: Bus Serial Peripheral Interface (acelerómetro, tarjetas SD).
* `i2c`: Bus Inter-Integrated Circuit.
* `uart`: Comunicación serie asíncrona.
* `mqtt`: Protocolo publish-subscribe para telemetría.
* `time-synchronization`: Sincronización temporal distribuida o pulsos hardware.
* `ping-pong-buffer`: Arquitectura de doble búfer en memoria RAM.

### 5. Lenguajes de Programación, Entornos y Análisis
* **Lenguajes:**
  * `c`: Firmware en lenguaje C.
  * `python`: Análisis de datos, drivers de PC o scripts DAQ.
  * `micropython`: Entorno embebido MicroPython.
* **Entornos y EDA:**
  * `mikroc`: Compilador MikroC PRO.
  * `mplab-x`: Entorno MPLAB X / IPE.
  * `kicad`: Diseño de circuitos impresos en KiCad.
  * `eagle`: Diseños en Autodesk EAGLE.
* **Observabilidad y Bases de Datos:**
  * `grafana`: Paneles de visualización en tiempo real.
  * `influxdb`: Base de datos para series temporales.
  * `docker`: Contenerización de servicios.

---

## 💡 Ejemplos de Asignación por Proyecto Real

### Caso 1: `ppp-2026-11-shm-nodos-sensores` (Geovanny Cullquicondo)
```yaml
topics:
  - rsa-ppp
  - ucuenca
  - shm
  - dspic33
  - adxl355
  - microsd
  - ping-pong-buffer
  - rs485
  - daisy-chain
  - python
```

### Caso 2: `ppp-2026-09-shm-ensamblaje-validacion` (David Timbi)
```yaml
topics:
  - rsa-ppp
  - ucuenca
  - shm
  - dspic33
  - rs485
  - daisy-chain
  - time-synchronization
  - microsd
```

### Caso 3: `ppp-2026-10-sensor-ultrasonico` (Franklin Andrade)
```yaml
topics:
  - rsa-ppp
  - ucuenca
  - dam-monitoring
  - ultrasonic-sensor
  - esp32
  - micropython
  - dsp
```

### Caso 4: `ppp-2025-06-dashboard-monitoreo` (Mauro Bravo)
```yaml
topics:
  - rsa-ppp
  - ucuenca
  - seismic-monitoring
  - iot
  - mqtt
  - grafana
  - influxdb
  - docker
```
