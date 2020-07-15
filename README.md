# Interfaz Gráfica en Java

Curso propuesto por el grupo de trabajo Semana de Ingenio y Diseño (**SID**) de la Universidad Distrital Francisco Jose de Caldas.

## Monitor

**Cristian Felipe Patiño Cáceres** - Estudiante de Ingeniería de Sistemas de la Universidad Distrital Francisco Jose de Caldas

# Clase 13

## Objetivos

* Reconocer la forma de utilizar Canvas para pintar diferentes elementos en pantalla e identificar que objetos intervienen en este proceso.
* Identificar las particularidades que tiene cada elemento para pintarse en pantalla y su forma de crearse.
* Explicar el funcionamiento de las aréas en canvas para realizar distintas acciónes entre elementos que pintemos en pantalla.
* Hacer uso introductorio de Eventos del Mouse para pintar objetos en pantalla con una interacción del usuario.

# Antes de Comenzar

Para continuar con el ejercicio deberá actualizar la carpeta **resources/img** ya que se han agregado nuevas imágenes. Estas las puede descargar en este mismo repositorio entrando a la carpeta **Clase13** seguido de **resources/img**.

Recordando un poco nuestro recorrido en la clase anterior revisamos el uso de animaciones a traves del objeto **Timer**, dentro de la exploración de las animación, se vieron temas como **Animaciónes para interactividad, Animaciones para muestra de información, Particularidades de animaciónes y diferentes enfoques de animaciones**

<div align='center'>
    <img  src='./gifs/Animacion1.gif'>
    <p></p>
</div>

# Uso de Canvas para pintar en Interfaces Gráficas

En esta sesión vamos a ver el uso de **Canvas** para pintar varias cosas dentro de nuestras interfaces gráficas de una manera introductoria, para explorar a fondo este tema se verán varios items como:
* **Preparativos para crear el Canvas**.
* **Pintando sobre el Canvas (Figuras / Texto / Imágenes)**.
* **Uso de Areas dentro de Canvas**.
* **Uso de EventosMouseListener sobre Canvas**.

# Preparativos para crear el Canvas.

**Canvas** es un elemento característico de la librería **awt** de Java y es fundamental para la creación de figuras, animaciónes, imágenes y demás formas que no se podrían construir con los objetos gráficos comunes, hasta el momento los objetos gráficos que hemos manejado hacen parte de la librería **Swing** de Java, existen diversas opiniones con el uso combinado de estas dos librerías pero no hay de que preocuparse, al fin de cuentas **Swing** esta totalmente basada en la librería **awt** por lo que esta pensada desde el inicio a convivir con su antecesora. 

El uso de canvas es importante en la creación de **Juegos2D** ya que este elemento proporciona muchas libertades para plasmar en pantalla y ademas una gran optimización para animaciónes e interacciones con el usuario. También es usado para la representación de gráficas como diagramas de barras, diagrama pastel, lineas temporales, diagramas de Gantt etc. Canvas no es exclusivo de Java, otros lenguajes como **JavaScript** usan el mismo modelo y tienen los mismos principios para pintar en pantalla.

Vamos a crear un nuevo componente al cual llamaremos **Lienzo** dentro del paquete **components** como este será un nuevo componente se creara con sus respectivas clases.

<div align='center'>
    <img  src='https://i.imgur.com/bos5uLS.png'>
    <p>Creación del componente Lienzo</p>
</div>

Vamos a realizar la estructura básica del componentes, empezamos con la clase **LienzoComponent** y como se ha repetido muchas veces, se realizara, la inyección junto al método get correspondiente de su clase compañera.

* **Declaración clase Template:**
```javascript
// Dentro de la clase LienzoComponent
private LienzoTemplate lienzoTemplate;
```

* **Creación de inyección:**
```javascript
// Dentro del constructor
lienzoTemplate = new LienzoTemplate(this);
```

* **Método get de la clase template:**
```javascript
public LienzoTemplate getLienzoTemplate(){
    return lienzoTemplate;
}
```

Ahora vamos con la clase **LienzoTemplate**, vamos a extender de un objeto tipo **Canvas**, como este es un objeto gráfico es posible realizar la extensión hacia este tipo de objeto, se debe importar la librería para soportar este tipo de objetos:
```javascript
import java.awt.Canvas;
```
```javascript
public class LienzoTemplate extends Canvas{

}
```

Vamos a terminar de cellar la inyección:

* **Declaración de clase Component:**
```javascript
// Dentro de la clase LienzoTemplate
private LienzoComponent lienzoComponent;
```
* **Terminando de crear la inyección:**
```javascript
public LienzoTemplate(LienzoComponent lienzoComponent){
    this.lienzoComponent = lienzoComponent;
}
```

También vamos a necesitar del servició **RecursosService** para obtener varios colores y fuentes:

* **Declaración servicio:**
```javascript
private RecursosService sRecursos;
```

* **Obtención servicio:**
```javascript
// Dentro del constructor
this.sRecursos = RecursosService.getService();
```

Como este componente esta heredando de un **Canvas**, es necesario implementar un método especial típico de un canvas el cual es un **paint**, como estamos heredando y no implementando este método se debe implementar de forma manual y es importante que tenga el decorador **@Override** o de lo contrario para Java este método será otro método cualquiera:
```javascript
@Override
public void paint(Graphics g){

}
```
El decorador **@Override** indica que es un método implementado, como hemos visto con el uso de los eventos, esté método **Paint**  es un método especial que se ejecuta automáticamente una vez se llama a la clase **Template**, esto hace parte del ciclo de vida de un canvas y se ejecuta estrictamente después de que se ejecuten las instrucciones del constructor. Si no tiene el decorador mencionado el método será tomado como cualquier otro y se deberá ejecutar manualmente (llamando al método). Por otro lado este método recibe un parámetro de tipo **Graphics**, este es el objeto que se encargará de pintar en pantalla.

Ahora vamos a configurar las características físicas del componente:

```javascript
// Detro del constructor
this.setBounds(2, 2, 496, 566);
this.setBackground(Color.WHITE);
this.setVisible(true);
```

Se puede notar que las coordenadas del componente no están estrictamente al comienzo, están en la posición 2 tanto en X como en Y, esto es por que este lienzo será incorporado dentro del componente **Configuraciones** específicamente dentro de su panel **pDibujo**, recordando un poco este panel cuenta con un borde gris de 2 pixeles de grosor, es por esto que el canvas se posicionara desde este punto y ademas se le restaran 4 pixeles de tamaño con respecto al panel contenedor por este motivo, si se dejara desde la posición inicial y al mismo tamaño del panel **pDibujo** este borde quedaría tapado:

<div align='center'>
    <img  src='https://i.imgur.com/ZIWVIB7.png'>
    <p>Componente Configuraciones</p>
</div>

vamos a incorporar el nuevo componente **Lienzo** al componente **Configuraciones**, para esto nos ubicamos en la clase **ConfiguracionesTemplate** y dentro de está realizaremos la declaración, ejemplificación e incorporación:

* **Declaración:**
```javascript
private LienzoComponent lienzoComponent;
```

* **Ejemplificación:**
```javascript
// Dentro del constructor
this.lienzoComponent = new LienzoComponent();
```

* **Incorporación:**
```javascript
// Dentro del constructor después de la creación de los paneles
this.pDibujo.add(lienzoComponent.getLienzoTemplate());
```

Ahora podemos comprobar la incorporación de nuestro componente, sin embargo, pasa algo curioso, cuando pasamos encima del canvas, como este esta posicionado encima del panel **pDibujos** ya no se están escuchando los **eventos del Mouse**, tenemos varias opciones para solucionar este conflicto. Por ejemplo podemos pasar el código de los eventos del mouse en el hijo, sin embargo esto requiere una modificación del código que ya hemos implementado, para no realizar esta modificación podemos pasar la escucha de los mismos eventos desde el padre contenedor hacia el componente hijo.

Dentro de la clase **LienzoComponent** vamos a pedir como parámetro al panelContenedor, al mismo tiempo se lo vamos a pasar como argumento a la clase **Template**:

```javascript
public LienzoComponent(JPanel pPadre){
    lienzoTemplate = new LienzoTemplate(this, pPadre);
}
```

Esto implica que una vez se ejemplifique el componente dentro de la clase **ConfiguracionesTemplate** se debe pasar el panel que lo contiene:

```javascript
this.lienzoComponent = new LienzoComponent(pDibujo);
```

Ahora en la clase **LienzoTemplate** vamos a recibir como parámetro al panelPadre:
```javascript
public LienzoTemplate(LienzoComponent lienzoComponent, JPanel pPadre){
    ...
    ...
}
```

Vamos a agregar la escucha de cada uno de los eventos del Mouse al componente, para esto llamaremos los métodos correspondientes para la escucha de este tipo de eventos:
```javascript
// Dentro del constructor
this.addMouseListener();
this.addMouseMotionListener();
this.addMouseWheelListener();
```

