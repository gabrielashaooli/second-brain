# Tipos de gráficos y cuándo usarlos

**TLDR:** Catálogo de los gráficos vistos en Visualización de Datos con su definición, el caso de uso correcto y cuándo NO usarlos. La regla que atraviesa todo: el tipo de gráfico se elige por la relación entre datos que se quiere mostrar (parte-todo, cambio en el tiempo, distribución, correlación, flujo, jerarquía), no por estética.

## Cómo elegir

Antes de elegir el gráfico, definir qué relación de datos se quiere comunicar:

- **Parte-todo** → pastel, áreas cuadradas (waffle), treemap, barras apiladas.
- **Cambio en el tiempo** → líneas, área, pendientes, cascada.
- **Comparación entre categorías** → barras, dot plot, radar.
- **Distribución** → boxplot, histograma, diagrama de violín.
- **Correlación / relación entre variables** → dispersión, burbujas.
- **Flujo / proceso** → Sankey, flowchart, embudo.
- **Jerarquía** → treemap.

## Familias

### Barras
Datos categóricos o agrupados. Rectángulos con línea base común; la longitud es proporcional al valor. Fácil de interpretar.
- **Verticales:** la variedad más común.
- **Horizontales:** buenas para nombres largos de categoría; imitan cómo procesamos la información de derecha a izquierda.
- **Agrupadas:** comparar subgrupos; cuidar el orden de las series y no meter demasiadas.
- **Apiladas:** dos variables categóricas (la primera a lo largo de la barra, la segunda como pilas dentro). Deben representar la suma de un todo; dificultan comparar subcomponentes entre categorías.
- **Apiladas 100%:** muestran porcentaje relativo en vez de valor absoluto.
- Tips: usar **línea base cero** (no truncar), ordenar intencionalmente, mover las etiquetas dentro de las barras, cuidar el contraste de la etiqueta según el fondo, bordes blancos para accesibilidad.

### Cascada (waterfall)
Tipo específico de barras que revela la historia detrás del cambio neto entre dos puntos, desagregando cada componente. Ej.: headcount (100 + 30 contrataciones − 12 salidas − 10 bajas = 116) o ingresos-egresos. Regla de oro: todas las barras con línea base cero. Usar color opuesto para positivos vs. negativos. (Nombre alterno habitual: *bridge chart*.)

### Líneas
Cambio de un valor respecto al tiempo, o comparar varias series en el tiempo. Requieren datos continuos y una relación significativa entre puntos sucesivos. Ideal 4-5 líneas; con más, usar color, grosor y etiquetas. "Pensar las líneas como pequeñas historias." ¿Empezar en cero? No obligatorio si el rango de medición es pequeño y la distancia a cero es grande, si la relación con cero no tiene sentido, o si hay valores negativos.

### Área
Gráfico de línea que además rellena la región debajo. Muestra cantidades totales y relaciones parte-todo a lo largo del tiempo. Contras: parte de la información queda oculta, es difícil juzgar las áreas, y más color = más solapamiento y desorden.

### Pastel / circular
Relación de una parte con el todo; cada porción tiene ángulo central, área y longitud de arco. Usar cuando importa la relación parte-todo y NO la comparación entre porciones, o para transmitir que un segmento es muy grande/pequeño. NO usar para comparar tamaños de categorías, comparar entre gráficos, ni con datos que no suman 100%. Tips: sin 3D, ordenar significativamente, etiquetas dentro, color estratégico, pocas rebanadas.

### Áreas cuadradas / waffle
Rejilla (típ. 10×10 = 100 cuadros) coloreada según los datos. Alternativa al pastel; buena para comparar magnitudes muy distintas y para presentaciones en vivo (llenar hacia un objetivo, embudos).

### Treemap (mapa de árbol)
Rectángulos anidados cuyo tamaño es proporcional al valor, para datos **jerárquicos** de varios niveles. Creado en los años 90 por **Ben Shneiderman** para visualizar el directorio de archivos de un disco. Usar cuando hay relación parte-todo entre muchas categorías, la comparación precisa no importa y hay que priorizar el espacio. Solo valores > 0. Contras: comparar áreas con precisión es difícil y puede abrumar.

### Boxplot (diagrama de caja y bigotes)
Resumen visual de una distribución: mínimo, máximo, mediana, cuartil bajo y alto. La caja delimita los cuartiles; la línea interior es la mediana; los bigotes marcan el rango; los outliers son puntos individuales. Bueno para comparar distribuciones de varios datasets y apto para grandes volúmenes. Contras: requiere curva de aprendizaje, no muestra la distribución tan preciso como un histograma. Variaciones: ancho variable (según tamaño del grupo), entallado/muescas (intervalo de confianza de la mediana) y **diagrama de violín** (boxplot + curva de distribución, el ancho = frecuencia aproximada).

### Dispersión (scatter)
Relación entre dos variables (X, Y); muestra puntos, no líneas. Naturaleza exploratoria (EDA), "pan de cada día en ciencia de datos". Se lee horizontal, vertical, por cuadrantes y por tendencia general. Tips: quitar líneas de tendencia si no aportan (salvo clusters de modelos), puntos transparentes cuando se amontonan, quitar/atenuar cuadrícula, añadir secciones y etiquetas. Apto para grandes volúmenes con agregación previa y hexbin (ver [[visualizacion-de-big-data]]).

### Burbujas (bubble)
Relación entre tres o más variables numéricas: posición X, posición Y y **tamaño** de la burbuja (a veces color o movimiento para más dimensiones). Cada burbuja es un punto de dato. Evitar si las dimensiones extra no aportan valor o si el esfuerzo cognitivo supera el beneficio. Narrar pieza por pieza, usar interactividad, o incluir palabras si es estático.

