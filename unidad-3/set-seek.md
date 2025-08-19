# Unidad 3

## 🔎 Fase: Set + Seek

## Actividad 09
### Modelando fuerzas

En tu mundo de pixeles tu puedes crear las fuerzas o las puedes modelar.

Vamos a probar con lo segundo, modelar una fuerza. Ve a la sección Modeling forces del texto guía.

>📤 Bitácora
>
>Inventa tres obras generativas interactivas, uno para cada una de las siguientes fuerzas:
>
>- Fricción.
>- Resistencia del aire y de fluidos.
>- Atracción gravitacional.
>Explica cómo modelaste cada fuerza.
>
>Conceptualmente cómo se relaciona la fuerza con la obra generativa.
>
>Copia el enlace a tu ejemplo en p5.js.
>
>Copia el código.
>
>Captura una imagen representativa de tu ejemplo.

# Obra #1: Fricción

La fuerza actúa como un motor invisible que define la evolución del movimiento, el ritmo y, en última instancia, la estética de la pieza. Al igual que en la vida real, puedo traducirla en términos visuales simulando la cantidad de partículas que se desprenden cuando un objeto metálico entra en contacto con una superficie áspera.

**Link:**
https://editor.p5js.org/Juan1022/full/VXf_D87W5

``` java
let person;
let surfaces = [];
let sparks = [];

function setup() {
  createCanvas(800, 300);
  
  

  // Crear superficies (x, ancho, color, fricción)
  surfaces.push(new Surface(0, 200, color(220), 0.01)); // piso liso
  surfaces.push(new Surface(200, 200, color(180), 0.03)); // áspero medio
  surfaces.push(new Surface(400, 200, color(140), 0.08)); // áspero alto
  surfaces.push(new Surface(600, 200, color(100), 0.12)); // muy áspero

  person = new Person(50, height - 60, 2);
}

function draw() {
  background(30);
  
// Empuje constante hacia la derecha
let push = createVector(0.05, 0);
person.applyForce(push);
  
  // Dibujar superficies
  for (let s of surfaces) {
    s.show();
  }

  // Detectar fricción y generar chispas
  for (let s of surfaces) {
    if (person.pos.x > s.x && person.pos.x < s.x + s.w) {
      let friction = person.vel.copy();
      friction.normalize();
      friction.mult(-1);
      let normalForce = 1;
      let frictionMag = s.mu * normalForce;
      friction.mult(frictionMag);
      person.applyForce(friction);

      // Generar chispas dependiendo de μ
      let sparkCount = int(map(s.mu, 0.01, 0.12, 1, 8));
      for (let i = 0; i < sparkCount; i++) {
        sparks.push(new Spark(person.pos.x - 20, person.pos.y + 15, s.mu));
      }
    }
  }

  // Actualizar y mostrar persona
  person.update();
  person.display();

  // Mostrar chispas
  for (let i = sparks.length - 1; i >= 0; i--) {
    sparks[i].update();
    sparks[i].display();
    if (sparks[i].finished()) sparks.splice(i, 1);
  }
}

// ----------------- CLASES -----------------

class Person {
  constructor(x, y, m) {
    this.pos = createVector(x, y);
    this.vel = createVector(2, 0);
    this.acc = createVector(0, 0);
    this.mass = m;
  }

  applyForce(force) {
    let f = force.copy();
    f.div(this.mass);
    this.acc.add(f);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  display() {
    fill(255);
    rect(this.pos.x, this.pos.y - 30, 20, 30); // cuerpo
    fill(200);
    rect(this.pos.x - 25, this.pos.y - 10, 20, 15); // bolso
  }
}

class Surface {
  constructor(x, w, col, mu) {
    this.x = x;
    this.w = w;
    this.col = col;
    this.mu = mu;
  }

  show() {
    noStroke();
    fill(this.col);
    rect(this.x, height - 40, this.w, 40);
  }
}

class Spark {
  constructor(x, y, mu) {
    this.pos = createVector(x, y);
    let angle = random(-PI / 4, PI / 4);
    let speed = map(mu, 0.01, 0.12, 1, 4);
    this.vel = p5.Vector.fromAngle(angle).mult(speed);
    this.lifetime = 255;
    this.size = map(mu, 0.01, 0.12, 2, 6);
    this.color = color(255, random(150, 255), 0);
  }

  update() {
    this.pos.add(this.vel);
    this.lifetime -= 10;
  }

  display() {
    noStroke();
    fill(this.color.levels[0], this.color.levels[1], this.color.levels[2], this.lifetime);
    ellipse(this.pos.x, this.pos.y, this.size);
  }

  finished() {
    return this.lifetime <= 0;
  }
}

```
<img width="993" height="363" alt="image" src="https://github.com/user-attachments/assets/085fcf14-de40-4106-ba27-ae97a378fe73" />

