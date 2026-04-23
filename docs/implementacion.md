# Implementación con dispositivos reales (2026)

## Lector EEG (no invasivo)

| Dispositivo | Tipo | Canales | Certificación | Uso en bypass |
|:---|:---|:---|:---|:---|
| **Emotiv EPOC X** | Diadema (electrodos secos) | 14 | Comercial | Investigación, alta resolución |
| **Naox LINK** | Intraural (dentro del oído) | 1 | FDA 510(k) | Larga duración, uso doméstico |
| **OpenBCI** | DIY / open source | 8-16 | Comercial | Prototipos de bajo costo |

**Recomendación para prototipo:** Emotiv EPOC X (buena relación costo/canales) o OpenBCI (abierto, flexible).

## Chip de reconocimiento

- **Hardware:** STM32F405, FPGA (Xilinx Zynq), o DSP (Texas Instruments).
- **Modelo:** Clasificador LDA (lineal) o red neuronal pequeña (2-3 capas). El modelo se entrena **offline** (con datos de voluntarios sanos). El chip solo ejecuta inferencia.
- **Consumo:** < 100 mW. Puede alimentarse con ultracapacitores + captación ambiental.

## Estimulador (FES/tSCS)

| Tipo | Dispositivo | Estado | Uso en bypass |
|:---|:---|:---|:---|
| **FES de superficie** | Compex, NeuroTrac | Comercial | Controlado por el chip (señal TTL) |
| **tSCS** | Sistemas de investigación (SingHealth, 2026) | En desarrollo | No invasivo, aplicable sobre la piel lumbar |
| **Electrodos líquidos** | Liquid Metal Electrodes (Yonsei University) | Prototipo | Para larga duración, reducen irritación |

**Recomendación para prototipo:** FES de superficie con electrodos autoadhesivos (estándar). Para versiones avanzadas, tSCS lumbar.

## Inconsciente artificial

- **Hardware:** STM32F746.
- **Reflejos preprogramados:** Tablas de decisión (sistema experto).
- **Comunicación:** Con el chip de reconocimiento por bus interno (SPI, I2C) dentro del mismo implante.

## Comunicación y alimentación

### Comunicación inalámbrica (gorro → chip)

- **Tecnología:** Bluetooth Low Energy (BLE).
- **Alcance:** 1-2 metros (suficiente para la distancia del cuero cabelludo al pecho).
- **Seguridad:** Cifrado AES-128.
- **Módulos sugeridos:** Nordic nRF52, Texas Instruments CC2640.

### Comunicación interna (chip → estimulador)

- **Tecnología:** Cable (bus SPI o I2C) dentro del mismo implante.
- **No necesita wireless.** El chip y el estimulador están en la misma placa (o muy cerca).

### Alimentación del implante (chip + estimulador)

- **Tecnología:** Ultracapacitores de grafeno + captación de energía ambiental (calor corporal, movimiento, RF ambiental). Esta tecnología de energía autónoma (desarrollada en el ecosistema Mackiber Labs) elimina la necesidad de baterías químicas y recarga externa.
- **Autonomía:** El implante puede operar de forma indefinida mientras el paciente esté vivo y en movimiento.

### Alimentación del gorro EEG

- **Batería recargable por inducción** (como unos auriculares inalámbricos). El paciente lo deja sobre una base al dormir.
- **Opcional:** Captación solar (paneles flexibles en el gorro).
