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

aqui hay una explicacion de las lineas de codigo, a demas de una nueva variable para demostrar cuando el valor del vector no podria ser modificado

- 
