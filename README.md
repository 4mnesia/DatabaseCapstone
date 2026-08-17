# DatabaseCapstone — Dataset SINADER (Chile, 2014–2024)

Repositorio de datasets del **Sistema Nacional de Declaración de Residuos (SINADER)** de Chile, utilizado como base para un **proyecto de capstone** enfocado en el análisis de residuos industriales y municipales.

---

## ¿Qué es SINADER?

**SINADER** (Sistema Nacional de Declaración de Residuos) es la plataforma oficial del **Ministerio del Medio Ambiente de Chile (MMA)** en la que los establecimientos generadores, receptores y gestores de residuos declaran anualmente:

- **Qué** residuos se generan (tipo, categoría, código LER).
- **Cuánto** se genera (toneladas).
- **Dónde** se genera y a dónde va (trazabilidad geográfica).
- **Cómo** se maneja (eliminación, valorización, reciclaje, etc.).

Forma parte del **RETC** (Registro de Emisiones y Transferencia de Contaminantes), sistema público que centraliza información ambiental de todo el país.

**Fuente oficial:** <https://datosretc.mma.gob.cl/dataset/residuos>

---

## Contenido del repositorio

11 archivos CSV, uno por año, cubriendo el período **2014–2024**.

| Año  | Archivo                              | Filas   |
|------|--------------------------------------|---------|
| 2014 | `gi-sinader-2014-ckan(Datos).csv`    | 8.663   |
| 2015 | `gi-sinader-2015-ckan(Datos).csv`    | 16.198  |
| 2016 | `gi-sinader-2016-ckan(Datos).csv`    | 45.216  |
| 2017 | `gi-sinader-2017-ckan(Datos).csv`    | 52.576  |
| 2018 | `gi-sinader-2018-ckan(Datos).csv`    | 80.446  |
| 2019 | `gi-sinader-2019-ckan(Datos).csv`    | 31.230  |
| 2020 | `gi-sinader-2020-ckan(Datos).csv`    | 23.942  |
| 2021 | `gi-sinader-2021-ckan(Datos).csv`    | 34.197  |
| 2022 | `gi-sinader-2022-ckan(Datos).csv`    | 34.580  |
| 2023 | `gi-sinader-2023-ckan(Datos).csv`    | 41.611  |
| 2024 | `gi-sinader-2024-ckan(Datos).csv`    | 241.688 |

**Total aprox.:** ~610.000 declaraciones · ~482 MB · almacenados con **Git LFS**.

**Formato:** CSV con separador `;` y codificación UTF-8.

---

## Estructura de los datos

Cada fila representa una **declaración de residuos** por establecimiento, mes y tipo de residuo. Las columnas se agrupan así:

### Identificación temporal
| Columna | Descripción |
|---|---|
| `año`, `mes` | Período de la declaración |

### Identificación del establecimiento
| Columna | Descripción |
|---|---|
| `id_vu` | ID único de la Ventanilla Única |
| `razon_social`, `rut_razon_social` | Empresa declarante |
| `nombre_establecimiento` | Nombre de la instalación |
| `rol_establecimiento` | Rol (generador industrial, receptor, transportista, etc.) |

### Clasificación económica (CIIU)
| Columna | Descripción |
|---|---|
| `rubro`, `rubro_id` | Sector económico (minería, agricultura, industria, etc.) |
| `ciiu4`, `ciiu4_id` | Clasificación CIIU nivel 4 |
| `ciiu6`, `ciiu6_id` | Clasificación CIIU nivel 6 (detallada) |

### Geolocalización
| Columna | Descripción |
|---|---|
| `region`, `region_id` | Región de Chile |
| `provincia`, `comuna` | Subdivisiones administrativas |
| `codigo_unico_territorial` | Código territorial |
| `latitud`, `longitud` | Coordenadas del establecimiento |

### Datos del residuo
| Columna | Descripción |
|---|---|
| `cantidad_toneladas` | **Métrica principal**: cantidad declarada en toneladas |
| `tipo_recepcion` | Tipo de recepción (solo para receptores) |
| `ler`, `ler_numero` | Código y número LER (Lista Europea de Residuos) |
| `ler_capitulo`, `ler_subcapitulo` | Jerarquía LER |

