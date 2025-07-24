# Unidad 1 - Continuación del Set + Seek 

## 🔎 Fase: Set + Seek

## Actividad 01
### La aleatoriedad en el arte generativo

*Piensa y describe en una sola frase y en tus propias palabras cómo la aleatoriedad influye en el arte generativo.*

>**📝 Bitácora (Respuesta):**
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

>**📝 Bitácora (Respuesta):**
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

## Actividad 05
### Distribución Normal

*Una vez has entendido el concepto de distribución normal, vas a pensar en una nueva manera de visualizarlo.*

*- Crea un nuevo sketch en p5.js que represente una distribución normal.*

*- Copia el código en tu bitácora.*

*- Coloca en enlace a tu sketch en p5.js en tu bitácora.*

*- Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.*

>**📝 Bitácora (Respuesta):**
>
> Utilicé una distribución normal para representar estrellas en el cielo el cual hace uso de la distribución Gaussiana para agrupar las estrellas en la parte superior del canva
>
```javascript
let stars = [];

function setup() {
  createCanvas(400, 400);
  background(0);
  noLoop();

  for (let i = 0; i < 1000; i++) {
    let x = random(width);        
    let y = randomGaussian(100, 50);
    stars.push({ x, y });
  }
}

function draw() {
  background(0);

  noStroke();
  fill(255, 255, 0);

  for (let star of stars) {
    if (star.y >= 0 && star.y <= height) {
      circle(star.x, star.y, random(1, 2.5));
    }
  }
}
```
**Enlace:**
https://editor.p5js.org/Juan1022/sketches/fWXKu8zKT

<img width="494" height="496" alt="image" src="https://github.com/user-attachments/assets/c9c6c0b9-281b-4b4d-912c-1f2d62803f7d" />

## Actividad 06
### Distribución personalizada: Lévy flight

*Analicemos juntos y detenidamente el concepto de Lévy flight.*
- Crea un nuevo sketch en p5.js donde modifiques uno de los ejemplos anteriores y adiciones de Lévy flight.
- Explica por qué usaste esta técnica y qué resultados esberabas obtener.
- Copia el código en tu bitácora.
- Coloca en enlace a tu sketch en p5.js en tu bitácora.
- Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.
  

  >**📝 Bitácora (Respuesta):**
  >
  > Usé el ejemplo de la actividad anterior en la que por medio de una distribución gaussiana hice un cielo de estrellas, siguiendo este concepto de galaxia, pensé
  > en que esta nueva distribución quedaba perfecto para generar "nebulosas" las cuales tendrián un comportamiento parecido a lo que dicta esta distirbución.
  >
  >Esperaba obtener varios puntos disperos con acumulaciones de varios en algunos lugares del canva
  >
  >
```javascript
  
  let stars = [];
  let clouds = [];

function setup() {
  createCanvas(400, 400);
  background(0);
  generateStars();
  generateNebulas();
  noLoop();
}

function draw() {
  background(0);

  // Dibujar estrellas
  noStroke();
  fill(180, 200, 30, 120);
  for (let star of stars) {
    circle(star.x, star.y, random(1, 2.5));
  }

  // Nubes de nebulosa
  noStroke();
  for (let p of clouds) {
    let c = chooseColor();
    fill(c[0], c[1], c[2], 60); 
    circle(p.x, p.y, random(2, 4)); 
  }
}

function generateStars() {
  stars = [];
  for (let i = 0; i < 1000; i++) {
    let x = random(width);
    let y = randomGaussian(100, 50);
    stars.push({ x, y });
  }
}

function generateNebulas() {
  clouds = [];
  
  let centers = 5;
  for (let j = 0; j < centers; j++) {
    let pos = createVector(random(width), random(height));
    
    for (let i = 0; i < 2000; i++) {
      let step = levy();
      let angle = random(TWO_PI);
      pos.add(cos(angle) * step, sin(angle) * step);
      
      if (pos.x > 0 && pos.x < width && pos.y > 0 && pos.y < height) {
        clouds.push(pos.copy());
      }

      // Reiniciar el origen para romper acumulación
      if (random() < 0.005) {
        pos = createVector(random(width), random(height));
      }
    }
  }
}

function levy() {
  return pow(random(1), -1.5);
}

function chooseColor() {
  let choice = floor(random(3));
  if (choice === 0) return [0, 100, 255];   
  if (choice === 1) return [150, 0, 255];   
  return [0, 200, 255];                     // Celeste
}

function mousePressed() {
  generateNebulas();
  redraw();
}
```
https://editor.p5js.org/Juan1022/full/MFznNy6mt

<img width="498" height="492" alt="image" src="https://github.com/user-attachments/assets/7d506282-6655-40d4-b12f-faefb9aa1468" />

## Actividad 07
### Ruido Perlin
*Analicemos junto el concepto de ruido Perlin analizando la figura 0.4: “A graph of Perlin noise values over time (left) and of random noise values over time (right)”.*

- Crea un nuevo sketch en p5.js donde los visualices.
- Explica el concepto qué resultados esberabas obtener.
- Copia el código en tu bitácora.
- Coloca en enlace a tu sketch en p5.js en tu bitácora.
- Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.

>**📝 Bitácora (Respuesta):**
>
> A diferencia del random que puede generar valores bastantes dispersos y desorganizados, el ruido de perlín genera cambios mas suaves y organicos, asi que con este experimento, usando como base el ruido de Perlin espero generar figuras, movimientos y texturas más organicas.
>
>
```javascript
function setup() {
  createCanvas(600, 400);
  noFill();
}

function draw() {
  background(0, 100, 150, 40); // Azul oscuro translúcido, simula el agua

  stroke(255, 255, 255, 70); // Blanco suave, semi-transparente
  strokeWeight(1.5);

  for (let y = 0; y < height; y += 10) {
    beginShape();
    for (let x = 0; x < width; x += 5) {
      let nx = x * 0.01;
      let ny = y * 0.01;
      let t = frameCount * 0.01; // tiempo
      let offset = noise(nx, ny + t) * 40;
      vertex(x, y + offset);
    }
    endShape();
  }
}
```

https://editor.p5js.org/Juan1022/full/sbxB1SJiq

<img width="746" height="494" alt="image" src="https://github.com/user-attachments/assets/956bebad-becb-4d33-b5cf-898b8947d46d" />