Ahora vamos a obtener los eventos del mouse que esta escuchando actualmente el padre contenedor, esto lo haremos con los métodos:
* **getMouseListeners**.
* **getMouseMotionListeners**.
* **getMouseWheelListeners**.

Correspondientemente:

```javascript
// Dentro del constructor
this.addMouseListener(pPadre.getMouseListeners());
this.addMouseMotionListener(pPadre.getMouseMotionListeners());
this.addMouseWheelListener(pPadre.getMouseWheelListeners());
```

Ahora seguramente el editor de código nos este marcando un error y esto es por que los métodos anteriormente usados, no van a retornar un evento especifico sino un arreglo de eventos por cada categoría, ya que un objeto perfectamente podría escuchar a dos eventos tipo **MouseListener** por dar un ejemplo. Sin embargo sabemos que el panel **pDibujo** esta escuchando solo un evento de cada tipo asi que en los 3 casos vamos a necesitar el evento ubicado en la posición 0:

```javascript
// Dentro del constructor
this.addMouseListener(pPadre.getMouseListeners()[0]);
this.addMouseMotionListener(pPadre.getMouseMotionListeners()[0]);
this.addMouseWheelListener(pPadre.getMouseWheelListeners()[0]);
```

Ahora una vez abramos nuestra aplicación podemos comprobar que efectivamente ahora el canvas también es capaz de escuchar los eventos del mouse que habíamos configurado para el panel contenedor. Nuestro canvas esta listo para usarse, en la siguiente sección vamos a explicar la forma en como pintar diferentes elementos en pantalla.

<div align='center'>
    <img  src='https://i.imgur.com/ZIWVIB7.png'>
    <p>Componente Lienzo con la escucha de eventos del Mouse de su componente Padre.</p>
</div>

# Pintando sobre el Canvas.

Vamos a explicar a continuación como pintar una serie de figuras, ademas se explicará una serie de particularidades en los momentos pertinentes.

## Pintando Textos / Strings

Vamos a ubicarnos dentro del método **paint** y vamos a usar el objeto **Graphics: g**, este objeto tiene una serie de métodos que nos proporcionara herramientas para pintar sobre el canvas. Para pintar un **Texto** vamos a utilizar el método **drawString**.
Este método recibe varias cosas por parámetro:
* **String:** Texto que se va a pintar:
* **Posición X:** Posición del texto en el eje X dentro del canvas.
* **Posición Y:** Posición del texto en el eje Y dentro del canvas.

```javascript
@Override
public void paint(Graphics g){
    g.drawString("Rectangulos", (this.getWidth() / 2) - 50, 15);
}
```
Podemos comprobar que se ha pintado este texto dentro del canvas:
<div align='center'>
    <img  src='https://i.imgur.com/WwoW3Wo.png'>
    <p>Texto pintado dentro del String</p>
</div>

Ademas de pintar un texto podemos proporcionar algunos elementos para decorarlo, por ejemplo podemos darle un color y un tipo de fuente, para esto usaremos al servicio **sRecursos**:
```javascript
@Override
public void paint(Graphics g){
    g.setColor(sRecursos.getColorAzul());
    g.setFont(sRecursos.getFontTitulo());
    g.drawString("Rectangulos", (this.getWidth() / 2) - 50, 15);
}
```
Es importante que los métodos **setColor y setFont** vayan antes de pintar el String o cualquier figura, una vez cambiamos el color le estamos indicando al canvas que todo lo que se vaya a pintar de ahí en adelante tendrá ese color, si quisiéramos pintar mas adelante una figura con otro color se debe llamar nuevamente el método **setColor** y configurarle el color que se usará. Lo mismo pasaría en caso de querer cambiar la fuente:

<div align='center'>
    <img  src='https://i.imgur.com/PY8RMUF.png'>
    <p>String pintado con un color y fuente configurada.</p>
</div>

Vamos a pintar a continuación otros Strings dentro del canvas, pero para realizar una **Modularización de código** vamos a crear un nuevo método llamado **pintarStrings** que va a recibir como parámetro a un objeto tipo **Graphics** y vamos a llamarlo desde el método paint:
* **Método pintarStrings:**
```javascript
public void pintarStrings(Graphics g){
    g.setColor(sRecursos.getColorAzul());
    g.setFont(sRecursos.getFontTitulo());
    g.drawString("Rectangulos", (this.getWidth() / 2) - 50, 15);
    g.drawString("Arcos", (this.getWidth() / 2) - 23, 165);
    g.drawString("Óvalos y Polígonos", (this.getWidth() / 2) - 65, 315);
    g.drawString("Imágenes", (this.getWidth() / 2) - 35, 465);
}
```
**Llamada del método:**
```javascript
@Override
public void paint(Graphics g){
    this.pintarStrings(g);
}
```

<div align='center'>
    <img  src='https://i.imgur.com/SEcGAsA.png'>
    <p>Strings pintados dentro del canvas.</p>
</div>

## Pintando Rectángulos

Para pintar rectángulos y figuras en general dentro del canvas el objeto **g** proporcionan dos tipos de métodos:
* **draw**: Dibuja la figura sin un relleno, es decir solo pintara el contorno de la figura.
* **fill**: Dibuja la figura con relleno.

En ambos casos pintara la figura con el color que se ha configurado previamente.

Vamos a crear un método llamado **pintarRectangulos** de igual forma recibirá el objeto **Graphics** para poder pintarlos:
```javascript
public void pintarRectangulos(Graphics g){
    
}
```

Por precaución vamos a configurar el color, esto ya que posiblemente en algún otro punto se podría cambiar la configuración del color en el canvas:
```javascript
public void pintarRectangulos(Graphics g){
    g.setColor(sRecursos.getColorMorado());
}
```

Primero vamos a pintar un rectángulo con relleno para esto usaremos el método **fillRect** que recibe por parámetros:
* **Posición en X**.
* **Posición en Y**.
* **Ancho del rectángulo**.
* **Alto del rectángulo**.
```javascript
public void pintarRectangulos(Graphics g){
    g.setColor(sRecursos.getColorMorado());
    g.fillRect(160, 0, 200, 30);
}
```
Llamamos el método desde el método **paint**:
```javascript
// Dentro de Paint
this.pintarRectangulos(g);
```

<div align='center'>
    <img  src='https://i.imgur.com/lzvoZQ2.png'>
    <p>Rectángulo pintado dentro del canvas</p>
</div>

Se puede observar que el rectángulo se pinto de forma correcta y este tiene relleno, sin embargo este esta tapando al String que previamente habíamos pintado. Esto se hizo a propósito para revisar las particularidades con el **eje Z** y es que a diferencia de los objetos gráficos de **Swing** en el canvas a medida que vamos pintando un elemento, este se va a posicionar más al frente que los elementos previos, asi que hay que tener un cuidado especial con el eje Z en estos casos, vamos a cambiar de lugar y tamaño del rectángulo, ademas vamos a cambiar el color a azul nuevamente:
```javascript
public void pintarRectangulos(Graphics g){
    g.setColor(sRecursos.getColorAzul());
    g.fillRect(15, 65, 100, 50);
}
```
<div align='center'>
    <img  src='https://i.imgur.com/lfSVZom.png'>
    <p>Rectángulo con relleno pintado.</p>
</div>

Vamos a pintar un rectángulo esta vez sin relleno, para eso utilizaremos el método **drawRect** este recibe los mismos parámetros que el anterior método usados:
```javascript
public void pintarRectangulos(Graphics g){
    g.setColor(sRecursos.getColorAzul());
    g.fillRect(15, 65, 100, 50);
    g.drawRect(135, 40, 100, 100);
}
```

<div align='center'>
    <img  src='https://i.imgur.com/st2uZAW.png'>
    <p>Rectángulo sin relleno</p>
</div>

También podemos pintar rectángulos con la particularidad de tener los bordes redondeados, para hacer esto podemos usar los métodos **fillRoundRect o drawRoundRect** ambos van a pedir por parámetros:
* **Posición en X**.
* **Posición en Y**.
* **Ancho del rectángulo**.
* **Alto del rectángulo**.
* **Ancho del borde redondeado**.
* **Alto del borde redondeado**.

Por lo general se suele dejar el ancho y alto del borde del mismo tamaño y entre mas alto es el numero mas redondeado se vera el borde relativo al tamaño del rectángulo.
```javascript
public void pintarRectangulos(Graphics g){
    g.setColor(sRecursos.getColorAzul());
    g.fillRect(15, 65, 100, 50);
    g.drawRect(135, 40, 100, 100);
    g.fillRoundRect(255, 40, 100, 100, 20, 20);
    g.drawRoundRect(400, 40, 50, 100, 20, 20);
}
```

<div align='center'>
    <img  src='https://i.imgur.com/IFKedmF.png'>
    <p>Rectángulos con bordes redondeados pintados.</p>
</div>

## Pintando Lineas

