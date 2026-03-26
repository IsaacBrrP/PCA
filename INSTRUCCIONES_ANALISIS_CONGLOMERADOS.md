# Instrucciones para Análisis de Conglomerados de Estudiantes

Este documento proporciona instrucciones para ejecutar el análisis de perfiles de estudiantes mediante conglomerados jerárquicos y no jerárquicos.

## Archivo Principal

**`analisis_conglomerados_estudiantes.Rmd`** - Archivo R Markdown completo con el análisis de clustering

## Requisitos Previos

### Instalar R y RStudio

1. Descargar e instalar R desde: https://cran.r-project.org/
2. Descargar e instalar RStudio desde: https://posit.co/download/rstudio-desktop/

### Instalar Paquetes Necesarios

Ejecutar en la consola de R:

```r
# Instalar paquetes requeridos
install.packages("tidyverse")
install.packages("cluster")
install.packages("factoextra")
install.packages("knitr")
install.packages("kableExtra")
install.packages("mclust")  # Para el índice de Rand ajustado
```

### Instalar LaTeX (para generar PDF)

**Windows:**
```r
install.packages("tinytex")
tinytex::install_tinytex()
```

**Mac:**
- Descargar MacTeX desde: https://www.tug.org/mactex/

**Linux:**
```bash
sudo apt-get install texlive-full
```

## Estructura del Proyecto

```
PCA/
├── base_estudiantes_conglomerados.csv    # Dataset de entrada
├── analisis_conglomerados_estudiantes.Rmd # Análisis principal
└── INSTRUCCIONES_ANALISIS_CONGLOMERADOS.md # Este archivo
```

## Cómo Ejecutar el Análisis

### Opción 1: Desde RStudio (Recomendado)

1. Abrir RStudio
2. Abrir el archivo `analisis_conglomerados_estudiantes.Rmd`
3. Verificar que el archivo `base_estudiantes_conglomerados.csv` esté en el mismo directorio
4. Hacer clic en el botón **"Knit"** en la barra superior
5. Seleccionar **"Knit to PDF"**
6. El archivo PDF se generará automáticamente en el mismo directorio

### Opción 2: Desde la Consola de R

```r
# Establecer directorio de trabajo
setwd("/ruta/a/tu/proyecto/PCA")

# Renderizar el documento
rmarkdown::render("analisis_conglomerados_estudiantes.Rmd")
```

### Opción 3: Ejecutar Chunks Individualmente

Para probar el código por partes:

1. Abrir `analisis_conglomerados_estudiantes.Rmd` en RStudio
2. Hacer clic en el botón verde "play" (▶️) en cada chunk de código
3. Los resultados aparecerán debajo de cada chunk

## Contenido del Análisis

El archivo `.Rmd` incluye las siguientes secciones:

### 1. Carga y Exploración de Datos
- Carga del dataset
- Estructura y resumen estadístico
- Identificación de valores faltantes
- **Responde:** Preguntas A1, A2

### 2. Preparación de Datos
- Selección de variables numéricas
- Conversión de variable categórica (sexo) a dummy
- Estandarización con `scale()`
- **Responde:** Preguntas B3, B4, B5

### 3. Conglomerados Jerárquicos
- Matriz de distancias euclidianas
- Clustering con método Ward (ward.D2)
- Dendrograma
- Análisis de silhouette para k = 2 a 6
- Determinación de k óptimo
- **Responde:** Preguntas C6, C7, C8

### 4. Conglomerados No Jerárquicos (K-Means)
- Aplicación de k-means con nstart = 25
- Visualización de clusters (PCA)
- Análisis de centroides
- **Responde:** Preguntas D9, D10, D11

### 5. Comparación de Métodos
- Tabla cruzada jerárquico vs k-means
- Índice de Rand ajustado
- Análisis de coincidencias y diferencias
- **Responde:** Preguntas E12, E13, E14

### 6. Interpretación de Perfiles
- Medias por cluster de cada variable
- Tablas resumen con `kable()`
- Descripción de cada cluster
- Identificación de grupos en riesgo y fortaleza técnica
- **Responde:** Pregunta E15

