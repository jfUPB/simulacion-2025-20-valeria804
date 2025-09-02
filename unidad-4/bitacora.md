# Evidencias de la unidad 4

## set-seek 

### Actividad 2

- ¿Qué está pasando en esta simulación? ¿Cuál es la interacción?
 
  el codigo dibuja una barra y un circulo en cada extremo, la barra rota alrededor de su centro cada vez que se presiona una tecla
  
- Nota que en cada frame se está trasladando el origen del sistema de coordenadas al centro de la pantalla. ¿Por qué crees que se hace esto?

  se coloca en el centro de la pantalla para que la rotacion ocurra al rededor del centro de la figura y no de la esquina 
  
- Cuál es la relación entre el sistema de coordenadas y la función rotate().

  rotate gira todo el sistema de coordenadas alrededor del origen actual, despues de llamar translate, el origen esta en el centro. line y circle se dibujan dentro del sistema rotado y no en el sistema original

- Observa que al dibujar los elementos gráficos parece que se está dibujando en la posición (0, 0) del sistema de coordenadas. ¿Por qué crees que se hace esto? y ¿Por qué aunque en cada frame se hace lo mismo, los elementos gráficos rotan?

  despues de translate y rotate ya no estamos en el sistema original de la ventana. cuando se dibuja la linea se dibuja centrada en el origen actual que esta en el centro de la pantalla y los circulos tambien son relativos a ese nuevo sistema de coordenadas rotado. line y circle es el mismo pero se ejecutan en un sistemas de coordenadas rotado. al aumentar angle el sistema gira antes de volver a dibujar

- Identifica el marco motion 101. ¿Qué es lo que se está haciendo en este marco?

  el objeto mover tiene position, velocity y acceleration, en este se suma la velocidad con la posicion para tener velocidad, y se suma velocidad con aceleracion para tener velocidad. acelerar es un vector que apunta hacia el mouse, la velocidad se actualiza sumando la aceleracion, o seas que cambia su velocidad con la direccion y cambia su posicion 
  
- ¿Qué hace la función heading()?

  heading() devuelve el ángulo de un vector en radianes, medido desde el eje X positivo. obtiene el ángulo de la velocidad, es decir, hacia dónde se está moviendo el objeto.

- ¿Qué hace la función push() y pop()? Realiza algunos experimentos para entender su funcionamiento.

  push() guarda el estado actual del sistema de coordenadas. pop() restaura ese estado guardado. como cada transformacion afecta el sistema de coordenadas global. cualquier translate() y rotate() que ocurra entre push() y pop() afecta solo a ese objeto, sin modificar el resto del dibujo ----->  si quitaras push() y pop(), las rotaciones se acumularían en todos los objetos que dibujes después.

- ¿Qué hace rectMode(CENTER)? Realiza algunos experimentos para entender su funcionamiento.

  por defecto se dibuja el rectángulo desde la esquina superior izquierda (CORNER mode). Con rectMode(CENTER), el rectángulo se dibuja centrado en el punto (x, y). lo que hace que al rotar el rectangulo lo hace al rededor de su centro y no de una esquina 

- ¿Cuál es la relación entre el ángulo de rotación y el vector de velocidad? Trata de dibujar en un papel el vector de velocidad y cómo se relaciona con el ángulo de rotación y la operación de traslación y rotación.

  el vector de velociada apunta en direccion en la que se mueve el objeto, se usa el angulo de ese vector para rotar el rectangulo en direccion a la velocidad. El translate(this.position.x, this.position.y) primero mueve el origen al punto donde está el objeto. Luego rotate(angle) alinea el sistema de coordenadas con la dirección de la velocidad.

### Actividad 3

mover.js

```js
class Vehicle {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    
    this.topspeed = 5;

    this.Heading = 0; // guardamos el último ángulo válido
  
    this.r = 16;
  }

  applyForce(force) {
    let f = force.copy();
    //f.div(mass); // ignoring mass right now
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topspeed);
    this.position.add(this.velocity);

    // reseteamos aceleración
    this.acceleration.mult(0);

    // si la velocidad no es cero, actualizamos el ángulo guardado
    if (this.velocity.mag() > 0) {
      this.Heading = this.velocity.heading();
    }
  }

  stop() {
    this.velocity.mult(0);
  }

  edges() {
    if (this.position.x > width) this.position.x = 0;
    else if (this.position.x < 0) this.position.x = width;

    if (this.position.y > height) this.position.y = 0;
    else if (this.position.y < 0) this.position.y = height;
  }

  show() {
    let angle = this.Heading; // usamos el último ángulo válido

    push();
    translate(this.position.x, this.position.y);
    rotate(angle + PI / 2);
    stroke(0);
    strokeWeight(2);
    fill(175);
    beginShape();
    vertex(0, -this.r * 1.5);
    vertex(-this.r, this.r);
    vertex(this.r, this.r);
    endShape(CLOSE);
    pop();
  }
}
```