Para dibujar lineas necesitamos el método **drawLine** que recibe como parámetros.
* **Coordenada en X del punto inicial**.
* **Coordenada en Y del punto inicial**.
* **Coordenada en X del punto final**.
* **Coordenada en Y del punto final**.

Vamos a crear el método **pintarLineas** que nuevamente recibirá el objeto **graphics**.
```javascript
public void pintarLineas(Graphics g){
}
```
Dentro del método vamos a configurar el color:

```javascript
public void pintarLineas(Graphics g){
    g.setColor(sRecursos.getColorMorado());
}
```

Vamos a pintar nuestra primera linea:

```javascript
public void pintarLineas(Graphics g){
    g.setColor(sRecursos.getColorMorado());
    g.drawLine(0, 150, 496, 150);
}
```
En este caso el punto inicial esta en las coordenadas **(0 , 150)** mientras su punto final esta en las coordenadas **(496 , 150)**. Se puede observar que en el punto inicial e final la coordenada en Y es la misma, esto quiere decir que sera una linea totalmente horizontal, sin embargo se pueden pintar lineas verticales y diagonales si se quiere.

<div align='center'>
    <img  src='https://i.imgur.com/zWtKGZg.png'>
    <p>Linea pintada en el canvas</p>
</div>

Pintaremos un par de lineas mas:
```javascript
public void pintarLineas(Graphics g){
    g.setColor(sRecursos.getColorMorado());
    g.drawLine(0, 150, 496, 150);
    g.drawLine(0, 300, 496, 300);
    g.drawLine(0, 450, 496, 450);
}
```
<div align='center'>
    <img  src='https://i.imgur.com/wRL0ink.png'>
    <p>Lineas pintadas en el Canvas</p>
</div>

## Pintando Arcos

Los arcos son figuras curvas que se pintan de acuerdo al posicionamiento basado en grados (°), estos pueden ser meramente curvas o una circunferencia entera en caso de pintar la figura sin relleno, pero en caso de pintar la figura con relleno va a mostrarse partes de un ovalo o circulo o en dado caso un ovalo o circulo completo.

Para dibujar esta figura podemos usar los métodos **fillArc y drawArc** que reciben por parámetros:
* **Posición en X**.
* **Posición en Y**.
* **Ancho de la figura**.
* **Alto de la figura**.
* **Posición en grados donde comienza el arco**.
* **Cantidad de grados que ocupa el arco**.

Vamos a dibujar algunos de estos arcos para comprender un poco el concepto, creamos primero el método **pintarArcos**:

```javascript
public void pintarArcos(Graphics g){
}
```
Vamos a configurar el color del canvas:
```javascript
public void pintarArcos(Graphics g){
    g.setColor(sRecursos.getColorAzul());
}
```

Ahora vamos a pintar un arco con relleno, en este caso solo queremos dibujar una cuarta parte de un circulo, para este caso se debe tener en cuenta varias cosas como:
* Al ser un circulo el ancho y el alto de la figura deben ser iguales.
* Como se quiere representar solo una cuarta parte de un circulo el grado inicial será 0° mientras que la cantidad de grados que ocupa el arco serán 90° (todo un circulo ocupa 360°).
```javascript
public void pintarArcos(Graphics g){
    g.setColor(sRecursos.getColorAzul());

    g.fillArc(15, 190, 100, 100, 0, 90);
}
```

Vamos a llamar este método dentro del **paint**:
```javascript
// Dentro del método paint
this.pintarArcos(g);
```

Nuestra interfaz se vera así:
<div align='center'>
    <img  src='https://i.imgur.com/PGnyqne.png'>
    <p>Pintando el primer arco</p>
</div>

Ahora queremos pintar su contra parte o espejo, es decir queremos pintar la otra cuarta parte del circulo que estaría ubicada en ela parte inferior izquierda, para lograr esto la figura debe tener el mismo tamaño y las mismas coordenadas de posición, pero deben cambiar su grado de inicio, este ahora debería iniciar en el grado 180°, en cuanto a los grados que ocupa, como sera de nuevo una cuarta parte ocupará los mismos 90°.
```javascript
public void pintarArcos(Graphics g){
    g.setColor(sRecursos.getColorAzul());

    g.fillArc(15, 190, 100, 100, 0, 90);
    g.fillArc(15, 190, 100, 100, 180, 90);
}
```

<div align='center'>
    <img  src='https://i.imgur.com/AHTh5rb.png'>
    <p>Segundo arco pintado</p>
</div>

Dibujando arcos como este podemos pintar diagramas pastel para representar datos de forma estadística por dar un ejemplo practico de este tipo de figuras. Para terminar de pintar las otras 2 cuartas partes del circulo vamos a cambiar el color del canvas y los dibujaremos usando la misma lógica.

```javascript
public void pintarArcos(Graphics g){
    g.setColor(sRecursos.getColorAzul());

    g.fillArc(15, 190, 100, 100, 0, 90);
    g.fillArc(15, 190, 100, 100, 180, 90);
    g.setColor(Color.ORANGE);
    g.fillArc(15, 190, 100, 100, 90, 90);
    g.fillArc(15, 190, 100, 100, 270, 90);
}
```

<div align='center'>
    <img  src='https://i.imgur.com/ORYN8eL.png'>
    <p>Dibujando el resto del circulo.</p>
</div>

Vamos a dibujar otro circulo con la misma lógica, pero esta vez sin relleno para crear solo el contorno de la circunferencia:
```javascript
// Dentro del método pintarArcos
g.setColor(sRecursos.getColorAzul());
g.drawArc(135, 190, 100, 100, 0, 90);
g.drawArc(135, 190, 100, 100, 180, 90);
g.setColor(Color.ORANGE);
g.drawArc(135, 190, 100, 100, 90, 90);
g.drawArc(135, 190, 100, 100, 270, 90);
```

<div align='center'>
    <img  src='https://i.imgur.com/XT7Y8VX.png'>
    <p>Dibujando arcos sin relleno</p>
</div>

Vamos a dejar otros ejemplos de arcos:
```javascript
// Dentro del método pintarArcos
g.setColor(sRecursos.getColorAzul());
g.fillArc(255, 190, 100, 100, 0, 180);

g.drawArc(375, 190, 100, 100, -15, 90);
g.setColor(Color.ORANGE);
g.drawArc(375, 190, 100, 100, 105, 90);
g.setColor(sRecursos.getColorMorado());
g.drawArc(375, 190, 100, 100, 225, 90);
```

<div align='center'>
    <img  src='https://i.imgur.com/Q4johow.png'>
    <p>Otros arcos dibujados</p>
</div>

Antes de continuar dibujando otras figuras podemos darnos cuenta que algunas figuras tienen un aspecto "pixeleado", esto se nota sobre todo en las curvas, como en los arcos o en los bordes redondeados de los recuadros, realmente no queremos que nuestras figuras se vean de esta manera. Podemos ayudarnos de un objeto derivado de Graphics que contiene métodos encargados de la mejora en la calidad del pintado, este objeto es **Graphics2D**. Vamos a crear este objeto para mejorar el aspecto, primero vamos a declarar este objeto:
* **Declaración:**
```javascript
private Graphics2D g2d;
```
Ahora vamos a ejemplificar este objeto, pero como mencionamos este se deriva del objeto **Graphics** por lo que este se creara con el método **create**. También se puede derivar de otros objetos como **Images**, esta ejemplificación la vamos a realizar al comienzo del método **paint** antes de llamar a los métodos de pintado:

```javascript
// Dentro del método paint al comienzo
g2d = (Graphics2D) g.create();
```

Ya tenemos nuestro objeto **Graphics2D** ejemplificado y listo para usar, este objeto cuenta con varios algoritmos de renderizado para mejorar la calidad de lo que se esta pintando en pantalla, para invocar algunos de estos debemos llamar al método **setRenderingHint** y usar al objeto especial de **awt RenderingHints** quien es el objeto que proporciona estos algoritmos, vamos a usar dos de ellos:

```javascript
// Dentro del método paint
g2d.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
g2d.setRenderingHint(RenderingHints.KEY_STROKE_CONTROL, RenderingHints.VALUE_STROKE_NORMALIZE);
```

* **KEY_ANTIALIASING**: Proporciona un difuminado sutil para que el usuario no persiva una pixelizacion en lo que se esta pintando. Para activarlo debemos llamar al parámetro **VALUE_ANTIALIAS_ON**.
* **KEY_STROKE_CONTROL**: Es un algoritmo de control que se encarga de activar o desactivar el anterior método **KEY_ANTIALIASING** solo en caso de ser necesario, por ejemplo para lineas rectas el uso de este algoritmo no será necesario, asi que esto reduce el consumo de recursos innecesarios, se activa con el parámetro **VALUE_STROKE_NORMALIZE**.

Ahora vamos a pintar todas nuestras figuras con este objeto por lo que vamos a cambiar el parámetro que reciben nuestros métodos de pintado:

