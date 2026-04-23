# Entrenamiento externo del sistema

El chip de reconocimiento no "adivina" señales. **Se entrena con datos reales antes de ser implantado.**

## Proceso

1. **Recolección de datos de voluntarios sanos** (5-10 personas). Cada voluntario realiza movimientos específicos (flexión de rodilla, extensión de codo, etc.) mientras se registra:
   - Señal cerebral (EEG, no invasivo).
   - Activación muscular real (EMG de superficie) o medición cinemática del movimiento (sensores inerciales).

2. **Entrenamiento centralizado (offline):** Los datos se envían (anónimos) a un servidor potente (o a la nube). Un algoritmo (clasificador LDA, red neuronal pequeña, SVM) aprende a asociar la señal cerebral (ruidosa) con la intención de movimiento (ej. "extender rodilla"). El resultado es un **modelo de reconocimiento** (no un autocompletado de señales).

3. **Instalación en el chip:** El modelo entrenado se compila a un formato ligero (matriz de pesos cuantizada, árbol de decisión) y se instala en el chip subcutáneo (como una actualización de firmware). El chip no aprende en tiempo real. Solo ejecuta el modelo.

4. **Calibración personal (opcional):** Cuando el paciente usa el sistema por primera vez, realiza una calibración corta (5-10 minutos). El chip ajusta los umbrales del modelo a su señal cerebral particular. No es re-entrenamiento completo. Es un ajuste fino de ganancias.

**El sistema es determinista, rápido y de bajo consumo. La parte pesada (el entrenamiento) ocurre fuera del cuerpo.**

## Ventajas

- El chip no necesita GPU ni gran capacidad de cómputo.
- El modelo puede mejorarse con nuevos datos (reentrenando en el servidor y actualizando el firmware).
- No hay riesgo de que el chip "aprenda algo malo" en tiempo real.
