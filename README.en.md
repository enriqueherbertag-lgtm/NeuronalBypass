[![License](https://img.shields.io/badge/License-Copyright%20%28c%29%202026%20Enrique%20Aguayo-red)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Concept%20under%20design-yellow)](https://github.com/enriqueherbertag-lgtm/NeuralBypass)
[![AI Assistance](https://img.shields.io/badge/AI%20Assistance-DeepSeek-brightgreen)](https://deepseek.com)
[![DOI](https://img.shields.io/badge/DOI-pending-blue)]()

# Non-invasive neuronal bypass for spinal cord injuries

**Intention recognition (EEG) + muscle stimulation (FES/tSCS). No brain surgery. No external batteries.**

## The problem

The spinal cord is damaged, but the brain and muscles are healthy. The cable is broken.

## The solution

Read the patient's intention (even if the signal is weak), recognize the pattern, and activate the corresponding muscles (FES) or the spinal cord (tSCS). The brain recalibrates itself through feedback.

**Key principles:**
- **Non-invasive:** Surface EEG (cap), transcutaneous stimulation.
- **External training:** The model learns with healthy volunteers. The chip only executes inference.
- **Autonomous:** Energy harvested from the body (ultracapacitors + body heat, movement).
- **Safe:** Emergency protocol (vital signs, LoRaWAN/satellite alerts).

## Components (real devices)

- **EEG reader:** Emotiv EPOC X, Naox LINK, OpenBCI.
- **Intention recognition chip:** STM32F405 (LDA classifier or small neural network).
- **Stimulator:** Surface FES (Compex, NeuroTrac) or tSCS.
- **Artificial unconscious:** STM32F746 (reflexes, homeostasis, fallback).

## Documentation

- [`Bypass-neuronal.md`](./Bypass-neuronal.md): complete document (principles, implementation, references).
- [`docs/implementation.md`](./docs/implementation.md): concrete devices, communication, power.
- [`docs/training.md`](./docs/training.md): how the model is trained offline with healthy volunteers.
- [`docs/vital_security.md`](./docs/vital_security.md): emergency and assistance protocol.

## Current status

- [x] Defined concept.
- [x] Complete documentation (principles, implementation, references).
- [ ] Concrete hardware selection (in progress).
- [ ] Component integration (EEG + chip + FES) in a laboratory prototype.
- [ ] Tests with healthy volunteers.
- [ ] Clinical trials (in collaboration with medical institutions).

## License

Copyright © 2026 Enrique Aguayo H. (Mackiber Labs). Non-commercial use permitted with attribution. For commercial use, contact eaguayo@migst.cl.

## Author

**Enrique Aguayo H.** – Mackiber Labs  
Contact: eaguayo@migst.cl  
ORCID: 0009-0004-4615-6825  
GitHub: @enriqueherbertag-lgtm
