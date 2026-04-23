# Seguridad vital: protocolo de emergencia y asistencia (prioridad máxima)

El inconsciente artificial (STM32) monitoriza continuamente los signos vitales del paciente y la calidad de la señal cerebral. **No espera instrucciones externas.** Actúa según un árbol de decisiones predefinido, con prioridad absoluta sobre cualquier otra función del sistema.

## 1. Jerarquía de prioridades

| Prioridad | Situación | Acción |
|:---|:---|:---|
| **1** | Riesgo vital inminente (paro cardíaco, pérdida de consciencia sin respuesta). | Detener todo. Alertar a emergencias. No mover al paciente. |
| **2** | Sospecha de hipoglucemia (marcadores: sudor, temblor, confusión, señal cerebral errática). | Sugerir al paciente (o cuidador) tomar azúcar/dulce (si está consciente). Si no responde, sube a prioridad 1. |
| **3** | Señal cerebral confusa o pérdida de conexión Bluetooth > 10 segundos. | Revisar estado del paciente (signos vitales). Si está bien, reintentar conexión. Si no, pasar a prioridad 2 o 1. |
| **4** | Medicación programada (o necesidad detectada por sensor). | Recordar al paciente (o cuidador) la dosis y el horario. No administrar automáticamente. |
| **5** | Fatiga muscular, sed, calor excesivo (sensores de temperatura, EMG). | Sugerir descanso o hidratación. Reducir o pausar la estimulación. |
| **6** | Caída detectada (acelerómetro). | Activar protocolo de caída: preguntar si está bien, pausar movimientos, alertar a familiar si no responde. |


## 2. Árbol de decisiones (simplificado)


1. Leer signos vitales y calidad de señal.

2. ¿Paciente consciente? (respuesta a estímulo sonoro o pregunta simple)
   - **No** → Prioridad 1 (emergencia). Ir a paso 6.
   - **Sí** → Continuar.

3. ¿Señal cerebral clara y estable?
   - **No** → Verificar hipoglucemia (marcadores: sudor, temblor, confusión)
        - **Sí** → Sugerir tomar azúcar/dulce.
        - Después de 2 minutos, reevaluar. Si no mejora → Prioridad 1.
   - **Sí** → Continuar.

4. ¿Medicación programada (o necesidad detectada)?
   - **Sí** → Recordar al paciente o cuidador (dosis, horario).
   - **No** → Continuar.

5. ¿Signos de fatiga, deshidratación, calor?
   - **Sí** → Sugerir descanso o hidratación. Reducir estimulación.
   - **No** → Continuar (todo normal).

6. **Alerta de emergencia activada (Prioridad 1):**
   - Detener toda estimulación.
   - Transmitir alerta por LoRaWAN, satélite, o red móvil (posición, signos vitales, identidad).
   - Llamar a ambulancia (o servicio contratado) si el sistema tiene esa capacidad.
   - Notificar a familiar (por mensaje, llamada, o alerta en app).
   - Esperar instrucciones de emergencias.

7. **Si no hay respuesta después de 60 segundos** (paciente no contesta, familiar no responde, emergencias no confirma):
   - Registrar el evento.
   - Continuar intentando notificar cada 5 minutos.
   - No reiniciar la estimulación hasta recibir confirmación de que el paciente está seguro.


## 3. Medicamentos y asistencia (sin automatismo)

El sistema **no administra medicamentos por sí mismo**. Solo **sugiere** al paciente (o al cuidador) la acción:

- *"Su glucosa está baja. Tome un dulce o azúcar."*
- *"Es hora de su medicación: [nombre], [dosis]."*
- *"Parece fatigado. Descanse 15 minutos."*
- *"Tiene temperatura elevada. Beba agua."*


**Si el paciente no responde o no puede hacerlo por sí mismo, el sistema eleva la prioridad y alerta a familiar o emergencias.**


## 4. Registro de eventos

El sistema guarda un log local (en el implante o en el gorro) de:

- Fecha, hora, tipo de evento (alerta, sugerencia, cambio de prioridad).
- Signos vitales en el momento.
- Acción tomada por el sistema.
- Respuesta del paciente (si la hubo).

Este registro puede ser consultado por el médico o familiar para ajustar el protocolo.

## 5. Personalización del protocolo

El árbol de decisiones y los umbrales (frecuencia cardíaca, temperatura, etc.) son **configurables** según las indicaciones del médico y las preferencias del paciente. Por ejemplo:

- Rango de glucosa seguro (70-140 mg/dL).
- Medicamentos, dosis y horarios.
- Contactos de emergencia (familiar, ambulancia, servicio contratado).
- Tiempo de espera antes de escalar a prioridad 1.

**El sistema no adivina. Sigue reglas predefinidas.**

## 6. Seguridad legal

- El sistema **no es un dispositivo de diagnóstico médico**. Es una asistencia.
- La responsabilidad final sobre la administración de medicamentos o la decisión de llamar a emergencias recae en el paciente o en su cuidador.
- El sistema solo **sugiere** y **alerta**. No actúa sin confirmación humana (excepto en la detención de la estimulación, que es automática por seguridad).

