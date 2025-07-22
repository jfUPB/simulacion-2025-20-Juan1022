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

>**Despues de la prueba**
>
>No ocurrió lo que queria, el walker solo empezó a ir hacia arriba, siento que tengo un error en como están distribuidos los valores del choice.
>

>**Solución**
>
>Ya luego de indagar descubrí que lo que pasaba era que habia empezado a usar valores decimales cuando el floor los convertía a enteros por lo que siempre los redondeaba a 1 o 0 por eso siempre iba hacia arriba.
