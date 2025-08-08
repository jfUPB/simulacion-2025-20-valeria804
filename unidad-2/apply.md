# Unidad 2


## 🛠 Fase: Apply

### Actividad 8 

- la obra esta compuesta por una multitud de particulas que se comportan con cierta autonomia pero que tambien responden colectivamente a fuerzas como la atraccion hacia el cursor, la friccion del movimiento y la separacion entre si. quiero representar las conexiones emocionales atraves de los diferentes comportamientos que presenta las particulas. Cuando una particula se acerca al cursor, sus colores cambian, haciendose mas intensos o mas calidos, evocando emociones fuertes.Si se mueve rapidamente, su trazo se alarga y su tonalidad cambia, las particulas estan lejos del cursor se oscurecen representando la lejania emocional

- en la ruta que sigue un objeto al moverse, los movimientos seran organicos gracias al uso de ruido Perlin, lo que generara un movimiento fluido. Cuando ocurra el movimiento y como se modulara en el tiempo, se aplicara al variar la velocidad de las particulas segun fuerzas internas (separacion) y externas (distancia al cursor). Sensacion de masa del objeto, las particulas pareceran tener peso gracias a como la aceleracion y friccion limitan la velocidad. y como las particulas seran atraidas al cursor

- se usara el algoritmo de movimiento interactivo, esto ya que las particulas estaran atraidas por el cursor

- su interactividad seran numeros del 1 al 4 para cambiar el color de las particulas y el mause para controlar su movimiento

