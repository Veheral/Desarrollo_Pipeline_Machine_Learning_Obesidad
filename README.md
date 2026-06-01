# **🧠 Pipeline de Machine Learning — Predicción del Nivel de Obesidad**
 
> Pipeline end-to-end de Machine Learning para la clasificación automática del nivel de obesidad a partir de hábitos alimentarios, condición física y datos demográficos.
 
**Autora:** Verónica Hernández Alonso  
**Institución:** EBIS Business Techscool  
**Fecha:** 15-05-2026  
**Dataset:** [Estimation of Obesity Levels — UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/544/estimation+of+obesity+levels+based+on+eating+habits+and+physical+condition)
 
---
 
## **📋 Descripción del Proyecto**
 
Una clínica de nutrición y medicina preventiva requiere un sistema de apoyo a la decisión que clasifique automáticamente el nivel de obesidad de sus pacientes en función de sus hábitos alimentarios, condición física y datos demográficos, con el objetivo de anticipar riesgos de salud y personalizar las intervenciones médicas.
 
---
 
## **📊 Dataset**
 
| Característica | Detalle |
|---|---|
| **Fuente** | UCI Machine Learning Repository |
| **Registros** | 2.111 |
| **Variables** | 17 (16 predictoras + 1 objetivo) |
| **Clases objetivo** | 7 niveles de obesidad |
| **Valores nulos** | Ninguno |
| **Población** | Personas de México, Perú y Colombia |
| **Datos sintéticos** | Parte del dataset generada con SMOTE para balanceo |
 
### **Variable objetivo — `NObeyesdad` (7 clases)**
 
`Insufficient_Weight` · `Normal_Weight` · `Overweight_Level_I` · `Overweight_Level_II` · `Obesity_Type_I` · `Obesity_Type_II` · `Obesity_Type_III`
 
---
 
## **🗂️ Estructura del Proyecto**
 
```
Desarrollo_Pipeline_Machine_Learning_Obesidad/
│
├── pipeline_obesidad.ipynb                              # Notebook principal con todo el pipeline
├── ObesityDataSet_raw_and_data_sinthetic.csv            # Dataset original UCI
├── Desarrollo_Pipeline_de_Machine_Learning_Veronica_Hernandez.pdf  # Informe completo
└── README.md
```
 
---
 
## **🚀 Pipeline**
 
```
EDA → Preprocesamiento → Feature Engineering → Selección de Variables → Modelado → PCA → Clustering → Conclusiones
```
 
### **Etapas detalladas**
 
| # | Etapa | Descripción |
|---|---|---|
| 1 | **EDA** | Estadística descriptiva, distribución de clases, histogramas, correlaciones, boxplots |
| 2 | **Preprocesamiento** | Encoding (Label, Ordinal, One-Hot), StandardScaler, división 80/20 estratificada |
| 3 | **Feature Engineering** | Cálculo de BMI (Weight/Height²), mapeo ordinal del target (0–6) |
| 4 | **Selección de variables** | Random Forest preliminar + umbral de importancia Gini ≥ 1% |
| 5 | **Modelado supervisado** | Comparativa de 4 algoritmos con StratifiedKFold (5-Fold) |
| 6 | **PCA** | Reducción de dimensionalidad, Scree Plot, proyección 2D |
| 7 | **K-Means Clustering** | Análisis no supervisado sobre 5 primeras componentes PCA |
| 8 | **Conclusiones** | Resultados finales y recomendación para producción |
 
---
 
## **🔧 Preprocesamiento**
 
- **Sin imputación**: el dataset no contiene valores nulos.
- **LabelEncoder**: variables binarias — `Gender`, `family_history_with_overweight`, `FAVC`, `SMOKE`, `SCC`.
- **OrdinalEncoder**: variables con orden natural — `CAEC`, `CALC` (No > Sometimes > Frequently > Always).
- **One-Hot Encoding**: variable nominal `MTRANS` (medio de transporte) con `drop_first=True`.
- **BMI calculado**: `Weight / Height²` como feature clínica adicional.
- **StandardScaler**: normalización estándar aplicada sobre train y transformada sobre test.
- **División**: 80% entrenamiento / 20% test, estratificada (`random_state=42`).
### **Variables seleccionadas (importancia Gini ≥ 1%)**
 
