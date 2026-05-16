# Diseño de la Aplicación — Juego Educativo LSM

---

## Concepto general

Aplicación móvil gamificada para el aprendizaje de Lengua de Señas Mexicana (LSM), inspirada en el modelo de aprendizaje por lecciones progresivas de apps como Duolingo.

El usuario aprende señas a través de lecciones estructuradas, con animaciones del avatar que muestran cómo hacer cada seña, y validación en tiempo real usando la cámara del dispositivo.

> La app tiene fines de repaso y estudio. No reemplaza a un profesor especializado.

---

## Pantallas principales

### 1. Mapa de lecciones (pantalla principal)
- Bloques temáticos organizados en un mapa de progresión
- Cada bloque contiene un conjunto de señas relacionadas
- Las estrellas indican el nivel de dominio alcanzado en cada bloque

### 2. Detalle de bloque
- Lista de logros del bloque
- Comodines disponibles
- Botón para iniciar lección

### 3. Flujo de lección
Cada lección consta de dos fases:

**Fase de presentación:**
- Se muestra la animación (tweening) del avatar haciendo la seña
- El usuario puede repetir la animación o continuar

**Fase de validación:**
- Se solicita al usuario que realice la seña indicada frente a la cámara
- El modelo de IA detecta y valida la seña en tiempo real
- Si la detecta correctamente → feedback positivo → siguiente seña
- Si no la detecta → notificación → opción de reintentar
- Botones disponibles: Omitir / Repetir animación / Intentar de nuevo

### 4. Dojo / Práctica libre
- Resumen de señas vistas, practicadas y aprendidas
- Botón para practicar señas específicas
- Acceso al Traductor (requiere cámara)

### 5. Traductor
- Vista de cámara en tiempo real
- El modelo detecta señas y muestra la traducción

### 6. Racha
- Calendario mensual con días de práctica marcados
- Racha de días consecutivos

### 7. Perfil de usuario
- Foto de perfil y nombre de usuario
- Modo oscuro, inventario, lista de amigos

### 8. Tienda
- Compras con monedas del juego (sin dinero real)
- Categorías: Personajes, Comodines, Protector de racha

### 9. Configuración
- Datos de cuenta y usuario
- Configuración de idioma de la interfaz

---

## Sistema de gamificación

| Elemento | Descripción |
|---|---|
| XP | Puntos ganados por completar lecciones correctamente |
| Racha | Días consecutivos de práctica |
| Monedas | Moneda del juego para la tienda |
| Estrellas | Nivel de dominio por bloque (1-3 estrellas) |
| Logros | Metas desbloqueables por bloque |
| Comodines | Ayudas durante las lecciones |

---

## Personajes

4 personajes jugables animados en Rive para el entregable:

| Personaje | Estado |
|---|---|
| 🐱 Gato de uniforme | Animado en Rive |
| 🦁 León maestra (chaleco LSM) | Animado en Rive |
| 🦢 Cisne elegante | Animado en Rive |
| 🐂 Toro con saco | Animado en Rive |

Personajes adicionales disponibles como imagen estática en la tienda. Personaje especial de evento (🦎 Dinosaurio verde) disponible en lanzamientos o eventos especiales.

---

## Flujo de trabajo de animaciones

```
1. Diseño del personaje en Clip Studio Paint
   └── Cada parte del cuerpo en capa separada
   └── Exportar cada capa como PNG transparente

2. Importar PNGs a Rive
   └── Hacer rigging (unir partes con huesos)
   └── Crear animación por seña

3. Exportar archivo .riv
   └── Integrar en React Native con paquete rive-react-native
   └── Activar animación por nombre desde el código
```

---

## Alcance del entregable (v1.0)

### ✅ Incluido
- Modelo de IA funcionando (Random Forest + LSTM)
- Backend FastAPI
- 1 personaje animado con las 11 señas
- Flujo completo de lección (animación → práctica → validación con cámara)
- Pantallas principales del mockup
- Sistema básico de XP y racha

### ⏳ Post-entrega (v2.0)
- 3 personajes adicionales animados
- Tienda funcional
- Lista de amigos
- Comodines
- Dinosaurio de evento especial
- Soporte multiidioma (Náhuatl, Maya, etc.)
