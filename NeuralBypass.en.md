# Non-invasive neuronal bypass for spinal cord injuries

**Authors:** Enrique Aguayo H., DeepSeek (technical assistance)

**License:** Copyright © 2026 Enrique Aguayo H. (Mackiber Labs). Non-commercial use permitted with attribution. For commercial use, contact eaguayo@migst.cl.

**Date:** April 2026

## Abstract

A non‑invasive neuronal bypass system is proposed. It reads the patient's cerebral signal (movement intention), recognizes it (even if weak or noisy), and translates it into functional electrical stimulation (FES) or transcutaneous spinal cord stimulation (tSCS). Unlike exoskeletons, the patient uses their own muscles, preserves sensory feedback, and engages their learning circuits (dopamine). The system includes an **artificial unconscious** (based on an STM32 microcontroller) that executes reflexes and autonomous movements if the signal fails.

**Key principle:** The brain does not need to learn to generate perfect signals. The system recognizes the intention, and the brain recalibrates itself through feedback (neuroplasticity).

## 1. The problem

A patient with a spinal cord injury has a healthy brain and healthy muscles. The problem is the cable (the spinal cord) that connects them. Current approaches use exoskeletons (external robots) or implanted stimulators that activate muscles without reading the brain's intention.

**Limitations of current approaches:**

- The patient does not feel the movement (no proprioceptive feedback).
- The brain does not learn (no recalibration).
- Rehabilitation is passive.
- Exoskeletons are expensive, heavy, and require constant maintenance.

## 2. External training of the system

The recognition chip does not "guess" signals. **It is trained with real data before being implanted.** The process is as follows:

1. **Data collection from healthy volunteers** (5–10 people). Each volunteer performs specific movements (knee flexion, elbow extension, etc.) while recording:
   - Brain signal (EEG, non‑invasive).
   - Actual muscle activation (surface EMG) or kinematic movement measurement (inertial sensors).

2. **Centralized (offline) training:** The data is sent (anonymized) to a powerful server (or the cloud). An algorithm (LDA classifier, small neural network, SVM) learns to associate the (noisy) brain signal with the movement intention (e.g., "extend knee"). The result is a **recognition model** (not a signal auto‑completion).

3. **Installation on the chip:** The trained model is compiled into a lightweight format (quantized weight matrix, decision tree) and installed on the subcutaneous chip (as a firmware update). The chip does not learn in real time. It only executes the model.

4. **Personal calibration (optional):** When the patient uses the system for the first time, a short calibration (5–10 minutes) is performed. The chip adjusts the model's thresholds to the patient's particular brain signal. This is not a full re‑training. It is a fine‑tuning of gains.

**The system is deterministic, fast, and low‑power. The heavy part (training) happens outside the body.**

## 3. State of the art: the two halves already exist

### 3.1 Reading intention (BCI)

Neuralink has shown that the brain signal can be read, decoded, and translated into digital commands in real time:

- **21 patients** with paralysis (spinal cord injury, ALS) are participating in clinical trials of the Telepathy implant.
- They have controlled computers, smartphones, and robotic arms with thought alone.
- Typing speeds of up to **40 words per minute** using a virtual keyboard.
- Information transfer rates exceeding **10 bits per second** (equivalent to mouse control in non‑disabled individuals) .
- The **GB‑PRIME study** at University College London Hospitals enrolled **7 patients** in the UK to evaluate the safety and functionality of the Neuralink N1 implant .

**Conclusion:** Reading intention is technically possible. The challenge is to do it without surgery (non‑invasive).

### 3.2 Spinal cord (tSCS) and muscle (FES) stimulation

Transcutaneous spinal cord stimulation (tSCS) is a **non‑invasive** technique that has already been shown to restore movement:

