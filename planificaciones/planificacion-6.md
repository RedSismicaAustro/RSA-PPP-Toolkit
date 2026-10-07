---
titulo: "Plan de Trabajo de Prácticas Preprofesionales: Dashboard de Monitoreo en Tiempo Real y Telemetría IoT para Estaciones Acelerográficas RSA (Stack TIG-MQTT)"
proyecto: "Dashboard de Monitoreo en Tiempo Real de Estaciones RSA (Stack Telegraf, InfluxDB, Grafana y MQTT)"
codigo_proyecto: "RSA-PPP-2025-06"
area_tematica: "Telemetría IoT, Observabilidad, Microservicios Docker, Bases de Datos de Series de Tiempo y Alertas Móviles"
estado: "Culminado"
version: "1.0"
fecha_creacion: "2025-10-17"
fecha_actualizacion: "2026-01-15"

pasante:
  nombre: "Mauro Martín Bravo Pintado"
  cedula: "N/D"
  correo: "martin.bravo@ucuenca.edu.ec"
  carrera: "Ingeniería en Telecomunicaciones / Computación"
  institucion: "Universidad de Cuenca"

tutoria:
  tutor_institucional: "Ing. Milton Muñoz. (Red Sísmica del Austro - RSA)"
  correo_tutor_institucional: "milton.munozc@ucuenca.edu.ec"

cronograma:
  duracion_total_horas: 144
  dedicacion_semanal_horas: 11
  duracion_semanas: 13
  fecha_inicio: "2025-10-17"
  fecha_fin_estimada: "2026-01-15"
  modalidad: "Presencial / Teletrabajo"

tecnologias:
  - "Protocolos y Mensajería IoT: MQTT (Mosquitto Broker, Paho-MQTT en Python, QoS 1, Retain flag, Last Will and Testament LWT)"
  - "Virtualización y Orquestación: Docker y Docker Compose (Entornos reproducibles con volúmenes persistentes y redes internas)"
  - "Ingesta y Transformación de Métricas: Telegraf (Plugin mqtt_consumer, parseo JSON a Influx Line Protocol)"
  - "Base de Datos de Series de Tiempo (TSDB): InfluxDB v2 (Buckets de almacenamiento, políticas de retención a 90 días, lenguaje Flux)"
  - "Visualización y Monitorización: Grafana (Dashboards de cuadrícula general de red y vista individual por estación)"
  - "Canales de Notificación y Alertas: Grafana Unified Alerting y Bot de Telegram integrado en dispositivos móviles"
  - "Entorno de Desarrollo y Simulación: WSL (Ubuntu 24.04 LTS en Windows), Visual Studio Code, Python 3, MQTT Explorer"

repositorio:
  url: "https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT"
  rama_base: "main"
---

---

## 1. Antecedentes y Justificación Técnica

1. **Contexto del Sistema y Estado Inicial del Artefacto:** 
   La Red Sísmica del Austro (RSA) dispone de una red en expansión de acelerógrafos y estaciones de auscultación estructural distribuidas geográficamente en presas (como Chanlud y Labrado), puentes y edificaciones críticas. Estas estaciones cuentan con microcontroladores o SBCs tipo Raspberry Pi que registran continuamente señales de vibración y disponen de clientes telemáticos para transmitir eventos sísmicos. No obstante, no se contaba con un sistema unificado y centralizado de observabilidad en tiempo real para supervisar la salud operativa de los equipos remotos (temperatura de procesador, espacio libre en tarjetas SD/disco, tiempo de actividad ininterrumpida y última señal de vida o *heartbeat*), obligando a realizar revisiones manuales aisladas por SSH o inspecciones presenciales en campo.

2. **Limitaciones Críticas o Cuellos de Botella Identificados:** 
   * **Falta de Detección Inmediata de Caídas:** Cuando un nodo remoto sufría un corte de energía, falla de conectividad celular/satelital o congelamiento del sistema operativo, el equipo central de la RSA tardaba días en identificar la anomalía, perdiendo valiosos registros ante sismos repentinos.
   * **Inexistencia de una Arquitectura Estandarizada de Tópicos:** Las estaciones enviaban datos sin una nomenclatura jerárquica clara de tópicos MQTT, mezclando metadatos de diagnóstico con señales de aceleración bruta y careciendo de mecanismos estandarizados de *Last Will and Testament* (LWT).
   * **Ausencia de Persistencia de Series Temporales:** No se disponía de un motor de bases de datos especializado en series de tiempo para registrar el historial de desempeño de las estaciones con políticas automáticas de depuración y retención de memoria.
   * **Carencia de Notificaciones Proactivas:** No existía una capa de alertas automáticas que notificara de forma push al personal técnico a través de canales móviles cuando una estación superaba umbrales térmicos peligrosos ($>60\,^\circ\text{C}$) o registraba agotamiento inminente de disco ($<1\text{ GB}$).

