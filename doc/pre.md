# Pre-analysis
--------------

## Introducción
Con el avance de las nuevas tecnologías de neuorimágenes (capaces de evaluar la estructura y funcionamiento del cerebro de manera no invasiva), varios grupos académicos han afrontado un dilema que aqueja a la humanidad desde hace siglos: ¿por qué algunas personas optan por ser religiosas y otras no? ¿cómo se diferencian biológicamente?

Con estas preguntas en mente, van Elk & Snoek (2017) [1] se propusieron realizar un estudio a gran escala (N = 211) intentando establecer una relación entre materia gris y la religisiodad de sus sujetos. Inspirados en estudios previos, los cuales hallaron correlaciones entre regiones particulares del cerebro (corteza orbitofrontal, lobo temporal, lobos parietales inferiores) y las creencias de los sujetos [2, 3, 4, 5], realizaron un análisis de morfometría basada en vóxel sobre las resonancias magnéticas estructurales (MRI) de las personas. No obstante, no encontraron cambios significativos en las estructuras mencionadas, y tampoco lo hicieron a nivel global (es decir, sobre el cerebro entero).

A raíz de esto, los autores sugieren que próximos estudios deberían focalizarse en estudios funcionales y multivariados. Dentro de esta línea, Brooks et. al (2022) [6], a partir de análisis en estado de reposo de resonancias magnéticas funcionales (rs-fMRI), hallaron diferencias en la topología de las redes funcionales de jóvenes cuyos padres eran religiosos (no disponían de información sobre las creencias de los jóvenes mismos). Por lo tanto, en este trabajo nos disponemos de analizar propiedades estructurales y funcionales de los sujetos en la base de datos pública AOMIC (N = 1000), la cual contiene información sobre religiosidad de sus sujetos (equivalente a la que disponían Elk & Snoek (2017)). De haber una diferencia significativa en la morfología y funcionamiento cerebral de las personas creyentes, la misma debería verse reflejada en las predicciones realizadas por un modelo de aprendizaje automático, aún corrigiendo por educación y estatus socioeconómico.

## Métodos
Los datos a analizar corresponden a aquellos de AOMIC publicados en [OpenNeuro](https://openneuro.org/datasets/ds002895), ya procesados. En particular, nos focalizaremos en la morfometría basada en vóxel y las resonancias magnéticas funcionales en estado de reposo. De estos datos, extraeremos los siguientes atributos:

### La homogeneidad regional sobre parcelas
La homogeneidad regional (ReHo) (```ReHoParcels```) es una medida utilizada en el análisis de datos de RMf para evaluar la sincronización local de la actividad cerebral. Evalúa la similitud de la serie temporal de un vóxel dado con sus vecinos, lo que sugiere cómo un grupo de vóxeles cercanos están conectados funcionalmente. Esencialmente, ReHo puede considerarse como una medida de la conectividad local o la coherencia de la actividad cerebral dentro de un grupo localizado de vóxeles. Esta medida aplicada sobre una parcelación determinada, nos da una medida de actividad promedio de cada region.

### El promedio de la connectividad funcional inter e intra redes
La conectividad functional (```FunctionalConnectivityParcels```) es una medida de co-relacionamiento funcional entre distintas regiones del cerebro, nosotros planteamos estudiar la fuerza de estas conectividades tanto dentro como entre las diferentes redes cerebrales (sensorimotor system, visual system, limbic system, central executive network (CEN), default mode network (DMN), salience network and dorsal attention network (DAN)) [7].

### Promedio por parcela de materia gris
El promedio por parcela del volumen de materia gris (```ParcelAggregation```) nos da una metrica general sobre la anatomia del cerebro que correlaciona con las medidas estudiadas en [1]

Con estas 3 familias de atributos podemos contabilizar tanto las relaciones funcionales locales (ReHo) como globales (conectividad funcional) como la anatomia del cerebro y de esta forma poder estudar si existen diferencias significativas entre los distintos grupos de religiosidad.


La parcelación a utilizar será aquella de Yao con 700 regiones de interés y 7 redes, dado que nos interesa primordialmente el análisis cortical. La herramienta a utilizar será Junifer 0.0.4 para la extracción de atributos y Julearn 0.3.1 para los métodos de aprendizaje automático.

Los datos clínicos que emplearemos como target son dos: si son religiosos y si fueron criados religiosamente (ambas constituyen tareas de clasificación). Estos análisis se realizarán con el mismo conjunto de atributos, pero con distintos modelos. Dado que el conjunto de personas religiosas es bajo (21% del total), balancearemos los datos empleando un mismo número de personas no religiosas, equiparando niveles educativos y socioeconómicos con aquellos de las personas religiosas. Lo mismo se aplicará sobre los datos de personas criadas religiosamente (34.1% del total).

Las figuras siguientes muestran el balance actual de los datos:


Los algoritmos de aprendizaje automático a utilizar constituyen al Support Vector Machine (SVM) y Random Forest. Los datos serán normalizados a través de z-score previo a su entrenamiento. Para la elección final del modelo, la evaluación se realizará a través de las métricas accuracy, precision y recall.

Luego de separar un conjunto de los datos (15% del total) para evaluación final, el esquema de entrenamiento será de evaluación cruzada con stratified K fold (5 folds). Se reportarán las métricas previamente mencionadas sobre el conjunto de datos apartado.

## Bibliografía
[1] Elk y Snoek 2017
[2] Hayward, Owen, Koenig, Steffens, & Payne, 2011
[3] Devinsky & Lai, 2008
[4] Newberg, Alavi et al., 2001
[5] Johnstone & Glass, 2008
[6] Snoek et al., 2021
[7] https://www.o8t.com/blog/brain-networks#:~:text=Depending%20on%20the%20granularity%20of,network%20(DMN)%2C%20salience%20network
