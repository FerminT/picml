# Pre-analysis
--------------

## Introducción
Con el avance de las nuevas tecnologías de neuorimágenes (capaces de evaluar la estructura y funcionamiento del cerebro de manera no invasiva), varios grupos académicos han afrontado un dilema que aqueja a la humanidad desde hace siglos: ¿por qué algunas personas optan por ser religiosas y otras no? ¿cómo se diferencian biológicamente?

Con estas preguntas en mente, van Elk & Snoek (2017) [1] se propusieron realizar un estudio a gran escala (N = 211) intentando establecer una relación entre materia gris y la religisiodad de sus sujetos. Inspirados en estudios previos, los cuales hallaron correlaciones entre regiones particulares del cerebro (corteza orbitofrontal, lobo temporal, lobos parietales inferiores) y las creencias de los sujetos [2, 3, 4, 5], realizaron un análisis de morfometría basada en vóxel sobre las resonancias magnéticas estructurales (MRI) de las personas. No obstante, no encontraron cambios significativos en las estructuras mencionadas, y tampoco lo hicieron a nivel global (es decir, sobre el cerebro entero).

A raíz de esto, los autores sugieren que próximos estudios deberían focalizarse en estudios funcionales y multivariados. Dentro de esta línea, Brooks et. al (2022) [6], a partir de análisis en estado de reposo de resonancias magnéticas funcionales (rs-fMRI), hallaron diferencias en la topología de las redes funcionales de jóvenes cuyos padres eran religiosos (no disponían de información sobre las creencias de los jóvenes mismos). Por lo tanto, en este trabajo nos disponemos de analizar propiedades estructurales y funcionales de los sujetos en la base de datos pública AOMIC (N = 1000), la cual contiene información sobre religiosidad de sus sujetos (equivalente a la que disponían Elk & Snoek (2017)). De haber una diferencia significativa en la morfología y funcionamiento cerebral de las personas creyentes, la misma debería verse reflejada en las predicciones realizadas por un modelo de aprendizaje automático, aún corrigiendo por educación y estatus socioeconómico.

## Métodos
Los datos a analizar corresponden a aquellos de AOMIC publicados en [OpenNeuro](https://openneuro.org/datasets/ds002895), ya procesados. En particular, nos focalizaremos en la morfometría basada en vóxel y las resonancias magnéticas funcionales en estado de reposo. De estos datos, extraeremos como atributos la homogeneidad regional sobre esferas (```FunctionalConnectivityParcels```) para el caso funcional y el promedio por parcela del volumen de materia gris (```ParcelAggregation```) para el caso estructural. La parcelación a utilizar será aquella de Yao con 700 regiones de interés y 7 redes, dado que nos interesa primordialmente el análisis cortical. La herramienta a utilizar será Junifer 0.0.4 para la extracción de atributos y Julearn 0.3.1 para los métodos de aprendizaje automático.

Los datos clínicos que emplearemos como target son dos: si son religiosos y si fueron criados religiosamente (ambas constituyen tareas de clasificación). Estos análisis se realizarán con el mismo conjunto de atributos, pero con distintos modelos. Dado que el conjunto de personas religiosas es bajo (21% del total), balancearemos los datos empleando un mismo número de personas no religiosas, equiparando niveles educativos y socioeconómicos con aquellos de las personas religiosas. Lo mismo se aplicará sobre los datos de personas criadas religiosamente (34.1% del total).

Los algoritmos de aprendizaje automático a utilizar constituyen al Support Vector Machine (SVM) y Random Forest. Los datos serán normalizados a través de z-score previo a su entrenamiento. Para la elección final del modelo, la evaluación se realizará a través de las métricas accuracy, precision y recall.

Luego de separar un conjunto de los datos (15% del total) para evaluación final, el esquema de entrenamiento será de evaluación cruzada con stratified K fold (5 folds). Se reportarán las métricas previamente mencionadas sobre el conjunto de datos apartado.

## Bibliografía
[1] Elk y Snoek 2017
[2] Hayward, Owen, Koenig, Steffens, & Payne, 2011
[3] Devinsky & Lai, 2008
[4] Newberg, Alavi et al., 2001
[5] Johnstone & Glass, 2008
[6] Snoek et al., 2021
