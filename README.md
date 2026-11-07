# Laboratorio S6: Preparación de Datos y Benchmarking (CSV vs Parquet)

**Materia:** Fundamentos de Big Data (IDSD413) – Escuela Politécnica Nacional  
**Profesor:** David Esteban Galarza García  
**Estudiante:** JAMES CAIÑO

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Jameslyo69/IDSD413-S6-CSV-vs-Parquet/blob/main/S6_Preparacion_Datos_Benchmarking.ipynb)

## Objetivo
Manipular y extender el script de exploración en Google Colab para evaluar de forma empírica el impacto del cuello de botella de I/O y del filtrado avanzado en disco (*Predicate Pushdown*) entre un formato basado en filas (CSV) y un formato columnar (Apache Parquet).

## Contenido del repositorio
| Archivo | Descripción |
|---|---|
| `S6_Preparacion_Datos_Benchmarking.ipynb` | Notebook con la solución de ambos retos, sus salidas y la documentación de cada celda. |
| `README.md` | Descripción del laboratorio, resultados y trazabilidad del desarrollo. |

> Los archivos `transacciones_epn.csv` (740.53 MB) y `transacciones_epn.parquet` (225.39 MB) no se incluyen en el repositorio por su tamaño; el notebook los genera al ejecutarse.

## Retos desarrollados

### Reto 1: Test de estrés de memoria
Se elevó el volumen del dataset sintético de 5 a **15 millones de registros** y se ejecutó el flujo celda por celda, registrando con el panel de recursos de Colab el tiempo y la RAM (antes, pico y después) de cada operación, en comparación con una línea base de 5M.

### Reto 2: Consulta analítica avanzada
Se extendió el código de filtrado para obtener únicamente el `monto` de las transacciones de la categoría **"Juguetes"** con estado **"Completado"**, aplicando múltiples filtros a nivel de disco (*Predicate Pushdown*) y proyección de columnas en Parquet, frente a la carga completa del CSV.

## Resultados principales (15M registros)
| Operación | CSV (s) | Parquet (s) | Aceleración |
|---|---|---|---|
| Escritura | 76.31 | 6.92 | 11.03x |
| Lectura completa | 11.61 | 2.91 | 3.99x |
| Consulta selectiva (`monto`) | 8.60 | 0.26 | 33.41x |
| Filtro `Electronica` | 12.91 | 2.01 | 6.42x |
| Group by por `estado` | 12.95 | 2.61 | 4.96x |
| **Reto 2:** `Juguetes` + `Completado` | 14.10 | 1.44 | **9.82x** |

- **Almacenamiento:** Parquet ocupa 225.39 MB frente a 740.53 MB del CSV (**69.56 % menos**).
- **Memoria:** la RAM pasó de 2.0–2.3 GB (5M) a 3.3–3.9 GB (15M), con un pico máximo de 3.9 GB; el entorno no colapsó.
- **Validación del Reto 2:** ambos formatos devolvieron las mismas **750,894 filas** y la misma suma (**$939,863,458.14**), con 1 columna cargada en Parquet frente a 5 en CSV.
## Ejecucion
1. Abrir el notebook con el botón **Open in Colab**.
2. Abrir el panel **Ver recursos** (indicador RAM/Disco, arriba a la derecha).
3. Ejecutar las celdas en orden, una por una, para observar el consumo de memoria de cada operación.

## historial de commits
| Etapa | Descripción |
|---|---|
| 1 | Notebook base del laboratorio (5M registros) |
| 2 | Línea base de tiempo y RAM con 5M |
| 3–8 | Reto 1: documentación de las celdas 1 a 6 con 15M |
| 9 | Reto 1: resumen comparativo y conclusiones |
| 10–11 | Reto 2: consulta con predicate pushdown múltiple y su análisis |
| 12 | Conclusiones generales |
| 13 | Documentación del repositorio (README) |

Historial completo: [ver commits](https://github.com/TU-USUARIO/IDSD413-S6-CSV-vs-Parquet/commits/main)

## Tecnologías
Python 3 · pandas · NumPy · PyArrow · Google Colab
