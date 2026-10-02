# LANDING_PAGE_A-B_EXPERIMENT

Análisis de un experimento A/B realizado sobre dos versiones de una landing page (**A** y **B**), con el objetivo de determinar cuál versión genera mejores resultados de negocio y respaldar esa decisión con evidencia estadística.

## Dataset

El archivo `landing_experiment.csv` contiene el registro de usuarios expuestos a cada versión de la página durante el experimento.

| Columna | Descripción |
|---|---|
| `user_id` | Identificador único del usuario |
| `date` | Fecha en la que el usuario fue expuesto a la página |
| `landing` | Versión de la página mostrada (A o B) |
| `region` | Región geográfica del usuario |
| `dispositivo` | Tipo de dispositivo utilizado |
| `traffic_source` | Canal por el que llegó el usuario |
| `user_type` | Tipo de usuario según su historial previo |
| `converted` | Indica si el usuario realizó una conversión |
| `gasto` | Monto gastado por el usuario (0 si no convirtió) |

El dataset no tiene valores ausentes; el único ajuste de tipos necesario es convertir `date` de texto a formato fecha.

## Estructura del notebook

**Paso 1 — Cargar y validar los datos**
Carga del CSV, revisión de tipos de datos, conversión de `date` a `datetime`, y validación de que las variables clave (usuarios únicos, rango de fechas, categorías de `landing`, `traffic_source`, `user_type`) tengan los valores esperados sin inconsistencias.

**Paso 2 — Gasto promedio por versión (A vs. B)**
Prueba t de Student sobre el gasto de los usuarios que sí convirtieron, para saber si una versión genera más valor económico por cliente que la otra.
- H₀: el gasto promedio es igual en ambas páginas
- H₁: el gasto promedio es diferente

**Paso 3 — Tasa de conversión entre A y B**
Prueba z sobre proporciones, para saber si una versión convierte más usuarios que la otra.
- H₀: la tasa de conversión es igual en ambas páginas
- H₁: la tasa de conversión es diferente

**Paso 4 — Relación entre fuente de tráfico y conversión**
Prueba chi-cuadrado de independencia entre `traffic_source` y `converted`, para identificar si algunos canales convierten mejor que otros.

**Paso 5 — Relación entre tipo de usuario y conversión**
Misma prueba chi-cuadrado, ahora entre `user_type` y `converted`, para ver si usuarios nuevos y recurrentes convierten de forma distinta.

**Paso 6 — Visualizaciones**
Gráficos de barras (absolutos) y de barras apiladas (proporciones) que respaldan visualmente los resultados de los Pasos 4 y 5.

**Paso 7 — Insight ejecutivo**
Resumen de hallazgos traducido a lenguaje de negocio, con recomendaciones accionables para stakeholders.

## Principales hallazgos

| Prueba | Resultado | Decisión |
|---|---|---|
| Gasto promedio (t-test) | A: $61.08 — B: $68.74 | Se rechaza H₀: diferencia significativa |
| Tasa de conversión (z-test) | A: 12.57% — B: 15.96% | Se rechaza H₀: diferencia significativa |
| Fuente de tráfico (chi²) | Email con la mayor tasa de conversión | Se rechaza H₀: sí hay relación |
| Tipo de usuario (chi²) | Usuarios nuevos convierten más que recurrentes | No se rechaza H₀: sin relación significativa |

**Recomendación principal:** implementar la versión B como página principal, por su mayor tasa de conversión (+3.4 puntos porcentuales, ~27% de mejora relativa) y mayor gasto promedio por usuario convertido, monitoreando el desempeño tras el cambio. Se sugiere además reforzar las campañas del canal Email, aunque su ventaja sobre los demás canales es modesta (entre 13.8% y 15.0% de conversión según el canal).

## Notas de revisión

- Las celdas de conclusión (Paso 3 y Paso 7) escriben la tasa de conversión de la página A como **0.12%**; el cálculo real en el código (2,512 / 19,982) da **12.57%**. Es un error de transcripción al redactar la conclusión, no un problema del cálculo ni del experimento: la prueba estadística en sí usa los números correctos.
- Las gráficas de barras apiladas (Paso 6) etiquetan el eje Y como "Cantidad de usuarios", pero los datos están normalizados a porcentaje; debería decir "% de usuarios".
- La diferencia de conversión entre canales de tráfico (13.8%–15.0%) es estadísticamente significativa por el tamaño de muestra, pero su magnitud es modesta; vale la pena matizar esa conclusión frente a la diferencia entre páginas A/B, que sí es grande.

## Cómo ejecutar

1. Instalar dependencias: `pandas`, `numpy`, `scipy`, `matplotlib`, `seaborn`.
2. Colocar `landing_experiment.csv` en el mismo directorio que el notebook.
3. Ejecutar las celdas en orden (Kernel → Restart & Run All).
