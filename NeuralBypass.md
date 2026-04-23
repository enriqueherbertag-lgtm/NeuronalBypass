# Bypass neuronal no invasivo para lesiones medulares

**Autores:** Enrique Aguayo H., DeepSeek (asistencia técnica)

**Licencia:** Copyright © 2026 Enrique Aguayo H. (Mackiber Labs). Uso no comercial permitido con atribución. Para uso comercial, contactar a eaguayo@migst.cl.

**Fecha:** abril de 2026

## Resumen

Se propone un sistema de bypass neuronal no invasivo que lee la señal cerebral del paciente (intención de movimiento), la reconoce (aunque sea débil o ruidosa), y la traduce en estimulación muscular funcional (FES) o estimulación espinal transcutánea (tSCS). A diferencia de los exoesqueletos, el paciente usa sus propios músculos, preserva la retroalimentación sensorial y activa sus circuitos de aprendizaje (dopamina). El sistema incluye un **inconsciente artificial** (basado en microcontrolador STM32) que ejecuta reflejos y movimientos autónomos si la señal falla.

**Principio clave:** El cerebro no necesita aprender a generar señales perfectas. El sistema reconoce la intención, y el cerebro se recalibra por sí mismo mediante la retroalimentación (neuroplasticidad).

## 1. El problema

Un paciente con lesión medular tiene el cerebro sano y los músculos sanos. El problema es el cable (la médula espinal) que conecta ambos. Los enfoques actuales usan exoesqueletos (robots externos) o estimuladores implantados que activan los músculos sin leer la intención del cerebro.

**Limitaciones de los enfoques actuales:**

- El paciente no siente el movimiento (no hay retroalimentación propioceptiva).
- El cerebro no aprende (no hay recalibración).
- La rehabilitación es pasiva.
- Los exoesqueletos son caros, pesados y requieren mantenimiento constante.

## 2. Entrenamiento externo del sistema

El chip de reconocimiento no "adivina" señales. **Se entrena con datos reales antes de ser implantado.** El proceso es el siguiente:

1. **Recolección de datos de voluntarios sanos** (5-10 personas). Cada voluntario realiza movimientos específicos (flexión de rodilla, extensión de codo, etc.) mientras se registra:
   - Señal cerebral (EEG, no invasivo).
   - Activación muscular real (EMG de superficie) o medición cinemática del movimiento (sensores inerciales).

2. **Entrenamiento centralizado (offline):** Los datos se envían (anónimos) a un servidor potente (o a la nube). Un algoritmo (clasificador LDA, red neuronal pequeña, SVM) aprende a asociar la señal cerebral (ruidosa) con la intención de movimiento (ej. "extender rodilla"). El resultado es un **modelo de reconocimiento** (no un autocompletado de señales).

3. **Instalación en el chip:** El modelo entrenado se compila a un formato ligero (matriz de pesos cuantizada, árbol de decisión) y se instala en el chip subcutáneo (como una actualización de firmware). El chip no aprende en tiempo real. Solo ejecuta el modelo.

4. **Calibración personal (opcional):** Cuando el paciente usa el sistema por primera vez, realiza una calibración corta (5-10 minutos). El chip ajusta los umbrales del modelo a su señal cerebral particular. No es re-entrenamiento completo. Es un ajuste fino de ganancias.

**El sistema es determinista, rápido y de bajo consumo. La parte pesada (el entrenamiento) ocurre fuera del cuerpo.**

## 3. Estado del arte: las dos mitades ya existen

### 3.1 Lectura de la intención (BCI)

Neuralink ha demostrado que la señal cerebral se puede leer, decodificar y traducir en comandos digitales en tiempo real:

- **21 pacientes** con parálisis (lesión medular, ELA) participan en ensayos clínicos del implante Telepathy.
- Han logrado controlar computadores, teléfonos inteligentes y brazos robóticos solo con el pensamiento.
- Velocidad de escritura de hasta **40 palabras por minuto** mediante teclado virtual.
- Tasa de transferencia de información superior a **10 bits por segundo** (equivalente al control de mouse en personas sin discapacidad) .
- El estudio **GB-PRIME** del University College London Hospitals reclutó a **7 pacientes** en Reino Unido para evaluar la seguridad y funcionalidad del implante N1 de Neuralink .

**Conclusión:** La lectura de la intención es técnicamente posible. El desafío es hacerlo sin cirugía (no invasivo).

### 3.2 Estimulación de la médula (tSCS) y músculos (FES)

La estimulación espinal transcutánea (tSCS) es una técnica **no invasiva** que ya ha demostrado restaurar el movimiento:

