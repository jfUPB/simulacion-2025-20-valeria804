# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 1

se crean las variables globales de posicion y velocidad, la variable posicion toma la direccion del objeto vector, almacena los datos para que se pueda usar 

```js
let position;
let velocity;

function setup() {
  createCanvas(640, 240);
  
  position = createVector(100, 100);
  velocity = createVector(2.5, 2);
}

function draw() {
  background(255);
  
  position.add(velocity);

  if (position.x > width || position.x < 0) {
    velocity.x = velocity.x * -1;
  }
  if (position.y > height || position.y < 0) {
    velocity.y = velocity.y * -1;
  }

  stroke(0);
  fill(127);
  strokeWeight(2);
  circle(position.x, position.y, 48);
}
```

- para sumar un vector se suman las variables en x y y respectivamente de cada uno, y esa suma forma un nuevo vector.
  en p5.js se usa -----> position.add(velocity); De por si, al usar vectores, no los podemos sumar como sumaríamos dos números, entonces nos toca usar unso métodos diferentes, en este lo que se hace es que se suma los valores de la velocidad y de posicion y cuando se hace, se reemplaza el valor de posicion por el nuevo que se creo; se modifica position

- debido a que la biblioteca de operaciones que se usa no permite eso, ya que se suman solo direcciones. Por esa razón usamos el .add, el cual suma los COMPONENTES de un vector con los de otro componente por componente.

### Actividad 2

cambiamos los this.x this.y por this.position, y ya ahi está implicito el usar position.x y position.y, esto hace que el código sea mucho más compacto y bonito. a demas se crea el vector y se guardan los valores en el constructor position 

```js
let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw(){
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
  this.position = createVector(width / 2,height / 2);
    //this.x = width / 2;
    //this.y = height / 2;
  }

show() {
    stroke(0);
    point(this.position.x, this.position.y);
    //point(this.position);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.position.x++;
    } else if (choice == 1) {
      this.position.x--;
    } else if (choice == 2) {
      this.position.y++;
    } else {
      this.position.y--;
    }
  }
}
```
### Actividad 3

- espero que los datos del vector se guarden en la variable posicion y que dicha direccion se guarde en playingVector, tambien se escribira los datos de posicion en la consola de nuevo, pero con la funcion playingVector se puede modificar los valores, lo cual hara que al final se vea en la consola los valores originales y despues los modificados

- <img width="219" height="33" alt="Captura de pantalla 2025-07-30 122104" src="https://github.com/user-attachments/assets/1720114a-b37e-481f-b433-8867000f48d9" />

-
```js
let position;
let myvar;

function setup() {
    createCanvas(400, 400);
    position = createVector(6,9); //se crea el vector
    console.log(position.toString()); //se imprime el dato en la consola
    playingVector(position); // se le pasa a la play el contenido de la valriable, y como la direccion del objeto esta en la variable, se le pasa la direccion del objeto a playingVector
    console.log(position.toString());
    noLoop();
  
    myvar=25;
    console.log(myvar);
    playWithMyVar(myvar);
    console.log(myvar);
}

function playWithMyVar(v){
  v = 47; //seguira siendo igual porque solo se modifico v, pero myvar sigue siendo igual
}

function playingVector(v){ // la direccion de manda a v, y de este modo se puede manipular el objeto
    v.x = 20; //a traves de esa direccion se modifica los valores en x y y, gracias a v.x
    v.y = 30;
}

function draw() {
    background(220);
    console.log("Only once");
}
```

- Recuerda los conceptos de paso por valor y paso por referencia en programación. Muestra ejemplos de este concepto en javascript.

  paso por valor cuando se pasa un valor primitivo (como un número, booleano o string) a una función, y se copia temporalmente el valor y la funcion trabaja con ella.

  ```js
  function setup() {
  noCanvas();
  
  let a = 5;
  cambiar(a);
  
  print("Valor final de a: " + a); // Imprime: 5
  }

  function cambiar(x) {
  x = x + 1;
  print("Dentro de la función: " + x); // Imprime: 6
  }

  ```

  paso por referencia es cuando pasas un objeto, como un array o un p5.Vector, estás pasando una referencia a la ubicación en memoria de ese objeto, la funcion puede modificar directamente el contenido de ese objeto, se modifican valores en x y y

