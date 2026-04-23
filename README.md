[![Licencia](https://img.shields.io/badge/Licencia-Copyright%20%28c%29%202026%20Enrique%20Aguayo-red)](LICENSE)
[![Estado](https://img.shields.io/badge/Estado-Concepto%20en%20diseño-yellow)](https://github.com/enriqueherbertag-lgtm/NeuralBypass)
[![Asistencia IA](https://img.shields.io/badge/Asistencia%20IA-DeepSeek-brightgreen)](https://deepseek.com)
[![DOI](https://img.shields.io/badge/DOI-pending-blue)]()


# Bypass neuronal no invasivo para lesiones medulares

**Reconocimiento de intención (EEG) + estimulación muscular (FES/tSCS). Sin cirugía cerebral. Sin baterías externas.**

[![DOI](https://img.shields.io/badge/DOI-pending-blue)](https://zenodo.org) (próximamente)

## El problema

La médula está dañada, pero el cerebro y los músculos están sanos. El cable está roto.

## La solución

Leer la intención del paciente (aunque la señal sea débil), reconocer el patrón, y activar los músculos correspondientes (FES) o la médula (tSCS). El cerebro se recalibra solo con la retroalimentación.

**Principios clave:**
- **No invasivo:** EEG de superficie (gorro), estimulación transcutánea.
- **Entrenamiento externo:** El modelo aprende con voluntarios sanos. El chip solo ejecuta.
- **Autónomo:** Energía captada del cuerpo (ultracapacitores + calor, movimiento).
- **Seguro:** Protocolo de emergencia (signos vitales, alertas LoRaWAN/satélite).

## Componentes (dispositivos reales)

- **Lector EEG:** Emotiv EPOC X, Naox LINK, OpenBCI.
- **Chip de reconocimiento:** STM32F405 (clasificador LDA o red pequeña).
- **Estimulador:** FES de superficie (Compex, NeuroTrac) o tSCS.
- **Inconsciente artificial:** STM32F746 (reflejos, homeostasis, fallback).

## Estado del arte (referencias)

- Neuralink (2026): 21 pacientes controlan computadores con implantes .
- tSCS (SingHealth, 2026): estimulación espinal no invasiva mejora la marcha .
- Integración EEG-FES (Universidade Estadual Paulista, 2021): sistema de lazo cerrado con 77% de precisión .

## Documentación

- [`Bypass-neuronal.md`](./NeuralBypass.md): documento completo (principio, implementación, referencias).
- [`Bypass-neuronal.md`](./NeuralBypass.en.md): documento completo (principio, implementación, referencias "Ingles").
- [`docs/implementacion.md`](./docs/implementacion.md): dispositivos concretos, comunicación, alimentación.
- [`docs/entrenamiento.md`](./docs/entrenamiento.md): cómo se entrena el modelo con voluntarios sanos.
- [`docs/seguridad_vital.md`](./docs/seguridad_vital.md): protocolo de emergencia.

## Estado actual

- [x] Concepto definido.
- [x] Documentación completa (principio, implementación, referencias).
- [ ] Selección de hardware concreto (en curso).
- [ ] Integración de componentes (EEG + chip + FES) en prototipo de laboratorio.
- [ ] Pruebas en voluntarios sanos.
- [ ] Ensayos clínicos (en colaboración con instituciones médicas).

## Licencia

Copyright © 2026 Enrique Aguayo H. (Mackiber Labs). Uso no comercial permitido con atribución. Para uso comercial, contactar a eaguayo@migst.cl.

## Autor

**Enrique Aguayo H.** – Mackiber Labs  
Contacto: eaguayo@migst.cl  
ORCID: 0009-0004-4615-6825  
GitHub: @enriqueherbertag-lgtm