- **Zorkot et al. (2026)** desarrollaron algoritmos automatizados para colocación personalizada de electrodos en tSCS, mejorando la selectividad muscular y facilitando su adopción clínica .
- El estudio **AIM RECOVER** (SingHealth, 2026) está desarrollando un sistema de tSCS de lazo cerrado con IA que ajusta los parámetros de estimulación en tiempo real basándose en EMG y sensores cinemáticos .
- **Journal of NeuroEngineering and Rehabilitation (2025)** demostró una **brain-spine interface no invasiva** (EEG + tSCS) que decodifica la intención de movimiento de la pierna con un clasificador LDA, logrando un AUC promedio de **0.83 ± 0.06** .
- El **proyecto ReverseStroke** (EIC, 2026) busca desarrollar un "puente digital" cerebro-espina para restaurar el movimiento de manos y brazos en pacientes con accidente cerebrovascular .

**Conclusión:** La estimulación espinal no invasiva funciona y está en pleno desarrollo clínico. La combinación con FES de superficie es directa.

### 3.3 Integración EEG-FES (sistemas cerrados)

Ya existen prototipos de laboratorio que integran lectura de intención con estimulación muscular:

- **Universidade Estadual Paulista (2021)** demostró un sistema de control de lazo cerrado EEG-FES en un paciente con paraplejia. El sistema alcanzó una precisión del **77%** en la clasificación de la intención de movimiento de la pierna y apagaba automáticamente la estimulación al detectar fatiga muscular .
- **Springer (2026)** publicó un estudio sobre una interfaz humano-máquina que combina FES multicanal con sensores inerciales para rehabilitación de miembros inferiores en pacientes con SCI. Los pacientes realizaron ciclismo y caminata con mayor velocidad y cadencia usando el sistema .
- **arXiv (2026)** presentó una neuroprótesis portátil que decodifica señales EMG residuales (32 canales) para controlar FES en el pie de pacientes con SCI. Dos pacientes lograron aumentar el rango de flexión del pie en un **33.6% y 40%** del rango funcional saludable .

**Conclusión:** La integración de lectura de intención y estimulación muscular ya funciona en laboratorio. El desafío es la miniaturización y portabilidad.

## 4. La solución: bypass neuronal no invasivo

El sistema consta de cinco componentes, todos **externos o subcutáneos**, sin cirugía cerebral.

### 4.1 Lector de señales cerebrales (EEG no invasivo)

- **Tecnología:** EEG de superficie (gorro o diadema con electrodos secos).
- **Dispositivos reales (2026):** Emotiv EPOC X (14 canales), Naox LINK (intraural), OpenBCI.
- **Función:** Capta la intención de movimiento (señal débil, ruidosa, pero con patrón identificable).

### 4.2 Chip de reconocimiento de intención

- **Tecnología:** Clasificador simple (ej. LDA, árbol de decisión) o red neuronal pequeña.
- **Hardware:** STM32, FPGA, o DSP de bajo consumo.
- **Función:** No "autocompleta" la señal. **Reconoce si hay una intención** (ej. "quiere extender la rodilla") basándose en el modelo previamente entrenado con datos de voluntarios sanos.
- **Entrenamiento externo:** Ver sección 2.

### 4.3 Estimulador muscular (FES/tSCS)

- **Tecnología:** Estimulación muscular funcional (FES) con electrodos de superficie, o estimulación espinal transcutánea (tSCS).
- **Dispositivos reales (2026):** Compex, NeuroTrac, o sistemas de investigación como el tSCS descrito en .
- **Función:** Activa los músculos (o la médula) con un patrón de estimulación predefinido según la intención reconocida. No es personalizado. Es estándar para ese movimiento.

### 4.4 Inconsciente artificial (reflejos y seguridad)

- **Tecnología:** Microcontrolador STM32.
- **Funciones:**
  - Reflejos de emergencia (ej. retirar la pierna ante un obstáculo).
  - Homeostasis del sistema (temperatura, energía).
  - Fallback: si la señal cerebral falla o es incierta, ejecuta movimientos autónomos preprogramados.
  - **Seguridad vital (prioridad máxima):** Monitoriza los signos vitales del paciente (a través de la conexión neuronal opcional o sensores externos) y, ante cualquier anomalía, detiene todo movimiento, corta la estimulación y transmite una alerta de emergencia.

### 4.5 Comunicación y alimentación

