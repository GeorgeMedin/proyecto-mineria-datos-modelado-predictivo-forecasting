# Proyecto de mineria de datos Modelado Predictivo Forecasting

Proyecto de analítica predictiva aplicado al sector retail, desarrollado en Python para abordar dos necesidades de negocio: la identificación de clientes con riesgo de abandono y el pronóstico semanal de ventas.

> Proyecto académico desarrollado para la asignatura **Inteligencia Analítica y Minería de Datos**.

## Objetivo del proyecto

Construir una solución analítica de principio a fin que permita:

- Identificar clientes con riesgo de abandono o *churn*.
- Analizar los factores relacionados con la pérdida de clientes.
- Estudiar la evolución temporal de las ventas.
- Pronosticar las ventas semanales para apoyar la planificación comercial.
- Convertir los resultados de los modelos en recomendaciones para el negocio.

## Problemas analizados

### 1. Predicción de abandono de clientes

Se formuló un problema de clasificación binaria en el que un cliente se considera en estado de *churn* cuando han transcurrido más de 90 días desde su última compra.

La variable **Recency** fue excluida del conjunto de variables predictoras, debido a que se utilizó para construir la variable objetivo. De esta manera, se evitó la fuga de información o *data leakage*.

### 2. Pronóstico de ventas

Los registros transaccionales fueron agregados semanalmente para analizar la tendencia, la estacionalidad y los cambios en el comportamiento de las ventas. Posteriormente, se construyó un modelo ARIMA para generar un pronóstico de 12 semanas.

## Dataset

El proyecto utiliza el dataset **Online Retail II**, compuesto por transacciones de una empresa de comercio minorista del Reino Unido.

Principales variables del conjunto original:

- `Invoice`: identificador de la factura.
- `StockCode`: código del producto.
- `Description`: descripción del artículo.
- `Quantity`: cantidad comprada.
- `InvoiceDate`: fecha y hora de la transacción.
- `Price`: precio unitario.
- `Customer ID`: identificador del cliente.
- `Country`: país del cliente.

El conjunto inicial contenía **1.067.371 registros**. Después del proceso de limpieza se conservaron **779.425 transacciones válidas**.

> Por razones de tamaño y licencia, el dataset puede no estar almacenado directamente en este repositorio. Consulte la fuente indicada en la sección de referencias.

## Metodología

El proyecto se desarrolló siguiendo las siguientes etapas:

1. Carga e integración de los datos.
2. Exploración inicial del conjunto transaccional.
3. Limpieza de registros nulos, cancelaciones y valores inconsistentes.
4. Revisión y tratamiento de valores atípicos.
5. Construcción de variables a nivel de cliente.
6. Definición de la variable objetivo `Churn`.
7. División de los datos en entrenamiento, validación y prueba.
8. Entrenamiento y comparación de modelos de clasificación.
9. Construcción y descomposición de la serie temporal semanal.
10. Entrenamiento y evaluación del modelo ARIMA.
11. Interpretación de los resultados desde una perspectiva de negocio.

## Variables utilizadas para clasificación

A partir de los registros transaccionales, se construyeron las siguientes características por cliente:

- `Frequency`: número de facturas únicas.
- `Monetary`: gasto total acumulado.
- `TotalQuantity`: cantidad total de unidades compradas.
- `AvgTicket`: valor promedio por pedido.
- `Is_UK`: indicador de residencia en el Reino Unido.

La base final para clasificación estuvo compuesta por **5.878 clientes**.

## Modelos evaluados

### Clasificación

Se compararon dos modelos:

- Regresión Logística.
- Random Forest Classifier.

El modelo seleccionado fue **Random Forest**, debido a su mejor desempeño general y a su capacidad para representar relaciones no lineales entre las variables.

### Series de tiempo

Para el pronóstico de ventas se utilizó un modelo **ARIMA(1,1,1)**. La serie semanal estuvo conformada por **106 semanas**, de las cuales:

- 94 semanas fueron utilizadas para entrenamiento.
- 12 semanas fueron reservadas para evaluación.

## Resultados principales

### Random Forest

- **Accuracy:** 70,75 %
- **Precision para Churn:** 69,45 %
- **Recall para Churn:** 75,95 %
- **F1-score para Churn:** 72,55 %
- **AUC-ROC:** 77,60 %