3. **Desacoplamiento del Alcance y Propuesta de Solución:** 
   El plan de trabajo de 144 horas se enfoca en el diseño, despliegue y validación de una plataforma completa de observabilidad basada en el **Stack TIG-MQTT (Telegraf, InfluxDB, Grafana y MQTT)** orquestado en contenedores Docker:
   * Diseñar una arquitectura jerárquica y segura de tópicos MQTT (`rsa/seismic/smart/<station>/telemetry/...`) y un agente de telemetría modular en Python (`cliente_mqtt.py`).
   * Configurar un colector de métricas desacoplado con Telegraf (`inputs.mqtt_consumer`) que parsee las tramas JSON e ingeste de forma determinística en InfluxDB v2 con política de retención de 90 días.
   * Implementar dos dashboards interactivos en Grafana: un panel general tipo cuadrícula (*grid*) con semáforos de estado para la red completa y un panel analítico detallado por estación.
   * Configurar un sistema de alertas proactivas vinculadas a un **Bot de Telegram** para notificar caídas de nodos (LWT), silencios de datos, temperaturas elevadas y espacio crítico en disco.
   * Desarrollar un simulador concurrente de $N$ estaciones (`simulacion_estaciones.py`) para ejecutar pruebas de estrés, escalabilidad y tolerancia a fallos.

---

## 2. Objetivos del Plan de Trabajo

### 2.1. Objetivo General
Diseñar, desplegar y validar una plataforma integral de telemetría y monitoreo en tiempo real basada en el Stack TIG-MQTT (Telegraf, InfluxDB, Grafana y Mosquitto) contenerizada en Docker, implementando un agente de métricas en Python con LWT, dashboards de visualización de red y un sistema de alertas proactivas integrado con Telegram para la red de estaciones acelerográficas de la RSA.

### 2.2. Objetivos Específicos
1. **[Fase 1 - Entorno y Definiciones - 10 h]:** Configurar el entorno de desarrollo en WSL (Ubuntu 24.04), definir la jerarquía estandarizada de tópicos MQTT, esquemas JSON de carga útil y variables de entorno, validando la comunicación con el broker de la RSA mediante MQTT Explorer.
2. **[Fase 2 - Agente de Telemetría - 24 h]:** Desarrollar en Python (`paho-mqtt`) el agente de telemetría modular (`cliente_mqtt.py`) para publicación periódica de estado (`state`), salud de hardware (`health`: temperatura, disco, uptime) y eventos sísmicos simulados, incorporando QoS 1, flag retain y configuración de LWT (*offline*).
3. **[Fase 3 - Ingesta Telegraf e InfluxDB - 30 h]:** Desplegar mediante Docker Compose los servicios de InfluxDB v2 y Telegraf, configurando el plugin `mqtt_consumer` para parseo de JSON, mapeo a mediciones/tags/fields y establecimiento de la política de retención de 90 días.
4. **[Fase 4 - Dashboards en Grafana - 30 h]:** Desplegar Grafana en Docker e implementar dos tableros de control interactivos: Vista General de Red (cuadrícula de estado, métricas y uptime global) y Vista Detalle por Estación (series temporales, gauges e historial de eventos) con variables de filtrado dinámico.
5. **[Fase 5 - Alertas y Bot de Telegram - 18 h]:** Configurar las reglas de alerta en Grafana Alerting para caídas de estación (LWT/silencio $>60\text{ s}$), temperatura crítica ($>60\,^\circ\text{C}$) y espacio bajo en disco ($<1\text{ GB}$), integrando un Bot de Telegram para entrega inmediata de notificaciones en móviles.
6. **[Fase 6 - Pruebas de Escalabilidad y Simulación - 20 h]:** Desarrollar el simulador de $N$ estaciones (`simulacion_estaciones.py`) para inyectar fallos concurrentes (caídas, silencios, ráfagas sísmicas) y validar la resiliencia, latencia y consumo de recursos (CPU/RAM) del servidor.
7. **[Fase 7 - Documentación y Entrega - 12 h]:** Elaborar los manuales técnicos de despliegue paso a paso (`docker-compose.yml`, `.env.example`), manual de operación para alta de estaciones y el Informe Final de Prácticas Preprofesionales.

