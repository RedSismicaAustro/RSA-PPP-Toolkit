---
titulo: "Plan de Trabajo de Prácticas Preprofesionales: Sistema de Adquisición, Monitoreo IoT y Procesamiento de Sensores Estructurales (Presa Chanlud)"
proyecto: "Monitoreo y Telemetría de Instrumentación Estructural - Presa Chanlud"
codigo_proyecto: "RSA-PPP-2023-01"
area_tematica: "Sistemas Embebidos, Visión por Computador, Redes IoT y Telemetría"
estado: "Culminado"
version: "1.0"
fecha_creacion: "2023-11-22"
fecha_actualizacion: "2024-04-12"

pasante:
  nombre: "Henry Castro"
  cedula: "N/D"
  correo: "henry.castro@ucuenca.edu.ec"
  carrera: "Ingeniería en Electrónica y Telecomunicaciones"
  institucion: "Universidad de Cuenca"

tutoria:
  tutor_institucional: "Ing. Milton Muñoz. (Red Sísmica del Austro - RSA)"
  correo_tutor_institucional: "milton.munozc@ucuenca.edu.ec"

cronograma:
  duracion_total_horas: 144
  dedicacion_semanal_horas: 14
  duracion_semanas: 10.5
  fecha_inicio: "2023-11-22"
  fecha_fin_estimada: "2024-04-10"
  modalidad: "Presencial / Remota"

tecnologias:
  - "Hardware y SBCs: Raspberry Pi 3/4 (Host 10.22.147.186), Estación Concentradora, Nodos Sensores"
  - "Lenguajes de Programación: Python 3, Bash Shell Scripting, SQL"
  - "Visión Artificial y Procesamiento de Señales: OpenCV (Filtro Gaussiano, Ecualización de Histograma, Algoritmo Canny, ROI dinámico)"
  - "Redes y Protocolos IoT: Red Ad-Hoc 802.11 (IBSS wlan0, enrutamiento estático multihop), MQTT (Paho-MQTT, JSON), SSH, RealVNC"
  - "Almacenamiento y Cloud: Google Drive API, compresión TAR.GZ, CSV estructurado, Contenedores Docker / SQL"
  - "Diseño Electrónico: Altium Designer (Librerías de esquemáticos, PCB Layout, DRC, Generación de Gerbers/Drill)"

repositorio:
  url: "https://github.com/RedSismicaAustro/pasantias-chanlud-coordinometros"
  rama_base: "main"
---

---

## 1. Antecedentes y Justificación Técnica

1. **Contexto del Sistema y Estado Inicial del Artefacto:** 
   La Red Sísmica del Austro (RSA) gestiona la auscultación y monitoreo de la salud estructural de la Presa Chanlud a través de instrumental especializado que incluye coordinómetros ópticos (medición de desplazamientos del péndulo inverso y directo), vertederos de aforo y una red de acelerógrafos para registro sísmico continuo. Al inicio de la pasantía, los coordinómetros operaban con un software preliminar en Raspberry Pi (`imageDetected2.py`) que capturaba imágenes del hilo del péndulo pero presentaba vulnerabilidad al ruido visual y cambios lumínicos dentro del túnel de la presa. En cuanto a la red de acelerógrafos, la difusión de eventos sísmicos mediante MQTT no contaba con una lógica refinada de discriminación temporal ni un formato estandarizado para los disparos de estación. Además, existían prototipos de circuitos impresos (PCB) que requerían corrección de diseño y compatibilidad de librerías en Altium Designer, así como la necesidad imperiosa de interconectar los nodos de la presa en un esquema de red inalámbrica Ad-Hoc sin depender de puntos de acceso comerciales.

