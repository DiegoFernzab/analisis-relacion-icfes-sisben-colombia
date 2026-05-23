# Análisis de Factores Socioeconómicos y Conectividad en los Resultados del ICFES Saber 11

**Pontificia Universidad Javeriana — Facultad de Ingeniería — Ingeniería de Sistemas** 
**Curso:** Procesamiento Alto Volumen de Datos 
**Docente:** John Corredor Franco, PhD 
**Equipo:** Juan Santiago Méndez · Juan Martín Trejos · Diego Zabala · Juan David Ordoñez

---

## Descripción del Proyecto

Este proyecto analiza la relación entre los resultados de las pruebas ICFES Saber 11 y los factores socioeconómicos, educativos y de conectividad en los municipios de **Sogamoso (Boyacá)** y **Pitalito (Huila)**. A partir de datos abiertos del ICFES, el SISBÉN y el Ministerio de Educación Nacional (MEN), se construyeron modelos de clasificación supervisada que permiten predecir el nivel de desempeño de un estudiante y orientar intervenciones de política pública educativa focalizadas.

El proyecto sigue la metodología **CRISP-DM** y está dividido en dos entregas. Se desarrollaron dos versiones del pipeline analítico para verificar la hipótesis del techo de información de los datos.

---

## Objetivo de Negocio

> Identificar y cuantificar la relación entre los resultados de las pruebas Saber 11 y los factores socioeconómicos, de conectividad y de trayectoria educativa a nivel municipal, con énfasis en Sogamoso y Pitalito, con el fin de orientar la priorización de intervenciones del Ministerio de Educación.

---

## Municipios de Estudio

| Municipio | Código DANE | Departamento | Perfil |
|-----------|-------------|--------------|--------|
| **Sogamoso** | 15759 | Boyacá | Ciudad intermedia industrial. NBI 3.59%, ~131k hab. Conectividad media. |
| **Pitalito** | 41551 | Huila | Ciudad intermedia agroindustrial. Perfil más vulnerable. Menor conectividad. |

Se seleccionaron estos dos municipios por tener **perfiles socioeconómicos similares en distinta región geográfica**, lo que permite que el modelo aprenda patrones generalizables y no patrones regionales específicos de Boyacá.

---

## Estructura del Repositorio

```
analisis-relacion-icfes-sisben-colombia/
│
├── ProyectoFinal_v1.ipynb               # Pipeline completo v1 (kgxf-xxbe + SISBEN + MEN)
├── ProyectoICFES_v2_DataIcfes.ipynb     # Pipeline v2 (DataIcfes completo, ~84 columnas)
│
├── Entrega2_BigData_ICFES.docx          # Documento completo Entregas 1 y 2
├── Informe_Avance_v1_ICFES.docx         # Informe de avance y transicion v1 → v2
│
├── data/
│   ├── icfes/                           # Archivos kgxf-xxbe filtrados (v1)
│   ├── dataicfes/
│   │   ├── Boyaca/                      # Archivos .txt DataIcfes por semestre
│   │   └── Huila/                       # Archivos .txt DataIcfes por semestre
│   ├── sisben/                          # Dataset SISBEN personas 12-17 años
│   └── men/                             # Dataset MEN estadisticas por municipio
│
└── README.md
```

---

## Versiones del Pipeline

### Versión 1 `ProyectoFinal_v1.ipynb`
Pipeline construido con el dataset `kgxf-xxbe` de datos.gov.co.

**Datasets:**
- ICFES kgxf-xxbe (~19 columnas por estudiante)
- SISBÉN personas filtrado 1217 años (913 registros totales)
- MEN estadísticas por municipio

**Features del modelo (~16):** variables del hogar (estrato, educación padres, internet, computador), institucionales (colegio oficial/rural), indicadores SISBÉN agregados por segmento, variables del MEN, y 3 variables derivadas (capital_educativo, recursos_tech, ventaja_hogar).

**Arquitectura:**
```
SECCIÓN 0 Setup e importaciones globales
SECCIÓN 1 Dataset ICFES: carga, selección, encoding, nulos, distribuciones, limpieza
SECCIÓN 2 Dataset SISBEN: carga, filtro edad, encoding, agregación por municipio+zona+grupo
SECCIÓN 3 Dataset MEN: carga, limpieza de strings "%", preparación
SECCIÓN 4 Joins (ICFES + SISBEN + MEN) y construcción del target (terciles P33/P66)
SECCIÓN 5 Feature engineering, correlaciones, eliminación multicolinealidad, normalización
SECCIÓN 6 Modelos supervisados: RF, XGBoost, LightGBM + accuracy adyacente
+ Clasificador binario riesgo crítico (P10)
SECCIÓN 7 Bono: Red Neuronal MLP
```

**Resultados v1:**