- **Comunicación:** Bluetooth Low Energy (BLE) entre el gorro EEG (externo) y el chip subcutáneo (implante). El chip y el estimulador se conectan por cable (dentro del mismo implante).
- **Alimentación del implante:** Ultracapacitores de grafeno + captación de energía ambiental (calor corporal, movimiento). Esta tecnología de energía autónoma (desarrollada en el ecosistema Mackiber Labs) permite que el implante funcione sin baterías químicas ni recarga externa.
- **Alimentación del gorro:** Batería recargable por inducción (como unos auriculares inalámbricos). El paciente lo deja sobre una base al dormir.

## 5. Diagrama conceptual


[Cerebro del paciente]
↓
[Gorro EEG (externo)] → (Bluetooth LE) → [Chip subcutáneo (reconocimiento)]
↓
(cable dentro del implante)
↓
[Estimulador (FES/tSCS)]
↓
[Músculos / médula del paciente]
↓
[Inconsciente artificial (STM32)]
(reflejos, homeostasis, fallback, seguridad vital)



**Todos los componentes son externos o subcutáneos. No hay cirugía cerebral. No hay baterías que recargar (excepto el gorro, que se carga por inducción).**

## 6. Ventajas sobre exoesqueletos

| Aspecto | Exoesqueleto | Bypass neuronal (este proyecto) |
|:---|:---|:---|
| Movimiento | Máquina externa | **Propios músculos** |
| Retroalimentación | Limitada (visual) | **Natural (propioceptiva)** |
| Aprendizaje cerebral | Lento | **Rápido (dopamina)** |
| Peso | 10-20 kg | **< 1 kg** (implante + gorro) |
| Costo | Decenas de miles de USD | **Estimado < 10,000 USD** |
| Invasividad | No invasivo (externo) | **Mínima** (subcutáneo, sin cirugía cerebral) |
| Autonomía | Horas (baterías) | **Indefinida** (captación ambiental + inducción) |
| Rehabilitación | Pasiva | **Activa** (el cerebro se recalibra solo) |

## 7. Implementación con dispositivos reales (2026)

### Lector EEG (no invasivo)

| Dispositivo | Tipo | Canales | Certificación | Uso en bypass |
|:---|:---|:---|:---|:---|
| **Emotiv EPOC X** | Diadema (electrodos secos) | 14 | Comercial | Investigación, alta resolución |
| **Naox LINK** | Intraural (dentro del oído) | 1 | FDA 510(k) | Larga duración, uso doméstico |
| **OpenBCI** | DIY / open source | 8-16 | Comercial | Prototipos de bajo costo |

**Recomendación para prototipo:** Emotiv EPOC X (buena relación costo/canales) o OpenBCI (abierto, flexible).

### Chip de reconocimiento

- **Hardware:** STM32F405, FPGA (Xilinx Zynq), o DSP (Texas Instruments).
- **Modelo:** Clasificador LDA (lineal) o red neuronal pequeña (2-3 capas). El modelo se entrena **offline** (con datos de voluntarios sanos). El chip solo ejecuta inferencia.
- **Consumo:** < 100 mW. Puede alimentarse con ultracapacitores + captación ambiental.

### Estimulador (FES/tSCS)

| Tipo | Dispositivo | Estado | Uso en bypass |
|:---|:---|:---|:---|
| **FES de superficie** | Compex, NeuroTrac | Comercial | Controlado por el chip (señal TTL) |
| **tSCS** | Sistemas de investigación (SingHealth, 2026) | En desarrollo | No invasivo, aplicable sobre la piel lumbar |
| **Electrodos líquidos** | Liquid Metal Electrodes (Yonsei University) | Prototipo | Para larga duración, reducen irritación |

**Recomendación para prototipo:** FES de superficie con electrodos autoadhesivos (estándar). Para versiones avanzadas, tSCS lumbar.

### Inconsciente artificial

- **Hardware:** STM32F746.
- **Reflejos preprogramados:** Tablas de decisión (sistema experto).
- **Comunicación:** Con el chip de reconocimiento por bus interno (SPI, I2C) dentro del mismo implante.

## 8. Comunicación y alimentación

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

## 9. Limitaciones y desafíos

