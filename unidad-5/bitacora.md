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
```

```

Mi código: https://editor.p5js.org/Juan1022/sketches/-xTbGZMim

<img width="795" height="306" alt="image" src="https://github.com/user-attachments/assets/da22e4ed-7f32-49d4-9686-9217ebb74451" />

# A System of Systems
## Implementación Unidad 2: Forces

Agregué en la función run() de cada partícula tres fuerzas: gravedad, viento y fricción. Cada una se calcula como un vector y se añade a la aceleración con applyForce().

El codigo ya tenia implementada la gravedad y le añadí una fuerza horizontal dictada con un noise() para que varie la dirección, también añadí el compomente de fricción para reducir gradualmente la velocidad con los cambios.

## Creación y destrucción de la particula
### Creación
En cada frame el emitter se encarga de añadir una nueva particula con la función addparticle().
### Destrucción
Usa la misma logica del ejemplo pasado.

``` sketch.js
// The Nature of Code
// Daniel Shiffman
// Adaptado para integrar Unidad 2: Fuerzas (gravedad, viento, fricción)

// ==================== PARTICLE ====================
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
  }

  run() {
    // Gravedad (constante hacia abajo)
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);

    // Viento (horizontal, cambia con noise)
    let wind = createVector(noise(frameCount * 0.01) - 0.5, 0);
    this.applyForce(wind);

    // Fricción (opuesta al movimiento)
    let c = 0.01; // coeficiente de fricción
    let friction = this.velocity.copy();
    friction.mult(-1);
    friction.normalize();
    friction.mult(c);
    this.applyForce(friction);

    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}

// ==================== EMITTER ====================
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  run() {
    // Loop backwards para borrar partículas muertas
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

// ==================== SKETCH ====================
let emitters = [];

function setup() {
  createCanvas(640, 240);
  createP("Haz click para añadir sistemas de partículas (con fuerzas de la Unidad 2).");
}

function draw() {
  background(255);
  for (let emitter of emitters) {
    emitter.addParticle();
    emitter.run();
  }
}

function mousePressed() {
  emitters.push(new Emitter(mouseX, mouseY));
}

```
https://editor.p5js.org/Juan1022/full/qz6n7Pmz2
<img width="797" height="299" alt="image" src="https://github.com/user-attachments/assets/53ba4df7-0463-43b1-8766-cb7b80b5273c" />

# a Particle System with Inheritance and Polymorphism
## Implementación Unidad 3: Oscillation
Creé una clase Pendulum que calcula su ángulo, aceleración angular y posición basándose en la gravedad. Dibujé la piñata como una esfera al final de la cuerda del péndulo, porque quería que el emitter no estuviera fijo, sino que simulara una piñata real en movimiento, mezcla los conceptos de oscilación (unidad 5) con partículas (unidad 3).

## Creación y destrucción de la particula
Es la misma logica de los ejemplos anteriores, cada particula tiene su tiempo de vida que en cada frame se va actualizando hasta llegar a 0, todo esto mientras se revisa el array constantemente.

```
// The Nature of Code
// Daniel Shiffman adaptado
// Piñata-emitter en forma de péndulo

let emitter;
let pendulum;

function setup() {
  createCanvas(640, 360);
  // Creamos el péndulo (piñata)
  pendulum = new Pendulum(createVector(width / 2, 0), 200);
  // El emitter se inicializa en la posición de la piñata
  emitter = new Emitter(width / 2, 200);
}

function draw() {
  background(255);

  // Actualizamos el péndulo
  pendulum.update();
  pendulum.display();

  // Actualizamos la posición del emitter a la de la piñata
  emitter.origin.set(pendulum.position.x, pendulum.position.y);

  // Generamos partículas como si cayeran de la piñata
  emitter.addParticle();
  emitter.run();
}

// ---------------- CLASE PARTICLE ----------------
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

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}

// ---------------- CLASE CONFETTI ----------------
class Confetti extends Particle {
  show() {
    let angle = map(this.position.x, 0, width, 0, TWO_PI * 2);

    rectMode(CENTER);
    fill(200, 100, 255, this.lifespan);
    stroke(0, this.lifespan);
    strokeWeight(2);
    push();
    translate(this.position.x, this.position.y);
    rotate(angle);
    square(0, 0, 12);
    pop();
  }
}

// ---------------- CLASE EMITTER ----------------
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    let r = random(1);
    if (r < 0.5) {
      this.particles.push(new Particle(this.origin.x, this.origin.y));
    } else {
      this.particles.push(new Confetti(this.origin.x, this.origin.y));
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

// ---------------- CLASE PENDULUM (piñata) ----------------
class Pendulum {
  constructor(origin, r) {
    this.origin = origin.copy(); // punto fijo
    this.position = createVector(); 
    this.r = r; // longitud del brazo
    this.angle = PI / 4; // ángulo inicial
    this.aVelocity = 0.0; 
    this.aAcceleration = 0.0;
    this.damping = 0.995; // fricción
    this.ballr = 32; // tamaño de la piñata
  }

  update() {
    let gravity = 0.4;
    this.aAcceleration = (-1 * gravity / this.r) * sin(this.angle);
    this.aVelocity += this.aAcceleration;
    this.aVelocity *= this.damping;
    this.angle += this.aVelocity;

    // Posición de la piñata
    this.position.set(
      this.r * sin(this.angle),
      this.r * cos(this.angle),
      0
    );
    this.position.add(this.origin);
  }

  display() {
    stroke(0);
    fill(127);
    // Cuerda
    line(this.origin.x, this.origin.y, this.position.x, this.position.y);
    // Piñata (bola)
    fill(255, 150, 0);
    ellipse(this.position.x, this.position.y, this.ballr, this.ballr);
  }
}
```
https://editor.p5js.org/Juan1022/sketches/yHmugENWe
<img width="727" height="411" alt="image" src="https://github.com/user-attachments/assets/a3330cc7-932a-4055-a991-266afb31422b" />


