<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=auto&height=220&section=header&text=ESP-WROOM-32&fontSize=70&fontAlignY=38&animation=twinkling&desc=Documentación%20Técnica%20y%20Arquitectura%20IoT&descSize=20&descAlignY=62&descAlign=50" width="100%" alt="Header ESP32"/>

<br>

<img src="https://altronics.cl/image/catalog/productos/electronica/tarjetas/esp32-nodemcu-esp32-s/esp32-nodemcu-esp32-s-5.jpg" alt="Módulo ESP-WROOM-32" width="750">

**El corazón del desarrollo IoT: Arquitectura de hardware, periferia, gestión energética y entornos de desarrollo**

![Lenguaje](https://img.shields.io/badge/C%2FC%2B%2B-ESP--IDF%20%7C%20Arduino-blue)
![MicroPython](https://img.shields.io/badge/MicroPython-Compatible-green)
![Licencia](https://img.shields.io/badge/Licencia-MIT-lightgrey)
![Estado](https://img.shields.io/badge/Estado-Documentación%20activa-brightgreen)

<br>

👤 **Autor**
**Julián Camilo Pérez Torres**
*Documentación Técnica y Arquitectura de Sistemas Embebidos*

</div>

<br>

---

## 📌 Tabla de Contenidos

- [🧠 1. Definición y Arquitectura de la ESP32](#-1-definición-y-arquitectura-de-la-esp32)
  - [1.1 Visión General del Módulo ESP-WROOM-32](#11-visión-general-del-módulo-esp-wroom-32)
  - [1.2 Arquitectura Interna de Procesamiento](#12-arquitectura-interna-de-procesamiento)
  - [1.3 Estructura de Memoria y Almacenamiento](#13-estructura-de-memoria-y-almacenamiento)
  - [1.4 Coprocesador ULP y Modos de Energía](#14-coprocesador-ulp-ultra-low-power-y-modos-de-energía)
- [🔌 2. Características Técnicas y Periféricos Avanzados](#-2-características-técnicas-y-periféricos-avanzados)
  - [2.1 Distribución Funcional de Pines](#21-distribución-funcional-de-pines-pinout-detallado)
  - [2.2 Interfaces de Comunicación Serie](#22-interfaces-de-comunicación-serie)
  - [2.3 ADC](#23--adc-convertidor-analógico-a-digital)
  - [2.4 DAC](#24--dac-convertidor-digital-a-analógico)
  - [2.5 PWM (LEDC)](#25️-pwm-ledc---control-de-led-y-motores)
- [⚖️ 3. Comparativa: C/C++ vs MicroPython](#️-3-comparativa-programación-en-cc-vs-micropython)
  - [3.1 Matriz de Comparación Técnica](#31-matriz-de-comparación-técnica)
  - [3.2 Análisis Profundo: C / C++](#32-análisis-profundo-c--c-esp-idf--arduino-core)
  - [3.3 Análisis Profundo: MicroPython](#33-análisis-profundo-micropython)
- [🚀 4. Primeros Pasos](#-4-primeros-pasos)
- [📚 5. Referencias y Bibliografía Oficial](#-5-referencias-y-bibliografía-oficial)

---

## 🧠 1. Definición y Arquitectura de la ESP32

### 1.1 Visión General del Módulo ESP-WROOM-32

Es un microcontrolador utilizado principalmente en sistemas embebidos, robótica, automatización, electrónica e Internet de las cosas (IoT).

Un microcontrolador es un circuito integrado que contiene los elementos necesarios para ejecutar un programa y controlar diferentes dispositivos electrónicos. El ESP32 se caracteriza por tener una gran cantidad de periféricos integrados, además de comunicación inalámbrica mediante Wi-Fi y Bluetooth.

Una de las principales ventajas del ESP32 es que puede recibir información de sensores, procesarla y posteriormente controlar otros dispositivos como motores, LEDs, relés o pantallas.

> 💡 **Nota de Innovación:** El módulo está diseñado con un cristal oscilador de 40 MHz integrado, memoria flash SPI dedicada y una antena en trazado de PCB, cumpliendo con certificaciones internacionales como FCC, CE, IC, TELEC, KCC y SRRC.

<br>

### 1.2 Arquitectura Interna de Procesamiento

El procesador central se basa en el chip **ESP32-D0WDQ6** (o variantes de la serie ESP32-D0WD).

* 🏎️ **Unidad Central de Procesamiento (CPU):** Dual-Core Tensilica Xtensa® 32-bit LX6.
  * **Frecuencia de reloj:** Ajustable dinámicamente desde **80 MHz hasta 240 MHz**.
  * **Rendimiento informático:** Hasta **600 DMIPS** (Dhrystone MIPS).
  * **Arquitectura Harvard:** Buses de datos e instrucciones independientes para acelerar la ejecución del código.

* 🔋 **Coprocesador ULP:** Un microcontrolador complementario ultra eficiente basado en arquitectura RISC que permanece activo mientras la CPU principal se apaga.

| Subsistema | Componentes y Capacidades |
| :--- | :--- |
| 🧩 **Núcleos CPU** | Core 0 (PRO_CPU: Protocol Core) y Core 1 (APP_CPU: Application Core). |
| 🔒 **Aceleradores Criptográficos** | Hardware dedicado para AES (128/192/256-bit), SHA-2, RSA, ECC y Generador de Números Aleatorios (TRNG). |
| 📡 **Radio de Comunicaciones** | Wi-Fi 802.11 b/g/n (hasta 150 Mbps) + Bluetooth v4.2 Classic (BR/EDR) y BLE (Bluetooth Low Energy). |

<br>

### 1.3 Estructura de Memoria y Almacenamiento

El espacio de direccionamiento de memoria del ESP32 está segmentado para optimizar la velocidad de lectura y el almacenamiento no volátil:

1. 🏛️ **448 KB de ROM Interna:** Contiene el código de arranque (*Bootloader*), rutinas de inicialización del chip e interfaces de bajo nivel para periféricos.
2. ⚡ **520 KB de SRAM Interna:** Dividida en bloques de memoria para datos e instrucciones, utilizada activamente por el sistema operativo de tiempo real (FreeRTOS).
3. ⏱️ **8 KB de SRAM en RTC (Fast RTC Memory):** Accesible por la CPU principal cuando arranca desde un modo de reposo.
4. 🌙 **8 KB de SRAM en RTC (Slow RTC Memory):** Accesible exclusivamente por el coprocesador ULP durante el modo *Deep Sleep*.
5. 💾 **4 MB de Flash SPI Externa (típico):** Memoria no volátil donde se almacena el binario del programa, el sistema de archivos (SPIFFS/LittleFS) y configuraciones NVS. La capacidad varía según el fabricante del módulo (2 MB, 4 MB, 8 MB o 16 MB).

<br>

### 1.4 Coprocesador ULP (Ultra Low Power) y Modos de Energía

El ESP32 implementa una gestión energética avanzada diseñada para aplicaciones IoT alimentadas por batería o energía solar.

| Modo de Energía | Frecuencia / Estado | Consumo de Corriente Promedio |
| :--- | :--- | :--- |
| 🔴 **Active** | CPU dual a 240 MHz / Wi-Fi y Bluetooth activos | ~160 mA a 260 mA (picos de 500 mA en TX) |
| 🟡 **Modem-Sleep** | CPU activa / Modem Wi-Fi y BT desactivados | ~20 mA a 68 mA |
| 🟢 **Light-Sleep** | CPU pausada (Clock Gated) / RTC activo | ~0.8 mA |
| 🌙 **Deep-Sleep** | CPU apagada / Coprocesador ULP activo | ~10 µA a 15 µA |
| 💤 **Hibernation** | CPU apagada / Solo temporizador RTC a 150 kHz | ~5 µA |

> ⚠️ **Gestión Térmica y Alimentación:** En modo *Active* con transmisión Wi-Fi pico (802.11b, +20 dBm de salida), la placa consume picos de corriente elevados. La fuente de alimentación externa debe ser capaz de suministrar dicha corriente limpiamente.

---

## 🔌 2. Características Técnicas y Periféricos Avanzados

### 2.1 Distribución Funcional de Pines (Pinout Detallado)

El módulo WROOM-32 expone un total de 38 pines físicos (30-36 pines GPIO disponibles externamente, según la variante).

> 🚫 **ADVERTENCIA DE VOLTAJE:** Las líneas de entrada/salida (GPIO) del ESP32 funcionan estrictamente a **3.3V**. Aplicar 5V directamente causará daños irreversibles en el chip.

| Categoría | Pines GPIO | Descripción / Limitaciones Técnicas |
| :--- | :--- | :--- |
| 📥 **GPI (Sólo Entrada)** | `GPIO 34`, `GPIO 35`, `GPIO 36 (VP)`, `GPIO 39 (VN)` | No poseen transistores de salida ni resistencias internas (*Pull-Up / Pull-Down*). Solo funcionan como entradas digitales o analógicas. |
| 🚫 **Flash Reservados** | `GPIO 6`, `GPIO 7`, `GPIO 8`, `GPIO 9`, `GPIO 10`, `GPIO 11` | Interconectados a la memoria Flash SPI interna. **No deben ser utilizados bajo ninguna circunstancia.** |
| ⚙️ **Strapping Pins** | `GPIO 0`, `GPIO 2`, `GPIO 5`, `GPIO 12 (MTDI)`, `GPIO 15 (MTDO)` | Controlan el modo de arranque (*Bootloader*) y voltajes de la flash. Si se conectan cargas flotantes en el arranque, la placa puede no iniciar. |
| 👆 **Táctiles (Capacitive Touch)** | `T0 (GPIO 4)`, `T1 (GPIO 0)`, `T2 (GPIO 2)`, `T3 (GPIO 15)`, `T4 (GPIO 13)`, `T5 (GPIO 12)`, `T6 (GPIO 14)`, `T7 (GPIO 27)`, `T8 (GPIO 33)`, `T9 (GPIO 32)` | 10 sensores táctiles capacitivos integrados capaces de detectar variaciones por contacto humano. |

<br>

### 2.2 Interfaces de Comunicación Serie

* 🧷 **UART:** Protocolo de comunicación asíncrono punto a punto que utiliza dos líneas principales (TX para transmitir y RX para recibir). El ESP32 integra 3 controladores UART (`UART0`, `UART1`, `UART2`) con soporte para RS485 e IrDA, utilizados para la depuración por consola y conexión de módulos GPS o GSM.
* ⚡ **SPI:** Bus de comunicación síncrono de 4 hilos (MOSI, MISO, CLK, CS) orientado a la alta velocidad de transferencia de datos. El ESP32 incluye 4 controladores SPI (2 de uso general: `HSPI`, `VSPI`) capaces de operar hasta a 80 MHz, ideales para pantallas TFT o tarjetas microSD.
* 📟 **I2C:** Protocolo serie síncrono de 2 hilos (SDA para datos y SCL para reloj) diseñado para interconectar múltiples sensores o pantallas usando una sola línea mediante direccionamiento por software. Soporta velocidades de 100 kbit/s (Standard-mode) y 400 kbit/s (Fast-mode).
* 🎵 **I2S:** Interfaz estándar de bus serie enfocada en la transmisión de datos de audio digital continuo entre dispositivos como DACs externos, micrófonos MEMS y procesadores de sonido. El ESP32 dispone de 2 controladores I2S con soporte para acceso directo a memoria (DMA).

<br>

### 2.3 📊 ADC (Convertidor Analógico a Digital)

Un **ADC** es un circuito integrado que muestrea una tensión analógica continua proveniente del mundo físico (como la salida de un sensor de temperatura o potenciómetro) y la convierte en un valor digital cuantificado que el microcontrolador puede procesar.

El ESP32 incluye dos módulos ADC de aproximación sucesiva (SAR ADC) con una resolución programable de **9 a 12 bits** (rango entero de 0 a 4095).

* 🔄 **Flujo de Lectura:** Entrada Analógica (0-3.3V) → Etapa de Atenuación (0dB, 2.5dB, 6dB, 11dB) → ADC de 12 bits → Valor Digitalizado (0-4095).
* 🟢 **ADC1 (8 Canales):** Accesible a través de `GPIO 32` a `GPIO 39`. **Está totalmente disponible sin importar el estado del Wi-Fi.**
* 🔴 **ADC2 (10 Canales):** Asignado a `GPIO 0`, `GPIO 2`, `GPIO 4`, `GPIO 12`-`15`, `GPIO 25`-`27`.

  > 📌 **Restricción Crítica:** El convertidor **ADC2 está internamente enlazado a la pila de controladores de red Wi-Fi**. Por lo tanto, no se pueden tomar lecturas de ADC2 cuando la radio Wi-Fi está encendida y transmitiendo.

<br>

### 2.4 🔊 DAC (Convertidor Digital a Analógico)

Un **DAC** realiza el proceso opuesto al ADC: transforma números enteros digitales codificados en binario en un nivel de tensión analógico continuo.

A diferencia de la mayoría de microcontroladores que emulan señales analógicas aproximadas usando pulsos digitales (PWM), el ESP32 cuenta con **2 canales DAC reales de 8 bits**.

* 🎚️ **Resolución:** 8 bits (valores discretos entre `0` y `255`).
* 📍 **Canales y Pines Fijos:** `DAC1` en `GPIO 25` · `DAC2` en `GPIO 26`.
* 📈 **Rango de Salida:** Voltaje que varía linealmente de `0.0V` hasta `VCC` (~3.3V).
* 💡 **Casos de Uso:** Generación de formas de onda (senoidales, triangulares), síntesis de audio analógico de baja fidelidad y control preciso de voltaje de referencia.

<br>

### 2.5 ⚙️ PWM (LEDC - Control de LED y Motores)

**PWM** es una técnica utilizada para variar la cantidad de energía entregada a una carga encendiendo y apagando rápidamente una señal digital a una frecuencia constante. La proporción de tiempo que la señal permanece encendida (*Duty Cycle*) determina el voltaje promedio percibido.

El ESP32 no posee un temporizador PWM genérico tradicional; en su lugar, utiliza el hardware especializado **LEDC (LED Control)**, diseñado para generar ondas cuadradas de alta precisión sin interrumpir el procesamiento del núcleo:

* 🎛️ **16 Canales Independientes:** Divididos en 2 grupos (High Speed y Low Speed) de 8 canales cada uno.
* 🔀 **Asignación Flexible:** Cualquier canal LEDC se puede enlazar dinámicamente a cualquier pin GPIO de salida digital.
* 📊 **Resolución Ajustable:** Configurable desde 1 hasta 16 bits para el ciclo de trabajo.
* 🌊 **Frecuencia:** Escalable desde frecuencias muy bajas hasta decenas de MHz, ideal para el atenuado suave de LEDs (*dimming*) y el control de servomotores o variadores de velocidad.

---

## ⚖️ 3. Comparativa: Programación en C/C++ vs MicroPython

### 3.1 Matriz de Comparación Técnica

| Criterio | 🔷 C / C++ (ESP-IDF / Arduino Framework) | 🐍 MicroPython (Engine VM) |
| :--- | :--- | :--- |
| **Nivel de Abstracción** | Bajo / Medio (control directo de hardware) | Alto (interpretación de código en runtime) |
| **Tiempo de Compilación** | Largo (compilación nativa con toolchain GCC) | Inexistente (carga directa del script `.py`) |
| **Velocidad de Ejecución** | **Nativa (ejecución directa a 240 MHz)** | Intermedia (bucle de interpretación en la VM) |
| **Uso de Memoria Flash** | Muy bajo (~150 KB a 500 KB según librerías) | Alto (~300 KB consumidos por el runtime) |
| **Consumo de Memoria RAM** | Mínimo (asignación manual e indexada) | Elevado (recolección de basura / Garbage Collector) |
| **Sistema Operativo Subyacente** | **FreeRTOS integrado de forma transparente** | Monohilo con arquitectura asíncrona (`uasyncio`) |
| **Entorno Interactivo (REPL)** | No soportado | **Soportado (consola interactiva por USB/UART)** |

<br>

### 3.2 Análisis Profundo: C / C++ (ESP-IDF / Arduino Core)

* 🔄 **Flujo de Trabajo:** Código C/C++ → Compilador GCC Xtensa → Binario ejecutable `.bin` → Ejecución nativa en CPU.

**🟢 Ventajas**
1. **Rendimiento Crítico:** Adecuado para procesamiento digital de señales (DSP), algoritmos de criptografía y lectura de sensores de ultra alta frecuencia.
2. **Control Multinúcleo Nativo:** Capacidad de asignar mediante **FreeRTOS** tareas específicas directamente a la `PRO_CPU` (Core 0) o a la `APP_CPU` (Core 1).
3. **Eficiencia Energética:** Permite apagar manualmente buses enteros y reducir los tiempos de despertar al mínimo posible para prolongar la vida útil de baterías.

**🔴 Desventajas**
1. **Mayor Tiempo de Desarrollo:** Mayor complejidad en el manejo de memoria dinámica, punteros y desbordamientos de búfer.
2. **Depuración Lenta:** Proceso prolongado de escribir código, compilar, flashear la memoria mediante UART y probar.

<br>

### 3.3 Análisis Profundo: MicroPython

* 🔄 **Flujo de Trabajo:** Script Python (`.py`) → Firmware MicroPython (VM) → Traducción en tiempo real → CPU ESP32.

**🟢 Ventajas**
1. **Curva de Aprendizaje Mínima:** Desarrollar en sintaxis Python 3 reduce drásticamente el tiempo necesario para completar proyectos.
2. **Entorno REPL:** Permite conectarse por consola de comandos y probar líneas de código, comandos I2C o cambiar el estado de un pin en tiempo real sin reiniciar el microcontrolador.
3. **Manejo Nativo de Datos:** Facilidad para estructurar objetos complejos, hacer *parsing* de arrays JSON o peticiones HTTP/Sockets en pocas líneas.

**🔴 Desventajas**
1. **Sobrecostos de Latencia:** La recolección de basura (*Garbage Collection*) de Python puede pausar la ejecución del sistema durante milisegundos imprevistos.
2. **Imposibilidad de Tiempo Real Estricto:** Inadecuado si la aplicación requiere responder a interrupciones en rangos de microsegundos (µs).

---

## 🚀 4. Primeros Pasos

### Requisitos

* Cable micro-USB (datos, no solo carga)
* Driver del chip USB-Serial (CP2102 o CH340, según el fabricante del módulo)
* Uno de los siguientes entornos: [Arduino IDE](https://www.arduino.cc/en/software), [PlatformIO](https://platformio.org/) o [ESP-IDF](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html)

### Ejemplo mínimo — C++ (Arduino Framework)

```cpp
#define LED_PIN 2

void setup() {
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  digitalWrite(LED_PIN, HIGH);
  delay(500);
  digitalWrite(LED_PIN, LOW);
  delay(500);
}
```

### Ejemplo mínimo — MicroPython

```python
from machine import Pin
from time import sleep

led = Pin(2, Pin.OUT)

while True:
    led.value(1)
    sleep(0.5)
    led.value(0)
    sleep(0.5)
```

---

## 📚 5. Referencias y Bibliografía Oficial

1. 📄 **Espressif Systems.** (2023). *ESP32 Series Datasheet* (Versión 4.1). Espressif Systems Documentation Center. [espressif.com](https://www.espressif.com/sites/default/files/documentation/esp32_datasheet_en.pdf)
2. 📘 **Espressif Systems.** (2024). *ESP32 Technical Reference Manual* (Versión 5.2). [espressif.com](https://www.espressif.com/sites/default/files/documentation/esp32_technical_reference_manual_en.pdf)
3. 🛠️ **Espressif Systems.** (2024). *ESP-IDF Programming Guide: FreeRTOS Architecture & Peripheral Drivers* (v5.2). [docs.espressif.com](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/)
4. 🐍 **MicroPython Open Source Project.** (2024). *MicroPython documentation for the ESP32 platform*. [docs.micropython.org](https://docs.micropython.org/en/latest/esp32/quickref.html)
5. 🔬 **Cadence Design Systems.** (2021). *Tensilica Xtensa LX6 Microprocessor Architecture Overview*. [cadence.com](https://www.cadence.com/en_US/home/tools/ip/tensilica-ip.html)
6. 📖 **Kolban, R.** (2018). *Kolban's Book on ESP32*. [github.com/nkolban/esp32-snippets](https://github.com/nkolban/esp32-snippets)

---

<div align="center">

**Documentación mantenida por Julián Camilo Pérez Torres**

</div>
