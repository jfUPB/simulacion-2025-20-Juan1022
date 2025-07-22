# Unidad 1

## 🔎 Fase: Set + Seek

## Actividad 01
### La aleatoriedad en el arte generativo

*Piensa y describe en una sola frase y en tus propias palabras cómo la aleatoriedad influye en el arte generativo.*

>**📝 Bitácora (Respuesta)**
>
>La aletoreidad le aplica una capa mucho más abierta e interpretativa al arte, logrando transmitir varios conceptos de una misma experiencia.


## Actividad 02
### Ejemplo de aleatoriedad en el arte generativo

 **Preguntas**  
 
*Luego de ver el trabajo de Sofía piensa y escribe en TUS PROPIAS palabras*  

*¿Cuál es el papel de la aleatoriedad en su obra?*  

*Según tu perfil profesional, ¿cómo se aplica el concepto de aleatoriedad en el tipo de proyectos que desarrollas. Ilustra tu respuesta con ejemplos concretos?*

>**📝 Bitácora (Respuesta)** 
> - El papel que toma la aletoreidad en la obra de Sofi viene de la mano con las paletas de colores y el movimientos de las esferas que en conjunto pueden generar ambientes bastante organicos con distintas vibras.
> - En los videojuegos y la animación he encontrado ejemplos donde la aleatoriedad se aplica de forma poderosa, especialmente a través de la música. En títulos como Death Stranding y Outer Wilds, la banda sonora suena de forma aleatoria durante el gameplay. Esto permite que un momento simple o aparentemente monótono se transforme, de forma inesperada, en una experiencia emotiva o impactante.
>
> Acá hay un link que lleva a un momemnto grabado que ejemplifica de lo que estoy hablando:
>
> https://youtu.be/fd19DiyG8AM?si=DlDZt4iwVdclTidr&t=96


## Actividad 03
### Caminatas aleatorias

*Usando de referencia la unidad 0: Randomness del libro guia "The Nature of Code"*

**Preguntas**
**Realiza el siguiente experimento y reporta los resultados en tu bitácora:**

*- Modifica el código del ejemplo Example 0.1: A Traditional Random Walk.*

*- Antes de ejecutar el código, escribe en tu bitácora qué esperas que suceda.*

*- Ejecuta el código y escribe en tu bitácora qué sucedió realmente.*

*- Ocurrió lo que esperabas? ¿Por qué crees que sí o por qué crees que no?*

>**📝 Bitácora (Respuesta)**

```javascript
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

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
    const choice = floor(random(10)); //Aqui el valor random porque voy a trabajar con probabilidades bastante distantes.
    if (choice == 6) {
      this.y++; // Esto hará que haya una probabilidad de 50% de que vaya hacia abajo
    } else if (choice == 8) {
      this.x--; // Una probabilidad de 25% hacia la izquierda
    } else {
      this.y--;
                 // No quiero que se mueva hacia la derecha
    }
  }
}
```
>Lo que espero que pase con este codigo es que el walker empiece a moverse desde el centro de la pantalla y vaya pintando la parte izquierda de la pantalla.

>**Despues de la prueba:**
>No ocurrió lo que queria, el walker solo empezó a ir hacia arriba, siento que tengo un error en como están distribuidos los valores del choice.
>

>**Solución:**
>Ya luego de indagar descubrí que lo que pasaba era que habia empezado a usar valores decimales cuando el floor los convertía a enteros por lo que siempre los redondeaba a 1 o 0 por eso siempre iba hacia arriba.

## Actividad 04
### Distribuciones de probabilidad

*- En tus propias palabras cuál es la diferencia entre una distribución uniforme y una no uniforme de números aleatorios.*

*- Modifica el código de la caminata aleatoria para que utilice una distribución no uniforme, favoreciendo el movimiento hacia la derecha.*

>**📝 Bitácora (Respuesta)**
>
> En una distribución uniforme por ejemplo todo los valores que pueden salir tienden a salir con la una regularidad parecida y en una distribución no uniforme como la Gaussiana, los valores tienden a agruparse y hay un grupo de valores que tienden a repetirse más que otros.

```javascript
function setup() {
  createCanvas(100, 100);

  background(200);

  describe('Three horizontal black lines are filled in randomly. The top line spans entire canvas. The middle line is very short. The bottom line spans two-thirds of the canvas.');
}

function draw() {
  // Style the circles.
  noStroke();
  fill(0, 10);

  // Uniform distribution between 0 and 100.
  let x = random(50,100);
  let y = 25;
  circle(x, y, 5);

  // Gaussian distribution with a mean of 50 and sd of 1.
  x = randomGaussian(70,1);
  y = 50;
  circle(x, y, 5);

  // Gaussian distribution with a mean of 50 and sd of 10.
  x = randomGaussian(70, 10);
  y = 75;
  circle(x, y, 5);
}

```
De está manera los valores se concentran entre el 50 y el 100 que pues representan el lado derecho del canva.
