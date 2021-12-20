![Logo-DANE-color-2019](https://user-images.githubusercontent.com/78028496/146801129-8f1da309-063e-4071-b12e-4e876c41fb59.jpg)


# Red neuronal para la clasificación de individuos en grupos étnicos para el censo poblacional de Colombia 2018.

## Introducción.

El Departamento Administrativo Nacional de Estadística – DANE tiene como misión fundamental garantizar la disponibilidad y la calidad de la información estadística estratégica para el desarrollo económico y político del país, en el marco de sus objetivos misionales, así como garantizar la producción, disponibilidad y calidad de la información estadística estratégica; y dirigir, planear, ejecutar, coordinar, regular y evaluar la producción y difusión de información oficial básica.

En ese sentido, el DANE tiene como objetivo mejorar la disponibilidad de estadísticas relevantes con niveles de desagregación más detallados (relacionado con grupos poblacionales, o con dominios geográficos); así como de producir estadísticas relevantes con una mayor frecuencia. Un medio para lograrlo es a partir de la integración de las diferentes fuentes de información tradicionales (como encuestas y censos) con fuentes alternativas (como imágenes satelitales, registros administrativos, técnicas de big data, entre otros).

Con el pasar de los años nos encontramos que los avances tecnológicos han hecho que podamos hacer una mejor manipulación de la información y que el procesamiento que le podamos dar a dicho información sea el mas óptimo para mejorar el conocimiento, en este caso centrado en el sociodemográfico, ayudándonos a categorizar de una manejar mas exacta las diferentes poblaciones, centrándonos en el los grupos étnicos, que son identificados con las preguntas de autorreconociento, en las diferentes encuestas que realiza periódicamente el Departamento Administrativo Nacional de Estadística (DANE), gracias a este tipo de preguntas se puede indagar a cual grupo étnico reconocido institucionalmente pertenecen en Colombia. 

En el 2018 se realizó el censo de población en Colombia, el cual intenta identificar la composición de la población colombiana, esto para tomar decisiones y orientar políticas públicas en aspecto sociales, una de las variables medidas fue el reconocimiento a los diferentes grupos étnicos, debido a la posible desinformación o errónea recogida de las observaciones, un gran porcentaje de la población no se clasificó en un grupo étnico particular generando errores a la hora de identificar a la población del país. El problema fue identificado debido a que durante las proyecciones del censo del 2005 se esperaba que en el 2018 se tuviera cerca de 4 millones de afro colombianos, cifra que con el censo de 2018 fue cercana a 2 millones.

Esto se debe principalmente a dos problemas:
1. Las personas fueron mal clasificadas al momento de ser encuestadas.
2. Error por parte de los encuestadores al no tomaron el dato verídico.

Para mitigar este problema principalmente se plantea un modelo de redes neuronales en que se toman a todas aquellas personas que tuvieron una errónea clasificación en su grupo étnico.

En este trabajo desarrollamos la implementación de una red neuronal autoencoder, esto para la reclasificación del grupo étnico de personas autorreconociddas como sin ningún grupo a la categoría negros, se usaron variables sociodemograficas relacionadas con el autorreconocimiento y este método planteado se comparó con otras metodologías tales como una regresión logística y maquinas de soporte vectorial usando algunas medidas de bondad de ajuste.

## Contenido.

  ### 1.1. Selección de variables.
  Esta etapa determina el poder predictivo del modelo debido a que genera mayor eficacia a la hora del entrenamiento, en principio, se debe escoger la mínima cantidad de  variables siguiendo el principio de parsimonia.  En la disponibilidad de variables para el proyecto se emplearon las siguientes:
  
      * P_SEXO: Sexo.
      * PA11_COD_ETNIA: Pueblo indígena de pertenencia.
      * PA12_CLAN: Clan de pertenencia.
      * PA_LUG_NAC: Lugar de nacimiento.
      * PA1_DPTO_NAC: Departamento de nacimiento.
      * P_PARENTESCO: Relación de parentesco con el jefe(a) del hogar.
      * UA_CLASE: Clase.
      * UVA_ESTATER: Vivienda en una territorialidad étnicaGMO.
      * UVA1_TIPOTER: Tipo de territorialidad étnica.
      * UVA2_CODTER: Código de territorialidad étnica.
      * PA1_GRP_ETNIC: Reconocimiento étnico – Variable de respuesta.
      
   ### 1.2. Balanceo de categorías.
   En el proyecto se hizo enfasís en la población afrodescendiente, observardo la distribución de categorías en la variable de respuesta se determinó que presentaba un           desbalance, una proporción de alrededor del 7% del conjunto total de datos (Censo de población nacional y vivienda 2018).  para balancear las categorías se extrajo una muestra aleatoria de la misma cantidad de afrodescedientes del resto de los índividuos pertenecientes a las otras categorías (autoreconocimiento étnico).
   
   ### 2.1. Entrenamiento de la red neuronal autoencoder. 
   En esta primera red tenemos una función de activación relu, con 100 épocas y 100 lotes, con 1141 neuronas en la capa superior, y en las capas ocultas tenemos 500, 300 y 10 neuronas. estos hiperparametros fueron escogidos ya que después de haber realizado el procedimiento muchas veces fueron los valores que tuvieron una muy buena precisión.

  como se observa en este primer modelo se obtiene una precisión del 45% lo cual no nos pareció la mejor opción, también nos damos cuenta que según la matriz de confusión no esta clasificando bien, ya que tiene un recall de 6.5 %, es decir que clasifica a menos personas afros cuando en realidad son afros, lo cual hace que descartamos este modelo y continuemos viendo para el segundo modelo.
  
![Autoencoder](https://user-images.githubusercontent.com/78028496/146802129-b661592c-0fa7-4507-8f4a-119fae5037fa.png)

### 2.2. Red neuronal densa.
Una alternativa ha sido entrenar una red neuronal densa para la predicción del autoreconocimiento étnico específicamente la categoría afrodecendiente, la estructura de la red esta compuesta por 1138 neuronas de entrada, dos capas intermedias de 600 neuronas y una capa de 2 neuronas de salida que permitirá identificar la respuesta binaria de la predicción, donde 1 el individuo fue clasificado como afrodescendiente y 0 no afrodescendiente.

En el assessment del modelo se utilizó un 70% del conjunto de datos para entrenamiento y un 30% para evaluación, se observó que el accuracy del entrenamiento y evaluación estaban muy cercanos, es decir, reflejando un buen ajuste.

![red densa 1](https://user-images.githubusercontent.com/78028496/146804503-6aa280a5-94d9-45ac-b147-e6f2697f7921.png)

En la evaluación del modelo se obtiene un “accuracy” del 57% (porcentaje total de aciertos en el modelo), se debe resaltar que el modelo presenta una sensibilidad del 69 %,
es decir, logra identificar el 69% de los individuos que se autor reconocieron como afrodescendientes en el conjunto de datos de evaluación.

![Red densa 2](https://user-images.githubusercontent.com/78028496/146804242-bb33e63a-a902-4105-b969-adc54de18bc2.png)

el modelo identifica 608.746 individuos que se reconocieron como afrodescendientes frente a un total de 885.429 afrodescendientes que contenía el conjunto de datos de evaluación, sin embargo, hay 483.407 individuos que el modelo clasifica de manera errónea afirmando que son afrodescendientes, de aquí se identifica el 62% de los casos identificados por el modelo corresponden a la realidad.

![red densa 3](https://user-images.githubusercontent.com/78028496/146804305-85f93d14-5545-4373-8e4e-1aedc92ae4db.png)
  
La prueba ROC que permite determinar la capacidad discriminatoria del clasificador según el área bajo la curva; mientras mayor sea más poder discriminatorio tendrá el clasificador. Esta área puede interpretarse como la probabilidad de que, ante un par de individuos, uno que se auto reconozca afrodescendiente y el otro no, el modelo los clasifique correctamente.



Equipo RNAE - DANE

Juan Pablo Paez - jppaezg@unal.edu.co 

Steven Cifuentes - stevencifuentes@gmail.com / https://github.com/Steven-CR


  
  
  