| Desafío | Solución propuesta | Estado |
|:---|:---|:---|
| **Variabilidad entre pacientes** | El modelo de reconocimiento se entrena con varios voluntarios sanos. Una calibración corta (5 minutos) ajusta umbrales por paciente. | En diseño. |
| **Latencia** | El clasificador LDA es rápido (< 10 ms en un STM32). El tiempo total (señal → FES) debe ser < 100 ms para movimientos fluidos. | En diseño (estimado). |
| **Irritación de la piel por electrodos** | Usar electrodos de hidrogel (desechables) o electrodos líquidos (Yonsei University). | En investigación. |
| **Rehabilitación activa** | El sistema es un asistente. El paciente debe entrenar para que su cerebro aprenda a emitir señales más claras. Con el tiempo, el reconocimiento de intención podría necesitar menos corrección. | Hipótesis por validar. |
| **Regulación médica (FDA, CE)** | El sistema usa componentes ya certificados (Emotiv, FES comerciales). El implante subcutáneo requeriría aprobación regulatoria. | Pendiente (fase de prototipo). |

## 10. Seguridad vital (prioridad máxima)

El inconsciente artificial monitoriza continuamente los signos vitales del paciente (a través de la conexión neuronal opcional o de sensores externos). Si detecta una anomalía, **no espera instrucciones del cerebro principal**. Actúa de inmediato.

### Condiciones de activación

- Frecuencia cardíaca fuera del rango seguro.
- Saturación de oxígeno (SpO2) por debajo del umbral.
- Temperatura corporal anormal.
- Pérdida de la conexión Bluetooth por más de `X` segundos.
- Detección de inmovilidad súbita (acelerómetro en el gorro o en el implante).

### Acciones inmediatas

1. **Detener cualquier movimiento** del paciente (prioridad absoluta).
2. **Cortar la estimulación** (si estaba activa).
3. **Transmitir alerta de emergencia** por LoRaWAN, satélite, o red móvil (si el paciente tiene un gateway doméstico).
4. **Enviar posición y signos vitales** al servicio de emergencias (o a los contactos predefinidos).
5. **Esperar instrucciones** (o ejecutar rutinas básicas de primeros auxilios, si el sistema tiene capacidad para hacerlo sin riesgo).

**Esta regla tiene prioridad sobre cualquier otra instrucción del sistema.**

## 11. Conclusión

El bypass neuronal no invasivo es una alternativa prometedora a los exoesqueletos y a los implantes invasivos. Es más barato, más ligero, y permite que el paciente use sus propios músculos, preservando la retroalimentación sensorial y el aprendizaje cerebral.

**Las dos mitades del sistema ya existen:** Neuralink (lectura de intención) y tSCS/FES (estimulación muscular/espinal). El proyecto propone integrarlas en un solo sistema cerrado, no invasivo y autónomo, con un inconsciente artificial (STM32) que gestiona reflejos, homeostasis y seguridad vital.

**Próximos pasos:**

1. **Prototipo de laboratorio:** Integrar un EEG comercial (Emotiv EPOC X) con un STM32 y un estimulador FES (Compex). Probar en voluntarios sanos.
2. **Validación de la hipótesis de neuroplasticidad:** Demostrar que el cerebro del paciente se recalibra con el uso del sistema.
3. **Prototipo subcutáneo:** Miniaturizar el chip de reconocimiento y el estimulador en un implante (sin baterías químicas, con ultracapacitores + captación ambiental).
4. **Ensayos clínicos:** En colaboración con instituciones médicas, probar en pacientes con lesión medular.

## 12. Referencias

1. **Neuralink GB-PRIME trial (UCLH, 2026).** Siete pacientes con implante N1 controlan computadores y teléfonos con el pensamiento .
2. **Neuralink Telepathy results (2026).** 21 pacientes, velocidad de escritura de 40 palabras por minuto, control de brazos robóticos .
3. **Zorkot et al. (2026).** Algoritmos automatizados para colocación de electrodos en tSCS no invasiva .
4. **AIM RECOVER trial (SingHealth, 2026).** Sistema de tSCS de lazo cerrado con IA para rehabilitación de SCI .
5. **Journal of NeuroEngineering and Rehabilitation (2025).** BSI no invasiva (EEG + tSCS) con clasificador LDA .
6. **Universidade Estadual Paulista (2021).** Sistema EEG-FES de lazo cerrado en paciente con paraplejia .
7. **Springer (2026).** HMI con FES multicanal + sensores inerciales para rehabilitación de miembros inferiores .
8. **arXiv (2026).** Neuroprótesis portátil EMG + FES para restauración de movimiento del pie .
9. **Mackiber Labs – tecnología de energía autónoma (ultracapacitores + captación ambiental).** Ecosistema de investigación aplicada.

---

**Autor:** Enrique Aguayo H. (Mackiber Labs) – Contacto: eaguayo@migst.cl – ORCID: 0009-0004-4615-6825

**Licencia:** Copyright © 2026 Enrique Aguayo H. Uso no comercial permitido con atribución. Para uso comercial, contactar al autor.



