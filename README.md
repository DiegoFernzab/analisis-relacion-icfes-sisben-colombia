# Análisis ICFES y Factores Socioeconómicos en Colombia

---

## 1. Descripción

Este proyecto analiza la relación entre los resultados de las pruebas ICFES Saber 11 y factores socioeconómicos, educativos y de conectividad, utilizando datos abiertos del ICFES, SISBÉN y el Ministerio de Educación (MEN).

El propósito es identificar patrones y brechas territoriales que permitan orientar la toma de decisiones en política pública educativa.

---

## 2. Objetivo del proyecto

Identificar y cuantificar la relación entre los resultados de las pruebas Saber 11 y variables como:

- Condiciones socioeconómicas  
- Acceso a internet y tecnología  
- Indicadores educativos municipales  

### Municipios de estudio

- Bogotá D.C.  
- Sogamoso  
- Bugalagrande  

---

## 3. Contexto del problema

El rendimiento académico no depende únicamente del estudiante, sino también de su entorno.

### Factores relevantes

- Diferencias en conectividad  
- Condiciones de pobreza  
- Calidad del sistema educativo  

### Ejemplo de contraste territorial

| Municipio      | Características principales |
|---------------|---------------------------|
| Bogotá        | Alto desempeño, alta conectividad |
| Sogamoso      | Nivel intermedio |
| Bugalagrande  | Mayor vulnerabilidad, menor desempeño |

---

## 4. Estructura del repositorio

```bash
proyecto-icfes-analisis
│
├── analisis.ipynb
├── entrega1_presentacion.pdf
├── Proyecto-Procesamiento1.pdf
│
├── data/
│
└── README.md
```

---

## 5. Análisis Exploratorio de Datos (EDA)

Se realizó un análisis exploratorio para comprender la estructura, distribución y relaciones entre variables.

### 5.1 Distribuciones

- Histograma del puntaje global  
- Distribución unimodal con dispersión  
- Presencia de valores atípicos  

### 5.2 Comparación por municipio

- Bogotá presenta el mayor puntaje promedio  
- Sogamoso muestra comportamiento intermedio  
- Bugalagrande presenta menor desempeño  

### 5.3 Análisis temporal

- Evolución del puntaje global por periodo  
- Bogotá mantiene niveles superiores  
- Bugalagrande presenta mayor variabilidad  

### 5.4 Análisis por área

Se evaluaron:

- Matemáticas  
- Lectura Crítica  
- Inglés  

**Resultados:**

- Lectura Crítica presenta mejores promedios  
- Inglés muestra mayor variabilidad  
- Bugalagrande tiene menor desempeño en todas las áreas  

### 5.5 Variables socioeconómicas analizadas

- Estrato de vivienda  
- Acceso a internet  
- Acceso a computador  
- Clasificación SISBÉN  
- Hacinamiento (H_5)  
- Índices I1 – I4  

### 5.6 Hallazgos principales

- Existe relación entre estrato socioeconómico y puntaje  
- El acceso a internet se asocia con mejores resultados  
- Mayor cobertura educativa implica mejores puntajes  
- Factores socioeconómicos influyen directamente en el desempeño académico  

---

## 6. Visualizaciones incluidas

El proyecto incluye:

- Histogramas  
- Series de tiempo  
- Gráficos de barras  
- Boxplots  
- Diagramas de dispersión  
- Matriz de correlación  

---

## 7. Metodología

Se sigue la metodología CRISP-DM:

1. Entendimiento del negocio  
2. Entendimiento de los datos  
3. Preparación de datos  
4. Exploración y análisis  
5. Modelado (etapas posteriores)  
6. Evaluación  

---

## 8. Tecnologías utilizadas

- Python  
- Pandas  
- Matplotlib  
- Jupyter Notebook  
- Apache Spark (etapas posteriores)  

---

## 9. Fuentes de datos

- ICFES Saber 11 (Datos abiertos)  
- SISBÉN (DNP)  
- Ministerio de Educación Nacional (MEN)  

---

## 10. Ejecución del proyecto

### Clonar repositorio

```bash
git clone https://github.com/tu-usuario/tu-repo.git
```

### Instalar dependencias

```bash
pip install pandas matplotlib notebook
```

### Ejecutar

```bash
jupyter notebook
```

### Abrir

```bash
analisis.ipynb
```

---

## 11. Conclusiones preliminares

- Las brechas educativas tienen un componente territorial importante  
- La conectividad es un factor clave en el desempeño académico  
- El nivel socioeconómico influye significativamente en los resultados  
- Existen diferencias entre áreas del conocimiento  

---

## 12. Autores

- Juan Santiago Méndez  
- Juan Martín Trejos  
- Diego Zabala  
- Juan David Ordoñez  

---

## 13. Notas

Proyecto académico desarrollado en la Pontificia Universidad Javeriana, en el curso de Procesamiento de Datos, dirigida por el profesor JOHN CORREDOR FRANCO
