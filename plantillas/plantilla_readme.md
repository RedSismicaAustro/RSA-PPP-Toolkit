# [nombre-del-repositorio-en-kebab-case]

> **Red Sísmica del Austro (RSA) — Universidad de Cuenca**  
> Prácticas Preprofesionales (PPP) • [Área Temática Principal]

---

## 📋 Ficha Técnica del Proyecto

| Campo | Detalle |
| :--- | :--- |
| **Código Institucional** | `[ej. RSA-PPP-2026-12]` |
| **Nombre del Proyecto** | [Nombre formal y completo del proyecto de práctica] |
| **Pasante** | [Nombres y Apellidos] (`[correo.estudiante@ucuenca.edu.ec]`) |
| **Carrera / Institución** | [Ingeniería en Telecomunicaciones / Computación] — Universidad de Cuenca |
| **Tutor Institucional** | Ing. Milton Muñoz (`milton.munozc@ucuenca.edu.ec`) — RSA |
| **Duración / Horas** | [144] horas ([10.5] semanas) • [Presencial / Híbrida] |
| **Fecha de Ejecución** | [YYYY-MM-DD] al [YYYY-MM-DD] |
| **Estado** | `[En Ejecución / Culminado / Por Iniciar]` |

<!-- Descomentar si el proyecto es continuación de una fase previa:
> 📖 **Proyecto Precedente:** Este repositorio es la continuación técnica de [`[Código Precedente]` - [Nombre Precedente]]([URL-repositorio-precedente]). Para detalles de empalme, mapeo de archivos y lecciones aprendidas, consulta la guía [docs/referencia_proyecto_anterior.md](docs/referencia_proyecto_anterior.md).
-->

---

## 🎯 Descripción General y Objetivos

[Breve descripción de 2 a 3 párrafos del problema técnico, el propósito de la práctica y la solución desarrollada].

### Objetivos Clave
1. **[Objetivo 1 - Hardware/Entorno]:** [Descripción concisa].
2. **[Objetivo 2 - Firmware/Módulos Base]:** [Descripción concisa].
3. **[Objetivo 3 - Protocolos/Comunicación]:** [Descripción concisa].
4. **[Objetivo 4 - Memoria/Procesamiento]:** [Descripción concisa].
5. **[Objetivo 5 - Sensores/Adquisición]:** [Descripción concisa].
6. **[Objetivo 6 - Software de Soporte PC]:** [Descripción concisa].
7. **[Objetivo 7 - Validación Experimental y Documentación]:** [Descripción concisa].

> 📄 Para consultar el cronograma detallado semana a semana, la carga horaria y los checkpoints verificables, revisa el [Plan de Trabajo Oficial](docs/planificacion.md).

---

## 🏗️ Arquitectura del Sistema y Flujo de Trabajo

```text
+-----------------------------------------------------------------------+
|                       DIAGRAMA DE ARQUITECTURA                        |
+-----------------------------------------------------------------------+
|                                                                       |
|  [Hardware / Sensores] ---> [Microcontrolador / Firmware]             |
|                                         |                             |
|                                         v (Protocolo de Transporte)   |
|                             [Concentrador / Gateway]                  |
|                                         |                             |
|                                         v (Extracción / Almacenamiento)|
|                             [Suite PC / Base de Datos / Visualización]|
+-----------------------------------------------------------------------+
```

### Especificación Técnica de Buses y Formato de Datos
* **Interfaces y Protocolos:** [ej. SPI a 4 MHz, RS485 a 2 Mbps, UART 115200 bps, I2C 400 kHz].
* **Estructura de la Trama / Paquete (si aplica):**
  | Campo | Longitud | Descripción |
  | :--- | :---: | :--- |
  | **Cabecera** | [X] bytes | Identificador de sincronismo (`0xAA 0x55`) |
  | **ID Dispositivo** | [X] bytes | Dirección del nodo |
  | **Timestamp / Contador** | [X] bytes | Estampa de tiempo o contador incremental |
  | **Payload** | [X] bytes | Muestras físicas o datos adquiridos |
  | **Checksum / CRC** | [X] bytes | Comprobación de integridad |

---

## 📂 Estructura del Repositorio

