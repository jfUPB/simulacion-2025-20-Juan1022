# Evidencias de la unidad 5
# Actividad 2

# An array of particles
## Implementación Unidad 0: Randomness
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

# a Particle System with a Repeller.
## Implementación Unidad 0: Perlin Noise
le añadí un ruido a la fuera del repulsor para que este en vez de tener una fuerza constante tenga una variable, utilicé la función noise() con una variable de "tiempo" que se incrementa en cada cuadro. Este valor de ruido, que oscila entre 0 y 1, se "mapea" a un rango de poder más amplio para el repulsor. El resultado es que la fuerza de repulsión no es estática, sino que pulsa y varía de forma orgánica y no predecible.

## Creación y destrucción de la particula
### Creación
De la msima manera

### Destrucción
De la misma manera
``` emitter.js
//{!1} The Emitter manages all the particles.
class Emitter {

  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  applyForce(force) {
    //{!3} Applying a force as a p5.Vector
    for (let particle of this.particles) {
      particle.applyForce(force);
    }
  }

  applyRepeller(repeller) {
    //{!4} Calculating a force for each Particle based on a Repeller
    for (let particle of this.particles) {
      let force = repeller.repel(particle);
      particle.applyForce(force);
    }
  }

  run() {
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
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.acceleration = createVector(0, 0);
    this.lifespan = 255.0;
  }

  run() {
    this.update();
    this.show();
  }

  applyForce(f) {
    this.acceleration.add(f);
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
    return this.lifespan < 0.0;
  }
}

```
``` repeller.js
class Repeller {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.basePower = 150; // Almacenamos el valor original de poder
    this.noiseTime = random(1000); // Un valor aleatorio para empezar en un punto diferente del ruido
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(127);
    circle(this.position.x, this.position.y, 32);
  }

  repel(particle) {
    let force = p5.Vector.sub(this.position, particle.position);
    let distance = force.mag();
    distance = constrain(distance, 5, 50);

    // Usamos noise() para variar el poder del repulsor
    let noiseValue = noise(this.noiseTime);
    this.power = map(noiseValue, 0, 1, this.basePower * 0.5, this.basePower * 2.0);

    let strength = (-1 * this.power) / (distance * distance);
    force.setMag(strength);
    return force;
  }
}
```
``` sketch.js
// One ParticleSystem
let emitter;
let repeller;

function setup() {
  createCanvas(640, 240);
  emitter = new Emitter(width / 2, 60);
  repeller = new Repeller(width / 2, 250);
}

function draw() {
  background(255);
  emitter.addParticle();

  // Actualizamos el noiseTime del repulsor
  repeller.noiseTime += 0.01;

  let gravity = createVector(0, 0.1);
  emitter.applyForce(gravity);
  emitter.applyRepeller(repeller);
  emitter.run();

  repeller.show();
}
```


https://editor.p5js.org/Juan1022/full/YZP24BxrD
<img width="764" height="289" alt="image" src="https://github.com/user-attachments/assets/34de4009-6a1b-43e2-b372-c2a379f9baae" />


#Apply
## Primeros bocetos: 

Mi idea inicial era tomar una imagene de un boceto de una persona y por medio de emitter y fuerzas recrear su cabello, este seria un cabello elemental con particulas de agua y lava, sin embargo ese concepto fué mutando hasta llegar a este nuevo concepto que conserva ideas como la de la imagen en pantalla y los elementos de la naturaleza:
<img width="1151" height="645" alt="image" src="https://github.com/user-attachments/assets/c58f99e6-f8b5-40df-bc2e-5c114d258cbb" />

Con esta obra busco expresar todo lo que las estaciones, momentos, sonidos y expresiones pueden generar. en resumen, busco que las personas puedan experimentar y observar cómo un simple gesto puede generar una variedad de paisajes emocionales. Es una forma de traducir emociones internas en fenómenos visuales y auditivos.

### Polimorfismo y Herencia

**Herencia:** La clase Particle es la base que da a las partículas propiedades como su posición, velocidad y vida útil. Las otras clases de partículas (Heart, Bubble, etc.) heredan esas propiedades y no necesitan reescribirlas.

**Polimorfismo:** Las clases hijas usan el polimorfismo para tener su propia versión del método display(). Cuando el emisor le dice a una partícula que se dibuje, el programa sabe si dibujar un corazón, una burbuja o una hoja, sin que tengas que decírselo explícitamente.

