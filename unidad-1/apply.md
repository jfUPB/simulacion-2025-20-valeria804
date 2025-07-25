# Unidad 1

## 🛠 Fase: Apply

- Una obra generativa es una creacion artistica o visual que no esta completamente bajo el control de su autor, sino que es producto de un sistema que produce resultados de manera autónoma. En lugar de diseñar cada detalle de la obra manualmente, el artista establece un conjunto de reglas, algoritmos o condiciones y deja que el sistema genere variaciones o versiones unicas. Este tipo de obras se distingue por incluir elementos como la aleatoriedad, el codigo, la interaccion o la respuesta a datos externos. de este modo, una misma obra puede ofrecer diferentes resultados cada vez que se ejecuta

- 
 ```js
let branches = [];
let flowers = [];
let flowerWalker;

function setup() {
  createCanvas(600, 600);
  background(255);

  for (let i = 0; i < 3; i++) {
    branches.push(new Branch(random(width), height));
  }

  flowerWalker = new FlowerWalker();
}

function draw() {
  background(255);

  // Crear nuevas ramas cada cierto tiempo
  if (frameCount % 100 === 0) {
    branches.push(new Branch(random(width), height));
  }

  // Dibujar ramas
  for (let branch of branches) {
    branch.update();
    branch.display();
  }

  // Crear flores constantemente
  if (frameCount % 5 === 0) {
    flowers.push(flowerWalker.step());
  }

  // Dibujar flores
  for (let flower of flowers) {
    flower.display();
  }
}

function mousePressed() {
  branches.push(new Branch(mouseX, height));
}

// Clase rama
class Branch {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.offset = random(1000);
    this.path = [];
  }

  update() {
    // Movimiento orgánico base
    let angle = noise(this.offset) * TWO_PI;
    let organicOffset = cos(angle) * 0.5;

    // Inclinación hacia el mouse
    let targetX = mouseX;
    let diffX = targetX - this.pos.x;
    let steer = map(diffX, -width / 2, width / 2, -1, 1);  // normalizamos

    // Combinamos ambos
    let xOffset = organicOffset + steer * 0.5;

    this.offset += 0.01;
    this.pos.x += xOffset;
    this.pos.y -= 2.5;

    this.path.push(this.pos.copy());
  }

  display() {
    strokeWeight(2);
    stroke(70, 40, 20);
    noFill();
    beginShape();
    for (let p of this.path) {
      vertex(p.x, p.y);
    }
    endShape();
  }
}


// Clase flor
class Flower {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.size = random(8, 16);
    this.color = color(random(200, 255), random(120, 200), random(200, 255), 220);
  }

  display() {
    noStroke();
    fill(this.color);
    ellipse(this.pos.x, this.pos.y, this.size);
  }
}

// Caminante con pasos más espaciados (cortos y largos)
class FlowerWalker {
  constructor() {
    this.x = random(width);
    this.y = random(height * 0.05, height * 0.35);
  }

  step() {
    let angle = random(TWO_PI);
    let stepSize = this.levy();

    this.x += cos(angle) * stepSize;
    this.y += sin(angle) * stepSize;

    this.x = constrain(this.x, 0, width);
    this.y = constrain(this.y, height * 0.05, height * 0.4);

    return new Flower(this.x, this.y);
  }

  levy() {
    let r = random(1);
    if (r < 0.01) {
      return random(80, 160); // salto grande
    } else {
      return abs(randomGaussian() * 8); // pasos suaves pero más largos
    }
  }
}

 ```

https://editor.p5js.org/valeria804/sketches/0kYsigTjX


<img width="449" height="446" alt="Captura de pantalla 2025-07-25 070341" src="https://github.com/user-attachments/assets/ec0bef6a-56d0-49aa-ae4f-33b8b975b2b7" />
