# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 1

la aleatoriedad retira el control total del artista acercandolo mas a la aleatoriedad de mundo natural; el arte generativo permite crear resultados unicos e impredecibles, de esta forma siempre se renueva, genera sorpresa y variacion, haciendo que cada experiencia sea unica y personal

### Actividad 2

- su aleatoriedad permite que cada visualizacion sea unica e impredesible, pero mas que solo lograr una experiencia unica y personal tambien permite conectar con la narrativa del mensaje que busca dar, con la cancion concreta "la semilla" representa el sentimiento de lo natural y de que nosotros como humanos no lo controlamos. esto tambien permite que el artista que lo esta manipulando cree junto a la maquina representando como la musica se manifiesta en ella

- en mi perfil profesional que busco enfocarme en la animacion y creacion de mundos, por lo tanto esto puede facilitar el trabajo haciendo que se creen varios elementos en la escena de forma aleatoria, esto para que se vea mas natural y no tan rigido, de esta forma se puede facilitar el trabajo y generar un resultado mas estetico, ejemplos podrian ser:


### Actividad 3

``` js

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

buscar que es un constructor