- **Zorkot et al. (2026)** developed automated algorithms for personalized electrode placement in tSCS, improving muscle selectivity and facilitating clinical adoption .
- The **AIM RECOVER** trial (SingHealth, 2026) is developing a closed‑loop tSCS system with AI that adjusts stimulation parameters in real time based on EMG and kinematic sensors .
- The **Journal of NeuroEngineering and Rehabilitation (2025)** demonstrated a non‑invasive brain‑spine interface (EEG + tSCS) that decodes leg movement intention with an LDA classifier, achieving an average AUC of **0.83 ± 0.06** .
- The **ReverseStroke** project (EIC, 2026) aims to develop a digital brain‑spine bridge to restore hand and arm movement in stroke patients .

**Conclusion:** Non‑invasive spinal cord stimulation works and is undergoing active clinical development. The combination with surface FES is straightforward.

### 3.3 Integration EEG–FES (closed‑loop systems)

Laboratory prototypes already integrate intention reading with muscle stimulation:

- **Universidade Estadual Paulista (2021)** demonstrated a closed‑loop EEG‑FES control system in a patient with paraplegia. The system achieved **77% accuracy** in classifying leg movement intention and automatically turned off stimulation when muscle fatigue was detected .
- **Springer (2026)** published a study on a human‑machine interface combining multichannel FES with inertial sensors for lower‑limb rehabilitation in SCI patients. Patients performed cycling and walking with higher speed and cadence using the system .
- **arXiv (2026)** presented a portable neuroprosthesis that decodes residual EMG signals (32 channels) to control FES in the foot of SCI patients. Two patients increased their foot flexion range by **33.6% and 40%** of the healthy functional range .

**Conclusion:** Integration of intention reading and muscle stimulation already works in the laboratory. The challenge is miniaturization and portability.

## 4. The solution: non‑invasive neuronal bypass

The system consists of five components, all **external or subcutaneous**, without brain surgery.

### 4.1 Brain signal reader (non‑invasive EEG)

- **Technology:** Surface EEG (cap or headband with dry electrodes).
- **Real devices (2026):** Emotiv EPOC X (14 channels), Naox LINK (in‑ear), OpenBCI.
- **Function:** Captures movement intention (weak, noisy, but with an identifiable pattern).

### 4.2 Intention recognition chip

- **Technology:** Simple classifier (e.g., LDA, decision tree) or small neural network.
- **Hardware:** STM32, FPGA, or low‑power DSP.
- **Function:** Does NOT "autocomplete" the signal. **Recognizes whether there is an intention** (e.g., "wants to extend the knee") based on the model previously trained with healthy volunteers.
- **External training:** See section 2.

### 4.3 Muscle stimulator (FES/tSCS)

- **Technology:** Functional electrical stimulation (FES) with surface electrodes, or transcutaneous spinal cord stimulation (tSCS).
- **Real devices (2026):** Compex, NeuroTrac, or research systems like the tSCS described in .
- **Function:** Activates muscles (or the spinal cord) with a predefined stimulation pattern according to the recognized intention. Not personalized. A standard pattern for that movement.

### 4.4 Artificial unconscious (reflexes and safety)

- **Technology:** STM32 microcontroller.
- **Functions:**
  - Emergency reflexes (e.g., withdraw leg when encountering an obstacle).
  - System homeostasis (temperature, energy).
  - Fallback: if the brain signal fails or is uncertain, execute pre‑programmed autonomous movements.
  - **Vital safety (highest priority):** Continuously monitors the patient's vital signs (via the optional neural connection or external sensors) and, in case of any anomaly, stops all movement, cuts stimulation, and transmits an emergency alert.

### 4.5 Communication and power

- **Communication:** Bluetooth Low Energy (BLE) between the EEG cap (external) and the subcutaneous chip (implant). The chip and stimulator are connected by a cable (inside the same implant).
- **Implant power:** Graphene ultracapacitors + ambient energy harvesting (body heat, movement). This autonomous energy technology (developed within the Mackiber Labs ecosystem) eliminates the need for chemical batteries or external recharging.
- **EEG cap power:** Inductively rechargeable battery (like wireless headphones). The patient places it on a base at night.

## 5. Conceptual diagram