sketch.js

```js
let vehicle;

function setup() {
  createCanvas(640, 240);
  vehicle = new Vehicle();
}

function draw() {
  background(255);

  if (keyIsDown(LEFT_ARROW)) {
    vehicle.applyForce(createVector(-0.5, 0));
  } else if (keyIsDown(RIGHT_ARROW)) {
    vehicle.applyForce(createVector(0.5, 0));
  } else {
    vehicle.stop(); // ahora se queda quieto pero mantiene dirección
  }

  vehicle.update();
  vehicle.edges();
  vehicle.show();
}
```
en cada update() la aceleración se reinicia con this.acceleration.mult(0), entonces si no hay tecla pulsada no queda fuerza acumulada → el movimiento se detiene.

### Actividad 4

- Identifica motion 101. ¿Qué modificación hay que hacer al motion 101 cuando se quiere agregar fuerzas acumulativas? Trata de recordar     por qué es necesario hacer esta modificación.

 en applyForce la fuerza produce aceleracion siendo a=F/m, lo cual hace que cambien la velocidad y esta velocidad cambie la posicion, cuando hay varias fuerzas se acumulan todas en la aceleracion, haciendo que en cada frame todas las fuerzas se sumen en applyForce y despues de usarlas en update se resetea la aceleracion en cero, de lo contrario las fuerzas se acumularan indefinidamente.

 esto mismo se aplica al movimiento angular, donde hay un torque que representa la fuerza, angularAcceleration) que es la aceleración lineal, velocidad angular (angularVelocity) que es la velocidad lineal. Ángulo (angle) que equivale a posicion 

- Identifica dónde está el Attractor en la simulación. Cambia el color de este.

 el attractor se inicializa attractor = new Attractor(); en el centro y se dibuja en draw con attractor.display(); para cambiarle el color hay que editarlo dentro de attractor.js:

 ```js
display() {
    ellipseMode(CENTER);
    stroke(0);
    if (this.dragging) {
      fill(50);
    } else if (this.rollover) {
      fill(100);
    } else {
      fill(175, 200);
    }
    fill(0, 150, 255, 200);
    ellipse(this.position.x, this.position.y, this.mass * 2);
  }
 ```

- Observa que el Attractor tiene dos atributos this.dragging y this.rollover. Estos atributos no se modifican en el código, pero            permitirían mover el attractor con el mouse y cambiar su color cuando el mouse está sobre él. ¿Cómo podrías modificar el código para      que esto funcione? considera las funciones que ofrece p5.js para interactuar con el mouse.

 el rollover identifica si el mouse esta encima de del attractor y dragging identifica si se hace click sobre el attractor, el codigo modificado es este 

 en attractor.js

 ```js
class Attractor {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.mass = 20;
    this.G = 1;
    this.dragOffset = createVector(0, 0);
    this.dragging = false;
    this.rollover = false; 
  }

  attract(mover) {
    // Calculate direction of force
    let force = p5.Vector.sub(this.position, mover.position);
    // Distance between objects
    let distance = force.mag();
    // Limiting the distance to eliminate "extreme" results for very close or very far objects
    distance = constrain(distance, 5, 25);

    // Calculate gravitional force magnitude
    let strength = (this.G * this.mass * mover.mass) / (distance * distance);
    // Get force vector --> magnitude * direction
    force.setMag(strength);
    return force;
  }

  // Method to display
  display() {
    ellipseMode(CENTER);
    stroke(0);
    if (this.dragging) {
      fill(50);
    } else if (this.rollover) {
      fill(100);
    } else {
      fill(175, 200);
    }
    ellipse(this.position.x, this.position.y, this.mass * 2);
  }
  
  // The methods below are for mouse interaction
  handlePress(mx, my) {
    let d = dist(mx, my, this.position.x, this.position.y);
    if (d < this.mass) {
      this.dragging = true;
      this.dragOffset.x = this.position.x - mx;
      this.dragOffset.y = this.position.y - my;
    }
  }

  handleHover(mx, my) {
    let d = dist(mx, my, this.position.x, this.position.y);
    if (d < this.mass) {
      this.rollover = true;
    } else {
      this.rollover = false;
    }
  }

  stopDragging() {
    this.dragging = false;
  }

  handleDrag(mx, my) {
    if (this.dragging) {
      this.position.x = mx + this.dragOffset.x;
      this.position.y = my + this.dragOffset.y;
    }
  }
}
 ```