```js
function setup() {
  noCanvas();
  
  let v = createVector(10, 20); // un vector (objeto)

  mover(v); // le pasamos la referencia del vector
  
  print("Después de mover: " + v.x); // Imprime: 15
}

function mover(vector) {
  vector.x += 5;
  print("Dentro de la función: " + vector.x); // Imprime: 15
}

```

- en el codigo se usa el paso por referencia ya que a v se le manda la direccion del vector y de este modo puede manipularlo

### Actividad 4

- ¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?

  El método mag() calcula la magnitud del vector, es decir, su longitud desde el origen (0, 0) hasta su posicion en el espacio. medir la distancia del vector al origen usando el Teorema de Pitágoras.

  mag() calcula la magnitud real del vector (con raiz cuadrada). magSq() devuelve la magnitud al cuadrado, esta es mas eficiente, ya que al no usar raiz cuadrada consume menos recursos, aunque no es usada para saber valores exactos 
  
- ¿Para qué sirve el método normalize()?

   normalize() transforma el vector para que tenga una longitud de 1, es decir, lo convierte en un vector unitario que mantiene su dirección, pero pierde su escala. se usa cuando se necesita solo la direccion 

- Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?

  dot() sirve para saber si dos vectores van en la misma dirección o en direcciones opuestas.. Si el resultado es alto, apuntan en la       misma dirección.

-  El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?

  la version estatica se llama desde la clase p5.Vector y le pasas los dos vectores como parámetros 	p5.Vector.dot(v1, v2). en la version de instancia tienes un vector y llamas el método desde ese vector, 	v1.dot(v2).

-  Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.

   El producto cruz entre dos vectores en 3D genera un nuevo vector perpendicular a ambos. su orientacion Sigue la regla de la mano derecha. Si los dedos apuntan del primer al segundo vector, el pulgar indica la dirección del resultado. y su magnitud representa el área entre los dos vectores, siendo igual al área del paralelogramo que forman los dos vectores originales.

-  ¿Para que te puede servir el método dist()?

   dist() mide la distancia entre dos vectores en el espacio (como puntos). Las coordenadas de un punto se pueden representar mediante los componentes de un vector que se extiende desde el origen hasta el punto.

-  ¿Para qué sirven los métodos normalize() y limit()?

  normalize() -> Convierte el vector en uno unitario (longitud 1). limit(max) -> Restringe la longitud máxima del vector a un valor específico, evita que algo se mueva demasiado rapido

### Actividad 5

```js
let l = 0;
let arriba;

function setup() {
    createCanvas(400, 400);
    l = 0.5;
    arriba = 1;
}

function draw() {
    background(200);
    
    let v0 = createVector(50, 50);
    let v1 = createVector(200, 0);
    let v2 = createVector(0, 200);
    let v3 = p5.Vector.lerp(v1, v2, l);
    let origen = p5.Vector.add(v1, v0);
    let destino = p5.Vector.add(v0, v2);
    let dir = p5.Vector.sub(destino, origen);
    
    // Interpolación de color
    let c1 = color(255, 0, 0);    // rojo
    let c2 = color(0, 0, 255);    // azul
    let interpolado = lerpColor(c1, c2, l);  // color interpolado

    drawArrow(v0, v1, 'red');
    drawArrow(v0, v2, 'blue');
    drawArrow(v0, v3, interpolado);  // color cambia dinámicamente
    drawArrow(origen, dir, 'green');
  
    if(l <= 0) arriba = true;
  
    if(l >= 1) arriba = false;
    
    if(arriba){
        l += 0.01;
    } else {
        l -= 0.01;
    }
}

function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(3);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}
```
se usa .lerp() para interpolar entre los vectores v1 y v2 según el valor l, que va de 0 a 1.

usando esto 

```js
if (l <= 0) arriba = true;
if (l >= 1) arriba = false;
```
hace que l sube de 0 a 1 y luego baja de 1 a 0 continuamente. y la velocidad constante se determina con pasos en 0.01