```js
let movers = [];
let colorSystem;
let t = 0.0;

const CONFIG = {
  particles: {
    count: 100,
    separationRadius: 25
  },
  forces: {
    noiseStrength: 0.4,
    separation: 1
  }
};

function setup() {
  createCanvas(1000, 700);
  colorMode(RGB);
  
  colorSystem = new EmotionalColorSystem();
  
  // Crear partículas
  for (let i = 0; i < CONFIG.particles.count; i++) {
    movers.push(new EmotionalMover(
      random(width * 0.3, width * 0.7),
      random(height * 0.3, height * 0.7)
    ));
  }
}

function draw() {
  background(20, 20, 30, 40); // Rastro suave
  
  // Actualizar y dibujar partículas
  for (let mover of movers) {
    mover.update();
    mover.display();
  }

  t += 0.01;
}

class EmotionalMover {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(random(-0.5, 0.5), random(-0.5,       0.5));
    this.acceleration = createVector();
    this.topspeed = random(1.5, 6); 
    this.size = random(6, 12);
    this.personalityFactor = random(0.4, 1.6);// determina la atraccion(sesibilidad a la distancia)
    this.speedMultiplier = random(0.8, 1.5); 
  }

  update() {
    // 1. Dirección hacia el mouse
    let mouse = createVector(mouseX, mouseY);
    
    let dir = p5.Vector.sub(mouse, this.position);// Vector hacia el mouse

    dir.normalize();

    dir.mult(0.15 * this.personalityFactor);// Vector hacia el mouse
    
    this.acceleration = dir;
    
    // 2. Agrega ruido emocional
    let chaosLevel = map(p5.Vector.dist(mouse, this.position), 0, 200, 0.6, 0.1);//calcula el caos
    let noiseForce = this.calculateEmotionalNoise(chaosLevel);//determina el ruido de la particula, si se mueve calmado o caotico
    this.acceleration.add(noiseForce);
    
    // 3. Agrega separación entre partículas
    let separationForce = this.calculateSeparation();
    this.acceleration.add(separationForce);
    
    // 4. Aplicar física
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topspeed);
    this.position.add(this.velocity);
    
    // Mantener en pantalla
    this.handleBoundaries();
  }
  
  //metodo
  calculateEmotionalNoise(chaosLevel) {
    
    // Crear offset único para cada partícula basado en su posición
    let xoff = this.position.x * 0.01 + t;
    let yoff = this.position.y * 0.01 + t;
    
    // Ruido para X e Y 
    let noiseX = noise(xoff) * 2 - 1; // crea direccion
    let noiseY = noise(yoff) * 2 - 1;
    
    // Crear vector que apunta a direccion aleatoria
    let noiseVec = createVector(noiseX, noiseY);
    let magnitude = chaosLevel * CONFIG.forces.noiseStrength *         this.personalityFactor;
    
    noiseVec.mult(magnitude);
    return noiseVec;//devuelve la fuerza que se aplicara a la aceleracion
  }
  
  calculateSeparation() {
    let sum = createVector();
    let count = 0;
    
    for (let other of movers) {
      let distance = p5.Vector.dist(this.position, other.position);
      if (distance > 0 && distance < CONFIG.particles.separationRadius) {
        let diff = p5.Vector.sub(this.position, other.position);
        diff.normalize();
        diff.div(distance);
        sum.add(diff);//Calcula un vector que apunta en la dirección opuesta a la otra.
        count++;
      }
    }
    
    if (count > 0) {
      sum.div(count);
      sum.mult(CONFIG.forces.separation);
    }
    
    return sum;//Devuelve esa fuerza aplicada a la partícula
  }
  
  handleBoundaries() {
    let margin = 30;
    
    if (this.position.x < margin) {
      this.velocity.x += 0.1;
    } else if (this.position.x > width - margin) {
      this.velocity.x -= 0.1;
    }
    
    if (this.position.y < margin) {
      this.velocity.y += 0.1;
    } else if (this.position.y > height - margin) {
      this.velocity.y -= 0.1;
    }
    
    this.position.x = constrain(this.position.x, 10, width - 10);
    this.position.y = constrain(this.position.y, 10, height - 10);
  }

  display() {
    let emotionalColor = colorSystem.getEmotionalColor(this);
    
    // Tamaño basado en características de la partícula
    let baseSize = this.size * (0.8 + this.speedMultiplier * 0.4); 
    
    // Variación sutil por velocidad actual (solo un 20% de influencia)
    let speed = this.velocity.mag();
    let speedRatio = speed / this.topspeed;
    let dynamicSize = baseSize * (0.9 + speedRatio * 0.4); 
    
    push();
    translate(this.position.x, this.position.y);
    
    // Halo basado en características de la partícula + proximidad al mouse
    let mouseDistance = dist(mouseX, mouseY, this.position.x, this.position.y);
    let proximityHalo = map(mouseDistance, 0, 150, 40, 5);
    let personalityHalo = this.speedMultiplier * 20;
    let haloAlpha = proximityHalo + personalityHalo;
    
    fill(red(emotionalColor), green(emotionalColor), blue(emotionalColor), haloAlpha);
    noStroke();
    ellipse(0, 0, dynamicSize * 1.6);
    
    // Partícula principal
    fill(emotionalColor);
    noStroke();
    ellipse(0, 0, dynamicSize);
    
    pop();
  }
}

class EmotionalColorSystem {
  constructor() {
    this.palettes = {
      calm: [color(80, 150, 255), color(150, 200, 255)],
      // Azul suave -> Azul claro
      passionate: [color(255, 80, 100), color(255, 150, 50)],   
      // Rojo intenso -> Naranja
      melancholy: [color(100, 80, 150), color(150, 120, 180)],  
      // Púrpura oscuro -> Púrpura claro  
      energetic: [color(255, 200, 50), color(255, 100, 150)]    
      // Amarillo -> Rosa vibrante
    };
    this.currentPalette = 'calm';
  }
  
  getEmotionalColor(mover) {
    let mouseDistance = dist(mouseX, mouseY, mover.position.x, mover.position.y);
    let velocity = mover.velocity.mag();
    
    // Factores emocionales para lerpColor
    let proximityFactor = map(mouseDistance, 0, 300, 1, 0);
    let velocityFactor = map(velocity, 0, mover.topspeed, 0, 1);
    let emotionalIntensity = map(mouseDistance, 0, 150, 1, 0.3);
    
    let [colorA, colorB] = this.palettes[this.currentPalette];
    
    // 1. MEZCLA PRINCIPAL: velocidad determina la mezcla base entre colorA y colorB
    let velocityMix = lerpColor(colorA, colorB, velocityFactor);
    
    // 2. MEZCLA EMOCIONAL: proximidad al mouse añade blanco 
    let excitementColor = color(255, 255, 255);
    let emotionalMix = lerpColor(velocityMix, excitementColor, proximityFactor * 0.4);
    
    // 3. MEZCLA DE INTENSIDAD: distancia modula la intensidad general
    let darkColor = color(50, 50, 80); // Color base oscuro
    let intensityMix = lerpColor(darkColor, emotionalMix, emotionalIntensity);
    
    // 4. MEZCLA TEMPORAL: variación sutil en el tiempo
    let timeColor = color(
      red(colorB) + sin(frameCount * 0.02) * 30,
      green(colorA) + cos(frameCount * 0.025) * 20,
      blue(colorB) + sin(frameCount * 0.018) * 25
    );
    let timeFactor = (sin(frameCount * 0.01 + mover.position.x * 0.01) + 1) * 0.1; // 0-0.2
    let finalMix = lerpColor(intensityMix, timeColor, timeFactor);
    
    // Alpha dinámico basado en velocidad y proximidad
    let baseAlpha = 180;
    let velocityAlpha = map(velocity, 0, mover.topspeed, 0, 75);
    let proximityAlpha = map(mouseDistance, 0, 100, 75, 0);
    let finalAlpha = constrain(baseAlpha + velocityAlpha + proximityAlpha, 120, 255);
    
    return color(red(finalMix), green(finalMix), blue(finalMix), finalAlpha);
  }
  
  setPalette(paletteName) {
    if (this.palettes[paletteName]) {
      this.currentPalette = paletteName;
    }
  }
}

// cambio de paletas
function keyPressed() {
  switch(key) {
    case '1':
      colorSystem.setPalette('calm');
      break;
    case '2':
      colorSystem.setPalette('passionate');
      break;
    case '3':
      colorSystem.setPalette('melancholy');
      break;
    case '4':
      colorSystem.setPalette('energetic');
      break;
  }
}
```

- https://editor.p5js.org/valeria804/sketches/vJSEv4PvB

- <img width="653" height="449" alt="Captura de pantalla 2025-08-07 223238" src="https://github.com/user-attachments/assets/6e2b0a82-4a94-4f43-a5c8-c34a5e9bf890" />

<img width="649" height="441" alt="Captura de pantalla 2025-08-07 223304" src="https://github.com/user-attachments/assets/59629aa7-ab3d-4d38-b8c3-21237d5ebd80" />