```javascript
 public void pintarStrings(Graphics2D g2d){
     ...
 }
 public void pintarRectangulos(Graphics2D g2d){
     ...
 }
 public void pintarLineas(Graphics2D g2d){
     ...
 }
 public void pintarArcos(Graphics2D g2d){

 }
```

Dentro de cada método de pintado en vez de usar el objeto g (Graphics) para pintar ahora usaremos el objeto g2d (Graphics2D).
<div align='center'>
    <img  src='https://i.imgur.com/lbKWZ1B.png'>
    <p>Usando el objeto Graphics2D</p>
</div>

Para finalizar, una vez se llamen cada uno de los métodos de pintado vamos a enviar como argumento al objeto Graphics2D:
```javascript
this.pintarStrings(g2d);
this.pintarRectangulos(g2d);
this.pintarLineas(g2d);
this.pintarArcos(g2d);
```

Veamos como luce nuestro canvas ahora:
<div align='center'>
    <img  src='https://i.imgur.com/h0s2p2R.png'>
    <p>Figuras pintadas con una mejor calidad.</p>
</div>

Noten que las figuras mejoraron su calidad significativamente y no solo estas, los Strings también tienen una mejor definición de pintado.

## Pintando Óvalos

Los óvalos son circunferencias que se pintan de forma similar a los rectángulos, esto debido a que recibe parámetros similares:
* **Posición en X**.
* **Posición en Y**.
* **Ancho del ovalo**.
* **Alto del ovalo**.

Y también pueden ser pintados mediante los métodos **draw o fill**.

Creamos el método **pintarOvalos**, de una vez vamos a pintarlo con el objeto **Graphics2D**:
```javascript
public void pintarOvalos(Graphics2D g2d){

}
```

Configuramos el color al canvas:
```javascript
public void pintarOvalos(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
}
```

Vamos a dibujar un circulo primero es decir un óvalo con mismas dimensiones de ancho y alto, ademas vamos a dibujarlo con relleno:
```javascript
public void pintarOvalos(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.fillOval(15, 340, 100, 100);
}
```

Vamos a llamar al método desde **paint** pasandole como argumento el objeto **Graphics2D**:
```javascript
// Dentro del método paint
this.pintarOvalos(g2d);
```

<div align='center'>
    <img  src='https://i.imgur.com/SeI7C5g.png'>
    <p>Dibujando el primer ovalo</p>
</div>

Ahora vamos a dibujar dos óvalos con solo su contorno, para esto usaremos el método draw:
```javascript
// Dentro del método pintarOvalos
g2d.drawOval(160, 340, 50, 100);
g2d.drawOval(135, 365, 100, 50);
```
<div align='center'>
    <img  src='https://i.imgur.com/dsRruTT.png'>
    <p>Dibujando dos óvalos</p>
</div>

## Pintando Polígonos

Los polígonos son figuras abstractas que podemos crear de acuerdo a un sistema de puntos, nosotros podemos indicar cuantos puntos tendrá y las coordenadas de cada uno de estos puntos, el sistema se encargará de unir todos los puntos para crear la figura. Esto quiere decir que para dibujar un polígono se usara los métodos **fillPolygon o drawPolygon** y va a recibir por parámetros:
* **Array de enteros int[]:** Arreglo que representa las coordenadas de cada uno de los puntos de la figura en el eje X.
* **Array de enteros int[]:** Arreglo que representa las coordenadas de cada uno de los puntos de la figura en el eje Y.
* **Cantidad de puntos:** El numero de puntos que tiene la figura.

De esta forma la coordenada en X que esta ubicada en la posición [0] de su arreglo se complementará con la coordenada en Y que esta ubicada en la posición [0] en su respectivo arreglo.

Vamos a crear el método pintarPoligonos y de una vez vamos a configurar el color al canvas: 
```javascript
public void pintarPoligonos(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
}
```

Vamos a crear nuestro primer polígono y en este caso crearemos un **Triangulo**, este tendrá entonces 3 puntos, lo pintaremos con relleno asi que primero llamamos al método **fillPolygon**:
```javascript
public void pintarPoligonos(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.fillPolygon();
}
```

Vamos a configurar los parámetros necesarios, que son los dos arreglos para las coordenadas en cada eje y el numero de puntos que tiene la figura:

```javascript
public void pintarPoligonos(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.fillPolygon(
        new int[]{}, new int[]{}, 3
    );
}
```

Ya le pasamos los parámetros necesarios pero ahora en cada uno de los arreglos debemos configurar las coordenadas de cada uno de los 3 puntos, empezamos con el punto de arriba y este será nuestra guiá para configurar las coordenadas de los otros 2 puntos.
```javascript
public void pintarPoligonos(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.fillPolygon(
        new int[]{305}, new int[]{340}, 3
    );
}
```
En el anterior código estamos indicando que las coordenadas del primer punto son (305 en x, 340 en y). De acuerdo a estas coordenadas iniciales vamos a realizar los otros dos puntos, teniendo en cuenta que este punto es el que estará en la parte superior central del triangulo podríamos configurar el punto que esta mas a la izquierda y en la parte inferior.
```javascript
public void pintarPoligonos(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.fillPolygon(
        new int[]{305, 255}, new int[]{340, 440}, 3
    );
}
```

Aquí configuramos el segundo punto que esta en las coordenadas (255 en x, 440 en y) y si se dan cuenta restamos pixeles en el eje X por lo que estará 50px mas a la izquierda, por otro lado en el eje Y le sumamos pixeles, esto quiere decir que estará abajo por 100px más. Ahora solo falta configurar el siguiente punto:

```javascript
public void pintarPoligonos(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.fillPolygon(
        new int[]{305, 255, 355}, new int[]{340, 440, 440}, 3
    );
}
```
Aquí configuramos el tercer punto que esta en las coordenadas (355 en x, 440 en y) y si se dan cuenta sumamos pixeles en el eje X por lo que estará 50px mas a la derecha con respecto al punto del medio, mientras que en el eje Y lo dejamos igual que el anterior punto. 
Para ver la figura vamos a llamar al método desde **paint**:
```javascript
// Dentro del método paint
this.pintarPoligonos(g2d);
```
<div align='center'>
    <img  src='https://i.imgur.com/dhVy9ii.png'>
    <p>Pintando el primer polígono: triangulo</p>
</div>

A continuación vamos a realizar un Octágono con la misma lógica antes explicada pero con la diferencia de que no tendrá relleno:
```javascript
// Dentro del método pintarPoligono
g2d.drawPolygon(
    new int[]{400, 450, 475, 475, 450, 400, 375, 375}, 
    new int[]{340, 340, 365, 415, 440, 440, 415, 365}, 
    8
);
```
<div align='center'>
    <img  src='https://i.imgur.com/qQCPJbf.png'>
    <p>Octágono dibujado en el canvas</p>
</div>

## Pintando Imágenes 

En el canvas también podemos dibujar imágenes, para esto debemos hacer uso del método **drawImage**. Aunque podemos pintar una imagen con el propósito de mostrarla como se haría comúnmente con los Labels, usualmente en canvas y en el desarrollo de videojuegos se toma otro enfoque, ya que se suelen usar **Sprites**, primero explicaremos la forma de pintar una imagen normalmente y luego entraremos a mas detalle con el otro enfoque. Primero creamos el método **pintarImagenes**:
```javascript
public void pintarImagenes(Graphics2D g2d){
    
}
```

Lo primero que vamos a hacer es traernos la imagen a traves de un **ImageIcon**, asi que vamos a declarar un objeto de este tipo y de una vez lo vamos a ejemplificar:
```javascript
// Al inicio de la clase LienzoTemplate
private ImageIcon imagen = new ImageIcon("Clase13/resources/img/sonic.png");
```

Dentro del método **pintarImagenes** vamos a pintar la imagen con el método **drawImagen**, este puede recibir multiples parámetros, pero en esta ocasión vamos a usar el que recibe los parámetros básicos:
* **Imagen**: Esta debe ser de tipo **Image** es decir que es necesario decirle a nuestro objeto **ImageIcon** que queremos obtener su imagen con el método **getImage()**.
* **Posición en X**. 
* **Posición en Y**. 
* **Ancho de la Imagen**. 
* **Alto de la Imagen**. 
* **Observable:** Es un objeto que notifica de algún modo en caso de que exista una modificación de una imágen, se suele utilizar en animaciónes asi que por ahora se va a dejar en null.

```javascript
public void pintarImagenes(Graphics2D g2d){
    g2d.drawImage(imagen.getImage(), 230, 480, 40, 80, null);
}
```

Llamamos al método desde **paint**:
```javascript
// Dentro del método paint
this.pintarImagenes(g2d);
```

<div align='center'>
    <img  src='https://i.imgur.com/kNIarxQ.png'>
    <p>Pintando una imagén</p>
</div>