El modelo logró identificar aproximadamente tres de cada cuatro clientes que realmente pertenecían a la clase *churn*.

### ARIMA(1,1,1)

- **MAE:** £97.475,40
- **RMSE:** £102.334,47
- **MAPE:** 37,07 %

El modelo capturó el comportamiento general de la serie, aunque presentó dificultades para anticipar los incrementos abruptos de ventas asociados con la temporada de fin de año.

## Aplicaciones para el negocio

Los resultados del proyecto pueden apoyar decisiones relacionadas con:

- Campañas de retención dirigidas a clientes con mayor riesgo de abandono.
- Programas de fidelización orientados a aumentar la frecuencia de compra.
- Priorización de clientes según riesgo y valor comercial.
- Planificación de inventarios y abastecimiento.
- Preparación logística para temporadas de alta demanda.
- Seguimiento periódico de las ventas y del comportamiento de los clientes.

## Tecnologías utilizadas

- Python
- pandas
- NumPy
- Matplotlib
- Seaborn
- scikit-learn
- statsmodels
- Jupyter Notebook / Google Colab

## Estructura sugerida del repositorio

```text
retail-churn-forecasting-python/
├── data/
│   └── README.md
├── notebooks/
│   └── retail_churn_forecasting.ipynb
├── reports/
│   └── informe_final.pdf
├── images/
│   ├── churn_distribution.png
│   ├── confusion_matrix.png
│   ├── seasonal_decomposition.png
│   └── arima_forecast.png
├── requirements.txt
├── .gitignore
├── LICENSE
└── README.md
```

## Instalación

Clone el repositorio:

```bash
git clone https://github.com/USUARIO/retail-churn-forecasting-python.git
cd retail-churn-forecasting-python
```

Cree y active un entorno virtual:

```bash
python -m venv .venv
```

En Windows:

```bash
.venv\Scripts\activate
```

En Linux o macOS:

```bash
source .venv/bin/activate
```

Instale las dependencias:

```bash
pip install -r requirements.txt
```

## Ejecución

Abra el notebook principal ubicado en la carpeta `notebooks` y ejecute las celdas en el orden establecido.

```bash
jupyter notebook notebooks/retail_churn_forecasting.ipynb
```

También puede cargar el notebook en Google Colab y ejecutarlo desde el navegador.

## Limitaciones

- El modelo de churn solo considera clientes identificados, por lo que no cubre las compras anónimas.
- La definición de churn depende del umbral de 90 días establecido para este proyecto.
- El modelo ARIMA es univariado y no incorpora promociones, festivos ni inversión publicitaria.
- Los patrones históricos pueden cambiar debido a factores económicos, comerciales o logísticos.
- Los resultados corresponden a un ejercicio académico y deben validarse antes de aplicarse en un entorno empresarial real.

## Mejoras futuras

- Incorporar variables externas mediante modelos SARIMAX.
- Evaluar métodos alternativos de forecasting y validación temporal.
- Calcular el valor de vida del cliente o `Customer Lifetime Value`.
- Combinar el riesgo de churn con el valor económico de cada cliente.
- Ajustar el umbral de clasificación según el costo de falsos positivos y falsos negativos.
- Automatizar el entrenamiento, monitoreo y generación de alertas.
- Integrar las predicciones con un CRM o una herramienta de inteligencia de negocios.

## Autores

- Jorge Andrés Medina Rincón
- Michael Daniel Peñuela Bello
- Wendy Yasmin Tautiva Melo

## Referencias

- Chen, D., Sain, S. L. y Guo, K. (2012). *Data mining for the online retail industry: A case study of RFM model-based customer segmentation using data mining*. Journal of Database Marketing & Customer Strategy Management, 19(3), 197-208.
- Fawcett, T. (2006). *An introduction to ROC analysis*. Pattern Recognition Letters, 27(8), 861-874.
- Box, G. E. P., Jenkins, G. M., Reinsel, G. C. y Ljung, G. M. (2015). *Time Series Analysis: Forecasting and Control* (5.ª ed.). Wiley.
- Dickey, D. A. y Fuller, W. A. (1979). *Distribution of the estimators for autoregressive time series with a unit root*. Journal of the American Statistical Association, 74(366a), 427-431.

## Licencia

Este proyecto fue desarrollado con fines académicos. Si desea reutilizar el código, incluya una licencia apropiada en el repositorio y conserve el reconocimiento de sus autores.
