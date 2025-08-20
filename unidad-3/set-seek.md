# Unidad 3

## 🔎 Fase: Set + Seek

### Actividad 4

el marco motion 101 tiene el objetivo de simular el movimiento de un objeto

Variables principales del Mover

- position → la ubicación del objeto en el lienzo (un vector createVector(x, y)).

- velocity → la rapidez y dirección en que se mueve el objeto.

- acceleration → la variación de la velocidad (lo que “empuja” al objeto).

- topSpeed → límite máximo de la velocidad.

en cada frame se actualiza la fisica del objeto. Básicamente: posición = posición + velocidad; velocidad = velocidad + aceleración.
```js 
this.velocity.add(this.acceleration); // suma aceleración a la velocidad
this.velocity.limit(this.topSpeed);   // limita la velocidad máxima
this.position.add(this.velocity);     // actualiza la posición
```

Si defines una aceleración constante hacia abajo, simulas gravedad.
Si haces que la aceleración apunte hacia el mouse, simulas atracción.
Si añades fuerzas variadas, puedes simular partículas, boids, colisiones, etc.

### Actividad 5

1. el problema es que esta mostrando solamente la ultima variable que se guardo. o sea que se esta sobrescribiendo la aceleración con una sola fuerza.
2. lo que hay que hacer es sumar todas las fuerzas y ahi aplicarlas a la aceleracion
3. ```js
   applyForce(force) {
    // Como m = 1, a = F
    this.acceleration.add(force);
    }
   ```

### Actividad 6

1. como la aceleracion no es permanente, una vez aplicas esas fuerzas, la aceleración “ya hizo su trabajo” al modificar la velocidad., por lo tanto es necesario borrarlo ya que ya fue aplicado. Si no se borra, en el siguiente frame se volvería a aplicar la misma fuerza acumulada. La aceleración se recalcula en cada frame a partir de las fuerzas de ese momento.
   
2. se resetaea la aceleracion para preparar al siguiente frame. si se resetea antes se pierde la informacion antes de que afecte a la velocidad y si nunca se resetea se acumularía indefinidamente, simulando una fuerza constante “fantasma” aunque ya no se aplique. Multiplicamos la aceleración por 0 al final de update() porque la aceleración es “temporal”, depende solo de las fuerzas de ese frame.
En el siguiente frame se recalcula con las nuevas fuerzas, mientras que la velocidad sí sigue acumulando inercia.

### Actividad 7

lo que sucede aqui es que se esta afectando tambien al objto original (wind, gravety). La masa solo afecta cómo se convierte esa fuerza en aceleración para ese objeto, no debería alterar el vector global de la fuerza.

el hecho de que force es un objeto que se pasa por referencia hace que cualquier cambio dentro de la función afecta al objeto externo.

Tipos primitivos (números, booleanos, strings, null, undefined, Symbol, BigInt)
→ se pasan por valor.
Eso significa que si pasas una variable de este tipo a una función, dentro de la función se crea una copia independiente.
Cambiarla dentro de la función no afecta a la variable original.

Objetos y arrays (y por extensión instancias como p5.Vector)
→ se pasan por referencia.
Eso significa que dentro de la función no recibes una copia, sino una referencia al mismo objeto en memoria.
Cambiar sus propiedades sí afecta al original.

### Actividad 8

1. en "let friction = this.velocity.copy();" copy() crea un nuevo objeto p5.Vector con los mismos valores numéricos. friction y this.velocity son dos objetos distintos en memoria. Si modificas friction, no se altera this.velocity. mientras que "let friction = this.velocity;" no crea una copia, se asigna otra referencia al mismo objeto en memoria. Si modificas friction, también cambias this.velocity.

2. Si en lugar de copiar la velocidad haces que friction y velocity apunten al mismo objeto, al modificar la fricción estarías alterando la velocidad original directamente

3. this.velocity.copy() → nuevo objeto → pasa por valor (se copia el contenido numérico de cada componente).
this.velocity → misma referencia al objeto → pasa por referencia (ambas variables apuntan al mismo vector en memoria).

### Actividad 9

#### friccion 

- la fuerza de viento fue modelada como un vector con direccion hacia la posicion del mouse y su magnitud depende la distancia del objeto al mouse

  ```js
  let dir = createVector(mouseX, mouseY).sub(mover.position);
   dir.setMag(0.1); // magnitud controlada
   mover.applyForce(dir);
  ```