En esta obra recorro conceptos como el noise() de la unidad 0 para aletorizar los colores de los elementos, utilizo vectores como velocidad,fuerza, posición que me sirven para darle vida a las particulas, utilizo fuerza de atracción para implementar cierta interacción con el usuari, el sistema de particulas e implementación de sonidos, de esta manera hago el recorrido por todas las unidades de Nature of code.

### Vida de la particula
Para controlar esto uso el mismo sistema que usan los ejemplos del libro, Cada vez que se crea una nueva partícula (en el constructor de la clase Particle), su lifespan se inicializa a un valor alto (en tu caso, 255). Este valor funciona como un contador de vida regresivo, tambien se me ocurrió usar este elemento para controlar el tamaño de las particulas que cuando se crean son pequeñas pero a medida que va reduciendo las particulas van creciendo.

https://editor.p5js.org/Juan1022/full/mFSGdGHs8

https://editor.p5js.org/Juan1022/full/uXfKFzDMh - Jugando con valores de lifespan y la velocidad del emitter.

<img width="1841" height="796" alt="image" src="https://github.com/user-attachments/assets/15aec5ae-e3ab-4878-a263-f96a1cd12924" />

<img width="3840" height="2160" alt="image" src="https://github.com/user-attachments/assets/14c382a3-2550-4055-beee-197196f7a9a7" />

<img width="3840" height="2160" alt="image" src="https://github.com/user-attachments/assets/1a551e9c-950c-4438-98c6-6386caea556b" />

#Codigos:
``` sketch.js
let img;
let blizzardSound;

let emitter;
let particleType = 1;

let showHeartEyes = false;
let showUnderwaterEffect = false;
let fallingLeaves = [];

function preload() {
  img = loadImage('Chica.png');
  soundFormats('mp3');
  blizzardSound = loadSound('Winter.mp3'); 
}

function setup() {
  createCanvas(1920, 1080);
  
  let imgWidth = 1024;
  let imgHeight = 600;

  let imgX = (width - imgWidth) / 2;
  let imgY = (height - imgHeight) / 2;

  let emitterX = imgX + imgWidth / 1.99;
  let emitterY = imgY + imgHeight / 2 + 50; 
  emitter = new Emitter(emitterX, emitterY); 

  blizzardSound.setVolume(0.5);
  blizzardSound.setLoop(true);
}

function draw() {
  background(0);
  
  let imgWidth = 1024;
  let imgHeight = 600;
  let imgX = (width - imgWidth) / 2;
  let imgY = (height - imgHeight) / 2;

  image(img, imgX, imgY, imgWidth, imgHeight);

  // Use the new EyeHeart class for the eyes
   if (showHeartEyes) {
    let newXOffset = 30;
    let newYOffset = 80;
    
    // Suma un valor para mover ambos corazones a la derecha
    let moveRight = 10; // Ajusta este valor para moverlos más o menos
    
    let leftHeart = new EyeHeart(imgX + imgWidth / 2 - newXOffset + moveRight, imgY + imgHeight / 2 - newYOffset);
    leftHeart.display();
    
    let rightHeart = new EyeHeart(imgX + imgWidth / 2 + newXOffset + moveRight, imgY + imgHeight / 2 - newYOffset);
    rightHeart.display();
  }

  for (let i = fallingLeaves.length - 1; i >= 0; i--) {
    let p = fallingLeaves[i];
    p.update();
    p.display();
    if (p.isDead()) {
      fallingLeaves.splice(i, 1);
    }
  }
  if (particleType === 3 && frameCount % 10 === 0) {
    let newLeaf = new AutumnLeaf(random(width), random(-100, 0));
    newLeaf.velocity = createVector(random(-0.5, 0.5), random(1, 3));
    fallingLeaves.push(newLeaf);
  }

  if (showUnderwaterEffect) {
    noStroke();
    fill(0, 0, 100, 70); 
    rectMode(CORNER);
    rect(0, 0, width, height);
  }

  emitter.run();
  
  if (frameCount % 5 === 0) {
    emitter.addParticle(particleType); 
  }
}

function keyPressed() {
  showHeartEyes = false;
  showUnderwaterEffect = false;
  fallingLeaves = []; 
  if (blizzardSound.isPlaying()) {
    blizzardSound.stop();
  }

  if (key === '1') {
    particleType = 1;
    showHeartEyes = true;
  } else if (key === '2') {
    particleType = 2;
    blizzardSound.play();
  } else if (key === '3') {
    particleType = 3;
  } else if (key === '4') {
    particleType = 4;
    showUnderwaterEffect = true;
  }
}
```

