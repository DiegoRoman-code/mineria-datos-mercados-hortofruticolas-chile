
# 📊 Minería de Datos: Análisis de precios y volúmenes de frutas y hortalizas en Chile

Este proyecto consiste en el análisis de datos de precios y volúmenes de productos agrícolas comercializados en mercados mayoristas chilenos. Se utilizó la metodología **CRISP-DM** para desarrollar un proceso de análisis que abarca desde la exploración y limpieza de los datos hasta la construcción y evaluación de modelos de Machine Learning.

> Proyecto académico desarrollado para la asignatura Minería de Datos 2 de Ingeniería en Informática en Duoc UC.

## 🎯 Objetivos del proyecto

- Estudiar el comportamiento de los precios y volúmenes de comercialización de productos agrícolas.
- **Regresión:** estimar el volumen por unidad mediante la variable `Volumen_Unidad`.
- **Clasificación:** identificar el mercado de comercialización entre los cuatro mercados con mayor frecuencia.
- **Clustering:** agrupar productos según sus características para identificar patrones y segmentos.

## 📂 Dataset utilizado

- **Fuente:** [Chilean Fruit and Veg. Wholesale Price & Volume](https://www.kaggle.com/datasets/sebastiantarebustos/odepa-chilean-fruit-and-vegetables-volume-price).
- **Origen de los datos:** información basada en registros de ODEPA.
- **Período analizado:** enero de 2021 a diciembre de 2022.
- **Tamaño del conjunto:** aproximadamente 49.000 registros.
- **Información disponible:** precios, volúmenes, productos, variedades, mercados, unidades de comercialización, fechas y datos geográficos.

### Variables y transformación de datos

Durante la preparación se trabajó con variables como `Producto`, `Variedad`, `Mercado`, `Unidad`, `Fecha`, `Precio` y `Volumen`.

También se incorporaron variables derivadas, entre ellas `Precio_tratado` y `Precio_Unidad`, utilizadas en diferentes experimentos de modelado.

**Importante:** para ejecutar el notebook, descarga el conjunto de datos desde Kaggle y cárgalo cuando lo solicite el código.

## 🔄 Metodología CRISP-DM

El desarrollo se organizó en seis etapas:

1. **Comprensión del negocio:** definición de los objetivos del análisis y de indicadores relacionados con los precios y volúmenes de los mercados.
2. **Exploración de los datos:** revisión de estadísticas descriptivas, valores faltantes, distribuciones, valores atípicos y relaciones entre variables.
3. **Preparación de los datos:**
   - Imputación de variedades no especificadas utilizando la moda de cada producto.
   - Detección y tratamiento de valores atípicos mediante el rango intercuartílico (IQR).
   - Generación de variables derivadas para los experimentos.
   - Escalado de variables para aplicar K-Means.
4. **Modelado:** ejecución de distintos experimentos con combinaciones de variables y algoritmos.
5. **Evaluación:** comparación de modelos mediante métricas específicas para regresión, clasificación y clustering.
6. **Validación:** aplicación de validación cruzada de cinco particiones y evaluación temporal con datos de diciembre de 2022.

### División temporal para clasificación

- **Entrenamiento:** enero de 2021 a mayo de 2022.
- **Prueba:** junio a noviembre de 2022.
- **Validación final:** diciembre de 2022.

## 📈 Resultados obtenidos

### 1. Regresión: estimación de `Volumen_Unidad`

Se compararon distintos algoritmos para determinar cuál ofrecía mejores resultados al estimar el volumen por unidad.

| Modelo | Mejor combinación de variables | R² de prueba | RMSE de prueba |
|---|---|---:|---:|
| **XGBoost** | Precio_Unidad + Volumen | **0.9571** | **3175.59** |
| KNN | Precio_Unidad + Volumen | 0.9565 | 3826.02 |
| Árbol de Decisión | Precio_Unidad + Volumen | 0.9286 | 4899.09 |
| Random Forest | Precio_tratado + Volumen | 0.9283 | 4910.95 |
| Regresión Lineal | Por completar | Por completar | Por completar |

**Mejor resultado:** XGBoost obtuvo el mayor R² de prueba entre los modelos reportados, con un valor de 0.9571.

La validación cruzada de XGBoost se realizó con cinco particiones. Los valores promedio y de desviación deben incorporarse cuando estén disponibles.

### 2. Clasificación: identificación del mercado

El objetivo fue predecir el mercado de comercialización considerando los cuatro mercados más frecuentes del conjunto de datos.

| Modelo | Variables utilizadas | Accuracy de prueba | F1 de prueba |
|---|---|---:|---:|
| **Random Forest** | Precio, Numero_Unidad, Producto | **0.6631** | **0.6651** |
| XGBoost | Todas las variables | 0.6390 | 0.6417 |
| KNN | Precio, Numero_Unidad, Producto, Variedad | 0.6293 | 0.6302 |
| Árbol de Decisión | Precio, Numero_Unidad, Producto, Variedad | 0.4808 | Por completar |

#### Validación temporal

La evaluación realizada con datos de diciembre de 2022 para Random Forest obtuvo los siguientes resultados:

- **Accuracy:** 0.5857
- **F1-score:** 0.5906

El rendimiento fue inferior al obtenido en el conjunto de prueba, lo que evidencia dificultades para mantener la misma capacidad predictiva frente a datos de un período posterior.

### 3. Clustering: segmentación de productos

Se aplicaron algoritmos de agrupamiento para identificar perfiles de productos según sus características de precio y volumen.

| Algoritmo | Coeficiente Silhouette |
|---|---:|
| **K-Means** | **0.8974** |
| Clustering Jerárquico (Ward) | 0.8924 |

Para K-Means se trabajó con cuatro grupos, seleccionados mediante el método del codo.

Los segmentos identificados representan distintos comportamientos comerciales, incluyendo productos de precio medio con bajo volumen, productos de precio elevado y poco volumen, productos de mayor volumen y un grupo de características intermedias.

## 📝 Conclusiones

- Las variables relacionadas con el precio y el volumen tuvieron un papel importante en los experimentos de regresión.
- XGBoost presentó el mejor resultado de regresión reportado, mientras que Random Forest destacó en clasificación.
- La disminución del rendimiento en diciembre de 2022 muestra que la capacidad de generalización del clasificador debe seguir mejorándose.
- Los resultados de clustering permitieron reconocer cuatro grupos de productos con diferentes características comerciales.
- La comparación de algoritmos y métricas permitió identificar fortalezas y limitaciones de los modelos aplicados.

## ⚠️ Limitaciones del análisis

- El uso de `Volumen` para predecir `Volumen_Unidad` puede generar una relación muy fuerte entre predictor y objetivo, elevando las métricas de regresión. Por ello, los resultados deben interpretarse considerando esta dependencia.
- La sustitución de valores atípicos por la media del producto puede disminuir la variabilidad original de los precios.
- El modelo de clasificación presenta un rendimiento moderado y todavía podría beneficiarse de ajustes de hiperparámetros.
- El análisis abarca únicamente los años 2021 y 2022, lo que limita el estudio de tendencias y patrones estacionales de largo plazo.
- Algunas métricas de experimentos secundarios están pendientes de completar.

## 🧰 Tecnologías y herramientas

- **Lenguaje:** Python
- **Manipulación de datos:** pandas, NumPy
- **Machine Learning:** scikit-learn, XGBoost
- **Visualización:** matplotlib, seaborn
- **Entorno de desarrollo:** Google Colab
- **Metodología:** CRISP-DM

## 🚀 Cómo ejecutar el proyecto

1. Descarga el dataset desde [Kaggle](https://www.kaggle.com/datasets/sebastiantarebustos/odepa-chilean-fruit-and-vegetables-volume-price).
2. Abre el notebook del proyecto en Google Colab.
3. Carga el archivo de datos cuando lo solicite la primera celda.
4. Ejecuta las celdas en orden para reproducir el análisis, el procesamiento y los experimentos.

### Abrir en Google Colab

Reemplaza `TU_USUARIO` y `NOMBRE_DEL_REPO` por los datos reales de tu repositorio:

[![Abrir en Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/TU_USUARIO/NOMBRE_DEL_REPO/blob/main/mineria_datos_2.ipynb)

## 📁 Estructura del repositorio

```text
├── mineria_datos_2.ipynb
├── README.md
└── datos/
    └── archivo.xlsx
```

*La estructura de la carpeta `datos/` debe ajustarse a los archivos que realmente hayas incluido en el repositorio.*

## 👥 Autores

- **Paulina Solís**
- **Diego Román**