la fuerza de friccion se calcula cuando el objeto toca el suelo. parte de la velocidad pero con direccion opuesta, y su magnitud depende del coeficiente c de cada objeto. tambien cuando entra en contacto con las paredes del canva

```js
if (mover.contactEdge()) {
  let friction = mover.velocity.copy();
  friction.mult(-1);
  friction.setMag(mover.frictionCoefficient);
  mover.applyForce(friction);
}
```
el uso de un vector de gravedad 

```js
let gravity = createVector(0, 0.1 * mover.mass);
mover.applyForce(gravity);
```
<img width="540" height="447" alt="Captura de pantalla 2025-08-20 065225" src="https://github.com/user-attachments/assets/cac695f1-f6da-410f-b291-6bcc05972403" />


#### resistencia del aire y de fluidos 

en la obra generativa tambien aplique este concepto con las StickyZones, las cuales son esferas que se agregan con click derecho y que crean una zona amarilla en la que si el objeto entre en contacto con ella genera como un arrastre extra. cada zona tiene un coeficiente de arrastre adicional (drag force)

esta modelado asi: 

Dirección: opuesta a la velocidad (m.vel.copy().mult(-1)).
Magnitud: constante e independiente de la velocidad, igual al mu de la zona (friction.setMag(z.mu)).
Activación: solo mientras el mover está dentro del radio de la zona.

v es quevalente a m.vel y se crea un vector opuesto a la velocidad (mult(-1)).

usando la formula proporcional a v^2
```js
let v = m.vel.mag();
if (v > 0) {
  let drag = m.vel.copy().mult(-1);
  drag.setMag(z.mu * v * v); // now F ~ v^2
  m.applyForce(drag);
}
```

solo se activa dentro de la zona

```js
if (m.pos.dist(z.pos) < z.r) {
   // aquí se activa la sticky force
}
```

"drag.setMag(z.mu * v * v);"

z.mu es la constante 𝑘 (la viscosidad o resistencia del "fluido").
v * v es v^2, la magnitud de la velocidad al cuadrado.
setMag(...) asegura que ese vector opuesto tenga la magnitud correcta según la fórmula.

<img width="537" height="449" alt="Captura de pantalla 2025-08-20 065146" src="https://github.com/user-attachments/assets/f9b7a510-248e-4455-b799-3ff100fca21b" />

#### codigo

mover.js

```js
// mover.js
class Mover {
  constructor(x, y, m, frictionCoeff) {
    this.pos = createVector(x, y);
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.mass = m;
    this.radius = m * 8;
    this.c = frictionCoeff; // coeficiente de fricción propio
    this.trail = []; // para estela
    this.maxTrail = 120;
  }

  applyForce(force) {
    // F = ma -> a = F/m
    let f = p5.Vector.div(force, this.mass);
    this.acc.add(f);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);

    // guardar en el trail (para el efecto generativo)
    this.trail.push(this.pos.copy());
    if (this.trail.length > this.maxTrail) this.trail.shift();
  }

  // aplica fricción normal (ej: contacto con el suelo)
  applyFriction(ifContact = true, frictionOverride = null) {
    if (!ifContact) return;

    let mu = frictionOverride !== null ? frictionOverride : this.c;
    let friction = this.vel.copy();
    if (friction.mag() === 0) return;
    friction.mult(-1);
    friction.setMag(mu);
    this.applyForce(friction);
  }

  bounceEdges() {
    let bounce = -0.9;
    // derecha/izquierda
    if (this.pos.x > width - this.radius) {
      this.pos.x = width - this.radius;
      this.vel.x *= bounce;
    } else if (this.pos.x < this.radius) {
      this.pos.x = this.radius;
      this.vel.x *= bounce;
    }
    // suelo
    if (this.pos.y > height - this.radius) {
      this.pos.y = height - this.radius;
      this.vel.y *= bounce;
    }
    // techo
    if (this.pos.y < this.radius) {
      this.pos.y = this.radius;
      this.vel.y *= bounce;
    }
  }

  contactEdge() {
    return (this.pos.y >= height - this.radius - 1);
  }

  display(baseColor) {
    noStroke();
    // pintar estela generativa
    push();
    for (let i = 0; i < this.trail.length; i++) {
      let p = this.trail[i];
      let t = i / this.trail.length;
      let alpha = map(i, 0, this.trail.length, 10, 120);
      let size = map(t, 0, 1, this.radius * 0.2, this.radius * 1.1);
      fill(red(baseColor), green(baseColor), blue(baseColor), alpha);
      ellipse(p.x, p.y, size, size);
    }
    pop();

    // cuerpo
    stroke(30);
    strokeWeight(1);
    fill(baseColor);
    ellipse(this.pos.x, this.pos.y, this.radius * 2, this.radius * 2);
  }
}
```