| Modelo | Dataset | Accuracy | F1 macro | Acc. Adyacente |
|--------|---------|----------|----------|----------------|
| RF tuning | Completo | 0.4675 | 0.4620 | 0.8665 |
| XGBoost base | Completo | 0.4646 | 0.4608 | **0.8727** |
| LightGBM base | Completo | 0.4659 | 0.4607 | 0.8698 |
| RF tuning | Solo ICFES | 0.4654 | 0.4554 | 0.8553 |

---

### Versión 2 `ProyectoICFES_v2_DataIcfes.ipynb`
Pipeline construido con los microdatos completos de **DataIcfes** (archivos `.txt` directos del ICFES por departamento).

**Hipótesis a verificar:**
> *El techo de F1 macro ~0.46 del v1 se debía a la limitada información de kgxf-xxbe, no al algoritmo. Con ~28 variables adicionales por estudiante (incluyendo estu_inse_individual, fami_numlibros, cole_caracter, cole_jornada y variables de hábitos), el modelo debería superar ese techo.*

**Variables nuevas vs v1:**

| Variable | Descripción | Impacto esperado |
|----------|-------------|-----------------|
| `estu_inse_individual` | Índice NSE continuo por estudiante | Muy alto reemplaza join proxy SISBÉN |
| `estu_nse_establecimiento` | NSE promedio del colegio (14) | Alto |
| `fami_numlibros` | Número de libros en el hogar (proxy capital cultural) | Alto |
| `fami_cuartoshogar` | Cuartos en el hogar (proxy hacinamiento) | Alto |
| `cole_caracter` | Carácter del colegio (académico / técnico) | Alto |
| `cole_jornada` | Jornada escolar (mañana / tarde / completa) | Medio |
| `fami_tieneautomovil` + `lavadora` + `serviciotv` | Bienes del hogar `bienes_hogar` derivada | Alto |
| `estu_dedicacioninternet` | Horas semanales de uso de internet | Medio |
| `estu_dedicacionlecturadiaria` | Hábito lector diario | Medio |
| `estu_horassemanatrabaja` | Si trabaja y cuántas horas | Medio |
| `fami_situacioneconomica` | Percepción situación económica del hogar | Medio |

**Arquitectura:**
```
SECCIÓN 0 Setup e importaciones globales
SECCIÓN 1 Carga DataIcfes: loop sobre archivos .txt por departamento (Boyacá + Huila)
SECCIÓN 2 Exploración inicial
SECCIÓN 3 Encoding completo (~28 variables), imputación de nulos
SECCIÓN 4 Target (terciles), feature engineering (6 variables derivadas), correlaciones
SECCIÓN 5 Normalización, split temporal train/test
SECCIÓN 6 Modelos supervisados: RF, XGBoost, LightGBM + verificación de hipótesis
+ Clasificador binario riesgo crítico (P10) con análisis de umbrales
```

---

## Modelos Implementados

### Clasificación Multiclase (Supervisado)
**Target:** `categoria_punt` 3 categorías por terciles de PUNT_GLOBAL:

| Clase | Rango aprox. | Etiqueta |
|-------|-------------|----------|
| 0 | < P33 (~248 pts) | Bajo |
| 1 | P33 P66 (~248294 pts) | Medio |
| 2 | > P66 (~294 pts) | Alto |

**Algoritmos:** Random Forest · XGBoost · LightGBM · MLP (bono)

### Clasificador Binario de Riesgo Crítico
**Target:** estudiantes con PUNT_GLOBAL P10 (~207 puntos) 
**AUC-ROC:** 0.70 · Recall: 0.50 · Precision: 0.20


---

## Métricas Clave

### ¿Por qué accuracy adyacente?
El accuracy estándar penaliza igual un error de 1 nivel (BajoMedio) que uno de 2 niveles (BajoAlto). Para clases ordinales esto es incorrecto. La **accuracy adyacente** cuenta como acierto cualquier predicción a distancia 1 del valor real, siendo más honesta para este tipo de problema.

### Análisis de errores (mejor modelo v1 Test 2022):
```
Total estudiantes evaluados: 7,409
Exactamente correctos: 3,464 (46.8%) 
Error de 1 nivel: 2,956 (39.9%) (adyacente tolerable)
Error grave (2 niveles): 989 (13.3%) 
```

---

## Hallazgos Principales

1. **El capital educativo del hogar es el predictor dominante** (importancia Gini ~0.175), superando al acceso a internet (~0.054) y al estrato (~0.018). Las políticas de conectividad sin acompañamiento familiar tienen impacto limitado.

2. **Efecto multiplicador educación × tecnología**: `ventaja_hogar` (capital_educativo × recursos_tech) es el 4.º predictor. La tecnología amplifica su beneficio solo cuando hay capital educativo familiar.

3. **El tipo de colegio importa más que el internet** individual: `COLE_NATURALEZA` ocupa el 3.er lugar en importancia. La calidad institucional tiene un efecto estructural sobre el puntaje.

