# Evidencias de la unidad 4

## Explicación conceptual de la obra

* ¿Qué concepto de la unidad 4 y cómo lo aplicaste en la obra?
> Tu respuesta aquí: tome el concepto de pendulo y su movimiento oscilatorio; lo extendi para que algunos pendulos fueran simples y otros dobles. estan distrubuidos en la parte superior del lienzo 
>

* ¿Qué concepto de la unidad 3 y cómo lo aplicaste en la obra?
> Tu respuesta aquí: aplique la fuerza de friccion y la atraccion, el mouse actúa como un atractor con un rango amplio de influencia
>

* ¿Qué concepto de la unidad 2 y cómo lo aplicaste en la obra?
> Tu respuesta aquí: aplique el uso de vectores y el marco motion 101
> Los vectores se utilizan para calcular la posición del bob del pendulo. ademas, el motion 101 se refleja en como se actualizan las velocidades y aceleraciones angulares de los pendulos de manera gradual y controlada, lo que produce un movimiento mas natural.

* ¿Qué concepto de la unidad 1 y cómo lo aplicaste en la obra?
> Tu respuesta aquí: aplique el Lévy flight y del ruido Perlin
> El levy flight se usa para definir las longitudes iniciales de los pendulos y para decidir si sera simple o doble, de manera que la mayoria son simples pero de vez en cuando aparece un doble inesperado. El ruido perlin se aplica para variar suavemente los colores y grosores de los pendulos

## ¿Cómo resolviste la interacción?
> Tu respuesta aquí: cuando el cursor se acerca a los pendulos, estos sienten una fuerza de atraccion que modifica su movimiento
> al hacer clic sobre el lienzo, el pendulo mas cercano recibe un impulso extra en su velocidad angular, lo que le da al usuario la capacidad de influir directamente en la dinámica del sistema

## Enlace a la obra en el editor de p5.js

[Aquí está mi obra](https://editor.p5js.org/valeria804/sketches/7YWMo8-1J)

## Código de la obra 

pendulum.js

``` js
class Pendulum {
  constructor(origin, r, isDouble = false) {
    this.origin = origin.copy();
    this.r = r;
    this.angle = random(-PI / 4, PI / 4);

    this.aVelocity = 0.0;
    this.aAcceleration = 0.0;
    this.damping = 0.995; // fricción
    this.ballr = 20;

    this.isDouble = isDouble;
    if (this.isDouble) {
      this.child = new Pendulum(
        createVector(0, 0),
        r * 0.7,
        false
      );
    }
  }

  update(gravity, attractor) {
    let force = gravity * sin(this.angle);

    // --- atracción del mouse (Motion 101) ---
    let bob = this.getBob();
    let dir = p5.Vector.sub(attractor, bob);
    let d = dir.mag();
    let range = 250; // rango más amplio
    if (d < range) {
      dir.normalize();
      let strength = map(d, 0, range, 0.2, 0); // fuerza más fuerte pero suave
      force += strength * dir.x; // influye en la aceleración angular
    }

    this.aAcceleration = (-1 * force) / this.r;
    this.aVelocity += this.aAcceleration;
    this.aVelocity *= this.damping;
    this.angle += this.aVelocity;

    if (this.isDouble) {
      this.child.origin = this.getBob();
      this.child.update(gravity, attractor);
    }
  }

  applyImpulse() {
    this.aVelocity += random(-0.2, 0.2); // impulso extra
  }

  getBob() {
    let x = this.r * sin(this.angle) + this.origin.x;
    let y = this.r * cos(this.angle) + this.origin.y;
    return createVector(x, y);
  }

  display(colA, colB, noiseFactor) {
    let bob = this.getBob();

    stroke(lerpColor(colA, colB, noise(noiseFactor)));
    strokeWeight(3 + noise(noiseFactor + 100) * 3);
    line(this.origin.x, this.origin.y, bob.x, bob.y);
    fill(lerpColor(colA, colB, noise(noiseFactor + 200)));
    ellipse(bob.x, bob.y, this.ballr * 1.2);

    if (this.isDouble) {
      this.child.display(colA, colB, noiseFactor + 500);
    }
  }
}

```

sketch.js

```js
let pendulums = [];
let gravity = 0.4;
let attractor;
let colA, colB;

function setup() {
  createCanvas(600, 500);

  colA = color(50, 150, 200);
  colB = color(100, 255, 150);

  attractor = createVector(width / 2, height / 2);

  // --- generar número aleatorio de péndulos ---
  let numPendulums = int(random(20, 40));

  for (let i = 0; i < numPendulums; i++) {
    // --- distribución superior ---
    let x = i * (width / numPendulums) + random(-10, 10);
    let y = random(20, 60);

    // --- Lévy flight para longitud y ángulo ---
    let r = levyFlightLength();

    // --- probabilidad de dobles (menos frecuentes) ---
    let isDouble = random(1) < 0.25;

    pendulums.push(new Pendulum(createVector(x, y), r, isDouble));
  }
}

function draw() {
  background(240);

  attractor.set(mouseX, mouseY);

  for (let i = 0; i < pendulums.length; i++) {
    pendulums[i].update(gravity, attractor);
    pendulums[i].display(colA, colB, i * 0.1);
  }
}

// --- impulso al péndulo más cercano ---
function mousePressed() {
  let closest = null;
  let minDist = Infinity;

  for (let p of pendulums) {
    let d = p.getBob().dist(createVector(mouseX, mouseY));
    if (d < minDist) {
      minDist = d;
      closest = p;
    }
  }

  if (closest) closest.applyImpulse();
}

// --- Lévy flight ---
function levyFlightLength() {
  let beta = 1.5;
  let u = randomGaussian(0, 1) * pow(abs(randomGaussian(0, 1)), -1 / beta);
  let step = constrain(abs(u) * 60, 60, 200);
  return step;
}

```

## Captura de pantalla representativa


<img width="399" height="333" alt="Captura de pantalla 2025-09-05 075550" src="https://github.com/user-attachments/assets/69a1db1d-eadf-4454-9c4a-bad69ca891e7" />