# Obra #2: Resistencia del aire y fluidos.
La fuerza es el motor invisible que moldea el balanceo del péndulo. El viento, al oponerse o impulsarlo, transforma su ritmo y crea un movimiento único, resultado del juego entre física y espectador.

**Link:**
https://editor.p5js.org/Juan1022/full/vJbgJSlFt

``` java
let pendulum;
let windForce = 0;

function setup() {
  createCanvas(600, 400);
  pendulum = new Pendulum(width / 2, 100, 200);
}

function draw() {
  background(220);

  // Viento directo según teclas
  let wind = 0;
  if (keyIsDown(LEFT_ARROW)) {
    wind = -0.002; // viento hacia la izquierda
  }
  if (keyIsDown(RIGHT_ARROW)) {
    wind = 0.002; // viento hacia la derecha
  }

  // Aplicar viento directamente
  if (wind !== 0) {
    pendulum.applyWind(wind);
  }

  // Dibujar indicador de viento
  drawWindIndicator(wind);

  pendulum.update();
  pendulum.display();
}

function drawWindIndicator(wind) {
  push();
  translate(width / 2, 40);

  if (wind > 0) {
    // Flecha hacia la derecha
    drawArrow(color(0, 150, 255), 60);
  } else if (wind < 0) {
    // Flecha hacia la izquierda
    drawArrow(color(255, 100, 100), -60);
  } else {
    noStroke();
    fill(100);
    textAlign(CENTER);
    textSize(16);
    text("Sin viento", 0, 5);
  }

  pop();
}

function drawArrow(col, len) {
  stroke(col);
  strokeWeight(3);
  fill(col);
  line(0, 0, len, 0);
  if (len > 0) {
    triangle(len, 0, len - 10, -5, len - 10, 5);
  } else {
    triangle(len, 0, len + 10, -5, len + 10, 5);
  }
}

class Pendulum {
  constructor(x, y, r) {
    this.origin = createVector(x, y);
    this.position = createVector();
    this.r = r;
    this.angle = PI / 4;
    this.aVelocity = 0.0;
    this.aAcceleration = 0.0;
    this.damping = 0.995;
    this.gravity = 0.4;
  }

  update() {
    let force = (-1 * this.gravity / this.r) * sin(this.angle);
    this.aAcceleration = force;
    this.aVelocity += this.aAcceleration;
    this.aVelocity *= this.damping;
    this.angle += this.aVelocity;
  }

  applyWind(force) {
    // El viento se aplica como aceleración angular directa
    this.aVelocity += force;
  }

  display() {
    this.position.set(
      this.r * sin(this.angle),
      this.r * cos(this.angle)
    );
    this.position.add(this.origin);

    stroke(0);
    strokeWeight(2);
    line(this.origin.x, this.origin.y, this.position.x, this.position.y);
    fill(127);
    ellipse(this.position.x, this.position.y, 48, 48);
  }
}

```
<img width="653" height="436" alt="image" src="https://github.com/user-attachments/assets/89e7fa32-933c-4b36-9be7-630560dc7aee" />


