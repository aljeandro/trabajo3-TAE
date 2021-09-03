
# **Trabajo 3 Técnicas en Aprendizaje Estadístico**

# **Integrantes**

- **Alejandro Ortiz Mejía**

## **Introducción** 

En el área de la inteligencia artificial y la estadística los problemas de clasificación son muy comúnes, quizás incluso más que los de regresión [1, p.130]. En estos se busca determinar a qué clase o categoría pertenece un individuo dadas ciertas características de este.

A menudo los individuos que requieren ser clasificados son imágenes; por ejemplo, en el área de la agronomía resulta útil saber si un fruto esta maduro o no, o, en el contexto de la pandemia del covid-19, podría ser de mucha ayuda reconocer cuándo una persona no está utilizando tapabocas por medio de una cámara de seguridad, todo esto sin la supervisión permanente de un humano.

## **Planteamiento del problema**

**Objetivo:** construir y validar un modelo de aprendizaje estadístico para clasificar imágenes de sujetos con gafas de sol del conjunto de datos *CMU Face Images Data Set*.

### Descripción del Conjunto de Datos

En este proyecto se lleva a cabo la clasificación de personas que están usando o no gafas de sol; para ello, se utiliza el conjunto de datos llamado *CMU Face Images Data Set*, el cual contiene 640 imágenes cuyo tamaño es de 120 filas por 128 columnas; además, dichas imágenes se encuentran en escala de grises.

Cada una de las imágenes está nombrada siguiendo la siguiente convención:

(id_usuario) (pose) (expresión) (ojos).pgm
  
  - (id_usuario): Corresponde a la identificación de la persona en la imagen, este campo tiene 20 valores en total: an2i, at33, boland, bpm, ch4f, cheyer, choon, danieln, glickman, karyadi, kawamura, kk49, megak, mitchell, night, phoebe, saavik, steffi, sz24, y tammo.
  
  - (pose): Es la posición de la cabeza de la persona, este campo puede contener 4 valores: straight (derecho), left (izquierda), right (derecha), up (arriba).
  
  - (expresión): Es la expresión facial de la persona, este campo puede contener 4 valores:  neutral (neutro), happy (feliz), sad (triste), angry (enojado).
  
  - (ojos): es el estado de los ojos de la persona, este campo puede tener dos valores: open (sin gafas de sol), sunglasses (con gafas de sol).

A continuación, veamos algunas imágenes del conjunto de datos:

![image](/images/an2i_straight_neutral_sunglasses.png) ![image](/images/an2i_straight_sad_open.png) ![image](/images/bpm_right_angry_open.png) ![image](/images/bpm_right_angry_sunglasses.png) 

### Preprocesamiento de las Imágenes

Para un manejo más sencillo, las imagenes fueron convertidas al formato "png".

Debido a un error en la cámara con la que se tomaron las fotografías, 12 de las imágenes se dañaron, por tanto, estas serán borradas manualmente.
  
  Como se mencionó anteriormente, todas las imágenes están en escala de grises, eso quiere decir que en su representación como matriz, en los 3 canales (R, G, B) tendrán el mismo valor, por lo tanto, para evitar la redundancia de información y optimizar el modelo, solamente se tomará la información de uno de los canales.

Posteriormente, la matriz que representa a cada imagen se "aplana" con el fin de que quede en dimensión (15360, 1).

Por otro lado, al ser aprendizaje supervisado, es necesario obtener un vector con la variable respuesta, para ello se utiliza la información contenida en el nombre de cada imagen, así que si el nombre de la imagen contiene la palabra *sunglasses*, entonces su respectiva etiqueta será 1, de lo contrario será 0.
  
  
  Por último, se normalizan las imágenes, dividiendo cada entrada de cada imagen entre la norma euclidiana de sus componentes.

## Elección, Funcionamiento y Configuración del Modelo

En este trabajo se probaron dos tipos distintos de modelos de clasificación, con el fin de determinar cuál de los dos ofrece un mejor desempeño:

### 1. Regresión Logística con Regularización

La regresión logística con regularización (ver Figura 1) es un modelo que permite hacer predicciones desde una perspectiva probabilística, es decir, la respuesta siempre estará dada por un número entre 0 y 1, lo cual resulta útil para hacer clasificaciones. Sin embargo, su desempeño es alto solo si las clases o categorías de clasificación están separadas por un hiperplano, debido a su naturaleza lineal. Adicionalmente, la regularización es una técnica que evita que el modelo se sobreajuste a los datos de entrenamiento, evitando que el modelo capture el ruido del conjunto de datos [2], permitiendo así que el modelo generalice mejor. La técnica de regularización en este problema se llama Regularización L2. 

