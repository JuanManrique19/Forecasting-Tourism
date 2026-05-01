# 📈 Time Series Forecasting — Comparación de Modelos

Análisis y predicción de una serie temporal diaria con estacionalidad semanal y anual marcada. El objetivo fue identificar el modelo más adecuado para este tipo de datos comparando múltiples enfoques de forecasting.

---

## 🔍 Contexto del problema

El dataset contiene una variable `y` medida diariamente a lo largo de 3 años. Tras el análisis exploratorio, se identificaron los siguientes patrones:

- **Estacionalidad anual**: pico en verano (junio–julio), valle en invierno
- **Estacionalidad semanal**: valores más altos los fines de semana
- **Tendencia creciente** a largo plazo
- **Sin correlación clara** con festivos oficiales de España

El contexto probable del dataset es el **sector turístico**, dado que la estacionalidad coincide con vacaciones y comportamiento de fin de semana.

---

## 🧪 Hipótesis exploradas

| Hipótesis | Resultado |
|---|---|
| Estacionalidad anual | ✅ Confirmada (correlación moderada con año anterior) |
| Influencia de festivos | ❌ No confirmada visualmente |
| Estacionalidad semanal (fin de semana) | ✅ Confirmada (patrón ascendente lunes → domingo) |
| Dataset relacionado al turismo | ✅ Hipótesis más coherente con los patrones |

---

## 🤖 Modelos evaluados

### 1. Alisado Simple (Simple Exponential Smoothing)
Modelo de referencia. Asigna pesos decrecientes a observaciones pasadas.

**Limitación:** Produce una línea futura constante. No captura tendencia ni estacionalidad → **descartado**.

### 2. Alisado Triple — Holt-Winters
Extiende el alisado incorporando componente de tendencia y estacionalidad.

- **Modelo aditivo** (`seasonal="add"`): estacionalidad de amplitud constante
- **Modelo multiplicativo** (`seasonal="mul"`): estacionalidad proporcional al nivel

Ambos se configuraron con `seasonal_periods=7` (ciclo semanal). El modelo multiplicativo obtuvo un AICc ligeramente inferior → **mejor ajuste dentro de esta familia**.

**Limitación:** Solo puede modelar una estacionalidad a la vez (semanal o anual, no ambas simultáneamente).

### 3. Prophet (Meta/Facebook)
Modelo aditivo diseñado para series temporales con múltiples estacionalidades, festivos y cambios de tendencia.

**Ventajas clave para este dataset:**
- Modela estacionalidad semanal **y** anual de forma simultánea
- Robusto frente a valores atípicos
- Detecta automáticamente *changepoints* en la tendencia

---

## 📊 Resultados — Validación Cruzada (Prophet)

| Parámetro | Valor |
|---|---|
| Periodo inicial de entrenamiento | 730 días |
| Periodo entre evaluaciones | 180 días |
| Horizonte de predicción | 365 días |
| **MAPE promedio** | **~25%** |

El MAPE se mantiene estable a lo largo del horizonte, lo que indica que el modelo **no pierde precisión** al predecir más lejos en el tiempo.

---

## 🏆 Conclusión

**Prophet es el modelo ganador** para este tipo de serie temporal.

Captura simultáneamente la tendencia creciente, la estacionalidad semanal (pico domingos) y la anual (pico verano), con un margen de error aceptable y consistente a lo largo del horizonte de predicción de 1 año.

---

## 🗂️ Estructura del repositorio

```
forecasting-tourism/
├── Ejercicio_Forecasting.ipynb   ← Análisis completo con código y conclusiones
├── data/
│   └── datosEjercicio.csv        ← Dataset (serie temporal diaria, 3 años)
└── README.md
```

---

## ⚙️ Instalación y uso

```bash
# Clonar el repositorio
git clone https://github.com/tu-usuario/forecasting-tourism.git
cd forecasting-tourism

# Instalar dependencias
pip install pandas numpy matplotlib seaborn plotly statsmodels prophet holidays

# Abrir el notebook
jupyter notebook Ejercicio_Forecasting.ipynb
```

O directamente en Google Colab:

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/tu-usuario/forecasting-tourism/blob/main/Ejercicio_Forecasting.ipynb)

---

## 🛠️ Stack técnico

`Python` · `pandas` · `statsmodels` · `Prophet` · `plotly` · `matplotlib` · `seaborn` · `scipy`

---

## 👤 Autor

**Juan David Manrique** — Analista de Datos | Barcelona 
[![LinkedIn](https://img.shields.io/badge/LinkedIn-blue?style=flat&logo=linkedin)](https://linkedin.com/in/juan-david-manrique-quiroga-089a94168)