---

## 3. Plan de Trabajo Detallado y Checkpoints Cuantificables

### Fase 1: Preparación del Entorno, Definición de Tópicos y Acceso MQTT
* **Duración:** 10 horas (Semanas 1 y 2 / 14–22 de octubre de 2025)
* **Objetivo:** Establecer el entorno operativo en WSL, homologar la jerarquía de tópicos y validar el acceso al broker MQTT institucional.
* **Actividades técnicas:**
  1. Configurar el entorno de trabajo en Windows Subsystem for Linux (WSL con Ubuntu 24.04) integrado con Visual Studio Code y control de versiones Git.
  2. Inspeccionar el tráfico existente y validar conectividad hacia el broker institucional de la RSA mediante la herramienta MQTT Explorer.
  3. Definir la taxonomía jerárquica estándar de tópicos MQTT:
     - `rsa/seismic/smart/<station>/telemetry/state` (Mensaje de estado con Retain y LWT).
     - `rsa/seismic/smart/<station>/telemetry/health` (Métricas periódicas de hardware).
     - `rsa/seismic/smart/<station>/telemetry/heartbeat` (Marca de último evento activo).
     - `rsa/seismic/smart/<station>/events/detected` (Eventos sísmicos discretos con amplitud y confianza).
     - `rsa/seismic/smart/<station>/events/data` (Datos acelerométricos crudos).
  4. Especificar los esquemas de carga útil JSON y umbrales por defecto: temperatura de CPU ($60\,^\circ\text{C}$), espacio en disco ($1\text{ GB}$), intervalo de silencio ($60\text{ s}$).
  5. Crear la plantilla de variables de entorno `.env.example` para resguardo seguro de credenciales.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 1.1:** Entorno WSL y repositorio Git [RSA-Intern-TIG-MQTT](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT) configurados.
  - [ ] **CP 1.2:** Especificación formal de tópicos y esquemas JSON documentada en el repositorio.
  - [ ] **CP 1.3:** Conexión bidireccional exitosa verificada con el broker de la RSA mediante MQTT Explorer.

---

### Fase 2: Implementación del Agente de Telemetría en Python
* **Duración:** 24 horas (Semanas 2 a 4 / 23 de octubre – 7 de noviembre de 2025)
* **Objetivo:** Desarrollar el agente modular en Python (`cliente_mqtt.py`) para adquisición/simulación de métricas, gestión de LWT y publicación estructurada.
* **Actividades técnicas:**
  1. Desarrollar la arquitectura modular del agente utilizando `paho-mqtt` en Python 3.
  2. Implementar funciones de captura de métricas del sistema:
     - `temp_cpu`: Lectura de temperatura del procesador con variaciones graduales simuladas ($40\text{ a }85\,^\circ\text{C}$).
     - `disk_free_gb`: Monitoreo del espacio disponible en disco con decremento progresivo.
     - `uptime_s`: Cálculo de segundos transcurridos desde el arranque del sistema (`/proc/uptime`).
     - `last_event_ts`: Generación de estampas de tiempo en formato ISO 8601 UTC.
  3. Configurar el mensaje **Last Will and Testament (LWT)**: publicar automáticamente `{"id": "<station>", "status": "offline"}` en el tópico `state` ante desconexión inesperada del socket TCP.
  4. Implementar publicación con QoS 1 y bandera *retain* activa para tópicos de estado y heartbeat.
  5. Diseñar el generador probabilístico de eventos sísmicos simulados (`simular_evento_sismico()`) con identificador de evento, amplitud y nivel de confianza.
  6. Centralizar configuraciones en `configuracion_mqtt.json` y `configuracion_dispositivo.json`.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 2.1:** Script `cliente_mqtt.py` publicando periódicamente métricas de salud y estado en el broker.
  - [ ] **CP 2.2:** Mecanismo LWT validado: conmutación inmediata a `offline` al cortar el proceso del agente.
  - [ ] **CP 2.3:** Generador de eventos sísmicos emitiendo tramas JSON válidas en el tópico de eventos.