### Pendientes (slopegraph)
Muestra si el valor de la primera columna es mayor, menor o igual que el de la segunda. Solo dos puntos: enfatiza el **cambio**, no las cantidades absolutas ni lo intermedio. No usar si el punto central del periodo es esencial o si no hay conexión real entre categorías.

### Radar / araña
Datos en varias dimensiones en coordenadas polares, de 0 (centro) a un máximo; el rango de cada dimensión se normaliza entre sí. Comparación rápida en espacio compacto (deportes, notas por materia). Difícil comparar por área; presentar gradualmente y ordenar los ejes con criterio; si la audiencia no está familiarizada, preferir líneas.

### Dot plot
Codifica datos en un punto/círculo. Bueno para conjuntos pequeños e identificar atípicos. Variantes: **Cleveland dot plot** (valores cuantitativos por categoría) y **connected dot plot** (dos o más series, enfatizando la diferencia o cambio entre ellas).

### Viñetas (bullet)
Variación de barras: una barra principal sobre una pila secundaria menos prominente. Para comparaciones, mostrar progreso o umbrales (valor observado vs. objetivo). Tips: color cuidadoso, pocas palabras, animación efectiva.

### Sankey
Creado por el capitán **Matthew Sankey en 1898** para mostrar la eficiencia energética de una máquina de vapor. Resume volumen y dirección de flujos entre etapas de un proceso (embudos de conversión/ventas, asignación de presupuesto, ciclo de vida de productos, recorridos de contratación). NO usar si los datos son categóricos sin flujo inherente o si se requieren comparaciones precisas. Alternativas: diagrama aluvial, coordenadas paralelas.

### Flowchart
Representa un proceso o flujo de trabajo con formas estándar: óvalo (inicio/fin), diamante (decisión), rectángulo (proceso), etc., conectadas por flechas. Documenta y controla la lógica de un sistema o algoritmo. Tips: usar color/contraste para enfocar, quitar elementos distractores.

### Unidades / pictogramas
Marcadores individuales (íconos, formas, imágenes) donde cada símbolo codifica una cantidad (ISOTYPE, símbolos, waffle). Ventaja: símbolos reconocibles → comunicación memorable que "humaniza" el dato (recordar que son personas, no solo números). No usar si la audiencia prefiere un gráfico estándar o si hay más de una dimensión. Tamaños consistentes, leyenda, animación en vivo.

### Tablas
Datos en filas y columnas (números, texto, símbolos). Interactúan con el sistema de comunicación **verbal** (se leen línea por línea). Usar cuando hay variedad de datos, la audiencia tiene necesidades distintas, o se complementa la historia principal. Diseño: definir orientación, quitar bordes/sombreados innecesarios, incorporar un elemento visual, ordenar intencionalmente, etiquetas de super-categoría, alinear con cuidado.

## Buenas prácticas para evitar distorsión

- Mantener proporciones precisas.
- Elegir la representación gráfica correcta.
- Evitar el exceso de efectos visuales (nada de 3D gratuito).
- Usar una escala apropiada.
- Simplificar el diseño.

Dos tips que resumen la materia: **remover distracciones** de tus gráficos y **enfocar la atención** para que otros sepan dónde mirar (ver [[atributos-preatentivos-y-jerarquia-visual]]).

```mermaid
graph TD
  R[Relación a mostrar] --> PT[Parte-todo]
  R --> T[Cambio en el tiempo]
  R --> C[Comparar categorias]
  R --> D[Distribucion]
  R --> CO[Correlacion]
  R --> F[Flujo/proceso]
  R --> J[Jerarquia]
  PT --> pastel & waffle & treemap & apiladas[Barras apiladas]
  T --> lineas & area & pendientes & cascada
  C --> barras & dotplot[Dot plot] & radar
  D --> boxplot & histograma & violin[Violin]
  CO --> dispersion & burbujas
  F --> sankey & flowchart & embudo[Embudo]
  J --> treemap
```

## Preguntas de examen

1. ¿Qué relación de datos justifica un gráfico de pastel y en qué tres casos NO deberías usarlo?
2. Explica la diferencia entre un slopegraph y un gráfico de líneas: ¿qué enfatiza cada uno y cuándo eliges uno sobre el otro?
3. ¿Qué cinco estadísticos resume un boxplot y qué desventaja tiene frente a un histograma?
4. ¿Por qué un treemap requiere datos jerárquicos y valores estrictamente mayores que cero? ¿Quién lo inventó y para qué?
5. Da un ejemplo de dato que se comunique mejor con un Sankey y explica por qué; ¿cuándo sería una mala elección?
6. Enumera las cinco buenas prácticas para evitar la distorsión de datos.

## Fuentes

- `raw/articles/Modulo 1 Visualizacion de Datos v2.pdf` (catálogo completo de tipos de gráficos, tips de diseño, buenas prácticas anti-distorsión).
- `raw/notes/MIACD 3 visualización de datos.txt` (pastel, Sankey 1898, dispersión, pendientes, radar, waffle, barras apiladas, treemap/Shneiderman, cascada, unidades).
- `raw/notes/MIACD 4 visualización de datos.txt` (gráficos aptos para grandes volúmenes).

Relacionadas: [[visualizacion-de-datos-fundamentos]] · [[color-en-visualizacion]] · [[atributos-preatentivos-y-jerarquia-visual]] · [[visualizacion-de-big-data]] · [[herramientas-de-visualizacion]] · [[maestria-miacd]]
