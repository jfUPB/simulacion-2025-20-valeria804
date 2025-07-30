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


- 