---

### Fase 3: Ingesta de Telemetría con Telegraf e InfluxDB v2
* **Duración:** 30 horas (Semanas 4 a 6 / 7–18 de noviembre de 2025)
* **Objetivo:** Desplegar Telegraf e InfluxDB en contenedores Docker y canalizar el flujo de ingesta continua con retención a 90 días.
* **Actividades técnicas:**
  1. Diseñar el archivo `docker-compose.yml` para levantar los contenedores de InfluxDB v2 y Telegraf con volúmenes persistentes (`influxdb_data`, `telegraf_config`) y red interna bridge.
  2. Inicializar InfluxDB v2: crear la organización `rsa`, el bucket principal `telemetry` y generar tokens de autenticación de operador.
  3. Configurar la política de retención de datos (*Retention Policy*): fijar ciclo de depuración automática en 90 días (`90d`).
  4. Configurar el archivo `telegraf.conf`:
     - Habilitar plugin de entrada `inputs.mqtt_consumer` suscrito a `rsa/seismic/smart/+/telemetry/#` y `events/#`.
     - Definir parser de formato de datos JSON con extracción de tags (`station`, `org`, `app`) y campos numéricos (`temp_cpu`, `disk_free_gb`, `uptime_s`).
     - Habilitar plugin de salida `outputs.influxdb_v2` con URL interna de Docker y token seguro.
  5. Ejecutar pruebas de estrés de persistencia verificando la continuidad de ingesta tras reinicios del daemon Docker.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 3.1:** Contenedores de InfluxDB y Telegraf operando de forma estable y persistente en Docker.
  - [ ] **CP 3.2:** Archivo `telegraf.conf` ingiriendo y convirtiendo los payloads MQTT a Line Protocol sin pérdidas.
  - [ ] **CP 3.3:** Bucket `telemetry` en InfluxDB almacenando métricas con retención confirmada a 90 días y consultas en Data Explorer.

---

### Fase 4: Diseño e Implementación de Dashboards en Grafana
* **Duración:** 30 horas (Semanas 6 a 8 / 19–26 de noviembre de 2025)
* **Objetivo:** Implementar tableros de visualización interactivos en Grafana para supervisión en tiempo real del estado de red y análisis individual de estaciones.
* **Actividades técnicas:**
  1. Integrar el servicio Grafana al despliegue de Docker Compose, exponiendo el puerto 3000 con persistencia en `grafana_data`.
  2. Configurar InfluxDB como fuente de datos (*DataSource*) en Grafana mediante consultas Flux.
  3. Diseñar el **Dashboard de Resumen General de Red (Vista Red RSA):**
     - Panel Grid con tarjetas de estado de cada estación: indicador visual verde/rojo (`Online`/`Offline`), temperatura actual, espacio libre en disco y tiempo transcurrido desde el último evento.
     - Indicador global de estaciones activas vs inactivas.
  4. Diseñar el **Dashboard de Detalle por Estación (Vista Estación):**
     - Selector desplegable de estación mediante variables de plantilla (`$station`).
     - Gráfica de serie temporal de temperatura del procesador con líneas de umbral.
     - Indicador de estado de almacenamiento (espacio libre en GB y porcentaje de uso).
     - Timeline de eventos sísmicos detectados y registro de reinicios del sistema.
  5. Exportar la estructura de los dashboards a archivos JSON para garantizar su reproducibilidad.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 4.1:** DataSource InfluxDB validado y comunicándose en alta velocidad con Grafana.
  - [ ] **CP 4.2:** Dashboard de Resumen General de Red operativo mostrando el estado de múltiples estaciones en tiempo real.
  - [ ] **CP 4.3:** Dashboard de Detalle por Estación completado y archivos JSON exportados al repositorio.

---

