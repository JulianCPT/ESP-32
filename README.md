<div align="center">

<!-- Título principal con efecto dinámico en SVG -->
<img src="https://capsule-render.vercel.app/api?type=waving&color=auto&height=220&section=header&text=ESP-WROOM-32&fontSize=70&fontAlignY=38&animation=twinkling&desc=Documentación%20Técnica%20y%20Arquitectura%20IoT&descSize=20&descAlignY=62&descAlign=50" width="100%" alt="Header ESP32"/>

<br>

<img src="https://altronics.cl/image/catalog/productos/electronica/tarjetas/esp32-nodemcu-esp32-s/esp32-nodemcu-esp32-s-5.jpg" alt="Descripción de la imagen">

**El corazón del desarrollo IoT: Arquitectura de hardware, periferia, gestión energética y entornos de desarrollo**

<br>

---

<br>

👤 Autor
**Julián Camilo Pérez Torres**  
*Documentación Técnica y Arquitectura de Sistemas Embebidos*

<br>

---

</div>

<br>

## 📌 Tabla de Contenidos

<br>

- [⚡ Documentación Técnica Completa: ESP-WROOM-32](#-documentación-técnica-completa-esp-wroom-32)
    - [👤 Autor](#-autor)
  - [📌 Tabla de Contenidos](#-tabla-de-contenidos)
  - [🧠 1. Definición y Arquitectura de la ESP32](#-1-definición-y-arquitectura-de-la-esp32)
    - [1.1 Visión General del Módulo ESP-WROOM-32](#11-visión-general-del-módulo-esp-wroom-32)
    - [1.2 Arquitectura Interna de Procesamiento](#12-arquitectura-interna-de-procesamiento)
    - [1.3 Estructura de Memoria y Almacenamiento](#13-estructura-de-memoria-y-almacenamiento)
    - [1.4 Coprocesador ULP (Ultra Low Power) y Modos de Energía](#14-coprocesador-ulp-ultra-low-power-y-modos-de-energía)
  - [🔌 2. Características Técnicas y Periféricos Avanzados](#-2-características-técnicas-y-periféricos-avanzados)
    - [2.1 Distribución Funcional de Pines (Pinout Detallado)](#21-distribución-funcional-de-pines-pinout-detallado)
    - [2.2 Interfaces de Comunicación Serie](#22-interfaces-de-comunicación-serie)
    - [2.3 📊 ADC (Convertidor Analógico a Digital)](#23--adc-convertidor-analógico-a-digital)
    - [2.4 🔊 DAC (Convertidor Digital a Analógico)](#24--dac-convertidor-digital-a-analógico)
    - [2.5 ⚙️ PWM (LEDC - Control de LED y Motores)](#25️-pwm-ledc---control-de-led-y-motores)
  - [⚖️ 3. Comparativa: Programación en C/C++ vs MicroPython](#️-3-comparativa-programación-en-cc-vs-micropython)
    - [3.1 Matriz de Comparación Técnica](#31-matriz-de-comparación-técnica)
    - [3.2 Análisis Profundo: C / C++ (ESP-IDF / Arduino Core)](#32-análisis-profundo-c--c-esp-idf--arduino-core)
    - [3.3 Análisis Profundo: MicroPython](#33-análisis-profundo-micropython)
  - [📚 4. Referencias y Bibliografía Oficial](#-4-referencias-y-bibliografía-oficial)

<br>

---

<br>

## 🧠 1. Definición y Arquitectura de la ESP32

### 1.1 Visión General del Módulo ESP-WROOM-32
Es un microcontrolador utilizado principalmente en sistemas embebidos, robótica, automatización, electrónica e Internet de las cosas, conocido como IoT.

Un microcontrolador es un circuito integrado que contiene los elementos necesarios para ejecutar un programa y controlar diferentes dispositivos electrónicos. El ESP32 se caracteriza por tener una gran cantidad de periféricos integrados, además de comunicación inalámbrica mediante Wi-Fi y Bluetooth.

Una de las principales ventajas del ESP32 es que puede recibir información de sensores, procesarla y posteriormente controlar otros dispositivos como motores, LEDs, relés o pantallas.

> 💡 **Nota de Innovación:** El módulo está diseñado con un cristal oscilador de 40 MHz integrado, memoria flash SPI dedicada y una antena en trazado de PCB, cumpliendo con certifaciones internacionales como FCC, CE, IC, TELEC, KCC y SRRC[cite: 1].

<br>

### 1.2 Arquitectura Interna de Procesamiento
El procesador central se basa en el chip **ESP32-D0WDQ6** (o variantes de la serie ESP32-D0WD)[cite: 1].

* 🏎️ **Unidad Central de Procesamiento (CPU):** Dual-Core Tensilica Xtensa® 32-bit LX6[cite: 1, 5].
  *  **Frecuencia de reloj:** Ajustable dinámicamente desde **80 MHz hasta 240 MHz**[cite: 1].
  *  **Rendimiento informático:** Hasta **600 DMIPS** (Dhrystone MIPS)[cite: 1].
  *  **Arquitectura Harvard:** Buses de datos e instrucciones independientes para acelerar la ejecución del código[cite: 2, 5].
* 🔋 **Coprocesador ULP:** Un microcontrolador complementario ultra eficiente basado en arquitectura RISC que permanece activo mientras la CPU principal se apaga[cite: 1, 2].

| Subsistema | Componentes y Capacidades |
| :--- | :--- |
| 🧩 **Núcleos CPU** | Core 0 (PRO_CPU: Protocol Core) y Core 1 (APP_CPU: Application Core)[cite: 2, 3]. |
| 🔒 **Aceleradores Criptográficos** | Hardware dedicado para AES (128/192/256-bit), SHA-2, RSA, ECC y Generador de Números Aleatorios (TRNG)[cite: 1, 2]. |
| 📡 **Radio de Comunicaciones** | Wi-Fi 802.11 b/g/n (hasta 150 Mbps) + Bluetooth v4.2 BR/EDR y BLE (Bluetooth Low Energy)[cite: 1]. |

<br>

---

### 1.3 Estructura de Memoria y Almacenamiento
El espacio de direccionamiento de memoria del ESP32 está segmentado para optimizar la velocidad de lectura y el almacenamiento no volátil[cite: 1, 2]:

1. 🏛️ **440 KB de ROM Interna:** Contiene el código de arranque (*Bootloader*), rutinas de inicialización del chip e interfaces de bajo nivel para periféricos[cite: 1, 2].
2. ⚡ **520 KB de SRAM Interna:** Dividida en bloques de memoria para datos e instrucciones, utilizada activamente por el sistema operativo de tiempo real (FreeRTOS)[cite: 1, 2, 3].
3. ⏱️ **8 KB de SRAM en RTC (Fast RTC Memory):** Accesible por la CPU principal cuando arranca desde un modo de reposo[cite: 1, 2].
4. 🌙 **8 KB de SRAM en RTC (Slow RTC Memory):** Accesible exclusivamente por el coprocesador ULP durante el modo *Deep Sleep*[cite: 1, 2].
5. 💾 **4 MB de Flash SPI Externa:** Memoria no volátil donde se almacena el binario del programa, el sistema de archivos (SPIFFS/LittleFS) y configuraciones NVS[cite: 1, 2].

<br>

---

### 1.4 Coprocesador ULP (Ultra Low Power) y Modos de Energía
El ESP32 implementa una gestión energética avanzada diseñada para aplicaciones IoT alimentadas por batería o energía solar[cite: 1, 2].

| Modo de Energía | Frecuencia / Estado | Consumo de Corriente Promedio |
| :--- | :--- | :--- |
| 🔴 **Active** | CPU dual a 240 MHz / Wi-Fi y Bluetooth activos[cite: 1] | ~160 mA a 260 mA (picos de 500 mA en TX)[cite: 1] |
| 🟡 **Modem-Sleep** | CPU activa / Modem Wi-Fi y BT desactivados[cite: 1] | ~20 mA a 68 mA[cite: 1] |
| 🟢 **Light-Sleep** | CPU pausada (Clock Gated) / RTC activo[cite: 1] | ~0.8 mA[cite: 1] |
| 🌙 **Deep-Sleep** | CPU apagada / Coprocesador ULP activo[cite: 1] | ~10 µA a 15 µA[cite: 1] |
| 💤 **Hibernation** | CPU apagada / Solo temporizador RTC a 150 kHz[cite: 1] | ~5 µA[cite: 1] |

> ⚠️ **Gestión Térmica y Alimentación:** En modo *Active* con transmisión Wi-Fi pico (802.11b, +20 dBm output), la placa consume picos de corriente elevados[cite: 1]. La fuente de alimentación externa debe ser capaz de suministrar dicha corriente limpiamente.

---

<br>

## 🔌 2. Características Técnicas y Periféricos Avanzados

### 2.1 Distribución Funcional de Pines (Pinout Detallado)
El módulo WROOM-32 expone un total de 38 pines físicos (36 pines GPIO disponibles externamente)[cite: 1].

> 🚫 **ADVERTENCIA DE VOLTAJE:** Las líneas de entrada/salida (GPIO) del ESP32 funcionan strictly a **3.3V**[cite: 1]. Aplicar 5V directamente causará daños irreversibles en el chip.

| Categoría | Pines GPIO | Descripción / Limitaciones Técnicas |
| :--- | :--- | :--- |
| 📥 **GPI (Sólo Entrada)** | `GPIO 34`, `GPIO 35`, `GPIO 36 (VP)`, `GPIO 39 (VN)`[cite: 1, 2] | No poseen transistores de salida ni resistencias internamente (*Pull-Up / Pull-Down*)[cite: 1, 2]. Solo funcionan como entradas digitales o analógicas[cite: 1, 2]. |
| 🚫 **Flash Reservados** | `GPIO 6`, `GPIO 7`, `GPIO 8`, `GPIO 9`, `GPIO 10`, `GPIO 11`[cite: 1, 2] | Interconectados a la memoria Flash SPI interna[cite: 1, 2]. **No deben ser utilizados bajo ninguna circunstancia.**[cite: 1, 2] |
| ⚙️ **Strapping Pins** | `GPIO 0`, `GPIO 2`, `GPIO 5`, `GPIO 12 (MTDI)`, `GPIO 15 (MTDO)`[cite: 1, 2] | Controlan el modo de arranque (*Bootloader*) y voltajes de la flash[cite: 1, 2]. Si se conectan cargas flotantes en el arranque, la placa puede no iniciar[cite: 1, 2]. |
| 👆 **Táctiles (Capacitive Touch)**| `T0 (GPIO 4)`, `T1 (GPIO 0)`, `T2 (GPIO 2)`, `T3 (GPIO 15)`, `T4 (GPIO 13)`, `T5 (GPIO 12)`, `T6 (GPIO 14)`, `T7 (GPIO 27)`, `T8 (GPIO 33)`, `T9 (GPIO 32)`[cite: 1, 2] | 10 sensores táctiles capacitivos integrados capaces de detectar variaciones por contacto humano[cite: 1, 2]. |

<br>

---

### 2.2 Interfaces de Comunicación Serie
* 🧷 **UART (Universal Asynchronous Receiver-Transmitter):** Protocolo de comunicación asíncrono punto a punto que utiliza dos líneas principales (TX para transmitir y RX para recibir). El ESP32 integra 3 controladores UART (`UART0`, `UART1`, `UART2`) con soporte para RS485 e IrDA, utilizados para la depuración por consola y conexión de módulos GPS o GSM[cite: 1, 2].
* ⚡ **SPI (Serial Peripheral Interface):** Bus de comunicación síncrono de 4 hilos (MOSI, MISO, CLK, CS) orientado a la alta velocidad de transferencia de datos. El ESP32 incluye 3 buses SPI (`SPI`, `HSPI`, `VSPI`) capaces de operar hasta a 80 MHz, ideales para pantallas TFT o tarjetas microSD[cite: 1, 2].
* 📟 **I2C (Inter-Integrated Circuit):** Protocolo serie síncrono de 2 hilos (SDA para datos y SCL para reloj) diseñado para interconectar múltiples sensores o pantallas usando una sola línea mediante un direccionamiento por software. Soporta velocidades de 100 kbit/s (Standar-mode) y 400 kbit/s (Fast-mode)[cite: 1, 2].
* 🎵 **I2S (Inter-IC Sound):** Interfaz estándar de bus serie enfocada en la transmisión de datos de audio digital continuo entre dispositivos como DACs externos, micrófonos MEMS y procesadores de sonido. El ESP32 dispone de 2 controladores I2S con soporte para acceso directo a memoria (DMA)[cite: 1, 2].

<br>

---

### 2.3 📊 ADC (Convertidor Analógico a Digital)
Un **ADC** (*Analog-to-Digital Converter*) es un circuito integrado que muestrea una tensión analógica continua proveniente del mundo físico (como la salida de un sensor de temperatura o potenciómetro) y la convierte en un valor digital cuantificado que el microcontrolador puede procesar[cite: 1, 2].

El ESP32 incluye dos módulos ADC de aproximación sucesiva (SAR ADC) con una resolución programable de **9 a 12 bits** (rango entero de 0 a 4095)[cite: 1, 2].

* 🔄 **Flujo de Lectura:** Entrada Analógica (0-3.3V) ➔ Etapa de Atenuación (0dB, 2.5dB, 6dB, 11dB) ➔ ADC de 12 bits ➔ Valor Digitalizado (0-4095)[cite: 1, 2].
* 🟢 **ADC1 (8 Canales):** Accesible a través de `GPIO 32`, `GPIO 33`, `GPIO 34`, `GPIO 35`, `GPIO 36`, `GPIO 37`, `GPIO 38` y `GPIO 39`[cite: 1, 2]. **Está totalmente disponible sin importar el estado del Wi-Fi.**[cite: 1, 2]
* 🔴 **ADC2 (10 Canales):** Asignado a `GPIO 0`, `GPIO 2`, `GPIO 4`, `GPIO 12`, `GPIO 13`, `GPIO 14`, `GPIO 15`, `GPIO 25`, `GPIO 26` y `GPIO 27`[cite: 1, 2].
  > 📌 **Restricción Crítica:** El convertidor **ADC2 está internamente enlazado a la pila de controladores de red Wi-Fi**[cite: 1, 2]. Por lo tanto, no se pueden tomar lecturas de ADC2 cuando la radio Wi-Fi está encendida y transmitiendo[cite: 1, 2].

<br>

---

### 2.4 🔊 DAC (Convertidor Digital a Analógico)
Un **DAC** (*Digital-to-Analog Converter*) realiza el proceso opuesto al ADC: transforma números enteros digitales codificados en código binario en un nivel de tensión analógico continuo.

A diferencia de la mayoría de microcontroladores que emulan señales analógicas aproximadas usando pulsos digitales (PWM), el ESP32 cuenta con **2 canales DAC reales de 8 bits** impulsados por una arquitectura interna de red de resistencias (R-2R)[cite: 1, 2].

* 🎚️ **Resolución:** 8 bits (Permite generar valores discretos entre `0` y `255`)[cite: 1, 2].
* 📍 **Canales y Pines Fijos:**
  * **DAC1:** Mapeado exclusivamente al pin `GPIO 25`[cite: 1, 2].
  * **DAC2:** Mapeado exclusivamente al pin `GPIO 26`[cite: 1, 2].
* 📈 **Rango de Salida:** Genera un voltaje que varía linealmente de `0.0V` hasta `VCC` (aproximadamente `3.3V`)[cite: 1, 2].
* 💡 **Casos de Uso:** Generación directa de formas de onda complejas (senoidales, triangulares), síntesis de audio analógico de baja fidelidad y control preciso de voltaje de referencia.

<br>

---

### 2.5 ⚙️ PWM (LEDC - Control de LED y Motores)
**PWM** (*Pulse Width Modulation* o Modulación por Ancho de Pulso) es una técnica utilizada para variar la cantidad de energía entregada a una carga encendiendo y apagando rápidamente una señal digital a una frecuencia constante. La proporción de tiempo que la señal permanece encendida (*Duty Cycle* o Ciclo de Trabajo) determina el voltaje promedio percibido.

El ESP32 no posee un temporizador PWM genérico tradicional; en su lugar, utiliza el hardware especializado **LEDC (LED Control)** diseñado para generar ondas cuadradas de alta precisión sin interrumpir el procesamiento del núcleo[cite: 1, 2]:

* 🎛️ **16 Canales Independientes:** Divididos en 2 grupos (High Speed y Low Speed) de 8 canales cada uno[cite: 1, 2].
* 🔀 **Asignación Flexible:** Cualquier canal LEDC se puede enlazar dinámicamente a **cualquier pin GPIO de salida digital**[cite: 1, 2].
* 📊 **Resolución Ajustable:** Configurable desde 1 bit hasta 16 bits de resolución para el ciclo de trabajo (*Duty Cycle*)[cite: 1, 2].
* 🌊 **Frecuencia:** Operación escalable desde frecuencias muy bajas hasta decenas de MegaHz, ideal para el atenuado suave de LEDs (dimming) y el control de servomotores o variadores de velocidad[cite: 1, 2].

---

<br>

## ⚖️ 3. Comparativa: Programación en C/C++ vs MicroPython

### 3.1 Matriz de Comparación Técnica

| Criterio | 🔷 C / C++ (ESP-IDF / Arduino Framework) | 🐍 MicroPython (Engine VM) |
| :--- | :--- | :--- |
| **Nivel de Abstracción** | Bajo / Medio (Control directo de hardware)[cite: 3] | Alto (Interpretación de código en runtime)[cite: 4] |
| **Tiempo de Compilación** | Largo (Compilación nativa con toolchain GCC)[cite: 3] | Inexistente (Carga directa del script `.py`)[cite: 4] |
| **Velocidad de Ejecución** | **Nativa (240 MHz purificación a ensamblador)**[cite: 1, 3] | Intermedia (Bucle de interpretación en la VM)[cite: 4] |
| **Uso de Memoria Flash** | Muy Bajo (~150 KB a 500 KB según librerías)[cite: 3] | Alto (~300 KB consumidos por el runtime)[cite: 4] |
| **Consumo de Memoria RAM**| Mínimo (Asignación manual e indexada)[cite: 3] | Elevado (Recolección de basura / Garbage Collector)[cite: 4] |
| **Sistema Operativo Subyacente**| **FreeRTOS integrado de forma transparente**[cite: 3] | Monohilo con arquitectura asíncrona (`uasyncio`)[cite: 4] |
| **Entorno Interactivo (REPL)**| No soportado[cite: 3] | **Soportado (Consola interactiva por USB/UART)**[cite: 4] |

<br>

---

### 3.2 Análisis Profundo: C / C++ (ESP-IDF / Arduino Core)

* 🔄 **Flujo de Trabajo:** Código C/C++ ➔ Compilador GCC Xtensa ➔ Binario Ejecutable `.bin` ➔ Ejecución Nativa en CPU[cite: 3].

#### 🟢 Ventajas
1.  **Rendimiento Crítico:** Adecuado para el procesamiento digital de señales (DSP), algoritmos de criptografía y lectura de sensores de ultra alta frecuencia[cite: 1, 3].
2.  **Control Multinúcleo Nativo:** Capacidad de asignar mediante **FreeRTOS** tareas específicas directamente a la `PRO_CPU` (Core 0) o a la `APP_CPU` (Core 1)[cite: 3].
3.  **Eficiencia Energética:** Permite apagar manualmente buses enteros y reducir los tiempos de despertar al mínimo posible para prolongar la vida útil de baterías[cite: 1, 3].

#### 🔴 Desventajas
1.  **Mayor Tiempo de Desarrollo:** Mayor complejidad en el manejo de memoria dinámica, punteros y desbordamientos de búfer[cite: 3].
2.  **Depuración Lenta:** Proceso prolongado de escribir código, compilar, flashear la memoria mediante UART y probar[cite: 3].

<br>

---

### 3.3 Análisis Profundo: MicroPython

* 🔄 **Flujo de Trabajo:** Script Python (`.py`) ➔ Firmware MicroPython (VM) ➔ Traducción en Tiempo Real ➔ CPU ESP32[cite: 4].

#### 🟢 Ventajas
1.  **Curva de Aprendizaje Mínima:** Desarrollar en sintaxis Python 3 reduce drásticamente el tiempo necesario para completar proyectos[cite: 4].
2.  **Entorno REPL (Read-Eval-Print Loop):** Permite conectarse por consola de comandos y probar líneas de código, comandos I2C o cambiar el estado de un pin en tiempo real sin reiniciar el microcontrolador[cite: 4].
3.  **Manejo Nativo de Datos:** Facilidad para estructurar objetos complejos, parsing de arrays JSON o peticiones HTTP/Sockets en pocas líneas[cite: 4].

#### 🔴 Desventajas
1.  **Sobrecostos de Latencia:** La recolección de basura (*Garbage Collection*) de Python puede pausar la ejecución del sistema durante milisegundos imprevistos[cite: 4].
2.  **Imposibilidad de Tiempo Real Estricto:** Inadecuado si la aplicación requiere responder a interrupciones en rangos de microsegundos ($\mu s$)[cite: 4].

---

<br>

## 📚 4. Referencias y Bibliografía Oficial

1. 📄 **Espressif Systems.** (2023). *ESP32 Series Datasheet* (Versión 4.1). Espressif Systems Documentation Center.  
   [https://www.espressif.com/sites/default/files/documentation/esp32_datasheet_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32_datasheet_en.pdf)

2. 📘 **Espressif Systems.** (2024). *ESP32 Technical Reference Manual* (Versión 5.2). Espressif Systems Architecture Team.  
   [https://www.espressif.com/sites/default/files/documentation/esp32_technical_reference_manual_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32_technical_reference_manual_en.pdf)

3. 🛠️ **Espressif Systems.** (2024). *ESP-IDF Programming Guide: FreeRTOS Architecture & Peripheral Drivers* (v5.2 Documentation).  
   [https://docs.espressif.com/projects/esp-idf/en/latest/esp32/](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/)

4. 🐍 **MicroPython Open Source Project.** (2024). *MicroPython documentation for the ESP32 platform*. MicroPython Core Development Team.  
   [https://docs.micropython.org/en/latest/esp32/quickref.html](https://docs.micropython.org/en/latest/esp32/quickref.html)

5. 🔬 **Cadence Design Systems.** (2021). *Tensilica Xtensa LX6 Microprocessor Architecture Overview*. Technical White Paper.  
   [https://www.cadence.com/en_US/home/tools/ip/tensilica-ip.html](https://www.cadence.com/en_US/home/tools/ip/tensilica-ip.html)

6. 📖 **Kolban, R.** (2018). *Kolban's Book on ESP32*. Publicación independiente sobre desarrollo embebido en arquitectura Xtensa LX6.  
   [https://github.com/nkolban/esp32-snippets](https://github.com/nkolban/esp32-snippets)