![image](/images/ec4.jpg)         
  Figura 1. Regresión Logística.

En este problema en específico, el vector X corresponde a la imagen, p es el número de pixeles, es decir 15360 (120 * 128 tomando un solo canal),Y es la variable respuesta y los beta sub i son los parámetros que deberá aprender el modelo.

Para el entrenamiento de este modelo, el conjunto de datos fue dividido en dos partes así:

  - **Entrenamiento**: este conjunto se utiliza para enseñarle al modelo a hacer las predicciones, para ello se destina el 80 %, es decir, 502 imágenes.

  - **Test**: Este conjunto se utiliza para conocer el desempeño real que tiene el modelo, para ello se destina el 20 %, es decir, 126 imágenes.

Para el entrenamiento del modelo, se utiliza una función de costo, la cual le indica al modelo qué tanto ha aprendido del conjunto de entrenamiento, esta función está en términos de los parámetros y las características de entrada que en este caso son los pixeles de las imágenes. El principal objetivo es minimizar dicha función, ya que cuanto menor es su valor, menos errores está cometiendo en la predicción; sin embargo, es importante evitar el sobreajuste a los datos de entrenamiento, para ello se utiliza una técnica llamada regularización. Para llevar a cabo la minimización de la función de costo, se utiliza una técnica llamada "descenso del gradiente" la cual consiste básicamente en actualizar los parámetros del modelo en cada iteración sobre el conjunto de entrenamiento. Finalmente, cuando la función de costo deja de disminuir, el entrenamiento se detiene y se guarda el valor de los parámetros que hicieron que la función de costo se minimizara.
Finalizada la etapa de entrenamiento, el modelo está listo para ser testeado para conocer su desempeño en "condiciones reales", para posteriormente comenzar a realizar las predicciones.

### 2. Perceptrón Multicapa

El perceptrón multicapa es una red neuronal artificial formada por múltiples capas, de tal manera que tiene la capacidad para resolver problemas que no son linealmente separables [3].

En esta red cada neurona (excepto las de entrada) realiza calculos con las entradas que recibió de la capa de neuronas anterior a ella, dichos calculos incluye la aplicación de una función de activación, que en este caso es la función *relu*; posteriormente, el resultado de una neurona es enviado a la siguiente capa en la cual se repite el mismo procedimiento hasta llegar a la capa de salida donde se realizan los últimos calculos para finalmente entregar la predicción. La arquitectura del Perceptrón Multicapa (ver figura 2) le permite aprender los diferentes patrones que hay en las imágenes.

El perceptron multicapa tiene 3 tipos de capas:

- Capa de entrada: Constituida por aquellas neuronas que introducen los patrones de entrada en la red. En estas neuronas no se produce procesamiento [3].

- Capas ocultas: Formada por aquellas neuronas cuyas entradas provienen de capas anteriores y cuyas salidas pasan a neuronas de capas posteriores [3].

- Capa de salida: Neuronas cuyos valores de salida se corresponden con las salidas de toda la red [3].

![image](/images/RedNeuronalArtificial.png)

Figura 2. Perceptrón Multicapa [3].

En este caso, el perceptrón multicapa se construyó con la siguiente arquitectura:

- **Capa de entrada**: por medio de esta se ingresa la información de la imagen, por tanto, está constituida por 15360 neuronas (120 * 128 tomando un solo canal).

- **Capas ocultas**: mediante un ejercicio de ensayo y error, se logró determinar que 2 capas ocultas mostraban un buen desempeño; la primera capa oculta contiene 20 neuronas, y la segunda contiene 5.

- **Capa de salida**: Dado que nos encontramos en un problema de clasificación binaria, la capa de salida únicamente tiene 1 neurona.

Para el entrenamiento de este modelo, el conjunto de datos fue dividido en tres partes así:

- **Entrenamiento**: se destinó el 60 %, es decir, 376 imágenes.

- **Validación**: este conjunto se utilizó para reajustar los hiperparámetros y para hacer la interrupción anticipada del entrenamiento, con el fin de evitar el sobreajuste; para ello se destinó el 20 % de las imágenes, es decir, 126 imágenes.

- **Test**: se destinó el 20 %, es decir, 126 imágenes.

