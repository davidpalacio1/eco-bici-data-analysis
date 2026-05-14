# 🚲 EcoBici Data Analysis — Ciudad Universitaria (UBA)

Análisis exploratorio y modelado predictivo del sistema de bicicletas públicas **EcoBici** en Ciudad Universitaria (UBA), combinando datos operativos con variables climáticas.

Proyecto desarrollado como parte del curso **Laboratorio de Datos** (FCEN - UBA).

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/davidpalacio1/eco-bici-data-analysis/blob/main/analisis_ecobici.ipynb)

---

## 📋 El problema

El sistema EcoBici enfrenta un problema de **gestión de inventario dinámico**: las bicicletas se acumulan en algunas estaciones y escasean en otras según el horario y las condiciones del día. Predecir la demanda con anticipación permite redistribuir bicicletas de forma proactiva — reduciendo la frustración del usuario y el costo operativo del rebalanceo.

Este análisis responde tres preguntas concretas:
1. ¿Cuándo y cuánto varía la demanda de EcoBici en Ciudad Universitaria?
2. ¿Qué variables externas (temperatura, lluvia, día de la semana) explican esas variaciones?
3. ¿Con qué precisión se puede predecir la demanda diaria en estaciones específicas?

---

## 📁 Estructura del repositorio

```
eco-bici-data-analysis/
│
├── analisis_ecobici.ipynb      # Notebook principal
├── full_data.csv               # Disponibilidad horaria de bicicletas por estación
├── clima.csv                   # Variables meteorológicas diarias
├── viajes_diarios.csv          # Viajes agregados por estación (para regresión)
└── README.md
```

---

## 🔍 Contenido del análisis

### 1. Procesamiento de datos

- Limpieza y selección de variables desde los datos operativos de EcoBici
- Construcción de variables temporales: hora, día de semana, mes, estación del año (hemisferio sur)
- Conversión a `datetime` y clasificación estacional
- Join con datos climáticos diarios por fecha
- Detección y tratamiento de anomalías en la disponibilidad horaria

---

### 2. Análisis descriptivo — ¿Cómo se usa EcoBici?

- **Pico de uso:** los horarios de mayor actividad coinciden con los turnos de entrada y salida universitaria: 8–10 hs y 17–19 hs en días hábiles
- **Fin de semana vs. hábiles:** la demanda cae entre un 35–45% los fines de semana, con un perfil horario más plano (uso recreativo vs. traslado)
- **Estacionalidad:** la primavera y el otoño concentran la mayor demanda; el invierno registra caídas de hasta el 40% respecto al promedio anual
- **Diferencias entre estaciones:** la estación frente al Pabellón II tiene rotación significativamente más alta que las periféricas, actuando como hub de la red en Ciudad Universitaria

---

### 3. Análisis exploratorio — El rol del clima

- **Temperatura:** relación positiva y clara con la demanda. Días con temperaturas superiores a 20°C registran, en promedio, entre 20–30% más viajes que días bajo 15°C
- **Precipitaciones:** efecto negativo fuerte y asimétrico. Un día de lluvia (>1mm) reduce la demanda entre un 40–60% respecto a días equivalentes sin lluvia
- **Interacción temperatura × lluvia:** la lluvia en días fríos prácticamente paraliza el sistema; en días cálidos el efecto es menor (algunos usuarios continúan usando la bici con lluvia leve)
- **Calendario académico:** se detecta una caída sostenida durante el receso de verano (enero–febrero) incluso controlando por temperatura, lo que confirma que la demanda está traccionada por la actividad universitaria y no solo por el clima

---

### 4. Regresión lineal — Predicción de demanda

**Variable objetivo:** viajes diarios en la estación Plaza Italia

**Tres modelos comparados:**

| Modelo | Variables | R² test | RMSE test |
|---|---|---|---|
| Geográfico | Estaciones cercanas (distancia) | — | — |
| Por correlación ✓ | Top 5 variables por correlación con target | **~0.70** | menor |
| Variables de origen | Temperatura, lluvia, día semana, mes | — | — |

- **Mejor modelo:** selección por correlación, con R² ≈ 0.70 en test
- **Validación:** train/test split + **K-Fold Cross-Validation** para confirmar estabilidad fuera de muestra
- **Sin data leakage:** las variables de disponibilidad de otras estaciones se calculan con rezago temporal (valor del día anterior) para evitar usar información futura

**Interpretación del modelo ganador:** la disponibilidad rezagada de estaciones vecinas predice bien la demanda del día siguiente — lo que implica que el sistema tiene **inercia espacial**: cuando una estación tiene alta disponibilidad hoy, tiende a tener alta demanda mañana.

---

## 🧠 Hallazgos principales

- **La lluvia es el predictor más potente a corto plazo:** una caída de 40–60% en la demanda en días de precipitación supera al efecto de temperatura o día de la semana. Para la operación diaria del sistema, el pronóstico meteorológico es más útil que el histórico de demanda.

- **El calendario académico es el driver estructural de largo plazo:** la demanda en período lectivo aproximadamente duplica la del receso, independientemente del clima. Esto sugiere que EcoBici en Ciudad Universitaria funciona esencialmente como un sistema de transporte universitario, no recreativo.

- **R² ≈ 0.70 con 5 variables:** el modelo explica el 70% de la varianza diaria de demanda usando solo disponibilidad de estaciones vecinas y variables temporales — sin necesidad de datos de tráfico ni sensores adicionales.

- **Diferencias funcionales entre estaciones:** las estaciones centrales (alta rotación, muchas llegadas y salidas) se comportan diferente a las periféricas (baja rotación, acumulación). Un modelo único para todas las estaciones subestima la heterogeneidad — modelos por cluster de estación mejorarían el R².

---

## 🚀 Próximos pasos

- Incorporar datos de toda la red de EcoBici (no solo Ciudad Universitaria) para generalizar el modelo
- Explorar modelos de series de tiempo (ARIMA, Prophet) para capturar la autocorrelación temporal
- Construir un modelo de clustering de estaciones por perfil de uso para personalizar las predicciones
- Estimar el impacto económico del rebalanceo proactivo vs. reactivo usando la demanda predicha

---

## 🛠️ Tecnologías

| Herramienta | Uso |
|---|---|
| `pandas` | Carga, limpieza y transformación de datos |
| `numpy` | Operaciones numéricas |
| `matplotlib` | Visualizaciones: series de tiempo, barras, scatter |
| `seaborn` | Heatmaps y distribuciones |
| `sklearn` | Regresión lineal, train/test split, K-Fold CV |

---

## 🚀 Cómo usar

### ▶️ Opción 1 — Google Colab (recomendado)

Clic en el badge al comienzo del README.

### 💻 Opción 2 — Local

```bash
git clone https://github.com/davidpalacio1/eco-bici-data-analysis.git
cd eco-bici-data-analysis
pip install pandas numpy matplotlib seaborn scikit-learn
jupyter notebook analisis_ecobici.ipynb
```

---

## 👤 Autor

**David Palacio Velásquez**  
[LinkedIn](https://www.linkedin.com/in/davidpalacio-velasquez-3864b6298) · davidpalacio1@gmail.com  
Estudiante de Ciencias de Datos y Matemáticas — UBA  
[Ver otros proyectos](https://github.com/davidpalacio1)
