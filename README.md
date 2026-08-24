# Guía Técnica: Módulo ESP-WROOM-32

Documentación completa sobre la arquitectura, pinout, periféricos clave (ADC, DAC, PWM) y un análisis comparativo entre los entornos de programación C/C++ y MicroPython para el módulo ESP-WROOM-32.

---

## 1. Definición y Arquitectura de la ESP32

El **ESP-WROOM-32** es un módulo Wi-Fi + Bluetooth/BLE de alto rendimiento desarrollado por Espressif Systems. Está diseñado para aplicaciones que van desde redes de sensores de baja potencia hasta el procesamiento intensivo de audio y voz.

### Estructura y Arquitectura Externa e Interna
* **Arquitectura del Procesador:** Microprocesador dual-core **Tensilica Xtensa 32-bit LX6** que opera a una frecuencia de reloj de hasta 240 MHz.
* **Coprocesador ULP (Ultra Low Power):** Permite realizar mediciones ADC y monitorear periféricos mientras los núcleos principales están en modo de reposo profundo (Deep Sleep).
* **Memoria Integrada:**
  * 520 KB de SRAM interna.
  * Memoria Flash externa de 4 MB (en la mayoría de módulos estándar).
* **Conectividad Inalámbrica:**
  * Wi-Fi 802.11 b/g/n (hasta 150 Mbps).
  * Bluetooth v4.2 BR/EDR y Bluetooth Low Energy (BLE).

---

## 2. Características y Periféricos Principales

### Resumen de Especificaciones
* **Voltaje de Alimentación (Placa):** 5V (vía USB) o 3.3V (Pin 3V3).
* **Voltaje de Operación de Pines (GPIO):** **3.3V** (*Importante: No tolera 5V directamente*).
* **Consumo de Corriente:** ~80 mA en funcionamiento normal; <10 µA en Deep Sleep.

### Conexiones y Mapa de Pines (Pinout)
La placa dispone de hasta 36 pines GPIO, aunque no todos están disponibles para uso general:
* **GPIs Exclusivos de Entrada:** Pines GPIO 34, 35, 36 (VP) y 39 (VN) solo funcionan como entradas digitales o analógicas (no tienen resistencias pull-up/pull-down internas ni salidas digitales).
* **Pines Reservados para Memoria Flash:** GPIO 6 a 11 están conectados internamente a la Flash y **no deben utilizarse**.
* **Pines Strapping (Arranque):** GPIO 0, 2, 5, 12 y 15 influyen en el modo de arranque de la placa (Boot Mode).

### ADC (Convertidor Analógico a Digital)
El ESP32 integra 2 convertidores ADC de 12 bits (resolución de 0 a 4095):
* **ADC1:** 8 canales (GPIO 32 a 39). Funciona sin restricciones.
* **ADC2:** 10 canales (GPIO 0, 2, 4, 12 a 15, 25 a 27). **Limitación:** No puede utilizarse mientras el módulo Wi-Fi esté activo.

### PWM (Modulación por Ancho de Pulso)
* El ESP32 no usa un canal PWM tradicional fijo por pin, sino un módulo **LEDC (LED Control)**.
* Dispone de **16 canales PWM independientes** asignables a cualquier GPIO de salida.
* Permite configurar la frecuencia de operación (hasta decenas de kHz) y la resolución del ciclo de trabajo (de 1 a 16 bits).

### DAC (Convertidor Digital a Analógico)
* Dispone de **2 canales DAC de 8 bits** (resolución de 0 a 255) para generar señales analógicas reales (onda senoidal, audio, etc.):
  * **DAC1:** GPIO 25 (Salida 0 a 3.3V).
  * **DAC2:** GPIO 26 (Salida 0 a 3.3V).

---

## 3. Comparativa: Programación en C/C++ vs MicroPython

| Criterio | C / C++ (ESP-IDF / Arduino Framework) | MicroPython |
| :--- | :--- | :--- |
| **Rendimiento** | **Máximo.** Ejecución compilada a código máquina directamente sobre el hardware. | **Moderado.** Interpretado mediante la máquina virtual de MicroPython. |
| **Uso de Memoria** | Muy eficiente. La huella de memoria en RAM y Flash es mínima. | Requiere ~300 KB de Flash solo para el firmware y consume más RAM. |
| **Curva de Aprendizaje**| Mayor complejidad en gestión de memoria, punteros y compilación. | **Baja.** Sintaxis muy limpia, idéntica a Python 3. |
| **Prototipado** | Lento (ciclo continuo de escribir code, compilar y flashear). | **Ultrarrápido.** Soporta ejecuciones en tiempo real mediante el REPL interactivo. |
| **Multitarea** | Integra soporte nativo y completo para **FreeRTOS**. | Control de concurrencia limitado (`uasyncio`), sin hilos reales por núcleo. |
| **Acceso al Hardware**| Control total de interrupciones, tareas ULP y registros a bajo nivel. | Abstracciones avanzadas; algunas funciones de bajo nivel están limitadas. |

### Ventajas y Desventajas

#### C / C++
* **Ventajas:** Máxima velocidad de procesamiento, optimización de batería y control absoluto del chip. Ideal para productos comerciales finales.
* **Desventajas:** Tiempos de compilación largos y mayor tiempo de desarrollo.

#### MicroPython
* **Ventajas:** Facilidad de uso, ideal para docencia, validación rápida de ideas y depuración sin volver a compilar.
* **Desventajas:** Inadecuado para procesamiento en tiempo real estricto o algoritmos pesados de matemáticas/criptografía.
