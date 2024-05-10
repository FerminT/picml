# Pre-analysis
--------------

## Introducción
Con el avance de las nuevas tecnologías de neuorimágenes (capaces de evaluar la estructura y funcionamiento del cerebro de manera no invasiva), varios grupos académicos han afrontado un dilema que aqueja a la humanidad desde hace siglos: ¿por qué algunas personas optan por ser religiosas y otras no? ¿cómo se diferencian biológicamente?

Con estas preguntas en mente, van Elk & Snoek (2017) [1] se propusieron realizar un estudio a gran escala (N = 211) intentando establecer una relación entre materia gris y la religisiodad de sus sujetos. Inspirados en estudios previos, los cuales hallaron correlaciones entre regiones particulares del cerebro (corteza orbitofrontal, lobo temporal, lobos parietales inferiores) y las creencias de los sujetos [2, 3, 4, 5], realizaron un análisis de morfometría basada en vóxel sobre las resonancias magnéticas estructurales (MRI) de las personas. No obstante, no encontraron cambios significativos en las estructuras mencionadas, y tampoco lo hicieron a nivel global (es decir, sobre el cerebro entero).

A raíz de esto, los autores sugieren que próximos estudios deberían focalizarse en estudios funcionales y multivariados. Dentro de esta línea, Brooks et. al (2022) [6], a partir de análisis en estado de reposo de resonancias magnéticas funcionales (rs-fMRI), hallaron diferencias en la topología de las redes funcionales de jóvenes cuyos padres eran religiosos (no disponían de información sobre las creencias de los jóvenes mismos). Por lo tanto, en este trabajo nos disponemos de analizar propiedades estructurales y funcionales de los sujetos en la base de datos pública AOMIC (N = 1000), la cual contiene información sobre religiosidad de sus sujetos (equivalente a la que disponían Elk & Snoek (2017)). De haber una diferencia significativa en la morfología y funcionamiento cerebral de las personas creyentes, la misma debería verse reflejada en las predicciones realizadas por un modelo de aprendizaje automático, aún corrigiendo por sexo, edad, educación y estatus socioeconómico.

## Métodos
Los datos a analizar corresponden a aquellos de AOMIC publicados en [OpenNeuro](https://openneuro.org/datasets/ds002895), ya procesados. En particular, nos focalizaremos en la morfometría basada en vóxel y las resonancias magnéticas funcionales en estado de reposo. La parcelación a utilizar será aquella de Yao con 100 regiones de interés y 7 redes, dado que nos interesa primordialmente el análisis cortical. La herramienta a utilizar será Junifer 0.0.4 para la extracción de atributos y Julearn 0.3.1 para los métodos de aprendizaje automático.

Los siguientes atributos serán extraídos:

### La homogeneidad regional sobre parcelas
La homogeneidad regional (ReHo) (```ReHoParcels```) es una medida utilizada en el análisis de datos de RMf para evaluar la sincronización local de la actividad cerebral. Evalúa la similitud de la serie temporal de un vóxel dado con sus vecinos, lo que sugiere cómo un grupo de vóxeles cercanos están conectados funcionalmente. Esencialmente, ReHo puede considerarse como una medida de la conectividad local o la coherencia de la actividad cerebral dentro de un grupo localizado de vóxeles. Esta medida aplicada sobre una parcelación determinada, nos da una medida de actividad promedio de cada region.

### El promedio de la conectividad funcional inter e intra redes
La conectividad functional (```FunctionalConnectivityParcels```) es una medida de co-relacionamiento funcional entre distintas regiones del cerebro. Planteamos estudiar la fuerza de estas conectividades tanto dentro como entre las diferentes redes cerebrales (sensorimotor system, visual system, limbic system, central executive network (CEN), default mode network (DMN), salience network and dorsal attention network (DAN)) [7], a partir del promedio entre las regiones correspondientes.

### Promedio por parcela de materia gris
El promedio por parcela del volumen de materia gris (```ParcelAggregation```) nos da una metrica general sobre la anatomía del cerebro que correlaciona con las medidas estudiadas en [1].

Con estas 3 familias de atributos, podemos contabilizar tanto las relaciones funcionales locales (ReHo) como globales (conectividad funcional), así como la anatomía general del cerebro. De haber diferencias significativas en la predicción de religiosidad a partir de esos atributos, un estudio de ablación nos permitiría determinar cuáles de ellos son los que más aportan a la tarea de predicción.

Los datos clínicos que emplearemos como target son dos: si son religiosos y si fueron criados religiosamente (ambas constituyen tareas de clasificación). Estos análisis se realizarán con el mismo conjunto de atributos, pero con distintos modelos. Dado que el conjunto de personas religiosas es bajo (21% del total), balancearemos los datos empleando un mismo número de personas no religiosas, equiparando edad, sexo, niveles educativos y socioeconómicos con aquellos de las personas religiosas. Lo mismo se aplicará sobre los datos de personas criadas religiosamente (34.1% del total).


Los algoritmos de aprendizaje automático a utilizar constituyen al Support Vector Machine (SVM) y Random Forest. Los datos serán normalizados a través de z-score previo a su entrenamiento. Para la elección final del modelo, la evaluación se realizará a través de las métricas accuracy, precision y recall.

Luego de separar un conjunto de los datos (15% del total) para evaluación final, el esquema de entrenamiento será de evaluación cruzada con stratified K fold (5 folds). Se reportarán las métricas previamente mencionadas sobre el conjunto de datos apartado.

## Bibliografía
[1] van Elk, M., & Snoek, L. (2020). The relationship between individual differences in gray matter volume and religiosity and mystical experiences: A preregistered voxel-based morphometry study. The European journal of neuroscience, 51(3), 850–865. https://doi.org/10.1111/ejn.14563

[2] Owen AD, Hayward RD, Koenig HG, Steffens DC, Payne ME. Religious factors and hippocampal atrophy in late life. PLoS One. 2011 Mar 30;6(3):e17006. doi: 10.1371/journal.pone.0017006. PMID: 21479219; PMCID: PMC3068149.
[3] Orrin Devinsky, George Lai, Spirituality and Religion in Epilepsy, Epilepsy & Behavior, Volume 12, Issue 4, 2008, Pages 636-643, ISSN 1525-5050.
[4] Newberg A, Alavi A, Baime M, Pourdehnad M, Santanna J, d'Aquili E. The measurement of regional cerebral blood flow during the complex cognitive task of meditation: a preliminary SPECT study. Psychiatry Res. 2001 Apr 10;106(2):113-22. doi: 10.1016/s0925-4927(01)00074-9. PMID: 11306250.
[5] Johnstone, Brick & Glass, Bret A. (2008). Support for a neuropsychological model of spirituality in persons with traumatic brain injury. Zygon 43 (4):861-874.
[6] Brooks, S.J., Tian, L., Parks, S.M. et al. Parental religiosity is associated with changes in youth functional network organization and cognitive performance in early adolescence. Sci Rep 12, 17305 (2022). https://doi.org/10.1038/s41598-022-22299-6
[7] https://www.o8t.com/blog/brain-networks#:~:text=Depending%20on%20the%20granularity%20of,network%20(DMN)%2C%20salience%20network