para la interpolacion de color use lerpColor() para crear un valor intermedio entre rojo y azul, con la variable interpolado se guarda esa funcion y se le asigna a drawArrow, y de este modo se convierta en su nuevo color

para el vector verde, establecio origen como la punta del vector v1, calculada como v0 + v1. destino como la punta del vector v2, calculada como v0 + v2. usanod las propiedades de los vectores dir = destino - origen da un nuevo vector desde la punta de v1 hasta la punta de v2. con drawArrow(origen, dir, 'green') se dibuja ese vector como una flecha desde la punta roja hasta la azul.

- ¿Cómo funciona lerp() y lerpColor().

  El método .lerp() (de Linear Interpolation) crea un vector intermedio entre dos vectores, basandose en un parametro amt (entre 0 y 1), que indica cuanto peso se da a cada vector. lerpColor() se usa para interpolar entre dos colores, de forma parecida a lerp(), permite crear un color intermedio entre dos colores dados, usando un valor amt entre 0 y 1 dependiendo en que momento del tiempo esta.

- ¿Cómo se dibuja una flecha usando drawArrow()?
  
  drawArrow() dibuja una flecha a partir de un vector base y una dirección, compuesto por (inicio, final, color). para crear la flecha Traslada el origen del sistema de coordenadas a la base del vector, despues dibuja la linea de la flecha desde (0,0) a (vec.x, vec.y) y rota el sistema, despues dibuja la punta de la flecha como un triangulo y la punta la pone al final del vector

### Actividad 6

- Representa una forma sencilla pero fundamental de simular el movimiento de objetos usando vectores en una pantalla, se tiene en cuenta que cada objeto riene una posicion, una velocidad y una acelaracion (y todas estan representadas por vectores)

  (position) representa dónde está el objeto en el espacio. (velocity) representa cómo se mueve: en qué dirección y con qué rapidez. y el movimiento es la suma de esos dos vectores; en cada instante de tiempo el objeto se mueve al sumar su velocidad a su posición. en el marco basico no hay aceleracion ya que hay velocidad constante.

  Geometricamente, puede entenderse como una construcción vectorial en el plano 2D. La posición es un punto en el espacio (x,y). La velocidad es un vector que apunta en una dirección y tiene una magnitud. Al sumar el vector de velocidad a la posición, se obtiene una nueva posición.

- se crean metodos para actualizar, mostrar y verificar los bordes, usando this.position: vector de posición. this.velocity: vector de velocidad.

  "this.position = createVector(random(width), random(height));" inicializa el estado de posicion del objeto
  "this.velocity = createVector(random(-2, 2), random(-2, 2));" define la direccion y magnitud del movimiento
  "this.position.add(this.velocity);" actualiza la posicion sumando el vector velocidad, siendo la base principal de motion 101

### Actividad 7

velocidad+=aceleracion,  posicion+=velocidad

- aceleracion constante: Siempre se le añade el mismo pequeño cambio a la velocidad en cada frame, lo cual hace que la velocidad aumente gradualmente, dicha velocidad se suma a la posicion y geenra el movimiento final. al inicio no hay velocidad inicial, "this.velocity.add(this.acceleration);" con eso se aplica la aceleración al vector velocidad. Suma la velocidad actual a la posición, haciendo que el objeto se mueva.

- aceleracion aleatoria: el objeto recibe una nueva aceleración aleatoria en cada frame, lo que provoca que su movimiento sea erratico e impredecible. "p5.Vector.random2D()" genera un vector unitario en una dirección aleatoria. "mult(random(2))" escala esa dirección con una magnitud aleatoria entre 0 y 2. esto hace que cada frame cambia completamente la dirección y fuerza de la aceleracion. despues se aplica los pasos basicos de este modelo: La aceleración modifica la velocidad. Se limita la velocidad para evitar que se dispare. La posición se actualiza con base en la velocidad. en este caso la aceleracion no es calculada sino randomizada.

- aceleracion hacia el mouse: en cada frame, el objeto calcula un vector de aceleracion que apunta desde su posición actual hacia el cursor del mouse. el vector se normaliza (para solo tener direccion), luego se escala a una magnitud específica, y se aplica como aceleración. El vector se convierte en una aceleración constante hacia el mouse. y se siguen los pasos normales del modelo

