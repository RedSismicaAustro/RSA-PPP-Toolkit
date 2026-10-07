# Guía de Referencia y Transición Técnica
## Proyecto Precedente: [Nombre del Proyecto Anterior]

Este documento establece el puente formal de continuidad técnica entre el proyecto precedente desarrollado por **[Nombre del Pasante Anterior]** (`[Código-Precedente, ej. RSA-PPP-2026-09]`) y la fase actual ejecutada por **[Nombre del Pasante Actual]** (`[Código-Actual, ej. RSA-PPP-2026-11]`).

---

## 📌 Datos del Proyecto Base (Precedente)

| Parámetro | Detalle |
| :--- | :--- |
| **Código Institucional** | `[ej. RSA-PPP-2026-09]` |
| **Nombre del Proyecto** | [Nombre completo del proyecto precedente] |
| **Pasante Autor** | [Nombre del Pasante Anterior] (`[correo@ucuenca.edu.ec]`) |
| **Tutor Institucional** | Ing. Milton Muñoz (`milton.munozc@ucuenca.edu.ec`) — RSA |
| **Repositorio Oficial** | [[RSA-PPP/nombre-repo-anterior]](https://github.com/RSA-PPP/[nombre-repo-anterior]) |
| **Estado** | Culminado ([Mes Año]) |

---

## 🔍 Resumen de Logros Heredados (Qué está 100% probado y operativo)

> [!NOTE]
> Esta sección describe el punto de partida funcional. No es necesario re-probar desde cero lo que ya fue validado en la etapa anterior a menos que se indique explícitamente.

1. **[Módulo o Hardware 1]:**
   * [Descripción del circuito, placas fabricadas, voltajes o infraestructura operativa].
   * [Especificaciones probadas con instrumental].
2. **[Módulo de Firmware / Software 2]:**
   * [Configuraciones de periféricos, relojes, osciladores o servicios que ya funcionan con estabilidad].
3. **[Protocolos de Comunicación / Enlaces 3]:**
   * [Transceptores, velocidades de baudios, sincronismos o latencias medidas y validadas experimentalmente].

---

## ⚠️ Cuellos de Botella, Limitaciones o Incidencias Heredadas (¡Leer con Atención!)

> [!WARNING]
> En esta sección se documenta con precisión la causa raíz de cualquier problema técnico pendiente que motivó la apertura de la nueva fase de la pasantía.

### Diagnóstico del Problema Técnico:
* **Descripción de la anomalía o cuello de botella:**  
  [Explicar qué falló o qué no se pudo completar en la fase anterior: ej. incompatibilidad física, latencia excesiva, pérdida de paquetes, falta de detección mecánica, etc.].
* **Causa Raíz Identificada:**  
  [Detallar si se originó en hardware (diseño de PCB, componente), firmware (máquina de estados, bloqueos en interrupción) o software].

### Estrategia de Solución para el Proyecto Actual (`[Código-Actual]`):
1. **Acción 1 ([Hardware / Diseño]):** [Qué cambio físico o retrabajo se ejecutará].
2. **Acción 2 ([Firmware / Arquitectura]):** [Qué enfoque metodológico o de código resolverá la limitación].

---

## 🗺️ Mapa de Navegación del Repositorio Anterior

Si necesitas consultar el código fuente histórico o reportes en [[nombre-repo-anterior]](https://github.com/RSA-PPP/[nombre-repo-anterior]), utiliza este mapa como guía de orientación:

| Carpeta en Repo Anterior | ¿Qué contiene? | ¿Cómo utilizarlo en tu proyecto? |
| :--- | :--- | :--- |
| `firmware/` (o `src/`) | [Archivos principales históricos] | **Referencia de estructura:** [Indicar qué partes del código sirven de modelo]. |
| `[carpeta_comunicaciones]/` | [Scripts de red o protocolos] | **Referencia de comunicación:** [Indicar qué funciones pueden reutilizarse]. |
| `[carpeta_drivers]/` | [Controladores de bajo nivel probados] | **Librería base:** [Indicar si ya fueron migrados a tu repositorio o cómo consultarlos]. |
| `[carpeta_pruebas_aisladas]/` | [Ensayos preliminares o descontinuados] | **Solo lectura / Histórico:** Experimentos previos. No intentar compilarlos directamente. |
| `docs/` | Esquemas electrónicos, BOM y memorias técnicas | **Consulta obligatoria:** [Revisar diagramas, esquemáticos y pinout antes de intervenir hardware]. |

---

## 📦 Elementos Ya Migrados al Repositorio Actual

Para arrancar el proyecto de manera ordenada sin duplicar esfuerzos ni contaminar el historial con código obsoleto:

1. **Librerías y Drivers pre-cargados:**
   * `[ruta/archivo1]`: [Breve descripción de su función].
   * `[ruta/archivo2]`: [Breve descripción de su función].
2. **Documentación migrada:**
   * Los esquemáticos y notas técnicas relevantes se encuentran organizados en `docs/hardware/`.

---

## 🧭 Recomendaciones para el Nuevo Pasante

1. **No modificar librerías base sin consultar:** Si un driver heredado ya funciona, enfócate en el nuevo desarrollo y no alteres su lógica interna sin validar.
2. **Revisar la bitácora de incidencias:** Consulta la carpeta `docs/troubleshooting/` para no repetir errores o diagnósticos ya resueltos en el pasado.
3. **Validación paso a paso:** Antes de integrar todo el sistema, verifica cada módulo de forma aislada apoyándote en los firmwares de prueba de `firmware/tests/`.
