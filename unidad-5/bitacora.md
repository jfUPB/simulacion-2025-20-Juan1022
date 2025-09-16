# Evidencias de la unidad 5
# Actividad 2

# An array of particles
## Implementación Unidad 1: Randomness
En el particle.js realicé los siguientes cambios basados en la Unidad #1 Randomness
this.size = random(5,15) - Aquí cada particula tiene un tamaño distinto que varia aleatoriamente.
this.col = color(random(255), random(255), random(255)) - El color incial al crearse también es aleatorio.
jitter = createVector(random(-0.2,0.2), random(-0.2,0.2)) - El movimiento es creado con un ruido aleatorio en cada frame.

## Creación y destrucción de la particula
### Creación
Cada frame se crea una particula nueva que se va acumulando en un array y se van guardando todas las particulas creadas en pantalla.
### Destrucción
Cada particula tiene un tiempo de vida dictado por la propiedad "lifespan" que se va reduciendo en cada update() cuando esta es < 0, se elimina esa particula del array con la función splice() que sirve para quitar, reemplazar o insertar elementos en los arrays.


### Codigo fuente
``` particle.js
 // The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Simple Particle System

// A simple Particle class

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  // Is the particle still useful?
  isDead() {
    return (this.lifespan < 0.0);
  }
}
```

Mi código: https://editor.p5js.org/Juan1022/sketches/-xTbGZMim

<img width="795" height="306" alt="image" src="https://github.com/user-attachments/assets/da22e4ed-7f32-49d4-9686-9217ebb74451" />



