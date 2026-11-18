# Facturación residencial de energía eléctrica 2023–2024

Trabajo de la electiva: limpieza, análisis exploratorio, agrupamiento y modelos supervisados
sobre el archivo de facturación residencial.

## División de la exposición

La presentación se divide en **3 bloques autocontenidos**, uno por expositor. Cada notebook
arranca con una tarjeta `### Bloque X · Expositor X`, el objetivo, el dataset de entrada/salida
y el mapa ordenado de sus gráficas.

| Bloque | Expositor | Notebook | Cubre |
|---|---|---|---|
| 1 | Persona 1 | `01_limpieza_y_exploratorio.ipynb` | Calidad de datos, EDA univariado/bivariado y problemáticas 4.1 y 4.2 |
| 2 | Persona 2 | `02_clustering.ipynb` | Segmentación de cuentas con K-Means (4.3) y agrupamiento jerárquico municipal (4.4) |
| 3 | Persona 3 | `03_modelos_supervisados.ipynb` | Mora > 60 días con Árbol de Decisión (4.5) y priorización de inspecciones con Random Forest (4.6) |

**Reglas de independencia:**

- La **01** lee el CSV crudo y exporta `data/datos_limpios.parquet`.
- La **02** y la **03** leen exclusivamente el Parquet; no vuelven a limpiar el CSV ni lo sobrescriben.
- La **03** conserva el corte temporal estricto (`<= 2024-06` vs `>= 2024-07`), la ventana móvil
  de 12 meses y la exportación de `data/top_1000_cuentas_inspeccion.csv`.

## Mapa de gráficas

Todas las figuras comparten la misma estética (`sns.set_theme(style='whitegrid', palette='deep')`,
paleta `PALETA`, `FIGSIZE` y tamaños de fuente), para que los tres expositores se vean como un
solo estudio. Antes de cada gráfica hay una celda Markdown con el título y la pregunta que
responde; después, una lectura rápida para el expositor.

### Bloque 1 · `01_limpieza_y_exploratorio.ipynb` (4 gráficas)

| # | Gráfica | Sección |
|---|---|---|
| G1 | Panel univariado 2×2: consumo, tarifa, estrato y tramo de mora | 3.3 |
| G2 | Panel multivariado 1×2: consumo vs. personas y consumo vs. clima | 3.4 |
| G3 | Mapa de calor de correlaciones (Spearman) | 3.4 |
| G4 | Barras categóricas 2×2 (medidor, zona, clima, lectura) + tabla χ² | 3.4 |

Las problemáticas 4.1 y 4.2 se responden con tablas y pruebas estadísticas (sin figuras).

### Bloque 2 · `02_clustering.ipynb` (6 gráficas)

| # | Gráfica | Sección |
|---|---|---|
| G1 | Codo y silueta para elegir k (1×2) | 3.5 / 4.3 |
| G2 | Perfil de segmentos: heatmap, tamaño y mora (1×3) | 3.5 / 4.3 |
| G3 | Dispersión de segmentos (1×2) | 3.5 / 4.3 |
| G4 | Indicadores municipales: correlación y tarifa vs. mora (1×2) | 3.6 / 4.4 |
| G5 | Dendrograma de Ward y costo de unión (1×2) | 3.6 / 4.4 |
| G6 | Perfil promedio de los grupos municipales (heatmap) | 3.6 / 4.4 |

### Bloque 3 · `03_modelos_supervisados.ipynb` (5 gráficas)

| # | Gráfica | Sección |
|---|---|---|
| G1 | Árbol de decisión para mora > 60 días | 3.7 / 4.5 |
| G2 | Matriz de confusión y métricas del árbol (1×2) | 3.7 / 4.5 |
| G3 | Riesgo por incremento de factura: Caribe vs. resto (heatmap) | 3.7 / 4.5 |
| G4 | Importancia de variables por permutación (1×1) | 3.8 / 4.6 |
| G5 | Ranking de inspección y mejora del Top 20% (1×2) | 3.8 / 4.6 |

El corte temporal y balance de clases (4.5) y la comparación de AUC/ROC (4.6) se presentan con
tablas y métricas impresas, sin figuras.

## Contenido del repositorio

| Archivo | Descripción |
|---|---|
| `01_limpieza_y_exploratorio.ipynb` | Bloque 1: limpieza, EDA y problemáticas 4.1–4.2 |
| `02_clustering.ipynb` | Bloque 2: K-Means de cuentas y agrupamiento municipal |
| `03_modelos_supervisados.ipynb` | Bloque 3: árbol de mora y Random Forest de inspecciones |
| `data/datos_limpios.parquet` | Dataset limpio que produce la 01 y consumen la 02 y la 03 |
| `data/top_1000_cuentas_inspeccion.csv` | Las 1.000 cuentas recomendadas para inspección (4.6) |

## Cómo ejecutarlo

```bash
python -m venv venv
venv\Scripts\activate          # Windows
pip install -r requirements.txt
```

Abre los notebooks **en orden** (01 → 02 → 03) y ejecuta `Restart & Run All`.
La 01 necesita el CSV crudo; la 02 y la 03 solo necesitan el Parquet.

## Sobre el archivo crudo

El CSV de facturación pesa **329 MB**, por encima del límite de 100 MB por archivo de GitHub,
así que **no está en el repositorio** y no se puede subir con un `git push` normal.
Cada persona debe descargarlo aparte y dejarlo en:

- `Energia/data/`, o
- la raíz del repositorio.

**El nombre no importa.** La 01 busca cualquier `*.csv` que tenga las columnas del dataset
(`id_cuenta`, `periodo`, `consumo_kwh`, `tarifa_cu_cop_kwh`, `dias_mora`) y muestra en pantalla
cuál encontró:

```
Archivo de facturación encontrado: ...\facturacion_energia_residencial_2023_2024.csv
```

Si aparece `FileNotFoundError`, el archivo no está en ninguna de las carpetas revisadas:
déjalo en `Energia/data/` y vuelve a ejecutar la primera celda.

### ¿Y si no tengo el CSV crudo?

El Parquet limpio **sí está en el repositorio**, así que la 02 y la 03 se pueden ejecutar
sin el archivo crudo. Solo la 01 lo necesita para reconstruir el Parquet.

## Advertencias

- No subir el CSV crudo a Git (está en `.gitignore`).
- El Parquet pesa 59,7 MB: por encima del tamaño recomendado por GitHub (50 MB), pero por
  debajo del límite duro (100 MB).
- La 03 escribe `data/top_1000_cuentas_inspeccion.csv`; si se vuelve a ejecutar, el archivo
  se regenera con diferencias de punto flotante en la última cifra, sin cambios reales.
- Las etiquetas de inspección no son aleatorias: el ranking del Bloque 3 debe validarse con
  una muestra aleatoria de visitas antes de prometer la mejora como resultado.
