# 🚲 EcoBici Data Analysis — Ciudad Universitaria (UBA)

Análisis exploratorio y modelado del sistema de bicicletas públicas **EcoBici** en Ciudad Universitaria (UBA), combinando datos operativos y variables climáticas.

Proyecto desarrollado como parte del curso **Laboratorio de Datos** (FCEN - UBA).

---

## 📊 Objetivo

Entender patrones de uso del sistema EcoBici y construir un modelo predictivo para la demanda de viajes en estaciones de la red.

---

## 📁 Estructura del repositorio

```
eco-bici-data-analysis/
│
├── analisis_ecobici.ipynb      # Notebook principal
├── full_data.csv               # Disponibilidad de bicis (horario)
├── clima.csv                   # Datos meteorológicos diarios
├── viajes_diarios.csv          # Viajes por estación (regresión)
└── README.md
```

---

## 🔍 Contenido

### 🧼 Procesamiento de datos

* Limpieza y selección de variables
* Traducción y estandarización de columnas
* Construcción de variables temporales
* Conversión a `datetime`
* Clasificación por estación del año (hemisferio sur)

---

### 📈 Análisis descriptivo

* Agregación por fecha y hora (suma de estaciones)
* Comparación por estación del año (promedios)
* Patrones horarios por día de la semana
* Detección de anomalías

---

### 🌦️ Análisis exploratorio

* Evolución temporal del uso
* Comparación días hábiles vs fines de semana
* Impacto del calendario académico
* Relación con clima:

  * temperatura
  * precipitaciones
* Comparación entre estaciones

---

### 📉 Regresión lineal

* Modelado de viajes en estación Plaza Italia
* Tres modelos con hasta 5 variables:

  * selección geográfica
  * selección por correlación
  * variables de origen
* Validación:

  * train/test split
  * **K-Fold Cross Validation**
* Selección de modelo sin data leakage

---

## 🧠 Resultados principales

* Mayor temperatura → mayor uso del sistema
* Lluvia → menor demanda
* Patrones horarios alineados con actividad universitaria
* Diferencias funcionales entre estaciones
* Modelo final con buen desempeño fuera de muestra (**R² ≈ 0.70**)

---

## 🚀 Cómo usar

### ▶️ Opción 1 — Google Colab (recomendado)

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/davidpalacio1/eco-bici-data-analysis/blob/main/analisis_ecobici.ipynb)

---

### 💻 Opción 2 — Local

```bash
git clone https://github.com/davidpalacio1/eco-bici-data-analysis.git
cd eco-bici-data-analysis
pip install pandas numpy matplotlib seaborn scikit-learn
jupyter notebook analisis_ecobici.ipynb
```

---

## 🛠️ Tecnologías

* pandas
* numpy
* matplotlib
* seaborn
* scikit-learn

---

## 👤 Autor

**David Palacio Velásquez**
Estudiante de Ciencias de Datos y Matemáticas — UBA

---

## 📄 Licencia

Uso académico — datos públicos.
