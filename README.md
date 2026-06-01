# Desarrollo_Pipeline_Machine_Learning_Obesidad
Pipeline completo de Machine Learning para predecir niveles de obesidad usando el dataset de UCI. Incluye EDA, preprocesamiento, ingeniería de variables, selección de características, modelos supervisados, PCA y clustering. Mejor modelo: Random Forest (98.6% accuracy).

🧠 Pipeline de Machine Learning — Predicción del Nivel de Obesidad

Pipeline end-to-end de Machine Learning para la clasificación automática del nivel de obesidad a partir de hábitos alimentarios, condición física y datos demográficos.

Autora: Verónica Hernández Alonso
Institución: EBIS Business Techscool
Fecha: 15-05-2026
Dataset: Estimation of Obesity Levels — UCI Machine Learning Repository

📋 Descripción del Proyecto
Una clínica de nutrición y medicina preventiva requiere un sistema de apoyo a la decisión que clasifique automáticamente el nivel de obesidad de sus pacientes en función de sus hábitos alimentarios, condición física y datos demográficos, con el objetivo de anticipar riesgos de salud y personalizar las intervenciones médicas.

📊 Dataset
CaracterísticaDetalleFuenteUCI Machine Learning RepositoryRegistros2.111Variables17 (16 predictoras + 1 objetivo)Clases objetivo7 niveles de obesidadValores nulosNingunoPoblaciónPersonas de México, Perú y ColombiaDatos sintéticosParte del dataset generada con SMOTE para balanceo
Variable objetivo — NObeyesdad (7 clases)
Insufficient_Weight · Normal_Weight · Overweight_Level_I · Overweight_Level_II · Obesity_Type_I · Obesity_Type_II · Obesity_Type_III

🗂️ Estructura del Proyecto
Desarrollo_Pipeline_Machine_Learning_Obesidad/
│
├── pipeline_obesidad.ipynb                              # Notebook principal con todo el pipeline
├── ObesityDataSet_raw_and_data_sinthetic.csv            # Dataset original UCI
├── Desarrollo_Pipeline_de_Machine_Learning_Veronica_Hernandez.pdf  # Informe completo
└── README.md

🚀 Pipeline
EDA → Preprocesamiento → Feature Engineering → Selección de Variables → Modelado → PCA → Clustering → Conclusiones
Etapas detalladas
#EtapaDescripción1EDAEstadística descriptiva, distribución de clases, histogramas, correlaciones, boxplots2PreprocesamientoEncoding (Label, Ordinal, One-Hot), StandardScaler, división 80/20 estratificada3Feature EngineeringCálculo de BMI (Weight/Height²), mapeo ordinal del target (0–6)4Selección de variablesRandom Forest preliminar + umbral de importancia Gini ≥ 1%5Modelado supervisadoComparativa de 4 algoritmos con StratifiedKFold (5-Fold)6PCAReducción de dimensionalidad, Scree Plot, proyección 2D7K-Means ClusteringAnálisis no supervisado sobre 5 primeras componentes PCA8ConclusionesResultados finales y recomendación para producción

🔧 Preprocesamiento

Sin imputación: el dataset no contiene valores nulos.
LabelEncoder: variables binarias — Gender, family_history_with_overweight, FAVC, SMOKE, SCC.
OrdinalEncoder: variables con orden natural — CAEC, CALC (No > Sometimes > Frequently > Always).
One-Hot Encoding: variable nominal MTRANS (medio de transporte) con drop_first=True.
BMI calculado: Weight / Height² como feature clínica adicional.
StandardScaler: normalización estándar aplicada sobre train y transformada sobre test.
División: 80% entrenamiento / 20% test, estratificada (random_state=42).

Variables seleccionadas (importancia Gini ≥ 1%)
BMI · Weight · FCVC · Age · Height · Gender

🤖 Modelos entrenados
ModeloTipoParámetros claveCV AccuracyTest AccuracyMacro F1Random Forest ⭐Ensemble (Bagging)n_estimators=200—98.58%98.55%Gradient BoostingEnsemble (Boosting)n_est=150, lr=0.1, depth=497.57%98.58%98.59%Logistic RegressionLinealmax_iter=2000———SVMKernel RBFC=10, gamma=scale94.31%93.14%92.97%

Mejor modelo: Random Forest con Test Accuracy = 98.58% y Macro F1 = 98.55%

Classification Report — Random Forest
ClasePrecisionRecallF1-scoreInsufficient_Weight1.000.980.99Normal_Weight0.980.980.98Overweight_Level_I0.970.980.97Overweight_Level_II0.980.980.98Obesity_Type_I1.001.001.00Obesity_Type_II0.980.980.98Obesity_Type_III0.980.980.98macro avg0.990.990.99

📉 PCA — Reducción de Dimensionalidad

15 componentes principales retienen el 90% de la varianza original.
La proyección 2D confirma buena separabilidad en los extremos del espectro (Insufficient_Weight y Obesity_Type_III) con mayor solapamiento esperado en niveles intermedios.


🔵 K-Means Clustering (No Supervisado)

Aplicado sobre las 5 primeras componentes PCA.
Número óptimo de clusters determinado con el método del codo + Silhouette Score.
k=9 clusters → Silhouette Score = 0.267 (separación moderada-buena).
Los clusters se alinean parcialmente con los grupos de peso, validando estructura natural en los datos. El mayor número de clusters respecto a las clases etiquetadas sugiere la existencia de subperfiles dentro de algunas categorías.


🏆 Conclusiones

Rendimiento excepcional: Random Forest y Gradient Boosting alcanzan un 98.6% de accuracy con Macro F1 > 98%.
Variables dominantes: Weight, BMI, Height, historial familiar y Age son los predictores más relevantes.
Variables de comportamiento: CAEC (frecuencia de ingesta entre comidas) y FAF (actividad física) tienen importancia secundaria pero estadísticamente significativa.
Estructura latente: K-Means identifica 9 clusters naturales más allá de las 7 etiquetas oficiales.
Recomendación para producción: desplegar Random Forest como clasificador principal con umbral de confianza configurable para escalar a revisión manual los casos ambiguos entre clases adyacentes.


⚙️ Instalación
bash# Clonar el repositorio
git clone https://github.com/Veheral/Desarrollo_Pipeline_Machine_Learning_Obesidad.git
cd Desarrollo_Pipeline_Machine_Learning_Obesidad

# Crear entorno virtual
python -m venv venv
source venv/bin/activate        # Linux/Mac
venv\Scripts\activate           # Windows

# Instalar dependencias
pip install -r requirements.txt
Dependencias principales
pandas
numpy
scikit-learn
matplotlib
seaborn
jupyter

▶️ Uso
bash# Lanzar el notebook
jupyter notebook pipeline_obesidad.ipynb

👤 Autora
Verónica Hernández Alonso
GitHub · EBIS Business Techscool · 2026