`BMI` · `Weight` · `FCVC` · `Age` · `Height` · `Gender`
 
---
 
## **🤖 Modelos entrenados**
 
| Modelo | Tipo | Parámetros clave | CV Accuracy | Test Accuracy | Macro F1 |
|---|---|---|---|---|---|
| **Random Forest** ⭐ | Ensemble (Bagging) | n_estimators=200 | — | **98.58%** | **98.55%** |
| Gradient Boosting | Ensemble (Boosting) | n_est=150, lr=0.1, depth=4 | 97.57% | 98.58% | 98.59% |
| Logistic Regression | Lineal | max_iter=2000 | — | — | — |
| SVM | Kernel RBF | C=10, gamma=scale | 94.31% | 93.14% | 92.97% |
 
> **Mejor modelo: Random Forest** con Test Accuracy = **98.58%** y Macro F1 = **98.55%**
 
### **Classification Report — Random Forest**
 
| Clase | Precision | Recall | F1-score |
|---|---|---|---|
| Insufficient_Weight | 1.00 | 0.98 | 0.99 |
| Normal_Weight | 0.98 | 0.98 | 0.98 |
| Overweight_Level_I | 0.97 | 0.98 | 0.97 |
| Overweight_Level_II | 0.98 | 0.98 | 0.98 |
| Obesity_Type_I | 1.00 | 1.00 | 1.00 |
| Obesity_Type_II | 0.98 | 0.98 | 0.98 |
| Obesity_Type_III | 0.98 | 0.98 | 0.98 |
| **macro avg** | **0.99** | **0.99** | **0.99** |
 
---
 
## **📉 PCA — Reducción de Dimensionalidad**
 
- **15 componentes principales** retienen el **90% de la varianza** original.
- La proyección 2D confirma buena separabilidad en los extremos del espectro (`Insufficient_Weight` y `Obesity_Type_III`) con mayor solapamiento esperado en niveles intermedios.
---
 
## **🔵 K-Means Clustering (No Supervisado)**
 
- Aplicado sobre las **5 primeras componentes PCA**.
- Número óptimo de clusters determinado con el **método del codo + Silhouette Score**.
- **k=9 clusters** → Silhouette Score = **0.267** (separación moderada-buena).
- Los clusters se alinean parcialmente con los grupos de peso, validando estructura natural en los datos. El mayor número de clusters respecto a las clases etiquetadas sugiere la existencia de subperfiles dentro de algunas categorías.
---
 
## **🏆 Conclusiones**
 
- **Rendimiento excepcional**: Random Forest y Gradient Boosting alcanzan un **98.6% de accuracy** con Macro F1 > 98%.
- **Variables dominantes**: `Weight`, `BMI`, `Height`, historial familiar y `Age` son los predictores más relevantes.
- **Variables de comportamiento**: `CAEC` (frecuencia de ingesta entre comidas) y `FAF` (actividad física) tienen importancia secundaria pero estadísticamente significativa.
- **Estructura latente**: K-Means identifica 9 clusters naturales más allá de las 7 etiquetas oficiales.
- **Recomendación para producción**: desplegar **Random Forest** como clasificador principal con umbral de confianza configurable para escalar a revisión manual los casos ambiguos entre clases adyacentes.
---
 


 
## **▶️ Uso**
 
```bash
# **Lanzar el notebook**
jupyter notebook pipeline_obesidad.ipynb
```
 
---
 
## **👤 Autora**
 
**Verónica Hernández Alonso**  
[GitHub](https://github.com/Veheral) · EBIS Business Techscool · 2026
 