2. **Limitaciones Críticas o Cuellos de Botella Identificados:** 
   * **Inestabilidad en la Detección de Bordes del Péndulo:** El algoritmo Canny se ejecutaba sobre el encuadre completo de la imagen sin preprocesamiento de contraste, provocando falsos positivos en el cálculo de posición del hilo debido al fondo ruidoso.
   * **Falsa Clasificación de Eventos Sísmicos en MQTT:** En el servidor concentrador (`Server-Pi`), cuando una única estación generaba múltiples eventos sucesivos durante la ventana de agregación, el script `extractor_eventos_MQTT.py` clasificaba erróneamente el disparo como un evento local/sísmico en lugar de catalogarlo como un evento aislado de un solo sensor.
   * **Inexistencia de Enrutamiento Automatizado en Túneles:** La topología de la presa exigía comunicación inalámbrica Ad-Hoc entre nodos y gateways intermedios; la configuración manual de interfaces de red (`wlan0`) y tablas de ruteo estático (`rutasEstaticas.sh`) resultaba propensa a errores humanos y dificultaba el despliegue en campo.
   * **Persistencia y Respaldo Incompletos:** Las mediciones generadas quedaban dispersas localmente en las tarjetas SD de las Raspberry Pi sin un pipeline automático de respaldo en la nube (Google Drive) ni sincronización periódica hacia brokers MQTT remotos.
   * **Errores de Conectividad en Hardware:** Existencia de fallas en esquemáticos y pistas de circuitos impresos previos para módulos de prácticas que impedían su fabricación confiable.

3. **Desacoplamiento del Alcance y Propuesta de Solución:** 
   El plan de trabajo aborda de manera modular:
   * La corrección y estandarización del flujo de eventos MQTT en acelerógrafos, implementando conteo de estaciones únicas para una correcta categorización sísmica.
   * La corrección formal del diseño esquemático y ruteo PCB en Altium Designer con verificación de reglas de diseño (DRC) y generación de Gerbers.
   * El perfeccionamiento del pipeline de visión por computador en OpenCV (filtro gaussiano, ecualizador de histograma, reducción del área de Canny a ROI de interés y evaluación de un controlador adaptativo de ventana móvil).
   * La automatización integral de la configuración de red Ad-Hoc y ruteo multihop en Linux mediante scripts parametrizados por JSON.
   * La implementación del sistema de persistencia en CSV, publicación periódica MQTT cada 2 horas y respaldo mensual comprimido en Google Drive.

---

## 2. Objetivos del Plan de Trabajo

### 2.1. Objetivo General
Desarrollar, optimizar e integrar un sistema integral de adquisición de datos, procesamiento por visión por computador, enrutamiento en red Ad-Hoc y telemetría automatizada (MQTT y Google Drive) para los sensores estructurales de la Presa Chanlud (coordinómetros, vertederos y acelerógrafos), incluyendo la corrección de diseños electrónicos en Altium Designer y la estandarización del software embebido en Raspberry Pi.

### 2.2. Objetivos Específicos
1. Desarrollar e implementar los scripts en Python para la publicación estandarizada (`PublicarEventoMQTT.py`) y extracción/clasificación de eventos sísmicos (`extractor_eventos_MQTT.py`) bajo el protocolo MQTT, discriminando eventos aislados de locales/sísmicos mediante el conteo de estaciones únicas en ventanas de espera.
2. Corregir y completar el diseño de hardware de la PCB de prácticas preprofesionales en Altium Designer, depurando esquemáticos, asignando footprints correctos, resolviendo violaciones de DRC y generando la documentación de fabricación (Gerbers, Drill y Pick&Place).
3. Optimizar el algoritmo de visión artificial para la medición de posición en coordinómetros (`imageDetected2.py`), aplicando un filtro gaussiano para reducción de ruido, ecualización de histograma para contraste y delimitación del área de Canny a la región de interés (ROI).
4. Investigar y evaluar experimentalmente la propuesta de un controlador adaptativo de ventana móvil para el seguimiento dinámico del hilo del péndulo ante oscilaciones mecánicas.
5. Desarrollar la herramienta de software `automated_configuration.py` para la autoconfiguración de nodos en red Ad-Hoc (`wlan0`) y enrutamiento estático multihop a partir de archivos de configuración centralizados en formato JSON.
6. Implementar el módulo de telemetría y persistencia `almacenamiento_transmision.py` para la emisión periódica de estados por MQTT cada 2 horas, almacenamiento local de registros en CSV y empaquetado mensual comprimido (`tar.gz`) con subida desatendida a Google Drive.
7. Validar experimentalmente las soluciones en el entorno embebido de la Raspberry Pi (10.22.147.186) y redactar el informe técnico de prácticas preprofesionales con la documentación de soporte.