### Fase 5: Sistema de Alertas Proactivas en Grafana e Integración con Telegram
* **Duración:** 18 horas (Semanas 8 y 9 / 27 de noviembre – 5 de diciembre de 2025)
* **Objetivo:** Automatizar la detección de incidentes en la red y despachar alertas en tiempo real a dispositivos móviles vía Telegram.
* **Actividades técnicas:**
  1. Configurar el motor unificado de alertas de Grafana (*Grafana Alerting*):
     - **Regla 1 (Caída de Estación):** Disparo inmediato si el estado reporta `offline` (por LWT) o si no se reciben heartbeats en $>60\text{ s}$.
     - **Regla 2 (Temperatura Elevada):** Disparo si `temp_cpu` supera el umbral de advertencia ($>60\,^\circ\text{C}$).
     - **Regla 3 (Agotamiento de Disco):** Disparo si `disk_free_gb` cae por debajo de $1\text{ GB}$.
  2. Crear y configurar un bot institucional en Telegram ("Alarmas Grafana bot") mediante BotFather y obtener el `Chat ID` del grupo técnico de la RSA.
  3. Configurar el canal de contacto (*Contact Point*) en Grafana vinculado a la API del Bot de Telegram.
  4. Diseñar plantillas de mensajes claros con formato Markdown indicando: Estación afectada, Métrica violada, Valor medido, Severidad y Fecha/Hora UTC.
  5. Ejecutar ensayos de inyección de valores anómalos simulando caídas y sobrecalentamiento para verificar la recepción instantánea de alertas (`Firing`) y resolución (`Resolved`).
* **Checkpoints medibles para revisión:**
  - [ ] **CP 5.1:** Reglas de alerta configuradas en Grafana para caída de nodo, temperatura y espacio en disco.
  - [ ] **CP 5.2:** Bot de Telegram enlazado exitosamente como canal de notificación activo.
  - [ ] **CP 5.3:** Recepción móvil verificada en Telegram de alertas de disparo y restauración con capturas de evidencia.

---

### Fase 6: Pruebas de Escalabilidad, Simulación Masiva y Hardening
* **Duración:** 20 horas (Semanas 9 a 12 / 8 de diciembre de 2025 – 10 de enero de 2026)
* **Objetivo:** Evaluar la resiliencia y el comportamiento del servidor central ante cargas concurrentes masivas de estaciones e inyección de fallos.
* **Actividades técnicas:**
  1. Desarrollar el script de simulación masiva `simulacion_estaciones.py` capaz de instanciar concurrentemente de 10 a 50+ estaciones virtuales con identificadores únicos (`NOM00`, `NOM01`, ..., `NOM09`).
  2. Configurar perfiles controlados de falla distribuida: asignar a estaciones específicas incrementos de temperatura anómalos, consumos acelerados de disco o cortes de silencio.
  3. Ejecutar pruebas de carga durante intervalos sostenidos evaluando la estabilidad del broker Mosquitto, Telegraf e InfluxDB.
  4. Monitorear el consumo de recursos de la máquina servidora: uso de CPU, ocupación de RAM y latencia en la visualización de Grafana.
  5. Implementar hardening: fijar límites de recursos en `docker-compose.yml` (`mem_limit`, `cpus`), políticas de reinicio automático (`restart: unless-stopped`) y respaldos de configuración.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 6.1:** Script `simulacion_estaciones.py` ejecutándose con 10 estaciones simultáneas inyectando fallas programadas.
  - [ ] **CP 6.2:** Dashboard de Grafana reflejando con exactitud los estados anómalos de las estaciones simuladas en simultáneo.
  - [ ] **CP 6.3:** Reporte de desempeño documentando consumo óptimo de memoria y estabilidad de servicios Docker.

---

### Fase 7: Documentación Técnica, Guías de Operación e Informe Final
* **Duración:** 12 horas (Semanas 12 y 13 / 11–15 de enero de 2026)
* **Objetivo:** Consolidar el repositorio institucional, elaborar las guías de administración del sistema y redactar el informe técnico final.
* **Actividades técnicas:**
  1. Elaborar el Manual de Despliegue Técnico paso a paso en formato Markdown (`README.md`): instrucciones de instalación en WSL/Linux, variables de entorno y comandos Docker Compose.
  2. Redactar el Manual de Operación y Mantenimiento: procedimientos para dar de alta nuevas estaciones, reconfigurar umbrales de alerta y recuperar servicios caídos.
  3. Redactar el Informe Técnico Final formal de prácticas preprofesionales (`Informe_MartinBravo.pdf`) en formato LaTeX/PDF, compilando justificación técnica, diagramas de arquitectura, capturas de dashboards y conclusiones.
  4. Revisión técnica con el tutor institucional y suscripción de actas de acreditación de 144 horas.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 7.1:** Repositorio institucional [RSA-Intern-TIG-MQTT](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT) con código limpio, scripts, dashboards JSON y documentación técnica.
  - [ ] **CP 7.2:** Documento formal institucional (`Informe_MartinBravo.pdf`, 22 páginas) aprobado por el tutor institucional de la RSA.

