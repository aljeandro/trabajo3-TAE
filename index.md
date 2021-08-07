## **Introducción** 

En el área de la inteligencia artificial y la estadística los problemas de clasificación son muy comúnes, incluso más que los de regresión. En estos se busca determinar a qué clase o categoría pertenece un individuo dadas ciertas características de este.

A menudo los individuos que requieren ser clasificados son imágenes; por ejemplo, en el área de la agronomía resulta útil saber si un fruto esta maduro o no, o, en el contexto de la pandemia del covid-19, podría ser de mucha ayuda reconocer cuándo una persona no está utilizando tapabocas por medio de una cámara de seguridad, todo esto sin la supervisión permanente de un humano.

## Planteamiento del problema

### **Descripción del conjunto de datos**

En este proyecto se lleva a cabo la clasificación de personas que están usando o no gafas de sol; para ello, se utiliza el conjunto de datos llamado *CMU Face Images Data Set*, el cual contiene 640 imágenes cuyo tamaño es de 120 filas por 128 columnas; además, dichas imágenes se encuentran en escala de grises.

Cada una de las imágenes está nombrada siguiendo la siguiente convención:

(id_usuario) (pose) (expresión) (ojos).pgm
  
  - (id_usuario): Corresponde a la identificación de la persona en la imagen, este campo tiene 20 valores en total: an2i, at33, boland, bpm, ch4f, cheyer, choon, danieln, glickman, karyadi, kawamura, kk49, megak, mitchell, night, phoebe, saavik, steffi, sz24, y tammo.
  
  - (pose): Es la posición de la cabeza de la persona, este campo puede contener 4 valores: straight (derecho), left (izquierda), right (derecha), up (arriba).
  
  - (expresión): Es la expresión facial de la persona, este campo puede contener 4 valores:  neutral (neutro), happy (feliz), sad (triste), angry (enojado).
  
  - (ojos): es el estado de los ojos de la persona, este campo puede tener dos valores: open (sin gafas), sunglasses (con gafas de sol).

A continuación, veamos algunas imágenes del conjunto de datos:

![image](/images/an2i_straight_neutral_sunglasses.png)

### **Preprocesamiento de las imágenes** 

Debido a un error en la cámara con la que se tomaron las fotografías, 12 de las imágenes se dañaron, por tanto, estas serán borradas manualmente.
  
  Como se mencionó anteriormente, todas las imágenes están en escala de grises, eso quiere decir que en su representación como matriz, en los 3 canales (R, G, B) tendrán el mismo valor, por lo tanto, para evitar la redundancia de información y optimizar el modelo, solamente se tomará la información de uno de los canales.

Posteriormente, la matriz que representa a cada imagen se "aplana" con el fin de que quede en dimensión 1.

Por otro lado, al ser aprendizaje supervisado, es necesario obtener un vector con la variable respuesta, para ello se utiliza la información contenida en el nombre de cada imagen, así que si el nombre de la imagen contiene la palabra *sunglasses*, entonces su respectiva etiqueta será 1, de lo contrario será 0.
  
  
  




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
