# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 1

la aleatoriedad retira el control total del artista acercandolo mas a la aleatoriedad de mundo natural; el arte generativo permite crear resultados unicos e impredecibles, de esta forma siempre se renueva, genera sorpresa y variacion, haciendo que cada experiencia sea unica y personal

### Actividad 2

- su aleatoriedad permite que cada visualizacion sea unica e impredesible, pero mas que solo lograr una experiencia unica y personal tambien permite conectar con la narrativa del mensaje que busca dar, con la cancion concreta "la semilla" representa el sentimiento de lo natural y de que nosotros como humanos no lo controlamos. esto tambien permite que el artista que lo esta manipulando cree junto a la maquina representando como la musica se manifiesta en ella

- en mi perfil profesional que busco enfocarme en la animacion y creacion de mundos, por lo tanto esto puede facilitar el trabajo haciendo que se creen varios elementos en la escena de forma aleatoria, esto para que se vea mas natural y no tan rigido, de esta forma se puede facilitar el trabajo y generar un resultado mas estetico, ejemplos podrian ser:

una animacion en la se crean particulas que simulan fuego, humo o polvo, y para cada particula se asigna un valor aleatorio: valores como velocidad, direccion y duracion 

### Actividad 3

```js

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
  }
}
```

let walker es una variable global para poder accederla en todas las funciones que se llame 

walker.step() es un metodo que indica en que espacio del lienzo se hace el paso siguiente, indica en donde se va a dibujar 

el metodo strock() en la funcion draw() 

#### modificacion 

quiero hacer que la particula tenga un movimiento diagonal tambien, en el original solo esta moviendose en el eje "x" y "y". para esto primero deberia modificar este 4 ¨const choice = floor(random(4));¨ ya que asi se puede ejegir mas opciones de movimiento,

puedo cambiar para que se mueva en 8, ya que antes solo eran 4 direcciones ahora seran 8 direcciones diagonales

replicando un poco el codigo original, para hacer un moviento diagonal deberia agregar esa nueva opcion 4, que ocurre si no se ejecuta ninguna de las otras. el movimiento diagonal deberia ser posible si se indican dos direccion, por ejemplo si quiero que vaya arriba a la derecha deberia poner la posicion en x y y en su lado positivo this.x++; this.y++;. y del mismo modo con las demas nuevas direcciones

el codigo quedaria asi 

```js
let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {  
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(8));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else if (choice == 3) {
      this.y--;
    } else if (choice == 4) {
      this.x++;
      this.y++;
    } else if (choice == 5) {
      this.x--;
      this.y--;
    } else if (choice == 6) {
      this.x++;
      this.y--;
    } else if (choice == 7) {
      this.x--;
      this.y++;
    }
  }
}
```
al ejecutarlo funciona como se esperaba porque ejecute esa misma logica con las demas direcciones diagonales 

### Actividad 4

en la distribucion uniforme todos los valores tienen la misma probabilidad de salir, se usa el random() 

en la distribucion no uniforme algunos valores son mas probables de salir que otros, presentan mayor frecuencia, estos valores son cercanos a una media. este es muy util para crear animaciones mas naturales y fluidas ya que ocurren pequeños movimientos en su mayoria. se usa el randomGaussian()

usando el randomGaussian() seria "let stepX = randomGaussian() * 1 + 1;" este tiene una media en 0, al multiplicarlo por 1 se mantiene la escala de la desviación estándar que es entre -1 y 1. al sumarlo por uno desplaza los valores a la derecha, haciendo que no se centren en 0 si no en uno. por ejemplo si saliera un numero a la izquierda, o sea negativo, al sumalo mas uno se vuelve positivo, ya no va a la izquierda todavia se mueve a la derecha, pero un poco menos.

```js
let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    // Movimiento en X favoreciendo la derecha
    let stepX = randomGaussian() * 1 + 1; // media desplazada a la derecha
    let stepY = randomGaussian() * 1;     // movimiento vertical sin sesgo

    this.x += stepX;
    this.y += stepY;
  }
}
```

### Actividad 5

```js
function setup() {
  createCanvas(640, 360);
  background(255);
  stroke(0);
}

function draw() {
  let media = width / 2;
  let desviacion = 60;

  let x = randomGaussian() * desviacion + media;
  let y = random(height);

  point(x, y);
}
```

en la funcion draw se crea un bucle en el que cada vez se dibuja un punto nuevo. la media que es el centro de la distribucion se define en el centro del canvas: 640 / 2 = 320. en la desviacion estandar se define cuanto se pueden desviar los valores generados, los valores grandes se encuentran mas dispersos.

randomGaussian() genera un numero con media 0 y desviaciun 1. se multiplica por desviacion para ajustar que tan separados estan los puntos y se suma la media para centrar la distribucion en el medio del canvas. todo eso para x

let y = random(height); elige una altura (y) completamente aleatoria entre 0 y la altura del canvas

https://editor.p5js.org/valeria804/sketches/g3DspbuOU

<img width="597" height="363" alt="Captura de pantalla 2025-07-23 074733" src="https://github.com/user-attachments/assets/05bee5ce-6e28-4d77-a250-590961f6cf15" />


### Actividad seis

```js
let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {  
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    // Paso direccional aleatorio
    let angle = random(TWO_PI);

    // Magnitud de paso: Levy flight usando distribución normal con una probabilidad baja de gran salto
    let stepSize = this.levy();

    // Convertir ángulo y magnitud en desplazamiento x, y
    let dx = cos(angle) * stepSize;
    let dy = sin(angle) * stepSize;

    this.x += dx;
    this.y += dy;

  }

  // Función para generar un salto estilo Lévy
  levy() {
    let r = random(1);
    if (r < 0.01) {
      // 1% de probabilidad de un salto grande
      return random(50, 100);
    } else {
      // 99% de probabilidad de pasos pequeños (gaussiano)
      return abs(randomGaussian() * 2);
    }
  }
}
```
https://editor.p5js.org/valeria804/sketches/08ryIRmGO

<img width="591" height="237" alt="Captura de pantalla 2025-07-23 084514" src="https://github.com/user-attachments/assets/3f6b9d1f-5b77-42d4-a25e-137228a81761" />  