# A particle system with forces.
## Implementación Unidad 1: Spring Forces
el concepto que apliqué es la fuerza de resorte, basada en la Ley de Hooke. Lo implementé uniendo cada partícula con la siguiente, calculando la fuerza para que siempre intenten regresar a una "longitud de reposo" específica. Así, si se estiran, se jalan; y si se comprimen, se empujan, creando un efecto elástico.

## Creación y destrucción de la particula
### Creación
Las partículas se crean continuamente dentro de la función draw() en sketch.js, donde se llama a emitter.addParticle(). Este método, a su vez, instancia un nuevo objeto Particle y lo agrega al arreglo this.particles del emisor. El código limita el número de partículas a un máximo de 100 con la condición if (this.particles.length < 100), lo que previene un consumo excesivo de memoria.

### Destrucción
La misma logica de los ejemplos anteriores.


### Codigo fuente
``` emitter
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

class Emitter {

  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    // Limit the number of particles to avoid performance issues
    if (this.particles.length < 100) {
      this.particles.push(new Particle(this.origin.x, this.origin.y));
    }
  }

  applyForce(force) {
    for (let particle of this.particles) {
      particle.applyForce(force);
    }
  }

  run() {
    // --- INICIO DE LA IMPLEMENTACIÓN DEL RESORTE ---
    
    // Constantes para el resorte
    let k = 0.1; // Constante de rigidez del resorte
    let restLength = 40; // Longitud en reposo del resorte

    // Iteramos a través de todas las partículas para conectar las adyacentes
    for (let i = 0; i < this.particles.length - 1; i++) {
      let p1 = this.particles[i];
      let p2 = this.particles[i + 1];
      
      // Calcular el vector de fuerza entre las dos partículas
      let force = p5.Vector.sub(p1.position, p2.position);
      let d = force.mag();
      let stretch = d - restLength;
      
      force.normalize();
      force.mult(-1 * k * stretch);
      
      // Aplicar las fuerzas a ambas partículas
      p1.applyForce(force);
      // La segunda partícula recibe la fuerza opuesta
      p2.applyForce(force.copy().mult(-1));
      
      // Dibujar la línea para visualizar el resorte
      stroke(0, 100);
      strokeWeight(1);
      line(p1.position.x, p1.position.y, p2.position.x, p2.position.y);
    }
    // --- FIN DE LA IMPLEMENTACIÓN DEL RESORTE ---

    // Este bucle se mantiene para actualizar y eliminar las partículas muertas
    for (let i = this.particles.length - 1; i >= 0; i--) {
      const particle = this.particles[i];
      particle.run();
      if (particle.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}
```

``` particle.js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Simple Particle System
// A simple Particle class

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0.0);
    this.velocity = createVector(random(-1, 1), random(-2, 0));
    this.lifespan = 255.0;
    this.mass = 1; // Let's do something better here!
  }

  run() {
    this.update();
    this.show();
  }

  applyForce(force) {
    let f = force.copy();
    f.div(this.mass);
    this.acceleration.add(f);
  }

  
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    this.lifespan -= 2.0;
  }

  
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  // Is the particle still useful?
  isDead() {
    return this.lifespan < 0.0;
  }
}
```

``` sketch.js


let emitter;

function setup() {
  createCanvas(1280, 480);
  emitter = new Emitter(width / 4, 10);
}

function draw() {
  background(220); // Un fondo sólido para ver mejor las líneas

  // Aplicar fuerza de gravedad a todas las partículas
  let gravity = createVector(0, 0.1);
  emitter.applyForce(gravity);

  emitter.addParticle();
  emitter.run();
}
```
https://editor.p5js.org/Juan1022/full/Y_9vCwY5b
<img width="805" height="590" alt="image" src="https://github.com/user-attachments/assets/080aaaf0-3587-4f45-845f-6e9a1ded8bb9" />