---

## 3. Plan de Trabajo Detallado y Checkpoints Cuantificables

### Fase 1: Publicación y Clasificación de Eventos Sísmicos por MQTT (Acelerógrafos)
* **Duración:** 20 horas (Semanas 1 y 2)
* **Objetivo:** Estandarizar la publicación de eventos detectados por acelerógrafos y refactorizar el extractor de eventos en el servidor central para eliminar la falsa clasificación de eventos aislados.
* **Actividades técnicas:**
  1. Desarrollar el script `PublicarEventoMQTT.py` para publicar alertas de acelerógrafos recibiendo como parámetros: Fecha (AAMMDD), Hora (segundos del día) y Duración (segundos).
  2. Formatear la carga útil en JSON e integrar variables dinámicas de ubicación y dispositivo (ej. `chanlud`, `cha01`) hacia el tópico MQTT `registrocontinuo/eventos`.
  3. Auditar el código de `extractor_eventos_MQTT.py` en el equipo `Server-Pi` (`/home/rsa/pasantías/mqtt/`).
  4. Modificar la función `procesamiento_datos(alm)` para rastrear identificadores únicos de estación: si múltiples eventos en la ventana de espera provienen de una sola estación, categorizar como evento *aislado*; si provienen de estaciones distintas, clasificar como evento *local* o *sísmico*.
  5. Realizar pruebas de estrés con simulaciones de tráfico MQTT validando la matriz de confusión de eventos.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 1.1:** Script `PublicarEventoMQTT.py` ejecutándose correctamente por línea de comandos y publicando tramas JSON válidas en el broker.
  - [ ] **CP 1.2:** Lógica de `extractor_eventos_MQTT.py` refactorizada con filtrado por ID único de estación.
  - [ ] **CP 1.3:** Ensayos de simulación completados sin falsos positivos de eventos locales ante ráfagas de una única estación.

---

### Fase 2: Corrección de Hardware y Diseño de PCB en Altium Designer
* **Duración:** 24 horas (Semanas 2 y 3)
* **Objetivo:** Subsanar los errores de diseño electrónico y generar los archivos de fabricación para la PCB del módulo de prácticas de instrumentación.
* **Actividades técnicas:**
  1. Revisar los esquemáticos del proyecto `PCB_Practicas-Preprofesionales` e inspeccionar conexiones de peinetas, sensores, resistencias y etapas de alimentación.
  2. Crear y actualizar las librerías esquemáticas (`SchLib`) y de huellas (`PcbLib`) para garantizar encapsulados normalizados.
  3. Ejecutar la sincronización Engineering Change Order (ECO) entre esquemático y PCB layout (`PCB1-Practicas_Pre.PcbDoc`).
  4. Rediseñar el ruteo de pistas críticas, planos de alimentación y plano de masa (GND).
  5. Ejecutar la comprobación de reglas de diseño (Design Rule Check - DRC) hasta alcanzar 0 violaciones de espaciado, anchos de pista y conectividad.
  6. Generar el paquete completo de fabricación Gerber (GTL, GBL, GTS, GBS, GTO, GKO), archivos de taladrado (NC Drill `.TXT`), reporte de estado y archivo de componentes.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 2.1:** Esquemático y librerías de componentes depurados sin errores de compilación en Altium.
  - [ ] **CP 2.2:** Reporte DRC (`Design Rule Check - PCB1-Practicas_Pre.html`) con 0 advertencias y 0 violaciones de ruteo.
  - [ ] **CP 2.3:** Directorio `Project Outputs` con conjunto completo de archivos Gerber y Drill listos para manufactura.

