Análisis ICFES y Factores Socioeconómicos en Colombia
Descripción

Este proyecto analiza la relación entre los resultados de las pruebas ICFES Saber 11 y factores socioeconómicos, educativos y de conectividad, utilizando datos abiertos del ICFES, SISBÉN y el Ministerio de Educación (MEN).

El objetivo es identificar patrones y brechas territoriales que permitan orientar la toma de decisiones en política pública educativa.

Objetivo del proyecto

Identificar y cuantificar la relación entre los resultados de las pruebas Saber 11 y variables como:

Condiciones socioeconómicas
Acceso a internet y tecnología
Indicadores educativos municipales

Con énfasis en los municipios:

Bogotá D.C.
Sogamoso
Bugalagrande

Tal como se plantea en el documento del proyecto .

Contexto del problema

Los resultados académicos no dependen únicamente del estudiante, sino del entorno en el que se desarrolla:

Diferencias en conectividad
Condiciones de pobreza
Calidad del sistema educativo

Por ejemplo:

Bogotá presenta alto desempeño y mayor conectividad
Sogamoso presenta un nivel intermedio
Bugalagrande refleja mayor vulnerabilidad

Esto se evidencia en el análisis del documento de investigación .

Estructura del repositorio
proyecto-icfes-analisis
│
├── analisis.ipynb
├── entrega1_presentacion.pdf
├── Proyecto-Procesamiento1.pdf
│
├── data/
├── outputs/
│
└── README.md
Análisis Exploratorio de Datos

Se realizó un análisis exploratorio para entender la estructura, distribución y relaciones entre variables.

1. Distribuciones
Histograma del puntaje global
Distribución aproximadamente unimodal con dispersión y presencia de valores atípicos
2. Comparación por municipio
Bogotá presenta el mayor puntaje promedio
Sogamoso comportamiento intermedio
Bugalagrande menor desempeño

Este resultado coincide con lo documentado en la entrega .

3. Análisis temporal
Evolución del puntaje global por periodo
Bogotá mantiene los niveles más altos en la mayoría de los periodos
Bugalagrande presenta mayor variabilidad
4. Análisis por área

Se compararon Matemáticas, Lectura Crítica e Inglés:

Lectura Crítica presenta promedios ligeramente superiores
Inglés muestra mayor variabilidad
Bugalagrande se mantiene por debajo en todas las áreas
5. Variables socioeconómicas analizadas
Estrato de vivienda
Acceso a internet
Acceso a computador
Clasificación SISBÉN
Hacinamiento (H_5)
Índices I1–I4
6. Hallazgos principales
Existe relación entre estrato socioeconómico y puntaje
El acceso a internet se asocia con mejores resultados
Los municipios con mejor cobertura educativa presentan mejores puntajes
Los factores socioeconómicos influyen directamente en el desempeño académico
Visualizaciones incluidas

El análisis incluye:

Histogramas de distribución
Series de tiempo
Gráficos de barras
Boxplots
Diagramas de dispersión
Matriz de correlación

Tal como se plantea en la sección de exploración de datos .

Metodología

Se sigue la metodología CRISP-DM:

Entendimiento del negocio
Entendimiento de los datos
Preparación de datos
Exploración y análisis
Modelado (etapas posteriores)
Evaluación
Tecnologías utilizadas
Python
Pandas
Matplotlib
Jupyter Notebook
Apache Spark (en etapas posteriores)
Fuentes de datos
ICFES Saber 11 (Datos Abiertos)
SISBÉN (DNP)
MEN - Educación (SIMAT)
Ejecución del proyecto
Clonar el repositorio:
git clone https://github.com/tu-usuario/tu-repo.git
Instalar dependencias:
pip install pandas matplotlib notebook
Ejecutar Jupyter Notebook:
jupyter notebook
Abrir el archivo:
analisis.ipynb
Conclusiones preliminares
Las brechas educativas tienen un componente territorial importante
La conectividad es un factor clave en el desempeño académico
El nivel socioeconómico influye significativamente en los resultados
Existen diferencias entre áreas del conocimiento que deben considerarse en intervenciones
Autores
Juan Santiago Méndez
Juan Martín Trejos
Diego Zabala
Juan David Ordoñez
Notas

Proyecto académico desarrollado en la Pontificia Universidad Javeriana, en el marco del curso de Procesamiento de Datos a Gran Escala.