Sin embargo como hemos mencionado muchas veces pintar imágenes dentro del Canvas implica que se querrá realizar una animación y para esto los desarrolladores se basan en **Sprites**, vamos a mostrar la imagén que queremos pintar para explicar mejor el concepto:
<div align='center'>
    <img  src='https://i.imgur.com/dtDRlaJ.png'>
    <p>Sprite de Sonic</p>
</div>

Como se dan cuenta un **Sprite** consiste en una secuencia que da paso a una animación, en este caso no nos vamos a enfocar en animaciones dentro del canvas pero si a la metodología que se utiliza con el pintado de este tipo de imágenes, por lo general en este tipo de casos no se busca pintar toda la imágen de una vez, mas bien se busca recorrer de alguna forma la imagen para ir mostrando cada parte de la animación.

Para pintar solo un tramo de la imágen general podemos utilizar el método **drawImage** nuevamente pero esta vez vamos a necesitar mas parámetros:

* **Posición inicial en X**. 
* **Posición inicial en Y**. 
* **Posición final en X**. 
* **Posición final en Y**. 
* **Posición en X inicial sobre la imagen General**.
* **Posición en Y inicial sobre la imagen General**.
* **Posición en X final sobre la imagen General**.
* **Posición en Y final sobre la imagen General**.
* **Observable:** Por ahora se va a dejar en null.

Noten que ahora se tienen 4 parámetros nuevos y también algunos de ellos han cambiado. Por ejemplo ya no vamos a manejar el ancho y alto de la imagen, vamos a jugar con la posición inicial y final de la imagen dentro del canvas, siendo la posición inicial la coordenada de la **parte superior izquierda**, mientras que la posición final será la de la parte **inferior derecha** de la imágen.
 
Por otra parte, con los 4 parámetros nuevos que estamos añadiendo es como si estuviéramos creando un cuadro imaginario dentro de la imágen general y solo pintará lo que esta dentro de ese cuadro: 
<div align='center'>
    <img  src='https://i.imgur.com/qisZrOE.png'>
    <p>Analogía de lo que sucede con los nuevos parámetros</p>
</div>

vamos a pintar nuestra primer imágen usando este enfoque, primero debemos obtener la imágen que contiene el **Sprite** asi que declararemos y ejemplificaremos un **ImageIcon** para obtener la imagen:
```javascript
// Al inicio de la clase LienzoTemplate
private ImageIcon sprites = new ImageIcon("Clase13/resources/img/sprites.png");
```
***Nota:** El otro ImageIcon ya no se usara mas asi que se puede borrar su declaración y ejemplificación.*

Vamos a dejar nuevamente vació el método pintarImagenes:
```javascript
public void pintarImagenes(Graphics2D g2d){
}
``` 
Ahora vamos a pintar nuestra primer imágen con el enfoque mencionado:

```javascript
public void pintarImagenes(Graphics2D g2d){
    g2d.drawImage(
        sprites.getImage(), 90, 490, 135, 550, 0, 0, 45, 60,  null
    );
}
```

En el anterior código estamos indicando:
* La imágen se ubicara en la posición (90 en x, 490 en y) del Canvas.
* La imagen tendrá una posición final en las coordenadas (135 en x, 550 en y) del Canvas.
* Lo anterior quiere decir que la imágen tendrá un ancho de **45px** y un alto de **60px** (135 - 90, 550 - 490).
* Sobre la imagen general se creará un cuadro imaginario que estará ubicado inicialmente en las coordenadas (0, 0) de la imágen.
* Este cuadro imaginario terminará en las coordenadas (45, 60) de la imágen.
* Esto quiere decir que el recuadro tendrá el mismo ancho que la imágen que se mostrará en el canvas.

Vamos a mostrar que sucede:
<div align='center'>
    <img  src='https://i.imgur.com/iT4F7rH.png'>
    <p>Pintando primera parte del Sprite.</p>
</div>

Vamos a pintar la segunda parte de la imágen:
```javascript
public void pintarImagenes(Graphics2D g2d){
    g2d.drawImage(
        sprites.getImage(), 90, 490, 135, 550, 0, 0, 45, 60,  null
    );
    g2d.drawImage(
        sprites.getImage(), 135, 490, 180, 550, 45, 0, 90, 60,  null
    );
}
```

<div align='center'>
    <img  src='https://i.imgur.com/S6QE7EU.png'>
    <p>Pintando segunda parte del Sprite</p>
</div>

Ahora para no estar pintando cada parte por separado podemos encontrar un patron entre las dos imágenes que pintamos para hacer la automatización del dibujo de las imágenes sobre la pantalla.

Podemos notar que el patron va en aumento en **45px** sobre todas las coordenadas que involucran el eje X tanto para la posición en el canvas de la imágen como en la posición del recuadro dentro de la imágen general:

<div align='center'>
    <img  src='https://i.imgur.com/Q3A9cl7.png'>
    <p>Patron identificado.</p>
</div>

Vamos a borrar las anteriores imágenes pintadas y vamos a crear un ciclo for para automatizar el dibujo de todas las imágenes, como en el **Sprite** hay contenidas 7 imágenes el ciclo empezara desde 0 y terminara en 7-1:
```javascript
public void pintarImagenes(Graphics2D g2d){
    for(int i = 0; i < 7; i++){
    }
}
```

Dentro del ciclo y por cada iteración vamos a dibujar las imágenes:

```javascript
public void pintarImagenes(Graphics2D g2d){
    for(int i = 0; i < 7; i++){
        g2d.drawImage();
    }
}
```

Como el patron consiste en sumar 45 a medida que incrementan las imágenes, y nos podemos guiar de la primera imágen, sabemos que esta:

* Inicia en la posición 90px sobre el eje X del canvas.
* Termina en la posición 135px del eje X del canvas.
* Inicia en la posición 0px sobre el eje X de la imágen.
* Termina en la posición 45px del eje X de la imágen.

Asi que basta con sumarle a esas coordenadas (45 * i) de tal forma que para la primera imágen se le sumara 0 ya que el i inicia en 0:

```javascript
public void pintarImagenes(Graphics2D g2d){
    for(int i = 0; i < 7; i++){
        g2d.drawImage(
            sprites.getImage(), 
            90 + (45 * i), 490, 135 + (45 * i), 550, 
            0 + (45 * i), 0, 45 + (45 * i), 60,  
            null
        );
    }
}
```
De esta forma se verán pintadas nuestras imágenes:
<div align='center'>
    <img  src='https://i.imgur.com/X4m3u07.png'>
    <p>Imágenes pintadas en el Canvas.</p>
</div>

# Uso de Areas dentro de Canvas

Las **areas y objetos gráficos especiales de figuras** son también una parte importante del canvas, gracias a estos podríamos realizar una manipulación de las figuras pintadas dentro del canvas, por ejemplo podríamos seleccionar algún circulo y mover la posición de esto (con el uso de eventos claro). Con las Areas también se pueden realizar operaciones como la **sustracción o combinación** de figuras, esto es importante y realizaremos un ejemplo de esto. 

Por ahora vamos a comentar la llamada de todos los otros métodos de pintura para dejar vació el canvas:

```javascript
@Override
public void paint(Graphics g){
    g2d = (Graphics2D) g.create();
    g2d.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
    g2d.setRenderingHint(RenderingHints.KEY_STROKE_CONTROL, RenderingHints.VALUE_STROKE_NORMALIZE);
    // this.pintarStrings(g2d);
    // this.pintarRectangulos(g2d);
    // this.pintarLineas(g2d);
    // this.pintarArcos(g2d);
    // this.pintarOvalos(g2d);
    // this.pintarPoligonos(g2d);
    // this.pintarImagenes(g2d);
}
```

Vamos a crear un método nuevo llamado **pintarAreas**:
```javascript
public void pintarAreas(Graphics2D g2d){

}
```

Para realizar el ejemplo de combinación y sustracción vamos a dibujar primero un rectángulo y vamos a suponer que este podría ser un panel cualquiera con un contenido que sera un texto:

```javascript
public void pintarAreas(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.fillRect(80, 30, 340, 340);

    g2d.setColor(Color.WHITE);
    g2d.setFont(sRecursos.getFontTitulo());
    g2d.drawString("Soy un Panel", 200, 200);
}
```
<div align='center'>
    <img  src='https://i.imgur.com/IbxaVJM.png'>
    <p>Simulando la creación de un panel dentro del canvas</p>
</div>

Ya tenemos nuestro panel simulado ahora supongamos que queremos que nuestro panel tenga un borde redondeado, sabemos que con los bordes que nos proporciona **Swing** no es posible realizar este tipos de bordes, asi que podríamos crear nuestro propio borde redondeado pintándolo.

Para realizar esto NO basta con pintar un recuadro redondeado encima, de echo este tapara lo que esta contenido en el panel, debemos crear Areas para manipulación de estas figuras. Para trabajar con areas no podemos pintar las figuras como lo trabajamos anteriormente, debemos crear **ObjetosGráficos especiales de Figuras**. A modo de identificación, las figuras que creemos de ahora en adelante las pintaremos de otro color, pero cuando se trabaja con areas la forma de cambiar el color es con el método **setPaint**.

