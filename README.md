# Competencia 3 - Agustin Diaz Barquinero y Ezequiel Pereyra

## 1. Resumen Ejecutivo y Solución Final

**Nota:** Esta entrega es el resultado de una decisión de **último momento** ante el fallo de nuestra estrategia principal.

Inicialmente se planeó una estrategia compleja (desglosada más abajo) para esta nueva competencia. La misma **fracasó rotundamente**: todos los modelos daban "helicóptero" en el Leaderboard público (así como el ensemble de los 10), aun cuando los mismos testeados en nuestra validación local arrojaban resultados que considerábamos buenos y aceptables.

Ante la urgencia, recurrimos a una estrategia probada: un modelo utilizado en la Competencia 2, más básico, para testearlo con el bot. Con este enfoque logramos estabilizar los resultados y alcanzamos el status de **"Danger"**.

### 🏆 La Solución Final
La entrega definitiva es un ensamble simple (promedio) de **3 corridas** de este modelo de contingencia.

* **Archivo del Modelo:** `zlightgbm_danger.ipynb`
* **Estrategia:** LightGBM "Legacy" (C2) ensamblado con 3 semillas.




### --------- Estrategia planeada para C3 que fracasó estripirosamente----
Nuestra estrategia final consiste en un **ensamble** de las predicciones de **10 modelos** de LightGBM.

Aunque todos comparten una base de ingeniería de características similar, introdujimos diversidad variando:
1.  **Estrategias de Submuestreo (Undersampling):** Probamos tasas de 0.25 y 0.05.
2.  **Deflactores:** Variantes ajustadas por índice UVA, RIPTE e IPC.
3.  **Selección de Variables:** Modelos con y sin *ranks*, variables de *random forest*, o *lags* específicos.
4.  **Semillas y Autores:** Entrenamiento colaborativo para maximizar la aleatoriedad.

> **Dato Técnico Importante:** Para cada uno de los 10 modelos candidatos listados abajo, se aplicó **Semillerio con 100 semillas (100 seeds)** 

### 1. Arquitectura del Ensamble
El *score* final se obtiene promediando las probabilidades de los siguientes modelos candidatos. La diversidad entre ellos (especialmente entre las estrategias M1/M2 y el resto) fue clave para estabilizar la predicción final.

| ID | Estrategia / Features Principales | Semilla (Autor) | Ganancia (Validación) |
|:---:|:---|:---:|:---:|
| **M1** | FINAL (Undersampling = 0.25) | 100003 (Eze) | 421.600.000 |
| **M2** | FINAL (Undersampling = 0.25) | 990013 (Agus) | 421.600.000 |
| **M3** | Todo (sin Ranks) + Dummies + Lags 6,12 (Und=0.05) | 100003 (Eze) | 433.440.000 |
| **M4** | Todo (sin Ranks) + Lags 6,12 (Und=0.05) | 990013 (Agus) | 433.440.000 |
| **M5** | **Todo + Lags 6 y 12 (Und=0.05)** | **100003 (Eze)** | **436.800.000** |
| **M6** | **Todo + Lags 6 y 12 (Und=0.05)** | **990013 (Agus)** | **436.800.000** |
| **M7** | Todo + Lags 6 y 12 c/UVA (Und=0.05) | 100003 (Eze) | 435.200.000 |
| **M8** | Todo (sin Ranks) + Lags 6,12 + RIPTE (Und=0.05) | 100003 (Eze) | 430.800.000 |
| **M9** | Todo (sin Ranks) + Lags 6,12 + RF + Ratio Avg | 100003 (Eze) | 433.200.000 |
| **M10** | Todo (sin Ranks) + Lags 6,12 c/UVA (Und=0.05) | 990013 (Agus) | 428.000.000 |


## 2. Estructura del Repositorio y Guía de Archivos

Para facilitar la identificación de los experimentos, los notebooks siguen una **convención de nombres** estricta que vincula el código con la tabla de resultados anterior.

### 📂 Nomenclatura de Archivos
Los archivos principales siguen el patrón:
`m{ID}_{Estrategia}_{Semilla}.ipynb`

* **`m{ID}`**: Identificador único del modelo (coincide con la tabla de la Sección 2).
* **`{Estrategia}`**: Breve descripción (ej: `uva`, `under_25`, `ranks`).
* **`{Semilla}`**: Semilla aleatoria utilizada para reproducibilidad.

### 🗺️ Mapeo de Modelos
A continuación, se indican los archivos específicos para reproducir cada modelo:

* **M1:** `m1_final_under_25_100003.ipynb`
* **M2:** `m2_final_under_25_990013.ipynb`
* **M3:** `m3_ranks_dummies_100003.ipynb`
* **M4:** `m4_ranks_lags_990013.ipynb`
* **M5:** `m5_todo_lags_100003.ipynb`
* **M6:** `m6_todo_lags_990013.ipynb`
* **M7:** `m7_todo_uva_100003.ipynb`
* **M8:** `m8_ranks_ripte_100003.ipynb`
* **M9:** `m9_rf_ratio_100003.ipynb`
* **M10:** `m10_uva_990013.ipynb`

### 🚀 Ensamble Final
El archivo que consolida las salidas de los 10 modelos anteriores y genera el submission final es:

* **Script de Ensamble:** `ensembles_fallidos.ipynb`