La ciencia detrás de la fase de entrenamiento del Perceptrón Multicapa es muy similar a la descrita en la Regresión Logística, con la excepción de que en la fase de entrenamiento del Perceptrón Multicapa se agregaron elementos como el entrenamiento por *mini - batches* lo cual permite entrenar la red más rápidamente; además, se agregó un solucionador denominado *adam* el cual ayuda a que la función de costo no se quede "atascada" en mínimos locales ni puntos de "silla"; también se aplicó una técnica para evitar el sobreajuste a los datos llamada "interrupción anticipada", el cual detiene el entrenamiento de la red cuando el desempeño en el conjunto de validación deja de mejorar.
Cuando la fase de entrenamiento termina, los parametros que hacen que se minimice el costo son guardados y el modelo está listo para ser testeado y realizar predicciones.


## **Resultados**

### 1. Regresión Logística

- **F1score**

Para medir el desempeño de este modelo, se utilizó la métrica F1score que se calcula como se muestra en la figura 3:

![image](/images/ec5.jpg)

Figura 3. F1score.

Donde:

- **Precision** (Precisión): es la razón entre los elementos que el modelo etiquetó como positivos y que en realidad lo eran, y todos los elementos que el modelo marcó como positivos; es decir, en nuestro contexto, es el resultado de dividir la cantidad de sujetos que el modelo predijo que tenían gafas y que en realidad sí las tenían entre todos los sujetos que el modelo predijo que tenían gafas (es decir,  los que etiquetó que tenían gafas de sol y en relidad sí las tenían más los que etiquetó que tenían gafas de sol que en realidad no las tenían); en otras palabras, dice qué tan bueno es el modelo para detectar a los sujetos que en realidad sí tienen gafas de sol sin confundirlos con aquellos que en realidad no tienen.

- **Recall** (Sensibilidad): es la razón entre los elementos que el modelo  etiquetó como positivos y que en realidad lo eran, y todos los elementos positivos; es decir, en nuestro contexto, es el resultado de dividir la cantidad de sujetos que el modelo predijo que tenían gafas de sol y que en realidad sí las tenían entre todos los sujetos que sí tenían gafas de sol (es decir, los que etiquetó que tenían gafas de sol y en relidad sí las tenían más los que etiquetó como que no tenían gafas de sol pero que en realidad sí las tenían); en otras palabras, dice qué tan bueno es el modelo para detectar a los sujetos que sí tienen gafas de sol de entre todos los que en realidad sí las estaban usando.

**F1score en entrenamiento Regresión Logística = 0.79**
**F1score en test Regresión Logística = 0.78**

Este resultado nos muestra un desempeño aceptable, aunque con algunas falencias.

- **Matriz de Confusión**

La matriz de confusión es una herramienta que nos ayuda a saber cómo está clasificando el modelo respecto a las diferentes clases o categorías de clasificación; más específicamente, nos ayuda a saber si el modelo está confundiendo a una clase con otra.

La matriz de confusión obtenida en entrenamiento en la regresión logística se muestra en la figura 4.

![image](/images/matriz_LR_train.png)
Figura 4. Matriz de Confusión de la Regresión Logística.

La matriz de confusión obtenida en test en la regresión logística se muestra en la figura 5.

![image](/images/Matriz1.jpg)

Figura 5. Matriz de Confusión de la Regresión Logística en test.

A partir de esta matriz, se puede decir que está clasificando relativamente bien a los sujetos que **no** están usando gafas de sol; sin embargo, está confundiendo a muchos sujetos (el 23 %) que usan gafas de sol con los que no las usan. Esto puede estar sucediendo debido a las diferentes posiciones de las cabezas de los sujetos y a que hay algunos sujetos que en las imágenes donde no usan gafas de sol sí utilizan gafas normales, lo cual puede suponer una dificultad para el modelo.

- **¿Qué está aprendiendo el modelo?**

Por medio de la figura 6 podemos ver gráficamente los coeficientes aprendidos por la regresión logística.

![image](/images/coef.jpg)

Figura 6. Coeficientes aprendidos por la Regresión Logística; colores más intensos representan coeficientes más grandes en magnitud, es decir, más importantes.

En esta imagen podemos ver claramente el contorno de los sujetos, además, en la zona de los ojos resalta un intenso color rojo, lo cual nos indica que dicha zona es la más importante para el modelo.

- **Demostración de funcionamiento**

En la figura 7 se observa varias pruebas de funcionamiento de la Regresión Logística con la respectiva predicción en la parte superior de cada imagen. De estas pruebas, falló solamente en una, ubicada en la parte superior derecha, en la cual predijo que el sujeto estaba usando gafas de sol cuando en realidad solo tenía gafas normales; es posible que la pata de las gafas hayan afectado la predicción.