y el sketch,js:

```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let movers = [];
let attractor;

function setup() {
  createCanvas(640, 240);

  for (let i = 0; i < 20; i++) {
    movers.push(new Mover(random(width), random(height), random(0.1, 2)));
  }
  attractor = new Attractor();
}

function draw() {
  background(255);

  attractor.display();

  for (let i = 0; i < movers.length; i++) {
    let force = attractor.attract(movers[i]);
    movers[i].applyForce(force);

    movers[i].update();
    movers[i].show();
  }
}

function mouseMoved() {
  attractor.handleHover(mouseX, mouseY);
}

function mousePressed() {
  attractor.handlePress(mouseX, mouseY);
}

function mouseDragged() {
  attractor.handleHover(mouseX, mouseY);
  attractor.handleDrag(mouseX, mouseY);
}

function mouseReleased() {
  attractor.stopDragging();
}

```

handlePress(mx, my) detecta si se hizo click dentro del circulo y guarda el offset entre el mouse y el centro para que al arrastrar no salte.

en handleHover(mx, my) se activa el rollover si el mouse esta dentro del radio, stopDragging() suelta el objeto cuando dejas de presionar el mouse.

y en handleDrag(mx, my) se actualiza la posicion del attractor mientras se arrastra con el mouse. posición = mouse + offset capturado.

### Actividad 5

- Observa de nuevo esta parte del código ¿Cuál es la relación entre r y theta con las posiciones x y y? Puedes repasar entonces la definición de coordenadas polares y cómo se convierten a coordenadas cartesianas.

  su relacion es la transformacion entre coordenadas polares y coordenadas cartesianas, en polares un punto se represneta por P(r,θ), y en cartesianas P(x,y). la conversion de polares a cartesianas es x = r * cos(theta) y y = r * sin(theta), el coseno controla la proyección en el eje X, el seno controla la proyección en el eje Y. lo que haces es mover el punto en un círculo alrededor del centro. al incrementar theta, el punto parece girar, aunque en realidad solo cambias el ángulo con una distancia r y recalculas sus coordenadas cartesianas.

- primera modificacion

 p5.Vector.fromAngle(theta) es un vector unitario de longitud 1 apuntando en la dirección del ángulo theta. circle(v.x, v.y, 48); hace que se dibuje un circulo en la posicion (x,y)=(cos(θ),sin(θ)). El centro del círculo siempre queda a 1 unidad del origen, ya que no se multiplica por r, haciendo que no se vea el movimiento 

- segunda modificacion

  fromAngle() recibe dos parámetros: el ángulo y la magnitud, haciendo que el vector apunte a direccion teta con una longitud r, esto hace que el círculo gira con un radio grande r, como en el código original.

  siendo la versión "vectorizada" de la conversión polar a cartesiana.

### Actividad 6

- Recuerda estos conceptos: velocidad angular, frecuencia, periodo, amplitud y fase.

 Amplitud (A): Es el valor máximo (positivo o negativo) que alcanza la onda.

 Velocidad angular (ω): Es la rapidez con la que la onda oscila en radianes por segundo.

 Frecuencia (f): Número de oscilaciones por segundo

 Periodo (T): El tiempo que tarda la función en completar un ciclo completo.

 Fase (φ): El “desplazamiento inicial” de la onda en el tiempo.
  
- Realiza una simulación en la que puedas modificar estos parámetros y observar cómo se comporta la función sinusoide.

  tomando el ejemplo del profe quise modificar el parametro de velocidad angular

 ```js
  let amplitude = 200;
let angle = 0;
let angleVelocity = 0.05;

function setup() {
  createCanvas(640, 240);
}

function draw() {
  background(255);
  
  let x = amplitude * sin(angle);
  angle += angleVelocity;

  stroke(0);
  strokeWeight(2);
  translate(width / 2, height / 2);

  fill(127);
  line(0, 0, x, 0);
  circle(x, 0, 48);
   
}

function keyPressed(){
  angleVelocity += TWO_PI/360;  
}
```

### Actividad 7


## Explicación conceptual de la obra

* ¿Qué concepto de la unidad 4 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:xd
>

* ¿Qué concepto de la unidad 3 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
>

* ¿Qué concepto de la unidad 2 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
>

* ¿Qué concepto de la unidad 1 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
>

## ¿Cómo resolviste la interacción?
> Tu respuesta aquí:
>

## Enlace a la obra en el editor de p5.js

[Aquí está mi obra](URL)

## Código de la obra 

``` js

```

## Captura de pantalla representativa









