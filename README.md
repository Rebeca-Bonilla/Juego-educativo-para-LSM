# Juego Educativo para LSM 🤟

Aplicación móvil gamificada para el aprendizaje de Lengua de Señas Mexicana (LSM), con reconocimiento de señas en tiempo real mediante inteligencia artificial.

> "Esta es una aplicación con fines de repaso y estudio, no reemplaza a un profesor especializado."

---

## ¿Qué es?

Un juego de aprendizaje por lecciones donde el usuario aprende señas del LSM a través de:
- Animaciones 2D de cada seña con tweening
- Validación en tiempo real usando la cámara del dispositivo
- Sistema de XP, rachas y recompensas para motivar el estudio diario

---

## Tecnologías

| Capa | Tecnología |
|---|---|
| Frontend (móvil) | React Native + Expo (TypeScript) |
| Animaciones | Rive |
| Backend | FastAPI (Python) |
| Modelo IA | scikit-learn (Random Forest) + Keras (LSTM) |
| Landmarks | MediaPipe (Hands + Face) |
| Control de versiones | Git + GitHub |

---

## Estructura del repositorio

```
Juego-educativo-para-LSM/
├── app/          → Proyecto React Native (Expo)
├── backend/      → API FastAPI + modelo entrenado
├── dataset/      → Scripts de captura y procesamiento
├── docs/         → Documentación del proyecto
└── README.md
```

---

## Cómo correr el proyecto

### Frontend
```bash
cd app
npm install
npx expo start
```
Escanea el QR con Expo Go en tu celular (iOS o Android).

### Backend
```bash
cd backend
pip install -r requirements.txt
uvicorn main:app --reload
```

---

## Dataset

El modelo fue entrenado con un dataset propio de 11 señas del LSM:

`hola`, `adiós`, `tú`, `yo`, `gracias`, `sí`, `no`, `por favor`, `de nada`, `mamá`, `papá`

Capturado con personas diestras y zurdas. Los landmarks se extraen con MediaPipe (Hands + Face) y se normalizan para invarianza de lateralidad.

Ver protocolo completo en [`docs/protocolo-dataset.md`](docs/protocolo-dataset.md)

---
---

## Estado del proyecto

🚧 En desarrollo activo — Proyecto Terminal