![image](/images/Regresion.jpg)

Figura 7. Demostración de funcionamiento de la Regresión Logística.

### 2. Perceptrón Multicapa

- **F1score**

Al igual que con el modelo de Regresión Logística, con el Perceptrón Multicapa también se utilizó la métrica del F1score explicada anteriormente.

**F1score en entrenamiento del Perceptrón Multicapa = 0.98**
**F1score en test del Perceptrón Multicapa = 0.96**

Este resultado nos muestra un desempeño muy bueno, lo cual quiere decir que este modelo es tanto preciso como sensible, como se podrá comprobar en la matriz de confusión a continuación. Sin embargo, es preciso decir que en pruebas anteriores, el puntaje obtenido en el F1score oscilaba entre 0.85 y 0.95, esta variabilidad podría deberse a la inicialización aleatoria de los parámetros del modelo, a la división aleatoria del conjunto de datos entre los conjuntos de entrenamiento, validación y test, y a que el número de imágenes en el conjunto de datos es relativamente baja. Por lo tanto, es posible que el desempeño real del modelo en términos del F1score sea menor a 0.96.

- **Matriz de Confusión**

La matriz de confusión obtenida en el Perceptrón Multicapa en entrenamiento se muestra en la figura 8.

![image](/images/matriz_PM_train.jpg)

Figura 8. Matriz de Confusión en entrenamiento del Perceptrón Multicapa.

La matriz de confusión obtenida en el Perceptrón Multicapa en test se muestra en la figura 9.

![image](/images/Matriz2.jpg)

Figura 9. Matriz de Confusión en test del Perceptrón Multicapa.

A partir de esta matriz, se puede decir que está clasificando relativamente bien tanto a los sujetos que **sí** están usando gafas como a los que **no** lo hacen, ya que de aquellos sujetos que en realidad no usan gafas de sol solo está clasificando a 3 de ellos incorrectamente (es decir, el 6 %); similarmente, de aquellos sujetos que sí utilizan gafas de sol solo está clasificando mal a 3 de ellos (es decir, el 4 %).

- **Demostración de funcionamiento**

En la figura 10 se observa varias pruebas de funcionamiento del Perceptrón Multicapa con la respectiva predicción en la parte superior de cada imagen. En este caso, las imágenes de esta demostración son las mismas que las usadas en la Regresión Logística. 
Como se observa, todas las imágenes fueron etiquetadas correctamente.

![image](/images/Perceptron.jpg)

Figura 10. Demostración de funcionamiento del Perceptrón Multicapa.

## **Conclusiones**

En el área de la inteligencia artifical existen muchos modelos diferentes que pueden ser aplicables a un mismo problema, y este no es la excepción. Por tanto, cuando se tiene este gran abanico de opciones, unas de las habilidades más importantes del investigador es saber qué modelo se ajusta más a sus necesidades y saber, por supuesto, cómo configurar sus hiperparámetros para que su desempeño sea óptimo.

Durante este informe se mostró el funcionamiento y desempeño de dos modelos de clasificación binaria. El primero de ellos, la Regresión Logística, aunque tenía un desempeño aceptable, se quedaba bastante atrás a comparación de su contraparte, el Perceptrón Multicapa. ¿Qué pudo estar sucediendo?, como se mencionó anteriormente, la Regresión Logística funciona muy bien cuando las categorías de clasificación están separadas por un hiperplano, lo cual puede significar una limitación para este problema, ya que, por otro lado, el Perceptrón Multicapa tiene una mayor flexibilidad, lo cual le permite resolver problemas que no son linealmente separables, que, en consecuencia, le podría estar significando esa diferencia en el desempeño.

Finalmente, a partir de los resultados, podemos concluir que ambos modelos están generalizando relativamente bien, es decir, no solo funciona bien con los datos que ha aprendido, sino que también con los datos que el modelo no conocía.


## **Referencias**

[1] G. James, D. Witten, T. Hastie y R. Tibshirani, *An Introduction to Statistical Learning With Applications in R*, 2a ed. New York, NY: Springer, 2021.

[2]P. Gupta. "Regularization in Machine Learning". Medium. https://towardsdatascience.com/regularization-in-machine-learning-76441ddcf99a (accedido el 3 de septiembre de 2021).

[3] Colaboradores de los proyectos Wikimedia. "Perceptrón multicapa - Wikipedia, la enciclopedia libre". Wikipedia, la enciclopedia libre. https://es.wikipedia.org/wiki/Perceptrón_multicapa (accedido el 7 de agosto de 2021).






