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