### 7. Recomendaciones de Acompañamiento
- Acciones institucionales por cluster
- Conclusión sobre método más útil
- **Responde:** Preguntas F16, F17

### 8. Conclusiones Finales
- Resumen de hallazgos
- Impacto institucional

## Dataset: base_estudiantes_conglomerados.csv

### Variables incluidas:

- **id_estudiante:** Identificador único
- **sexo:** Masculino/Femenino
- **edad:** Edad del estudiante
- **semestre:** Semestre actual
- **promedio_general:** Promedio académico (0-100)
- **asistencia_pct:** Porcentaje de asistencia
- **horas_estudio_semana:** Horas dedicadas al estudio por semana
- **habilidad_programacion:** Nivel de habilidad en programación (0-10)
- **habilidad_estadistica:** Nivel de habilidad en estadística (0-10)
- **participacion_clase:** Nivel de participación en clase (0-10)
- **horas_trabajo_semana:** Horas de trabajo externo por semana
- **estres_academico:** Nivel de estrés académico (0-10)
- **proyectos_entregados_pct:** Porcentaje de proyectos entregados

## Solución de Problemas

### Error: "No se encuentra el archivo .csv"
- Verificar que `base_estudiantes_conglomerados.csv` esté en el mismo directorio que el `.Rmd`
- O modificar la ruta en el chunk `carga-datos`

### Error: "Package 'xxx' is not installed"
- Instalar el paquete faltante: `install.packages("nombre_paquete")`

### Error al generar PDF
- Verificar que LaTeX esté instalado
- Alternativamente, cambiar output a HTML: `output: html_document` en el YAML

### Error con mclust
- Instalar el paquete: `install.packages("mclust")`

### Advertencias sobre kableExtra en PDF
- Son normales si las tablas son muy anchas
- El parámetro `scale_down` en `kable_styling()` las ajusta automáticamente

## Personalización

### Cambiar número de clusters
Modificar el chunk donde se determina k_optimo o establecer manualmente:
```r
k_optimo <- 3  # Cambiar a número deseado
```

### Cambiar método de clustering jerárquico
En el chunk `clustering-jerarquico`, cambiar:
```r
hc_ward <- hclust(matriz_distancias, method = "complete")  # Otras opciones: "single", "average", "complete"
```

### Modificar variables de clustering
En el chunk `preparacion-datos`, ajustar la selección:
```r
variables_clustering <- datos %>%
  select(variable1, variable2, variable3)  # Personalizar
```

## Salida Esperada

Al ejecutar exitosamente, se generará:

1. **analisis_conglomerados_estudiantes.pdf** - Documento completo con:
   - Texto explicativo en español
   - Código R visible
   - Gráficos de alta calidad
   - Tablas formateadas
   - Respuestas a todas las preguntas guía

## Tiempo de Ejecución

- Aproximadamente 30-60 segundos dependiendo del hardware
- La generación del PDF puede tomar tiempo adicional

## Notas Importantes

1. El código usa `set.seed(123)` para reproducibilidad
2. Todos los chunks tienen `echo=TRUE` para mostrar el código
3. Las advertencias están desactivadas con `warning=FALSE`
4. Los mensajes están desactivados con `message=FALSE`
5. El análisis está completamente en español

## Contacto y Soporte

Para problemas con el análisis:
- Revisar los mensajes de error en la consola de R
- Verificar versiones de paquetes: `sessionInfo()`
- Consultar documentación de paquetes: `?nombre_funcion`

## Referencias

- **tidyverse:** https://www.tidyverse.org/
- **cluster:** https://cran.r-project.org/package=cluster
- **factoextra:** https://rpkgs.datanovia.com/factoextra/
- **kableExtra:** https://haozhu233.github.io/kableExtra/

---

**Curso:** CD3002C - Licenciatura en Inteligencia de Negocios
**Institución:** Tecnológico de Monterrey
**Tema:** Análisis de Conglomerados (Clustering Jerárquico y No Jerárquico)