```particle.js
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = p5.Vector.random2D().mult(random(0.5, 2));
    this.lifespan = 255; // Vida inicial
  }

  update() {
    this.position.add(this.velocity);
    this.lifespan -= 2;
  }
  
  applyForce(force) {
    this.velocity.add(force);
  }

  display() {
    // Este método es sobrescrito por las subclases
  }

  isDead() {
    return this.lifespan < 0;
  }
}
```

```emitter.js
class Emitter {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.particles = [];
  }

  addParticle(type) {
    let newParticle;
    if (type === 1) {
      newParticle = new Heart(this.position.x, this.position.y);
    } else if (type === 2) {
      newParticle = new Snowflake(this.position.x, this.position.y);
    } else if (type === 3) {
      newParticle = new AutumnLeaf(this.position.x, this.position.y);
    } else if (type === 4) {
      newParticle = new Bubble(this.position.x, this.position.y);
    }
    this.particles.push(newParticle);
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];

      // Calcular la fuerza de atracción del mouse
      let mouse = createVector(mouseX, mouseY);
      let attractionForce = p5.Vector.sub(mouse, p.position);
      attractionForce.normalize();
      attractionForce.mult(0.5); // Ajusta la intensidad de la fuerza

      p.applyForce(attractionForce); // Aplicar la fuerza
      p.update();
      p.display();

      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}
```

La siguiente es la estrucutra base de cada tipo de particula, solo pondré una, sin embargo hay más.

```heart.js
class Heart extends Particle {
  constructor(x, y) {
    super(x, y);
    this.color = color(random(150, 255), 0, random(150, 255));
  }

  display() {
    let size = map(this.lifespan, 255, 0, 0, 50);
    fill(this.color, this.lifespan);
    noStroke();
    
    // Draw heart shape
    beginShape();
    vertex(this.position.x, this.position.y);
    bezierVertex(this.position.x - size / 2, this.position.y - size / 2, this.position.x - size, this.position.y + size / 3, this.position.x, this.position.y + size);
    bezierVertex(this.position.x + size, this.position.y + size / 3, this.position.x + size / 2, this.position.y - size / 2, this.position.x, this.position.y);
    endShape(CLOSE);
  }
}
```
#Justificación
Considero que mi nota es **5**.

* **1er componente:** Realicé y documenté el debido proceso de la etapa de investigación (actividad 2). Los conocimientos obtenidos me sirvieron y fueron aplicados en la fase de `apply`, de donde implementé los conceptos de creación y destrucción de las partículas, que fueron recurrentes en todos los ejemplos.

* **2do componente:** Presento el concepto que me sirvió de base sólida para la creación del proyecto. El resto fueron simples cambios en algunos elementos del código, con ideas que fueron surgiendo al verlo reproduciendo en pantalla.

* **3er componente:** Me cercioré de hacer uso de los conocimientos que me suministraron la investigación y la aplicación en las unidades anteriores, con conceptos desde:
    * **Unidad 0: `Randomness`:** El color de las partículas está dictado por este mismo.
    * **Unidad 1: Vectores:** La clase `Particle` hace uso de elementos como velocidad, fuerza, posición, etc.
    * **Unidad 2: Fuerzas:** Fuerzas de atracción aplicadas al movimiento del mouse para generar interacción con el usuario.
    * **Unidad 3:** De esta unidad, lo que más me quedó fue el concepto de añadir sonidos; aunque no fue el foco de esta unidad, fue una parte clave de mi `apply`.
    * **Unidad 4:** Sistema de partículas. Al ser lo principal en este `apply`, es la base de todo el proyecto.

* **4to componente:** Probé la obra muchas veces y fui encontrando algunos errores. Sin embargo, fueron solucionados. Los errores se presentaban mayormente con el control del tamaño de las partículas, que hacían uso del `lifespan`, y al cambiar el `lifespan` empezaban a tomar formas y posiciones extrañas.