```javascript
public void pintarAreas(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.fillRect(80, 30, 340, 340);

    g2d.setColor(Color.WHITE);
    g2d.setFont(sRecursos.getFontTitulo());
    g2d.drawString("Soy un Panel", 200, 200);
    
    g2d.setPaint(Color.ORANGE);
}
```

Vamos a crear un nuevo rectángulo redondeado en forma de objeto para esto necesitamos ejemplificarlo con la clase **RoundRectangle2D**:
```javascript
public void pintarAreas(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.fillRect(80, 30, 340, 340);

    g2d.setColor(Color.WHITE);
    g2d.setFont(sRecursos.getFontTitulo());
    g2d.drawString("Soy un Panel", 200, 200);
    
    g2d.setPaint(Color.ORANGE);
    RoundRectangle2D rectanguloRedondeado = new RoundRectangle2D.Double();
}
```
Para ejemplificar este objeto necesitamos importar su respectiva librería:
```javascript
import java.awt.geom.RoundRectangle2D;
```

Ya ejemplificamos nuestro objeto, ahora es necesarió indicarle sus propiedades gráficas mediante el método **setRoundRect**, recordando que sus propiedades gráficas son:
* **Posición en X**.
* **Posición en Y**.
* **Ancho**.
* **Alto**.
* **Ancho Borde redondeado**.
* **Alto Borde redondeado**.

```javascript
public void pintarAreas(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.fillRect(80, 30, 340, 340);

    g2d.setColor(Color.WHITE);
    g2d.setFont(sRecursos.getFontTitulo());
    g2d.drawString("Soy un Panel", 200, 200);
    
    g2d.setPaint(Color.ORANGE);
    RoundRectangle2D rectanguloRedondeado = new RoundRectangle2D.Double();
    rectanguloRedondeado.setRoundRect(80, 30, 340, 340, 100, 100);
}
```

Ya ejemplificamos nuestro objeto y le dimos sus propiedades gráficas, ahora debemos añadirlo al canvas. En ves de pintarlo directamente vamos a crear un **Area** que recibe por parámetro al recuadro, de esta forma vamos a poder manipular el area:

```javascript
public void pintarAreas(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.fillRect(80, 30, 340, 340);

    g2d.setColor(Color.WHITE);
    g2d.setFont(sRecursos.getFontTitulo());
    g2d.drawString("Soy un Panel", 200, 200);
    
    g2d.setPaint(Color.ORANGE);
    RoundRectangle2D rectanguloRedondeado = new RoundRectangle2D.Double();
    rectanguloRedondeado.setRoundRect(80, 30, 340, 340, 100, 100);

    Area area = new Area(rectanguloRedondeado);
}
```

Ahora vamos a pintar el area con el método **draw** del objeto **graphics2D**:
```javascript
public void pintarAreas(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.fillRect(80, 30, 340, 340);

    g2d.setColor(Color.WHITE);
    g2d.setFont(sRecursos.getFontTitulo());
    g2d.drawString("Soy un Panel", 200, 200);
    
    g2d.setPaint(Color.ORANGE);
    RoundRectangle2D rectanguloRedondeado = new RoundRectangle2D.Double();
    rectanguloRedondeado.setRoundRect(80, 30, 340, 340, 100, 100);

    Area area = new Area(rectanguloRedondeado);
    g2d.draw(area);
}
```
<div align='center'>
    <img  src='https://i.imgur.com/Sl2Fi2X.png'>
    <p>Objeto de rectangulo redondeado pintado</p>
</div>

Ya tenemos pintado el contorno del borde que queremos crearle a nuestro panel, lo que queremos hacer es que se borre los espacios azules que sobran en cada esquina, ahora vamos a crear otro objeto, este será un rectangulo que ocupara el espacio total del panel esto mediante la ejemplificación de un objeto tipo **Rectangle**:

```javascript
public void pintarAreas(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.fillRect(80, 30, 340, 340);

    g2d.setColor(Color.WHITE);
    g2d.setFont(sRecursos.getFontTitulo());
    g2d.drawString("Soy un Panel", 200, 200);
    
    g2d.setPaint(Color.ORANGE);
    RoundRectangle2D rectanguloRedondeado = new RoundRectangle2D.Double();
    rectanguloRedondeado.setRoundRect(80, 30, 340, 340, 100, 100);

    Area area = new Area(rectanguloRedondeado);

    Rectangle rectangulo = new Rectangle(80, 30, 340, 340);

    g2d.draw(area);
}
```

Vamos a añadir este rectangulo a una nueva area, que en este caso llamaremos **areaFueraBorde**:
```javascript
public void pintarAreas(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.fillRect(80, 30, 340, 340);

    g2d.setColor(Color.WHITE);
    g2d.setFont(sRecursos.getFontTitulo());
    g2d.drawString("Soy un Panel", 200, 200);
    
    g2d.setPaint(Color.ORANGE);
    RoundRectangle2D rectanguloRedondeado = new RoundRectangle2D.Double();
    rectanguloRedondeado.setRoundRect(80, 30, 340, 340, 100, 100);

    Area area = new Area(rectanguloRedondeado);

    Rectangle rectangulo = new Rectangle(80, 30, 340, 340);
    Area areaFueraBorde = new Area(rectangulo);

    g2d.draw(area);
}
```

Ahora vamos a hacer el acto de **sustracción**, con ambas Areas, la Sustracción hará que el area de sustracción (**areaFueraBorde**) se transforme por la parte que le sobra con respecto al área que esta sustrayendo (**area**), esto quiere decir que el area **areaFueraBorde** representará ahora las partes sobrantes en cada esquina:
```javascript
public void pintarAreas(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.fillRect(80, 30, 340, 340);

    g2d.setColor(Color.WHITE);
    g2d.setFont(sRecursos.getFontTitulo());
    g2d.drawString("Soy un Panel", 200, 200);
    
    g2d.setPaint(Color.ORANGE);
    RoundRectangle2D rectanguloRedondeado = new RoundRectangle2D.Double();
    rectanguloRedondeado.setRoundRect(80, 30, 340, 340, 100, 100);

    Area area = new Area(rectanguloRedondeado);

    Rectangle rectangulo = new Rectangle(80, 30, 340, 340);
    Area areaFueraBorde = new Area(rectangulo);
    areaFueraBorde.subtract(area);
    
    g2d.draw(area);
}
```

Si corremos la aplicación no se notara ningún cambio pero internamente sabemos que ahora el area **areaFueraBorde** representa las esquinas sobrantes:
<div align='center'>
    <img  src='https://i.imgur.com/vEMX6O6.png'>
    <p>AreaFueraBorde antes y después de la sustracción</p>
</div>

Ahora queremos que esa area no sea visible. Para esto vamos a configurar el canvas dándole un contexto, esto quiere decir que se le indicara al canvas que lo que se pinte de ahora en adelante se pintara unicamente sobre el contexto indicado, para hacer esto necesitamos llamar al método **setClip**:

```javascript
public void pintarAreas(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.fillRect(80, 30, 340, 340);

    g2d.setColor(Color.WHITE);
    g2d.setFont(sRecursos.getFontTitulo());
    g2d.drawString("Soy un Panel", 200, 200);
    
    g2d.setPaint(Color.ORANGE);
    RoundRectangle2D rectanguloRedondeado = new RoundRectangle2D.Double();
    rectanguloRedondeado.setRoundRect(80, 30, 340, 340, 100, 100);

    Area area = new Area(rectanguloRedondeado);

    Rectangle rectangulo = new Rectangle(80, 30, 340, 340);
    Area areaFueraBorde = new Area(rectangulo);
    areaFueraBorde.subtract(area);
    g2d.setClip(areaFueraBorde);

    g2d.draw(area);
}
```

Con lo anterior estamos indicando que lo que se pinte de ahi en adelante solo se vera afectado en el area **areaFueraBorde**, vamos a pintar un recuadro de color blanco que ocupe todo el tamaño del panel:

```javascript
public void pintarAreas(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.fillRect(80, 30, 340, 340);

    g2d.setColor(Color.WHITE);
    g2d.setFont(sRecursos.getFontTitulo());
    g2d.drawString("Soy un Panel", 200, 200);
    
    g2d.setPaint(Color.ORANGE);
    RoundRectangle2D rectanguloRedondeado = new RoundRectangle2D.Double();
    rectanguloRedondeado.setRoundRect(80, 30, 340, 340, 100, 100);

    Area area = new Area(rectanguloRedondeado);

    Rectangle rectangulo = new Rectangle(80, 30, 340, 340);
    Area areaFueraBorde = new Area(rectangulo);
    areaFueraBorde.subtract(area);
    g2d.setClip(areaFueraBorde);

    g2d.setColor(Color.WHITE);
    g2d.fillRect(80, 30, 340, 340);

    g2d.draw(area);
}
```

