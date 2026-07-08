# Historia de la visualización de datos

**TLDR:** La visualización moderna nace del cruce entre cartografía, estadística y comercio; se organiza en épocas (Friendly): medición y teoría (1600s), nuevas formas gráficas (1700s), inicios de la gráfica moderna (1800-1850), Edad de Oro (1850-1900), "edades oscuras modernas" (1900-1950) y el renacimiento con Tukey, Bertin y la computadora (1950-1975).

## Por qué importa la historia

Casi todas las formas gráficas que usamos hoy (barras, líneas, pastel, dispersión, histograma) ya existían en el siglo XIX. Excel "se fusiló" a Playfair. Conocer los hitos ayuda a entender que cada gráfico se inventó para resolver un problema de comunicación concreto.

## Épocas (según Michael Friendly, *Milestones*)

- **Pre-siglo XVII:** raíces en los primeros mapas, pinturas rupestres y depicción visual. El primer mapa conocido y el diagrama heliocéntrico de **Copérnico** ("no estamos en el centro de las cosas") como narrativas visuales tempranas.
- **1600-1699 — Medición y teoría:** problemas de medición física (tiempo, distancia, espacio) para astronomía y navegación; nace la geometría analítica, la teoría de errores y la probabilidad. **Van Langren (1644)**: primer gráfico conocido de datos estadísticos (12 estimaciones de la longitud Toledo-Roma); solo el gráfico revela la enorme variación entre estimaciones.
- **1700-1799 — Nuevas formas gráficas:** isolíneas y curvas de nivel; los mapas empiezan a mostrar más que geografía. **William Playfair (1759-1823)**, ingeniero y economista escocés, inventor de la mayoría de formas modernas: gráfico de **barras** y de **líneas** (*Commercial and Political Atlas*, 1786) y luego el de **pastel** (*Statistical Breviary*, 1801). Quería que los datos "hablaran a los ojos".
- **1800-1850 — Inicios de la gráfica moderna:** crecimiento explosivo. Se inventan todas las formas modernas de despliegue (barras, pastel, histogramas, líneas, curvas de nivel, dispersión). **Minard** introduce las barras de ancho variable y proporcional (precursor del mosaic plot); quería que los datos se pudieran "calcular con el ojo".
- **1850-1900 — Edad de Oro de la gráfica estadística:** oficinas estatales de estadística en toda Europa; teoría estadística (Gauss, Laplace, Quetelet). Belleza e innovación sin precedentes. Es la época de **Charles Joseph Minard** (mapa de flujo de la campaña de Napoleón en Rusia, 1812), **Florence Nightingale** (diagrama de la rosa / coxcomb, 1858: demostró que la mayoría de bajas del ejército británico eran por condiciones sanitarias, no combate) y **John Snow** (mapa de puntos del cólera en Londres, 1854: localizó la bomba de agua contaminada — precursor de la epidemiología).
- **1900-1950 — "Edades oscuras modernas":** pocas innovaciones gráficas; la cuantificación formal y los modelos estadísticos desplazan a las imágenes ("los números eran precisos; las imágenes, solo imágenes"). Pero la gráfica se vuelve mainstream: entra a libros de texto, currículo y uso gubernamental. Descubrimientos vía gráficas: número atómico (Moseley), diagrama Hertzsprung-Russell de estrellas.
- **1950-1975 — Renacimiento:** impulsado por tres desarrollos: (a) **John W. Tukey** y el *Exploratory Data Analysis* (boxplot y otras formas simples y efectivas); (b) **Jacques Bertin** publica *Sémiologie Graphique* (1967), que organiza los elementos visuales según las relaciones en los datos ("lo que Mendeléyev hizo por los elementos químicos"); (c) el **procesamiento por computadora**, que permite generar gráficos por programa. Surgen representaciones multivariadas (Andrews plots, caras de Chernoff), animación y teoría perceptual.
- **Siglos XIX-XX y era digital:** avances estadísticos y tecnológicos (GIS); herramientas modernas (Tableau, Power BI, D3.js) y visualización interactiva y narrativa (*data storytelling*).

## Atribución: la Ley de Stigler

Stigler's Law of Eponymy: *"ningún descubrimiento científico lleva el nombre de su descubridor original."* Las atribuciones estándar (Playfair, Herschel para el scatterplot, Tukey para el boxplot) son las que la comunidad convino, no necesariamente las de prioridad histórica.

```mermaid
graph LR
  A[Pre-XVII: mapas, Copernico] --> B[1600s: medicion y teoria - Van Langren 1644]
  B --> C[1700s: nuevas formas - Playfair barras/lineas 1786]
  C --> D[1800-1850: gráfica moderna - Minard]
  D --> E[1850-1900: Edad de Oro - Nightingale, Snow]
  E --> F[1900-1950: edades oscuras - gráfica mainstream]
  F --> G[1950-1975: renacimiento - Tukey EDA, Bertin, computadora]
  G --> H[Era digital: Tableau, PowerBI, D3, storytelling]
```

## Contradicción / hueco a señalar

En las **clases grabadas** (transcripciones MIACD 1-6) el profesor Salgado NO menciona a **Tufte, Bertin ni Wilkinson**; "Cleveland" aparece solo como nombre de un tipo de gráfico (dot plot), no como autor. Toda la genealogía de autores clásicos (Bertin, Tukey, Stigler, la Edad de Oro) proviene del paper de **Friendly (2004)** asignado como lectura, no de la exposición en clase. La clase se centró en los casos ilustrativos (Playfair, Minard, Nightingale, Snow) que sí están en el PDF del Módulo 1.

## Preguntas de examen

1. Nombra las seis épocas de Friendly y un hito representativo de cada una.
2. ¿Qué formas gráficas inventó Playfair y en qué obras? ¿Por qué se le considera el inventor de la gráfica estadística moderna?
3. ¿Qué demostraron Nightingale y Snow con sus visualizaciones y por qué fueron socialmente relevantes?
4. ¿Por qué Friendly llama "edades oscuras modernas" al periodo 1900-1950 si la gráfica se volvió mainstream?
5. Explica los tres desarrollos que detonaron el renacimiento de la visualización (1950-1975).
6. ¿Qué afirma la Ley de Stigler y qué implica para atribuir la invención de un gráfico?

## Fuentes

- `raw/articles/L1 Milestones_in_the_History_of_Data_Visualization.pdf` (Michael Friendly, 2004 — épocas, autores, Ley de Stigler, Van Langren, Playfair, Minard, Bertin, Tukey).
- `raw/articles/Modulo 1 Visualizacion de Datos v2.pdf` (Playfair, Minard, Nightingale, Snow, era digital).
- `raw/notes/MIACD 1 visualización de datos.txt` (relato del profesor: rupestres, primer mapa, Copérnico, Playfair, Minard, Nightingale, cólera).

Relacionadas: [[visualizacion-de-datos-fundamentos]] · [[tipos-de-graficos]] · [[maestria-miacd]]