---

### Fase 3: Preprocesamiento y Detección de Bordes en Coordinómetros (OpenCV)
* **Duración:** 28 horas (Semanas 4 y 5)
* **Objetivo:** Incrementar la precisión y robustez en la medición óptica de desplazamientos del hilo del péndulo mediante técnicas avanzadas de procesamiento digital de imágenes.
* **Actividades técnicas:**
  1. Establecer conexión remota con la Raspberry Pi de coordinómetros (IP `10.22.147.186`, usuario `rsa`) vía SSH, VSCode y RealVNC.
  2. Analizar el banco de imágenes de prueba capturadas en el túnel de la presa (`/home/rsa/fotos/`).
  3. Incorporar etapa de preprocesamiento en `imageDetected2.py`:
     - Filtro gaussiano para suavizado y atenuación de ruido de alta frecuencia.
     - Ecualización de histograma para maximizar el contraste relativo entre el hilo y el fondo del coordinómetro.
  4. Implementar la reducción del área de análisis: restringir el algoritmo de Canny exclusivamente a la mitad o franja crítica de la imagen donde oscila el hilo.
  5. Desarrollar la rutina de cálculo métrico del desplazamiento y exportación estructurada de resultados a formato CSV en `/home/rsa/resultados/`.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 3.1:** Entorno de desarrollo remoto en Raspberry Pi validado y operativo vía VNC/VSCode.
  - [ ] **CP 3.2:** Pipeline de preprocesamiento (Gauss + Ecualización) integrado y documentado en `imageDetected2.py`.
  - [ ] **CP 3.3:** Reducción de falsas detecciones comprobada visualmente en `/home/rsa/resultados/tmp/` y registros consolidados en archivos CSV.

---

### Fase 4: Algoritmo de Seguimiento Adaptativo (Controlador de Ventana Móvil)
* **Duración:** 18 horas (Semana 6)
* **Objetivo:** Explorar y evaluar una arquitectura de controlador dinámico de área de interés para el seguimiento focalizado de bordes en el coordinómetro.
* **Actividades técnicas:**
  1. Diseñar el algoritmo de ventana móvil en `/home/rsa/pasantías/idea_controlador/imageDetected2.py`: inicializar la ROI en la posición más probable del hilo y desplazar dinámicamente la ventana en cada iteración según el borde más izquierdo/derecho detectado.
  2. Implementar mecanismos de detección de bordes singulares vs. bordes pares.
  3. Realizar pruebas de seguimiento con series fotográficas secuenciales.
  4. Caracterizar los límites operacionales del controlador: identificar condiciones de pérdida de seguimiento o falsas detecciones ante saltos mecánicos abruptos que excedan la ventana.
  5. Documentar el análisis comparativo entre la ventana fija optimizada y el controlador adaptativo.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 4.1:** Script experimental del controlador adaptativo desarrollado y ejecutable en la Raspberry Pi.
  - [ ] **CP 4.2:** Reporte de desempeño técnico documentando beneficios de resolución y escenarios de vulnerabilidad por desplazamiento excesivo.

---