Como mencionamos al proporcionar de contexto al canvas este solo se vera reflejado en el area proporcionada (**areaFueraBorde**).

<div align='center'>
    <img  src='https://i.imgur.com/Z63SAEF.png'>
    <p>Pintando recuadro blanco afectando solo area de contexto.</p>
</div>

Ya le proporcionamos los bordes redondeados a nuestro panel ficticio de forma satisfactoria, sin embargo los bordes se ven algo pixeleados y es que el efecto de **setClip** hace que la sustracción se vea borrosa. Para arreglar esto y para poder dibujar mas cosas sobre el canvas sin problemas debemos dejar el contexto nulo de nuevo:
```javascript
public void pintarAreas(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.fillRect(80, 30, 340, 340);

    g2d.setColor(Color.WHITE);
    g2d.setFont(sRecursos.getFontTitulo());
    g2d.drawString("Soy un Panel", 200, 200);
    
    g2d.setPaint(Color.ORANGE);
    RoundRectangle2D rectanguloRedondeado = new RoundRectangle2D.Double();
    rectanguloRedondeado.setRoundRect(80, 30, 340, 340, 100, 100);

    Area area = new Area(rectanguloRedondeado);

    Rectangle rectangulo = new Rectangle(80, 30, 340, 340);
    Area areaFueraBorde = new Area(rectangulo);
    areaFueraBorde.subtract(area);
    g2d.setClip(areaFueraBorde);

    g2d.setColor(Color.WHITE);
    g2d.fillRect(80, 30, 340, 340);

    g2d.setClip(null);
    g2d.draw(area);
}
```

<div align='center'>
    <img  src='https://i.imgur.com/Xdls9LS.png'>
    <p>Bordes implementados</p>
</div>

Lo que acabamos de ver es la base para crear bordes redondeados reales en objetos Gráficos **Swing** como botones, labels, paneles etc. 
No solo podríamos realizarlo con recuadros bordeados podemos hacerlo con óvalos, polígonos etc. A continuación podríamos cambiar el objeto del recuadro redondeado por un circulo por ejemplo:

```javascript
public void pintarAreas(Graphics2D g2d){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.fillRect(80, 30, 340, 340);

    g2d.setColor(Color.WHITE);
    g2d.setFont(sRecursos.getFontTitulo());
    g2d.drawString("Soy un Panel", 200, 200);
    
    g2d.setPaint(Color.ORANGE);
    // RoundRectangle2D rectanguloRedondeado = new RoundRectangle2D.Double();
    // rectanguloRedondeado.setRoundRect(80, 30, 340, 340, 100, 100);
    
    Ellipse2D circulo = new Ellipse2D.Double();
    circulo.setFrameFromCenter((80 + 340 / 2), (30 + 340 / 2), 340 + 80, 340 + 30);
        
    Area area = new Area(circulo);
    
    Rectangle rectangulo = new Rectangle(80, 30, 340, 340);
    Area areaFueraBorde = new Area(rectangulo);
    areaFueraBorde.subtract(area);
    g2d.setClip(areaFueraBorde);

    g2d.setColor(Color.WHITE);
    g2d.fillRect(80, 30, 340, 340);

    g2d.setClip(null);
    g2d.draw(area);
}
```

<div align='center'>
    <img  src='https://i.imgur.com/RjoNOmy.png'>
    <p>Borde implementando un circulo.</p>
</div>

# Uso de EventosMouseListener sobre Canvas

Ya hemos usado antes los eventos del mouse para proporcionar interactividad a distintas partes de nuestro proyecto, a este punto ya dominamos las funcionalidades de los eventos del Mouse, en este caso podríamos aprovechar el manejo de eventos para dibujar figuras de manera interactiva con el usuario. Lo que queremos realizar específicamente es que una vez el usuario oprima el botón del mouse dentro del canvas, lo mantenga oprimido y arrastre el mouse, este pueda dibujar una figura en tiempo real. En este caso vamos a dibujar un rectangulo.


Lo primero que vamos a realizar es dejar vació el canvas y para eso vamos a comentar la llamada del método **pintarAreas** dentro del método **paint**:
```javascript
@Override
public void paint(Graphics g){
    g2d = (Graphics2D) g.create();
    g2d.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
    g2d.setRenderingHint(RenderingHints.KEY_STROKE_CONTROL, RenderingHints.VALUE_STROKE_NORMALIZE);
    // this.pintarStrings(g2d);
    // this.pintarRectangulos(g2d);
    // this.pintarLineas(g2d);
    // this.pintarArcos(g2d);
    // this.pintarOvalos(g2d);
    // this.pintarPoligonos(g2d);
    // this.pintarImagenes(g2d);
    // this.pintarAreas(g2d);
}
```

Ahora vamos a concentrarnos en la clase **LienzoComponent**, vamos a implementar las interfaces **MouseListener y MouseMotionListener**. 

```javascript
public class LienzoComponent implements MouseListener, MouseMotionListener{

}
```

También vamos a implementar sus respectivos métodos: 
```javascript
@Override
public void mouseClicked(MouseEvent e) {}

@Override
public void mousePressed(MouseEvent e) {}

@Override
public void mouseReleased(MouseEvent e) {}

@Override
public void mouseEntered(MouseEvent e) {}

@Override
public void mouseExited(MouseEvent e) {}

@Override
public void mouseDragged(MouseEvent e) {}

@Override
public void mouseMoved(MouseEvent e) {}
```

Vamos a posicionarnos de nuevo en la clase **LienzoTemplate** y lo primero que vamos a realizar es añadirle la escucha de estos eventos del mouse al canvas, como se menciono antes, una clase puede escuchar diferentes eventos del mismo tipo asi que aunque el Canvas ya esta escuchando eventos del Mouse de su panel Contenedor, aun así no habrá ningún problema:

```javascript
// Dentro del Constructor
this.addMouseListener(this.lienzoComponent);
this.addMouseMotionListener(this.lienzoComponent);
```
Vamos a crear un método al cual llamaremos **pintarRectanguloTiempoReal**, este será el encargado de dibujar la figura en tiempo real a traves de los eventos del Mouse:
```javascript
public void pintarRectanguloTiempoReal(){
}
```

Nos pasamos a nuestra clase **LienzoComponent** y vamos a necesitar capturar la posición inicial una vez se oprima el botón del mouse, este enfoque se asemeja al usado para mover la ventana a traves de la barra del título, para eso creamos dos variables que van a capturar esta posición inicial.
* **Declaración:**
```javascript
private int posicionInicialX, posicionInicialY;
```
* **Captura de posición inicial:**
Para capturar esto nos vamos a basar del principio de arrastre que usamos con la ventana y vamos a usar en este caso el **MousePressed**:
```javascript
@Override
public void mousePressed(MouseEvent e) {
    this.posicionInicialX = e.getX();
    this.posicionInicialY = e.getY();
}
```

En este caso como el contexto es el canvas nada mas solo usaremos las posiciónes con respecto a este (**getX, getY**).

Ahora como realizaremos una acción mientras el usuario mantiene oprimido el botón del mouse y se esta arrastrado vamos a configurar el evento **mouseDragged**. Allí vamos a
llamar al método que se encargará de pintar la figura:
```javascript
@Override
public void mouseDragged(MouseEvent e) {
    this.lienzoTemplate.pintarRectanguloTiempoReal();
}
```
Para poder dibujar la figura vamos a necesitar enviarle 3 argumentos al método:
* **Posición inicial en X**.
* **Posición inicial en Y**.
* **El objeto del evento para capturar la posición de arrastre**.

```javascript
@Override
public void mouseDragged(MouseEvent e) {
    this.lienzoTemplate.pintarRectanguloTiempoReal(posicionInicialX, posicionInicialY, e);
}
```

Así mismo se debe recibir estos elementos como parámetros dentro del método en la clase **LienzoTemplate**:
```javascript
public void pintarRectanguloTiempoReal(int x, int y, MouseEvent e){

}
```
Vamos a configurar el color del canvas para que el rectángulo pueda ser visualizado:
```javascript
public void pintarRectanguloTiempoReal(int x, int y, MouseEvent e){
    g2d.setColor(sRecursos.getColorAzul());
}
```
Dentro del método debemos pintar el rectángulo, para esto llamaremos al método **drawRect**:

```javascript
public void pintarRectanguloTiempoReal(int x, int y, MouseEvent e){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.drawRect();
}
```

La posición inicial del rectángulo la estamos recibiendo por parámetro, así que lo vamos a colocar:

```javascript
public void pintarRectanguloTiempoReal(int x, int y, MouseEvent e){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.drawRect(x, y);
}
```