### Tratamiento aplicado (jerárquico)
| Columna | Descripción |
|---|---|
| `tratamiento_n1_name` | Nivel 1: Eliminación / Valorización |
| `tratamiento_n2_name` | Nivel 2: Reciclaje / Coprocesamiento / Relleno / etc. |
| `tratamiento_n3_name` | Nivel 3: detalle específico (ej. "Reciclaje de metales") |

### Trazabilidad (destino)
| Columna | Descripción |
|---|---|
| `trazabilidad__id_vu` | ID del establecimiento receptor |
| `trazabilidad__nombre_establecimiento` | Nombre del receptor |
| `trazabilidad__razon_social` | Empresa receptora |
| `trazabilidad__comuna`, `trazabilidad__region`, `trazabilidad__pais` | Destino geográfico |

---

## ¿Qué se puede hacer con estos datos?

Este dataset habilita análisis a nivel país, sector, empresa e incluso instalación. Algunas líneas de trabajo:

### Análisis descriptivo
- Evolución interanual de generación de residuos por región, rubro o empresa.
- Ranking de sectores/empresas con mayor generación.
- Distribución geográfica (mapas de calor de generación por comuna).
- Composición de residuos según código LER.

### Modelado y machine learning
- **Clustering** de establecimientos por perfil de residuos (K-Means, DBSCAN).
- **Clasificación** del tipo de tratamiento probable dado el rubro y tipo de residuo.
- **Detección de anomalías** en declaraciones (outliers en toneladas por sector).
- **Series de tiempo**: forecasting de generación futura (ARIMA, Prophet, LSTM).
- **Análisis de redes**: grafo generador → receptor para estudiar cadenas de valorización.

### Aplicaciones para empresas
- **Benchmarking**: comparar la huella de residuos vs. competidores del mismo rubro/región.
- **Compliance ambiental**: verificar consistencia de declaraciones y trazabilidad.
- **Economía circular**: identificar oportunidades de valorización (residuos de A que pueden ser insumo de B).
- **Optimización logística**: analizar flujos generador–receptor para reducir distancias de transporte.

### Políticas públicas
- Evaluación del cumplimiento de la **Ley REP** (Responsabilidad Extendida del Productor).
- Medición de avance hacia metas de reciclaje nacional.
- Identificación de zonas de sacrificio o concentración de residuos peligrosos.

---

## Uso del repositorio

Los CSVs están almacenados con **Git LFS**, así que necesitas tenerlo instalado antes de clonar.

```bash
# 1. Instalar Git LFS (si no lo tienes)
#    Windows: descargar desde https://git-lfs.com
#    macOS:   brew install git-lfs
#    Linux:   sudo apt install git-lfs

# 2. Inicializar LFS en tu equipo (una sola vez)
git lfs install

# 3. Clonar el repositorio (descarga automática de los CSVs)
git clone <URL_DEL_REPOSITORIO>.git
```

### Ejemplo de carga en Python (pandas)

```python
import pandas as pd

df = pd.read_csv(
    "gi-sinader-2024-ckan(Datos).csv",
    sep=";",
    encoding="utf-8",
    low_memory=False,
)
print(df.shape)
print(df.columns.tolist())
```

---

## Uso en Jupyter Notebook

A continuación un flujo listo para copiar/pegar en un notebook. Está pensado para cargar los **11 CSVs**, unificarlos en un solo `DataFrame` y dejarlo listo para análisis exploratorio.

### Celda 1 — Imports y configuración

```python
import glob
import os
from pathlib import Path

import pandas as pd

# Ajusta esta ruta a la carpeta donde clonaste el repo
# Ejemplos:
#   Windows: Path(r"C:\ruta\a\DatabaseCapstone")
#   macOS/Linux: Path("/ruta/a/DatabaseCapstone")
DATA_DIR = Path("./")  # por defecto: directorio actual del notebook

# Configuración de visualización de pandas
pd.set_option("display.max_columns", 100)
pd.set_option("display.width", 200)
pd.set_option("display.float_format", "{:,.2f}".format)
```

