## **Introducción** 

En el área de la inteligencia artificial y la estadística los problemas de clasificación son muy comúnes, incluso más que los de regresión. En estos se busca determinar a qué clase o categoría pertenece un individuo dadas ciertas características de este.

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
  
  - (ojos): es el estado de los ojos de la persona, este campo puede tener dos valores: open (sin gafas), sunglasses (con gafas de sol).

A continuación, veamos algunas imágenes del conjunto de datos:

![image](/images/an2i_straight_neutral_sunglasses.png) ![image](/images/an2i_straight_sad_open.png) ![image](/images/bpm_right_angry_open.png) ![image](/images/bpm_right_angry_sunglasses.png) 

### Preprocesamiento de las Imágenes

Debido a un error en la cámara con la que se tomaron las fotografías, 12 de las imágenes se dañaron, por tanto, estas serán borradas manualmente.
  
  Como se mencionó anteriormente, todas las imágenes están en escala de grises, eso quiere decir que en su representación como matriz, en los 3 canales (R, G, B) tendrán el mismo valor, por lo tanto, para evitar la redundancia de información y optimizar el modelo, solamente se tomará la información de uno de los canales.

Posteriormente, la matriz que representa a cada imagen se "aplana" con el fin de que quede en dimensión 1.

Por otro lado, al ser aprendizaje supervisado, es necesario obtener un vector con la variable respuesta, para ello se utiliza la información contenida en el nombre de cada imagen, así que si el nombre de la imagen contiene la palabra *sunglasses*, entonces su respectiva etiqueta será 1, de lo contrario será 0.
  
  
  Por último, se normalizan las imágenes, dividiendo cada entrada de cada imagen entre la norma euclidiana de sus componentes.

## Elección y Configuración del Modelo

En este trabajo se probaron dos tipos distintos de modelos de clasificación, con el fin de determinar cuál de los dos ofrece un mejor desempeño:

### 1. Regresión Logística

La regresión logística (ver Figura 1) es un modelo que permite hacer predicciones desde una perspectiva probabilística, es decir, la respuesta siempre estará dada por un número entre 0 y 1, lo cual resulta útil para hacer clasificaciones. Sin embargo, su desempeño es alto solo si las dos clases o categorías de clasificación están separadas por un hiperplano, debido a su naturaleza lineal.

![image](/images/ec4.jpg)         
  Figura 1. Regresión Logística.

En este problema en específico, el vector X corresponde a la imagen, p es el número de pixeles, es decir 15360 (120 * 128 tomando un solo canal), y Y es la variable respuesta.

Para el entrenamiento de este modelo, el conjunto de datos fue dividido en dos partes así:

- **Entrenamiento**: este conjunto se utiliza para enseñarle al modelo a hacer las predicciones, para ello se destina el 80 %, es decir, 502 imágenes.
- **Test**: Este conjunto se utiliza para conocer el desempeño real que tiene el modelo, para ello se destina el 20 %, es decir, 126 imágenes.

### 2. Perceptron Multicapa

El perceptrón multicapa es una red neuronal artificial formada por múltiples capas, de tal manera que tiene la capacidad para resolver problemas que no son linealmente separables [1].

El perceptron multicapa tiene 3 tipos de capas:

- Capa de entrada: Constituida por aquellas neuronas que introducen los patrones de entrada en la red. En estas neuronas no se produce procesamiento [1].
- Capas ocultas: Formada por aquellas neuronas cuyas entradas provienen de capas anteriores y cuyas salidas pasan a neuronas de capas posteriores [1].
- Capa de salida: Neuronas cuyos valores de salida se corresponden con las salidas de toda la red [1].

![image](/images/RedNeuronalArtificial.png)

Figura 2. Perceptrón Multicapa [1].

En este caso, el perceptrón multicapa se construyó con la siguiente arquitectura:

- **Capa de entrada**: por medio de esta se ingresa la información de la imagen, por tanto, está constituida por 15360 neuronas (120 * 128 tomando un solo canal).
- **Capas ocultas**: mediante un ejercicio de ensayo y error, se logró determinar que 2 capas ocultas mostraban un buen desempeño; la primera capa oculta contiene 20 neuronas, y la segunda contiene 5.
- **Capa de salida**: Dado que nos encontramos en un problema de clasificación binaria, la capa de salida únicamente tiene 1 neurona.

Para el entrenamiento de este modelo, el conjunto de datos fue dividido en dos partes así:

- **Entrenamiento**: se destinó el 60 %, es decir, 376 imágenes.
- **Validación**: este conjunto se utilizó para reajustar los hiperparámetros y para hacer la interrupción anticipada del entrenamiento, con el fin de evitar el sobreajuste; para ello se destinó el 20 % de las imágenes, es decir, 126 imágenes.
- **Test**: se destinó el 20 %, es decir, 126 imágenes.

## **Resultados**

### 1. Regresión Logística





## **Referencias**

[1] Colaboradores de los proyectos Wikimedia. "Perceptrón multicapa - Wikipedia, la enciclopedia libre". Wikipedia, la enciclopedia libre. https://es.wikipedia.org/wiki/Perceptrón_multicapa (accedido el 7 de agosto de 2021).














You can use the [editor on GitHub](https://github.com/aljeandro/trabajo3-TAE/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/aljeandro/trabajo3-TAE/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