Ahora debemos configurar el ancho y alto del rectángulo, para esto basta con capturar la posición del mouse a traves del evento que se activa a medida que el mouse se esta arrastrando. Le debemos restar ademas la posicion inicial, en ambos casos:
```javascript
public void pintarRectanguloTiempoReal(int x, int y, MouseEvent e){
    g2d.setColor(sRecursos.getColorAzul());
    g2d.drawRect(x, y, e.getX() - x , e.getY() - y);
}
```

Si probamos nuestros eventos veremos algo como esto:
<div align='center'>
    <img  src='./gifs/Canvas1.gif'>
    <p>Intento de dibujo dentro de canvas</p>
</div>

Al parecer nuestro intento de pintar tiene algo extraño, y esto es debido a que a medida que se mueve el mouse mientras se tiene oprimido el botón izquierdo, se esta pintando un rectángulo nuevo, es decir que por cada pixel que mueve el mouse se esta pintando un nuevo rectángulo, debemos hallar la forma en que solo se vea el rectángulo actual y se borren los rectángulos anteriores, hay muchas formas de hacer esto, vamos hacer una forma rápida de hacerlo, no es la mejor forma pero por practicidad y facilidad se realizará así.

Vamos a hacer que por cada vez que se active el evento, se pinte primero un rectángulo blanco que ocupe todo el canvas, esto hará un efecto de borrar cualquier rectangulo anterior que se haya dibujado a parte del actual:
```javascript
public void pintarRectanguloTiempoReal(int x, int y, MouseEvent e){
    g2d.setColor(Color.WHITE);
    g2d.fillRect(0, 0, this.getWidth(), this.getHeight());
    g2d.setColor(sRecursos.getColorAzul());
    g2d.drawRect(x, y, e.getX() - x , e.getY() - y);
}
```

Vamos a ver que ocurre una vez probamos nuestro método:
<div align='center'>
    <img  src='./gifs/Canvas2.gif'>
    <p>Segundo intento de pintar rectángulos.</p>
</div>

Efectivamente podemos pintar rectángulos en tiempo real y se esta viendo unicamente el rectángulo actual acorde al movimiento del mouse. Sin embargo hay 2 problemas:
* Si se ha terminado de pintar un rectángulo, es decir el usuario ha soltado el mouse y luego quiere pintar otro el anterior será borrado y esto es debido al enfoque de pintar un rectángulo blanco en el canvas, este problema se dejará asi por esta sesión y mas adelante se podrá discutir.
* El programa unicamente pinta el rectángulo hacia una dirección, si se intenta pintar un rectángulo  hacia arriba o a la izquierda con respecto al punto inicial este no se pintara, este problema si lo vamos a resolver.

Para esto debemos realizar una condición para los 4 casos:
* En caso de pintar un rectángulo con dirección **inferior derecha** con respecto al punto inicial.
* En caso de pintar un rectángulo con dirección **superior derecha** con respecto al punto inicial.
* En caso de pintar un rectángulo con dirección **inferior izquierda** con respecto al punto inicial.
* En caso de pintar un rectángulo con dirección **superior izquierda** con respecto al punto inicial.

```javascript
public void pintarRectanguloTiempoReal(int x, int y, MouseEvent e){
    g2d.setColor(Color.WHITE);
    g2d.fillRect(0, 0, this.getWidth(), this.getHeight());
    g2d.setColor(sRecursos.getColorAzul());
    // Dirección Inferior Izquierda
    if(e.getX() - x < 0 && e.getY() - y > 0)

    // Dirección Superior Izquierda
    if(e.getX() - x < 0 && e.getY() - y < 0)

    // Dirección Superior Derecha
    if(e.getX() - x > 0 && e.getY() - y < 0)

    // Dirección Inferior Derecha
    if(e.getX() - x > 0 && e.getY() - y > 0)
        g2d.drawRect(x, y, e.getX() - x , e.getY() - y);
}
```

Ya se crearon los 4 casos y solo el último es el que ya está configurado (el que esta por defecto).

Para el caso en que se dibuje el recuadro con dirección **Inferior Izquierda**:
* El punto inicial en X ahora será la coordenada que ira moviendo el usuario
* El punto inicial en Y permanece estático.
* El ancho del rectangulo sera la posición estática en X menos la posición que va actualizando con cada arrastre (para que quede positivo).
* El alto sera igual a la posición que se va actualizando con el arrastre menos la posición estática en Y.
```javascript
public void pintarRectanguloTiempoReal(int x, int y, MouseEvent e){
    g2d.setColor(Color.WHITE);
    g2d.fillRect(0, 0, this.getWidth(), this.getHeight());
    g2d.setColor(sRecursos.getColorAzul());
    // Dirección Inferior Izquierda
    if(e.getX() - x < 0 && e.getY() - y > 0)
        g2d.drawRect(e.getX(), y, x - e.getX(), e.getY() - y);
    // Dirección Superior Izquierda
    if(e.getX() - x < 0 && e.getY() - y < 0)

    // Dirección Superior Derecha
    if(e.getX() - x > 0 && e.getY() - y < 0)

    // Dirección Inferior Derecha
    if(e.getX() - x > 0 && e.getY() - y > 0)
        g2d.drawRect(x, y, e.getX() - x , e.getY() - y);
}
```

Para el caso en que se dibuje el recuadro con dirección **Superior Izquierda**:
* El punto inicial en X ahora será la coordenada que ira moviendo el usuario.
* El punto inicial en Y ahora será la coordenada que ira moviendo el usuario.
* El ancho del rectangulo sera la posición estática en X menos la posición que va actualizando con cada arrastre (para que quede positivo).
* El alto del rectangulo sera la posición estática en Y menos la posición que va actualizando con cada arrastre (para que quede positivo).

```javascript
public void pintarRectanguloTiempoReal(int x, int y, MouseEvent e){
    g2d.setColor(Color.WHITE);
    g2d.fillRect(0, 0, this.getWidth(), this.getHeight());
    g2d.setColor(sRecursos.getColorAzul());
    // Dirección Inferior Izquierda
    if(e.getX() - x < 0 && e.getY() - y > 0)
        g2d.drawRect(e.getX(), y, x - e.getX(), e.getY() - y);
    // Dirección Superior Izquierda
    if(e.getX() - x < 0 && e.getY() - y < 0)
        g2d.drawRect(e.getX(), e.getY(), x - e.getX(), y - e.getY());
    // Dirección Superior Derecha
    if(e.getX() - x > 0 && e.getY() - y < 0)

    // Dirección Inferior Derecha
    if(e.getX() - x > 0 && e.getY() - y > 0)
        g2d.drawRect(x, y, e.getX() - x , e.getY() - y);
}
```

Para el caso en que se dibuje el recuadro con dirección **Superior Derecha**:
* El punto inicial en X será la coordenada estática.
* El punto inicial en Y ahora será la coordenada que ira moviendo el usuario.
* El ancho sera igual a la posición que se va actualizando con el arrastre menos la posición estática en X.
* El alto del rectangulo sera la posición estática en Y menos la posición que va actualizando con cada arrastre (para que quede positivo).

```javascript
public void pintarRectanguloTiempoReal(int x, int y, MouseEvent e){
    g2d.setColor(Color.WHITE);
    g2d.fillRect(0, 0, this.getWidth(), this.getHeight());
    g2d.setColor(sRecursos.getColorAzul());
    // Dirección Inferior Izquierda
    if(e.getX() - x < 0 && e.getY() - y > 0)
        g2d.drawRect(e.getX(), y, x - e.getX(), e.getY() - y);
    // Dirección Superior Izquierda
    if(e.getX() - x < 0 && e.getY() - y < 0)
        g2d.drawRect(e.getX(), e.getY(), x - e.getX(), y - e.getY());
    // Dirección Superior Derecha
    if(e.getX() - x > 0 && e.getY() - y < 0)
        g2d.drawRect(x, e.getY(), e.getX() - x, y - e.getY()); 
    // Dirección Inferior Derecha
    if(e.getX() - x > 0 && e.getY() - y > 0)
        g2d.drawRect(x, y, e.getX() - x , e.getY() - y);
}
```

Una vez ejecutamos nuestra aplicación vemos algo como esto:

<div align='center'>
    <img  src='./gifs/Canvas3.gif'>
    <p>Pintando rectángulos en tiempo real sobre el canvas.</p>
</div>

# Resultado 

Si has llegado hasta aquí **!Felicidades!** has aprendido acerca del uso del **Canvas** y entre varias cosas como pintar figuras básicas como Strings, Lineas, Rectángulos, Arcos, Ovalos, Poligonos e Imagenes. Aprendiste sobre el uso de las Areas y como realizar operaciones entre ellas. Finalmente aprendiste a dibujar en tiempo real figuras proporcionando una gran interactividad a traves de los eventos del Mouse. En la siguiente clase no vamos a enfocar a revisar el Servicio Gráfico **GraficosAvanzadosServices** y como el Canvas tiene un papel fundamental en la mayoría del código que se encuentra allí.

# Actividad 

Estudiar el uso de canvas y como pintar sus figuras básicas.