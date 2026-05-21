# Historial de COVID-19 por Estados (EE. UU.) - Dataset Tracking

Este repositorio contiene el conjunto de datos histórico diario (`all-states-history.csv`) sobre la evolución de la pandemia de COVID-19 en los Estados Unidos. Los datos recopilados reflejan las cifras oficiales reportadas por los departamentos de salud de cada estado desde las fases iniciales de la pandemia hasta el cierre definitivo del proyecto de seguimiento masivo.

## 👥 Autores
* Yasira Blanco Moreno
* M.Angel Moreno Delgado
* Manuel Macarro de la Osa


### 📊 Resumen Ejecutivo del Dataset
* **Rango Temporal:** Desde el `13-01-2020` hasta el `07-03-2021` (Cierre oficial del *COVID Tracking Project*).
* **Volumen de Datos:** 20,780 filas × 41 columnas.
* **Entidades Geográficas:** 56 territorios analizados (los 50 estados de EE. UU. junto a distritos federales y territorios de ultramar como Puerto Rico, Guam e Islas Vírgenes).
---

## 🎯 Objetivo del proyecto
El objetivo principal es compilar y analizar este conjunto de datos **evaluar de forma cuantitativa la trayectoria de las tres primeras olas de contagio en EE. UU.**, permitiendo a investigadores, científicos de datos y tomadores de decisiones:
1. **Modelar la Carga Hospitalaria:** Entender la relación entre el aumento de casos positivos y la ocupación de camas generales, Unidades de Cuidados Intensivos (UCI) y ventiladores mecánicos.
2. **Analizar la Capacidad de Diagnóstico:** Analizar cómo evolucionó la infraestructura de pruebas del país (comparando el uso de PCR frente a la introducción tardía de pruebas de antígeno y anticuerpos).
3. **Estudiar la Letalidad Excesiva:** Identificar picos de mortalidad por regiones y evaluar el desfase temporal (*lag*) que existe entre la detección de un brote y el incremento de fallecidos.

---

## 📊 3. Hallazgos y Conclusiones Clave
Tras el perfil inicial y el análisis exploratorio de los datos (EDA), se destacan las siguientes conclusiones:

* **El Desfase Clínico (*The Data Lag*):** Existe una correlación temporal muy marcada. Tras un pico en la variable `positiveIncrease`, la variable `hospitalizedCurrently` suele alcanzar su máximo entre **10 y 14 días después**, mientras que el impacto en `deathIncrease` se manifiesta con un desfase de **21 a 28 días**.
* **Disparidad en la Infraestructura de Reporte:** El dataset evidencia que la gestión de datos en EE. UU. estuvo descentralizada. Métricas avanzadas como el uso acumulado de ventiladores (`onVentilatorCumulative`, con **93.79% de valores nulos**) o el desglose de muertes probables (`deathProbable`, **63.46% nulos**) demuestran que solo una minoría de los estados tenía la capacidad logística o el marco legal para reportar estas variables con regularidad.
* **Transición Tecnológica en Pruebas:** Los datos reflejan que la primera mitad del año 2020 dependió exclusivamente de pruebas virales (`totalTestsViral`). A partir del tercer trimestre de 2020, comenzaron a poblarse las variables de antígenos (`totalTestsAntigen`), marcando el cambio de estrategia hacia testeos rápidos de cribado masivo.

---

## 💡 4. Recomendaciones Estratégicas Formuladas
Para cualquier analista o equipo de investigación que trabaje con este dataset, se formulan los siguientes puntos:

1. **Normalización Obligatoria por Población:** No se deben comparar variables absolutas (como `positive` o `death`) directamente entre estados (ej. comparar California con Alaska). Es necesario cruzar este dataset con datos censales externos para hacer cálculos *per cápita* (por cada 100,000 habitantes).
2. **Uso de Medias Móviles para el Ruido Diario:** Debido a que los estados reportaban con menor frecuencia los fines de semana, las columnas `Increase` presentan valles artificiales los domingos y picos los martes. Es recomendable utilizar**medias móviles de 7 días** (`.rolling(window=7).mean()`) para suavizar las curvas de tendencia.
3. **Tratamiento de Anomalías (Valores Negativos):** Los valores negativos en los incrementos diarios representan auditorías de datos donde un estado reclasificó falsos positivos. Al entrenar modelos de Machine Learning predictivos, estos valores deben ser controlados o distribuidos de forma retroactiva para no alterar los gradientes.

## 🤝 5. Fuentes del proyecto
https://covidtracking.com/data/download

---

## 📂 6. Estructura del Proyecto
El repositorio mantiene una arquitectura limpia orientada a la reproducibilidad científica:

```text
├── data/
│   └── all-states-history.csv    <-- Dataset histórico principal (20,780 filas)
├── notebooks/
│   ├── 01_data_cleaning.ipynb     <-- Tratamiento de nulos y parseo de fechas (ISO)
│   ├── 02_exploratory_analysis.ipynb <-- Curvas epidemiológicas e hitos por estado
│   └── 03_predictive_modeling.ipynb <-- Modelos de regresión para ocupación de UCI
├── src/
│   ├── __init__.py
│   ├── utils.py                  <-- Funciones auxiliares (cálculo de medias móviles)
│   └── visualization.py          <-- Scripts para generación automática de mapas y gráficos
├── README.md                     <-- Documentación técnica (este archivo)
└── requirements.txt              <-- Dependencias del entorno (pandas, matplotlib, etc.)