### Fase 5: Automatización de Infraestructura de Red Ad-Hoc en Nodos Linux
* **Duración:** 20 horas (Semanas 7 y 8)
* **Objetivo:** Crear una herramienta automatizada que configure las interfaces inalámbricas y tablas de ruteo estático para despliegue de redes Ad-Hoc en la presa.
* **Actividades técnicas:**
  1. Estandarizar la plantilla de red Ad-Hoc (`wlan0` en canal 5, ESSID `REDRSAIOT`, clave WEP/WPA estática).
  2. Estructurar el archivo de configuración centralizado `Configuracion_ad-hoc.json` definiendo roles: `ip-local-host`, `ip-gateway-intermedio`, `ip-gateway-principal`.
  3. Diseñar la lógica de enrutamiento estático en `rutasEstaticas.sh`:
     - Caso Nodo Cliente: Enrutamiento hacia gateway intermedio con métrica 1 y salida a gateway principal con métrica 2.
     - Caso Gateway Intermedio (`ip-local-host == ip-gateway-intermedio`): Conmutación de rutas bidireccionales y activación de reenvío de paquetes (`sysctl net.ipv4.ip_forward=1`).
  4. Desarrollar el programa en Python `automated_configuration.py` que parsea el JSON y escribe dinámicamente `/etc/network/interfaces.d/wlan0` y `/etc/network/rutasEstaticas.sh`.
  5. Ejecutar ensayos de interconexión y comprobación de saltos de red mediante `ping`, `traceroute` y resolución de IPs.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 5.1:** Archivos plantilla (`format_wlan0`, `format_rutasEstaticas.sh`) y esquema JSON validados sintácticamente.
  - [ ] **CP 5.2:** Script `automated_configuration.py` completado, aplicando cambios de red sin intervención manual en archivos del sistema.
  - [ ] **CP 5.3:** Conectividad multihop verificada entre cliente, gateway intermedio y gateway principal en entorno Ad-Hoc.

---

### Fase 6: Sistema de Transmisión Automática MQTT, Persistencia y Respaldo en Cloud
* **Duración:** 22 horas (Semanas 8 y 9)
* **Objetivo:** Implementar la orquestación desatendida para envío de telemetría a servidores remotos y respaldo mensual de imágenes en Google Drive.
* **Actividades técnicas:**
  1. Integrar el cliente MQTT en `almacenamiento_transmision.py` / `mqtt.py`: publicación periódica cada 2 horas de las coordenadas detectadas hacia el tópico `coordinometro/status`.
  2. Diseñar el gestor de almacenamiento local en Raspberry Pi estructurando carpetas por sensor y fecha.
  3. Programar la rutina de empaquetado mensual: compresión en lote de las imágenes procesadas en formato `tar.gz`.
  4. Configurar la autenticación y script de sincronización con la API de Google Drive para subir automáticamente los paquetes comprimidos a la carpeta remota `Datos Presa Chanlud/Coordinometros`.
  5. Programar la ejecución desatendida mediante servicios del sistema o tareas cron en Linux.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 6.1:** Publicación telemática periódica activa en broker MQTT con recepción verificada de estados cada 2 horas.
  - [ ] **CP 6.2:** Módulo de compresión `.tar.gz` operando sin saturar la capacidad de memoria de la Raspberry Pi.
  - [ ] **CP 6.3:** Carga automatizada y verificada de archivos de respaldo en Google Drive (`Datos Presa Chanlud/Coordinometros`).

---

### Fase 7: Pruebas Integradas, Documentación Técnica e Informe de Prácticas
* **Duración:** 12 horas (Semanas 10 y 10.5)
* **Objetivo:** Consolidar todas las herramientas desarrolladas, verificar la estabilidad del sistema completo y redactar el informe técnico final.
* **Actividades técnicas:**
  1. Ejecutar una prueba integrada continua: captura, procesamiento Canny, registro CSV, publicación MQTT y respaldo.
  2. Compilar la documentación de credenciales, puertos, tópicos MQTT y comandos de ejecución para cada programa.
  3. Redactar el Informe Final de Prácticas Preprofesionales detallando metodología, arquitectura de software, algoritmos implementados, resultados obtenidos y recomendaciones para futuros pasantes.
  4. Revisión técnica con el tutor institucional y suscripción de actas de finalización.
* **Checkpoints medibles para revisión:**
  - [ ] **CP 7.1:** Sistema completo ejecutándose de forma estable en la Raspberry Pi de prueba.
  - [ ] **CP 7.2:** Documento `informe_practicas.docx` finalizado y validado por el tutor institucional de la RSA.