4. **Inglés es el área más sensible a la desigualdad territorial**: la brecha entre Sogamoso y Pitalito es sistemáticamente mayor en inglés que en Matemáticas y Lectura Crítica.

5. **SISBEN y MEN no mejoran el modelo** sobre Solo ICFES: con 2 municipios, las variables agregadas son prácticamente constantes dentro de cada municipio y son redundantes con el código del municipio.

6. **El techo no es del algoritmo**: todos los modelos (RF, XGBoost, LightGBM) convergen al mismo rango de métricas independientemente del tuning, lo que indica que el cuello de botella está en la información disponible, no en el algoritmo.

---

## Reproducción del Proyecto

### Requisitos

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost lightgbm
```

### Datos necesarios

**v1:**
- Descargar `kgxf-xxbe` desde [datos.gov.co](https://www.datos.gov.co/Educaci-n/Resultados-nicos-Saber-11/kgxf-xxbe/about_data) filtrado para Sogamoso (15759) y Pitalito (41551)
- SISBÉN personas: [hq2v-5umk](https://www.datos.gov.co/Estad-sticas-Nacionales/DNP-Sisb-n-Personas/hq2v-5umk/about_data)
- MEN estadísticas: [nudc-7mev](https://www.datos.gov.co/Educaci-n/MEN_ESTADISTICAS_EN_EDUCACION_EN_PREESCOLAR-B-SICA/nudc-7mev/data_preview)

**v2:**
- Archivos `.txt` DataIcfes por departamento descargados de [icfes.gov.co](https://www.icfes.gov.co) carpetas `Boyaca/` y `Huila/`

### Ejecución v1

```python
# En Google Colab editar rutas en Celda 0.2
ICFES_FILE = "ruta/al/icfes_sogamoso_pitalito.csv"
SISBEN_FILE = "ruta/al/sisben_personas.csv"
MEN_FILE = "ruta/al/men_estadisticas.csv"
```

### Ejecución v2

```python
# En Google Colab editar rutas en Celda 0.2
HUILA_PATH = Path("/content/sample_data/Huila") # carpeta con .txt de Huila
BOYACA_PATH = Path("/content/sample_data/Boyaca") # carpeta con .txt de Boyacá
```

---

## Fuentes de Datos

| Dataset | Fuente | URL |
|---------|--------|-----|
| ICFES Saber 11 (kgxf-xxbe) | ICFES / datos.gov.co | [Enlace](https://www.datos.gov.co/Educaci-n/Resultados-nicos-Saber-11/kgxf-xxbe/about_data) |
| DataIcfes completo | ICFES.gov.co | [Enlace](https://www.icfes.gov.co) |
| SISBÉN Personas | DNP / datos.gov.co | [Enlace](https://www.datos.gov.co/Estad-sticas-Nacionales/DNP-Sisb-n-Personas/hq2v-5umk/about_data) |
| MEN Estadísticas Educación | MEN / datos.gov.co | [Enlace](https://www.datos.gov.co/Educaci-n/MEN_ESTADISTICAS_EN_EDUCACION_EN_PREESCOLAR-B-SICA/nudc-7mev/data_preview) |
| Diccionario SISBÉN IV | DNP / ANDA | [Enlace](https://anda.dnp.gov.co/index.php/catalog/150/datafile/F18) |

---

## Tecnologías Utilizadas

| Herramienta | Uso |
|-------------|-----|
| Python 3.10 | Lenguaje principal |
| Pandas | Carga, limpieza y transformación de datos |
| NumPy | Operaciones numéricas |
| Matplotlib / Seaborn | Visualizaciones y EDA |
| Scikit-learn | Preprocesamiento, modelos y métricas |
| XGBoost | Clasificador Gradient Boosting |
| LightGBM | Clasificador Gradient Boosting rápido |
| Google Colab | Entorno de ejecución |

---

## Metodología CRISP-DM

```
1. Entendimiento del negocio Entrega 1, Sección 1
2. Entendimiento de los datos Entrega 1, Secciones 35
3. Preparación de datos Entrega 2, Secciones 1 y 4
4. Modelado Entrega 2, Sección 5
5. Evaluación Entrega 2, Sección 6
6. Despliegue Recomendaciones de política pública
```

---

## Autores

| Nombre | Rol |
|--------|-----|
| Juan Santiago Méndez | 
| Juan Martín Trejos | 
| Diego Zabala | 
| Juan David Ordoñez | 
---

## Notas Académicas

Proyecto académico desarrollado en la **Pontificia Universidad Javeriana**, curso de *Procesamiento de Datos de Alto Volumen*, dirigido por el profesor **John Corredor Franco, PhD**.

El proyecto explora la hipótesis de que el techo de rendimiento predictivo (~F1 macro 0.46) con datos públicos de datos.gov.co es superado al utilizar los microdatos completos de DataIcfes. Los resultados del Proyecto v2 verificarán o refutarán esta hipótesis con evidencia empírica.
