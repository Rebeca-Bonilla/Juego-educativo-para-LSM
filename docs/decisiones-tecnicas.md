# Decisiones Técnicas — Juego Educativo LSM

Este documento registra las decisiones de diseño técnico del proyecto y la justificación de cada una. Es útil para la redacción de la tesis y para onboarding de nuevos colaboradores.

---

## 1. ¿Por qué landmarks y no clasificación de imágenes?

**Decisión:** Usar landmarks extraídos con MediaPipe en lugar de clasificar imágenes directamente.

**Justificación:**
- Los landmarks son invariantes a condiciones de iluminación, color de piel y ropa
- Requieren mucho menos datos de entrenamiento que un clasificador de imágenes
- Son más livianos computacionalmente para correr en tiempo real en un dispositivo móvil
- Permiten calcular distancias y relaciones geométricas entre puntos (ej. mano-mentón) que son semánticamente relevantes para las señas

**Alternativa descartada:** Clasificación de imágenes con CNN (ej. via Roboflow). Requiere datasets mucho más grandes y es sensible a condiciones visuales.

---

## 2. ¿Por qué MediaPipe Hands + Face y no Holistic?

**Decisión:** Usar `Hands + Face` en lugar de `Holistic`.

**Justificación:**
- Las 11 señas del dataset involucran principalmente manos y, en algunos casos, contacto con la cara (mamá, papá, etc.)
- Holistic incluye pose corporal completa (33 puntos) que no aporta información relevante para estas señas y añade complejidad innecesaria
- Hands + Face es más eficiente y suficiente para el alcance del proyecto

---

## 3. ¿Por qué dos modelos (baseline + temporal)?

**Decisión:** Entrenar un modelo baseline (Random Forest) y un modelo temporal (LSTM).

**Justificación:**
- El baseline permite validar rápidamente que los landmarks son discriminativos para las señas
- El modelo temporal captura la dinámica de las señas que involucran movimiento, lo cual es esencial para señas dinámicas
- Comparar ambos modelos constituye un experimento válido y valioso para la tesis
- Si el baseline tiene desempeño aceptable, simplifica el despliegue en producción

---

## 4. ¿Por qué React Native + Expo y no Flutter?

**Decisión:** Usar React Native con Expo.

**Justificación:**
- La desarrolladora principal tiene experiencia previa en TypeScript y Vue.js
- React Native usa TypeScript, reduciendo la curva de aprendizaje significativamente
- Expo simplifica el setup, la build y el testing en dispositivo real (sin necesidad de Mac para iOS en desarrollo)
- El ecosistema de paquetes de React Native cubre todas las necesidades del proyecto (cámara, HTTP, Rive)

**Alternativa descartada:** Flutter (Dart). Requeriría aprender un lenguaje nuevo desde cero, lo cual no es viable dado el alcance del proyecto.

---

## 5. ¿Por qué FastAPI y no Django o Flask?

**Decisión:** Usar FastAPI para el backend.

**Justificación:**
- FastAPI es asíncrono por defecto, lo que mejora el rendimiento para inferencia en tiempo real
- Genera documentación automática (Swagger) sin configuración adicional
- Es más moderno y liviano que Django para una API pequeña de 3-5 endpoints
- Tiene soporte nativo para Pydantic, facilitando la validación de los datos de entrada (landmarks)
- El curso de Python del proyecto incluye una sección de FastAPI

**Alternativas descartadas:**
- Django: demasiado pesado para una API simple, orientado a aplicaciones web completas
- Flask: más simple pero sin tipado, validación automática ni documentación integrada

---

## 6. ¿Por qué Rive para las animaciones?

**Decisión:** Usar Rive para animar el avatar 2D.

**Justificación:**
- Rive tiene un paquete oficial para React Native con soporte activo
- Permite hacer tweening entre poses de forma nativa sin código adicional
- El editor de Rive es gratuito para proyectos no comerciales
- Los assets de Rive son livianos y eficientes en tiempo de ejecución
- Compatible con el flujo de trabajo de Clip Studio Paint (exportar capas como PNG)

---

## 7. ¿Por qué normalización de lateralidad?

**Decisión:** Normalizar todas las señas a una representación de "mano dominante derecha virtual" antes del entrenamiento.

**Justificación:**
- En LSM, las señas pueden ejecutarse con cualquier mano dominante según la persona
- Sin normalización, el modelo vería dos patrones distintos (espejados) para la misma seña
- Con un dataset pequeño, el modelo no tiene suficientes datos para aprender invariancia por sí mismo
- La normalización es simple: `x_new = 1 - x_old` para personas zurdas
- Se preserva la metadata de handedness original para reproducibilidad

**Lo que NO se hizo:** Separar en clases distintas (gracias_derecha / gracias_izquierda), lo cual duplicaría clases y reduciría datos por clase.

---

## 8. ¿Por qué split por participante y no por muestra?

**Decisión:** Al dividir train/val/test, se separa por participante completo, no por muestras individuales.

**Justificación:**
- Si las muestras de un mismo participante aparecen en train y en test, el modelo puede memorizar patrones específicos de esa persona
- Separar por participante garantiza que el modelo sea evaluado con personas que nunca vio durante el entrenamiento
- Esto hace la evaluación más honesta y representativa del desempeño en producción