[Patient's brain]
↓
[EEG cap (external)] → (Bluetooth LE) → [Subcutaneous chip (recognition)] → (cable inside the implant) → [Stimulator (FES/tSCS)]
↓
[Patient's muscles / spinal cord]
↓
[Artificial unconscious (STM32)]
(reflexes, homeostasis, fallback, vital safety)


**All components are external or subcutaneous. No brain surgery. No batteries to recharge (except the cap, which is inductively charged).**


## 6. Advantages over exoskeletons

| Aspect | Exoskeleton | Neuronal bypass (this project) |
|:---|:---|:---|
| Movement | External machine | **Own muscles** |
| Feedback | Limited (visual) | **Natural (proprioceptive)** |
| Brain learning | Slow | **Fast (dopamine)** |
| Weight | 10‑20 kg | **< 1 kg** (implant + cap) |
| Cost | Tens of thousands USD | **Estimated < 10,000 USD** |
| Invasiveness | Non‑invasive (external) | **Minimal** (subcutaneous, no brain surgery) |
| Autonomy | Hours (batteries) | **Indefinite** (energy harvesting + induction) |
| Rehabilitation | Passive | **Active** (brain recalibrates itself) |

## 7. Implementation with real devices (2026)

### EEG reader (non‑invasive)

| Device | Type | Channels | Certification | Use in bypass |
|:---|:---|:---|:---|:---|
| **Emotiv EPOC X** | Headband (dry electrodes) | 14 | Commercial | Research, high resolution |
| **Naox LINK** | In‑ear | 1 | FDA 510(k) | Long‑duration, home use |
| **OpenBCI** | DIY / open source | 8‑16 | Commercial | Low‑cost prototyping |

**Recommendation for prototype:** Emotiv EPOC X (good cost/channel ratio) or OpenBCI (open, flexible).

### Intention recognition chip

- **Hardware:** STM32F405, FPGA (Xilinx Zynq), or DSP (Texas Instruments).
- **Model:** LDA classifier or small neural network (2‑3 layers). The model is trained **offline** (with data from healthy volunteers). The chip only performs inference.
- **Power consumption:** < 100 mW. Can be powered by ultracapacitors + ambient energy harvesting.

### Stimulator (FES/tSCS)

| Type | Device | Status | Use in bypass |
|:---|:---|:---|:---|
| **Surface FES** | Compex, NeuroTrac | Commercial | Controlled by the chip (TTL signal) |
| **tSCS** | Research systems (SingHealth, 2026) | In development | Non‑invasive, applied over lumbar skin |
| **Liquid electrodes** | Yonsei University | Prototype | Long‑duration use, reduces irritation |

**Recommendation for prototype:** Surface FES with self‑adhesive electrodes (standard). For advanced versions, lumbar tSCS.

### Artificial unconscious

- **Hardware:** STM32F746.
- **Pre‑programmed reflexes:** Decision tables (expert system).
- **Communication:** With the recognition chip via internal bus (SPI, I2C) inside the same implant.

## 8. Communication and power

### Wireless communication (cap → chip)

- **Technology:** Bluetooth Low Energy (BLE).
- **Range:** 1‑2 meters (sufficient for the distance from the scalp to the chest).
- **Security:** AES‑128 encryption.
- **Suggested modules:** Nordic nRF52, Texas Instruments CC2640.

### Internal communication (chip → stimulator)

- **Technology:** Cable (SPI or I2C bus) inside the same implant.
- **No wireless needed.** The chip and stimulator are on the same board (or very close).

### Implant power (chip + stimulator)

- **Technology:** Graphene ultracapacitors + ambient energy harvesting (body heat, movement, ambient RF). This autonomous energy technology (developed within the Mackiber Labs ecosystem) eliminates the need for chemical batteries and external recharging.
- **Autonomy:** The implant can operate indefinitely as long as the patient is alive and moving.

### EEG cap power

- **Inductively rechargeable battery** (like wireless headphones). The patient places it on a base at night.
- **Optional:** Solar harvesting (flexible panels on the cap).

## 9. Limitations and challenges

| Challenge | Proposed solution | Status |
|:---|:---|:---|
| **Inter‑patient variability** | Recognition model trained with several healthy volunteers. Short calibration (5 minutes) adjusts thresholds per patient. | In design. |
| **Latency** | LDA classifier is fast (< 10 ms on an STM32). Total time (signal → FES) should be < 100 ms for fluid movements. | In design (estimated). |
| **Skin irritation from electrodes** | Use hydrogel electrodes (disposable) or liquid electrodes (Yonsei University). | Under research. |
| **Active rehabilitation** | The system is an assistant. The patient must train their brain to emit clearer signals. Over time, intention recognition may need less correction. | Hypothesis to be validated. |
| **Medical regulation (FDA, CE)** | The system uses already certified components (Emotiv, commercial FES). The subcutaneous implant would require regulatory approval. | Pending (prototype phase). |

## 10. Vital safety (highest priority)

The artificial unconscious continuously monitors the patient's vital signs (via the optional neural connection or external sensors). If it detects an anomaly, it **does not wait for instructions from the main brain**. It acts immediately.

### Trigger conditions

- Heart rate outside safe range.
- Blood oxygen saturation (SpO2) below threshold.
- Abnormal body temperature.
- Loss of Bluetooth connection for more than `X` seconds.
- Sudden immobility detected (accelerometer in the cap or implant).

### Immediate actions

1. **Stop all patient movement** (absolute priority).
2. **Cut stimulation** (if active).
3. **Transmit emergency alert** via LoRaWAN, satellite, or mobile network (if the patient has a home gateway).
4. **Send position and vital signs** to emergency services (or pre‑defined contacts).
5. **Await instructions** (or execute basic first‑aid routines if the system has the capability to do so without risk).

**This rule has priority over any other system instruction.**

## 11. Conclusion

The non‑invasive neuronal bypass is a promising alternative to exoskeletons and invasive implants. It is cheaper, lighter, and allows the patient to use their own muscles, preserving sensory feedback and brain learning.

**The two halves of the system already exist:** Neuralink (intention reading) and tSCS/FES (muscle/spinal stimulation). The project proposes integrating them into a single closed‑loop, non‑invasive, autonomous system, with an artificial unconscious (STM32) that manages reflexes, homeostasis, and vital safety.

**Next steps:**

1. **Laboratory prototype:** Integrate a commercial EEG (Emotiv EPOC X) with an STM32 and an FES stimulator (Compex). Test on healthy volunteers.
2. **Validation of the neuroplasticity hypothesis:** Demonstrate that the patient's brain recalibrates with system use.
3. **Subcutaneous prototype:** Miniaturize the recognition chip and stimulator into an implant (no chemical batteries, with ultracapacitors + ambient energy harvesting).
4. **Clinical trials:** In collaboration with medical institutions, test on patients with spinal cord injuries.

## 12. References

1. **Neuralink GB‑PRIME trial (UCLH, 2026).** Seven patients with the N1 implant control computers and phones with thought .
2. **Neuralink Telepathy results (2026).** 21 patients, typing speed of 40 words per minute, control of robotic arms .
3. **Zorkot et al. (2026).** Automated algorithms for personalized electrode placement in non‑invasive tSCS .
4. **AIM RECOVER trial (SingHealth, 2026).** Closed‑loop tSCS system with AI for SCI rehabilitation .
5. **Journal of NeuroEngineering and Rehabilitation (2025).** Non‑invasive BSI (EEG + tSCS) with LDA classifier .
6. **Universidade Estadual Paulista (2021).** Closed‑loop EEG‑FES system in a patient with paraplegia .
7. **Springer (2026).** HMI with multichannel FES + inertial sensors for lower‑limb rehabilitation .
8. **arXiv (2026).** Portable neuroprosthesis (EMG + FES) for foot movement restoration .
9. **Mackiber Labs – autonomous energy technology (ultracapacitors + ambient harvesting).** Applied research ecosystem.

---

**Author:** Enrique Aguayo H. (Mackiber Labs) – Contact: eaguayo@migst.cl – ORCID: 0009-0004-4615-6825

**License:** Copyright © 2026 Enrique Aguayo H. Non‑commercial use permitted with attribution. For commercial use, contact the author.