sketch.js

```js
// sketch.js - Campo de Deslizamiento
let moverA, moverB;
let stickyZones = []; // array de zonas {x,y,r,mu}
let gravityOn = true;
let windPower = 0.8;
let dragging = false;
let dragStart;

function setup() {
  createCanvas(600,500);
  pixelDensity(1);
  background(20);

  // moverA: alta fricción (ancla)
  moverA = new Mover(width * 0.33, 80, 3.5, 0.25);

  // moverB: baja fricción (vagabundo)
  moverB = new Mover(width * 0.66, 80, 2.2, 0.03);
}

function draw() {
  // ligero fade para acumulación visual
  fill(0, 0, 0, 16);
  rect(0, 0, width, height);

  // fondo
  noStroke();
  fill(8, 10, 18, 80);
  rect(0, 0, width, height);

  // manejar movers
  handleMover(moverA, color(220, 110, 90));
  handleMover(moverB, color(100, 160, 220));

  // dibujar zonas pegajosas
  drawStickyZones();
}

function handleMover(m) {
  // gravedad
  let g = createVector(0, 0.12 * m.mass);
  if (gravityOn) m.applyForce(g);

  // arrastre con mouse
  if (dragging) {
    let dragVec = p5.Vector.sub(createVector(mouseX, mouseY), dragStart);
    let force = dragVec.copy().setMag(min(dragVec.mag() * 0.03, 2.0));
    m.applyForce(force);
  }

  // aplicar sticky zones (nueva función)
  let inSticky = applyStickyZones(m);

  // fricción normal si toca suelo y no está en zona pegajosa
  if (!inSticky && m.contactEdge()) {
    m.applyFriction(true, null);
  }

  m.bounceEdges();
  m.update();
  m.display((m.c > 0.1) ? color(220, 120, 100, 180) : color(100, 140, 220, 140));
}

// NUEVA FUNCIÓN
function applyStickyZones(m) {
  let inSticky = false;
  for (let z of stickyZones) {
    let d = dist(m.pos.x, m.pos.y, z.x, z.y);
    if (d < z.r + m.radius) {
      // calcular fricción especial aquí directamente
      let v = m.vel.mag();
      if (v > 0) {
      let drag = m.vel.copy().mult(-1);
       drag.setMag(z.mu * v * v); // now F ~ v^2
       m.applyForce(drag);
      }

      inSticky = true;

      // efecto visual
      push();
      noStroke();
      fill(255, 200, 80, 40);
      ellipse(m.pos.x, m.pos.y, 8 + (z.r - d) * 0.2);
      pop();
    }
  }
  return inSticky;
}

function mousePressed() {
  if (mouseButton === LEFT) {
    dragging = true;
    dragStart = createVector(mouseX, mouseY);
  } else if (mouseButton === RIGHT) {
    // crear zona pegajosa en el mouse
    stickyZones.push({ x: mouseX, y: mouseY, r: 70, mu: 0.35 });
  }
  return false;
}

function mouseReleased() {
  if (mouseButton === LEFT) {
    let dragEnd = createVector(mouseX, mouseY);
    let dir = p5.Vector.sub(dragEnd, dragStart);
    let force = dir.copy().setMag(min(dir.mag() * 0.02, 6.0));
    moverA.applyForce(force);
    moverB.applyForce(force);
    dragging = false;
  }
}

function drawStickyZones() {
  for (let z of stickyZones) {
    noFill();
    stroke(255, 180, 60, 80);
    strokeWeight(2);
    ellipse(z.x, z.y, z.r * 2);
    noStroke();
    fill(255, 180, 60, 8);
    ellipse(z.x, z.y, z.r * 1.4);
  }
}
```

https://editor.p5js.org/valeria804/sketches/ErtGwd71X 