# Obra #3: Fuerza Gravitacional.

La fuerza en esta obra fue modelada como una atracción gravitacional: las bolas con mayor masa ejercen más atracción, lo que hace que las pequeñas sean absorbidas por ellas. Este modelo no solo aporta realismo físico, sino que refuerza el concepto de crecimiento colectivo y transformación al representar cómo los cuerpos se buscan, se encuentran y terminan fusionándose en uno nuevo.

**Link:**
https://editor.p5js.org/Juan1022/full/u3t5SffOK

``` java
let particles = [];
const K_SIZE = 1; // factor opcional para escalar visualmente el tamaño (1 = natural)

function setup() {
  createCanvas(800, 600);
  for (let i = 0; i < 20; i++) {
    const r0 = random(6, 12);
    particles.push(new Particle(random(width), random(height), r0));
  }
}

function draw() {
  background(20);

  let newParticles = [];
  let toRemove = new Set();

  // Fuerzas + colisiones (solo j>i)
  for (let i = 0; i < particles.length; i++) {
    const p = particles[i];

    // gravedad newtoniana
    for (let j = 0; j < particles.length; j++) {
      if (i !== j) {
        const other = particles[j];
        const f = p.attract(other);
        other.applyForce(f);
      }
    }

    // colisiones
    for (let j = i + 1; j < particles.length; j++) {
      const a = particles[i];
      const b = particles[j];
      if (a.collides(b)) {
        // punto de impacto (aproximado como punto medio)
        const pos = p5.Vector.add(a.pos, b.pos).mult(0.5);

        // masa total = suma (masa ~ r^2)
        const newMass = a.mass + b.mass;

        // conservar momento lineal
        const newVel = p5.Vector.add(
          p5.Vector.mult(a.vel, a.mass),
          p5.Vector.mult(b.vel, b.mass)
        ).div(newMass);

        // crear nueva partícula en el instante y posición del choque
        const newR = K_SIZE * sqrt(newMass);
        const np = new Particle(pos.x, pos.y, newR);
        np.mass = newMass; // coherencia masa-radio
        np.vel = newVel;
        np.color = lerpColor(a.color, b.color, 0.5);

        newParticles.push(np);
        toRemove.add(a);
        toRemove.add(b);
      }
    }
  }

  // eliminar las colisionadas y añadir las nuevas fusionadas
  particles = particles.filter(p => !toRemove.has(p));
  particles = particles.concat(newParticles);

  // actualizar y dibujar
  for (const p of particles) {
    p.update();
    p.show();
  }
}

class Particle {
  constructor(x, y, r) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(2);
    this.acc = createVector(0, 0);
    this.r = r;
    this.mass = (r / K_SIZE) * (r / K_SIZE); // masa = r^2 (coherente con 2D)
    this.color = color(random(120, 255), random(120, 255), random(160, 255));
  }

  applyForce(force) {
    // a = F / m
    this.acc.add(p5.Vector.div(force, this.mass));
  }

  attract(other) {
    const G = 0.5; // ajusta la intensidad
    const dir = p5.Vector.sub(this.pos, other.pos);
    let d = dir.mag();
    d = constrain(d, 5, 50); // evita singularidades
    const strength = (G * this.mass * other.mass) / (d * d);
    dir.setMag(strength);
    return dir; // fuerza sobre "other" hacia "this"
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  show() {
    noStroke();
    fill(this.color);
    circle(this.pos.x, this.pos.y, this.r * 2);
  }

  collides(other) {
    const d = p5.Vector.dist(this.pos, other.pos);
    return d <= this.r + other.r;
  }
}
```

<img width="894" height="663" alt="image" src="https://github.com/user-attachments/assets/c54bbabc-d8fb-41d9-9e2f-9eb65858e331" />