---

## 4. Resumen de Fases y Cronograma Semanal (144 Horas)

El plan de trabajo se estructuró en un total de **144 horas** distribuidas en bloques semanales de **14 horas** (salvo las semanas quincenales y de cierre):

| Fase | Semanas | Horas | Enfoque Principal |
| :---: | :---: | :---: | :--- |
| **Fase 1** | Semanas 1–2 | **20 h** | Publicación y extracción de eventos MQTT para acelerógrafos; corrección de eventos aislados. |
| **Fase 2** | Semanas 2–3 | **24 h** | Corrección de esquemáticos, PCB Layout, reglas DRC y salida de Gerbers en Altium. |
| **Fase 3** | Semanas 4–5 | **28 h** | Preprocesamiento (Gauss/Histograma), optimización de Canny y exportación CSV de coordinómetros. |
| **Fase 4** | Semana 6 | **18 h** | Algoritmo adaptativo de ventana móvil para seguimiento dinámico de bordes del péndulo. |
| **Fase 5** | Semanas 7–8 | **20 h** | Automatización de configuración de red Ad-Hoc y ruteo multihop en Linux desde JSON. |
| **Fase 6** | Semanas 8–9 | **22 h** | Telemetría MQTT cada 2 horas, persistencia local y respaldo automático en Google Drive (`.tar.gz`). |
| **Fase 7** | Semanas 10–10.5 | **12 h** | Ensayos integrados de larga duración, manual de operación e Informe Final de Prácticas. |
| **Total** | **~10.5 Semanas** | **144 h** | **Planificación Global de Prácticas Preprofesionales** |

---

## 5. Entregables y Criterios de Éxito de la Pasantía

1. **Hardware y Diseño Electrónico en Altium Designer:**
   * Proyecto `PCB_Practicas-Preprofesionales` depurado con esquemático sin errores de pines ni nodos flotantes.
   * Archivo de PCB (`PCB1-Practicas_Pre.PcbDoc`) ruteado y verificado con 0 violaciones de reglas de diseño (DRC).
   * Paquete de producción industrial en `Project Outputs` con archivos Gerber, Drill y reporte de estado.
2. **Software de Mensajería y Clasificación Sísmica (MQTT):**
   * Script `PublicarEventoMQTT.py` para publicación estructurada de eventos detectados por acelerógrafos.
   * Script `extractor_eventos_MQTT.py` refactorizado con conteo de estaciones únicas, garantizando clasificación infalible entre eventos aislados y sismos locales.
3. **Pipeline de Visión Artificial para Coordinómetros:**
   * Script `imageDetected2.py` con etapas de filtro gaussiano, ecualizador de histograma y Canny acotado a ROI.
   * Prototipo experimental de controlador dinámico de ventana móvil en `/home/rsa/pasantías/idea_controlador/`.
   * Registros de series de tiempo de desplazamiento en archivos `.csv` en `/home/rsa/resultados/`.
4. **Automatización de Redes Ad-Hoc en Linux:**
   * Script `automated_configuration.py` para despliegue desatendido de `/etc/network/interfaces.d/wlan0` y `/etc/network/rutasEstaticas.sh` basado en esquemas JSON.
   * Soporte comprobado para nodos cliente y nodos con función de gateway intermedio con reenvío IP.
5. **Telemetría y Respaldo Cloud Automatizado:**
   * Script `almacenamiento_transmision.py` con envío telemático periódico cada 2 horas al tópico `coordinometro/status`.
   * Rutina de empaquetado mensual y sincronización desatendida a Google Drive en la carpeta `Datos Presa Chanlud/Coordinometros`.
6. **Informe Técnico Final y Documentación:**
   * Informe formal de prácticas preprofesionales documentando la metodología, código fuente, comandos de ejecución en Raspberry Pi y recomendaciones técnicas.
