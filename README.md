# Estimación de Mortalidad en Pacientes que han Sufrido un Fallo Cardíaco
**Proyecto del Módulo V del Diplomado Introducción Analítica a la Ciencia de Datos**

**Autores:** Lucas Cordero Lesly Monserrat, Martinez Leon José Emilio, Rodríguez Rodríguez Ian Alam, Rojas Lagunas Kevin Antonio, Silva Pérez Liliana.

##### **Objetivo**
Este trabajo pretende explorar la aplicación y desempeño de distintos modelos de Aprenddizaje Supervisado para la predicción de mortalidad en pacientes que han presentado fallo cardíaco.

Usaremos la base de datos Heart Failure Clinical Records [(Ahmad et tal., 2017)](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0181001), de acceso libre con licencia [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/legalcode), a través de [Kaggle](https://www.kaggle.com/datasets/nimapourmoradi/heart-failure-clinical-records).


# Predicción de Mortalidad por Fallo Cardíaco Mediante Machine Learning 🫀

## 📌 Resumen del Proyecto
El fallo cardíaco es una patología crítica que demanda estrategias de intervención temprana. Este proyecto se enfoca en el análisis y modelado de registros médicos para predecir el riesgo de mortalidad de un paciente (`DEATH_EVENT`). 

Dado el contexto médico, el objetivo técnico central no fue buscar el modelo computacionalmente más complejo, sino priorizar la seguridad clínica. Por lo tanto, el desarrollo se enfocó en **maximizar la métrica de Recall (Sensibilidad)**, minimizando el riesgo de un Falso Negativo (predecir que un paciente sobrevivirá, negándole cuidados intensivos).

---

## 📊 Sobre los Datos
* **Origen:** Instituto de Cardiología de Faisalabad y el Hospital Aliado de Faisalabad, Pakistán (Ahmad et al., 2017), obtenidos vía Kaggle.
* **Volumen:** 299 registros de pacientes descritos por 13 características clínicas numéricas y categóricas.
* **Desbalance de clases:** 67.9% de sobrevivientes frente a 32.1% de fallecimientos, tratado mediante ponderación (`class_weight='balanced'`).

---

## 🛠️ Metodología y Preprocesamiento
1. **Limpieza y Prevención de Data Leakage:** Se excluyó la variable temporal (`time`), ya que contiene información posterior al evento clínico que generaría una sobreestimación artificial del modelo.
2. **Escalamiento:** Se implementó `RobustScaler` para manejar variables numéricas con escalas dispares (como plaquetas y creatinina fosfoquinasa) y presencia de valores atípicos (outliers).
3. **Validación:** Partición estratificada (80% entrenamiento, 20% prueba) y validación cruzada enfocada en Recall, F1-Score y ROC-AUC.

---

## 🏆 Modelos Evaluados y Resultados
Se evaluaron diversos algoritmos: Regresión Logística (con regularizaciones L1 y L2), Random Forest, AdaBoost, XGBoost y un Perceptrón Multicapa (MLP).

**El modelo ganador fue la Regresión Logística optimizada mediante GridSearchCV**.
* **¿Por qué?** A pesar de la potencia de los modelos de ensamblaje o las redes neuronales, el modelo lineal simple demostró mayor capacidad de generalización y el Recall más alto para la clase minoritaria. Los modelos como MLP tuvieron el peor desempeño clínico debido a la escasez de datos (solo 299 registros).

---

## 🧠 Interpretabilidad Médica (Análisis SHAP)
Para garantizar la transparencia del modelo y evitar el efecto "caja negra", se utilizó la interpretabilidad de SHAP. Las tres variables clínicas más determinantes para predecir el riesgo fueron:

1. **Creatinina Sérica (`serum_creatinine`):** Valores altos incrementan drásticamente la probabilidad de predicción de fallecimiento (indicador de falla renal).
2. **Fracción de Eyección (`ejection_fraction`):** Valores bajos impulsan la predicción de mortalidad (ineficiencia del bombeo cardíaco).
3. **Edad (`age`):** Fuerte correlación positiva con el riesgo clínico natural.

---

## 📂 Estructura del Repositorio
* `notebook_analisis.ipynb`: Código completo en Python (EDA, preprocesamiento, entrenamiento y evaluación).
* `dataset_heart_failure.csv`: Conjunto de datos original.
* `Reporte_Tecnico_Ejecutivo.pdf`: Documento formal con el desglose metodológico y visualizaciones analíticas.
* `Presentacion_Proyecto.pdf`: Diapositivas ejecutivas con el resumen de hallazgos.