> [!IMPORTANT]
> **Estructura Raíz Inmutable:** Para mantener la coherencia con los lineamientos de la RSA, la estructura de carpetas raíz no debe ser alterada ni renombrada sin previa coordinación con el tutor.

```text
[nombre-del-repositorio]/
├── data/
│   ├── evidence/              # Capturas instrumentales, registros binarios o forenses
│   └── raw_samples/           # Volcados de datos brutos (.bin, .raw, .csv de prueba)
│
├── docs/
│   ├── hardware/              # Reportes de hardware, esquemáticos, pinouts o conexionado
│   ├── troubleshooting/       # Bitácora de problemas técnicos detectados y soluciones
│   ├── planificacion.md       # Documento rector del plan de trabajo (144h) y checkpoints
│   └── referencia_proyecto_anterior.md  # (Opcional) Documento puente si es continuación
│
├── firmware/ (o software/)
│   ├── common/                # Definiciones globales, encabezados y tipos de datos
│   ├── drivers/               # Controladores de periféricos y librerías base
│   ├── [modulo_principal]/    # Código fuente principal de la aplicación
│   └── tests/                 # Firmwares/scripts de pruebas unitarias y diagnósticos
│
├── [herramientas_pc]/         # Scripts en Python / utilitarios para volcado y graficación
│   ├── requirements.txt       # Dependencias de software
│   └── [script_procesamiento].py
│
├── README.md                  # Este documento
└── .gitignore                 # Exclusiones de Git
```

---

## 🛠️ Herramientas y Requisitos de Desarrollo

* **Hardware / Instrumental:** [ej. Microcontrolador dsPIC33 / ESP32, Programador PICkit 3, Osciloscopio Hantek, Multímetro].
* **Entorno de Compilación:** [ej. MikroC PRO for dsPIC v7.1.0 / MPLAB X IDE v6.20 / VS Code + PlatformIO].
* **Herramientas de PC:**
  - Python 3.10+ (dependencias en `python/requirements.txt`):
    ```bash
    pip install -r python/requirements.txt
    ```
  - Editor Hexadecimal [HxD](https://mh-nexus.de/en/hxd/) (si aplica para análisis de memoria o flash).

---

## 🚀 Puesta en Marcha y Compilación

### 1. Clonar el repositorio
```bash
git clone https://github.com/RSA-PPP/[nombre-del-repositorio].git
cd [nombre-del-repositorio]
```

### 2. Compilación del Firmware
1. Abrir el entorno [IDE correspondiente].
2. Abrir el proyecto ubicado en `firmware/[modulo_principal]/`.
3. Compilar el proyecto (`Build`) y comprobar que no existan errores ni advertencias de desbordamiento de memoria.
4. Grabar en el microcontrolador mediante [PICkit / USB / FTDI].

### 3. Ejecución de Scripts de Apoyo
```bash
cd [herramientas_pc]/
python [script_procesamiento].py
```

---

## 🛡️ Reglas de Trabajo y Control de Versiones

Para asegurar la calidad y trazabilidad del proyecto durante las prácticas:

1. **Rama Principal:** La rama activa de trabajo es **`main`**. Realiza commits frecuentes y atómicos al finalizar cada bloque o jornada de trabajo.
2. **Formato de Commits:** Utiliza la convención estándar en minúsculas:
   - `feat: [nueva funcionalidad o controlador implementado]`
   - `fix: [corrección de bug en firmware, circuito o script]`
   - `docs: [actualización de documentación, esquemas o bitácora]`
   - `test: [incorporación o ejecución de pruebas unitarias/experimentales]`
   - `refactor: [optimización de código sin cambio de comportamiento]`
3. **Integridad de Archivos:** No subas archivos temporales, binarios pesados no requeridos (`.hex` intermedios, archivos temporales de compilador `.dci`, `.lst`) ni datos masivos. Apóyate en el archivo `.gitignore`.

---

## 📞 Contacto y Soporte Institucional

* **Tutor Institucional:** Ing. Milton Muñoz (`milton.munozc@ucuenca.edu.ec`)
* **Institución:** [Red Sísmica del Austro (RSA)](https://redsismicaaustro.github.io/RSA-Metodologias) — Universidad de Cuenca