### Celda 2 — Cargar todos los CSVs en un solo DataFrame

```python
def cargar_sinader(data_dir: Path) -> pd.DataFrame:
    """
    Lee todos los CSVs de SINADER (2014-2024) y los concatena en un único DataFrame.
    Añade la columna `archivo_origen` para trazabilidad.
    """
    archivos = sorted(data_dir.glob("gi-sinader-*.csv"))
    if not archivos:
        raise FileNotFoundError(f"No se encontraron CSVs en {data_dir.resolve()}")

    dataframes = []
    for archivo in archivos:
        print(f"Leyendo {archivo.name} ...")
        df_año = pd.read_csv(
            archivo,
            sep=";",
            encoding="utf-8",
            low_memory=False,
        )
        df_año["archivo_origen"] = archivo.name
        dataframes.append(df_año)

    df = pd.concat(dataframes, ignore_index=True)
    print(f"\nTotal declaraciones cargadas: {len(df):,}")
    print(f"Rango de años: {df['año'].min()} – {df['año'].max()}")
    return df


df = cargar_sinader(DATA_DIR)
df.head()
```

### Celda 3 — Sanity checks rápidos

```python
# Dimensiones y memoria
print(f"Filas: {len(df):,}")
print(f"Columnas: {df.shape[1]}")
print(f"Memoria: {df.memory_usage(deep=True).sum() / 1024**2:,.1f} MB")

# Valores nulos por columna (top 15)
print("\nNulos por columna (top 15):")
print(df.isna().sum().sort_values(ascending=False).head(15))

# Declaraciones por año
print("\nDeclaraciones por año:")
print(df.groupby("año").size())
```

### Celda 4 — Vistazo general por región y rubro

```python
# Top 10 regiones por toneladas declaradas
top_regiones = (
    df.groupby("region")["cantidad_toneladas"]
      .sum()
      .sort_values(ascending=False)
      .head(10)
)
print("Top 10 regiones por toneladas totales (2014-2024):")
print(top_regiones)

# Top 10 rubros por toneladas declaradas
top_rubros = (
    df.groupby("rubro")["cantidad_toneladas"]
      .sum()
      .sort_values(ascending=False)
      .head(10)
)
print("\nTop 10 rubros por toneladas totales (2014-2024):")
print(top_rubros)
```

### Celda 5 (opcional) — Guardar en Parquet para reuso rápido

Cargar los 11 CSVs cada vez es lento. Guarda el DataFrame unificado en **Parquet** y en las siguientes sesiones cárgalo de una sola vez (≈10× más rápido).

```python
# Guardar una sola vez
df.to_parquet(DATA_DIR / "sinader_2014_2024.parquet", index=False)

# En próximas sesiones basta con:
# df = pd.read_parquet(DATA_DIR / "sinader_2014_2024.parquet")
```

> Requisito: `pip install pyarrow` (o `fastparquet`).

### Consejo — Cargar solo un año o columnas específicas

Si el DataFrame completo consume demasiada memoria, puedes filtrar en la lectura:

```python
# Solo un año
df_2024 = pd.read_csv(
    DATA_DIR / "gi-sinader-2024-ckan(Datos).csv",
    sep=";",
    encoding="utf-8",
    low_memory=False,
)

# Solo columnas relevantes (más rápido y menos memoria)
columnas = [
    "año", "mes", "region", "comuna", "rubro",
    "cantidad_toneladas", "tratamiento_n1_name",
    "ler", "latitud", "longitud",
]
df_liviano = pd.concat([
    pd.read_csv(f, sep=";", encoding="utf-8", usecols=columnas, low_memory=False)
    for f in sorted(DATA_DIR.glob("gi-sinader-*.csv"))
], ignore_index=True)
```

---

## Licencia y atribución

Los datos son publicados por el **Ministerio del Medio Ambiente de Chile** bajo el marco de datos abiertos del Estado. Al utilizarlos, cita la fuente:

> Ministerio del Medio Ambiente de Chile — RETC / SINADER.
> Disponible en: <https://datosretc.mma.gob.cl/dataset/residuos>
