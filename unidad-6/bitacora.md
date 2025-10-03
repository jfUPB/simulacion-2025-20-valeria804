# Evidencias de la unidad 6

## SetSeek

### Actividad 1

me gustan estas dos imagenes

<img width="762" height="762" alt="image" src="https://github.com/user-attachments/assets/02a4702e-deef-4768-848e-b1a7551fc824" />

me parece interesante como cambiando un solo atribito se pueden crear cosas tan diferentes, lineas que parecen sin orden pero a la vez con orden, tambien como se crean profundidad solo mediante lineas 

<img width="916" height="615" alt="image" src="https://github.com/user-attachments/assets/00d01db8-b21c-4803-b91e-1db1d1c818af" />

me parece interesante como propone nuevas formas de presentar los conceptos para dar una energia totalmente diferente. 

- me inspira como sus algoritmos tienen restricciones claras o reglas, pero al mismo tiempo hay suficiente aleatoriedad de la máquina y del humano para que cada pieza se sienta viva e inesperada.

  su trabajo tiene mucha densidad de lineas, patrones y curvas que se superponen, cambios sutiles de color o grosor, y de ritmo visual. eso le da profundidad y textura, que se ve diferente dependiendo de parametros sutiles

### Actividad 02

- ¿Qué es una fuerza de dirección (steering force)?

  es una fuerza que permite que un objeto cambie su trayectoria de forma suave y controlada para moverse hacia un objetivo. se basa en la diferencia entre la velocidad deseada (la direccion hacia donde querríamos movernos) y la velocidad actual del objeto. esta fuerza no empuja directamente al objeto en la direccion del objetivo como alguna fuerza fisica (gravedad, atraccion, friccion), sino que ajusta gradualmente su rumbo, generando movimientos mas naturales y suaves

- ¿Qué diferencia tiene este tipo de fuerza con las que ya hemos estudiado en el contexto de la simulación de agentes?

  las fuerzas que hemos visto antes suelen ser directas y físicas. estas fuerzas cambian la aceleracion y la velocidad del agente, de forma mecanica y sin intencion. mientras que steering force trabaja con un modelo de decision: el agente decide hacia donde ir calculando la diferencia entre lo que quiere (desired velocity) y lo que esta haciendo.

- ¿Qué relación tiene la steering force con Craig Reynolds y su trabajo en simulación de comportamiento animal?

  la steering force es el nucleo del trabajo de Reynolds porque le permitio modelar comportamientos animales colectivos sin necesidad de programar explicitamente “formas de bandada”. cada agente actua con su propio steering y el grupo se autoorganiza. en el modelo cada agente sigue tres reglas basicas: Separación → evitar chocar con otros. Alineación → moverse en la misma dirección que los vecinos. Cohesión → moverse hacia el centro del grupo.

### Actividad 03

- Explica brevemente la estructura de datos usada para el campo de flujo y cómo se generan sus vectores.

  el campo se guarda como una matriz 2D "this.field[col][row]" donde cada celda contiene un vector de direccion normalizado. estos vectores se generan usando ruido perlin, produciendo angulos suaves y continuos en el espacio, lo que hace que el flujo tenga coherencia en lugar de ser caotico

- Describe con tus palabras cómo un agente utiliza el campo para calcular su fuerza de dirección.

  el agente toma su posicion actual y lo convierte a indices de la cuadricula, de ese modo obtiene el vector correspondiente a esa celda, que representa su velocidad deseada. luego calcula la steering force, siendo la diferencia entre esa velocidad deseada y su velocidad actual. esa fuerza se limita por un maximo y se aplica como aceleracion, ajustando suavemente el rumbo del agente

- Lista los parámetros clave identificados (resolución, maxspeed, maxforce).

  resolution: tamaño de cada celda de la grilla: 20.

  maxspeed: velocidad máxima del agente: entre 2 y 5.

  maxforce: fuerza máxima de dirección: 0.1 y 0.5.

- Describe la modificación que realizaste al código y explica detalladamente el efecto que tuvo en el movimiento y comportamiento colectivo de los agentes. Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.

https://github.com/user-attachments/assets/9a3cd811-bfb9-4f25-ba88-1748b9b708f3

para llegar a esto modifique la resolucion del campo flowfield = new FlowField(4);

y cambie la forma en que se generan los vectores, haciendo que el angulo dependa de la posicion (i, j) de la celda = let angle = (i * 0.3 + j * 0.3) % TWO_PI; los angulos cambian de manera regular en la cuadricula debido a que % TWO_PI asegura que el angulo siempre este entre 0 y 2π.