---

## 4. Resumen de Fases y Cronograma Semanal (144 Horas)

El trabajo se ejecutó a lo largo de **13 semanas** (del 17 de octubre de 2025 al 15 de enero de 2026) con una dedicación promedio de **11 horas semanales**:

| Fase | Semanas | Horas | Enfoque Principal |
| :---: | :---: | :---: | :--- |
| **Fase 1** | Semanas 1–2 | **10 h** | Entorno WSL, taxonomía de tópicos MQTT y verificación de conectividad con broker RSA. |
| **Fase 2** | Semanas 2–4 | **24 h** | Agente de telemetría en Python (`cliente_mqtt.py`), métricas de CPU/disco/uptime, retain y LWT. |
| **Fase 3** | Semanas 4–6 | **30 h** | Despliegue Docker de Telegraf e InfluxDB v2, plugin `mqtt_consumer` y política de retención 90d. |
| **Fase 4** | Semanas 6–8 | **30 h** | Despliegue de Grafana, DataSource InfluxDB y diseño de Dashboards (Resumen de Red y Detalle por Estación). |
| **Fase 5** | Semanas 8–9 | **18 h** | Reglas de alerta en Grafana (caída, temperatura, disco) e integración con Bot de Telegram móvil. |
| **Fase 6** | Semanas 9–12 | **20 h** | Simulador masivo de estaciones (`simulacion_estaciones.py`), pruebas de estrés y hardening Docker. |
| **Fase 7** | Semanas 12–13 | **12 h** | Manuales de despliegue/operación, consolidación de repositorio e Informe Técnico Final. |
| **Total** | **13 Semanas** | **144 h** | **Planificación Global de Prácticas Preprofesionales** |

---

## 5. Entregables y Criterios de Éxito de la Pasantía

1. **Agente de Telemetría y Simulador Concurrente en Python:**
   * Script `cliente_mqtt.py` para publicación estructurada de estado operativo, salud de hardware y eventos sísmicos simulados.
   * Configuración de *Last Will and Testament* (LWT) para notificación automática e instantánea del estado `offline`.
   * Script `simulacion_estaciones.py` para generación masiva de tráfico y perfiles de falla concurrentes ($N \ge 10$ estaciones).
2. **Infraestructura de Ingesta Contenerizada (Docker Compose):**
   * Archivo `docker-compose.yml` orquestando Telegraf, InfluxDB v2 y Grafana con volúmenes persistentes y límites de recursos.
   * Archivo de configuración `telegraf.conf` con plugin `mqtt_consumer` optimizado para parsear tramas JSON hacia Influx Line Protocol.
   * Bucket de almacenamiento en InfluxDB configurado con política de retención estricta de 90 días.
3. **Dashboards Interactivos en Grafana:**
   * **Dashboard Resumen General de Red:** Cuadrícula interactiva con indicadores de estado en línea/fuera de servicio, temperatura, espacio en disco y última marca de evento de toda la red sísmica.
   * **Dashboard Detalle por Estación:** Gráficas de series temporales de temperatura, gauges de almacenamiento, métricas de uptime y líneas de tiempo de eventos por nodo.
   * Archivos de exportación JSON de los dashboards versionados en el repositorio.
4. **Sistema de Alertas Móviles con Bot de Telegram:**
   * Reglas de monitoreo continuo en Grafana Alerting para caídas de enlace, silencios de datos, sobretemperatura ($>60\,^\circ\text{C}$) y espacio bajo en disco ($<1\text{ GB}$).
   * Bot institucional de Telegram integrado entregando notificaciones push en tiempo real a los ingenieros de la RSA.
5. **Repositorio Institucional y Documentación Formal:**
   * Código fuente completo, archivos de configuración y guías paso a paso alojados en: [RSA-Intern-TIG-MQTT](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT).
   * Documento formal institucional (`Informe_MartinBravo.pdf`, 22 páginas) compilando la arquitectura, resultados experimentales y manual de operaciones.
