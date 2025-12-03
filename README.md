# Competencia 3 - Agustin Diaz Barquinero y Ezequiel Pereyra


Nuestra estrategia final consiste en un **ensamble** de las predicciones de **10 modelos** de LightGBM.

Aunque todos comparten una base de ingeniería de características similar, introdujimos diversidad variando:
1.  **Estrategias de Submuestreo (Undersampling):** Probamos tasas de 0.25 y 0.05.
2.  **Deflactores:** Variantes ajustadas por índice UVA, RIPTE e IPC.
3.  **Selección de Variables:** Modelos con y sin *ranks*, variables de *random forest*, o *lags* específicos.
4.  **Semillas y Autores:** Entrenamiento colaborativo para maximizar la aleatoriedad.

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

> **Nota:** "Todo (sin Ranks)" indica que se utilizaron todas las variables generadas excepto las de tipo *Rank*.

## 2. Estructura del Repositorio
* `/notebooks`: Contiene el análisis exploratorio y el tuning de hiperparámetros.
* `/src`: Scripts de python para reproducir el entrenamiento y la inferencia.



