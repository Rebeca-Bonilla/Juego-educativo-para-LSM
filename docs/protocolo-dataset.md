# Protocolo de Captura del Dataset — LSM

## Descripción general

Dataset propio de señas en Lengua de Señas Mexicana (LSM) para entrenar un modelo de reconocimiento basado en landmarks extraídos con MediaPipe (Hands + Face).

### Señas incluidas (11)

| Seña | Tipo |
|---|---|
| hola | Dinámica |
| adiós | Dinámica |
| tú | Estática |
| yo | Estática |
| gracias | Semi-estática |
| sí | Semi-estática |
| no | Semi-estática |
| por favor | Semi-estática |
| de nada | Dinámica |
| mamá | Dinámica |
| papá | Dinámica |

---

## Participantes

- Mínimo recomendado: 5 personas
- Objetivo: 15 personas
- Se incluyen personas diestras y zurdas
- Metadata registrada en `metadata/participants.csv`

### Campos registrados por participante
```
ID, Nombre completo, Mano dominante, Edad, Género
```

---

## Configuración de grabación

### Equipo
- Cámara de celular o webcam
- No se requiere equipo especializado

### Condiciones físicas
- **Posición de la cámara:** altura del pecho/hombros del participante
- **Distancia:** suficiente para ver manos completas Y cara completa en cuadro
- **Iluminación:** buena luz frontal, sin contraluz
- **Fondo:** preferentemente liso, cualquier color que contraste con la piel
- **Ropa:** que contraste con el tono de piel (evitar colores piel/beige)

### Duración por video
- Señas estáticas: ~2 segundos
- Señas semi-estáticas: ~2-3 segundos
- Señas dinámicas: ~2-3 segundos (movimiento completo)

---

## Procedimiento por sesión

Duración estimada por participante: 30-40 minutos

1. Explicar las 11 señas al participante antes de grabar
2. Practicar cada seña 2-3 veces sin grabar
3. Grabar en el siguiente orden:
   - hola × 10 videos
   - adiós × 10 videos
   - tú × 10 videos
   - yo × 10 videos
   - gracias × 10 videos
   - sí × 10 videos
   - no × 10 videos
   - por favor × 10 videos
   - de nada × 10 videos
   - mamá × 10 videos
   - papá × 10 videos
4. Registrar datos del participante en `participants.csv`

**Total por participante: 110 videos**

---

## Nomenclatura de archivos

```
p[id]_[d/z]_[número].mp4

Ejemplos:
p01_d_001.mp4  → persona 1, diestra, muestra 1
p04_z_003.mp4  → persona 4, zurda, muestra 3
```

---

## Criterios de aceptación de videos

### ✅ Se acepta si:
- Se ven las dos manos completas en cuadro
- Se ve la cara completa en cuadro
- Buena iluminación sin sombras fuertes
- La seña se ejecuta completa y con claridad
- No hay movimientos extraños al inicio o final del clip

### ❌ Se rechaza si:
- Una o ambas manos están cortadas fuera del cuadro
- La cara no es visible (necesaria para señas que involucran contacto facial)
- Iluminación insuficiente
- La seña está incompleta o es incorrecta
- Hay obstrucciones (objetos, personas) en cuadro

---

## Sistema de tracking

El seguimiento del dataset se lleva en `metadata/participants.csv` con código de colores:

| Color | Significado |
|---|---|
| 🟡 Amarillo | Grabado |
| 🟠 Naranja | Subido a Drive |
| 🔴 Rojo | Rechazado (repetir) |
| 🟢 Verde | Procesado (landmarks extraídos) |

---

## Estructura de carpetas en Drive

```
LSM_dataset/
├── raw/
│   ├── hola/
│   ├── adios/
│   ├── tu/
│   ├── yo/
│   ├── gracias/
│   ├── si/
│   ├── no/
│   ├── por_favor/
│   ├── de_nada/
│   ├── mama/
│   └── papa/
├── landmarks/
│   ├── baseline/
│   └── temporal/
├── splits/
│   ├── baseline/
│   └── temporal/
└── metadata/
    └── participants.csv
```

---

## Normalización de lateralidad

Para garantizar que el modelo sea invariante a si la persona es diestra o zurda, se aplica la siguiente normalización durante el procesamiento:

- Si `handedness = right` → landmarks se dejan igual
- Si `handedness = left` → se refleja horizontalmente: `x_new = 1 - x_old`

Esto transforma todas las señas a una representación de "mano dominante derecha virtual", evitando que el modelo aprenda dos patrones distintos para la misma seña.

---

## Formato del dataset procesado

### Baseline (Random Forest)
Un CSV por muestra con una sola fila (3 frames aplanados):
```
f1_lm0_x, f1_lm0_y, f1_lm0_z, ..., f3_lm20_z,
dist_mano_menton, dist_mano_nariz,
label, participant, handedness
```

### Temporal (LSTM)
Un CSV por muestra con una fila por frame:
```
frame, lm0_x, lm0_y, lm0_z, ..., lm20_z,
dist_mano_menton, dist_mano_nariz, label
```

### Split de datos
```
Train: 70%
Validación: 15%
Test: 15%

Split por participante (no por muestra)
para evitar data leakage
```
