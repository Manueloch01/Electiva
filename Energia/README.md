# Facturación residencial de energía eléctrica 2023–2024

Trabajo de la electiva: limpieza, análisis exploratorio, agrupamiento y modelos supervisados
sobre el archivo de facturación residencial.

## Contenido

| Archivo | Cubre |
|---|---|
| `01_limpieza_y_exploratorio.ipynb` | Fases 3.1 a 3.4 y problemáticas 4.1 y 4.2 |
| `02_clustering.ipynb` | Fases 3.5 y 3.6 y problemáticas 4.3 y 4.4 |
| `03_modelos_supervisados.ipynb` | Fases 3.7 y 3.8 y problemáticas 4.5 y 4.6 |
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
