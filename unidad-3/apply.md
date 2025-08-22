# Unidad 3


## 🛠 Fase: Apply

una obra en la que las particulas se atraen gravitacionalmente como estrellas. los tamaños de las estrellas se generan con una distribucion que privilegia valores pequeños, levy flight, de modo que la mayoria sean diminutas. a medida que pasa el tiempo. las estrellas tienen una ligera perturbacion en su direccion que forma corrientes continuas en el espacio, definidas por el ruido perlin. y cada vez que se hace un click se agrega una estralla

para modelar los n cuerpos aplique el metodo attract(other) de cada Body,en donde se calcula la atraccion gravitatoria entre dos cuerpos con la ley de newton

```js
let force = p5.Vector.sub(this.pos, other.pos);
    let d = constrain(force.mag(), 5, 25); // evitar singularidades
    let G = 1;
    let strength = (G * this.mass * other.mass) / (d * d);
    force.setMag(strength);
    other.applyForce(force);
  }
```
la ley de gravitacion universal se implemento en cada par de particulas, usando masas para la intensidad, fuerza inversa al cuadrado de la distancia, y actualizando con integracion frame a frame.

doby.js

```js
class Body {
  constructor(x, y, m) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(0.5);
    this.acc = createVector(0, 0);
    this.mass = m;
    this.r = sqrt(this.mass) * 2; // radio relacionado a la masa
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acc.add(f);
  }

  attract(other) {
    let force = p5.Vector.sub(this.pos, other.pos);
    let d = constrain(force.mag(), 5, 25); // evitar singularidades
    let G = 1;
    let strength = (G * this.mass * other.mass) / (d * d);
    force.setMag(strength);
    other.applyForce(force);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  show() {
    noStroke();
    fill(255, 200);
    ellipse(this.pos.x, this.pos.y, this.r * 2);
  }
}
```

sketch.js

```js
let bodies = [];
let noiseOffset = 0;

function setup() {
  createCanvas(700,700);

  // Crear distribución custom: más cuerpos pequeños que grandes
  for (let i = 0; i < 30; i++) {
    let mass = LevyFlight(); 
    bodies.push(new Body(random(width), random(height), mass));
  }
}

function draw() {
  // Fondo con Perlin noise suave
  let bg = map(noise(noiseOffset), 0, 1, 10, 30);
  background(bg, bg * 2, bg * 3);
  noiseOffset += 0.01;

  // Interacciones gravitatorias (N cuerpos)
  for (let i = 0; i < bodies.length; i++) {
    for (let j = 0; j < bodies.length; j++) {
      if (i != j) {
        bodies[i].attract(bodies[j]);
      }
    }
  }

  // Actualizar y mostrar
  for (let body of bodies) {
    body.update();
    body.show();
  }
}

// Genera una distribución sesgada: mayoría pequeñas masas, pocas grandes
function LevyFlight() {
  let r = random();
  return pow(r, 4) * 50 + 2; // ajusta exponente para más sesgo
}

// Interactividad: click = nueva estrella
function mousePressed() {
  let mass = customDistribution();
  bodies.push(new Body(mouseX, mouseY, mass));
}
```

https://editor.p5js.org/valeria804/sketches/sXV_65vny 

<img width="461" height="463" alt="Captura de pantalla 2025-08-22 070726" src="https://github.com/user-attachments/assets/0b7ee58e-be35-4112-9a64-3593cfac7e94" />

